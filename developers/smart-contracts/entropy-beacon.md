# 🎲 Entropy Beacon

Wir danken [Entropic Labs](https://entropiclabs.io/beacon/), die kryptographisch verifizierbare On-Chain-Zufälligkeit für Kujira bereitstellen.

Ihr Entropy Beacon ist eine dezentrale Zufallsquelle für Blockchain-Entwickler, die eine sichere Zufallszahlengenerierung in vertrauenslosen On-Chain-Umgebungen bietet, die erste, die dies auf Cosmos-basierten Blockchains tut.&#x20;

Im Folgenden werden einige Hauptaspekte des Entropy Beacon behandelt:

### On-chain Entropie&#x20;

Es ist derzeit unmöglich, völlig unvorhersehbare Zufallszahlen auf der Kette zu erzeugen. Beacon beabsichtigt, einen sicheren, unvorhersehbaren, billigen Zufallszahlengenerator anzubieten.

### Freie Einreichungen

Wenn Smart Contracts On-Chain-Entropie benötigen, fordern sie diese beim Beacon an. Off-Chain-Arbeiter nehmen die Anfrage auf, erzeugen sie und rufen den Beacon-Vertrag an. An diesem Punkt verwendet der Entropie-Beacon Gas, um die Callback-Funktion des anfordernden Vertrags aufzurufen. Kuijra hat einen Gebührenzuschuss eingerichtet, der von einer Protokoll-Brieftasche bezahlt wird, um Anreize für Off-Chain-Arbeiter zu schaffen, die Funktion aufzurufen und die Beacon-Gasgebühren zu decken. Dadurch müssen Protokolle, die Zufälligkeiten nutzen, keine zusätzlichen Gebühren übernehmen oder an die Nutzer weitergeben.

### Vereinfachte Entwicklung

Beacon bietet die komfortabelste Entwicklererfahrung aller existierenden Zufallslösungen, ohne komplexe Tokenomics und Abonnementmodelle und mit einer ergonomischen API für die Interaktion mit dem System.

### Ermöglicht dApps der nächsten Generation

Ein On-Chain-Entropie-Beacon eröffnet Hunderte von bisher unerreichbaren Anwendungsfällen. Lotteriesysteme, fälschungssichere Werbegeschenke, komplexe DeFi-Spiele und unzählige andere erfordern die dezentrale Generierung von sicheren Zufallszahlen.

### Wie funktioniert es?

Beacon verwendet VRF-Kryptographie, um zu gewährleisten, dass die generierten Zufallszahlen nachweislich unverfälscht sind. Eine detailliertere Erklärung finden Sie[ hier](https://entropiclabs.io/beacon/docs/how-it-works/) in den Dokumentationen von Entropic Labs.

### Schnellstart

Für Entwickler, die Verträge entwickeln wollen, die den Entropy Beacon nutzen, gibt es in den Dokumenten von Entropic Labs eine ausführliche Erklärung, die alles abdeckt, was man von Anfang bis Ende wissen muss, und zwar [hier](https://entropiclabs.io/beacon/docs/quickstart/).

### Integration

Für Entwickler, die das Entropy Beacon in bestehende Smart Contracts integrieren möchten, gibt es in den Dokumenten von Entropic Labs ebenfalls eine ausführliche Erklärung zu diesem Thema, die Sie [hier](https://entropiclabs.io/beacon/docs/integration/) finden können;

### Mainnet Adresse

Die Mainnet-Adresse von Entropy Beacon lautet [kujira1x623ehq3gqx9m9t8asyd9cgehf32gy94mhsw8l99cj3l2nvda2fqrjwqy5](https://finder.kujira.app/kaiyo-1/contract/kujira1x623ehq3gqx9m9t8asyd9cgehf32gy94mhsw8l99cj3l2nvda2fqrjwqy5)

### Hilfreiche Links

Weitere Informationen finden Sie auf Entropic's [Github](https://github.com/EntropicLabs), [Twitter](https://twitter.com/Entropic\_Labs) und [Discord](https://discord.com/invite/Qp9ZcZJnKC).


