# Kapitel 4 — Analyse und Problemanalyse (Gliederungsideen)

## Leitgedanke des Kapitels

Kapitel 4 setzt die ersten beiden Aktivitäten des DSRM-Prozesses aus Kapitel 3 um: die **Problemidentifikation** (Aktivität 1) und die **Zieldefinition** (Aktivität 2). Am Ende des Kapitels steht deshalb ein konkreter **Anforderungskatalog** an das zu entwickelnde Artefakt, der in Kapitel 6 als Bewertungsmaßstab wiederverwendet wird.

Die Struktur folgt der Bewegung von **Breite** zu **Tiefe**. In der Breite wird die Landschaft der Vibe-Coding-Plattformen vermessen und geklärt, wie weit ein Migrationspfad überhaupt reichen kann. In der Tiefe wird der konkrete Fall — Replit und das dort erzeugte Repo — präzise analysiert. Die Breite begründet also, *warum* der Blick auf einen konkreten Ausgangspunkt eingeengt wird; die Tiefe liefert die eigentliche Substanz.

Abgrenzung zu Kapitel 2 bleibt wichtig: Kapitel 2 beschreibt Konzepte neutral (Tools, Electron, Local-First, Sicherheit). Kapitel 4 **vergleicht und entscheidet** — es bewertet die Plattformen gegeneinander und analysiert das gewählte Artefakt im Detail.

## 4.1 Breite — Vibe-Coding-Plattformen im Vergleich und Reichweite des Migrationspfads

### 4.1.1 Unterschiede der Vibe-Coding-Plattformen

- Wie unterscheiden sich die Plattformen voneinander? Fokussierte Auswahl von drei Plattformen — Replit, Lovable, Figma Make — die je einen Plattformtyp repräsentieren (Code-Agenten werden bewusst erst in 4.2.1 eingeführt).
- Zentrale Unterscheidungsdimensionen:
  - **Full-Stack vs. Frontend-only:** Erzeugt die Plattform ein eigenes Backend oder nur die UI?
  - **Eigenes Backend vs. BaaS:** Läuft die Logik als eigener Code oder als externer Dienst (z. B. Supabase)?
  - **Code-Ownership/Export:** Bekommt man echten, exportierbaren Code oder bleibt man in der Sandbox gefangen (Vendor-Lock-in)?
  - **Deployment-Modell:** Wo läuft das Ergebnis — in der Cloud des Anbieters oder frei übertragbar?
  - **Grad der „echten" Code-Erzeugung:** Full-App-Generierung vs. Assistenz im Entwickler-Workflow vs Nur Frontend. 
- Erste grobe Einordnung:
  - **Replit** — Full-Stack mit eigenem Backend und Frontend; erzeugt eine lauffähige Client-Server-App.
  - **Lovable + BaaS** — Frontend-Generierung mit externem Backend; Backend bleibt außerhalb des eigenen Codes.
  - **Figma Make** — reines Design-to-Code; nur UI, keine Logik, kein Backend.
  - Die Code-Agenten (Cursor, Copilot, Claude Code/Artifacts) werden in 4.1 bewusst **nicht** verglichen; sie dienen in 4.2.1 als zweiter Vergleichspol für die Begründung der Replit-Wahl.

### 4.1.2 Gemeinsamkeiten der Plattformen

- Wo sind die Plattformen ähnlich? Über die Unterschiede hinweg teilen sie eine gemeinsame Charakteristik.
- Natürlichsprachliche Eingabe (Prompting), sehr schnelle Prototypenerstellung, Fokus auf sichtbare Funktionen statt auf Architektur.
- Cloudgebundenheit, Black-Box-Charakter des Codes, fehlende Trennung der Belange.
- Genau diese Gemeinsamkeiten definieren die **Problemklasse**, auf die der Migrationspfad abzielt: „laienhaft beauftragter, KI-generierter Web-Prototyp".

### 4.1.3 Warum ein universeller Migrationspfad für alle Plattformen nicht möglich ist

- Warum lässt sich nicht einfach „für alle" Plattformen eine App bauen und migrieren?
- Jede Plattform erzeugt einen anderen Output: Full-Stack vs. Frontend-only vs. reine UI, jeweils mit eigenem Tech-Stack und eigener Cloud-Kopplung.
- Ein konkreter Migrationspfad setzt ein konkretes Ausgangsartefakt voraus — insbesondere ein **eigenes Backend**, das in einen lokalen Prozess überführt werden kann. Fertige Backends (Supabse) benötigen einen anderen Ansatz wegen ihrem Black-Box-Charakter und müssten gesondert betrachtet werden.
- Platformen generieren verschiedene Artefakte, besonders auf Wunsch, das erhöhrt die möglichen Migrationspfade und ist in einer Bachelorarbeit nicht, im Detail, für alle Tools überschaubar
- Was sich verallgemeinern lässt, ist die **Methodik** (der Ansatz, die Schrittfolge, die Sicherheitsprinzipien). Die **konkreten Transformationsschritte und Refactoring-Patterns** sind an den jeweiligen Stack gebunden.
- Daraus folgt die bewusste Eingrenzung auf einen repräsentativen Ausgangspunkt, an dem der Migrationspfad konkret demonstriert wird.

### 4.1.4 Reichweite des Migrationspfads: Welches Frontend-Framework?

- Für welches Frontend-Framework lässt sich der Migrationspfad **genau** anwenden? (React — das Framework des konkreten Falls.)
- Die Framework-Agnostik aus FF1 muss ehrlich eingegrenzt werden:
  - Die **Methodik** ist framework-agnostisch (die Vorgehensweise, Prinzipien und Schritte gelten allgemein).
  - Die **konkreten Refactoring-Patterns** sind framework-spezifisch und werden an React demonstriert.
  - Für ein anderes Framework wie **Svelte, oder Vue** müssten diese Patterns angepasst werden (anderes Komponentenmodell, andere Reaktivität, kein React-Äquivalent für manche Muster).
- **Aufwand und Zeit** als Vergleichsdimension: Was kostet die Übertragung des Migrationspfads auf ein anderes Framework, und wo liegt die sinnvolle Grenze der Verallgemeinerung?
- Das schärft FF1: „framework-agnostisch" bedeutet nicht „für jedes Framework fertig", sondern „die Methode ist übertragbar, die Implementierung nicht".

## 4.2 Tiefe — Replit und das konkrete Ausgangsartefakt

### 4.2.1 Warum Replit die geeignetste Plattform ist

- Replit als **Kompromiss** zwischen zwei Plattformtypen:
  - Vibe-Coding-Plattform: Man beschreibt die App in natürlicher Sprache und erhält eine vollständige Anwendung.
  - Code-Agent-Plattform (Claude Code etc.) : Man bekommt eine Vollständige App, muss sich aber technisch damit auseiander setzen, API-Key, Architektur, kein Fertiger Tech-Stack im Hintergrund gesetzt. 
- Der entscheidende Punkt: Replit erzeugt ein **eigenes Backend und Frontend**, also eine echte Full-Stack-Anwendung mit realem Code, ohne fertig Backend.
- Gegenüber reinen Frontend-Generatoren (Lovable/BaaS) hat Replit eine migrierbare Backend-Schicht, die in einen lokalen Prozess (Electron Main) überführt werden kann.
- Gegenüber reinen Code-Agenten liefert Replit den vollständigen Full-Stack-Kontext, bei gleichbleibender Komplexität, der anderen Vibe-Coding-Tools, für den Nutzer.
- Deshalb ist Replit der geeignetste Ausgangspunkt für die Desktop-Transformation — und genau deshalb wird es als Fallstudie gewählt.

### 4.2.2 Analyse des erstellten Repos (Ist-Zustand)
- Entstehungsprozess & Prompting-Strategie der Demonstrator-App (Studienplaner).
- Das tatsächlich erzeugte Replit-Repo präzise analysieren — dies ist die Faktenbasis des Kapitels.
- Technische Bestandsaufnahme: Stack (React-Frontend, Express-Backend), Struktur, Deployment.
- Code-Beschaffenheit: Kopplung, fehlende Trennung der Belange, Black-Box-Charakter.
- Cloud-Abhängigkeiten identifizieren: Datenbank, API-Layer, ggf. Auth — jede Abhängigkeit ist später ein Migrationspunkt.
- Datenmodell und Funktionsumfang erfassen: Sie definieren, was die Transformation erhalten muss.
- Konkrete Befunde stammen aus der Code-Analyse und sind in `.agents/artifacts/` dokumentiert.
- Wie bekommt man das ganze Projekt lokal zum laufen, auserhalb der Online-Platform?

### 4.2.3 Anforderungen und Probleme (Ableitung des Anforderungskatalogs)

- Aus der Repo-Analyse die Anforderungen und Probleme ableiten — das ist die Zieldefinition (DSRM-Aktivität 2).
- **Funktionale Anforderungen:** Der Funktionsumfang des Studienplaners bleibt nach der Transformation vollständig erhalten.
- **Local-First-Anforderungen (FF1, FF3):** Offline-Fähigkeit, lokale SQLite-Datenhaltung, Datensouveränität, automatisierte Schema-Migrationen.
- **Sicherheitsanforderungen (FF4):** Rückgriff auf Kapitel 2.3.3 — kein direkter Dateisystem-/Node-Zugriff aus dem Renderer, minimale IPC-Schnittstellen, Validierung, Least Privilege.
- **Qualitätsanforderungen (FF2):** Trennung der Belange, Reduktion der Kopplung, Wartbarkeit.
- Jede Anforderung einer Forschungsfrage zuordnen (Rückverfolgbarkeit).
- Ergebnis ist ein strukturierter **Anforderungskatalog** (funktional / nicht-funktional, je mit Quelle und Priorität) — zugleich der Bewertungsmaßstab für Kapitel 6.

## Festgelegte Entscheidungen (Stand: 2026-08-24)

- **Umfang des Plattform-Vergleichs:** 3 Plattformen — Replit, Lovable, Figma Make — je ein Typ, dargestellt in einer Vergleichstabelle. Der Code-Agent-Pol (Cursor/Copilot/Claude Code) wird **nur in 4.2.1** eingeführt, als zweite Richtung des Replit-Kompromisses.
- **Framing der Framework-Agnostik:** Festgelegt — die **Methodik** ist übertragbar, die **konkreten Refactoring-Patterns** sind framework-spezifisch (Demonstration an React). Beleg: Svelte/Vue-Beispiel plus „Aufwand" als Vergleichsdimension. FF1 bleibt ehrlich: „Methode übertragbar, Implementierung nicht".
- **Abgrenzung zu Kapitel 2:** Expliziter Rahmensatz plus **migrationsrelevante Dimensionen** (Full-Stack vs. Frontend, eigenes Backend vs. BaaS, Code-Ownership, Deployment) als das Neue gegenüber der neutralen Tool-Beschreibung in 2.1.1.
- **Tiefe der Repo-Analyse:** Architekturebene in Kap. 4 (Struktur, Kopplung, Cloud-Abhängigkeiten, Datenmodell, Funktionsumfang). Codebeispiele gehören nach Kap. 5.
- **Anforderungskatalog:** Formale Tabelle mit IDs (`FA-01`, `LF-01`, `SEC-01`, `Q-01`), je mit Quelle, FF-Zuordnung und Priorität — als referenzierbarer Bewertungsmaßstab für Kap. 6.
- **Quellenbasis:** In `.agents/artifacts/` liegt noch **kein** Repo-Analyse-Artefakt vor (nur 001/002, beides Meta). Die Ist-Zustand-Analyse des Replit-Repos muss **zuerst** als DSR-Artefakt dokumentiert werden, bevor 4.2.2/4.2.3 geschrieben werden.
