# 💠 Kernmodul-Konten

Auf der Kujira-Blockchain gibt es einige "Core Module"-Konten. Diese werden von Modulen innerhalb des Cosmos SDK und unseren eigenen maßgeschneiderten Modulen erstellt. Im Allgemeinen sind sie für den Aufbau der Kette von geringer Bedeutung, aber einige wenige, wie z. B. "fee_collector", sind für den Betrieb der Kette entscheidend;

## fee\_collector

[kujira17xpfvakm2amg962yls6f84z3kell8c5lp3pcxh](https://finder.kujira.app/kaiyo-1/address/kujira17xpfvakm2amg962yls6f84z3kell8c5lp3pcxh?p=1)

Dieses Konto dient als Inkassostelle für alle 3rd Part Fee im Kujira-Netzwerk. FIN, USK, Orca und alle anderen Protokolle senden Token an diese Adresse. Von hier aus werden sie an das Modul `Distribution` weitergeleitet, das sie unter den Validatoren und Stakern verteilt.

Dies wird häufig genug in intelligenten Verträgen verwendet, dass es in `kujira-rs` unter `kujra::utils::fee_address()` verfügbar ist.

## gov

[kujira10d07y265gmmuvt4z0w9aw880jnsr700jt23ame](https://finder.kujira.app/kaiyo-1/address/kujira10d07y265gmmuvt4z0w9aw880jnsr700jt23ame?p=1)

Dies ist die Adresse des Governance-Moduls. Wenn Sie privilegierte Aktionen für Ihren Vertrag festlegen müssen, die nur über einen Governance-Vorschlag aufgerufen werden können, verwenden Sie diese Adresse. Alternativ können Sie auch den `sudo`-Einstiegspunkt als besser geeignet ansehen.

Im Allgemeinen sollten Verträge mit dem `gov`-Modul als `admin`-Parameter instanziiert werden, um sicherzustellen, dass nachfolgende Code-Änderungen dem gleichen Governance-Prozess unterliegen wie die erste Instanziierung.&#x20;

##
