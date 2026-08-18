# Kurzurteil

Der Entwurf ist strukturell klar und für FF4 (Sicherheitsrisiken KI-generierten Codes und gehärtete Zielarchitektur) unmittelbar relevant. Der Aufbau von Bedrohungsmodell über KI-spezifische Risiken zu überprüfbaren Zielanforderungen ist logisch tragfähig, und die Anforderungsliste in 2.3.3 ist fachlich präzise und angemessen an die spätere Evaluation anschlussfähig.

Der zentrale Mangel ist der vollständig fehlende Quellenapparat. Ein Grundlagenabschnitt, der Vertrauensannahmen, Angriffsflächen, KI-Risiken und Sicherheitskategorien wie Injection oder Broken Access Control benennt, muss diese Begriffe an etablierte Quellen (insbesondere OWASP, Threat-Modelling-/STRIDE-Literatur) anbinden. Der Abschnitt ist außerdem mit 36 Zeilen für die inhaltliche Bedeutung (Kern von FF4) unverhältnismäßig knapp und verallgemeinert an mehreren Stellen über die konkret betrachtete Electron- beziehungsweise Studienplaner-Architektur hinaus.

**Gesamtbewertung:** fachlich überzeugendes Gerüst, aber noch ein Rohgerüst. Vor der Überführung in LaTeX müssen Quellen ergänzt, der Geltungsbereich eingegrenzt und der Abschnitt inhaltlich vertieft werden. Empfehlung: **draft-quality / gezielte substanzielle Überarbeitung**, keine vollständige Neufassung nötig.

# Wichtigste Stärken

- Die Selbstabgrenzung zu 2.2.3 ist klar: Electron-Mechanismen werden nicht neu dokumentiert, sondern als Grundlage für Sicherheitsziele genutzt.
- Das Bedrohungsmodell unterscheidet sauber zwischen Schutzobjekten (lokalem Datenbestand, sqlite-Datei, Konfiguration, Anwendungsintegrität) und Angriffsflächen (Renderer, Preload-/IPC, Dateisystem, native Module, Paketierung/Updates).
- Das Angriffsszenario „manipulierte Renderer-Eingabe → zu weitreichende Preload-/IPC-Schnittstelle → privilegierte Operation“ knüpft direkt an das Prozess- und Vertrauensgrenzenmodell aus 2.2.3 an und konkretisiert es.
- Die KI-spezifischen Risiken (fehlende Validierung, überprivilegierte Hilfsfunktionen, hartkodierte Geheimnisse, halluzinierte Paketnamen) adressieren FF4 unmittelbar und sind nicht auf die Electron-Runtime beschränkt.
- Die Zielanforderungen sind überprüfbar und bewusst nicht als bereits erfüllte Eigenschaften formuliert; die Verschiebung der Umsetzung in die Analyse- und Entwurfskapitel ist methodisch sauber.
- Die Anforderungsliste spannt den Bogen vom theoretischen Sicherheitsmodell zur späteren Analyse, zum Entwurf und zur Evaluation des Artefakts.

# Kritische Verbesserungsbereiche

1. **Quellenapparat vollständig ergänzen.** Der Abschnitt enthält keinerlei Zitate. Vertrauensannahmen, Bedrohungsmodell, Angriffsflächen und die Kategorien Injection, Broken Access Control und Security Misconfiguration benötigen Belege – insbesondere OWASP Top 10 sowie etablierte Threat-Modelling-Literatur. **[BELEG ERFORDERLICH]**
2. **Geltungsbereich präzise eingrenzen.** „Eine lokale Desktop-Anwendung …“, „eine klassische Web-Anwendung“ und „Dadurch entfallen bestimmte serverseitige Schutzgrenzen“ verallgemeinern auf alle Desktop- beziehungsweise Web-Anwendungen. Zu scopen auf die betrachtete Electron-Zielanwendung beziehungsweise die cloudzentrierte Web-Referenzarchitektur des Studienplaners. **[AUSSAGE ZU ABSOLUT]**
3. **„Systematisch eingeordnet“ belegen oder verschieben.** Der Satz „Diese Risiken werden … systematisch eingeordnet“ kündigt eine Klassifikation an, die in diesem Abschnitt nicht erfolgt. Entweder eine kleine Zuordnungstabelle (Risiko → OWASP-Kategorie → Zielanforderung) einfügen oder die systematische Einordnung ausdrücklich dem Analyse-Kapitel zuweisen. **[BELEG ERFORDERLICH]**
4. **Inhaltlich vertiefen.** Der Abschnitt benennt Schutzobjekte und Anforderungen, begründet aber kaum, warum genau diese Schutzobjekte für den Studienplaner relevant sind (z. B. warum gespeicherte Zugangsdaten im lokalen Einzelplatzszenario existieren sollen) und wie die Anforderungen aus dem Bedrohungsmodell abgeleitet werden. Für eine Grundlagenbasis von FF4 ist das Verhältnis von Anspruch zu Umfang zu knapp.
5. **Mangelnde Verknüpfung zum konkreten Ausgangsartefakt.** Die „besonderen Risiken eines KI-generierten Ausgangssystems“ bleiben abstrakt. Nicht die Befunde selbst, aber die Herleitung, warum gerade diese Risiken aus KI-generiertem Code typisch sind, sollte hier mindestens skizziert oder als Arbeitsannahme gekennzeichnet und auf spätere Analyse verwiesen werden.
6. **Terminologie vereinheitlichen.** „für die Nutzenden erkennbar“ weicht von der im Kapitel (Local-First-Draft) verwendeten Formulierung „die nutzende Person“ ab. Zudem fehlen Acronym-Erstdefinitionen beziehungsweise `\gls{}`-Marker für Electron, Renderer, IPC, SQLite. **[BEGRIFF DEFINIEREN]**

# Detaillierte Analyse

## Funktion des Kapitels

Die Eingangsabgrenzung ist vorbildlich: Sie benennt explizit, was der Abschnitt nicht leistet (keine erneute Electron-Dokumentation) und woraus er schöpft (Grundlage für Sicherheitsziele und Anforderungen). Auch der Verweis, dass die konkrete Umsetzung späteren Kapiteln vorbehalten bleibt, verhindert eine unerwünschte Vorwegnahme des Entwurfs.

## Bedrohungsmodell und Angriffsflächen (2.3.1)

Die Unterscheidung von Schutzobjekten und Angriffsflächen ist fachlich angemessen und vollständig für den betrachteten Kontext. Der Renderer-, Preload- und IPC-Fokus spiegelt korrekt das Vertrauensgrenzenmodell aus 2.2.3 wider. Auffällig ist die fehlende Herleitung: Warum gerade diese Schutzobjekte, welche Sicherheitsziele (Vertraulichkeit, Integrität, Verfügbarkeit) jeweils betroffen sind und welchem Bedrohungsmodell-Ansatz (z. B. STRIDE) gefolgt wird, bleibt offen. Die Nennung von „gespeicherten Zugangsdaten“ als Schutzobjekt sollte geprüft werden, ob sie für ein lokales Einzelplatzszenario ohne Authentifizierung wirklich zutrifft oder sich auf die übernommene Web-Persistenz bezieht.

## Sicherheitsrisiken KI-generierter Anwendungen (2.3.2)

Der Abschnitt ist inhaltlich treffend und FF4-relevant. Die genannten Kategorien (Injection, Broken Access Control, Security Misconfiguration) sind OWASP-Terminologie und müssen zwingend belegt werden. Die Aussage zur Lieferkette (halluzinierte Paketnamen mit nachträglich veröffentlichtem bösartigen Paket) ist anschaulich, aber ebenfalls unbelegt. Zu unterscheiden bleibt, welche Aussagen als Literaturbefund und welche als Arbeitsannahme für die Fallstudie gelten. Es fehlt ein Übergang, der die KI-spezifischen Risiken mit dem in 2.3.1 etablierten Bedrohungsmodell verbindet.

## Sicherheitsanforderungen an das Zielartefakt (2.3.3)

Die acht Anforderungen sind gut formuliert und überprüfbar („darf nicht“, „müssen … erneut validiert“, „begrenzt werden“). Sie bilden einen brauchbaren Ausgangspunkt für spätere Evaluationskriterien. Zu prüfen ist die Vollständigkeit: Es fehlen Anforderungen zu Verschlüsselung der lokalen Datenbank, zu Updates/gefälschten Updatekanälen, zur Abgrenzung von Benutzerdaten gegenüber dem unveränderlichen Installationsverzeichnis sowie zur Absicherung der Lieferkette über Paket-Hashing/Lockfiles. Diese Punkte sollten entweder ergänzt oder bewusst als Nicht-Ziele markiert werden.

## Sprachliche und formale Qualität

Stil, Satzbau und Ton sind wissenschaftlich und präzise. Die Sprache ist deutlich knapper und pointierter als im Local-First-Draft, was angesichts der Bedeutung des Themas teilweise zu Lasten der Herleitung geht. Grammatik und Rechtschreibung sind unauffällig. Auffällig ist die inkonsistente Personenbezeichnung („die Nutzenden“ vs. „die nutzende Person“ im übrigen Kapitel) sowie das Fehlen von Quellenhinweisen für die spätere Überführung, die im Geschwister-Draft gepflegt werden.

## Qualitätsdichte

Die Aussagedichte pro Zeile ist hoch, insbesondere in der Anforderungsliste. Bezogen auf die inhaltliche Bedeutung des Abschnitts für FF4 ist die Gesamtdichte jedoch gering: Viele Kernaussagen stehen als Behauptung ohne Herleitung oder Beleg. Es fehlt die Balance zwischen präzisem Gerüst und der für einen Grundlagenabschnitt erwarteten Nachvollziehbarkeit.

# Zitatbasierte Befunde

> **Zitat:** „Eine lokale Desktop-Anwendung besitzt andere Vertrauensannahmen als eine klassische Web-Anwendung.“

- **Fundstelle:** 2.3.1, Absatz 1
- **Kategorie:** fachlich / begrifflich
- **Schweregrad:** wichtig
- **Problem:** Verallgemeinert auf alle lokalen Desktop- beziehungsweise Web-Anwendungen. „Klassische Web-Anwendung“ ist zudem unscharf (der bisherige Kapitelwortschatz nutzt „cloudzentriert“). Die Aussage gilt für die betrachtete Electron-Zielarchitektur und die cloudzentrierte Studienplaner-Referenzarchitektur, nicht universell.
- **Verbesserung:** „Die hier betrachtete lokale Desktop-Anwendung besitzt andere Vertrauensannahmen als die cloudzentrierte Web-Referenzarchitektur des Ausgangsartefakts.“
- **Belegbedarf:** [AUSSAGE ZU ABSOLUT]

> **Zitat:** „Diese Risiken werden anhand geeigneter Kategorien, beispielsweise Injection, Broken Access Control und Security Misconfiguration, systematisch eingeordnet.“

- **Fundstelle:** 2.3.2, Absatz 2
- **Kategorie:** Quelle / Struktur
- **Schweregrad:** wichtig
- **Problem:** OWASP-Terminologie ohne Quelle; die angekündigte systematische Einordnung erfolgt im Abschnitt nicht (keine Zuordnung von Risiko zu Kategorie).
- **Verbesserung:** OWASP Top 10 zitieren und eine kleine Zuordnung (Risiko → Kategorie → Zielanforderung) aufnehmen oder die Einordnung ausdrücklich in das Analyse-Kapitel verschieben.
- **Belegbedarf:** [BELEG ERFORDERLICH]

> **Zitat:** „Halluzinierte Paketnamen können die Lieferkette gefährden, wenn unter einem vom Modell vorgeschlagenen, aber eigentlich nicht existierenden Namen ein bösartiges Paket veröffentlicht wird.“

- **Fundstelle:** 2.3.2, Absatz 2
- **Kategorie:** fachlich / Quelle
- **Schweregrad:** wichtig
- **Problem:** Plausibles Szenario, aber unbelegt; es ist eine sicherheitspolitische Behauptung über Verhalten in Paketökosystemen, keine offensichtliche Selbstverständlichkeit.
- **Verbesserung:** Mit Studie oder Sicherheitsbericht zu supply-chain-Angriffen belegen oder als Arbeitsannahme der Fallstudie kennzeichnen.
- **Belegbedarf:** [BELEG ERFORDERLICH]

> **Zitat:** „Zu den relevanten Schutzobjekten gehören insbesondere … gespeicherte Zugangsdaten …“

- **Fundstelle:** 2.3.1, Absatz 2
- **Kategorie:** fachlich / Scope
- **Schweregrad:** geringfügig
- **Problem:** Im lokalen Einzelplatzszenario ohne Authentifizierungsdienst ist unklar, welche Zugangsdaten es geben soll; möglicherweise Überbleibsel der Web-Persistenz.
- **Verbesserung:** Entweder konkret benennen (z. B. übernommene Cloud-/BaaS-Zugangsdaten vor Migration) oder streichen.
- **Belegbedarf:** nein

> **Zitat:** „Sensible Daten dürfen nicht in Quellcode, Renderer-Bundles oder Protokollmeldungen eingebettet werden.“

- **Fundstelle:** 2.3.3, Anforderungsliste
- **Kategorie:** fachlich / Struktur
- **Schweregrad:** wichtig
- **Problem:** Gute, überprüfbare Anforderung, aber nicht klar aus dem vorangegangenen Bedrohungsmodell hergeleitet; außerdem fehlt eine Verschlüsselungsanforderung für die lokale Datenbank in der Liste.
- **Verbesserung:** Begründen, warum diese Anforderung aus den KI-Risiken (hartkodierte Geheimnisse) folgt, und eine Anforderung zur Verschlüsselung ruhender Daten beziehungsweise zur Trennung von Benutzerdaten und Installationsverzeichnis ergänzen beziehungsweise als Nicht-Ziel markieren.
- **Belegbedarf:** prüfen

# Priorisierte To-do-Liste

## A: Kritisch

- Quellen für Bedrohungsmodell, Angriffsflächen und Sicherheitskategorien ergänzen (OWASP Top 10, Threat-Modelling-Literatur, Electron-Dokumentation) und in `refs.bib` anlegen.
- Die angebliche systematische Einordnung der Risiken entweder mit einer Zuordnung im Abschnitt oder als expliziter Verweis auf das Analyse-Kapitel auflösen.
- Absolute Aussagen („eine lokale Desktop-Anwendung“, „eine klassische Web-Anwendung“) auf den untersuchten Kontext einschränken.

## B: Wichtig

- Schutzobjekte aus dem konkreten Studienplaner-Szenario ableiten und „gespeicherte Zugangsdaten“ verifizieren oder entfernen.
- Den Abschnitt inhaltlich vertiefen: Herleitung der Anforderungen, Unterscheidung Literaturbefund/Arbeitsannahme, Übergang von 2.3.1 zu 2.3.2.
- Anforderungsliste um Verschlüsselung, Update-/Paketierungssicherheit und Trennung Benutzer-/Installationsdaten prüfen und entweder ergänzen oder als Nicht-Ziel kennzeichnen.
- Terminologie vereinheitlichen („die nutzende Person“) und Acronym-Erstdefinitionen bzw. `\gls{}`-Marker für LaTeX vorbereiten.

## C: Optional

- Eine Zuordnungstabelle (Schutzobjekt → Sicherheitsziel → typische Bedrohung) als Zusatzwert prüfen.
- Pro Abschnitt einen Absatz zur Relevanz für die späteren Kapitel 4–6 ergänzen, um den roten Faden zu FF4 zu stärken.
- Kleinen Abschnitt „Quellenhinweise für die spätere Überführung“ analog zum Geschwister-Draft anlegen.

# Bewertungstabelle

| Kriterium | Bewertung (0--10) | Begründung |
|---|---:|---|
| Fachliche Präzision | 7 | Angriffsflächen und Anforderungen präzise; einzelne Verallgemeinerungen und unbelegte Sicherheitsaussagen. |
| Begriffs- und Terminologieklarheit | 7 | Gute Verwendung von Bedrohungsmodell-Terminologie; „klassische Web-Anwendung“ unscharf, OWASP-Begriffe unbelegt. |
| Scope und Geltungsbereich | 6 | Mehrere über den Untersuchungskontext hinausreichende Aussagen; Schutzobjekte teils spekulativ. |
| Argumentationslogik | 8 | Klarer Bogen Bedrohungsmodell → KI-Risiken → Anforderungen; Übergänge zwischen 2.3.1 und 2.3.2 noch zu sprungartig. |
| Bezug zur Forschungsfrage | 9 | Direkter Beitrag zu FF4; Anforderungen bereiten Analyse und Evaluation vor. |
| DSR-Passung | 7 | Grundlagenabschnitt, kein vollständiger Zyklus nötig; Anforderungen als spätere Evaluationskriterien operationalisierbar. |
| Wissenschaftlicher Ton | 8 | Sachlich, formal, unaufgeregt. |
| Sprachliche Qualität | 8 | Lesbar und präzise; teils zu stark verdichtet ohne Herleitung. |
| Rechtschreibung und Grammatik | 9 | Keine wesentlichen Fehler erkennbar. |
| LaTeX und Typografie | 5 | Markdown-Draft ohne Zitierschlüssel und ohne Glossar-Marker; Überführungsnotizen fehlen. |
| Quellen- und Belegfähigkeit | 3 | Keine einzige Quelle im gesamten Abschnitt; sämtliche Sicherheits- und KI-Risikoaussagen unbelegt. |
| Informations- und Qualitätsdichte | 6 | Hohe Dichte pro Zeile, aber inhaltlich deutlich unterbesetzt für die Relevanz von FF4. |
| Praktische Überarbeitbarkeit | 8 | Kurzes, klar strukturiertes Gerüst; Quellen und Vertiefung sind gut lokalisierbar, keine Neufassung nötig. |

# Gesamturteil

Der Entwurf ist ein fachlich richtiges, aber unvollständiges Gerüst. Die Struktur (Bedrohungsmodell → KI-Risiken → Zielanforderungen) und die überprüfbare Anforderungsliste sind tragfähig und unmittelbar FF4-relevant. Vor der Übernahme fehlen Belege für sämtliche Sicherheitsaussagen, eine Eingrenzung des Geltungsbereichs auf das Studienplaner-/Electron-Szenario und eine inhaltliche Vertiefung auf eine der Bedeutung angemessene Länge.

Die passende Einstufung lautet **draft-quality mit gezielter substanzieller Überarbeitung**. Nach Ergänzung des Quellenapparats, der Scope-Korrekturen und der Vertiefung ist der Abschnitt für eine Grundlagenbasis von FF4 gut übernahmefähig.
