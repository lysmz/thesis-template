# Kapitel 3 — Methodisches Vorgehen (Gliederungsideen)

## Leitgedanke des Kapitels

Das Kapitel legt das methodische Fundament der Arbeit und soll zwei Begriffe sauber voneinander trennen, die im Titel leicht verschwimmen:

- Die **Forschungsmethodik** der Arbeit — das Design Science Research (DSR), nach dem die Arbeit selbst vorgeht.
- Das **Artefakt** der Arbeit — der framework-agnostische Transformationsansatz („Migrationspfad“), der das Ergebnis der Forschung ist und in Kapitel 5 beschrieben wird.

Kapitel 3 beschreibt ausschließlich Ersteres: den methodischen Rahmen, seine Begründung und den konkreten Forschungsprozess. Diese Trennung sollte früh explizit gemacht werden, damit der Leser „Methodik“ (Forschung) nicht mit „Transformationsmethodik“ (Artefakt) verwechselt.

Der rote Faden: Die Forschungsfragen sind gestaltungsorientiert („Wie kann … überführt werden?“), nicht beschreibend. Dafür ist DSR der passende Rahmen. Konkret wird der DSRM-Prozess nach Peffers et al. verwendet, dessen sechs Aktivitäten eins zu eins auf die Kapitel 4–8 abgebildet werden.

## 3.1 Design Science Research als methodischer Rahmen

### 3.1.1 Grundidee und Charakteristika von DSR < schauen ob wir das wirklich brauchen.>

- DSR ist ein Forschungs- und Problemlösungsparadigma, das darauf abzielt, **praktische Probleme durch die Schaffung und Evaluation eines IT-Artefakts** zu lösen und dabei zugleich einen wissenschaftlichen Erkenntnisbeitrag zu liefern.
- Abgrenzung zur verhaltenswissenschaftlichen (deskriptiven) Forschung: Diese beschreibt und erklärt Phänomene; DSR ist **präskriptiv** und gestaltet Lösungen.
- Zentrale Begriffe: **Relevanz** (Beitrag zur Lösung eines realen Problems) und **Rigor** (methodische und wissenschaftliche Strenge) — beide müssen im Gleichgewicht stehen.
- Artefakttypen nach March & Smith: Konstrukte, Modelle, Methoden, Instanziierungen. Die Arbeit erzeugt primär eine **Methode** (den Migrationspfad) und sekundär eine **Instanziierung** (die Electron-Anwendung).
- Warum DSR hier passt: Der Erkenntnisbeitrag der Arbeit ist kein Naturgesetz und keine empirische Beschreibung, sondern eine wiederverwendbare Vorgehensweise samt validiertem Anwendungsbeispiel.

### 3.1.2 Einordnung nach Hevner: Relevanz, Rigor und die drei Zyklen

- Hevner et al. (2004) verankern DSR in der Wirtschaftsinformatik und formulieren sieben Guidelines; zentral ist dabei das Spannungsfeld zwischen **Relevanz** (Beitrag zur Lösung eines realen Problems) und **Rigor** (wissenschaftliche Strenge).
- Hevner (2007) modelliert DSR als **drei Zyklen**:
  - **Relevance Cycle** — verbindet die Forschung mit dem Anwendungsumfeld (reales Problem, Anforderungen).
  - **Rigor Cycle** — verbindet die Forschung mit der Wissensbasis (bestehende Theorien, Methoden, Artefakte).
  - **Design Cycle** — der innere Kern aus Bauen und Evaluieren des Artefakts.
- Rolle in dieser Arbeit: Hevner liefert die konzeptionelle Einordnung, **warum** sowohl das Praxisproblem (Relevanz) als auch die methodische Strenge (Rigor) zentral sind; Peffers liefert die konkrete Prozessschrittfolge. Hevner wird daher nur knapp als Rahmung genutzt, nicht als zweiter vollständiger Prozess.

### 3.1.3 Der DSRM-Prozess nach Peffers et al. < weiß nicht ob das gebraucht wird.>

- Peffers et al. operationalisieren DSR als Abfolge von sechs Aktivitäten:
  1. **Problemidentifikation und Motivation**
  2. **Definition der Ziele einer Lösung**
  3. **Design und Entwicklung** (des Artefakts)
  4. **Demonstration** (Anwendung des Artefakts auf eine Instanz des Problems)
  5. **Evaluation** (Messung, wie gut das Artefakt die Ziele erreicht)
  6. **Kommunikation** (Problem, Artefakt, Nutzen und Rigor an Fachwelt und Praxis)
- Der Prozess ist **iterativ**: Nach Demonstration oder Evaluation kann in frühere Aktivitäten zurückgesprungen werden (z. B. Design überarbeiten).
- Vier mögliche **Einstiegspunkte** (problem-, ziel-, design-/entwicklungs- und kontextzentriert). Die Arbeit nutzt den **problemzentrierten Einstieg**: Ausgangspunkt ist das identifizierte Problem der laienhaft KI-generierten Web-Prototypen.
- Ggf. kompakte Prozessdarstellung als Abbildung (`.drawio.svg`), die die sechs Aktivitäten mit Rückkopplungspfeilen zeigt.

## 3.2 Begründung der Methodenwahl

- **Warum DSR und nicht ein deskriptiver Ansatz?**
  - Alle vier Forschungsfragen (FF1–FF4) sind „Wie“-Fragen, die auf eine gestaltbare Lösung zielen, nicht auf die Beschreibung eines Ist-Zustands.
  - Der Erkenntnisbeitrag ist ein **Artefakt** (Migrationspfad) plus dessen Validierung — genau der Gegenstand von DSR.
  - Ein rein empirischer oder qualitativer Ansatz (z. B. Befragung, Literaturanalyse) könnte das Problem beschreiben, aber nicht die geforderte Transformationsmethodik hervorbringen und belegen.
- **Abgrenzung zu verwandten Ansätzen**
  - *Action Research*: zielt auf organisatorische Veränderung und Intervention, nicht auf ein übertragbares Design-Artefakt als Erkenntnisbeitrag.
  - *Fallstudie (Case Study)*: eine Forschungsstrategie zur Untersuchung eines Phänomens im Kontext; allein erzeugt sie kein Design-Artefakt. In dieser Arbeit dient die Fallstudie als **Demonstrations- und Evaluationsvehikel** innerhalb von DSR, nicht als eigenständige Methodik.
  - *Konstruktiver Forschungsansatz (Constructive Research)*: nah an DSR, aber primär betriebswirtschaftlich geprägt; DSR ist in der Wirtschaftsinformatik/Informatik der etabliertere Rahmen für IT-Artefakte.
- **Warum konkret der DSRM-Prozess nach Peffers et al.?**
  - Er ist der in der Wirtschaftsinformatik am weitesten verbreitete, klar strukturierte DSR-Prozess.
  - Seine sechs Aktivitäten lassen sich unmittelbar auf die Kapitelstruktur der Arbeit abbilden (vgl. 3.3.1).
  - Er erzwingt die **Kommunikation** als eigenständige Aktivität — für eine Abschlussarbeit essenziell.
- **Rolle der Fallstudie (Studienplaner)**
  - Die Fallstudie ist kein Selbstzweck, sondern die **Demonstrationsinstanz** des framework-agnostischen Ansatzes.
  - Begründung als Einzelfall: Der Studienplaner repräsentiert bewusst die Klasse „laienhaft in Auftrag gegebener, KI-generierter Web-Prototyp“ und erlaubt so die Validierung des Migrationspfads an einem authentischen Vertreter.

## 3.3 Skizzierung des eigenen Forschungsprozesses

### 3.3.1 Zuordnung der DSRM-Aktivitäten zu den Kapiteln

- Aktivität 1 (Problemidentifikation und Motivation) → **Kapitel 1** (Motivation, Problemstellung) und **Kapitel 4** (detaillierte Problemanalyse des Ist-Zustands).
- Aktivität 2 (Zieldefinition) → **Kapitel 4** (Ableitung der Anforderungen an das Artefakt).
- Aktivität 3 (Design und Entwicklung) → **Kapitel 5** (Architektur, Transformationsprozess, Implementierung).
- Aktivität 4 (Demonstration) → **Kapitel 6** (Anwendung des Migrationspfads auf die Fallstudie).
- Aktivität 5 (Evaluation) → **Kapitel 6** (methodische Evaluation, Abgleich mit den Anforderungen aus Kapitel 4).
- Aktivität 6 (Kommunikation) → die Arbeit selbst sowie **Kapitel 7** (Diskussion des Beitrags) und **Kapitel 8** (Fazit, Ausblick).
- Darstellung als **Zuordnungstabelle** (DSRM-Aktivität → Kapitel → zentrale Artefakte/Ergebnisse), ggf. ergänzt um eine Rückkopplungsnotation (Kapitel 6 kann auf Kapitel 5 zurückwirken).

<!-- PLACEHOLDER: Zuordnungstabelle DSRM-Aktivität -> Kapitel -> Artefakt (in LaTeX als \label{tab:dsrm-mapping}) -->
| DSRM-Aktivität | Kapitel | Zentrale Artefakte / Ergebnisse |
|---|---|---|
| 1 Problemidentifikation & Motivation | 1, 4 | Problemstellung, Ist-Zustands-Analyse |
| 2 Zieldefinition | 4 | Anforderungskatalog |
| 3 Design & Entwicklung | 5 | Architektur, Transformationsmethodik |
| 4 Demonstration | 6 | Fallstudien-Instanziierung |
| 5 Evaluation | 6 | Evaluationsergebnisse |
| 6 Kommunikation | 7, 8 | Diskussion, Fazit, Ausblick |

> Potenzielle Quelle für Aktivitätsbezeichnungen und -reihenfolge: Peffers et al. (2007), Abb. 1 (Prozessmodell). Die Zuordnung der Aktivitäten zu den Kapiteln ist eine eigene Synthese der Arbeit.

### 3.3.2 Iterative Design-Zyklen in der Praxis

- Der Forschungsprozess ist nicht linear, sondern besteht aus mehreren **Design-Zyklen**; jede Transformations-Teilaufgabe wird als Zyklus mit drei Phasen behandelt:
  1. **Plan/Design** — Aufgabe, betrachtete Alternativen, Begründung der Wahl.
  2. **Implementierung** — tatsächlich Gebautes, Abweichungen vom Plan.
  3. **Reflexion** — was funktionierte, was nicht, welche Muster und Sicherheitsaspekte sich zeigten.
- Diese Zyklen setzen die Iteration des DSRM-Prozesses konkret um und machen Rücksprünge (z. B. Designüberarbeitung nach einer Evaluationserkenntnis) sichtbar.
- Jeder Zyklus wird einem der Forschungsfragen FF1–FF4 zugeordnet (Rückverfolgbarkeit).

### 3.3.3 Dokumentation und Nachvollziehbarkeit

- Sämtliche Pläne, Designentscheidungen, Blocker und Reflexionen werden in `.agents/artifacts/` dokumentiert (eine Markdown-Datei pro Teilaufgabe).
- Diese Artefakte bilden die **DSR-Prüfspur** („audit trail“) der Arbeit und belegen Rigor: Entscheidungen sind nachvollziehbar begründet, Alternativen dokumentiert.
- Sie sind zugleich die primäre Quellenbasis für die Kapitel 4–6 und stellen die Rückverfolgbarkeit zu den Forschungsfragen sicher.
- Code allein genügt nicht als Dokumentation — gerade Blockaden und Trade-offs sind die wertvollsten Einsichten für die Arbeit.

## Offene Punkte für Quellen und spätere Überarbeitung

- Primärquellen in `refs.bib` aufnehmen: Peffers et al. (2007/2008, „A Design Science Research Methodology for Information Systems Research“, JMIS), Hevner et al. (2004, „Design Science in Information Systems Research“, MIS Quarterly), ggf. Hevner (2007, „A Three Cycle View“) und March & Smith (1995).
- Hevner ist als kurze Einordnung aufgenommen (3.1.2); beim Überführen nach LaTeX darauf achten, dass der Abschnitt knapp bleibt und nicht zu einem zweiten vollständigen Prozessmodell ausufert.
- Genaue Aktivitätsbezeichnungen und ggf. Seitenzahlen aus der Primärquelle verifizieren; deutsche Übersetzungen der Aktivitätsnamen konsistent verwenden.
- Abbildung des DSRM-Prozesses als `.drawio.svg` anlegen (sechs Aktivitäten mit Rückkopplungspfeilen, Einstiegspunkt „problemzentriert“ markieren).
- Zuordnungstabelle in 3.3.1 ist als Platzhalter angelegt; beim Überführen nach LaTeX entscheiden, ob sie als `\ref`-fähige `tabular`-Tabelle oder als Abbildung steht, und die Kapitel-/Artefaktspalten final befüllen.
- Begründung der Einzelfall-Studie ggf. mit Yin (Case Study Research) absichern; klarstellen, dass die Fallstudie Demonstrations- und Evaluationszweck dient, nicht eigenständige Methodik ist.
- Die Terminologie-Trennung (Forschungsmethodik vs. Transformationsmethodik als Artefakt) als expliziten Hinweis in den Fließtext aufnehmen, um Verwechslungen zu vermeiden.
- Umfang im Blick behalten: Das Kapitel ist ein Methodikkapitel einer Bachelorarbeit — konzise halten, keine allgemeine DSR-Lehrbuchdarstellung.
