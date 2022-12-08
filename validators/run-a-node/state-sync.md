# Kujira Statesync

## Einleitung
Statesync hilft Ihnen, schnell loszulegen, indem es den Anwendungsstatus auf einem aktuellen Stand synchronisiert und historische Daten überspringt.

## Schnellstart
Die Node sollte für diesen Prozess nicht laufen (stoppen Sie den Service, wenn Sie einen haben).

1. Installieren Sie die Dependencies, wenn Sie sie nicht bereits haben.
```bash
sudo apt install -y curl jq
```
2. Melden Sie sich als `kuji` an. Ändern Sie den Benutzer, wenn er anders ist, oder überspringen Sie ihn, wenn Sie bereits eingeloggt sind.
```bash
sudo su -l kuji
```
3) (Optional) Wenn Ihr Knoten bereits synchronisierte Daten hat, setzen Sie sie zurück. Sichern Sie Ihre Schlüssel!
```bash
kujirad --home $HOME/.kujira tendermint unsafe-reset-all
```
4. Führen Sie diese Befehle aus, um `config.toml` mit den aktuellen Statesync-Einstellungen zu ändern.
```bash
RPC=https://rpc-kujira.mintthemoon.xyz:443
LATEST_HEIGHT=$(curl -s $RPC/block | jq -r .result.block.header.height)
TRUST_HEIGHT=$((LATEST_HEIGHT - 2000))
TRUST_HASH=$(curl -s "$RPC/block?height=$TRUST_HEIGHT" | jq -r .result.block_id.hash)
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$RPC,$RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$TRUST_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" "$HOME/.kujira/config/config.toml"
```
5. Starten Sie Statesyncing! Dieser Befehl wird automatisch beendet, sobald die Node synchronisiert ist.
```bash
kujirad start --halt-height $LATEST_HEIGHT
```
6. Stellen Sie die ursprüngliche `config.toml` wieder her, um statesync zu deaktivieren.
```bash
mv $HOME/.kujira/config/config.toml.bak $HOME/.kujira/config/config.toml
```
7. Statesync ist abgeschlossen, starten Sie Ihren Knoten!
```bash
kujirad start
```
