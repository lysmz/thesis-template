# Einleitung — Entwurf

## Motivation und Relevanz

Viele Menschen möchten Anwendungen für den persönlichen Gebrauch erstellen, verfügen jedoch über keine Programmiererfahrung.
Durch KI-gestützte Werkzeuge wie Replit, Lovable oder Claude-Artifacts ist es ihnen heute möglich, innerhalb kurzer Zeit funktionsfähige Anwendungen zu erzeugen, ohne eine einzige Zeile Code selbst zu schreiben oder zu verstehen.
Diese Anwendungen nutzen die Erstellenden im Vertrauen darauf, sie seien sicher und produktionsreif.
Tatsächlich betrachten sie den erzeugten Code jedoch nie; sie beurteilen die Anwendung ausschließlich anhand ihrer sichtbaren Funktionen.
Der Quellcode bleibt für sie eine Blackbox, deren Qualität und Sicherheit sie nicht einschätzen können.

Diese Entwicklung ist deshalb relevant, weil derart selbst erstellte Anwendungen zunehmend nicht mehr nur in der abgeschirmten Sandbox des Browsers laufen, sondern als lokale Desktop-Anwendungen mit direktem Zugriff auf Dateisystem und Systemressourcen.
Eine Anwendung, die oberflächlich funktioniert, aber unbemerkt unsicher oder architektonisch mangelhaft ist, entfaltet in einer solchen lokalen Laufzeitumgebung ein ungleich höheres Schadenspotenzial als im Browser.
Genau an dieser Stelle setzt die vorliegende Arbeit an.

## Problemstellung

Von technischen Laien instruierte generative KI-Modelle fokussieren sich nahezu ausschließlich auf die Umsetzung der gewünschten Funktionen.
Fundamentale Qualitätsmerkmale des Software Engineerings nach ISO/IEC 25010 — Wartbarkeit, Trennung der Belange, Interoperabilität und Sicherheit — bleiben dabei regelmäßig unberücksichtigt.
Der entstehende Code ist häufig unstrukturiert und stark gekoppelt, erschwert dadurch spätere Wartung und Erweiterung und implementiert selten adäquate Sicherheitsmechanismen gegen gängige Schwachstellen, etwa aus den OWASP Top 10.
Da die Erstellenden nur die Funktionen, nicht aber den Code beurteilen, bleibt diese Diskrepanz für sie unsichtbar: Die Anwendung wirkt einsatzbereit, obwohl sie es nicht ist.

Besonders kritisch wird diese Diskrepanz, sobald eine solche Webanwendung in eine lokale Desktop-Umgebung überführt werden soll.
Die Zielumgebung stellt völlig andere Anforderungen als die isolierte Sandbox des KI-Werkzeugs, und die vom Werkzeug getroffenen Architekturentscheidungen lassen sich nicht unverändert übernehmen.
Technisch wäre es ohne Weiteres möglich, eine Webanwendung nur oberflächlich zu verpacken, etwa indem im Main-Prozess einer Electron-Anwendung eine lokale Web-API betrieben und das Frontend unverändert im Renderer ausgeliefert wird.
Ein solcher Ansatz lässt die Anwendung zwar auf dem Desktop laufen, reproduziert aber lediglich die client-server-basierte Webarchitektur samt ihrer Cloud-Kopplung und Sicherheitsfläche, anstatt sie in eine echte Local-First-Architektur zu überführen.
Ziel der Arbeit ist deshalb nicht die bloße Ausführbarkeit auf dem Desktop, sondern eine sichere, architektonisch saubere Transformation.
Für genau diese Klasse von Anwendungen fehlt bislang ein systematisches Vorgehen, um eine von Laien in Auftrag gegebene, KI-generierte Webanwendung sicher in eine lokale Desktop-Anwendung zu transformieren.

## Fallstudie und Zielsetzung

Um diese Lücke zu schließen, entwickelt die vorliegende Arbeit einen methodischen Migrationspfad für die Überführung KI-generierter Webanwendungen in sichere Local-First-Desktop-Anwendungen.
Validierungsobjekt ist eine konkrete Fallstudie: der Studienplaner, eine deutschsprachige Studienverwaltungsanwendung, die als Replit-Webprototyp vorliegt und in eine Electron-Anwendung überführt wird.
Entscheidend ist dabei, wie dieser Prototyp entstanden ist: Er wurde bewusst so in Auftrag gegeben, wie es eine Person ohne technischen Hintergrund tun würde.
Die verwendeten Prompts beschreiben ausschließlich das gewünschte Ergebnis — etwa „ich möchte einen Stundenplan" — und machen keine Vorgaben zur Implementierung, etwa zur Wahl eines bestimmten UI-Frameworks.
Der Studienplaner ist damit keine von einem erfahrenen Entwickler gestaltete Anwendung, sondern eine authentisch laienhaft erzeugte, und repräsentiert so genau die Klasse von Artefakten, auf die der Migrationspfad abzielt.

Die Wahl des Local-First-Paradigmas als Zielarchitektur ist dabei nicht beliebig.
Local-First bündelt mehrere für die Migration KI-generierter Software zentrale Eigenschaften: Offline-Fähigkeit, Datensouveränität und eine vom Netzwerk entkoppelte, dauerhaft verfügbare Nutzung.
Indem die Arbeit das vollständige Local-First-Paket adressiert, deckt sie die relevanten Teilprobleme gemeinsam ab und erhöht den praktischen Nutzen für ein breiteres Spektrum an Anwendungsfällen.

## Forschungsfragen

1. **FF1:** Wie können KI-generierte Webarchitekturen mithilfe eines framework-agnostischen Ansatzes systematisch in sichere Local-First-Desktop-Anwendungen überführt werden?
2. **FF2:** Welche konkreten Refactoring-Muster sind erforderlich, um KI-generierten UI- und Logikcode für den sicheren Betrieb in einer lokalen Desktop-Umgebung zu qualifizieren?
3. **FF3:** Wie kann eine cloudabhängige Datenbankspeicherung durch eine robuste lokale SQLite-Datenhaltung einschließlich automatisierter Schema-Migrationen für komplexe relationale Daten ersetzt werden?
4. **FF4:** Welche Sicherheitsrisiken entstehen durch den Einsatz KI-generierten Codes in lokalen Desktop-Anwendungen, und wie lassen sich diese durch architektonische Härtungsmaßnahmen wirksam minimieren?

## Aufbau der Arbeit

Der weitere Aufbau der Arbeit orientiert sich am Design-Science-Research-Prozess und gliedert sich in sieben weitere Kapitel.
Kapitel 2 legt den theoretischen Hintergrund, indem es KI-gestützte Entwicklungswerkzeuge, die Besonderheiten von Web-zu-Desktop-Architekturen, das Local-First-Paradigma sowie sicherheitsrelevante Grundlagen einordnet und die bestehende Forschungslücke herausarbeitet.
Kapitel 3 stellt den methodischen Rahmen der Arbeit vor, begründet die Wahl des Design-Science-Research-Ansatzes und skizziert den eigenen Forschungsprozess.
Kapitel 4 widmet sich der Analyse und Problemanalyse: Zunächst wird die Softwarelandschaft KI-gestützter Entwicklungswerkzeuge in der Breite betrachtet, etwa Ansätze wie Replit oder Lovable, bevor in die Tiefe des gewählten Ansatzes, der Replit-Umgebung und des daraus resultierenden Artefakts, eingestiegen wird.
Kapitel 5 behandelt Entwurf und Entwicklung des Artefakts, also die konzeptionelle Architektur und die praktische Umsetzung der Electron-Anwendung.
Kapitel 6 stellt die Evaluation und Demonstration des entwickelten Migrationspfads anhand der Fallstudie in den Mittelpunkt.
Kapitel 7 diskutiert die gewonnenen Erkenntnisse, reflektiert die verfolgten Lösungswege und ordnet die Ergebnisse im Vergleich zu bestehenden Ansätzen ein.
Kapitel 8 fasst die zentralen Erkenntnisse zusammen und gibt einen Ausblick auf offene Fragestellungen und zukünftigen Forschungsbedarf.
