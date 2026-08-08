# Bewertung

Der Entwurf ist insgesamt fachlich solide und deutlich präziser als die Planungsideen sowie `draft_2.2.1.md` und `draft_2.2.2.md`. Die wesentlichen Sicherheitsprinzipien werden korrekt dargestellt. Kritisch sind vor allem fehlende bzw. nicht nachgewiesene Quellen, einzelne terminologische Überdehnungen und einige ausgelassene Electron-Sicherheitsaspekte.

## Kritische Befunde

### 1. Nicht auflösbare Quelle `electron2026docs`

**Fundstelle:** `draft_2.2.3.md:3, 11`

`electron2026docs` wird zitiert, ist aber in `thesis/refs.bib` nicht vorhanden. Das führt zu einem fehlerhaften beziehungsweise unvollständigen Literaturverweis.

**Status:** Tatsächlicher Fehler.

**Korrektur:** Einen Eintrag für die konkrete Electron-Sicherheitsdokumentation ergänzen und möglichst abschnittsweise gezielt darauf verweisen. Die verwendete Electron-Version und das Abrufdatum sollten dokumentiert werden. Die Nachbardatei `draft_2.2.2.md` enthält zusätzlich nicht aufgelöste Schlüssel wie `tauri2025docs`, `tang2024tauri`, `nwjs2026docs` und `flutter2026docs`; das sollte vor der Übernahme in LaTeX ebenfalls bereinigt werden.

### 2. IPC-Senderprüfung fehlt trotz zentraler Sicherheitsargumentation

**Fundstelle:** `draft_2.2.3.md:33–39`

Der Text fordert eine Validierung der Eingabedaten, erwähnt aber nicht die Prüfung des IPC-Senders beziehungsweise des `senderFrame`. Die aktuelle Electron-Sicherheitsdokumentation empfiehlt ausdrücklich, den Absender aller IPC-Nachrichten zu validieren, da auch unerwartete Frames oder eingebettete Inhalte IPC-Nachrichten auslösen können.

**Status:** Fachlich relevante Lücke.

**Korrektur:** Ergänzen, dass neben Payload, Berechtigungen und Pfaden auch der aufrufende Renderer beziehungsweise Frame anhand einer Origin- oder Fenster-Allowlist geprüft werden muss.

### 3. Vermischung von Vertrauensgrenze, Privilegien und Betriebssystemrechten

**Fundstelle:** `draft_2.2.3.md:7–11, 19, 57`

„Privilegierte Vertrauenszone“, „Rechte“ und „Zugriff auf Betriebssystemressourcen“ werden teilweise synonym verwendet. Eine Vertrauenszone beschreibt, welchem Code vertraut wird; Privilegien beschreiben verfügbare Fähigkeiten; Sandboxing begrenzt die Prozessrechte. Der Main-Prozess besitzt außerdem nicht automatisch nur die für die Anwendung erforderlichen Rechte.

**Status:** Akademisch-technische Präzisionsschwäche, teilweise irreführend.

**Korrektur:** Begriffe explizit trennen:

- Renderer: grundsätzlich nicht vertrauenswürdiger beziehungsweise weniger privilegierter Web-Code.
- Preload: privilegierter Vermittlungscode mit begrenzter veröffentlichter Capability-Schnittstelle.
- Main-Prozess: privilegierte Anwendungslogik mit potenziell weitreichenden Node.js- und Betriebssystemzugriffen.
- Sandboxing: zusätzliche Begrenzung des Renderer-Prozesses, keine Einschränkung des Main-Prozesses.

## Wichtige Befunde

### 4. Context Isolation wird leicht überinterpretiert

**Fundstelle:** `draft_2.2.3.md:17`

Die Aussage, Context Isolation verhindere, dass Web-Inhalt „sich auf diesem Weg Zugriff auf dessen Funktionen verschafft“, ist zu weit formuliert. Context Isolation verhindert den direkten Zugriff auf den Preload-Kontext und dessen globale Objekte. Über `contextBridge` veröffentlichte Funktionen bleiben jedoch absichtlich erreichbar.

**Status:** Präzisionsproblem, kein grundlegender Sachfehler.

**Korrektur:** Formulieren, dass Context Isolation die direkten JavaScript-Kontexte trennt, aber veröffentlichte Bridge-Funktionen nicht ersetzt oder automatisch beschränkt.

### 5. Sandboxing bleibt zu unbestimmt

**Fundstelle:** `draft_2.2.3.md:19`

„Sandboxing kann die Rechte des Renderer-Prozesses weiter einschränken“ ist grundsätzlich korrekt, bleibt aber unpräzise. Es sollte klargestellt werden, dass Electron-Prozess-Sandboxing nicht mit vollständiger Betriebssystemisolation gleichzusetzen ist und dass seine konkrete Wirkung von Electron-Version, Renderer-Konfiguration und Preload-Ausführung abhängt.

**Status:** Schwache fachliche Präzision.

**Korrektur:** Den Begriff „Prozess-Sandboxing“ verwenden und die Aussage auf Renderer-Prozesse begrenzen. Zusätzlich sollte die Standardaktivierung versionsabhängig belegt werden, wie bereits in `ideen.md:130` vorgesehen.

### 6. Pfadvalidierung wird als zu umfassende Schutzmaßnahme dargestellt

**Fundstelle:** `draft_2.2.3.md:35, 39, 51`

Normalisierung, Basisverzeichnisse und Path-Traversal-Prüfungen sind erforderlich, verhindern aber nicht automatisch alle Dateisystemrisiken. Symbolische Links, Dateitypwechsel, konkurrierende Änderungen und TOCTOU-Probleme können zusätzliche Maßnahmen erfordern. Die Formulierung zu Race Conditions in Zeile 39 ist zudem sehr allgemein und wird nicht weiter erklärt.

**Status:** Überdehnung einer richtigen Grundidee.

**Korrektur:** Den Anspruch begrenzen: Pfadvalidierung reduziert Path-Traversal-Risiken; konkrete Schreiboperationen müssen zusätzlich atomar, möglichst innerhalb kontrollierter Verzeichnisse und mit geeigneten Dateisystemoperationen umgesetzt werden. Nur dann ausführlicher auf Race Conditions eingehen, wenn dies in der Fallstudie tatsächlich untersucht wird.

### 7. „Nur erwartete lokale Inhalte“ ist nicht ausreichend spezifiziert

**Fundstelle:** `draft_2.2.3.md:21, 45`

Die Forderung nach lokalen Inhalten oder erlaubten Ursprüngen ist korrekt, bleibt aber abstrakt. Electron empfiehlt zusätzlich die Begrenzung von Navigation, neu erzeugten Fenstern, WebViews und externen Links. Besonders `shell.openExternal` kann bei ungeprüften URLs sicherheitskritisch sein.

**Status:** Relevante, aber nicht zwingende Auslassung.

**Korrektur:** Für das konkrete Szenario festlegen:

- welche URL-Schemata erlaubt sind,
- ob ausschließlich gebündelte lokale Inhalte geladen werden,
- wie Navigation und neue Fenster behandelt werden,
- ob externe Links überhaupt unterstützt werden.

### 8. CSP wird korrekt relativiert, aber technisch unvollständig eingeordnet

**Fundstelle:** `draft_2.2.3.md:41–45`

Die Beschreibung von CSP als zusätzlicher Schutzschicht ist korrekt. Es fehlt jedoch die Unterscheidung zwischen CSP über HTTP-Header und CSP-Meta-Tag sowie der Hinweis, dass die konkrete Wirksamkeit vom verwendeten Ladeprotokoll und der Build-/Deployment-Strategie abhängt. Dies ist insbesondere relevant, wenn lokale Inhalte über `file://` geladen werden.

**Status:** Auslassung, kein Fehler.

**Korrektur:** Die konkrete CSP-Strategie erst nach Festlegung der Electron-Ladearchitektur beschreiben. Die Planungsideen weisen auf diesen offenen Punkt bereits hin (`ideen.md:132–133`).

### 9. Angriffskette ist plausibel, aber stellenweise zu linear

**Fundstelle:** `draft_2.2.3.md:47–53`

Die Darstellung „Renderer-Schwachstelle → Node.js-Zugriff → Dateisystemzugriff“ ist didaktisch verständlich, kann aber den Eindruck eines zwangsläufigen Ablaufs erwecken. Eine Renderer-Kompromittierung führt abhängig von Origin, Frame, Preload-API, IPC-Senderprüfung und Betriebssystemrechten zu unterschiedlichen Auswirkungen.

**Status:** Optionale Präzisierung.

**Korrektur:** Explizit von einem möglichen Angriffspfad sprechen und zwischen XSS, Privilege Escalation, IPC-Missbrauch und RCE unterscheiden. Die Aussage in Zeile 53 ist dafür bereits eine gute Grundlage.

### 10. Lieferkettensicherheit wird nur behauptet

**Fundstelle:** `draft_2.2.3.md:61`

Die Aussage zu Electron, nativen Modulen, Build-Werkzeugen und Abhängigkeiten ist plausibel und relevant für FF4. Sie benötigt jedoch eine eigene Quelle oder eine klare Eingrenzung. Build-Werkzeuge sind nicht in gleicher Weise Laufzeitbestandteil wie Electron, Chromium, Node.js oder ausgelieferte Abhängigkeiten.

**Status:** Beleg- und Abgrenzungsbedarf.

**Korrektur:** Laufzeit- und Lieferkettensicherheit getrennt behandeln und die Fallstudie konkret auswerten: Electron-Version, native Module, Lockfile, Paketquellen, Updateverfahren und Buildprozess.

## Struktur und Übergänge

### Stärken

- `draft_2.2.2.md:29–33` begründet die Auswahl von Electron.
- `draft_2.2.3.md:3` übernimmt logisch die Sicherheitsverantwortung aus dieser Auswahl.
- Die Reihenfolge Vertrauensgrenzen → Renderer-Konfiguration → Preload → IPC → CSP → Angriffskette ist grundsätzlich schlüssig.
- Der Übergang zur Transformation in `draft_2.2.3.md:55–63` verbindet den theoretischen Abschnitt sinnvoll mit den späteren Implementierungskapiteln.
- Die Abgrenzung aus `draft_2.2.1.md:63–65`, wonach lokale Ausführung nicht automatisch Local First bedeutet, wird nicht verwischt.

### Verbesserungsbedarf

1. Die Planungsideen sehen eigene Abschnitte zu „Least Privilege und Dateisystemzugriff“ sowie „Sicherheit der Lieferkette“ vor (`ideen.md:102–114`). Im Entwurf sind beide Themen nur in kurzen Absätzen enthalten.
2. Dadurch wirken Dateisystemzugriffe und Lieferkette gegenüber Renderer, Preload und IPC untergewichtet.
3. Die Formulierung „serverseitige beziehungsweise mainseitige Validierung“ aus `ideen.md:81` wurde sinnvoll auf den Main-Prozess angepasst. „Serverseitig“ sollte im Electron-Kontext vollständig vermieden werden, wenn kein Server beteiligt ist.
4. Der Schluss in Zeile 63 ist inhaltlich stark, bündelt aber sehr viele Maßnahmen in einem einzigen Satz. Eine Aufteilung würde die argumentative Nachvollziehbarkeit erhöhen.

## Terminologie

### Inkonsistenzen oder Definitionsbedarf

- „Web-Code“, „Web-Inhalt“ und „geladene Inhalte“ sollten definiert und anschließend gezielt verwendet werden.
- „Renderer“, „Renderer-Prozess“ und „Renderer-Kontext“ sind nicht vollständig austauschbar.
- „Preload-API“, „Bridge“, „Capability-Schnittstelle“ und „IPC-Schnittstelle“ bezeichnen unterschiedliche Ebenen und sollten nicht synonym verwendet werden.
- „Capability“ sollte bei der ersten Verwendung definiert werden. „Capability-basierte Schnittstelle“ ist präziser als „Capability-Schnittstelle“.
- „Least Privilege“ sollte entweder als „Prinzip der geringsten Rechte“ eingeführt oder konsequent englisch verwendet werden.
- „Path Traversal“ sollte als „Pfadüberquerung (Path Traversal)“ einmal erklärt werden.
- „Remote Code Execution“ und „Cross-Site-Scripting“ sollten bei der ersten Verwendung ausgeschrieben und mit den Abkürzungen RCE beziehungsweise XSS eingeführt werden.
- „Vertrauensbasis“ in Zeile 21 und „Vertrauensgrenze“ in Zeile 61 sind nicht dasselbe. Die Begriffe sollten bewusst differenziert werden.

## Quellenanforderungen

**Beleg erforderlich:**

- `draft_2.2.3.md:7–11`: Electron-Prozessmodell und Rollen von Main, Renderer und Preload.
- `draft_2.2.3.md:15–19`: `nodeIntegration`, Context Isolation und Sandboxing.
- `draft_2.2.3.md:27`: Risiken der Weitergabe roher IPC-APIs.
- `draft_2.2.3.md:33–39`: IPC-Sicherheit, Eingabevalidierung und Senderprüfung.
- `draft_2.2.3.md:41–45`: CSP und ihre Grenzen.
- `draft_2.2.3.md:61`: Lieferketten- und Abhängigkeitssicherheit.

Die vorhandene einmalige Referenz in Zeile 3 reicht für den gesamten Abschnitt nicht aus. Die Nachbardrafts arbeiten ebenfalls teilweise mit unbelegten technischen Aussagen, insbesondere `draft_2.2.2.md:19–31`. Das sollte nicht als Begründung dienen, die Beleglage im Zielabschnitt unverändert zu übernehmen.

**Quellenreichweite prüfen:**

- `electron2026docs` muss tatsächlich alle behaupteten Details abdecken.
- Produktdokumentation eignet sich für Electron-Verhalten und Empfehlungen, aber nicht allein für allgemeine Aussagen über XSS, RCE, Least Privilege oder Race Conditions.
- Aussagen über das konkrete Transformationsszenario benötigen zusätzlich Primärquellen aus der Implementierung beziehungsweise DSR-Artefakte.

## Relevanz für das Transformationsszenario

Der Abschnitt ist für FF1, FF2 und insbesondere FF4 relevant. Besonders gut sind:

- die Ableitung einer neuen Sicherheitsarchitektur aus dem Web-Prototyp,
- die explizite Trennung von Renderer, Preload und Main,
- die Formulierung konkreter Transformationsregeln in `draft_2.2.3.md:57–61`,
- die Verbindung von Prozesssicherheit mit lokaler Datenhaltung.

Noch zu schwach ist die Rückbindung an die konkrete Studienplaner-Fallstudie. Vor der Überführung in die finale Fassung sollte angegeben werden:

- ob ausschließlich lokal gebündelte Vite-Inhalte geladen werden,
- welche konkreten IPC-Kanäle entstehen,
- welche Datenzugriffe auf SQLite erfolgen,
- welche Verzeichnisse für Datenbanken, Importe und Exporte erlaubt sind,
- ob externe Links, Remote-Inhalte oder automatische Updates vorgesehen sind,
- welche Abhängigkeiten tatsächlich privilegiert ausgeführt werden.

## Optionale Verbesserungen

- Einen eigenen Unterabschnitt zu „Least Privilege und Dateisystemzugriff“ aufnehmen.
- Einen kurzen, klar abgegrenzten Absatz zur Lieferkettensicherheit ergänzen.
- Die Angriffskette durch ein Diagramm oder eine kompakte Tabelle mit Sicherheitsbarrieren visualisieren.
- Im Schluss zwischen theoretischen Sicherheitsprinzipien und später nachzuweisenden Implementierungsentscheidungen unterscheiden.
- Die Verwendung von „automatisch“, „unmittelbar“ und „erheblich“ reduzieren oder jeweils technisch begründen.

**Gesamturteil:** Der Entwurf ist als theoretischer Rohentwurf fachlich tragfähig und strukturell gut angelegt. Vor der Übernahme in die wissenschaftliche Fassung sind jedoch die fehlende Electron-Quelle, die IPC-Senderprüfung, die Begriffstrennung und die konkrete Rückbindung an die Fallstudie zwingend zu korrigieren.
