# 2.4 Das Local-First-Paradigma

Local First bezeichnet keine einzelne Technologie und keine bestimmte Desktop-Runtime, sondern ein Architekturleitbild: Daten und zentrale Anwendungsoperationen sollen zunächst auf dem Gerät der nutzenden Person
stattfinden, während ein Netzwerkdienst lediglich ergänzend für Synchronisation, Sicherung oder gemeinsame Nutzung eingesetzt wird. Diese Arbeitsdefinition der vorliegenden Arbeit orientiert sich an Kleppmann et al.
[BELEG ERFORDERLICH; theoretische Definition, Schlüssel `kleppmann2019localfirst` noch nicht in `refs.bib`] und ist ausdrücklich als Arbeitsdefinition zu verstehen [BEGRIFF DEFINIEREN]. Damit unterscheidet sich Local
First von einer cloudzentrierten Anwendung, in der die entfernte Datenbank die autoritative Datenquelle darstellt und der Client überwiegend als Repräsentation oder Cache dieser Daten dient [QUELLENREICHWEITE PRÜFEN; die
Aussage beschreibt die hier betrachtete serverzentrierte Referenzarchitektur, nicht jede Webarchitektur].

Die lokale Ausführung ist eine mögliche, aber keine hinreichende Voraussetzung für Local First. Eine Electron-Anwendung kann weiterhin für jede fachliche Operation ein entferntes Backend benötigen; umgekehrt kann eine
Webanwendung local-first-Eigenschaften besitzen, ohne als Desktop-Anwendung installiert zu sein. Für die vorliegende Arbeit sind daher die Desktop-Runtime, die lokale Persistenz und das Local-First-Paradigma begrifflich
zu trennen.

## 2.4.1 Kernprinzipien nach Kleppmann et al.

Auf der Literaturebene entwerfen Kleppmann et al. Local First als Gegenentwurf zu einem ausschließlich serverzentrierten Datenmodell: Die lokale Kopie der Daten gilt als primäre Kopie, ein Server kann sekundäre Kopien
für Synchronisation, Datensicherung oder den Zugriff weiterer Geräte bereitstellen [BELEG ERFORDERLICH; theoretische Definition, Kleppmann et al.]. Die Existenz eines Servers widerspricht dem Paradigma daher nicht,
solange die lokale Anwendung nicht von dessen ständiger Verfügbarkeit abhängt und die lokale Kopie nicht als entbehrlicher Cache behandelt wird.

Die sieben Ideale von Kleppmann et al. dienen hier als Orientierung, nicht als technische Checkliste, die durch den Einsatz einer bestimmten Datenbank automatisch erfüllt würde [BELEG ERFORDERLICH; theoretische
Definition, Kleppmann et al.]:

1. **„No spinners“ (unmittelbare Verfügbarkeit):** Lese- und Schreiboperationen hängen nicht von der
   Antwortzeit eines entfernten Servers ab; Änderungen werden zunächst lokal verarbeitet und anschließend im
   Hintergrund synchronisiert. Lokale Datenhaltung garantiert keine geringe Latenz, schafft aber die
   architektonische Voraussetzung dafür.
2. **Verfügbarkeit auf mehreren Geräten:** Die lokale Primärkopie soll auf weiteren Geräten verfügbar sein,
   ohne die lokale Autonomie aufzugeben; dazu müssen Änderungen übertragen und gegebenenfalls zusammengeführt
   werden. Synchronisation ist daher kein bloßer Kopiervorgang, sondern muss parallele Änderungen mehrerer
   Geräte berücksichtigen.
3. **Netzwerk als optionale Infrastruktur:** Die nutzende Person kann Daten auch ohne Verbindung lesen und
   verändern; ausstehende Änderungen werden nach Wiederherstellung der Verbindung synchronisiert. Dabei ist
   zwischen einer kurzen Unterbrechung und einem vollständig offlinefähigen System zu unterscheiden:
   Offlinefähigkeit setzt ein lokales Datenmodell, lokale Geschäftsoperationen und eine Strategie für
   ausstehende Änderungen voraus.
4. **Nahtlose Zusammenarbeit:** Parallele Änderungen werden erkannt und möglichst automatisch zusammengeführt.
   Bei fachlich strukturierten, semantisch abhängigen Daten ist eine rein textbasierte Zusammenführung oft
   unzureichend [BELEG ERFORDERLICH], weil die fachliche Bedeutung nicht aus der Zeichenfolge hervorgeht.
   Kollaboration ist eine zusätzliche Komplexitätsstufe und keine zwingende Voraussetzung jeder
   local-first-Anwendung.
5. **„The Long Now“ (langfristige Erhaltung):** Daten bleiben auch bei Wegfall eines Anbieters zugänglich;
   dafür müssen sie in einem nachvollziehbaren Format gespeichert, exportiert und gegebenenfalls in zukünftige
   Schema-Versionen überführt werden. Die Forderung nach Schema-Migration ist dabei eine technische Konsequenz
   der vorliegenden Arbeit, nicht unmittelbar eine Aussage der genannten Quelle [QUELLENREICHWEITE PRÜFEN].
6. **Sicherheit und Datenschutz als Standard:** Eine zentrale Datenbank mit den Daten sämtlicher Nutzender ist
   nicht nötig; optionale Synchronisation kann durch Ende-zu-Ende-Verschlüsselung die Einsichtsmöglichkeiten
   eines Servers begrenzen. Local First ist jedoch nicht automatisch sicherer [BELEG ERFORDERLICH;
   Sicherheitsimplikation der Arbeit]: Es reduziert die zentrale Datensammlung, ersetzt aber nicht den Schutz
   des Endgeräts und die Sicherheit der konkreten Anwendung. Die Daten müssen auf den Endgeräten gegen
   unberechtigten Zugriff, Schadsoftware, Verlust und unsichere Backups geschützt werden [AUSSAGE ZU ABSOLUT].
7. **Kontrolle und Besitz der Daten:** Gemeint ist nicht juristisches Eigentum, sondern die Möglichkeit, Daten
   unabhängig von den Vorgaben eines Dienstanbieters zu lesen, zu kopieren, zu exportieren, zu sichern und
   weiterzuverarbeiten; ein eingeschränktes Exportformat oder eine nur über proprietäre APIs erreichbare
   Datenbank begrenzt diese Kontrolle trotz lokaler Zwischenspeicherung.

Die Ideale gehen über die Forderung „offline verfügbar“ hinaus: Offlinefähigkeit, geringe Latenz und lokale Persistenz bilden die technische Grundlage, während Datensouveränität, langfristige Erhaltung, Datenschutz und
gegebenenfalls Kollaboration die Nutzung und den Lebenszyklus der Daten betreffen. Nicht jede Anwendung muss alle Ideale erfüllen; ein lokales Einzelplatzszenario kann von lokaler Verfügbarkeit, geringer Latenz,
Datenkontrolle und langfristig lesbaren Daten profitieren, ohne Echtzeitkollaboration anzubieten.

Für die Zusammenführung paralleler Änderungen sind auf der Literaturebene insbesondere Operational Transformation und Conflict-free Replicated Data Types (CRDTs) relevant; CRDTs verwenden Datenstrukturen, deren
Zusammenführung unter bestimmten Voraussetzungen reihenfolgeunabhängig zu einem konvergenten Zustand führt [BELEG ERFORDERLICH; theoretische Definition, Kleppmann et al.]. CRDTs beseitigen jedoch weder jede fachliche
Mehrdeutigkeit noch die Notwendigkeit eines geeigneten Datenmodells: Bei einem konkurrierend veränderten fachlichen Wert muss die Anwendung priorisieren, zusammenführen oder der nutzenden Person zur Entscheidung
vorlegen. Für eine Anwendung ohne Synchronisation sind weder CRDTs noch Operational Transformation erforderlich.

## 2.4.2 Lokale Datenhaltung und Offline-First

Auf der technischen Grundlagenebene ist die zentrale Konsequenz des Paradigmas die lokale Primärpersistenz: Die Anwendung muss die für ihre Kernfunktionen erforderlichen Daten dauerhaft auf dem Endgerät speichern und
lokal lesen und schreiben können. Ein flüchtiger UI-Zustand, ein Browser-Cache oder die optimistische Darstellung einer unbestätigten Serveränderung genügen dafür nicht.

Offline-First und Local First sind eng verwandte, aber nicht deckungsgleiche Begriffe; für die vorliegende Arbeit gilt die folgende Arbeitsdefinition [BEGRIFF DEFINIEREN; eigene Arbeitsdefinition, keine etablierte
Quelle]: Offline-First bezeichnet die Anforderung, dass die Anwendung auch ohne Verbindung funktionsfähig bleibt und Netzwerkzugriffe nicht den primären Interaktionspfad bilden; Local First umfasst zusätzlich, wem die
Daten unterstehen, wie sie langfristig erhalten werden und wie mehrere lokale Kopien zusammenarbeiten können. Eine Anwendung kann daher offlinefähig sein, ohne ihren Datenbestand in kontrollierbarer Form bereitzustellen;
umgekehrt kann eine lokale Anwendung die Datenhoheit stärken, ohne zu synchronisieren.

Für die lokale relationale Datenhaltung eignet sich SQLite als eingebettete Datenbank [PRODUKTDETAIL PRÜFEN; Belegtyp: Produktdokumentation, Schlüssel `sqlite2025about` noch nicht in `refs.bib`]: SQLite arbeitet als
serverlose, konfigurationsarme Bibliothek innerhalb des Anwendungsprozesses, speichert eine relationale Datenbank in einer Datei und unterstützt ACID-Transaktionen, sodass zusammengehörige Änderungen atomar verarbeitet
und bei einem Fehler zurückgerollt werden können [PRODUKTDETAIL PRÜFEN]. Diese Eigenschaften betreffen die Engine und sind nicht gleichzusetzen mit einer vollständigen Backup- oder Wiederherstellungsgarantie
[PRODUKTDETAIL PRÜFEN].

Im Zielartefakt ist die schreibbare SQLite-Datei mit den Benutzerdaten vom unveränderlichen Installationsverzeichnis zu unterscheiden [PRODUKTDETAIL PRÜFEN; eigene Zielanforderung]: Die Datenbankdatei liegt in einem
Benutzerdatenpfad und wird bei Updates nicht überschrieben, das Installationsverzeichnis enthält unveränderliche Programmressourcen. Die Speicherung in einer Datei vereinfacht Kopieren, Export und Backup, schützt aber
nicht automatisch vor Dateibeschädigung, versehentlichem Löschen, Ransomware oder dem Verlust des Endgeräts; Backups müssen konsistent erstellt, gegen unberechtigten Zugriff geschützt und wiederherstellbar sein [BELEG
ERFORDERLICH]. Ebenso ist zu entscheiden, ob sensible Inhalte unverschlüsselt abgelegt werden oder Verschlüsselungs- und Schlüsselverwaltungsmechanismen erforderlich sind.

Eine relationale lokale Datenbank benötigt ein explizit verwaltetes Schema; bei Weiterentwicklung müssen bestehende Datenbestände überführt werden, sodass ein leerer Erstinstallationsstand nicht der einzige relevante
Fall ist. Schema-Migrationen beschreiben diese Entwicklung als versionierte, reproduzierbare Änderungen; Reihenfolge, Abhängigkeiten, Default-Werte, Datenübernahmen und Wiederholbarkeit der Schritte sind zu
berücksichtigen. Migrationen sollten in transaktionalen Grenzen ausgeführt werden, damit ein Fehler nicht zu einem teilweise aktualisierten Schema führt; ob der eingesetzte Migration Runner dies für jeden Schritt
leistet, ist produktabhängig und wird daher als Zielanforderung formuliert [PRODUKTDETAIL PRÜFEN; normative Zielanforderung]. Ein typgesichertes ORM wie Drizzle kann die Umsetzung unterstützen, ersetzt aber nicht die
fachliche Prüfung der erzeugten SQL-Änderungen [PRODUKTDETAIL PRÜFEN; Quelle zur Drizzle-Architektur nachzutragen]. Für das Zielartefakt wird gefordert, dass Migrationen mit dem Anwendungspaket ausgeliefert, beim Start
kontrolliert angewendet und für alle unterstützten Ausgangsversionen getestet werden; die Anwendung muss erkennen, welche Migrationen bereits ausgeführt wurden, und einen bestehenden Datenbestand nicht grundlos durch
eine leere Datenbank ersetzen.

Für die Fallstudie ist dies relevant, weil die Ablösung der cloudbasierten Persistenz mehr als einen Datenbanktreiber-Wechsel bedeutet: Sie erfordert ein lokales relationales Modell, die Übernahme des bestehenden
Datenbestands und einen kontrollierten Lebenszyklus für spätere Schemaänderungen [eigene Fallstudienbeobachtung, in der Analyse zu belegen].

Eine local-first-Anwendung kann unterschiedliche Synchronisationsstrategien verwenden [eigene Ableitung]. Im Einzelplatzszenario ist keine Synchronisation erforderlich; die lokale Datenbank bleibt die autoritative
Persistenz und wird durch Backups ergänzt. Bei mehreren Geräten kann ein Dienst als sekundäre Ablage oder eine Synchronisation über lokales Netzwerk beziehungsweise Peer-to-Peer eingesetzt werden; die Wahl hängt von
Geräteanzahl, Kollaboration, Datenschutz, Verfügbarkeit und Wiederherstellbarkeit ab. Synchronisation ist nicht mit einer bloßen Kopie der Datenbank gleichzusetzen: Änderungen müssen erkannt, übertragen und
zusammengeführt werden; ein „Last Write Wins“-Verfahren reduziert technische Konflikte, kann fachlich relevante Änderungen aber unbemerkt überschreiben [BELEG ERFORDERLICH], weshalb bei relationalen Daten die
Synchronisationsebene (Datensatz, Attribut, fachliche Operation) festzulegen ist. Ob eine Mehrgeräte- oder Mehrpersonensynchronisation zum Zielartefakt gehört, wird in der Fallstudie begründet; das Paradigma schreibt sie
nicht zwingend vor.

## 2.4.3 Abgrenzung zu cloudzentrierten und traditionellen Desktop-Anwendungen

Die Abgrenzung bezieht sich im Folgenden auf cloudzentrierte beziehungsweise serverabhängige Webanwendungen; der weitergehende Begriff „Cloud-Native“ bezeichnet einen eigenständigen Architektur- und Entwicklungsansatz
und wird hier nicht als Bezeichnung der Ausgangsarchitektur verwendet [BEGRIFF DEFINIEREN].

Auf der Artefaktebene liegt in der betrachteten cloudzentrierten Web-Referenzarchitektur die dauerhaft maßgebliche Datenbasis im Backend; der Client verwaltet UI-Zustand und gegebenenfalls einen Cache, fachliche
Änderungen werden an den Server übermittelt und dort persistiert [eigene Fallstudienbeobachtung; aus dem Ausgangsartefakt zu belegen]. Vorteile sind zentrale Verwaltung, einfache Bereitstellung neuer Versionen,
gemeinsame Datenbasis und geräteübergreifende Zusammenarbeit; dem stehen Netzwerk- und Anbieterabhängigkeit, zentrale Angriffsziele und eingeschränkte Kontrolle über die gespeicherten Daten gegenüber [BELEG
ERFORDERLICH].

Eine traditionelle Desktop-Anwendung führt Programm und Daten überwiegend lokal aus, funktioniert ohne Netzwerk und greift unmittelbar auf lokale Dateien oder eine eingebettete Datenbank zu; sie ist jedoch nicht
automatisch local-first, wenn Synchronisation, Zusammenarbeit oder ein transparenter Export fehlen [AUSSAGE ZU ABSOLUT].

Local First lässt sich heuristisch als Verknüpfung beider Modelle beschreiben: Die lokale Anwendung verbindet die unmittelbare Interaktion und Datenkontrolle einer Desktop-Anwendung mit ausgewählten Netzwerkdiensten wie
Synchronisation, Backup und Kollaboration. Sie ist dafür nicht auf einen dauerhaft gekoppelten Cloud-Dienst angewiesen und bleibt auch als reine Einzelplatzanwendung funktionsfähig; der Begriff ist hier als heuristische
Einordnung und nicht als etablierte Definition zu lesen [BEGRIFF DEFINIEREN]. Der entscheidende Unterschied zur Cloud-Architektur liegt in Reihenfolge und Autorität der Operationen: Die lokale Kopie ist primär, der
Server optional.

Für das Transformationsszenario bedeutet dies, dass die Wahl von Electron allein noch keine Local-First-Architektur erzeugt. Electron stellt die Ausführungsumgebung und technische Mechanismen für eine kontrollierte
Vermittlung zwischen Web-UI und lokalen Ressourcen bereit; die wirksame Kontrolle entsteht erst durch Preload-/IPC-Design, Validierung und begrenzte Berechtigungen [PRODUKTDETAIL PRÜFEN]. Erst die Kombination aus lokaler
Datenbank, lokalem Datenzugriff, offlinefähiger Geschäftslogik und einem bewusst optionalen Netzwerkpfad verwirklicht die relevanten Local-First-Eigenschaften [normative Zielanforderung].

## 2.4.4 Relevanz für das Transformationsszenario

Für die Fallstudie werden aus diesen Grundlagen folgende Arbeitsanforderungen abgeleitet; sie sind bewusst als Anforderungen der Arbeit formuliert und nicht als universelle Eigenschaften jeder Local-First-Anwendung. Die
Überführung des KI-generierten Web-Prototyps verändert nicht nur den Speicherort der Daten, sondern die Autorität der Datenquelle, die Ausführungsreihenfolge der Geschäftsoperationen und die Verantwortung für
Datenintegrität, Migration, Backup und Wiederherstellung: Aus einer Anfrage an einen entfernten Dienst wird eine lokale Transaktion, die innerhalb der Anwendung ausgeführt und dauerhaft in SQLite gespeichert wird. Der
Netzwerkzugriff wird dadurch zur optionalen Erweiterung, sofern keine Funktion ausdrücklich auf einen externen Dienst angewiesen ist.

Für die Transformation ergeben sich mehrere architektonische Verschiebungen [normative Zielanforderung]: Erstens ersetzt eine lokale Primärpersistenz die entfernte autoritative Persistenz. Zweitens bildet eine
Datenzugriffsschicht fachliche Operationen auf das lokale relationale Modell ab, statt die Benutzeroberfläche direkt mit HTTP- oder BaaS-Aufrufen zu koppeln. Drittens werden Transaktionen und Integritätsbedingungen lokal
nachvollziehbar umgesetzt. Viertens ist zu klären, ob Identitäts-, Berechtigungs- und Mandantenkonzepte aus dem Cloud-System für das lokale Nutzungsszenario weiterhin erforderlich sind. Fünftens sind Migrationen und
Backups als Bestandteil des Anwendungslifecycles zu behandeln.

Auch die Benutzeroberfläche muss an die neue Zustandssemantik angepasst werden: Ein Ladezustand, der auf die Antwort eines Servers wartet, ist bei einer lokalen Transaktion nicht mehr der primäre Interaktionsablauf;
stattdessen sind lokale Erfolge, Validierungsfehler und gegebenenfalls ausstehende Synchronisationsvorgänge zu unterscheiden. Optimistische UI-Aktualisierungen lassen sich bei einer lokalen Datenbanktransaktion teilweise
durch unmittelbare lokale Persistierung ersetzen, bleiben jedoch bei einem späteren Abgleich mit weiteren Geräten oder einem Dienst relevant.

In der sicherheitsgehärteten Zielarchitektur wird die lokale Persistenz nicht direkt aus dem Renderer-Prozess angesprochen [AUSSAGE ZU ABSOLUT; bewusste Designentscheidung der Zielarchitektur]. Der Renderer verarbeitet
weiterhin Web-Code und erhält keinen allgemeinen Dateisystem- oder Datenbankzugriff; fachlich begrenzte Operationen werden über die in Abschnitt 2.2.3 beschriebene Preload- und IPC-Schnittstelle an den privilegierten
Anwendungsteil vermittelt, wo Eingaben validiert und auf erlaubte lokale Operationen begrenzt werden. Damit verbindet das Local-First-Zielbild die Datenarchitektur mit der Sicherheitsarchitektur der Electron-Anwendung.

Die lokale Ausführung erweitert zugleich die Verantwortung für Sicherheit und Betrieb [normative Zielanforderung]: Die Anwendung muss Datenbanken sicher anlegen und aktualisieren, Fehler bei Schreibvorgängen kontrolliert
behandeln, Backups ermöglichen und mit beschädigten oder nicht unterstützten Datenbankständen umgehen; „sicher“ und „kontrolliert“ sind dabei als noch zu operationalisierende Anforderungen zu verstehen. Bei einer
späteren Synchronisation kämen Konfliktregeln, Authentifizierung, Verschlüsselung und Wiederholbarkeit von Änderungen hinzu. Die Transformation reduziert die unmittelbare Abhängigkeit vom Cloud-Anbieter, beseitigt aber
nicht die Komplexität der Datenhaltung; sie verlagert diese Komplexität in die lokale Anwendung, wo sie explizit modelliert, getestet und abgesichert werden muss.

Daraus lassen sich für die spätere Analyse und Evaluation überprüfbare Kriterien ableiten [normative Zielanforderung]: die zentralen fachlichen Funktionen müssen ohne Netzwerk dauerhaft verfügbar sein, lokale
Schreibvorgänge müssen dauerhaft persistiert werden, vorhandene Datenbestände müssen nach der Migration wiederverwendbar bleiben, nicht unterstützte Datenbankstände müssen kontrolliert behandelt werden und Backup- und
Wiederherstellungsprozesse müssen nachvollziehbar sein.

Das Local-First-Paradigma liefert damit ein architektonisches Zielbild und zugleich Bewertungskriterien für die Transformation. Eine erfolgreiche Überführung besteht nicht darin, das Web-Frontend als installierbare Datei
zu verpacken; erforderlich ist eine lokale Daten- und Ausführungsarchitektur, in der die zentralen fachlichen Funktionen ohne Netzwerkverbindung verfügbar sind, Daten kontrollierbar gespeichert werden und spätere
Erweiterungen wie Synchronisation nicht durch unklare Datenhaltung verhindert werden. Die daraus entstehenden Sicherheitsanforderungen an lokale Daten, IPC, Dateisystemzugriffe und Abhängigkeiten werden in Abschnitt 2.3
aus Sicht des Bedrohungsmodells weiter untersucht.

## Quellenhinweise für die spätere Überführung

Die folgenden Quellen werden benötigt; die Schlüssel `kleppmann2019localfirst` und `sqlite2025about` sind nicht in `thesis/refs.bib` vorhanden und müssen vor der Überführung in die LaTeX-Fassung als tatsächlich geprüfte
Bibliographieeinträge angelegt werden; bis dahin bleiben im Fließtext die Marker-Tags stehen.

- **Kleppmann, M., Wiggins, A., van Hardenberg, P. und McGranaghan, M. (2019):** „Local-first software: You own
  your data, in spite of the cloud“, Proceedings of the 2019 ACM SIGPLAN International Symposium on New Ideas,
  New Paradigms, and Reflections on Programming and Software, S. 154--178, DOI: 10.1145/3359591.3359737 —
  vorläufiger Schlüssel `kleppmann2019localfirst`; Belegtyp: theoretische Definition (sieben Ideale, lokale
  Primärkopie, CRDTs).
- **SQLite-Dokumentation (offiziell):** vorläufiger Schlüssel `sqlite2025about`; Belegtyp:
  Produktdokumentation (serverlose Bibliothek, Dateispeicherung, ACID).
- **Drizzle-Dokumentation beziehungsweise dokumentierte Untersuchung des Migration Runners:** Belegtyp:
  Produktdokumentation [PRODUKTDETAIL PRÜFEN].
- **Allgemeine Sicherheits- und Betriebsaussagen** (Backups gegen Verlust, Ransomware, unberechtigten Zugriff;
  Vor-/Nachteile cloudzentrierter Systeme): Belegtyp: theoretische Definition [BELEG ERFORDERLICH].
- **Begriffsdefinitionen** (Offline-First als eigene Arbeitsdefinition, Abgrenzung cloudzentriert/„Cloud-Native“):
  Belegtyp: eigene Arbeitsdefinition, wo nötig mit Literaturnachweis [BEGRIFF DEFINIEREN].

Belegtypen im Text: **theoretische Definition** (aus Literatur nachzutragen), **Produktdokumentation** (offizielle Projektquellen), **eigene Fallstudienbeobachtung** (aus dem Ausgangsartefakt beziehungsweise
DSR-Artefakten zu belegen), **normative Zielanforderung** (Anforderung der Arbeit, in Entwurf und Evaluation zu verknüpfen).
