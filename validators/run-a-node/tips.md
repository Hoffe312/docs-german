# 🗒 Tipps

### Festplattennutzung

Um die Festplattengröße zu verwalten, können Sie die beibehaltenen Blöcke prunen. Hierfür verwende ich Primzahlen - wählen Sie Ihre eigenen in "app.toml".

```
pruning = "custom"

# These are applied if and only if the pruning strategy is custom.
pruning-keep-recent = "809"
pruning-keep-every = "0"
pruning-interval = "43"
```

Sie sollten auch überprüfen, was Sie indizieren

```
index-events = ["tx.hash", "tx.height"]
```

### Hinzufügen weiterer Peers

Sie sollten `/etc/security/limits.conf` ändern und Folgendes hinzufügen

```
*                soft    nofile          65535
*                hard    nofile          65535
```

Sie können dann die config.toml ändern, um die Anzahl der Verbindungen zu erhöhen. Dies kann zu einer Erhöhung der Ingress/Egress-Gebühren führen.

```
max_open_connections = 1900
max_num_inbound_peers = 50
max_num_outbound_peers = 50
```
