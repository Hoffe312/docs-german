# Preis-Orakel

Alle 30 Sekunden müssen alle 75 Validierer in der aktiven Gruppe von Kujira eine Transaktion posten, die den aktuellen Preis für eine Reihe von Vermögenswerten enthält, der aus einer Reihe von Quellen aggregiert wird. Dadurch wird eine On-Chain-Quelle der Wahrheit geschaffen, die über ein UI-Frontend, das CLI und auch in Smart Contracts abgefragt werden kann.

### REST

Sie können alle Wechselkurse hier abrufen: [https://lcd.kaiyo.kujira.setten.io/oracle/denoms/exchange\_rates](https://lcd.kaiyo.kujira.setten.io/oracle/denoms/exchange\_rates)

Oder nur einen bestimmten unter z.B. [https://lcd.kaiyo.kujira.setten.io/oracle/denoms/ATOM/exchange\_rate](https://lcd.kaiyo.kujira.setten.io/oracle/denoms/ATOM/exchange\_rate)

### CLI

```
kujirad query oracle exchange-rates
```

### CosmWASM

Das kujira-rs Paket bietet abfragbare Bindungen, um aktuelle Wechselkurse innerhalb Ihrer Smart Contracts abzurufen:

{% embed url="https://docs.rs/kujira/0.7.5/kujira/msg/enum.KujiraMsg.html" %}

```rust
use kujira::querier::KujiraQuerier;
use kujira::query::KujiraQuery;

pub fn query(deps: Deps<KujiraQuery>, env: Env, msg: QueryMsg) -> StdResult<Binary> {
    let q = KujiraQuerier::new(&deps.querier);
    let res = q.query_exchange_rate("ATOM".to_string())?;
    let price = res;
    Ok(to_binary(&price))
}
```

Note that the type signature for a `query` interface that uses this must be&#x20;

```rust
```
