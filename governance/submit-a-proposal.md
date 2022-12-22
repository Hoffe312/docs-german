---
Beschreibung: Einreichung eines Vorschlags für die Ausgaben des Gemeinschaftspools
---

# 🗄 Antrag einreichen

Reichen Sie einen Vorschlag für Ausgaben für den Gemeinschaftspool zusammen mit einer ersten Anzahlung ein. Die Einzelheiten des Vorschlags müssen in einer JSON-Datei angegeben werden.

Beispiel:

`$ kujirad tx gov submit-proposal community-pool-spend <path/to/proposal.json> --from=<key_or_address>`

Wobei proposal.json enthält:

```
{
  "title": "Community Pool Spend",
  "description": "Protokoll Launch Application",
  "recipient": "kujira1s5afhd6gxevu37mkqcvvsj8qeylhn0rz46zdlq",
  "amount": "10000000ukuji",
  "deposit": "10000000ukuji"
}
```

`kujirad tx gov submit-proposal community-pool-spend [proposal-file] [flags]`

### Optionen

```
  -a, --account-number uint Die Kontonummer des unterzeichnenden Kontos (nur im Offline-Modus)
  -b, --broadcast-mode string Transaktions-Broadcasting-Modus (sync|async|block) (Standard "sync")
      --dry-run ignoriert das --gas-Flag und führt eine Simulation einer Transaktion durch, sendet sie aber nicht
      --fee-account string Gebührenkonto zahlt Gebühren für die Transaktion, anstatt sie vom Unterzeichner abzuziehen
      --fees string Mit der Transaktion zu zahlende Gebühren; z.B.: 10uatom
      --from string Name oder Adresse des privaten Schlüssels, mit dem signiert werden soll
      --gas string Gaslimit, das pro Transaktion festgelegt wird; auf "auto" gesetzt, um automatisch genügend Gas zu berechnen (Standardwert 200000)
      --gas-adjustment float Anpassungsfaktor, der mit der von der tx-Simulation zurückgegebenen Schätzung zu multiplizieren ist; wird die Gasgrenze manuell festgelegt, wird dieses Flag ignoriert (Standardwert 1)
      --gas-prices string Gaspreise im Dezimalformat zur Bestimmung der Transaktionsgebühr (z. B. 0,1uatom)
      --generate-only Erstellt eine unsignierte Transaktion und schreibt sie auf STDOUT (wenn diese Option aktiviert ist, ist die lokale Keybase nicht zugänglich)
  -h, --help Hilfe für community-pool-spend
      --keyring-backend string Wähle das Backend des Keyrings (os|file|kwallet|pass|test|memory) (Standard "os")
      --keyring-dir string Das Keyring-Verzeichnis des Clients; wird es nicht angegeben, wird das Standardverzeichnis "home" verwendet
      --ledger Verwendet ein angeschlossenes Ledger-Gerät
      --node string <host>:<port> zur tendermint rpc-Schnittstelle für diese Kette (standardmäßig "tcp://localhost:26657")
      --note string Notiz, um der Transaktion eine Beschreibung hinzuzufügen (früher --memo)
      --offline Offline-Modus (lässt keine Online-Funktionalität zu)
  -o, --output string Ausgabeformat (text|json) (Standard "json")
  -s, --sequence uint Die Sequenznummer des Signierkontos (nur im Offline-Modus)
      --sign-mode string Wählen Sie den Signiermodus (direct|amino-json), dies ist eine erweiterte Funktion
      --timeout-height uint Legt eine Block-Timeout-Höhe fest, um zu verhindern, dass der tx über eine bestimmte Höhe hinaus übertragen wird
  -y, --yes Überspringt die Bestätigung der tx-Übertragung
```

Sobald ein Vorschlag eingereicht wurde, gibt es ein Einzahlungsfenster von 24 Stunden, in dem ein Schwellenwert von 10.000 KUJI von der Brieftasche des Vorschlagsgebers oder eines Community-Mitglieds erreicht werden muss, damit der Vorschlag auf der Kette hinterlegt wird und die Abstimmungsphase (48 Stunden) beginnt.

Das UI auf https://blue.kujira.app/govern unter jedem Vorschlag ermöglicht es den Nutzern, die Vorschläge, an die sie glauben, mit dem erforderlichen Einzahlungsbetrag zu unterstützen.

Die eingezahlten KUJI-Tokens müssen liquide sein (nicht abgesichert) und werden nach Erreichen der 10.000 KUJI-Schwelle an jede Wallet zurückgegeben. Wenn der Schwellenwert von 10.000 KUJI während der 24-stündigen Einzahlungsperiode nicht erreicht wird, werden die eingezahlten Token verbrannt.
