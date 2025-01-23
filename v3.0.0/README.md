# Beispieldateien zu xdomea v3.0.0

Die Dateien dienen der Aussonderung eines Vorgangs. Zentral ist die
xdomea-Nachricht Aussonderung.Aussonderung.0503
([`9fde4e7a-b41b-11ec-9225-005056871b7c_Aussonderung.Aussonderung.0503.xml`](9fde4e7a-b41b-11ec-9225-005056871b7c_Aussonderung.Aussonderung.0503.xml))
mit den Metadaten des Vorgangs und der darin enthaltenen Dokumente. In diesem
Beispiel enthält der Vorgang nur ein Dokument
([`f4bcaaf7-d7cb-11ef-a278-005056871b7c.txt`](f4bcaaf7-d7cb-11ef-a278-005056871b7c.txt)).
Die xdomea-Nachricht und die zum Vorgang gehörenden Dokumente, also die o.g.
Dateien, müssen zum Versand gemeinsam in eine ZIP-Datei gepackt werden; das
Vorgehen wird in der [`Makefile`](Makefile) beschrieben.

Relevante Dokumentation:

- [XRepository-Eintrag](https://www.xrepository.de/details/urn:xoev-de:xdomea:kosit:standard:xdomea_3.0.0)
- [Spezifikation (PDF)](https://www.xrepository.de/api/xrepository/urn:xoev-de:xdomea:kosit:standard:xdomea_3.0.0:dokument:Spezifikation_xdomea_3.0.0)
- [Schemadateien (XSD in ZIP)](https://www.xrepository.de/api/xrepository/urn:xoev-de:xdomea:kosit:standard:xdomea_3.0.0:xmlschema)

## Vorgaben zu UUIDs

- Jeder zusammenhängende Prozess, z.B. eine Aussonderung, bekommt eine
  übergreifende UUID. Zu einem Prozess können mehrere verschiedene Nachrichten
  gehören, z.B. eine Nachricht Aussonderung.Aussonderung.0503 und eine
  korrespondierende Nachricht Aussonderung.AussonderungImportBestaetigen.0506.
  Diese Nachrichten bekommen alle dieselbe ProzessID. Siehe Spezifikation
  Abschnitt 2.4.4, Seite 16. Element in xdomea: `Kopf/ProzessID`
  - Anders als in xdomea 4 hat eine Nachricht in xdomea 3 keine eigene UUID; sie
    wird durch die UUID des Prozesses und ggf. ihren Nachrichtentyp
    identifiziert. Das heißt insbesondere, dass es in xdomea 3 innerhalb eines
    Prozesses jede Nachricht eines Typs nur einmal geben kann. Sollen also
    beispielsweise mehrere Vorgänge mit jeweils einer eigenen Nachricht
    ausgesondert werden, ist die Aussonderung jedes Vorgangs aus xdomea-Sicht
    ein eigener Prozess.
- Jedes fachliche Objekt (z.B. Vorgang, Dokument, ...) in einer xdomea-Nachricht
  bekommt eine eigene UUID. Kommt das Objekt in mehreren Nachrichten vor, bleibt
  die UUID des Objekts gleich. Siehe Spezifikation Abschnitt 2.4.2, Seite 16.
  Das betrifft insbesondere folgende Elemente in einer xdomea-Nachricht:
  - Vorgang: `Schriftgutobjekt/Vorgang/Identifikation/ID`
  - Dokument: `Schriftgutobjekt/Vorgang/Dokument/Identifikation/ID`
- Jede Datei, die ein Dokument repräsentiert (Version bzw. Primärdokument),
  bekommt einen Dateinamen auf Basis einer UUID. Siehe Spezifikation Abschnitt
  5.1.35, Seite 116. Element in xdomea:
  `Schriftgutobjekt/Vorgang/Dokument/Version/Format/Primaerdokument/Dateiname`

## Vorgaben zu Dateinamen

Ein xdomea-Container (ZIP) enthält die Metadaten (xdomea-Nachricht, XML) sowie
die Primärdokumente (z.B. PDF, Word, ...) zu einem oder mehreren
Schriftgutobjekten, z.B. Vorgängen oder Akten. Vorgaben zu den Dateinamen finden
sich in der Spezifikation auf Seite 11.

- xdomea-Container (ZIP)
  - Muster: `<ProzessID>_<Nachrichtentyp>.zip`
  - Beispiel: `9fde4e7a-b41b-11ec-9225-005056871b7c_Aussonderung.Aussonderung.0503.zip`
- xdomea-Nachricht (XML)
  - Muster: `<ProzessID>_<Nachrichtentyp>.xml`
  - Beispiel: `9fde4e7a-b41b-11ec-9225-005056871b7c_Aussonderung.Aussonderung.0503.xml`
- Primärdokumente (Inhaltsdateien)
  - Muster: `<UUID>.<ext>` oder `<UUID>_<Betreff>.<ext>`
  - Beispiel: `f4bcaaf7-d7cb-11ef-a278-005056871b7c.txt`

Beispiel für die ZIP-Struktur:

~~~
9fde4e7a-b41b-11ec-9225-005056871b7c_Aussonderung.Aussonderung.0503.zip
├── 9fde4e7a-b41b-11ec-9225-005056871b7c_Aussonderung.Aussonderung.0503.xml
└── f4bcaaf7-d7cb-11ef-a278-005056871b7c.txt
~~~

Sollen mehrere xdomea-Container übergeben werden, z.B. mehrere Vorgänge in je
einem eigenen Container, kann eine zusätzliche Transportdatei verwendet werden,
die beispielsweise eine Liste aller Container samt ihrer Hashwerte enthält
(siehe Spezifikation Seite 11). Die xdomea-Spezifikation macht keine Vorgaben
zum Format oder Namen der Transportdatei.

## Sonstiges

- In xdomea-Nachrichten muss als Namespace-Präfix der String `xdomea` verwendet
  werden. Siehe Spezifikation Abschnitt 4.2, Seite 75.
