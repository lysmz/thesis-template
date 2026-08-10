# 2.4 Sicherheit lokaler Desktop-Anwendungen

## Funktion des Kapitels

Abschnitt 2.2.3 beschreibt das technische Sicherheitsmodell von Electron. Der vorliegende Abschnitt nimmt darauf auf und betrachtet die Sicherheitsprobleme aus Sicht der lokalen Desktop-Anwendung und des konkreten Transformationsszenarios. Im Mittelpunkt stehen die zu schützenden Ressourcen, mögliche Angriffsflächen und die besonderen Risiken eines KI-generierten Ausgangssystems.

Die Electron-Mechanismen werden dabei nicht erneut als technische Dokumentation vorgestellt. Sie dienen vielmehr als Grundlage für die Ableitung von Sicherheitszielen und überprüfbaren Anforderungen an das Zielartefakt.

## 2.4.1 Bedrohungsmodell und Angriffsflächen

Eine lokale Desktop-Anwendung besitzt andere Vertrauensannahmen als eine klassische Web-Anwendung. Daten, Programmcode und Laufzeit befinden sich zumindest teilweise auf dem Endgerät. Dadurch entfallen bestimmte serverseitige Schutzgrenzen, während neue Angriffsflächen entstehen.

Zu den relevanten Schutzobjekten gehören insbesondere lokale Studienplandaten, die SQLite-Datenbank, Konfigurationsdateien, gespeicherte Zugangsdaten und die Integrität der Anwendung selbst. Als mögliche Angriffsflächen sind der Renderer-Prozess, die Preload- und IPC-Schnittstellen, Dateisystemzugriffe, externe Links, Netzwerkfunktionen, native Module sowie Paketierungs- und Updateprozesse zu berücksichtigen.

Ein mögliches Angriffsszenario beginnt mit manipulierten oder unerwarteten Eingaben im Renderer. Werden diese Eingaben über eine zu weitreichende Preload- oder IPC-Schnittstelle an den Main-Prozess weitergegeben, können daraus unzulässige Dateisystem- oder Prozessoperationen entstehen. Die konkrete Auswirkung hängt dabei nicht nur vom ursprünglichen Fehler, sondern von den erreichbaren Fähigkeiten und den Rechten der privilegierten Anwendungsschicht ab.

## 2.4.2 Sicherheitsrisiken KI-generierter Anwendungen

KI-generierter Code kann Sicherheitsrisiken enthalten, ohne dass die zugrunde liegenden Annahmen dokumentiert oder für die Nutzenden erkennbar sind. Besonders relevant für die Transformation sind fehlende Eingabevalidierung, überprivilegierte Hilfsfunktionen, unsichere Standardkonfigurationen und eine unklare Trennung von Präsentation, Geschäftslogik und Infrastruktur.

Weitere Risiken betreffen hartkodierte Geheimnisse, unzureichende Fehlerbehandlung, fehlende Zugriffskontrollen und unsichere Abhängigkeiten. Halluzinierte Paketnamen können die Lieferkette gefährden, wenn unter einem vom Modell vorgeschlagenen, aber eigentlich nicht existierenden Namen ein bösartiges Paket veröffentlicht wird. Diese Risiken werden anhand geeigneter Kategorien, beispielsweise Injection, Broken Access Control und Security Misconfiguration, systematisch eingeordnet.

## 2.4.3 Sicherheitsanforderungen an das Zielartefakt

Aus dem Bedrohungsmodell und den Eigenschaften des Ausgangscodes werden Anforderungen an die Zielarchitektur abgeleitet:

- Der Renderer darf nicht direkt auf das Dateisystem, native Module oder allgemeine Node.js-APIs zugreifen.
- Privilegierte Operationen müssen über eine minimale und fachlich definierte Schnittstelle erfolgen.
- Eingaben aus dem Renderer müssen vor jeder privilegierten Operation erneut validiert werden.
- Dateipfade müssen auf erlaubte Verzeichnisse, Dateitypen und Operationen begrenzt werden.
- Sensible Daten dürfen nicht in Quellcode, Renderer-Bundles oder Protokollmeldungen eingebettet werden.
- Abhängigkeiten müssen nachvollziehbar, reproduzierbar und auf bekannte Quellen zurückführbar sein.
- Fehlerzustände müssen kontrolliert behandelt werden und dürfen keine internen Pfade oder Stacktraces offenlegen.
- Die Sicherheitsanforderungen müssen durch statische Prüfungen, Konfigurationskontrollen oder Tests überprüfbar sein.

Diese Anforderungen bilden eine Verbindung zwischen dem theoretischen Sicherheitsmodell und der späteren Analyse, dem Entwurf und der Evaluation des Artefakts. Die konkrete Umsetzung der IPC-Kanäle, Validatoren, Datenzugriffsfunktionen und Build-Konfigurationen wird deshalb nicht in diesem Grundlagenabschnitt vorweggenommen, sondern in den folgenden Kapiteln anhand der Fallstudie dokumentiert.
