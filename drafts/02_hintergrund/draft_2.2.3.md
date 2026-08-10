# 2.2.3 Sicherheitsmodell von Electron

## Funktion des Abschnitts

Electron ist keine automatisch sichere Ausführungsumgebung. Die Runtime stellt jedoch Prozess- und Kontextgrenzen bereit, mit denen privilegierte Desktop-Funktionen von nicht privilegiertem Web-Code getrennt werden können. Die Wirksamkeit dieser Grenzen hängt von der Konfiguration der Anwendung, der Gestaltung der Preload- und IPC-Schnittstellen sowie von der Vertrauenswürdigkeit der geladenen Inhalte ab~\cite{electron2026docs}.

Dieser Abschnitt beschreibt die technischen Grundlagen dieses Sicherheitsmodells. Die Bedrohungen für lokale Desktop-Anwendungen, die spezifischen Risiken KI-generierten Codes und die daraus abgeleiteten Anforderungen werden anschließend in Abschnitt 2.4 behandelt.

## Prozesse und Vertrauensgrenzen

Für die Sicherheitsbetrachtung ist zwischen dem Renderer-Prozess, dem Preload-Skript und dem Main-Prozess zu unterscheiden. Der Renderer-Prozess verarbeitet HTML, CSS und JavaScript und sollte grundsätzlich wie eine nicht privilegierte Web-Umgebung behandelt werden. Der Main-Prozess läuft dagegen in einer Node.js-Umgebung und kann auf Betriebssystemressourcen wie das Dateisystem, Prozesse oder native Module zugreifen. Er bildet damit eine privilegierte Vertrauenszone.

Das Preload-Skript wird vor dem Laden der Web-Anwendung ausgeführt und vermittelt zwischen beiden Bereichen. Es kann ausgewählte Funktionen für den Renderer verfügbar machen, ohne dem Web-Inhalt automatisch alle Node.js- oder Electron-APIs zu überlassen. Das Preload-Skript gehört dennoch selbst zur sicherheitskritischen Vertrauensgrenze. Eine zu weitreichende Schnittstelle kann die beabsichtigte Isolation trotz einer ansonsten restriktiven Prozesskonfiguration schwächen.

Electron verwendet intern neben Main- und Renderer-Prozessen weitere Prozesse, beispielsweise für Grafik-, Hilfs- und Absturzbehandlungsfunktionen. Für die vorliegende Arbeit ist jedoch vor allem die Unterscheidung zwischen privilegiertem Anwendungscode und nicht privilegiertem Web-Inhalt relevant. Sie bildet die Grundlage für die Gestaltung der späteren Zielarchitektur.

## Isolation des Renderer-Prozesses

Die wichtigste Sicherheitsentscheidung besteht darin, dem Renderer keinen direkten Zugriff auf privilegierte APIs zu geben. Die Node.js-Integration sollte im Renderer deaktiviert werden. Dadurch kann dort ausgeführter Web-Code nicht unmittelbar auf `require`, das Dateisystem oder andere Node.js-Funktionen zugreifen.

Zusätzlich sollte die Context Isolation aktiviert werden. Sie trennt den JavaScript-Kontext des geladenen Web-Inhalts vom Kontext des Preload-Codes. Dadurch wird verhindert, dass Web-Code globale Objekte des Preload-Kontexts unmittelbar verändert oder sich auf diesem Weg Zugriff auf dessen Funktionen verschafft.

Sandboxing kann die verfügbaren System- und Laufzeitfunktionen des Renderer-Prozesses weiter einschränken. Die konkrete Wirkung hängt von der verwendeten Electron-Version und der Fensterkonfiguration ab. Sicherheitsoptionen müssen deshalb versionsbezogen dokumentiert werden, anstatt allgemeine Beispiele ohne Prüfung zu übernehmen.

Die Isolation des Renderers ist keine vollständige Sicherheitsgarantie. Sie begrenzt zunächst nur den direkten Zugriff. Ein kompromittierter Renderer kann weiterhin versuchen, erlaubte Preload- oder IPC-Funktionen missbräuchlich aufzurufen. Die Sicherheit der Anwendung hängt daher ebenso von der Begrenzung dieser Schnittstellen ab.

## Preload als Capability-Schnittstelle

Das Preload-Skript sollte ausschließlich die Funktionen bereitstellen, die der Renderer für die fachliche Interaktion benötigt. Statt allgemeine Systemoperationen zu veröffentlichen, sollte die Schnittstelle fachliche Fähigkeiten anbieten. Eine Funktion wie `saveStudyPlan(plan)` begrenzt den möglichen Zugriff stärker als eine allgemeine Funktion wie `writeFile(path, data)`, die beliebige Pfade und Inhalte annehmen könnte.

Die Veröffentlichung solcher Funktionen erfolgt typischerweise über eine kontrollierte Bridge, beispielsweise `contextBridge`. Das vollständige Electron- oder Node.js-API darf nicht an den Renderer weitergereicht werden. Ebenso sollten keine rohen IPC-Funktionen wie `ipcRenderer.send` oder `ipcRenderer.invoke` öffentlich gemacht werden. Andernfalls könnte der Renderer beliebige Kanäle auswählen und die vorgesehene fachliche Begrenzung umgehen.

Auch Rückgabewerte und Fehlermeldungen gehören zur Schnittstellengestaltung. Sie sollten auf die fachliche Domäne der Anwendung begrenzt werden und keine internen Dateipfade, Stacktraces oder sonstigen sensiblen Informationen offenlegen. Die Preload-API ist damit als schmaler und überprüfbarer Vertrag zwischen Benutzeroberfläche und privilegierter Anwendungslogik zu verstehen.

## IPC als Kommunikationsgrenze

Inter-Process Communication (IPC) verbindet den Renderer mit dem Main-Prozess. Jeder IPC-Kanal stellt damit eine kontrollierte Verbindung in eine privilegierte Vertrauenszone dar. Die Tatsache, dass eine Anfrage aus dem eigenen Renderer stammt, reicht nicht aus, um ihre Eingaben als vertrauenswürdig anzunehmen.

Die Kanäle sollten deshalb eine eindeutige fachliche Verantwortung besitzen. Das Preload-Skript sollte nicht als allgemeiner Transport für beliebige Renderer-Anfragen dienen, sondern nur klar definierte Operationen vermitteln. Die konkrete Validierung von Datentypen, Wertebereichen, Ressourcenbezügen und Dateipfaden wird im Main-Prozess beziehungsweise in der privilegierten Anwendungsschicht durchgeführt.

IPC ist damit nicht lediglich ein technisches Transportmittel. Es bildet einen API-Vertrag zwischen zwei unterschiedlich privilegierten Ausführungskontexten. Seine konkrete Absicherung und die daraus folgenden Prüfanforderungen werden in Abschnitt 2.4 als Teil des Bedrohungsmodells und der Sicherheitsanforderungen wieder aufgegriffen.

## Geladene Inhalte und zusätzliche Schutzschichten

Die Vertrauenswürdigkeit der geladenen Inhalte ist ein weiterer Bestandteil des Electron-Sicherheitsmodells. Eine Anwendung sollte möglichst nur erwartete lokale Inhalte oder ausdrücklich erlaubte Ursprünge laden. Externe Webseiten, unkontrollierte Weiterleitungen und unnötige Remote-Ressourcen vergrößern die Vertrauensbasis der Anwendung.

Eine Content Security Policy (CSP) kann die Ausführung unerwarteter Skripte und die Einbindung nicht erlaubter Ressourcen begrenzen. Sie bildet damit eine zusätzliche Verteidigungsschicht gegen bestimmte Angriffe auf den Renderer. CSP ersetzt jedoch weder eine sichere Datenbehandlung noch eine Validierung privilegierter Anfragen. Ebenso sollte die Deaktivierung von Web-Sicherheitsmechanismen aus Gründen der Entwicklungsbequemlichkeit nicht Bestandteil einer Produktionskonfiguration sein.

## Bedeutung für die Transformation

Für die Transformation einer Web-Anwendung in eine Electron-Anwendung folgt daraus, dass der vorhandene Web-Code nicht unverändert in einen privilegierten Prozess verschoben werden darf. Der Renderer übernimmt Darstellung und nicht privilegierte Interaktion. Das Preload-Skript stellt minimale, fachlich definierte Fähigkeiten bereit. Der Main-Prozess führt privilegierte Operationen über kontrollierte Schnittstellen aus.

Electron bietet somit eine technische Grundlage für die Trennung von Web-Inhalt und Desktop-Funktionalität, erzwingt diese Trennung aber nicht. Die sicherheitsrelevante Transformationsleistung besteht darin, die Vertrauensgrenze explizit zu entwerfen und die Übergänge zwischen den Ausführungskontexten kontrollierbar zu machen. Welche Bedrohungen daraus für das konkrete Artefakt entstehen und welche Anforderungen sich daraus ableiten, wird in Abschnitt 2.4 untersucht.
