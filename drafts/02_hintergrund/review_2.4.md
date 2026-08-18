# Kurzurteil

Der Entwurf ist konzeptionell stark und für einen Grundlagenabschnitt bereits ungewöhnlich reflektiert. Besonders gelungen sind die Trennung von Desktop-Runtime, lokaler Persistenz und Local-First-Paradigma, die differenzierte Darstellung der sieben Ideale sowie die wiederholte Einschränkung, dass Electron, SQLite oder Offlinefähigkeit allein noch keine Local-First-Architektur begründen.

Der zentrale Überarbeitungsbedarf liegt nicht in der Grundargumentation, sondern in der wissenschaftlichen Absicherung und Verdichtung. Im aktuellen `thesis/refs.bib` fehlen die im Text verwendeten Schlüssel `kleppmann2019localfirst` und `sqlite2025about`. Außerdem werden zahlreiche technische, sicherheitsbezogene und architekturvergleichende Aussagen nur teilweise oder gar nicht belegt. Die Abschnitte 2.4.2 bis 2.4.4 wiederholen mehrere Kernaussagen zu lokaler Primärpersistenz, optionaler Synchronisation und der fehlenden Gleichsetzung von Electron mit Local First.

**Gesamtbewertung:** fachlich tragfähiger Entwurf mit guter Überarbeitungsbasis; vor der Überführung in LaTeX sind Quellenapparat, Terminologie und Redundanzen systematisch zu überarbeiten. Empfehlung: **draft-quality / gezielte substanzielle Überarbeitung**, nicht vollständige Neufassung.

# Wichtigste Stärken

- Die definitorische Eingangsdifferenzierung zwischen Desktop-Runtime, lokaler Persistenz und Local-First-Paradigma verhindert einen häufigen Kategorienfehler.
- Die sieben Ideale werden nicht lediglich aufgezählt, sondern mit Konsequenzen für Datenhaltung, Offlinebetrieb, Kollaboration, Datenportabilität und Sicherheit erläutert.
- Der Entwurf vermeidet die problematische Gleichsetzung von lokaler Datenhaltung und Local First. Die Aussagen in den Abschnitten 2.4.1 und 2.4.2 sind an dieser Stelle besonders präzise.
- Die Grenzen des Paradigmas werden ausdrücklich benannt: Local First ist nicht automatisch sicherer, CRDTs lösen nicht jede fachliche Konfliktfrage, und Kollaboration ist für eine Einzelplatzanwendung nicht zwingend.
- Die Verbindung zur Fallstudie ist konkret: lokale Primärpersistenz, SQLite, Schema-Migrationen, IPC, Validierung und Backups werden als Transformationsfolgen sichtbar gemacht.
- Die Abgrenzung zwischen technischer Grundlage und Zielanforderung ist vielfach bereits angelegt, insbesondere durch Formulierungen wie „für die vorliegende Arbeit“ und „für die Fallstudie“.

# Kritische Verbesserungsbereiche

1. **Quellenapparat vervollständigen und Reichweite prüfen.** Die beiden verwendeten Zitatschlüssel sind in der aktuellen `refs.bib` nicht vorhanden. Zudem deckt die Kleppmann-Arbeit nicht automatisch alle Aussagen zu SQLite, Backups, Migrationen, Electron-IPC oder den wirtschaftlichen Vor- und Nachteilen cloudzentrierter Systeme ab. Die Belegtypen müssen getrennt werden: theoretische Definition, Produktdokumentation, eigene Fallstudienbeobachtung und normative Zielanforderung.
2. **„Cloud-Native“ terminologisch von „cloudzentriert“ abgrenzen.** Der Text behandelt überwiegend eine cloudzentrierte beziehungsweise serverabhängige Referenzarchitektur. „Cloud-native“ bezeichnet dagegen einen weitergehenden Architektur- und Entwicklungsansatz. Der Begriff sollte definiert oder im Abschnittstitel durch den tatsächlich behandelten Begriff ersetzt werden.
3. **Local First und Offline-First sauber operationalisieren.** Die Unterscheidung ist plausibel, bleibt aber ohne explizite Definition der Mindestkriterien. Es sollte festgelegt werden, welche Kernfunktionen des Studienplaners ohne Netzwerk dauerhaft verfügbar sein müssen und welche Eigenschaften lediglich wünschenswert sind.
4. **Theorie, Fallstudie und Zielarchitektur sichtbar trennen.** Ab Absatz 2.4.3 wechseln allgemeine Aussagen, Beobachtungen zur Web-Referenzarchitektur und Anforderungen an das Zielartefakt teilweise ohne Kennzeichnung. Dadurch ist nicht immer klar, ob eine Aussage aus Literatur stammt, im Ausgangscode beobachtet wurde oder als Designentscheidung vorgeschlagen wird.
5. **Redundanzen reduzieren.** Primärpersistenz statt Cache, optionale Synchronisation, die Nichtgleichsetzung von Electron und Local First sowie die Verlagerung von Verantwortung werden mehrfach neu formuliert. Eine Verdichtung würde die hohe fachliche Qualität erhalten und den Abschnitt deutlich lesbarer machen.
6. **SQLite-, Migrations- und Bereitstellungsdetails präzisieren.** Aussagen zu ACID, transaktionalen Migrationen, Drizzle und dem Lebenszyklus lokaler Datenbanken müssen auf die konkret eingesetzte SQLite-/Drizzle-Konfiguration bezogen werden. Die Datenbankdatei ist außerdem von den schreibbaren Benutzerdaten vom unveränderlichen Installationsverzeichnis zu unterscheiden.

# Detaillierte Analyse

## Fachliche Präzision und Terminologie

Die Grunddefinition ist fachlich überzeugend. Die Formulierung, dass Local First keine einzelne Technologie oder Desktop-Runtime bezeichnet, setzt den richtigen Rahmen. Auch die Aussage, dass ein Server mit dem Paradigma vereinbar bleibt, solange die lokale Kopie nicht bloß ein entbehrlicher Cache ist, trifft den Kern der im Draft genannten Leitidee.

Die Bezeichnung „cloudzentriert“ ist im Fließtext überwiegend passend. Problematisch ist vor allem der Abschnittstitel „Abgrenzung zu Cloud-Native und traditionellen Desktop-Anwendungen“. Im weiteren Text wird keine Definition von Cloud-Native geliefert; stattdessen wird eine Architektur mit entferntem Backend, serverseitiger Persistenz und Netzwerkabhängigkeit beschrieben. Für die wissenschaftliche Präzision sollte entweder ein eigener Cloud-Native-Begriff mit belastbarer Quelle eingeführt oder durchgehend von „cloudzentrierten/serverabhängigen Webanwendungen“ gesprochen werden. **[BEGRIFF DEFINIEREN]**

Die Gleichsetzung von Local First mit einem „Hybrid zwischen den beiden Modellen“ ist als anschauliche Interpretation brauchbar, aber als Begriffsbestimmung zu stark. Local First kann einen optionalen Synchronisationsdienst integrieren, muss aber nicht zwingend eine Hybridarchitektur im Sinne eines dauerhaft gekoppelten Cloud-/Desktop-Systems sein. Die vorhandene Einschränkung „kann daher als Hybrid verstanden werden“ entschärft die Aussage; sie sollte dennoch als heuristische Einordnung und nicht als etablierte Definition kenntlich sein.

Die Unterscheidung zwischen Offline-First und Local First ist sinnvoll, sollte aber mit einer klaren Begriffsquelle oder als eigene Arbeitsdefinition markiert werden. Insbesondere „Offline-First bezeichnet vor allem ...“ und „Local First umfasst zusätzlich ...“ sind theoretische Definitionsaussagen, nicht bloße Fallstudienbeobachtungen. **[BEGRIFF DEFINIEREN] [BELEG ERFORDERLICH]**

## Argumentationsstruktur

Die Makrostruktur ist schlüssig: Definition und Ideale, technische Konsequenzen, Abgrenzung und schließlich Relevanz für die Transformation. Der Abschnitt 2.4.4 bildet einen guten Übergang zu Analyse und Entwurf.

Die Argumentation könnte dennoch stärker zwischen drei Ebenen unterscheiden:

- **Literaturebene:** Local-First-Ideale, lokale Primärkopie, optionale Netzwerkabhängigkeit und CRDTs.
- **Technische Grundlagenebene:** SQLite, Persistenz, Transaktionen, Migrationen und Synchronisationsmechanismen.
- **Artefaktebene:** das angenommene Einzelplatzszenario, die Ablösung der konkreten Web-Persistenz, die IPC-Grenze und die Anforderungen des Studienplaners.

Aktuell werden diese Ebenen vor allem ab Zeile 138 und erneut ab Zeile 164 vermischt. Eine kurze explizite Überleitung wie „Für die Fallstudie werden aus diesen Grundlagen folgende Arbeitsanforderungen abgeleitet“ würde die Argumentationslogik stabilisieren. Die anschließenden Anforderungen sollten dann als Anforderungen der Arbeit und nicht als universelle Eigenschaften jeder Local-First-Anwendung gekennzeichnet werden.

## Quellen und Belegfähigkeit

Die im Text angegebene Kleppmann-Quelle ist grundsätzlich passend für die sieben Ideale, die Rolle der lokalen Primärkopie und die Einordnung von CRDTs. Sie ist eine theoretisch-konzeptionelle Quelle. Sie belegt damit nicht automatisch:

- die konkrete Architektur des Studienplaners,
- die allgemeine Sicherheitsbewertung lokaler Datenhaltung,
- die konkrete Funktionsweise der eingesetzten Drizzle-Migrationsumgebung,
- Anforderungen an Backups, Dateipfade und Wiederherstellung,
- alle Vor- und Nachteile cloudzentrierter Systeme.

Die SQLite-Quelle ist als offizielle Produktdokumentation geeignet, um Eigenschaften der SQLite-Engine zu belegen. Sie sollte jedoch nicht als alleiniger Beleg für allgemeine Backup-, Ransomware-, Schlüsselverwaltungs- oder Betriebssicherheitsaussagen verwendet werden. Für Drizzle-spezifische Aussagen ist eine verifizierte Drizzle-Dokumentation oder eine nachvollziehbare technische Untersuchung des verwendeten Migration Runners erforderlich. Es werden hier keine neuen Quellen vorgeschlagen oder bibliografischen Angaben erfunden; die vorhandenen Schlüssel müssen anhand tatsächlich geprüfter Einträge ergänzt und überprüft werden.

Besonders die folgenden Aussagegruppen benötigen eine Beleg- oder Herkunftsentscheidung:

- **Kleppmann-nahe Theorie:** sieben Ideale, lokale Primärkopie, optionaler Server, CRDTs und Kollaboration.
- **Technische Produktdetails:** SQLite als serverlose Bibliothek, Datei-/Transaktionsverhalten, Drizzle und Migration Runner. **[PRODUKTDETAIL PRÜFEN]**
- **Allgemeine Sicherheits- und Betriebsbehauptungen:** lokale Daten seien gegen Verlust, Ransomware und unberechtigten Zugriff zu schützen; Backups müssten konsistent, geschützt und wiederherstellbar sein. Diese sind plausibel, aber derzeit unbelegt. **[BELEG ERFORDERLICH]**
- **Fallstudienbeobachtungen:** Die Aussagen zur cloudzentrierten Web-Referenzarchitektur in den Zeilen 141--142 müssen aus dem Ausgangscode, vorhandenen DSR-Artefakten oder einer späteren Analyse nachvollziehbar hervorgehen. Sie sind keine allgemeine Literaturdefinition.
- **Zielanforderungen:** Aussagen wie „Der Renderer darf ... nicht“ oder „Die Anwendung muss ... Backups ermöglichen“ sind als Anforderungen der Zielarchitektur zu kennzeichnen und später mit Entwurfs- beziehungsweise Evaluationsartefakten zu verknüpfen.

## DSR-Passung und Forschungsfragen

Als theoretischer Hintergrundabschnitt muss 2.4 keinen vollständigen DSR-Zyklus dokumentieren. Der Übergang zur Transformation ist jedoch gut vorbereitet. Der Text benennt bereits ein Problem, leitet architektonische Verschiebungen ab und deutet spätere Sicherheits- und Evaluationsfragen an.

Für die Anschlusskapitel sollten die in 2.4 formulierten Kriterien operationalisiert werden. Für das Einzelplatzszenario bieten sich insbesondere überprüfbare Kriterien an: Kernfunktionen ohne Netzwerk, dauerhafte lokale Schreibvorgänge, Wiederverwendung vorhandener Datenbestände nach Migration, kontrolliertes Verhalten bei nicht unterstützten Datenbankständen und nachvollziehbare Backup-/Wiederherstellungsprozesse. Die Kriterien sollten nicht als bereits erfüllte Eigenschaften des Zielartefakts formuliert werden, solange die Implementierung und Evaluation noch nicht vorliegen.

Der Bezug zu FF1 und FF3 ist stark: Der Abschnitt liefert das Zielbild für die framework-agnostische Transformation und motiviert lokale relationale Persistenz sowie Migrationen. Zu FF2 und FF4 besteht ein indirekter, aber sinnvoller Übergang über IPC, lokale Geschäftslogik und Sicherheitsverantwortung. Die konkreten Refactoring-Muster und Sicherheitsbefunde gehören jedoch in die späteren Analyse-, Entwurfs- und Evaluationskapitel.

## Sprachliche und formale Qualität

Die Sprache ist insgesamt wissenschaftlich, sachlich und gut lesbar. Satzbau, Grammatik und technische Terminologie sind überwiegend sicher. Die vielen kurzen Absätze unterstützen die Orientierung, führen bei 213 Zeilen aber auch zu einer gewissen Segmentierung.

Einige englische Begriffe sind fachlich gerechtfertigt, sollten bei der ersten Verwendung einheitlich behandelt werden: Local First, Offline-First, „No spinners“, „The Long Now“, Operational Transformation, Conflict-free Replicated Data Types, Last Write Wins und Renderer. Die Anführungszeichen und Schreibweisen sollten in der späteren LaTeX-Fassung konsistent an die gewählte Terminologie angepasst werden.

Die Datei ist ein Markdown-Draft, enthält aber bereits LaTeX-nahe Zitierformen wie `~\cite{...}`. Für die spätere Überführung ist zu prüfen, ob diese Syntax im finalen `.tex` korrekt in den jeweiligen Satz integriert wird. Der Abschnitt „Quellenhinweise für die spätere Überführung“ ist als Arbeitsnotiz sinnvoll, sollte aber nicht unverändert in den wissenschaftlichen Kapiteltext gelangen.

## Qualitätsdichte

Die Qualitätsdichte ist **hoch**, stellenweise jedoch durch Wiederholung reduziert. Inhaltlich relevante Wiederholungen sind:

- lokale Primärpersistenz statt Cache: Zeilen 14--17, 82--85 und 153--157;
- Electron allein erzeugt kein Local First: Zeilen 7--10 und 159--162;
- Synchronisation und Kollaboration sind nicht zwingend: Zeilen 46, 68--69, 78, 124--136 und 190--193;
- Local First verschiebt Sicherheits- und Betriebsverantwortung: Zeilen 53--58, 100--104 und 195--198.

Diese Wiederholungen sind nicht sachlich falsch. Zwei bis drei zentrale Aussagen könnten jedoch einmal definitorisch und einmal in der Anwendung auf die Fallstudie erscheinen; die übrigen Wiederholungen sollten gestrichen oder zu einer kompakten Synthese zusammengeführt werden. Der Exkurs zu Operational Transformation und CRDTs ist für die theoretische Einordnung relevant, sollte aber proportional zum tatsächlich geplanten Synchronisationsumfang bleiben.

# Zitatbasierte Befunde

> **Zitat:** „Daten und zentrale Anwendungsoperationen sollen zunächst auf dem Gerät der nutzenden Person stattfinden“

- **Fundstelle:** Einleitung, Absatz 1
- **Kategorie:** begrifflich / Quelle
- **Schweregrad:** wichtig
- **Problem:** Die zentrale Definition ist plausibel, wird aber erst am Ende des Absatzes mit Kleppmann belegt. Es bleibt offen, welche Teile direkte Literaturdefinition und welche Teile die Arbeitsdefinition dieser Arbeit sind.
- **Verbesserung:** Die Arbeitsdefinition ausdrücklich als solche markieren und die Literaturquelle unmittelbar an die übernommenen Merkmale anschließen.
- **Belegbedarf:** prüfen

> **Zitat:** „in der eine entfernte Datenbank die autoritative Datenquelle darstellt und der Client überwiegend als Repräsentation oder Cache dieser Daten dient“

- **Fundstelle:** Einleitung, Absatz 1
- **Kategorie:** fachlich / begrifflich / Quelle
- **Schweregrad:** wichtig
- **Problem:** Die Aussage beschreibt ein bestimmtes serverzentriertes Modell, klingt aber wie eine allgemeine Definition cloudzentrierter Anwendungen. Webanwendungen können auch lokale Schreibmodelle, Replikation oder andere Autoritätsmodelle besitzen.
- **Verbesserung:** Auf „die im Ausgangsartefakt betrachtete serverzentrierte Architektur“ einschränken oder den Begriff „cloudzentriert“ mit einer Quelle definieren.
- **Belegbedarf:** [QUELLENREICHWEITE PRÜFEN]

> **Zitat:** „Bei strukturierten Daten ist eine rein textbasierte Zusammenführung oft unzureichend“

- **Fundstelle:** Abschnitt 2.4.1, Absatz zum vierten Ideal
- **Kategorie:** fachlich / Quelle
- **Schweregrad:** wichtig
- **Problem:** Die Aussage ist technisch plausibel, verallgemeinert aber die Eignung textbasierter Merge-Verfahren über unterschiedliche Datenmodelle hinweg.
- **Verbesserung:** Den Geltungsbereich auf fachlich strukturierte, semantisch voneinander abhängige Daten einschränken und als Begründung für die spätere Modellwahl kennzeichnen.
- **Belegbedarf:** [BELEG ERFORDERLICH]

> **Zitat:** „Dafür müssen Daten in einem nachvollziehbaren Format gespeichert, exportiert und gegebenenfalls in zukünftige Schema-Versionen überführt werden können.“

- **Fundstelle:** Abschnitt 2.4.1, fünftes Ideal
- **Kategorie:** fachlich / Quelle
- **Schweregrad:** wichtig
- **Problem:** Langfristige Datenzugänglichkeit und Migration werden sinnvoll zusammengeführt, aber die konkrete Forderung nach Schema-Überführung ist eine technische Ausarbeitung der Arbeit und nicht ohne Weiteres eine direkte Aussage der genannten Local-First-Quelle.
- **Verbesserung:** Literaturbefund und eigene technische Konsequenz in zwei Sätze trennen.
- **Belegbedarf:** [QUELLENREICHWEITE PRÜFEN]

> **Zitat:** „Local First ist jedoch nicht automatisch sicherer.“

- **Fundstelle:** Abschnitt 2.4.1, sechstes Ideal
- **Kategorie:** fachlich / Quelle
- **Schweregrad:** wichtig
- **Problem:** Der wichtige Sicherheitsvorbehalt wird nicht belegt. Außerdem sollte zwischen geringerer zentraler Datensammlung, Schutz des Endgeräts und Sicherheit der konkreten Anwendung unterschieden werden.
- **Verbesserung:** Die Aussage als Sicherheitsimplikation der Arbeit kennzeichnen und die drei Ebenen getrennt erläutern.
- **Belegbedarf:** [BELEG ERFORDERLICH]

> **Zitat:** „Offline-First und Local First sind eng verwandte, aber nicht vollständig deckungsgleiche Begriffe.“

- **Fundstelle:** Abschnitt 2.4.2, Absatz 1
- **Kategorie:** begrifflich / Quelle
- **Schweregrad:** wichtig
- **Problem:** Die Abgrenzung ist zentral für die Arbeit, wird aber als etablierte Definition präsentiert, ohne eine Quelle oder eine explizite Arbeitsdefinition anzugeben.
- **Verbesserung:** Für beide Begriffe Mindestkriterien definieren und anschließend klar kennzeichnen, welche Definition in dieser Arbeit verwendet wird.
- **Belegbedarf:** [BEGRIFF DEFINIEREN] [BELEG ERFORDERLICH]

> **Zitat:** „SQLite arbeitet als serverlose und konfigurationsarme Bibliothek innerhalb des Anwendungsprozesses und speichert eine vollständige relationale Datenbank in einer Datei.“

- **Fundstelle:** Abschnitt 2.4.2, Absatz zu SQLite
- **Kategorie:** fachlich / Quelle
- **Schweregrad:** wichtig
- **Problem:** Die Aussage ist als SQLite-Grundbeschreibung geeignet, aber der verwendete Schlüssel `sqlite2025about` fehlt in der aktuellen `refs.bib`. „In einer Datei“ sollte außerdem nicht mit einem beliebigen Speicherort oder automatisch sicherem Dateibetrieb gleichgesetzt werden.
- **Verbesserung:** Den verifizierten Dokumentationsbeleg ergänzen und im Anwendungskontext zwischen SQLite-Datei, Benutzerverzeichnis und Installationsressourcen unterscheiden.
- **Belegbedarf:** [PRODUKTDETAIL PRÜFEN]

> **Zitat:** „Die Datenbank unterstützt ACID-Transaktionen“

- **Fundstelle:** Abschnitt 2.4.2, Absatz zu SQLite
- **Kategorie:** fachlich / Quelle
- **Schweregrad:** wichtig
- **Problem:** Die Aussage braucht die fehlende SQLite-Quelle und sollte die Betriebsbedingungen berücksichtigen. ACID-Eigenschaften der Engine sind nicht gleichbedeutend mit einer vollständigen Backup- oder Wiederherstellungsgarantie.
- **Verbesserung:** Den Beleg direkt anführen und die Aussage ausdrücklich von Backup, Dateisystemschutz und Wiederherstellbarkeit abgrenzen.
- **Belegbedarf:** [PRODUKTDETAIL PRÜFEN]

> **Zitat:** „Migrationen sollten möglichst in transaktionalen Grenzen ausgeführt werden“

- **Fundstelle:** Abschnitt 2.4.2, Absatz zu Schema-Migrationen
- **Kategorie:** fachlich / Produktdetail
- **Schweregrad:** wichtig
- **Problem:** Hier werden allgemeine Migrationspraxis, SQLite-Verhalten und die konkrete Drizzle-Ausführung vermischt. Nicht jede Änderung und nicht jeder Migration Runner verhält sich identisch.
- **Verbesserung:** Als Zielanforderung formulieren und für die verwendete Drizzle-/SQLite-Konfiguration prüfen, welche Schritte transaktional ausgeführt, abgebrochen oder wiederholt werden können.
- **Belegbedarf:** [PRODUKTDETAIL PRÜFEN]

> **Zitat:** „Insbesondere bei lokalen Anwendungen müssen Migrationen mit dem Anwendungspaket ausgeliefert, beim Start kontrolliert angewendet und für alle unterstützten Ausgangsversionen getestet werden.“

- **Fundstelle:** Abschnitt 2.4.2, Absatz zu Schema-Migrationen
- **Kategorie:** Struktur / fachlich
- **Schweregrad:** wichtig
- **Problem:** Die Empfehlung ist für das Zielartefakt plausibel, wird aber als allgemeine Notwendigkeit formuliert. Sie enthält bereits konkrete Designentscheidungen zur Paketierung, zum Startverhalten und zur Teststrategie.
- **Verbesserung:** Mit „Für das Zielartefakt wird daher gefordert ...“ als eigene Anforderung markieren und in späteren Kapiteln evaluierbar machen.
- **Belegbedarf:** prüfen

> **Zitat:** „Abgrenzung zu Cloud-Native und traditionellen Desktop-Anwendungen“

- **Fundstelle:** Überschrift 2.4.3
- **Kategorie:** begrifflich / Struktur
- **Schweregrad:** kritisch
- **Problem:** Der Abschnitt definiert Cloud-Native nicht und behandelt im Text hauptsächlich Cloud-Zentrierung beziehungsweise Serverabhängigkeit. Dadurch entsteht ein Terminologierisiko für die gesamte Arbeit.
- **Verbesserung:** Entweder Cloud-Native mit eigener Quelle und klaren Kriterien einführen oder die Überschrift auf die tatsächlich vorgenommene Abgrenzung anpassen.
- **Belegbedarf:** [BEGRIFF DEFINIEREN]

> **Zitat:** „Local First kann daher als Hybrid zwischen den beiden Modellen verstanden werden.“

- **Fundstelle:** Abschnitt 2.4.3, Absatz 3
- **Kategorie:** begrifflich / fachlich
- **Schweregrad:** wichtig
- **Problem:** Die Formulierung kann den Eindruck erwecken, Local First setze stets einen Cloud-Dienst voraus. Das widerspricht der später korrekt genannten Möglichkeit eines reinen Einzelbenutzer-/Einzelgerätszenarios.
- **Verbesserung:** Als optionale Verbindung lokaler Autonomie mit ausgewählten Netzwerkdiensten beschreiben und den rein lokalen Fall unmittelbar mit abdecken.
- **Belegbedarf:** [AUSSAGE ZU ABSOLUT]

> **Zitat:** „Electron stellt die Ausführungsumgebung und den kontrollierten Zugriff auf Betriebssystemressourcen bereit.“

- **Fundstelle:** Abschnitt 2.4.3, letzter Absatz
- **Kategorie:** fachlich / Produktdetail
- **Schweregrad:** wichtig
- **Problem:** Electron stellt APIs und Prozessgrenzen bereit, kontrolliert den Zugriff aber nicht automatisch im Sinne einer wirksamen Sicherheitsgarantie. Die Kontrolle entsteht erst durch Konfiguration, Preload-/IPC-Design und Validierung.
- **Verbesserung:** „Electron stellt die Ausführungsumgebung und technische Mechanismen für eine kontrollierte Vermittlung ... bereit“ wäre präziser.
- **Belegbedarf:** [PRODUKTDETAIL PRÜFEN]

> **Zitat:** „Die lokale Persistenz darf nicht direkt aus dem Renderer-Prozess heraus angesprochen werden.“

- **Fundstelle:** Abschnitt 2.4.4, Absatz 3
- **Kategorie:** fachlich / Struktur
- **Schweregrad:** wichtig
- **Problem:** Als Sicherheitsanforderung für die Zielarchitektur ist die Aussage überzeugend. Als allgemeine Aussage über jede Electron-Anwendung ist sie zu absolut und sollte als bewusste Designentscheidung der Arbeit erscheinen.
- **Verbesserung:** Den Satz mit „In der sicherheitsgehärteten Zielarchitektur ...“ beginnen und die Begründung auf die Vertrauensgrenze beziehen.
- **Belegbedarf:** [AUSSAGE ZU ABSOLUT]

> **Zitat:** „Für die Fallstudie des Studienplaners steht zunächst ein lokales Einzelplatzszenario im Mittelpunkt.“

- **Fundstelle:** Abschnitt 2.4.4, Absatz 4
- **Kategorie:** Scope / Struktur
- **Schweregrad:** wichtig
- **Problem:** Diese zentrale Eingrenzung ist nicht als aus Anforderungen oder einer dokumentierten Designentscheidung abgeleitet ausgewiesen. Sie steuert jedoch, ob Synchronisation und Kollaboration als Ziel oder Nichtziel gelten.
- **Verbesserung:** Die Scope-Entscheidung explizit begründen und in den Anforderungen der Fallstudie beziehungsweise im Methodikteil referenzieren.
- **Belegbedarf:** prüfen

> **Zitat:** „Die Anwendung muss Datenbanken sicher anlegen und aktualisieren, Fehler bei Schreibvorgängen kontrolliert behandeln, Backups ermöglichen und mit beschädigten oder nicht unterstützten Datenbankständen umgehen.“

- **Fundstelle:** Abschnitt 2.4.4, letzter Absatz vor der Schlussfolgerung
- **Kategorie:** fachlich / Struktur
- **Schweregrad:** wichtig
- **Problem:** Die Aufzählung enthält wertvolle Zielanforderungen, geht aber über die bisher explizit begründeten Local-First-Ideale hinaus. „Sicher“ und „kontrolliert“ sind zudem noch nicht operationalisiert.
- **Verbesserung:** Die Punkte als vorläufige Sicherheits- und Betriebsanforderungen kennzeichnen und jeweils ein späteres Prüf- oder Evaluationskriterium ergänzen.
- **Belegbedarf:** [BELEG ERFORDERLICH]

> **Zitat:** „Die im Draft verwendeten Schlüssel `kleppmann2019localfirst` und `sqlite2025about` sind daher als vorläufige Schlüssel zu verstehen“

- **Fundstelle:** Abschnitt „Quellenhinweise für die spätere Überführung“, letzter Absatz
- **Kategorie:** formal / Quelle
- **Schweregrad:** kritisch
- **Problem:** Die Arbeitsnotiz dokumentiert korrekt ein aktuelles Überführungsrisiko, gehört aber nicht in den späteren Kapiteltext. Gleichzeitig ist die Aussage eine konkrete Warnung, dass der Quellenapparat derzeit nicht kompilierbar beziehungsweise nicht vollständig aufgelöst ist.
- **Verbesserung:** Die Notiz vor der Überführung aus dem Kapitel entfernen und die tatsächlich geprüften Bibliographieeinträge im Quellenapparat anlegen.
- **Belegbedarf:** prüfen

# Priorisierte To-do-Liste

## A: Kritisch

- `kleppmann2019localfirst` und `sqlite2025about` gegen tatsächlich geprüfte Bibliographieeinträge abgleichen; keine vorläufigen Schlüssel in die LaTeX-Fassung übernehmen.
- Den Begriff „Cloud-Native“ definieren und mit dem verwendeten Begriff „cloudzentriert“ abgrenzen oder die Überschrift und den Text konsequent umbenennen.
- Für alle nicht durch Kleppmann beziehungsweise SQLite gedeckten Kernbehauptungen die Herkunft festlegen: Literaturbeleg, Produktdokumentation, eigene Fallstudienbeobachtung oder Zielanforderung.
- Die Quellenhinweise am Ende als interne Arbeitsnotiz aus dem finalen Kapiteltext entfernen.

## B: Wichtig

- Local First und Offline-First als Arbeitsdefinitionen mit klaren Mindestkriterien formulieren.
- Abschnitt 2.4.3 und 2.4.4 sprachlich in Literaturbefund, technische Konsequenz, Fallstudienbeobachtung und Zielarchitektur aufteilen.
- Wiederholungen zu Primärpersistenz, optionaler Synchronisation, Electron und Sicherheitsverantwortung kürzen.
- SQLite-Datei, Benutzerverzeichnis, Installationsressourcen, Migration Runner, Transaktionsgrenzen und Wiederherstellung für die konkrete Zielarchitektur prüfen.
- Aus den letzten Absätzen konkrete, später evaluierbare Kriterien für Offline-Kernfunktionen, Migration, Datenintegrität, Backup und Fehlerbehandlung ableiten.
- Aussagen wie „Electron stellt ... kontrollierten Zugriff bereit“ und „Die lokale Persistenz darf ... nicht“ als konfigurierte Designentscheidung der Zielarchitektur formulieren, nicht als automatische Eigenschaft der Runtime.

## C: Optional

- Den Exkurs zu Operational Transformation und CRDTs proportional zum tatsächlichen Synchronisationsumfang kürzen oder mit einer kurzen Motivation für die spätere Nichtberücksichtigung abschließen.
- Die Beispiele WinForms und Windows Presentation Foundation nur behalten, wenn die historische oder technologische Abgrenzung im weiteren Kapitel benötigt wird.
- Englische Fachbegriffe, Anführungszeichen und die spätere LaTeX-Zitationssyntax in einem abschließenden Formaldurchgang vereinheitlichen.

# Bewertungstabelle

| Kriterium | Bewertung (0--10) | Begründung |
|---|---:|---|
| Fachliche Präzision | 8 | Sehr differenzierte Kernaussagen; einzelne Produkt- und Sicherheitsdetails benötigen Einschränkungen. |
| Begriffs- und Terminologieklarheit | 8 | Local First, Offlinefähigkeit und lokale Persistenz werden gut getrennt; „Cloud-Native“ und „Hybrid“ bleiben problematisch. |
| Scope und Geltungsbereich | 8 | Das Einzelplatzszenario und die Nichtnotwendigkeit von Kollaboration werden klar benannt; einige allgemeine Aussagen sind noch zu weit. |
| Argumentationslogik | 8 | Schlüssiger Aufbau von Paradigma zu Transformation; Theorie, Fallstudie und Anforderungen sollten sichtbarer getrennt werden. |
| Bezug zur Forschungsfrage | 8 | Starker Beitrag zu FF1 und FF3, sinnvoller Übergang zu FF2 und FF4. |
| DSR-Passung | 7 | Gute Ableitung eines Zielbilds; Anforderungen und spätere Evaluationskriterien müssen noch expliziter operationalisiert werden. |
| Wissenschaftlicher Ton | 8 | Sachlich, reflektiert und überwiegend präzise formuliert. |
| Sprachliche Qualität | 8 | Gute Lesbarkeit und angemessener technischer Wortschatz; stellenweise viele kurze, ähnlich gebaute Absätze. |
| Rechtschreibung und Grammatik | 9 | Keine wesentlichen Fehler erkennbar; nur abschließende Terminologie- und Zeichensetzungsprüfung nötig. |
| LaTeX und Typografie | 6 | Draft ist Markdown, enthält aber LaTeX-nahe Zitate; vor allem Zitatschlüssel und Überführungsformat müssen geprüft werden. |
| Quellen- und Belegfähigkeit | 5 | Passende zentrale Quelle vorhanden, aber Schlüssel fehlen in `refs.bib` und viele Anspruchsgruppen sind unbelegt. |
| Informations- und Qualitätsdichte | 7 | Inhaltlich hoch, durch wiederholte Synthesen und einen längeren Synchronisationsexkurs teilweise verdichtet. |
| Praktische Überarbeitbarkeit | 9 | Die Struktur ist stabil; die notwendigen Korrekturen sind klar lokalisierbar und erfordern keine vollständige Neufassung. |

# Gesamturteil

Der Entwurf ist **nicht unmittelbar übernahmefertig**, aber eine sehr gute Grundlage für die spätere LaTeX-Fassung. Die fachliche Linie ist tragfähig, die Fallstudienrelevanz ist sichtbar, und mehrere typische Fehlgleichsetzungen werden ausdrücklich vermieden. Vor der Überführung müssen jedoch die Bibliographieeinträge und Belegreichweiten geklärt, „Cloud-Native“ und Offline-First terminologisch abgesichert, die Zielanforderungen als solche markiert und die Redundanzen gekürzt werden.

Die passende Einstufung lautet **draft-quality mit gezielter substanzieller Überarbeitung**. Eine vollständige inhaltliche Neufassung ist nicht erforderlich; nach A- und B-Maßnahmen ist der Abschnitt voraussichtlich nahe an einer übernahmefähigen Grundlagenfassung.
