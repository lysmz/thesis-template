# 2.2.3 Sicherheitsmodell von Electron

## Funktion des Abschnitts

Electron ist keine automatisch sichere Ausführungsumgebung [PRODUKTDETAIL PRÜFEN]. Die Runtime stellt jedoch Prozess- und Kontextgrenzen bereit, mit denen privilegierte Desktop-Funktionen von nicht privilegiertem Web-Code getrennt werden können. Die Wirksamkeit dieser Grenzen hängt von der Konfiguration der Anwendung, der Gestaltung der Preload- und IPC-Schnittstellen sowie von der Vertrauenswürdigkeit der geladenen Inhalte ab.

Dieser Abschnitt beschreibt die technischen Grundlagen dieses Sicherheitsmodells. Die Bedrohungen für lokale Desktop-Anwendungen, die spezifischen Risiken KI-generierten Codes und die daraus abgeleiteten Anforderungen werden anschließend in Abschnitt 2.3 behandelt.

## Prozesse und Vertrauensgrenzen

Für die Sicherheitsbetrachtung sind drei Begriffe getrennt zu halten. Die Vertrauenszone beschreibt, welchem Code vertraut wird; die Privilegien beschreiben die verfügbaren Fähigkeiten eines Prozesses; und die Betriebssystemrechte begrenzen den tatsächlichen Zugriff auf Systemressourcen. Diese Ebenen dürfen nicht synonym verwendet werden: Eine privilegierte Position im Prozessmodell bedeutet nicht automatisch eine entsprechende Freigabe auf Betriebssystemebene.

Im Prozessmodell von Electron ist zwischen dem Renderer-Prozess, dem Preload-Skript und dem Main-Prozess zu unterscheiden [PRODUKTDETAIL PRÜFEN]. Der Renderer-Prozess verarbeitet HTML, CSS und JavaScript und ist grundsätzlich als nicht vertrauenswürdiger Web-Code zu behandeln. Das Preload-Skript wird vor dem Laden der Web-Anwendung ausgeführt und stellt privilegierten Vermittlungscode mit einer begrenzten, veröffentlichten Capability-Schnittstelle dar. Der Main-Prozess läuft in einer Node.js-Umgebung und bildet privilegierte Anwendungslogik mit potenziell weitreichenden Node.js- und Betriebssystemzugriffen auf Dateisystem, Prozesse oder native Module. Der Main-Prozess besitzt damit nicht automatisch nur die für die Anwendung erforderlichen Rechte.

Das Preload-Skript kann ausgewählte Funktionen für den Renderer verfügbar machen, ohne dem Web-Inhalt automatisch alle Node.js- oder Electron-APIs zu überlassen. Es gehört dennoch selbst zur sicherheitskritischen Vertrauensgrenze: Eine zu weitreichende Schnittstelle kann die beabsichtigte Isolation trotz einer ansonsten restriktiven Prozesskonfiguration schwächen.

Electron verwendet intern neben Main- und Renderer-Prozessen weitere Prozesse, beispielsweise für Grafik-, Hilfs- und Absturzbehandlungsfunktionen. Für die vorliegende Arbeit ist jedoch vor allem die Unterscheidung zwischen privilegiertem Anwendungscode und nicht privilegiertem Web-Inhalt relevant. Sie bildet die Grundlage für die Gestaltung der späteren Zielarchitektur.

## Isolation des Renderer-Prozesses

Die wichtigste Sicherheitsentscheidung besteht darin, dem Renderer keinen direkten Zugriff auf privilegierte APIs zu geben. Die Node.js-Integration sollte im Renderer deaktiviert werden, sodass dort ausgeführter Web-Code nicht unmittelbar auf `require`, das Dateisystem oder andere Node.js-Funktionen zugreifen kann.

Zusätzlich sollte die Context Isolation aktiviert werden. Sie trennt die direkten JavaScript-Kontexte des geladenen Web-Inhalts und des Preload-Codes und verhindert, dass Web-Code globale Objekte des Preload-Kontexts unmittelbar verändert. Über eine Bridge veröffentlichte Funktionen bleiben jedoch absichtlich erreichbar; Context Isolation ersetzt diese Funktionen nicht und beschränkt sie nicht automatisch.

Das Prozess-Sandboxing kann die verfügbaren System- und Laufzeitfunktionen des Renderer-Prozesses weiter einschränken. Es ist nicht mit vollständiger Betriebssystemisolation gleichzusetzen, und seine konkrete Wirkung hängt von der verwendeten Electron-Version, der Fensterkonfiguration und der Preload-Ausführung ab. Sicherheitsoptionen müssen deshalb versionsbezogen dokumentiert und belegt werden, anstatt allgemeine Beispiele ohne Prüfung zu übernehmen. Das Prozess-Sandboxing betrifft den Renderer-Prozess; die privilegierte Anwendungslogik im Main-Prozess wird dadurch nicht eingeschränkt.

Die Isolation des Renderers ist keine vollständige Sicherheitsgarantie. Sie begrenzt zunächst nur den direkten Zugriff. Ein kompromittierter Renderer kann weiterhin versuchen, erlaubte Preload- oder IPC-Funktionen missbräuchlich aufzurufen. Die Sicherheit der Anwendung hängt daher ebenso von der Begrenzung dieser Schnittstellen ab.

## Preload als Capability-Schnittstelle

Das Preload-Skript sollte ausschließlich die Funktionen bereitstellen, die der Renderer für die fachliche Interaktion benötigt. Statt allgemeine Systemoperationen zu veröffentlichen, sollte die Schnittstelle fachliche Fähigkeiten anbieten. Eine Capability (Fähigkeit) bezeichnet dabei eine nicht weiter übertragbare Berechtigung zur Ausführung einer konkreten Operation; die Bezeichnung Capability-basierte Schnittstelle ist präziser als eine allgemeine Capability-Schnittstelle. Eine Funktion wie `saveStudyPlan(plan)` begrenzt den möglichen Zugriff stärker als eine allgemeine Funktion wie `writeFile(path, data)`, die beliebige Pfade und Inhalte annehmen könnte.

Die Veröffentlichung solcher Funktionen erfolgt typischerweise über eine kontrollierte Bridge, beispielsweise `contextBridge`. Das vollständige Electron- oder Node.js-API darf nicht an den Renderer weitergereicht werden. Ebenso sollten keine rohen IPC-Funktionen wie `ipcRenderer.send` oder `ipcRenderer.invoke` öffentlich gemacht werden. Andernfalls könnte der Renderer beliebige Kanäle auswählen und die vorgesehene fachliche Begrenzung umgehen.

Auch Rückgabewerte und Fehlermeldungen gehören zur Schnittstellengestaltung. Sie sollten auf die fachliche Domäne der Anwendung begrenzt werden und keine internen Dateipfade, Stacktraces oder sonstigen sensiblen Informationen offenlegen. Die Preload-API ist damit als schmaler und überprüfbarer Vertrag zwischen Benutzeroberfläche und privilegierter Anwendungslogik zu verstehen.

## IPC als Kommunikationsgrenze

Inter-Process Communication (IPC) verbindet den Renderer mit dem Main-Prozess [PRODUKTDETAIL PRÜFEN]. Jeder IPC-Kanal stellt damit eine kontrollierte Verbindung in eine privilegierte Vertrauenszone dar. Die Tatsache, dass eine Anfrage aus dem eigenen Renderer stammt, reicht nicht aus, um ihre Eingaben als vertrauenswürdig anzunehmen.

Neben der Validierung der Eingabedaten ist deshalb auch der Absender jeder IPC-Nachricht zu prüfen. IPC-Nachrichten können von unerwarteten Frames oder eingebetteten Inhalten ausgelöst werden. Der Main-Prozess sollte den aufrufenden Renderer beziehungsweise Frame anhand einer Origin- oder Fenster-Allowlist validieren, bevor eine Anfrage weiterverarbeitet wird.

Die Kanäle sollten eine eindeutige fachliche Verantwortung besitzen. Das Preload-Skript sollte nicht als allgemeiner Transport für beliebige Renderer-Anfragen dienen, sondern nur klar definierte Operationen vermitteln. Die konkrete Validierung von Datentypen, Wertebereichen, Ressourcenbezügen, Absender und Dateipfaden wird im Main-Prozess beziehungsweise in der privilegierten Anwendungsschicht durchgeführt. Die Pfadprüfung reduziert dabei das Risiko von Pfadüberquerungen (Path Traversal), schließt aber weitere Dateisystemrisiken wie symbolische Links oder konkurrierende Änderungen nicht automatisch aus.

IPC ist damit nicht lediglich ein technisches Transportmittel. Es bildet einen API-Vertrag zwischen zwei unterschiedlich privilegierten Ausführungskontexten. Seine konkrete Absicherung und die daraus folgenden Prüfanforderungen werden in Abschnitt 2.3 als Teil des Bedrohungsmodells und der Sicherheitsanforderungen wieder aufgegriffen.

## Geladene Inhalte und zusätzliche Schutzschichten

Die Vertrauenswürdigkeit der geladenen Inhalte ist ein weiterer Bestandteil des Electron-Sicherheitsmodells. Eine Anwendung sollte möglichst nur erwartete lokale Inhalte oder ausdrücklich erlaubte Ursprünge laden. Für das konkrete Szenario ist dabei festzulegen, welche URL-Schemata erlaubt sind, ob ausschließlich gebündelte lokale Inhalte geladen werden, wie Navigation und neue Fenster behandelt werden und ob externe Links unterstützt werden. Externe Webseiten, unkontrollierte Weiterleitungen und unnötige Remote-Ressourcen vergrößern die Vertrauensbasis der Anwendung.

Eine Content Security Policy (CSP) kann die Ausführung unerwarteter Skripte und die Einbindung nicht erlaubter Ressourcen begrenzen. Ob die CSP über einen HTTP-Header oder ein Meta-Tag umgesetzt wird und über welches Protokoll die Inhalte geladen werden, bestimmt ihre konkrete Wirksamkeit; bei lokal über `file://` geladenen Inhalten sind Ladeprotokoll und Build- beziehungsweise Deployment-Strategie zu berücksichtigen [PRODUKTDETAIL PRÜFEN]. CSP bildet damit eine zusätzliche Verteidigungsschicht gegen bestimmte Angriffe auf den Renderer, ersetzt jedoch weder eine sichere Datenbehandlung noch eine Validierung privilegierter Anfragen. Die Deaktivierung von Web-Sicherheitsmechanismen aus Gründen der Entwicklungsbequemlichkeit sollte nicht Bestandteil einer Produktionskonfiguration sein.

## Bedeutung für die Transformation

Für die Transformation einer Web-Anwendung in eine Electron-Anwendung folgt daraus, dass der vorhandene Web-Code nicht unverändert in einen privilegierten Prozess verschoben werden darf. Der Renderer übernimmt Darstellung und nicht privilegierte Interaktion. Das Preload-Skript stellt minimale, fachlich definierte Fähigkeiten bereit. Der Main-Prozess führt privilegierte Operationen über kontrollierte Schnittstellen aus. Dabei verlangt das Prinzip der geringsten Rechte (Least Privilege), dass sowohl die verfügbaren Fähigkeiten als auch die Betriebssystemrechte auf das für die Funktion erforderliche Maß begrenzt werden.

Eine Kompromittierung des Renderers ist nicht als zwangsläufige Angriffskette, sondern als möglicher Angriffspfad zu verstehen. Je nach Origin, Frame, Preload-API, IPC-Senderprüfung und Betriebssystemrechten ergeben sich unterschiedliche Auswirkungen: von Cross-Site-Scripting (XSS) im Renderer über eine Privilege Escalation über die Preload- oder IPC-Schnittstelle bis hin zu einer Remote Code Execution (RCE) im Main-Prozess.

Schließlich sind Laufzeit- und Lieferkettensicherheit getrennt zu betrachten [BELEG ERFORDERLICH]. Electron, Chromium, Node.js, native Module und über den Paketmanager bezogene Abhängigkeiten sind Laufzeitbestandteile der ausgelieferten Anwendung; ihre Versionen, Herkunft und Aktualisierungsverfahren müssen kontrolliert werden. Build-Werkzeuge und der Buildprozess gehören dagegen nicht in gleicher Weise zur Laufzeitumgebung, sind aber für die Integrität des erzeugten Artefakts relevant.

Electron bietet somit eine technische Grundlage für die Trennung von Web-Inhalt und Desktop-Funktionalität, erzwingt diese Trennung aber nicht. Die sicherheitsrelevante Transformationsleistung besteht darin, die Vertrauensgrenze explizit zu entwerfen und die Übergänge zwischen den Ausführungskontexten kontrollierbar zu machen. Welche Bedrohungen daraus für das konkrete Artefakt entstehen und welche Anforderungen sich ableiten, wird in Abschnitt 2.3 untersucht.

## Quellenhinweise für die spätere Überführung

Die für diesen Abschnitt benötigten Quellen sind nicht in `thesis/refs.bib` vorhanden; die Stellen sind im Fließtext mit `[PRODUKTDETAIL PRÜFEN]` beziehungsweise `[BELEG ERFORDERLICH]` markiert. Vor der Überführung in die LaTeX-Fassung sind tatsächlich geprüfte Bibliographieeinträge anzulegen; für die Electron-Sicherheitsdokumentation sind die verwendete Electron-Version und das Abrufdatum zu dokumentieren.

- **Electron-Sicherheitsdokumentation (offiziell):** Belegtyp: Produktdokumentation (Prozessmodell, `nodeIntegration`, Context Isolation, Prozess-Sandboxing, Preload- und IPC-Sicherheit, IPC-Senderprüfung, CSP-Grenzen); der im Entwurf verwendete Schlüssel `electron2026docs` ist nicht in `refs.bib` und als vorläufiger Schlüssel zu verstehen [PRODUKTDETAIL PRÜFEN].
- **Allgemeine Sicherheits- und Abhängigkeitsaussagen** (Least Privilege, XSS, RCE, Lieferketten- und Abhängigkeitssicherheit): Belegtyp: theoretische Definition aus Literatur nachzutragen [BELEG ERFORDERLICH].
- **CSP-Wirksamkeit in Abhängigkeit von Ladeprotokoll und Bereitstellung:** Belegtyp: Produktdokumentation beziehungsweise Web-Standard [BELEG ERFORDERLICH].
- **Aussagen zum konkreten Transformationsszenario** (gebündelte Inhalte, IPC-Kanäle, SQLite-Zugriffe, erlaubte Verzeichnisse, externe Links, Updates): Belegtyp: eigene Fallstudienbeobachtung aus DSR-Artefakten der Implementierung [BELEG ERFORDERLICH].
