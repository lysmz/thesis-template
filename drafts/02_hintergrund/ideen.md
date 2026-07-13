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

- Context Isolation: Trennung von Preload-Skript und Renderer.
- Preload-Skripte: definierte IPC-Schnittstelle zwischen Main und Renderer.
- Sandboxing: Einschränkung des Renderer-Prozesses.
- Node-Integration: standardmäßig deaktiviert — bewusste Entscheidung gegen Web-Entwicklungs-Gewohnheiten.
- Relevanz für die Transformation: Web-Code, der naiv in Electron läuft, umgeht typischerweise alle Sicherheitsmechanismen.

## 2.3 Das Local-First-Paradigma

### 2.3.1 Kernprinzipien nach Kleppmann et al.

- Die sieben Ideale des Local-First: Offline-First, Kollaboration, Datensouveränität, etc.
- Abgrenzung zu Cloud-Native: lokale Datenhaltung als Primärquelle, Cloud optional.
- Relevante Techniken: CRDTs, Operational Transformation, Merge-Konzepte.

### 2.3.2 Lokale Datenhaltung und Offline-First

- SQLite als lokale Datenbank: Vorteile (zero-config, embedded, ACID, gut getestet).
- Schema-Migrationen: Notwendigkeit bei komplexen relationalen Daten.
- Synchronisationsstrategien (falls Cloud-Anbindung später gewünscht).

### 2.3.3 Abgrenzung zu Cloud-Native und traditionellen Desktop-Apps

- Cloud-Native: Server-zentriert, Browser-Client, immer online.
- Traditionelle Desktop-Apps (z.B. WinForms, WPF): nativer Code, keine Web-Technologien.
- Local-First als Hybrid: Web-UI + lokale Datenhaltung + optionale Synchronisation.

### 2.3.4 Relevanz für das Transformationsszenario

- Local-First schließt automatisch Eigenschaften ein, die für viele Leser einzeln relevant sein können (Offline, Datensouveränität, Performance).
- Durch die Umsetzung als Gesamtpaket steigt der Wert der Arbeit — jeder Leser mit spezifischen Teilproblemen profitiert.
- Local-First adressiert die Defizite der Cloud-Web-Architektur (Latenz, Verfügbarkeit, Datenschutz).

## 2.4 Sicherheit in lokalen Desktop-Anwendungen

### 2.4.1 Threat-Modelle für lokal ausgeführte Anwendungen

- Andere Bedrohungen als in Web-Apps: lokale Dateisystem-Zugriffe, ausführbarer Code, Malware-Vektor durch Electron.
- Relevante Angriffsszenarien: XSS im Renderer → RCE im Main Process, Path-Traversal, Injection.

### 2.4.2 Relevante OWASP-Kategorien für KI-Code

- Welche OWASP Top 10 Risiken sind bei KI-Code besonders wahrscheinlich? (Injection, Broken Access Control, Security Misconfiguration).
- Besonderheit: KI-Code neigt zu hard-coded Secrets, unsicheren Dependencies, fehlender Input-Validierung.

### 2.4.3 Härtungsmechanismen

- Content Security Policy (CSP) für Electron.
- Context Isolation + Preload + IPC als Sicherheitsbarriere.
- Least-Privilege-Prinzip für Dateisystemzugriffe.
- Vermeidung von `nodeIntegration: true` und `contextIsolation: false`.

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
