# 💡 Smart Contracts

Die Kujira-Blockchain ist vollständig mit CosmWASM v1.0 und IBC v3-Unterstützung ausgestattet, was es Ihnen als Entwickler ermöglicht, Rust-basierte Smart Contracts zu erstellen, die mit dem Haus interagieren.

## Erste Schritte

Wenn Sie CosmWASM und Smart Contracts noch nicht kennen, empfehlen wir Ihnen, sich das CW-Template Repo anzuschauen und sich mit den grundlegenden Schnittstellen und dem Modell der CosmWASM Smart Contracts vertraut zu machen.&#x20;

{% embed url="https://github.com/InterWasm/cw-template" %}

Wenn Sie bereit für den Einsatz sind, benötigen Sie einen lokalen Build des Chain-Kerns, um Ihren Code auf der Chain zu speichern und Governance-Vorschläge zu unterbreiten, um ihn zu instanziieren.

```bash
git clone https://github.com/Team-Kujira/core
cd core
make install
```

### Deployment

Sie sind nun bereit, Ihren Code auf der Chain zu speichern:

```bash
cargo run-script optimize
kujirad keys add <local-account>
# This will generate an account for you, provide you with a seed phrase which 
# you must store safely, and the associated kujira address. You will need to 
# fund this address with KUJI tokens before the next step
kujirad tx wasm store <./path/to/optimized/code.wasm> --from <local-account>
```

Dies erzeugt bei erfolgreicher Übertragung einige Protokolle, die die `code_id`:&#x20 enthalten;

```yaml
logs:
- events:
  - attributes:
    - key: action
      value: /cosmwasm.wasm.v1.MsgStoreCode
    - key: module
      value: wasm
    - key: sender
      value: kujira1...
    type: message
  - attributes:
    - key: code_id
      value: "4"
    type: store_code
```

### Instanziierung

Ihr Code ist nun in der Chain gespeichert. Sie können ihn sich ein wenig wie eine Klasse in einer objektorientierten Sprache vorstellen. Wir müssen nun eine Instanz davon erstellen, mit der wir interagieren können...&#x20;

Auf Kujira muss dies über einen Governance-Vorschlag geschehen, um die Qualität des Codes und der Dapps, die im Netzwerk laufen, aufrechtzuerhalten.&#x20;

```bash
kujirad tx gov submit-proposal instantiate-contract 4 \
  '{"count": 0}' \
  --title "Instantiate CW-Template" \
  --description "A more detailed description of this proposal" \
  --label "CW-Template" \
  --from <local-account> \
  --admin <local-account-address> \
  --run-as <local-account-address> \
  --gas 1000000 \
  --fees 1250ukuji
```
Dadurch wird der Vorschlag in einem Finanzierungsstadium erstellt. Sie können dann [https://blue.kujira.app/govern](https://blue.kujira.app/govern) besuchen, um den Vorschlag zu finanzieren und ihn zur Abstimmung zu öffnen.

Wenn die Abstimmung erfolgreich war, wird der Vertrag instanziiert und die Instanz des Vertrags erhält eine eigene Adresse. Sie können die Adresse wie folgt finden:


```bash
kujirad query wasm list-contract-by-code 4
```

Sie können nun mit der Erstellung der Benutzeroberfläche für Ihren neu eingerichteten Smart Contract fortfahren.

{% content-ref url="../dapp-front-ends/" %}
[dapp-front-ends](../dapp-front-ends/)
{% endcontent-ref %}

## Benutzerdefinierte Module

{% content-ref url="oracle.md" %}
[oracle.md](oracle.md)
{% endcontent-ref %}

{% content-ref url="token-factory.md" %}
[token-factory.md](token-factory.md)
{% endcontent-ref %}

{% content-ref url="scheduler.md" %}
[scheduler.md](scheduler.md)
{% endcontent-ref %}
