# 2.2.3 Sicherheitsmodell von Electron

Electron stellt keine automatisch sichere Ausführungsumgebung dar. Die Runtime bietet jedoch Mechanismen, mit denen privilegierte Desktop-Funktionen von nicht privilegiertem Web-Code getrennt werden können. Ihre Wirksamkeit hängt von der konkreten Konfiguration der Anwendung, der Gestaltung der Preload- und IPC-Schnittstellen sowie von der Vertrauenswürdigkeit der geladenen Inhalte ab~\cite{electron2026docs}. Die zentrale Sicherheitsfrage lautet daher, wie eine Kompromittierung des Renderers verhindert oder zumindest so begrenzt werden kann, dass daraus kein unmittelbarer Zugriff auf das Betriebssystem entsteht.

## Vertrauensgrenzen der Anwendung

Für die Sicherheitsbetrachtung ist zwischen dem Renderer-Prozess, dem Preload-Skript und dem Main-Prozess zu unterscheiden. Der Renderer-Prozess verarbeitet HTML, CSS und JavaScript und sollte grundsätzlich wie eine nicht privilegierte Web-Umgebung behandelt werden. Der Main-Prozess läuft dagegen in einer Node.js-Umgebung und kann auf Betriebssystemressourcen wie das Dateisystem, Prozesse oder native Module zugreifen. Er bildet damit eine privilegierte Vertrauenszone.

Das Preload-Skript vermittelt zwischen beiden Bereichen. Es wird in einem besonderen Kontext geladen und kann ausgewählte Funktionen aus dem Main-Prozess für den Renderer verfügbar machen. Diese Vermittlung ist jedoch keine automatische Sicherheitsgarantie. Wird im Preload-Skript eine zu mächtige Schnittstelle veröffentlicht, kann eine Schwachstelle im Renderer weiterhin privilegierte Operationen auslösen. Das Preload-Skript gehört deshalb selbst zur sicherheitskritischen Vertrauensgrenze und muss ebenso sorgfältig geprüft werden wie der Main-Prozess.

Electron verwendet intern neben Main- und Renderer-Prozessen weitere Prozesse, beispielsweise für Grafik-, Hilfs- und Absturzbehandlungsfunktionen. Für die hier betrachtete Architektur ist dennoch vor allem die Trennung zwischen privilegierter Anwendungslogik und nicht privilegiertem Web-Inhalt relevant. Sie bildet die Grundlage für die nachfolgenden Konfigurations- und Entwurfsentscheidungen.

## Absicherung des Renderer-Prozesses

Die wichtigste Maßnahme besteht darin, dem Renderer keine direkten privilegierten APIs zur Verfügung zu stellen. Die Node.js-Integration sollte im Renderer deaktiviert werden. Dadurch kann dort ausgeführter Web-Code nicht unmittelbar auf `require`, das Dateisystem oder andere Node.js-Funktionen zugreifen. Eine Aktivierung dieser Integration würde die Sicherheitsgrenze zwischen Web-Inhalt und Betriebssystem erheblich schwächen, insbesondere wenn die Anwendung dynamische oder nicht vollständig vertrauenswürdige Inhalte lädt.

Zusätzlich sollte die Context Isolation aktiviert werden. Sie trennt den JavaScript-Kontext des geladenen Web-Inhalts vom Kontext des Preload-Codes. Dadurch wird verhindert, dass der Web-Inhalt globale Objekte des Preload-Kontexts unmittelbar verändert oder sich auf diesem Weg Zugriff auf dessen Funktionen verschafft. Context Isolation ersetzt jedoch nicht die sorgfältige Gestaltung der veröffentlichten API. Eine unsichere oder zu umfangreiche Preload-Schnittstelle bleibt auch bei aktivierter Context Isolation problematisch.

Sandboxing kann die Rechte des Renderer-Prozesses weiter einschränken. Es begrenzt die verfügbaren System- und Laufzeitfunktionen und reduziert damit die möglichen Auswirkungen einer erfolgreichen Ausnutzung von Schwachstellen. Die konkreten Fähigkeiten hängen von der Electron-Version und der gewählten Fensterkonfiguration ab. Sicherheitsoptionen sollten daher für die verwendete Version dokumentiert und nicht lediglich aus allgemeinen Beispielen übernommen werden.

Weitere relevante Schutzmaßnahmen betreffen die Navigation und geladene Inhalte. Die Anwendung sollte nur erwartete lokale Inhalte oder explizit erlaubte Ursprünge laden. Externe Webseiten, unkontrollierte Weiterleitungen und unnötige Remote-Ressourcen vergrößern die Vertrauensbasis. Die Deaktivierung von Web-Sicherheitsmechanismen aus Gründen der Entwicklungsbequemlichkeit ist daher kein vertretbarer Bestandteil einer gehärteten Produktionskonfiguration.

## Preload-Skript und Capability-Schnittstelle

Das Preload-Skript sollte ausschließlich die Funktionen bereitstellen, die der Renderer für die fachliche Interaktion tatsächlich benötigt. Statt allgemeine Systemoperationen zu veröffentlichen, sollte die Schnittstelle fachliche Fähigkeiten anbieten. Eine Funktion wie `saveStudyPlan(plan)` begrenzt den möglichen Zugriff stärker als eine allgemeine Funktion wie `writeFile(path, data)`, die beliebige Pfade und Inhalte annehmen könnte.

Die Veröffentlichung solcher Funktionen erfolgt typischerweise über eine kontrollierte Bridge, beispielsweise `contextBridge`. Das vollständige Electron- oder Node.js-API darf nicht an den Renderer weitergereicht werden. Ebenso sollten keine rohen IPC-Funktionen wie `ipcRenderer.send` oder `ipcRenderer.invoke` öffentlich gemacht werden. Andernfalls könnte der Renderer beliebige Kanäle auswählen und die vorgesehene fachliche Begrenzung umgehen.

Auch Rückgabewerte und Fehlermeldungen gehören zur Schnittstellengestaltung. Sie sollten auf die fachliche Domäne der Anwendung begrenzt werden und keine internen Dateipfade, Stacktraces oder sonstigen sensiblen Informationen offenlegen. Die Preload-API ist damit als schmaler, stabiler und überprüfbarer Vertrag zwischen Benutzeroberfläche und privilegierter Anwendungslogik zu verstehen.

## IPC als sicherheitsrelevanter API-Vertrag

Inter-Process Communication (IPC) verbindet den Renderer mit dem Main-Prozess. Jeder IPC-Kanal stellt damit einen potenziellen Eingangspunkt in eine privilegierte Vertrauenszone dar und sollte ähnlich wie ein öffentlicher Backend-Endpunkt behandelt werden. Dass eine Anfrage aus dem eigenen Renderer stammt, reicht nicht als Vertrauensannahme aus. Ein kompromittierter Renderer kann gültig aussehende IPC-Anfragen selbst erzeugen.

Jeder Kanal sollte deshalb eine eindeutige fachliche Verantwortung besitzen und die Eingaben im Main-Prozess validieren. Zu prüfen sind unter anderem Datentypen, erlaubte Wertebereiche, Ressourcenbezüge, Berechtigungen und Dateipfade. Bei Pfaden darf nicht davon ausgegangen werden, dass ein vom Renderer übergebener Wert bereits innerhalb des vorgesehenen Verzeichnisses liegt. Normalisierung, erlaubte Basisverzeichnisse und eine Prüfung auf Path Traversal sind Bestandteile einer sicheren Dateizugriffsschicht.

Die Validierung muss vor der privilegierten Operation erfolgen und darf nicht nur in der Benutzeroberfläche stattfinden. Clientseitige Prüfungen unterstützen die Benutzerführung, bilden aber keine Sicherheitsgrenze. Zusätzlich sollten Lese- und Schreiboperationen sowie unterschiedliche fachliche Berechtigungen über getrennte Funktionen abgebildet werden. Ein universeller Dateisystem-Wrapper würde dem Renderer mehr Rechte einräumen als für einzelne Anwendungsfälle erforderlich sind.

Fehlerbehandlung und Nebenläufigkeit sind ebenfalls relevant. Fehler dürfen keine internen Pfade oder Diagnoseinformationen an den Renderer zurückgeben. Mehrstufige Prüfungen und Schreibvorgänge müssen so entworfen werden, dass zwischen Prüfung und Ausführung keine inkonsistenten Zustände oder Race Conditions ausgenutzt werden können. Die IPC-Schnittstelle ist daher nicht nur ein technisches Transportmittel, sondern ein sicherheitsrelevanter API-Vertrag.

## Content Security Policy und geladene Inhalte

Eine Content Security Policy (CSP) kann die Ausführung unerwarteter Skripte und die Einbindung nicht erlaubter Ressourcen begrenzen. Sie erschwert damit bestimmte Cross-Site-Scripting-Angriffe und bildet eine zusätzliche Verteidigungsschicht. CSP ersetzt jedoch weder eine sichere Datenbehandlung noch Eingabevalidierung. Ein Angreifer, der bereits über eine erlaubte Funktion verfügt, wird durch eine CSP nicht automatisch an deren missbräuchlicher Nutzung gehindert.

Für eine lokal gebündelte Anwendung sollte festgelegt werden, welche Skript-, Stil-, Bild- und Netzwerkquellen erforderlich sind. Inline-Skripte, unkontrollierte externe Ressourcen und unnötige Remote-Inhalte sollten vermieden werden. Falls die Anwendung externe Webseiten oder Dienste einbindet, müssen deren Vertrauensstatus, Aktualisierungsverhalten und mögliche Interaktionen mit privilegierten APIs ausdrücklich berücksichtigt werden. Nicht vertrauenswürdiger Remote-Inhalt darf nicht gemeinsam mit einer weitreichenden Preload-API betrieben werden.

## Beispiel einer möglichen Angriffskette

Die Bedeutung der Sicherheitsgrenzen lässt sich anhand einer typischen Angriffskette verdeutlichen. Ein Fehler bei der Verarbeitung von Eingaben kann zunächst die Ausführung fremden JavaScript-Codes im Renderer ermöglichen. Bei aktivierter Node.js-Integration könnte dieser Code unmittelbar privilegierte APIs aufrufen. Ist die Node.js-Integration deaktiviert, kann eine zu mächtige Preload- oder IPC-Schnittstelle dennoch indirekte Zugriffe auf das Dateisystem, Prozesse oder Netzwerkfunktionen ermöglichen.

Eine unzureichende Pfadvalidierung könnte beispielsweise dazu führen, dass ein Angreifer außerhalb des vorgesehenen Anwendungsverzeichnisses liest oder schreibt. Die Architektur soll eine solche Angriffskette an mehreren Stellen unterbrechen: durch die Isolation des Renderers, eine minimale Preload-API, die Validierung von IPC-Eingaben, die Begrenzung von Dateisystemrechten und eine restriktive Content Security Policy.

Nicht jede Schwachstelle im Renderer führt bei korrekter Electron-Konfiguration automatisch zu einer Remote Code Execution oder zu einem vollständigen Systemzugriff. Die konkrete Auswirkung hängt davon ab, welche Schnittstellen erreichbar sind und über welche Rechte der Main-Prozess verfügt. Umgekehrt kann eine einzelne zu mächtige Bridge die Schutzwirkung mehrerer anderer Maßnahmen erheblich reduzieren. Entscheidend ist daher nicht die Existenz einzelner Sicherheitsoptionen, sondern ihr Zusammenspiel mit einem restriktiven Anwendungsdesign.

## Bedeutung für die Transformation

Die Überführung einer Web-Anwendung in Electron besteht aus Sicherheitssicht nicht darin, den vorhandenen Web-Code unverändert in einen privilegierten Prozess zu verschieben. Renderer, Preload und Main-Prozess müssen unterschiedliche Verantwortlichkeiten erhalten. Der Renderer übernimmt Darstellung und nicht privilegierte Interaktion. Das Preload-Skript stellt minimale, fachlich definierte Fähigkeiten bereit. Der Main-Prozess führt privilegierte Operationen erst nach einer erneuten Validierung und unter Anwendung des Least-Privilege-Prinzips aus.

Für die Transformation lassen sich daraus konkrete Regeln ableiten: Der Renderer erhält keinen direkten Dateisystemzugriff. Jeder IPC-Eingang validiert strukturierte Daten. Dateipfade werden auf erlaubte Verzeichnisse und Operationen begrenzt. Allgemeine System-APIs werden nicht an den Renderer weitergereicht. Remote-Inhalte werden nur geladen, wenn sie fachlich erforderlich sind, und nicht mit privilegierten Schnittstellen vermischt.

Diese Regeln bilden die konzeptionelle Grundlage für die spätere Implementierung. Die konkreten IPC-Kanäle, Validatoren, Datenzugriffsfunktionen und Pfadregeln werden dort an der Fallstudie beschrieben. Zusätzlich zur Prozesskonfiguration muss die Lieferkette berücksichtigt werden: Electron selbst, native Module, Build-Werkzeuge und weitere Abhängigkeiten erweitern die Vertrauensbasis der Anwendung. Laufzeitsicherheit und Abhängigkeitssicherheit sind daher getrennte, aber miteinander verbundene Betrachtungsebenen.

Electron bietet somit eine technische Grundlage für die Isolation von Web-Inhalt und Desktop-Funktionalität, erzwingt diese Isolation aber nicht. Sicherheit entsteht erst durch die Kombination aus deaktivierter Node.js-Integration, Context Isolation, Sandboxing, einer minimalen Preload-API, validiertem IPC, CSP und konsequenter Rechtebegrenzung. Die zentrale Transformationsleistung besteht daher nicht nur darin, Web-Code auf dem Desktop auszuführen, sondern eine explizite und überprüfbare Vertrauensgrenze zwischen Web-Inhalt und Betriebssystem zu entwerfen. Diese Sicherheitsdimension ergänzt die in Kapitel~\ref{sec:local-first} betrachtete Frage der lokalen Datenhaltung um die Frage, welche Komponenten auf lokale Ressourcen zugreifen dürfen.
