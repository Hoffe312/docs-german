# Run a Node

Diese Anleitung geht davon aus, dass Sie mit Ubuntu 22.04 LTS arbeiten - die Befehle funktionieren wahrscheinlich auch mit 20.04 LTS oder Debian.

## Grundlegende Maschineneinrichtung

1. SSH in Ihre Node
2. Aktualisieren Sie Ihren Rechner (Beantworten Sie die Aufforderungen mit yes / ok)

```
sudo apt update && sudo apt dist-upgrade -y
```

3. Installieren Sie die erforderlichen Werkzeuge

```
sudo apt install build-essential git unzip curl wget
```

## Umgebung vorbereiten

Die beste Vorgehensweise ist, die Node-Software unter einem isolierten, unprivilegierten Benutzer auszuführen. Wir werden in dieser Anleitung den Benutzer ```kuji`` anlegen; wenn Ihr Benutzername anders ist, ändern Sie ihn, wo immer er auftaucht. 

```
sudo useradd -m -s /bin/bash kuji
```

### Installiere Golang

Downloade `go` und extrahiere go 1.18.5

```
# entferne die alte go version

sudo rm -rvf /usr/local/go/

# downloade und installiere die aktuelle go Version

curl -fsSL https://golang.org/dl/go1.18.5.linux-amd64.tar.gz | sudo tar -xzC /usr/local

# Unnötige Installationen entfernen
rm go1.18.5.linux-amd64.tar.gz

# source go
cat <<EOF >> ~/.profile
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
EOF
source ~/.profile
go version
```

## Build node binary

1. Login ass ```kuji```(Überspringen, wenn Sie es bereits getan haben)

```
sudo su -l kuji
```

2. Laden Sie den Projektcode herunter und checken Sie in den Mainnet-Branch aus.

```bash
git clone https://github.com/Team-Kujira/core $HOME/kujira-core
cd $HOME/kujira-core
git checkout v0.7.1
make install
```

3. Überprüfen Sie, ob Ihr Binärprogramm funktioniert

```
kujirad version #should output v0.7.1
```

## Konfiguriere die chain

Dieser Abschnitt ist für das Hauptnetz (kaiyo-1) geschrieben; ändern Sie die ID, Genesis und Seeds nach Bedarf.

1. Login als kuji (überspringen Sie es, wenn Sie bereits eingeloggt sind).

```
sudo su -l kuji
```

2. Initialisieren Sie die Konfigurationsdateien und Verzeichnisse. Ersetzen Sie <moniker> durch einen öffentlichen Namen für Ihre Node.

```
kujirad init --chain-id kaiyo-1 <moniker>
```
3. Downloade die kaiyo-1 genesis.json Datei.

```
curl -fsSL -o $HOME/.kujira/config/genesis.json https://raw.githubusercontent.com/Team-Kujira/networks/master/mainnet/kaiyo-1.json
```
4. Stelle die chain-id ein. Dadurch wird sie in client.toml gespeichert, so dass Sie --chain-id nicht mehr benötigen.

```
kujirad config chain-id kaiyo-1
```

5. Setzen Sie einige Standardwerte in config.toml und app.toml.

```
sed -i 's/^timeout_commit =.*/timeout_commit = "1500ms"/' $HOME/.kujira/config/config.toml
sed -i "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.00119ukuji,0.00150ibc\/295548A78785A1007F232DE286149A6FF512F180AF5657780FC89C009E2C348F,0.000125ibc\/27394FB092D2ECCD56123C74F36E4C1F926001CEADA9CA97EA622B25F41E5EB2,0.00126ibc\/47BD209179859CDE4A2806763D7189B6E6FE13A17880FE2B42DE1E6C1E329E23,0.00652ibc\/3607EB5B5E64DD1C0E12E07F077FF470D5BC4706AFCBC98FE1BA960E5AE4CE07,617283951ibc\/F3AA7EF362EC5E791FE78A0F4CCC69FEE1F9A7485EB1A8CAB3F6601C00522F10,0.000288ibc\/EFF323CC632EC4F747C61BCE238A758EFDB7699C3226565F7C20DA06509D59A5,5ibc\/DA59C009A0B3B95E0549E6BF7B075C8239285989FF457A8EDDBB56F10B2A6986,0.00137ibc\/A358D7F19237777AF6D8AD0E0F53268F8B18AE8A53ED318095C14D6D7F3B2DB5,0.0488ibc\/4F393C3FCA4190C0A6756CE7F6D897D5D1BE57D6CCB80D0BC87393566A7B6602,78492936ibc\/004EBF085BBED1029326D56BE8A2E67C08CECE670A94AC1947DF413EF5130EB2,964351ibc\/1B38805B1C75352B28169284F96DF56BDEBD9E8FAC005BDCC8CF0378C82AA8E7\"/;" $HOME/.kujira/config/app.toml
```

6. (Optional) Konfigurieren Sie einige Seeds. Dies wird Ihrer Node helfen, Peers zu finden.

```
sed -i 's/^seeds =.*/seeds = "[email protected]rg:31897,[email protected]:11856"/' $HOME/.kujira/config/config.toml
```

## Starte die Node

Die Node ist jetzt einsatzbereit

1. (Optional) [State-sync](/validators/run-a-node/state-sync.md) wenn Sie einen Vorsprung gegenüber der Synchronisierung von Grund auf haben wollen.

2. Start der Blocksynchronisation

```bash
kujirad start
```

Und dann beobachten Sie eine ganze Reihe von Protokollmeldungen, während Ihre Node den Rückstand aufholt. Nachdem Sie sich vergewissert haben, dass es funktioniert, ist es an der Zeit, es als Dienst auf Systemebene zu installieren, damit es immer mit der Maschine startet.

## Die Node als Service registrieren

Ein systemd-Dienst sorgt dafür, dass `kujirad` im Hintergrund weiterläuft und startet es neu, wenn es anhält.

1. Erstellen Sie die Dienstdatei mit `sudo` unter Verwendung Ihres bevorzugten Texteditors in `/etc/systemd/system/kujirad.service`.

```
[Unit]
Description=kujirad
After=network.target

[Service]
Type=simple
User=kuji
ExecStart=/home/kuji/go/bin/kujirad start
Restart=on-abort
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target  
```

2. Laden Sie `systemd` neu, um den neuen Dienst zu übernehmen.
```bash
sudo systemctl daemon-reload
```
3. Starte den Service.
```bash
sudo systemctl start kujirad
```
4. Verfolgen Sie Ihre Serviceprotokolle
```bash
sudo journalctl -fu kujirad
```
5. (Optional) Aktivieren Sie den Dienst. Dadurch wird er bei jedem Systemstart gestartet.
```bash
sudo systemctl enable kujirad
```
