# Review — Einleitung (Entwurf, `entwurf.md`)

Modus A: akademisches Vollreview. Gegenstand: Markdown-Entwurf, nicht LaTeX. Das Kriterium „LaTeX und Typografie" wird daher als N/A (bzw. als Schreibkonvention) behandelt und nicht als Abzug gewertet. Das Originaldokument wurde nicht verändert; es wird nicht in `.agents/artifacts/` geschrieben und `write-dsr-artifact` nicht aufgerufen.

Gelesenes Quellmaterial: `context/project.md`, `context/thesis-writing.md`, `context/methodology.md`, `.agents/artifacts/_index.md`, `.agents/artifacts/001-*.md`, `.agents/artifacts/002-*.md`, `review-checklist.md`, `model-recommendations.md`.

---

## Kurzurteil

Der Entwurf besitzt eine klare, tragfähige Erzählstruktur (Motivation → Problemstellung → Zielsetzung → Forschungsfragen → Aufbau) und die Forschungsfragen FF1–FF4 stimmen inhaltlich mit `context/project.md` überein. Der Text ist als **brauchbarer Entwurf (draft-quality)** einzustufen, der jedoch **vollständig ohne Quellenbelege** auskommt und an einer zentralen Stelle einen fachlichen Widerspruch enthält: Die Zielarchitektur wird als „zustandslose Web-Architektur" bezeichnet, obwohl FF3 explizit von einer „cloudabhängigen Datenbankspeicherung" ausgeht und der tatsächliche Prototyp sessionbasiert mit PostgreSQL arbeitet. Vor einer Übernahme nach `content/1_einleitung.tex` müssen primär dieser Widerspruch, die fehlenden Belege (ISO/IEC 25010, OWASP Top 10, Local-First) und die nicht explizit formulierte Forschungslücke behoben werden.

---

## Wichtigste Stärken

1. **Kohärenter narrativer Bogen.** Die fünf Abschnitte folgen stringent der Logik „Problem aus KI-Prototypisierung → konkretes Migrationsszenario → Ziel → Fragen → Aufbau". Der rote Faden ist durchgängig erkennbar.
2. **Forschungsfragen sind konsistent.** FF1–FF4 entsprechen inhaltlich (übersetzt) exakt `context/project.md`; keine stillen Abweichungen im Zuschnitt der Fragen.
3. **Korrekte Einordnung des Web-Prototyps als Ausgangsartefakt.** Der Text stellt klar, dass die Webanwendung Startpunkt und nicht Zielprodukt ist (Zeile 31, 38) — deckungsgleich mit der „Core Mission" aus `project.md`.
4. **Gelungene Einengung auf ein konkretes Szenario.** Nach der breiten Motivation (Zeilen 5–12) wird sauber auf das Electron-/Local-First-Szenario fokussiert (Zeile 30–31).
5. **Local-First wird begründet statt nur behauptet.** Die Zielarchitektur wird über Offline-Fähigkeit, Datensouveränität und netzentkoppelte Verfügbarkeit gerechtfertigt (Zeile 41).
6. **DSR-Anbindung im Aufbau vorhanden.** Kapitel 4–7 lassen sich plausibel auf Analyse, Entwurf, Evaluation und Diskussion des Design-Science-Research-Prozesses abbilden (Zeile 54–60).
7. **Hohe Überarbeitbarkeit.** Klare Abschnittsgliederung, nummerierte Forschungsfragen und eine kompakte Länge machen gezielte Revisionen einfach.

---

## Kritische Verbesserungsbereiche (nach Priorität)

1. **Fachlicher Widerspruch „zustandslose Web-Architektur"** (Zeile 30) — widerspricht FF3 (cloudabhängige DB) und dem realen Prototyp (sessionbasiert, PostgreSQL). Höchste Priorität, da konzeptioneller Kernfehler.
2. **Vollständig fehlende Quellenbelege** — keine einzige Referenz; ISO/IEC 25010 (Zeile 18), OWASP Top 10 (Zeile 23), Local-First (Zeile 41) und die Produktnamen (Zeile 6) sind unbelegt.
3. **Forschungslücke wird nicht formuliert** — nur als Verweis auf Kapitel 2 (Zeile 55); die Einleitung muss die Lücke selbst benennen.
4. **Scope-Überdehnung** — „standardisierter Migrationspfad" (Zeile 36) und absolute Aussagen (Zeilen 12, 17, 27) reichen über das hinaus, was eine einzelne Fallstudie trägt.
5. **Defensive Metapassage im Ton** (Zeilen 42–43) — „Nicht jede Leserin und jeder Leser …" durchbricht den wissenschaftlichen Duktus und gehört gestrichen oder neutral formuliert.
6. **Begriff „No-Code into Real Code"** (Zeile 9) wird eingeführt, aber nicht definiert.

---

## Detaillierte Analyse

### Strukturell

- **Bogen vorhanden, aber ein Bruch in der Zielsetzung.** Der Abschnitt „Zielsetzung" formuliert in Zeile 36 das Ziel, springt in Zeile 40–43 aber unvermittelt in eine Rechtfertigung des Local-First-Paradigmas. Diese Rechtfertigung gehört logisch eher in die Problemstellung oder den Hintergrund (Kapitel 2), nicht zwischen Zielformulierung und Forschungsfragen. Der Abschnitt wirkt dadurch zweigeteilt.
- **Forschungslücke fehlt.** Die Einleitung behauptet implizit, eine Lücke zu schließen, benennt sie aber nicht. „arbeitet die bestehende Forschungslücke heraus" (Zeile 55) ist ein Zukunftsverweis, der das Desiderat nicht ersetzt. Eine Einleitung sollte das Desiderat selbst formulieren, idealerweise am Ende der Problemstellung.
- **Redundanz Motivation/Problemstellung.** Zeilen 5–7 und 17–18 beschreiben denselben Sachverhalt (KI erzeugt oberflächlich funktionsfähige Software) aus zwei Perspektiven. Teilweise Doppelung, die sich straffen lässt.
- **Gliederungsebene.** Die Überschriften „Motivation und Relevanz", „Problemstellung", „Zielsetzung", „Forschungsfragen", „Aufbau der Arbeit" sind sinnvoll und entsprechen dem erwarteten Schema einer Einleitung.

### Sprachlich

- **Grundsätzlich gutes wissenschaftliches Deutsch.** Präziser, weitgehend passivfreier Stil, angemessener Nominalgebrauch.
- **Überlange Sätze.** Zeile 22, 27 und 30 sind stark verschachtelt (drei bis vier Teilsätze, mehrfache Nebensätze). Für die spätere `.tex`-Fassung gilt die Ein-Satz-pro-Zeile-Regel; die Sätze sollten zusätzlich aufgeteilt werden.
- **Terminologische Unschärfe bei „sicher".** Das Wort tritt in FF1, FF2, Zeile 31 und 37 auf, meint aber wechselnd „IT-Sicherheit" (Security), „Betriebssicherheit" (Safety/Reliability) und „zuverlässig". Zeile 23 unterscheidet selbst „Sicherheitsmechanismen" und „funktionale Betriebssicherheit" — diese Differenzierung sollte konsistent durchgehalten werden.
- **Anglizismus/Fachterminus „No-Code into Real Code".** Als Eigenbezeichnung in Anführungszeichen akzeptabel, aber ohne Definition inhaltlich leer. Siehe Zitatbefund.
- **Rechtschreibung.** „frameworkagnostischen" (Zeile 47) sollte mit Bindestrich „framework-agnostischen" geschrieben werden (analog zu „Local-First", „No-Code").

### Formal / Schreibkonventionen

- **Markdown-Struktur konsistent** (ein `#`, fünf `##`). Für die spätere LaTeX-Fassung sind die Markdown-Überschriften in `\section{}` zu überführen und die nummerierte Liste der Forschungsfragen in Fließtext oder `\begin{enumerate}` zu übersetzen (Konvention: fortlaufender Fließtext statt Listen).
- **Keine Quellenverwaltung erkennbar.** Keine `\cite`-Platzhalter, keine Fußnoten. Als Markdown-Entwurf nachvollziehbar, muss aber vor der Übernahme nach `refs.bib` aufgelöst werden.
- **Begriffe für das Glossar.** „Local-First", „Electron", „DSR/Design Science Research", „OWASP", „ISO/IEC 25010", „No-Code into Real Code" sollten als Glossareinträge (`\newacronym`) vorgesehen werden.

### Thematisch (geprüfte Schwerpunkte)

- **FF1–FF4 vs. `project.md`:** konsistent, keine Abweichung. Einzig die Kommasetzung/der Bindestrich in „framework-agnostisch" (FF1) ist zu vereinheitlichen.
- **ISO/IEC 25010 (Zeile 18):** unbelegt und inhaltlich unscharf. Die Aufzählung in Zeilen 20–23 (Wartbarkeit, Kopplung, Trennung der Belange, Interoperabilität, Sicherheit) entspricht nur teilweise den ISO-25010-Qualitätsmerkmalen und ist nicht explizit zugeordnet. Entweder präzise den Merkmalen zuordnen (Maintainability, Modularity, Security, Interoperability) oder die ISO-Referenz streichen. `[BELEG ERFORDERLICH]`.
- **OWASP Top 10 (Zeile 23):** unbelegt. Zudem ist „implementiert automatisch generierter Code selten adäquate Sicherheitsmechanismen" eine empirische Verallgemeinerung, die über die Fallstudie hinausgeht. `[BELEG ERFORDERLICH]` + `[AUSSAGE ZU ABSOLUT]`.
- **Local-First-Rationale (Zeilen 40–43):** sachlich korrekt, aber „aktuell vieldiskutiertes und praxisrelevantes Architekturprinzip" ist unbelegt. Die defensive Passage „Nicht jede Leserin und jeder Leser …" ist ein Stilbruch. `[BELEG ERFORDERLICH]`.
- **„zustandslose Web-Architektur" (Zeile 30):** Kernfehler. FF3 (Zeile 49) setzt eine „cloudabhängige Datenbankspeicherung" voraus, der Prototyp laut Artifact 001 nutzt Session-Auth (`express-session` + `bcryptjs`) und PostgreSQL. Eine solche Anwendung ist **nicht** zustandslos. Vermutlich ist „zustandslos" im Sinne von „nicht lokal persistent auf dem Client" gemeint — das muss präzise formuliert werden. Die Checkliste warnt ausdrücklich vor der Gleichsetzung „stateless gleich nicht persistent". `[BEGRIFF DEFINIEREN]` + `[AUSSAGE ZU ABSOLUT]`.

---

## Zitatbasierte Befunde

> **Zitat:** „die Überführung einer klassischen, zustandslosen Web-Architektur in eine Electron-basierte Desktop-Laufzeitumgebung"

- **Fundstelle:** Abschnitt „Problemstellung", letzter Absatz (Zeile 30)
- **Kategorie:** fachlich / begrifflich
- **Schweregrad:** kritisch
- **Problem:** Widerspricht FF3 („cloudabhängige Datenbankspeicherung", Zeile 49) und dem realen Prototyp (Session-Auth + PostgreSQL, Artifact 001). Eine sessionbasierte Fullstack-Anwendung mit serverseitigem Zustand und Datenbank ist nicht „zustandslos". Zudem fällt die Formulierung unter die Checklisten-Warnung „stateless gleich nicht persistent".
- **Verbesserung:** z. B. „die Überführung einer klassischen, **cloudbasierten** Web-Architektur mit **serverseitiger Sitzungs- und Datenhaltung** in eine Electron-basierte Desktop-Laufzeitumgebung" — oder explizit klären, dass „zustandslos" hier die fehlende **lokale** Client-Persistenz meint.
- **Belegbedarf:** prüfen (Begriff schärfen, kein neuer Beleg nötig)

> **Zitat:** „Fundamentale Qualitätsmerkmale des Software Engineerings nach ISO/IEC 25010 bleiben dabei meist unberücksichtigt."

- **Fundstelle:** Abschnitt „Problemstellung", erster Absatz (Zeile 18)
- **Kategorie:** Quelle
- **Schweregrad:** wichtig
- **Problem:** Behauptung über KI-generierten Code ohne Quellenangabe; „meist" ist eine unbelegte Verallgemeinerung. Die nachfolgende Aufzählung (Zeilen 20–23) ist nicht sauber auf die ISO-25010-Merkmale abgebildet.
- **Verbesserung:** ISO/IEC 25010 als Norm referenzieren und die genannten Mängel explizit den Merkmalen (Maintainability, Modularity, Interoperability, Security) zuordnen — oder die ISO-Referenz streichen.
- **Belegbedarf:** neue Quelle erforderlich `[BELEG ERFORDERLICH]`

> **Zitat:** „implementiert automatisch generierter Code selten adäquate Sicherheitsmechanismen gegen gängige Schwachstellen, etwa aus den OWASP Top 10"

- **Fundstelle:** Abschnitt „Problemstellung", zweiter Absatz (Zeile 23)
- **Kategorie:** Quelle / fachlich
- **Schweregrad:** wichtig
- **Problem:** Empirische Verallgemeinerung („selten", „automatisch generierter Code") ohne Beleg; OWASP Top 10 ohne Referenz. Geht über die Fallstudie hinaus.
- **Verbesserung:** Aussage auf die Fallstudie bzw. KI-generierte Web-Prototypen einschränken und mit Quelle belegen; OWASP Top 10 mit Editionsjahr referenzieren.
- **Belegbedarf:** neue Quelle erforderlich `[BELEG ERFORDERLICH]`

> **Zitat:** „Die Hauptaufgabe von Softwareentwicklern besteht fortan darin, diese prototypischen Entwürfe in eine skalierbare, robuste und produktionsreife Softwarearchitektur zu überführen."

- **Fundstelle:** Abschnitt „Motivation und Relevanz", dritter Absatz (Zeile 12)
- **Kategorie:** fachlich
- **Schweregrad:** wichtig
- **Problem:** Absolute Zuspitzung („Hauptaufgabe … besteht fortan"), die den gesamten Berufsstand verallgemeinert. Nicht durch die Fallstudie gedeckt.
- **Verbesserung:** abschwächen, z. B. „Ein zunehmend zentraler Teil der Entwicklungsarbeit besteht darin, …".
- **Belegbedarf:** prüfen `[AUSSAGE ZU ABSOLUT]`

> **Zitat:** „einen standardisierten Migrationspfad … zu entwickeln"

- **Fundstelle:** Abschnitt „Zielsetzung", erster Absatz (Zeile 36)
- **Kategorie:** Scope / Geltungsbereich
- **Schweregrad:** wichtig
- **Problem:** „standardisiert" beansprucht Allgemeingültigkeit, die eine einzelne Fallstudie (ein Studienplaner) nicht belegen kann. Deckungsgleich mit der Hard Rule „kein breiterer Scope als das Artefakt trägt".
- **Verbesserung:** „einen **methodischen, framework-agnostischen** Migrationspfad" oder „eine **wiederverwendbare Migrationsmethodik, validiert an einer Fallstudie**".
- **Belegbedarf:** prüfen `[QUELLENREICHWEITE PRÜFEN]`

> **Zitat:** „Local-First ist nicht nur ein aktuell vieldiskutiertes und praxisrelevantes Architekturprinzip … Nicht jede Leserin und jeder Leser benötigt alle diese Eigenschaften gleichermaßen; manche interessieren sich vorrangig für die lokale Datenhaltung, andere für die Absicherung KI-generierten Codes …"

- **Fundstelle:** Abschnitt „Zielsetzung", dritter bis fünfter Absatz (Zeilen 40–43)
- **Kategorie:** fachlich / sprachlich
- **Schweregrad:** wichtig
- **Problem:** „vieldiskutiert und praxisrelevant" ist unbelegt; die Passage „Nicht jede Leserin und jeder Leser …" ist eine defensive Metarechtfertigung, die den wissenschaftlichen Ton durchbricht und den Argumentationsfluss unterbricht.
- **Verbesserung:** Beleg für die Relevanz von Local-First ergänzen; die Leser-Relativierung streichen oder sachlich in die Begründung der Zielarchitektur einbetten.
- **Belegbedarf:** neue Quelle erforderlich `[BELEG ERFORDERLICH]`

> **Zitat:** „arbeitet die bestehende Forschungslücke heraus"

- **Fundstelle:** Abschnitt „Aufbau der Arbeit", Kapitel 2 (Zeile 55)
- **Kategorie:** Struktur
- **Schweregrad:** wichtig
- **Problem:** Die Forschungslücke wird nur als Aufgabe von Kapitel 2 angekündigt, aber in der Einleitung nicht selbst benannt. Ohne explizite Lücke fehlt der Argumentation der zentrale Begründungsanker.
- **Verbesserung:** Das Desiderat (fehlender systematischer Migrationspfad KI-generierter Web- zu Desktop-Architekturen) in der Problemstellung konkret formulieren.
- **Belegbedarf:** prüfen

> **Zitat:** „KI-gestützter Entwicklungsumgebungen wie Replit, Lovable oder Claude-Artifacts"

- **Fundstelle:** Abschnitt „Motivation und Relevanz", erster Absatz (Zeile 6)
- **Kategorie:** Quelle
- **Schweregrad:** geringfügig
- **Problem:** Produktnamen ohne Beleg oder Fußnote; Relevanz der Auswahl nicht belegt.
- **Verbesserung:** je ein Beleg/Fußnote pro Werkzeug oder ein Überblicksnachweis zur Verbreitung von Low-/No-Code.
- **Belegbedarf:** prüfen `[PRODUKTDETAIL PRÜFEN]`

> **Zitat:** „in dieser Arbeit als „No-Code into Real Code" bezeichnet"

- **Fundstelle:** Abschnitt „Motivation und Relevanz", zweiter Absatz (Zeile 9)
- **Kategorie:** begrifflich
- **Schweregrad:** geringfügig
- **Problem:** Neuer Begriff wird eingeführt, aber nicht definiert. Unklar, ob er als etabliertes Vorgehensmodell oder als Eigenprägung zu verstehen ist.
- **Verbesserung:** Definition ergänzen (was umfasst das Modell, wer nutzt es?) und als Glossarbegriff aufnehmen; falls Eigenprägung, dies kenntlich machen.
- **Belegbedarf:** prüfen `[BEGRIFF DEFINIEREN]`

> **Zitat:** „müssen dann entweder vollständig verworfen werden"

- **Fundstelle:** Abschnitt „Problemstellung", dritter Absatz (Zeile 27)
- **Kategorie:** fachlich
- **Schweregrad:** geringfügig
- **Problem:** „vollständig" ist eine Totalaussage, die die Checklisten-Warnung trifft; in der Praxis werden Prototypen selten komplett verworfen.
- **Verbesserung:** „müssen dann entweder weitgehend neu aufgebaut oder grundlegend überarbeitet werden".
- **Belegbedarf:** nein `[AUSSAGE ZU ABSOLUT]`

> **Zitat:** „mithilfe eines frameworkagnostischen Ansatzes"

- **Fundstelle:** Abschnitt „Forschungsfragen", FF1 (Zeile 47)
- **Kategorie:** formal / Rechtschreibung
- **Schweregrad:** geringfügig
- **Problem:** Schreibweise „frameworkagnostisch" ohne Bindestrich; uneinheitlich zu „Local-First", „No-Code".
- **Verbesserung:** „framework-agnostisch".
- **Belegbedarf:** nein

---

## Priorisierte To-do-Liste

**A — kritisch (vor jeder Übernahme nach `content/`):**

- [ ] „zustandslose Web-Architektur" (Zeile 30) fachlich korrigieren bzw. präzise definieren; Widerspruch zu FF3 und zum Prototyp auflösen.
- [ ] Quellenbelege ergänzen: ISO/IEC 25010 (Zeile 18), OWASP Top 10 (Zeile 23), Local-First-Relevanz (Zeile 41). Ggf. Einträge in `refs.bib` anlegen.
- [ ] Forschungslücke in der Einleitung explizit formulieren (nicht nur auf Kapitel 2 verweisen).

**B — wichtig:**

- [ ] Scope-Überdehnungen zurücknehmen: „standardisiert" (Zeile 36), „Hauptaufgabe … besteht fortan" (Zeile 12), „fast ausschließlich" (Zeile 17), „vollständig verworfen" (Zeile 27).
- [ ] Defensive Metapassage (Zeilen 42–43) streichen oder sachlich umformulieren.
- [ ] Begriff „No-Code into Real Code" definieren (Zeile 9).
- [ ] Terminologie „sicher" vereinheitlichen (Security vs. Betriebssicherheit vs. zuverlässig).
- [ ] Redundanz zwischen Motivation und Problemstellung straffen.

**C — optional:**

- [ ] Überlange Sätze (Zeilen 22, 27, 30) aufteilen (spätestens bei der `.tex`-Fassung, Ein-Satz-pro-Zeile-Regel).
- [ ] „framework-agnostisch" mit Bindestrich (Zeile 47).
- [ ] Glossareinträge (`\newacronym`) für Local-First, Electron, DSR, OWASP, ISO/IEC 25010 vorsehen.
- [ ] Produktnamen (Zeile 6) mit Fußnoten/Belegen versehen.

---

## Bewertungstabelle

| # | Kriterium | Punkte (0–10) |
|---|---|---|
| 1 | Fachliche Präzision | 6 |
| 2 | Begriffs- und Terminologieklarheit | 6 |
| 3 | Scope und Geltungsbereich | 6 |
| 4 | Argumentationslogik | 7 |
| 5 | Bezug zur Forschungsfrage | 8 |
| 6 | DSR-Passung | 7 |
| 7 | Wissenschaftlicher Ton | 7 |
| 8 | Sprachliche Qualität | 7 |
| 9 | Rechtschreibung und Grammatik | 8 |
| 10 | LaTeX und Typografie | N/A (Markdown-Entwurf) |
| 11 | Quellen- und Belegfähigkeit | 4 |
| 12 | Informations- und Qualitätsdichte | 7 |
| 13 | Praktische Überarbeitbarkeit | 8 |

*Begründete Näherung, kein objektiver Messwert. Mittelwert ohne Kriterium 10: ≈ 6,8 / 10.*

---

## Gesamturteil

**Draft-quality (brauchbarer Entwurf).**

Der Text ist strukturell tragfähig, argumentativ kohärent und inhaltlich konsistent zu den Forschungsfragen aus `project.md`. Er ist **kein Fall für „rework"** — die Grundarchitektur der Argumentation stimmt. Er ist aber auch **noch nicht „nearly publishable"**, weil er vollständig ohne Quellenbelege auskommt, an einer Stelle („zustandslos") einen konzeptionellen Widerspruch enthält und die Forschungslücke nicht selbst benennt. Nach Abarbeitung der A-Punkte (Widerspruch auflösen, Belege ergänzen, Lücke formulieren) und der wichtigsten B-Punkte (Scope zurücknehmen, Ton glätten) ist der Entwurf reif für die Überführung nach `content/1_einleitung.tex`.
