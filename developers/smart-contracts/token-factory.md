# 🏭 Token Fabrik

Mit der Token Fabrik können Sie Ihre eigenen, vom Cosmos SDK Bank Module anerkannten Denoms erstellen.&#x20;

Dadurch kann Ihr Protokoll seine eigenen Token ausgeben, ohne den CW20-Standard unterstützen zu müssen. Das bedeutet, dass alle Token im Kujira-Netzwerk als Gasgebührenzahlungen kompatibel sind und als Protokolleinnahmen gesammelt und an die KUJI-Staker verteilt werden können.&#x20;

### CLI

Einen Denom erstellen

```
kujirad tx denom create-denom mydenom
```

Minten

```
kujirad tx denom mint 10000factory/kujira12345/mydenom kujira123...
```

und burnen

```
kujirad tx denom burn 10000factory/kujira12345/mydenom 
```

### CosmWASM Bindings

Diese Befehle können auch über Ihre Smart Contracts ausgegeben werden. Bitte beachten Sie die in kujira-rs veröffentlichten Bindings:&#x20;

{% embed url="https://docs.rs/kujira/0.7.5/kujira/msg/enum.KujiraMsg.html" %}

#### Erstellung

Damit der Vertrag einen Denom minten und burnen kann, muss er vom Vertrag selbst erstellt werden, oft bei der Instantiierung:&#x20;

```rust
use kujira::msg::{DenomMsg, KujiraMsg};

#[cfg_attr(not(feature = "library"), entry_point)]
pub fn instantiate(
    deps: DepsMut<KujiraQuery>,
    env: Env,
    _info: MessageInfo,
    msg: InstantiateMsg,
) -> Result<Response<KujiraMsg>, ContractError> {
    let addr = env.contract.address;
    let denom = format!("factory/{addr}/{denom}");
    let config = Config::new(msg.owner, denom);
    
    CONFIG.save(deps.storage, &config)?;
    Ok(Response::default()
        .add_message(DenomMsg::Create {
            subdenom: msg.denom,
        }))
}
```

#### Minting & Burning

Nun, da Ihr Vertrag der Eigentümer des Denom ist, können wir Token minten und verbrennen. Hier ist ein Beispielvertrag, dessen Zweck es ist, eine berechtigte Liste von Adressen zu erstellen, die Coins minten und burnen können, anstatt nur den Administrator:

```rust
#[cfg_attr(not(feature = "library"), entry_point)]
pub fn execute(
    deps: DepsMut<KujiraQuery>,
    _env: Env,
    info: MessageInfo,
    msg: ExecuteMsg,
) -> Result<Response<KujiraMsg>, ContractError> {
    let mut c = CONFIG.load(deps.storage)?;
    match msg {
        ExecuteMsg::Burn {} => {
            let amt = amount(&c.denom, info.funds)?;
            c.validate_permitted(info.sender)?;

            Ok(Response::default()
                .add_attributes(vec![attr("action", "burn"), attr("amount", amt)])
                .add_message(CosmosMsg::Custom(KujiraMsg::Denom(DenomMsg::Burn {
                    denom: c.denom,
                    amount: amt,
                }))))
        }
        ExecuteMsg::Mint { amount, recipient } => {
            c.validate_permitted(info.sender)?;
            Ok(Response::default()
                .add_attributes(vec![attr("action", "mint"), attr("amount", amount)])
                .add_message(CosmosMsg::Custom(KujiraMsg::Denom(DenomMsg::Mint {
                    denom: c.denom,
                    amount,
                    recipient,
                }))))
        }
        ExecuteMsg::Permit { address } => {
            c.validate_owner(info.sender)?;
            c.permit(address.clone());
            CONFIG.save(deps.storage, &c)?;
            Ok(Response::default()
                .add_attributes(vec![attr("action", "permit"), attr("address", address)]))
        }
    }
}
```
