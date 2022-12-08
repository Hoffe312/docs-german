# 🥙 Oracle Price Feeder

## Einleitung

Validatoren werden benötigt, um Preisfeeds für das On-Chain-Oracle zu übermitteln.
Die App "price-feeder" kann Preisdaten von mehreren Anbietern abrufen und
Oracle-Votes übermitteln, um diese Aufgabe zu erfüllen.


## Vorraussetzungen

- Diese Anleitung setzt vorraus, dass Sie Ubuntu 22.04 verwenden.
- 'price-feeder' benötigt Zugriff auf die RPC- und gRPC-Ports einer laufenden Node.
  Diese Anleitung geht davon aus, dass Sie es auf dem gleichen Rechner haben und `localhost` verwenden
  mit den Standard-Ports 26657 und 9090. Ändern Sie diese in `config.toml` nach Bedarf.
- Diese Anleitung geht davon aus, dass Sie das Oracle für einen Mainnet (`kaiyo-1`) Validator konfigurieren.


## Benutzereinstellungen

Es empfiehlt sich, die Node-Software unter einem isolierten, unprivilegierten Benutzer auszuführen.
In dieser Anleitung erstellen wir den Benutzer `kujioracle`; wenn Ihr Benutzername anders ist 
ändern Sie ihn, wo immer er erscheint.

### Erstelle den Benutzer `kujioracle` 
```bash
sudo useradd -m -s /bin/bash kujioracle
```


## Einrichtung der Build-Umgebung

Wenn Sie bereits die [Run a node](/validators/run-a-node/run-a-node.md) Anleitung gefolgt sind, sind die einzigen Schritte, die Sie in diesem Abschnitt benötigen, **Install go toolchain** #2 und #3 . 
Die Wiederholung der anderen Schritte kann nicht schaden, wenn Sie sich sicher sein wollen.

### Installieren von Build-Paketen
```bash
sudo apt install -y build-essential git unzip curl
```

### Installiere go toolchain
1. Downloade und extrahiere go 1.18.5.
```bash
curl -fsSL https://golang.org/dl/go1.18.5.linux-amd64.tar.gz | sudo tar -xzC /usr/local
```
2. Login als `kujioracle`.
```bash
sudo su -l kujioracle
```
3. Konfiguriere die Umgebungsvariablen für `kujioracle`.
```bash
cat <<EOF >> ~/.bashrc
export GOROOT=/usr/local/go
export GOPATH=\$HOME/go
export GO111MODULE=on
export PATH=\$PATH:/usr/local/go/bin:\$HOME/go/bin
EOF
source ~/.bashrc
go version  # should output "go version go1.18.5 linux/amd64"
```


## Build `price-feeder`

1. Login als `kujioracle` (überspringe dies, falls Sie schon eingeloggt sind).
```bash
sudo su -l kujioracle
```
2. Build `kujirad` v0.7.1. Wir werden die Binärdatei verwenden, um die Keyring-Datei zu erstellen.
```bash
git clone https://github.com/Team-Kujira/core
cd core
git checkout v0.7.1
make install
cd ..
kujirad version  # should output "0.7.1"
```
3. Build `price-feeder`.
```bash
git clone https://github.com/Team-Kujira/oracle-price-feeder
cd oracle-price-feeder
make install
price-feeder version
```


## Konfiguriere `price-feeder`

### Erstelle ein Wallet
Dieses Wallet ist relativ unsicher, lagere dort deshalb nur das Guthaben, was zum senden von Votes benötigt wird.

1. Login als `kujioracle` (überspringe dies, falls Sie schon eingeloggt sind).
```bash
sudo su -l kujioracle
```
2. Erstellen Sie das Wallet und ein Passwort für den Keyring.
```bash
kujirad keys add oracle
```
3. Konfigurieren Sie das `keyring-file` directory. Das erlaubt es dem `price-feeder` 
den Keyring zu finden.
```bash
mkdir ~/.kujira/keyring-file
mv ~/.kujira/*.info ~/.kujira/*.address ~/.kujira/keyring-file
```

### Einstellen der Feeder-Delegation
Mit diesem Schritt wird Ihre neue Wallet autorisiert, Oracle-Votes im Namen Ihres Validators zu senden.
Die Transaktion sollte von der Validator-Wallet gesendet werden, also führen Sie sie auf einer Node aus, auf der sie verfügbar ist.

- Ersetzen Sie `<oracle_wallet>` durch Ihre neue Wallet-Adresse.
- Ersetzen Sie `<validator_wallet>` durch den Namen Ihrer Validator-Wallet.

```bash
kujirad tx oracle set-feeder <oracle_wallet> --from <validator_wallet> --fees 250ukuji
```

### Erstelle die config-Datei
1. Login als `kujioracle` (skip if you're already logged in).
```bash
sudo su -l kujioracle
```
2. Erstellen Sie die Konfigurationsdatei mit Ihrem bevorzugten Texteditor (z. B. `nano`).
    - Ersetzen Sie "<wallet_adress>" durch Ihre Oracle-Wallet-Adresse (e.g. `kujira16jchc8l8hfk98g4gnqk4pld29z385qyseeqqd0`)
    - Ersetzen Sie `<Validator_address>` durch Ihre Validator-Adresse (e.g. `kujiravaloper1e9rm4nszmfg3fdhrw6s9j69stqddk7ga2x84yf`)

    ```toml title="~/config.toml"  
    gas_adjustment = 1.5
    gas_prices = "0.00125ukuji"
    enable_server = true
    enable_voter = true
    provider_timeout = "500ms"

    [server]
    listen_addr = "0.0.0.0:7171"
    read_timeout = "20s"
    verbose_cors = true
    write_timeout = "20s"

    [[deviation_thresholds]]
    base = "USDT"
    threshold = "2"

    [[currency_pairs]]
    base = "ATOM"
    providers = [
      "binance",
      "kraken",
      "osmosis",
    ]
    quote = "USD"

    [account]
    address = "<wallet_address>"
    chain_id = "kaiyo-1"
    validator = "<validator_address>"
    prefix = "kujira"

    [keyring]
    backend = "file"
    dir = "/home/kujioracle/.kujira"

    [rpc]
    grpc_endpoint = "localhost:9090"
    rpc_timeout = "500ms"
    tmrpc_endpoint = "http://localhost:26657"

    [telemetry]
    enable_hostname = true
    enable_hostname_label = true
    enable_service_label = true
    enabled = true
    global_labels = [["chain_id", "kaiyo-1"]]
    service_name = "price-feeder"
    type = "prometheus"

    [[provider_endpoints]]
    name = "binance"
    rest = "https://api1.binance.us"
    websocket = "stream.binance.us:9443"
    ```

#### Konfigurieren Sie die Währungspaare
Die in der obigen Konfiguration angegebenen `[[currency_pairs]]` sind nur ein Beispiel, jeder Validator
sollte dies ändern, um Preise für die von der Kette zugelassenen Denominationen zu übermitteln. Halten Sie ein Auge
für Governance-Vorschläge zur Einführung neuer Denominationen.

**NOTIZ:** Es ist wichtig, dass die Währungspaare in dieser Konfiguration genau mit denen in der aktuell konfigurierten Whitelist für die Chain übereinstimmen:

* Testnet: [harpoon-4](https://lcd.harpoon.kujira.setten.io/oracle/params)
* Mainnet: [kaiyo-1](https://lcd.kaiyo.kujira.setten.io/oracle/params)

Sie können die Oracle-Parameter auch mit `kujirad` abfragen

```
kujirad query oracle params
```


## Erweiterte Einstellungen

### Provider Endpoints
Es ist möglich, Standard-Provider-Endpunkte zu überschreiben (z.B. um auf einen anderen Mirror zu zeigen), indem man sie in `[[provider_endpoints]]` angibt.

#### Beispiel für Binance.US

```
[[provider_endpoints]]
name = "binance"
rest = "https://api.binance.us"
websocket = "stream.binance.us:9443"
```

### Adressen & stimmberechtigter Vertreter
Der Preisfeeder reicht im Namen Ihres Validators Transaktionen ein, die Preise für bestimmte Stückelungen enthalten. Das Feeder-Konto muss über genügend Mittel
verfügen, um das Gas für diese automatischen Abstimmungstransaktionen dauerhaft zu bezahlen. Aufgrund der Art und Weise, wie dies durch den Preis-Feeder durchgeführt
wird, ist es sehr empfehlenswert, ein delegiertes Feeder-Konto anstelle des Validator-Kontos zu verwenden, da der Wallet-Schlüssel des Feeder-Kontos potenziell stärker
gefährdet ist.

```
[account]
address = "kujira...." # feeder wallet address
chain_id = "<kaiyo-1>"
validator = "kujiravaloper...." # validator address
prefix = "kujira"
```

Vergewissern Sie sich, dass die `validator`-Adresse auf [Ihre Validator-Addresse](https://github.com/Team-Kujira/oracle-price-feeder/blob/master/config.example.toml#L52) eingestellt ist.

Vergewissern Sie sich, dass die `address` auf [Ihr feeder Wallet](https://github.com/Team-Kujira/oracle-price-feeder/blob/master/config.example.toml#L50) eingestellt ist. Standardmäßig ist dies die gleiche Adresse wie die `kujira'-Adresse für Ihren Validator-Schlüssel. Es ist jedoch wahrscheinlich, dass Sie einen delegierten Feeder-Account für Ihren Validator einrichten möchten, so dass Sie Price-Feeder auf einem separaten Benutzerkonto oder einem separaten Server mit unterschiedlichen Schlüsseln ausführen können.


So legen Sie einen Delegierten fest:

```
kujirad tx oracle set-feeder kujira1243.....
```

### Keyring Backend

Je nach Ihrer Infrastruktur haben Sie möglicherweise eine andere Präferenz für Ihr Keyring-Backend. Wenn Sie Price-Feeder auf einem separaten Konto oder einem separaten Server betreiben, können Sie mit verschiedenen Keyring-Backends experimentieren, ohne dass Ihr laufender Validator gefährdet wird - Sie riskieren jedoch, dass Ihre Validator-Schlüssel beschädigt werden, wenn Sie kein separates Konto oder keinen separaten Server für Price-Feeder verwenden (auch diese Konfiguration wird nicht empfohlen)

[https://github.com/Team-Kujira/oracle-price-feeder/blob/master/config.example.toml#L55-L57](https://github.com/Team-Kujira/oracle-price-feeder/blob/master/config.example.toml#L55-L57)

#### OS

```toml
[keyring]
backend = "os"
# dir is ignored for os
dir = "/home/kuji/.kujira"
```

#### DATEI

**Erstelle eine Keyring-Datei**

```bash
kujirad keys add oracle
mkdir ~/.kujira/keyring-file
mv ~/.kujira/*.address ~/.kujira/*.info ~/.kujira/keyring-file
```

**Setzen Sie die Passwortvariable**

```bash
export PRICE_FEEDER_PASS=<keyring_password>
```

**Update config.toml**

```toml
[keyring]
backend = "file"
dir = "/home/kuji/.kujira"
```

#### PASS

Möglicherweise müssen Sie zuerst `pass` installieren:

[https://www.passwordstore.org](https://www.passwordstore.org)

Und dann setzen Sie Ihre Passphrase auf env.

```
export PRICE_FEEDER_PASS=...  
```

```toml
[keyring]
backend = "pass"
# dir is ignored for pass
dir = "/home/kuji/.kujira"
```

### RPC Endpoints
Wenn price-feeder auf demselben Server wie Ihre Node läuft und Ihre Node die Standardports verwendet, sollte die Standard-RPC-Konfiguration funktionieren.

Wenn Sie eine andere Nodekonfiguration verwenden, müssen Sie diese RPC-Einstellungen möglicherweise an Ihre Infrastruktur anpassen.

```
[rpc]
grpc_endpoint = "localhost:9090"
rpc_timeout = "100ms"
tmrpc_endpoint = "http://localhost:26657"
```

### Telemetrie & Prometheus
Telemetre wird von dem [Cosmos SDK Telemetry module](https://github.com/cosmos/cosmos-sdk/blob/main/docs/core/telemetry.md) bereitgestellt. Um Metriken von einem laufenden Price-Feeder abzufragen, leiten Sie die Ausgabe an jq.

`curl "http://localhost:7171/api/v1/metrics" | jq`

Um Preis-Feeder-Metriken im Prometheus-Format zu veröffentlichen, muss die config.toml das Flag `prometheus_retention` enthalten

Beispiel für einen Telemetrie-Konfigurationsblock mit aktiviertem Prometheus-Format:

```
[telemetry]
enable_hostname = true
enable_hostname_label = true
enable_service_label = true
enabled = true
global_labels = [["chain_id", "kaiyo-1/harpoon-4"]]
service_name = "price-feeder"
type = "prometheus"
prometheus_retention = 120
```

Beispiel scrape\_config in Prometheus:

```
scrape_configs:
  - job_name: price-feeder
    metrics_path: '/api/v1/metrics'
    params:
      format: ['prometheus']
    static_configs:
      - targets:
          - <price-feeder-IP-addr>:7171
        labels: {}
```


## Run `price-feeder`

1. Login as `kujioracle` (skip if you're already logged in).
```bash
sudo su -l kujioracle
```
2. Run the app, providing the password on `stdin`.
```bash
echo <keyring_password> | price-feeder ~/config.toml
```

## Erstelle einen Service

Ein systemd-Dienst sorgt dafür, dass `price-feeder` im Hintergrund weiterläuft und startet ihn neu, wenn er anhält.

1. Erstellen Sie die Servicedatei mit `sudo` in Ihrem bevorzugten Texteditor. 
Ersetzen Sie `<keyring_password>` durch das von Ihnen erstellte Passwort.
```ini title="/etc/systemd/system/kujira-price-feeder.service"
[Unit]
Description=kujira-price-feeder
After=network.target

[Service]
Type=simple
User=kujioracle
ExecStart=/home/kujioracle/go/bin/price-feeder /home/kujioracle/config.toml --log-level debug
Restart=on-abort
LimitNOFILE=65535
Environment="PRICE_FEEDER_PASS=<keyring_password>"

[Install]
WantedBy=multi-user.target
```
2. Reload `systemd` to pick up the new service.
```bash
sudo systemctl daemon-reload
```
3. Start the service.
```bash
sudo systemctl start kujira-price-feeder
```
4. Tail your service logs.
```bash
sudo journalctl -fu kujira-price-feeder
```
5. (Optional) Enable the service. This will set it to start on every boot.
```bash
sudo systemctl enable kujira-price-feeder
```
