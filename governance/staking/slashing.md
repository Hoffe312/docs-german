# ❌ Slashing

Die Leitung eines Validators ist eine wichtige Aufgabe. Validatoren müssen strenge Standards erfüllen, das Netzwerk ständig überwachen und am [Konsens](./#Konsens) teilnehmen. Validatoren, die sich falsch verhalten und diese Standards nicht einhalten, werden zur Strafe geslashed.&#x20;

**Wenn ein Validator "geslashed" wird, verliert er einen kleinen Teil seiner selbst gesetzten KUJI sowie einen kleinen Teil der Einsätze seiner Delegatoren.** "Slashed"-Validatoren werden inhaftiert und von der Sicherung des Netzwerks ausgeschlossen, bis sie geeignete Maßnahmen ergreifen, um sich selbst zu entlassen. Solange die Validatoren inhaftiert sind, können sie nicht weiter geslashed werden, was ihnen Zeit gibt, sich zu sammeln und gleichzeitig den Schaden für ihre Delegierten zu minimieren.

Das Slashing erfolgt unter den folgenden Umständen:&#x20;

**Doppelunterzeichnung**: Ein Validator signiert zwei verschiedene Blöcke mit der gleichen Chain-ID auf der gleichen Höhe.

Stellen Sie sicher, dass die Validatoren, an die Sie delegieren, gegen Double Signing geschützt sind. In diesem Fall wird die schwerste Strafe von 5 % des Einsatzes des Validierers und jedes Delegierers fällig &#x20;

**Ausfallzeit**: Ein Validator kann nicht erreicht werden und ist für eine gewisse Zeit nicht ansprechbar.&#x20;

Dies kann zu einer Unterbrechung des Dienstes führen. Dies wird mit einer weniger strengen Strafe von 0,01% des Einsatzes des Validators und jedes Delegators geahndet.&#x20;

**Vermisste Stimmen**: Validatoren müssen als Teil des Kujira-Blockchain-Konsens abstimmen. &#x20;

Validatoren im Kuijra-Netzwerk können zum Beispiel Abstimmungen über Kujira-Preisorakel verpassen. Es ist wichtig, mit Validatoren zu arbeiten, die verpasste Abstimmungen minimieren. Dies wird ebenfalls mit einer weniger strengen Strafe von 0,01% der eingesetzten Positionen des Validators und jedes Delegators bestraft&#x20;



