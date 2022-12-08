# 🕶 Cosmovisor-Einrichtung

Die Einrichtung von Cosmovisor ist relativ einfach. Allerdings müssen bestimmte Umgebungsvariablen und Ordnerstrukturen festgelegt werden.

Cosmovisor ermöglicht es Ihnen, Binärdateien für Chain-Upgrades im Voraus herunterzuladen, was bedeutet, dass Sie Chain-Upgrades ohne (oder fast ohne) Ausfallzeiten durchführen können. Dies ist auch dann nützlich, wenn Ihre lokale Zeitzone bedeutet, dass ein Chain-Upgrade zu einem ungünstigen Zeitpunkt durchgeführt wird.

Es ist immer besser, wenn Sie diese Aufgaben automatisieren können, als sie spät in der Nacht erledigen zu müssen, und genau das versucht Cosmovisor zu tun.

## Installation

Holen Sie sich zunächst cosmovisor (empfohlene Vorgehensweise):

```bash
go get github.com/cosmos/cosmos-sdk/cosmovisor/cmd/cosmovisor

# or, with go >= 1.15 you can do
go install github.com/cosmos/cosmos-sdk/cosmovisor/cmd/cosmovisor@latest

# to target a specific version:
go install github.com/cosmos/cosmos-sdk/cosmovisor/cmd/cosmovisor@v1.0.0
```

{% hint style="danger" %}
When using cosmovisor, make sure that you do not have auto download of binaries on.
{% endhint %}

Ihre Installation kann mit bestätigt werden:

```bash
which cosmovisor
```

Das Ergebnis ist etwa so:

```bash
/home/<your-user>/go/bin/cosmovisor
```

{% hint style="info" %}
Die Erstellung aus dem Quellcode ermöglicht es Ihnen, eine bestimmte Version von Cosmovisor anzusteuern, falls Sie die Version 1.0.0 noch nicht verwenden möchten.
{% endhint %}

Sie können auch aus dem Quellcode bauen; Cosmovisor befindet sich in der Hauptversion `cosmos-sdk` auf Github, so dass Sie Git-Tags verwenden können, um eine bestimmte Version zu erreichen. In diesem Beispiel wird das Tag `v0.42.7` verwendet, das sich auf das Cosmos SDK bezieht, da es vor August 2021 keine Cosmovisor-spezifischen Tags gab. Die erste dieser Versionen war `cosmovisor/v0.1.0` und die zweite ist die aktuelle Version, `cosmovisor/v1.0.0`.

```bash
git clone https://github.com/cosmos/cosmos-sdk
cd cosmos-sdk
git checkout v0.42.7
export PATH=$PATH:/usr/local/go/bin
export PATH=$PATH:$(go env GO cosmovisor/cosmovisor $GOPATH/bin/cosmovisor
cd $HOME
```

## Umgebungsvariablen zu Ihrer Shell hinzufügen

In der Datei `.profile`, die sich normalerweise unter `~/.profile` befindet, fügen Sie hinzu:

```bash
export DAEMON_NAME=kujirad
export DAEMON_HOME=$HOME/.kujira
```

Geben Sie dann Ihr Profil ein, um Zugang zu diesen Variablen zu erhalten:

```bash
source ~/.profile
```

Sie können den Erfolg folgendermaßen bestätigen:

```
echo $DAEMON_NAME
```

Es sollte `kujirad` zurückgeben.

## Ordnerstruktur einrichten

Cosmovisor erwartet eine bestimmte Ordnerstruktur:

```bash
.
├── current -> genesis or upgrades/<name>
├── genesis
│   └── bin
│       └── $DAEMON_NAME
└── upgrades
    └── <name>
        └── bin
            └── $DAEMON_NAME
```

Machen Sie sich keine Sorgen um `current` - das ist einfach ein Symlink, der von Cosmovisor verwendet wird. Die anderen Ordner müssen eingerichtet werden, aber das ist
einfach:


```bash
mkdir -p $DAEMON_HOME/cosmovisor/genesis/bin
mkdir -p $DAEMON_HOME/cosmovisor/upgrades
```

## Genesis Binary einrichten

Cosmovisor muss wissen, welche Binärdatei bei Genesis zu verwenden ist. Wir legen diese in `$DAEMON_HOME/cosmovisor/genesis/bin` ab.

Suchen Sie zunächst den Speicherort der zu verwendenden Binärdatei:


```bash
which kujirad
```

Verwenden Sie dann den zurückgegebenen Pfad, um ihn in das von Cosmovisor erwartete Verzeichnis zu kopieren. Nehmen wir an, dass der vorherige Befehl `/home/your-
user/go/bin/ kujirad` zurückgegeben hat:

```bash
cp /home/<your-user>/go/bin/kujirad $DAEMON_HOME/cosmovisor/genesis/bin
```

Überprüfen Sie anschließend mit einem Tool wie `tree`, ob die Ordnerstruktur korrekt aussieht.

## Service einrichten

An Cosmovisor gesendete Befehle werden an die zugrunde liegende Binärdatei gesendet. So ist beispielsweise die Eingabe von "Cosmovisor Version" dasselbe wie die Eingabe von "Kujirad Version".

Genauso wie wir `kujirad` mit einem Prozessmanager verwalten würden, möchten wir sicherstellen, dass Cosmovisor automatisch neu gestartet wird, wenn etwas passiert, zum Beispiel ein Fehler oder ein Neustart.

Erstellen Sie zunächst die Servicedatei:

```bash
sudo nano /etc/systemd/system/cosmovisor.service
```

Ändern Sie den Inhalt der untenstehenden Datei, um sie an Ihr Setup anzupassen - `cosmovisor` befindet sich wahrscheinlich unter `~/go/bin/cosmovisor`, unabhängig davon, welchen Installationspfad Sie oben gewählt haben, aber es lohnt sich, das zu überprüfen.

```
[Unit]
Description=cosmovisor
After=network-online.target

[Service]
User=<your-user>
ExecStart=/home/<your-user>/go/bin/cosmovisor start
Restart=always
RestartSec=3
LimitNOFILE=4096
Environment="DAEMON_NAME=kujirad"
Environment="DAEMON_HOME=/home/<your-user>/.kujira"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_LOG_BUFFER_SIZE=512"

[Installation]
WantedBy=multi-user.target
```

{% hint style="info" %}
Eine Beschreibung, was die Umgebungsvariablen bewirken, finden Sie [hier](https://docs.cosmos.network/master/run-node/cosmovisor.html). Ändern Sie sie je nach Ihren Einstellungen.
{% endhint %}

Beachten Sie auch, dass wir die Puffergröße aufgrund eines [Live-Bugs in Cosmovisor] (https://github.com/cosmos/cosmos-sdk/pull/8590) vor Version `v1.0.0` explizit
festlegen. Wenn Sie `v1.0.0` verwenden, können Sie diese Zeile weglassen.

Darüber hinaus kann das gleiche Problem behoben werden, indem das Protokoll über die env-Variable reduziert wird. Wenn Sie unsicher sind, fragen Sie auf Discord.

## Cosmovisor starten

{% hint style="warning" %}
Wenn Sie von einem Snapshot synchronisieren, starten Sie Cosmovisor noch nicht.
{% endhint %}

Aktivieren Sie schließlich den Dienst und starten Sie ihn.

```bash
sudo -S systemctl daemon-reload
sudo -S systemctl enable cosmovisor
sudo systemctl start cosmovisor
```

Prüfen Sie, ob es läuft:

```
sudo systemctl status cosmovisor
```

Wenn Sie den Dienst nach dem Start überwachen müssen, können Sie die Protokolle mit anzeigen:

```bash
sudo journalctl -u cosmovisor -f
```
