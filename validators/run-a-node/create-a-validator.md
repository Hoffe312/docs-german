# 🛠 Erstelle einen Validator

## Einführung

Sobald Ihre Node synchronisiert ist, ist es an der Zeit, einen Validator zu erstellen. Dies kann auf einem separaten Rechner geschehen, falls gewünscht.

## Füge eine Validator-Wallet hinzu

1. Erstellen Sie die Wallet und ein Passwort für den Keyring. Ersetzen Sie "<Wallet>" durch einen beliebigen Namen, den Sie sich merken müssen!

```bash
kujirad keys add <wallet>
```
2. Bewahren Sie Ihren Wallet-Seed und Ihr Keyring-Passwort an einem sicheren Ort auf. Verwenden Sie den unten stehenden Befehl, wenn Sie sie jemals wiederherstellen müssen.

```bash
kujirad keys add --recover <wallet>
```

3. Besorgen Sie sich einige Token, um die ```create-validator```tx zu senden. Sie können Ihr Guthaben überprüfen über

```shell
kujirad query bank balances kujira....
```

Der nächste Teil ist die Verknüpfung Ihres Knotens mit Ihrem Konto, wodurch der Validator erstellt wird

## Sende die create-validator tx
- Ersetzen Sie `<moniker>` durch den Anzeigenamen Ihres Validators.
- Ersetzen Sie "<Wallet>" durch den Namen Ihres Validator-Wallets.
- Setze die `--commission` Werte auf Ihre gewünschten Provisionssätze.
- Ändern Sie die chain-id von "kaiyo-1", wenn das mainnet nicht das Ziel ist.

```bash
kujirad tx staking create-validator \
    --moniker=<moniker> \
    --amount=1000000ukuji \
    --pubkey=$(kujirad tendermint show-validator) \
    --from=<wallet> \
    --chain-id=kaiyo-1 \
    --commission-max-change-rate=0.02 \
    --commission-max-rate=0.15 \
    --commission-rate=0.05 \
    --min-self-delegation=1 \
    --gas=auto \
    --gas-prices=0.00125ukuji \
    --gas-adjustment=1.5
```

## Sichern Sie ihre Keys!
Es ist sehr wichtig, eine sichere Kopie des privaten Schlüssels Ihres Validators aufzubewahren, 
Damit können Sie den Validator wiederherstellen, falls Ihrem Server etwas zustößt.
Erstellen Sie ein Backup dieser Datei: `$HOME/.kujira/config/priv_validator_key.json`.
Wenn Sie diese Datei auf einem neuen Knoten wiederherstellen, wird er beginnen, Blöcke als Ihr Validator zu signieren.
Seien Sie sehr vorsichtig! Das gleichzeitige Signieren von zwei Nodes führt zu einem Hard Slash 
und einer Sperre.
