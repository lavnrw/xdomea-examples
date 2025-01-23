# Beispieldateien zu xdomea v4.0.0

Die Dateien dienen der Aussonderung eines Vorgangs. Zentral ist die
xdomea-Nachricht Aussonderung.Aussonderung.0503
([`518c7c31-d1a0-11ef-accb-005056871b7c_0503_1.xml`](518c7c31-d1a0-11ef-accb-005056871b7c_0503_1.xml))
mit den Metadaten des Vorgangs und der darin enthaltenen Dokumente. In diesem
Beispiel enthält der Vorgang nur ein Dokument
([`8bcadd35-d27b-11ef-ab6c-005056871b7c.txt`](8bcadd35-d27b-11ef-ab6c-005056871b7c.txt)).
Die xdomea-Nachricht und die zum Vorgang gehörenden Dokumente, also die o.g.
Dateien, müssen zum Versand gemeinsam in eine ZIP-Datei gepackt werden; das
Vorgehen wird in der [`Makefile`](Makefile) beschrieben.

Relevante Dokumentation:

- [XRepository-Eintrag](https://www.xrepository.de/details/urn:xoev-de:xdomea:kosit:standard:xdomea_4.0.0)
- [Spezifikation (PDF)](https://www.xrepository.de/api/xrepository/urn:xoev-de:xdomea:kosit:standard:xdomea_4.0.0:dokument:Spezifikation_zu_xdomea_4.0.0)
- [Schemadateien (XSD in ZIP)](https://www.xrepository.de/api/xrepository/urn:xoev-de:xdomea:kosit:standard:xdomea_4.0.0:xmlschema)

## Vorgaben zu UUIDs

- Jede Nachricht bekommt eine eigene UUID (das ist neu in xdomea 4). Siehe
  Spezifikation Abschnitt 5.1.30, Seite 114. Element in xdomea:
  `nachrichtenkopf/identifikation.nachricht/nachrichtenUUID`
- Jeder zusammenhängende Prozess, z.B. eine Aussonderung, bekommt eine
  übergreifende UUID. Zu einem Prozess können mehrere Nachrichten gehören, z.B.
  eine Nachricht Aussonderung.Aussonderung.0503 und eine korrespondierende
  Nachricht Aussonderung.AussonderungImportBestaetigen.0506, aber auch mehrere
  Nachrichten desselben Typs (letzteres ist neu in xdomea 4). Diese Nachrichten
  bekommen alle dieselbe ProzessID, aber jeweils eine eigene nachrichtenUUID.
  Siehe Spezifikation Abschnitt 2.4.5, Seite 18 und Abschnitt 3.3.13, Seite
  70--71. Element in xdomea: `nachrichtenkopf/ProzessID`
- Jedes fachliche Objekt (z.B. Vorgang, Dokument, ...) in einer xdomea-Nachricht
  bekommt eine eigene UUID. Kommt das Objekt in mehreren Nachrichten vor, bleibt
  die UUID des Objekts gleich. Siehe Spezifikation Abschnitt 2.4.3, Seite 18.
  Das betrifft insbesondere folgende Elemente in einer xdomea-Nachricht:
  - Vorgang: `Schriftgutobjekt/Vorgang/Identifikation/xdomeaUUID`
  - Dokument: `Schriftgutobjekt/Vorgang/DokumentOderDokumentMitSchriftstueck/Dokument/Identifikation/xdomeaUUID`
- Jede Datei, die ein Dokument repräsentiert (Version bzw. Primärdokument),
  bekommt einen Dateinamen auf Basis einer eigenen UUID. Es darf nicht die UUID
  des übergeordneten logischen Dokuments wiederverwendet werden, weil zu einem
  Dokument mehrere Versionen vorliegen können, die in unterschiedlichen Dateien
  gespeichert werden! Siehe Spezifikation Abschnitt 5.1.44, Seite 128. Element
  in xdomea:
  `Schriftgutobjekt/Vorgang/DokumentOderDokumentMitSchriftstueck/Dokument/Version/Format/Primaerdokument/Dateiname`

## Vorgaben zu Dateinamen

Ein xdomea-Container (ZIP) enthält die Metadaten (xdomea-Nachricht, XML) sowie
die Primärdokumente (z.B. PDF, Word, ...) zu einem oder mehreren
Schriftgutobjekten, z.B. Vorgängen oder Akten. Vorgaben zu den Dateinamen finden
sich in der Spezifikation auf Seite 10.

- xdomea-Container (ZIP)
  - Muster: `<ProzessID>_<nachrichtentyp>_<LfdNrNachrichtProTyp>.zip`
  - Beispiel: `518c7c31-d1a0-11ef-accb-005056871b7c_0503_1.zip`
- xdomea-Nachricht (XML)
  - Muster: `<ProzessID>_<nachrichtentyp>_<LfdNrNachrichtProTyp>.xml`
  - Beispiel: `518c7c31-d1a0-11ef-accb-005056871b7c_0503_1.xml`
- Primärdokumente (Inhaltsdateien)
  - Muster: `<UUID>.<ext>` oder `<UUID>_<Betreff>.<ext>`
  - Beispiel: `8bcadd35-d27b-11ef-ab6c-005056871b7c.txt`

Beispiel für die ZIP-Struktur:

~~~
518c7c31-d1a0-11ef-accb-005056871b7c_0503_1.zip
├── 518c7c31-d1a0-11ef-accb-005056871b7c_0503_1.xml
└── 8bcadd35-d27b-11ef-ab6c-005056871b7c.txt
~~~

Sollen mehrere xdomea-Container übergeben werden, z.B. mehrere Vorgänge in je
einem eigenen Container, kann eine zusätzliche Transportdatei verwendet werden,
die beispielsweise eine Liste aller Container samt ihrer Hashwerte enthält
(siehe Spezifikation Seite 11). Die xdomea-Spezifikation macht keine Vorgaben
zum Format oder Namen der Transportdatei.

Sollen mehrere xdomea-Container übergeben werden, z.B. mehrere Vorgänge in je
einem eigenen Container, kann eine zusätzliche Transportdatei verwendet werden,
die beispielsweise eine Liste aller Container samt ihrer Hashwerte enthält
(siehe Spezifikation Seite 11). Die xdomea-Spezifikation macht keine Vorgaben
zum Format oder Namen der Transportdatei. Außerdem sollten in diesem Fall die
übergreifende ProzessID in allen zusammengehörigen Nachrichten identisch sein
sowie die Elemente LfdNrNachrichtProTyp und GesamtanzahlNachrichtenProTyp
korrekt befüllt werden.

## Sonstiges

- In xdomea-Nachrichten muss als Namespace-Präfix der String `xdomea` verwendet
  werden. Siehe Spezifikation Abschnitt 4.2, Seite 79.
