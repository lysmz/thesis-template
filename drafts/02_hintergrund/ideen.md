# Kapitel 2 — Theoretischer Hintergrund (Gliederungsideen)

## 2.1 KI-generierte Software: Werkzeuge, Qualität und Grenzen

### 2.1.1 Überblick über KI-gestützte Entwicklungstools

Übersicht über die aktuelle Landschaft:
- **Replit** — Online-IDE mit KI-Agent; Fokus auf Full-Stack-Web-Prototyping, integrierte DB (Replit DB / PostgreSQL), Deployment in der Replit-Cloud. Stärke: schnelle, ausführbare Prototypen. Schwäche: Vendor-Lock-in, keine lokale Ausführung.
- **Lovable** — Spezialisiert auf Frontend-Generierung aus natürlicher Sprache und Screenshots; erzeugt React/Next.js-Apps, oft mit BaaS-Kopplung (Supabase). Stärke: schnelle UI-Generierung. Schwäche: hohe Cloud-Abhängigkeit, saubere Architektur zweitrangig.
- **Claude Artifacts / Cursor / GitHub Copilot** — Assistant-basierte Codegenerierung im Entwickler-Workflow; kein vollständiges Prototyping-Tool, sondern Ergänzung.
- **Figma Make** — Design-to-Code: UI-Designs aus Figma werden per KI in Komponenten-Code übersetzt (React/Vue/HTML+CSS). Stärke: pixelgenaue UI-Umsetzung, nahtlose Anbindung an bestehende Figma-Designsysteme. Schwäche: kein Backend, keine Geschäftslogik, statischer Output — Spezialisierung liegt klar auf UI/Frontend-Prototyping, nicht auf funktionsfähigen Full-Stack-Apps.
- **Weitere Nischen-Tools** — z.B. Bolt.new, V0 by Vercel.

Vergleichsdimensionen:
- Zielsetzung (Prototyping vs. Production-ready)
- Bevorzugter Tech-Stack (React, Next.js, Node, Python)
- Architektur-Charakter (stateless, Cloud-gebunden, monolithisch)
- Spezialisierung (Web-Apps, Demos, mobile Apps, Full-Stack, reines UI/Design-to-Code)

### 2.1.2 Charakteristika KI-generierten Codes

- Black-Box-Charakter: Code wird ohne architektonische Dokumentation oder Designentscheidungen generiert.
- Mangelnde Architektur: fehlende Separation of Concerns, hohe Kopplung, keine saubere Schichten-Trennung.
- Qualitätsdefizite nach ISO/IEC 25010: Wartbarkeit, kognitive Belastung, Kompatibilität, Sicherheit.
- Stand der Forschung: aktuelle Studien zur Code-Qualität von KI-generiertem Code (z.B. zu Bugs, Sicherheitslücken, Halluzinationen).
- Implikation für die Transformation: Code kann nicht "as-is" übernommen werden — systematisches Refactoring ist erforderlich.

## 2.2 Von der Web-Architektur zur Desktop-Anwendung

### 2.2.1 Charakteristika moderner Web-Architekturen

- Request-Response-Modell, stateless, Cloud-Backend als single source of truth.
- Client als dünne Präsentationsschicht (Browser).
- Abhängigkeiten: HTTP, REST/GraphQL, Cloud-DB (Firebase, Supabase, PostgreSQL), Authentifizierung über Drittanbieter.
- Problem: Diese Architektur ist für lokale Ausführung ungeeignet — keine Offline-Fähigkeit, keine lokale State-Verwaltung.

### 2.2.2 Desktop-Runtimes: Electron als Brückentechnologie

- Electron: Chromium + Node.js → Web-Technologien auf dem Desktop.
- Architektur: Main Process (Node.js) + Renderer Process (Chromium).
- Relevanz: Erlaubt die Wiederverwendung von Web-UI-Code im Desktop-Kontext.
- Alternativen: Tauri, NW.js, Flutter Desktop — Abgrenzung und Begründung für Electron.
- Problemzone: Web-Code läuft nicht einfach in Electron — Sicherheitsmodell, IPC, Dateisystemzugriff erfordern Anpassungen.

### 2.2.3 Sicherheitsmodell von Electron

#### Leitgedanke des Abschnitts

Der Abschnitt sollte Electron nicht als automatisch sichere Laufzeit darstellen. Electron stellt Sicherheitsmechanismen bereit, deren Wirksamkeit von der konkreten Konfiguration, der Gestaltung der IPC-Schnittstelle und der Vertrauenswürdigkeit des geladenen Inhalts abhängt. Die zentrale Frage lautet daher:

> Wie wird die privilegierte Desktop-Funktionalität so vom Web-Code getrennt, dass eine Kompromittierung des Renderers nicht unmittelbar zu einem Zugriff auf das Betriebssystem führt?

Der Abschnitt kann damit als Übergang von 2.2.2 zur späteren Sicherheitsanalyse dienen. 2.2.2 begründet die Wahl von Electron; 2.2.3 erklärt, welche Sicherheitsgrenzen Electron dafür anbietet und welche Verantwortung bei der Anwendung verbleibt.

#### Mögliche Gliederung

1. **Vertrauensgrenzen der Electron-Anwendung**
   - Der Renderer verarbeitet HTML, CSS und JavaScript und sollte grundsätzlich wie nicht privilegierter Web-Code behandelt werden.
   - Der Main-Prozess verfügt über weitreichende Rechte und muss deshalb als privilegierte Vertrauenszone betrachtet werden.
   - Preload-Skript und IPC bilden keine automatische Sicherheitsgarantie, sondern eine kontrollierte Vermittlungsschicht.
   - Electron besteht intern aus mehr als zwei Prozessen; für die Sicherheitsargumentation genügt die Unterscheidung zwischen privilegiertem Main-Prozess und unprivilegiertem Renderer.

2. **Sicherheitskonfiguration des Renderer-Prozesses**
   - `nodeIntegration: false`: Der Renderer erhält keinen direkten Zugriff auf Node.js-APIs.
   - `contextIsolation: true`: Der JavaScript-Kontext des Web-Inhalts wird vom Preload-Kontext getrennt.
   - `sandbox: true`: Die verfügbaren Betriebssystemrechte des Renderer-Prozesses werden zusätzlich begrenzt.
   - Navigation und externe Inhalte sollten auf erwartete Ursprünge beziehungsweise lokale Dateien beschränkt werden.
   - `webSecurity` sollte nicht deaktiviert werden; Entwicklungsbequemlichkeit darf nicht zum Sicherheitsprinzip werden.
   - Die Konfiguration sollte nicht nur genannt, sondern als Kombination von Schutzmaßnahmen erklärt werden. Keine einzelne Option verhindert alle Angriffe.

3. **Preload-Skript als minimale Capability-Schnittstelle**
   - Das Preload-Skript soll nicht das gesamte Node.js- oder Electron-API an den Renderer weiterreichen.
   - Über `contextBridge` werden nur fachlich erforderliche Funktionen veröffentlicht.
   - Die API sollte capabilities statt allgemeiner Systemzugriffe anbieten, beispielsweise `saveStudyPlan(plan)` statt eines beliebigen `writeFile(path, data)`.
   - Keine Weitergabe roher IPC-Funktionen wie `ipcRenderer.send` oder `ipcRenderer.invoke` an den Renderer.
   - Rückgabewerte und Fehler sollten kontrolliert und auf die Domäne der Anwendung begrenzt werden.
   - Das Preload-Skript ist Teil der privilegierten Vertrauensgrenze und muss deshalb ebenfalls geprüft werden.

4. **IPC als sicherheitsrelevanter API-Vertrag**
   - IPC-Kanäle sind Eingabepunkte in den privilegierten Main-Prozess und sollten wie öffentliche Backend-Endpunkte behandelt werden.
   - Jeder Kanal benötigt eine eindeutige fachliche Verantwortung und eine serverseitige beziehungsweise mainseitige Validierung.
   - Eingaben dürfen nicht allein deshalb als vertrauenswürdig gelten, weil sie aus dem eigenen Renderer stammen.
   - Relevante Prüfungen: Typen, erlaubte Wertebereiche, Dateipfade, Ressourcenbezug, Berechtigungen und erwarteter Aufruferkontext.
   - Fehler dürfen keine sensiblen Pfade, Stacktraces oder internen Daten an den Renderer weitergeben.
   - Die IPC-Schnittstelle sollte synchronisations- und transaktionsbewusst entworfen werden, damit Sicherheitsprüfungen nicht durch Race Conditions umgangen werden.

5. **Content Security Policy und nicht vertrauenswürdiger Inhalt**
   - Eine Content Security Policy (CSP) reduziert die Möglichkeiten von injiziertem JavaScript und erschwert bestimmte XSS-Angriffe.
   - CSP ist eine zusätzliche Verteidigungsschicht und ersetzt keine sichere Datenbehandlung oder Eingabevalidierung.
   - Inline-Skripte, unkontrollierte Remote-Inhalte und unnötige externe Ressourcen sollten vermieden werden.
   - Es muss entschieden werden, ob die Anwendung ausschließlich lokal gebündelte Inhalte lädt oder ob Remote-Inhalte notwendig sind. Letztere erhöhen die Vertrauens- und Updatekomplexität erheblich.
   - Remote-Inhalte sollten nicht mit privilegierten APIs vermischt werden.

6. **Typische Angriffskette als erklärendes Beispiel**
   - Ein XSS- oder Injection-Fehler im Renderer ermöglicht die Ausführung fremden JavaScript-Codes.
   - Bei aktivierter Node.js-Integration kann dieser Code unmittelbar auf privilegierte APIs zugreifen.
   - Bei einer zu mächtigen Preload- oder IPC-Schnittstelle kann der Angreifer indirekt Dateisystem-, Prozess- oder Netzwerkoperationen auslösen.
   - Eine unzureichende Pfadvalidierung kann zusätzlich Path Traversal ermöglichen.
   - Die Architektur soll deshalb die Angriffskette an mehreren Stellen unterbrechen: Renderer-Isolation, minimale API, Eingabevalidierung, Least Privilege und kontrollierte Dateisystemzugriffe.
   - Nicht jede XSS-Schwachstelle führt bei korrekter Electron-Konfiguration automatisch zu Remote Code Execution. Die konkrete Auswirkung hängt von den erreichbaren Schnittstellen und Rechten ab.

7. **Least Privilege und Dateisystemzugriff**
   - Der Main-Prozess sollte nur die Betriebssystemrechte erhalten, die für die konkrete Funktion erforderlich sind.
   - Renderer und IPC-Aufrufer sollten keine frei wählbaren Dateisystempfade kontrollieren dürfen.
   - Dateizugriffe sollten auf erlaubte Verzeichnisse, Dateitypen und Operationen begrenzt werden.
   - Datei-Dialoge und Pfadauflösung können genutzt werden, um Benutzerentscheidungen und technische Zugriffsbeschränkungen zu verbinden.
   - Schreib- und Leseoperationen sollten fachlich getrennt werden; ein allgemeiner Dateisystem-Wrapper wäre zu mächtig.

8. **Sicherheit der Lieferkette und der Anwendungspakete**
   - Electron-Sicherheit endet nicht bei der Prozesskonfiguration.
   - Abhängigkeiten, Build-Tools, native Module und automatisch aktualisierte Pakete erweitern die Vertrauensbasis.
   - Relevant sind veraltete oder kompromittierte Dependencies, unkontrollierte Postinstall-Skripte, fehlende Lockfiles und nicht überprüfte Updates.
   - Die Arbeit sollte zwischen Laufzeitsicherheit, Anwendungscode und Lieferkettensicherheit unterscheiden.
   - Für die Fallstudie könnte dokumentiert werden, welche Abhängigkeiten privilegiert ausgeführt werden und wie ihre Versionen beziehungsweise Herkunft geprüft werden.

#### Bezug zur Transformation

- Der ursprüngliche Web-Code darf nicht unverändert in den privilegierten Main-Prozess verschoben werden.
- Geschäftslogik, Datenzugriff und Betriebssystemintegration sollten getrennt betrachtet werden.
- Die Migration benötigt eine neue Sicherheitsarchitektur: Renderer für Darstellung und nicht privilegierte Interaktion, Preload für minimale Fähigkeiten, Main-Prozess für validierte privilegierte Operationen.
- Sicherheitsrelevante Entscheidungen sollten als Transformationsregeln formulierbar sein, beispielsweise: „Kein direkter Dateisystemzugriff aus dem Renderer“ oder „Jeder IPC-Eingang validiert strukturierte Daten“.
- Der Abschnitt sollte noch nicht die vollständige Umsetzung der Fallstudie vorwegnehmen. Er erklärt die Konzepte; konkrete Kanäle, Validatoren und Pfadregeln gehören in die späteren Entwurfs- und Implementierungskapitel.

#### Mögliche Kernthese

Electron bietet eine technische Grundlage für Isolation, erzwingt sie aber nicht. Sicherheit entsteht erst durch die Kombination aus abgeschalteter Node.js-Integration, Context Isolation, Sandboxing, minimaler Preload-API, validiertem IPC, CSP und konsequenter Rechtebegrenzung. Die entscheidende Transformationsleistung besteht deshalb nicht nur darin, Web-Code auf dem Desktop auszuführen, sondern eine explizite Vertrauensgrenze zwischen Web-Inhalt und Betriebssystem zu entwerfen.

#### Offene Punkte für Quellen und spätere Überarbeitung

- Electron-Dokumentation als Primärquelle für `contextIsolation`, `nodeIntegration`, Sandboxing, Preload und IPC verwenden.
- Prüfen, welche Sicherheitsoptionen in der konkret verwendeten Electron-Version standardmäßig aktiviert sind; nicht pauschal von „standardmäßig deaktiviert“ sprechen, ohne die Version zu nennen.
- CSP-Regeln anhand der tatsächlichen Build- und Deployment-Strategie der Anwendung formulieren.
- Prüfen, ob die Fallstudie lokale Dateien, Remote-Inhalte, externe Links oder automatische Updates verwendet.
- Die Begriffe XSS, RCE, IPC, Preload und Capability einmal definieren und anschließend konsistent verwenden.
- Sicherheitsmaßnahmen nicht als vollständig oder absolut wirksam darstellen; verbleibende Risiken und Fehlkonfigurationen ausdrücklich benennen.

## 2.3 Sicherheit lokaler Desktop-Anwendungen

> 2.2.3 erklärt das technische Sicherheitsmodell von Electron. 2.3 analysiert darauf aufbauend Bedrohungen, KI-spezifische Risiken und Anforderungen an das Zielartefakt. Die Electron-Mechanismen werden hier nicht erneut als Grundlagenkapitel beschrieben.

### 2.3.1 Bedrohungsmodell und Angriffsflächen

- Schutzobjekte: lokale Studienplandaten, SQLite-Datenbank, Konfiguration, Zugangsdaten und Anwendungsintegrität.
- Angriffsflächen: Renderer, Preload, IPC, Dateisystem, externe Links, Netzwerkzugriffe, native Module sowie Paketierungs- und Updateprozesse.
- Andere Vertrauensannahmen als in Web-Apps: Daten, Code und Laufzeit befinden sich teilweise auf dem Endgerät.
- Angriffskette als Anwendungsszenario: manipulierte Renderer-Eingabe → zu weitreichende Preload-/IPC-Schnittstelle → privilegierte Dateisystem- oder Prozessoperation.

### 2.3.2 Sicherheitsrisiken KI-generierter Anwendungen

- Fehlende Eingabevalidierung, überprivilegierte Hilfsfunktionen und unsichere Standardkonfigurationen.
- Fehlende Trennung von Präsentation, Geschäftslogik und Infrastruktur.
- Hardcodierte Geheimnisse, unzureichende Fehlerbehandlung und fehlende Zugriffskontrollen.
- Unsichere oder halluzinierte Abhängigkeiten als Risiko für die Softwarelieferkette.
- Einordnung anhand relevanter Kategorien wie Injection, Broken Access Control und Security Misconfiguration.

### 2.3.3 Sicherheitsanforderungen an das Zielartefakt

- Kein direkter Dateisystem-, Native- oder allgemeiner Node.js-Zugriff aus dem Renderer.
- Minimale, fachlich definierte Schnittstellen für privilegierte Operationen.
- Erneute Validierung jeder Renderer-Eingabe vor privilegierten Operationen.
- Begrenzung von Dateipfaden, Dateitypen und erlaubten Operationen.
- Keine sensiblen Daten in Quellcode, Bundles oder Fehlermeldungen.
- Nachvollziehbare und reproduzierbare Abhängigkeiten.
- Kontrollierte Fehlerbehandlung ohne Offenlegung interner Pfade oder Stacktraces.
- Überprüfbarkeit durch statische Analysen, Konfigurationskontrollen und Tests.

## 2.4 Das Local-First-Paradigma

### 2.4.1 Kernprinzipien nach Kleppmann et al.

- Die sieben Ideale des Local-First: Offline-First, Kollaboration, Datensouveränität, etc.
- Abgrenzung zu Cloud-Native: lokale Datenhaltung als Primärquelle, Cloud optional.
- Relevante Techniken: CRDTs, Operational Transformation, Merge-Konzepte.

### 2.4.2 Lokale Datenhaltung und Offline-First

- SQLite als lokale Datenbank: Vorteile (zero-config, embedded, ACID, gut getestet).
- Schema-Migrationen: Notwendigkeit bei komplexen relationalen Daten.
- Synchronisationsstrategien (falls Cloud-Anbindung später gewünscht).

### 2.4.3 Abgrenzung zu Cloud-Native und traditionellen Desktop-Apps

- Cloud-Native: Server-zentriert, Browser-Client, immer online.
- Traditionelle Desktop-Apps (z.B. WinForms, WPF): nativer Code, keine Web-Technologien.
- Local-First als Hybrid: Web-UI + lokale Datenhaltung + optionale Synchronisation.

### 2.4.4 Relevanz für das Transformationsszenario

- Local-First schließt automatisch Eigenschaften ein, die für viele Leser einzeln relevant sein können (Offline, Datensouveränität, Performance).
- Durch die Umsetzung als Gesamtpaket steigt der Wert der Arbeit — jeder Leser mit spezifischen Teilproblemen profitiert.
- Local-First adressiert die Defizite der Cloud-Web-Architektur (Latenz, Verfügbarkeit, Datenschutz).

## 2.5 Zusammenfassung und Forschungslücke

### 2.5.1 Synthese der Grundlagen

- Die vier Themenfelder (KI-Code, Web→Desktop, Local-First, Sicherheit) sind in der Literatur einzeln behandelt.
- Es existiert jedoch keine Arbeit, die diese Felder für das spezifische Transformationsszenario KI-Web-App → Local-First-Desktop integriert.

### 2.5.2 Identifikation der Forschungslücke

- Bestehende Arbeiten behandeln entweder KI-Code-Qualität, Local-First oder Electron-Sicherheit isoliert.
- Kein systematischer Migrationspfad existiert, der:
  - KI-generierten Web-Code analysiert
  - strukturiert refaktoriert
  - in eine Local-First-Electron-Architektur überführt
  - und dabei die spezifischen Sicherheitsrisiken adressiert.
- Diese Lücke adressiert die vorliegende Arbeit mit einem framework-agnostischen Transformationsansatz, validiert an einer konkreten Fallstudie.
