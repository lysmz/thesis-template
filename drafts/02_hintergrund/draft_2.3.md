# 2.3 Sicherheit lokaler Desktop-Anwendungen

## Funktion des Kapitels

Abschnitt 2.2.3 beschreibt das technische Sicherheitsmodell von Electron.
Der vorliegende Abschnitt nimmt darauf auf und betrachtet die Sicherheitsprobleme aus Sicht der lokalen Desktop-Zielanwendung beziehungsweise des konkreten Transformationsszenarios des Studienplaners.
Im Mittelpunkt stehen die zu schützenden Ressourcen, mögliche Angriffsflächen und die besonderen Risiken eines KI-generierten Ausgangssystems.

Die Electron-Mechanismen werden dabei nicht erneut als technische Dokumentation vorgestellt.
Sie dienen vielmehr als Grundlage für die Ableitung von Sicherheitszielen und überprüfbaren Anforderungen an das Zielartefakt.

## 2.3.1 Bedrohungsmodell und Angriffsflächen

Die hier betrachtete lokale Desktop-Zielanwendung besitzt andere Vertrauensannahmen als die cloudzentrierte Web-Referenzarchitektur des Ausgangsartefakts.
In der Web-Referenzarchitektur liegt die Durchsetzung von Zugriffsgrenzen maßgeblich im Backend, in der Zielanwendung befinden sich Daten, Programmcode und Laufzeit zumindest teilweise auf dem Endgerät.
Dadurch entfallen serverseitige Schutzgrenzen, während neue Angriffsflächen entstehen.

Zur Strukturierung der Bedrohungen lassen sich die Schutzziele Vertraulichkeit, Integrität und Verfügbarkeit zugrunde legen [BELEG ERFORDERLICH]; ein etabliertes Bedrohungsmodell wie STRIDE [BEGRIFF DEFINIEREN] kann zur Systematisierung herangezogen werden [BELEG ERFORDERLICH].

Zu den relevanten Schutzobjekten gehören im Studienplaner-Szenario insbesondere die lokalen Studienplandaten mit Kursen, Prüfungen und Semesterdaten, die SQLite-Datenbank [BEGRIFF DEFINIEREN], Konfigurationsdateien, gegebenenfalls übernommene Zugangsdaten des cloudzentrierten Ausgangssystems vor Abschluss der Datenmigration [PRODUKTDETAIL PRÜFEN] sowie die Integrität der Anwendung selbst.
Diese Schutzobjekte sind für den Studienplaner bedeutsam, weil die Studienplandaten das zentrale Arbeitsergebnis darstellen und ihr Verlust oder ihre unbefugte Veränderung die fachliche Nutzung unmittelbar beeinträchtigen würde.

Als mögliche Angriffsflächen sind der Renderer-Prozess [BEGRIFF DEFINIEREN], die Preload- und IPC-Schnittstellen [BEGRIFF DEFINIEREN], Dateisystemzugriffe, externe Links, Netzwerkfunktionen, native Module [BEGRIFF DEFINIEREN] sowie Paketierungs- und Updateprozesse zu berücksichtigen.

Ein mögliches Angriffsszenario beginnt, gemäß dem in Abschnitt 2.2.3 beschriebenen Vertrauensgrenzenmodell, mit manipulierten oder unerwarteten Eingaben im Renderer.
Werden diese Eingaben über eine zu weitreichende Preload- oder IPC-Schnittstelle an den Main-Prozess weitergegeben, können daraus unzulässige Dateisystem- oder Prozessoperationen entstehen.
Die konkrete Auswirkung hängt dabei nicht nur vom ursprünglichen Fehler, sondern von den erreichbaren Fähigkeiten und den Rechten der privilegierten Anwendungsschicht ab.

Die aus der lokalen Ausführung resultierenden Bedrohungen werden im Transformationsszenario dadurch überlagert, dass das Ausgangssystem KI-generierten Code enthält; die besonderen Risiken einer solchen Codebasis betrachtet der folgende Abschnitt.

## 2.3.2 Sicherheitsrisiken KI-generierter Anwendungen

Empirische Untersuchungen zeigen, dass KI-gestützt generierter Code in nennenswertem Umfang sicherheitsrelevante Schwachstellen aufweisen kann~\cite{perry2023users, pearce2022asleep, tihanyi2024secure, asare2023copilot}.
Ob und in welchem Umfang diese Befunde auf den Studienplaner zutreffen, ist als Arbeitsannahme der Fallstudie in der späteren Analyse zu prüfen [QUELLENREICHWEITE PRÜFEN].

KI-generierter Code kann Sicherheitsrisiken enthalten, ohne dass die zugrunde liegenden Annahmen dokumentiert oder für die nutzende Person erkennbar sind [AUSSAGE ZU ABSOLUT].
Besonders relevant für die Transformation sind fehlende Eingabevalidierung, überprivilegierte Hilfsfunktionen, unsichere Standardkonfigurationen und eine unklare Trennung von Präsentation, Geschäftslogik und Infrastruktur.
Weitere Risiken betreffen hartkodierte Geheimnisse, unzureichende Fehlerbehandlung, fehlende Zugriffskontrollen und unsichere Abhängigkeiten.
Diese Kategorien sind zunächst als Arbeitsannahmen zu verstehen, deren tatsächliches Auftreten im Ausgangsartefakt belegt werden muss [BELEG ERFORDERLICH].

Darüber hinaus kann KI-generierter Code die Lieferkette gefährden.
Wenn ein Modell einen derzeit nicht existierenden Paketnamen vorschlägt (Paket-Halluzination), kann unter diesem Namen ein bösartiges Paket veröffentlicht und im Rahmen der Abhängigkeitsauflösung bezogen werden~\cite{spracklen2025package}.

Diese Risiken werden anhand geeigneter Kategorien wie Injection, Broken Access Control und Security Misconfiguration eingeordnet [BELEG ERFORDERLICH].
Eine quellengestützte Zuordnung der konkreten Befunde zu einer etablierten Klassifikation wie der OWASP Top 10 wird bewusst nicht in diesem Grundlagenabschnitt vorweggenommen, sondern im Analyse-Kapitel anhand der Untersuchung des Ausgangsartefakts vorgenommen.

## 2.3.3 Sicherheitsanforderungen an das Zielartefakt

Aus dem Bedrohungsmodell in Abschnitt 2.3.1 und den Eigenschaften des KI-generierten Ausgangscodes in Abschnitt 2.3.2 werden die folgenden Zielanforderungen abgeleitet, die bewusst als zu erfüllende Anforderungen und nicht als bereits gesicherte Eigenschaften formuliert sind.

- Der Renderer darf nicht direkt auf das Dateisystem, native Module oder allgemeine Node.js-APIs zugreifen.
- Privilegierte Operationen müssen über eine minimale und fachlich definierte Schnittstelle erfolgen.
- Eingaben aus dem Renderer müssen vor jeder privilegierten Operation erneut validiert werden.
- Dateipfade müssen auf erlaubte Verzeichnisse, Dateitypen und Operationen begrenzt werden.
- Sensible Daten dürfen nicht in Quellcode, Renderer-Bundles oder Protokollmeldungen eingebettet werden.
- Abhängigkeiten müssen nachvollziehbar, reproduzierbar und auf bekannte Quellen zurückführbar sein.
- Fehlerzustände müssen kontrolliert behandelt werden und dürfen keine internen Pfade oder Stacktraces offenlegen.
- Die Sicherheitsanforderungen müssen durch statische Prüfungen, Konfigurationskontrollen oder Tests überprüfbar sein.
- Sensible Datenbestände, insbesondere die lokale Datenbank, müssen am Speicherort gegen unbefugten Zugriff geschützt werden.
- Benutzerdaten und das unveränderliche Installationsverzeichnis der Anwendung müssen getrennt verwaltet werden.
- Der Update- und Paketierungsprozess muss gegen manipulierte oder gefälschte Installationen abgesichert sein.

Einzelne Anforderungen folgen unmittelbar aus dem Bedrohungsmodell beziehungsweise den KI-Risiken; so greift die Forderung nach erneuter Validierung aller Renderer-Eingaben das Angriffsszenario in Abschnitt 2.3.1 auf, während das Verbot hartkodierter Geheimnisse dem entsprechenden Risiko aus Abschnitt 2.3.2 entspricht.

Diese Anforderungen bilden eine Verbindung zwischen dem theoretischen Sicherheitsmodell und der späteren Analyse, dem Entwurf und der Evaluation des Artefakts.
Die konkrete Umsetzung der IPC-Kanäle, Validatoren, Datenzugriffsfunktionen und Build-Konfigurationen wird deshalb nicht in diesem Grundlagenabschnitt vorweggenommen, sondern in den folgenden Kapiteln anhand der Fallstudie dokumentiert.

## Quellenhinweise für die spätere Überführung

Für die Sicherheitsrisiken KI-generierten Codes wurden die in `thesis/refs.bib` vorhandenen Schlüssel `perry2023users`, `pearce2022asleep`, `tihanyi2024secure`, `asare2023copilot` und `spracklen2025package` verwendet.
Für OWASP Top 10, ein etabliertes Bedrohungsmodell (etwa STRIDE) und die Schutzziele Vertraulichkeit, Integrität und Verfügbarkeit sind noch keine Einträge in `thesis/refs.bib` angelegt und müssen vor der Überführung in die LaTeX-Fassung ergänzt werden [BELEG ERFORDERLICH].
