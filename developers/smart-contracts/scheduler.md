# 🗓 Scheduler

Der On-Chain-Scheduler ermöglicht es, am Ende jedes N-ten Blocks eine automatisierte Vertragsausführung anzuhängen. Diese werden über Governance instanziiert, da sie jedem Block Rechenlast und Gaskosten hinzufügen und daher verwaltet werden müssen;

Aktuelle geplante Aufgaben können über [https://lcd.kaiyo.kujira.setten.io/kujira/scheduler/hook](https://lcd.kaiyo.kujira.setten.io/kujira/scheduler/hook) oder `kujirad query scheduler list-hooks` abgefragt werden.

Und ein Vorschlag für einen Hook kann so eingereicht werden:

```bash
kujirad tx gov submit-proposal create-hook <contract-addr> <executor-addr> \
  '{"count": 0}' \
  <frequency> \ # i.e. every Nth block 
  1000ukuji \ # funds to provide with each execution
  --title "Instantiate CW-Template" \
  --description "A more detailed description of this proposal" \
  --from <local-account> \
  --fees 250ukuji
```

