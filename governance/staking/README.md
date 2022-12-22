# 🥩 Staking

Staking ist eine Kernkomponente der Kujira-Blockchain. Es ist auch ein grundlegender Aspekt, der das zugrundeliegende Wertversprechen von KUJI vorantreibt, über das Sie [hier](staking-overview.md) mehr lesen können. Es ist auch ein wichtiger Teil der Sicherung der Kujira-Blockchain, bei der es um die Bindung von KUJI an bestimmte Validatoren geht.

Bevor wir tiefer in das Thema Staking eintauchen, hilft es zunächst, den Kujira-Blockchain-Konsens zu erörtern.&#x20;

### Konsens

Als Proof-of-Stake-Blockchain ist das Kujira-Netzwerk durch ein Verifizierungssystem namens Tendermint Consensus gesichert und wird durch das [Cosmos SDK](https://cosmos.network/) unterstützt;

Die folgende Abfolge von Schritten gibt eine einfache Aufschlüsselung der Tendermint Consensus funktioniert:

1. Ein Validator, genannt Proposer, wird ausgewählt, um einen neuen Transaktionsblock einzureichen.&#x20;
2. Die Validatoren stimmen in zwei Runden darüber ab, ob sie den vorgeschlagenen Block akzeptieren oder ablehnen. Wird er abgelehnt, wird ein neuer Antragsteller ausgewählt und der Prozess beginnt von neuem;
3. Wird der Block akzeptiert, wird er signiert und der Kette hinzugefügt.&#x20;
4. Die Transaktions- und Gasgebühren des Blocks werden an die Treasury und an die Validierer und Delegierer als Belohnung für den Einsatz verteilt. Die Proposer werden für ihre Teilnahme extra belohnt.&#x20;

Während sich dieser Prozess wiederholt, werden der Kette neue Blöcke hinzugefügt. Jeder Validator enthält eine Kopie aller Transaktionen, die im Kujira-Netzwerk getätigt werden, die vor der Abstimmung mit den vorgeschlagenen Transaktionsblöcken verglichen wird. Mehrere unabhängige Validatoren nehmen an der Konsensabstimmung teil, was es unmöglich macht, falsche Blöcke zu akzeptieren. Auf diese Weise können die Validatoren die Kujira-Blockchain sichern und die Gültigkeit jeder Transaktion gewährleisten.

Eine ausführlichere Erklärung zum Tendermint-Konsensus finden Sie [hier] (https://docs.tendermint.com/).
