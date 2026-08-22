# Kapitel 3 — Methodisches Vorgehen (Draft)

## Einordnung und Ziel des Kapitels

Das vorliegende Kapitel legt das methodische Fundament der Arbeit und klärt zunächst eine begriffliche Doppeldeutigkeit, die sich durch den Titel der Arbeit zieht.
Mit „Methodik" kann einerseits die Forschungsmethodik gemeint sein, nach der die Arbeit selbst vorgeht, andererseits die Transformationsmethodik, die als Ergebnis der Arbeit entwickelt wird.
Beides muss sauber getrennt werden.
Dieses Kapitel beschreibt ausschließlich die Forschungsmethodik, also den Rahmen, die Begründung und den konkreten Ablauf der Untersuchung.
Die Transformationsmethodik selbst ist das Artefakt der Arbeit und wird erst in Kapitel 5 behandelt.

Der rote Faden des Kapitels ergibt sich unmittelbar aus dem Charakter der Forschungsfragen.
Alle vier Forschungsfragen sind gestaltungsorientiert: Sie fragen, wie KI-generierte Webarchitekturen systematisch in sichere Local-First-Desktop-Anwendungen überführt werden können, welche Refactoring-Muster dafür erforderlich sind, wie cloudabhängige Datenhaltung durch lokale Speicherung ersetzt wird und welche Sicherheitsrisiken zu adressieren sind.
Solche „Wie"-Fragen verlangen keine Beschreibung eines Ist-Zustands, sondern die Entwicklung und Rechtfertigung einer Lösung.
Genau dafür bietet sich Design Science Research als methodischer Rahmen an.

## 3.1 Design Science Research als methodischer Rahmen

### 3.1.1 Grundidee und Charakteristika

Design Science Research ist ein Forschungs- und Problemlösungsparadigma, dessen Kern darin besteht, ein praktisches Problem durch die Schaffung und Evaluation eines IT-Artefakts zu lösen und dabei zugleich einen wissenschaftlichen Erkenntnisbeitrag zu erzeugen.
Damit unterscheidet sich DSR grundlegend von verhaltenswissenschaftlicher beziehungsweise deskriptiver Forschung: Während diese Phänomene beschreibt und erklärt, ist DSR präskriptiv angelegt und gestaltet Lösungen.
Die Arbeit verfolgt entsprechend kein Erklärungsziel, sondern ein Gestaltungsziel.

Als Artefakte kommen dabei unterschiedliche Typen in Betracht, insbesondere Konstrukte, Modelle, Methoden und Instanziierungen.
Die vorliegende Arbeit erzeugt primär eine Methode, nämlich einen framework-agnostischen Migrationspfad, und sekundär eine Instanziierung, nämlich die daraus hervorgehende Electron-Anwendung.
Beide zusammen bilden den Erkenntnisbeitrag der Arbeit.

### 3.1.2 Einordnung nach Hevner

Für die Einordnung von DSR ist das von Hevner geprägte Spannungsfeld zwischen Relevanz und Rigor zentral.
Relevanz bezeichnet dabei den Beitrag zur Lösung eines realen Problems, Rigor die wissenschaftliche Strenge des Vorgehens.
Beide müssen im Gleichgewicht stehen: Ein Artefakt, das ein Problem löst, aber methodisch nicht belastbar begründet ist, trägt ebenso wenig zur Forschung bei wie ein methodisch sauberes Artefakt ohne praktischen Nutzen.

Hevner modelliert Design Science als drei miteinander verbundene Zyklen.
Der Relevance Cycle verbindet die Forschung mit dem Anwendungsumfeld, also mit dem realen Problem und den daraus abgeleiteten Anforderungen.
Der Rigor Cycle verbindet die Forschung mit der Wissensbasis, also mit bestehenden Theorien, Methoden und Artefakten.
Der Design Cycle bildet den inneren Kern und umfasst das Bauen und Evaluieren des Artefakts.

In dieser Arbeit liefert die Drei-Zyklen-Sicht die konzeptionelle Rahmung dafür, warum sowohl das Praxisproblem als auch die methodische Strenge berücksichtigt werden müssen.
Die konkrete Schrittfolge des Vorgehens stammt dagegen aus dem Prozessmodell nach Peffers, das im Folgenden vorgestellt wird.

### 3.1.3 Der DSRM-Prozess nach Peffers

Der Prozess nach Peffers operationalisiert Design Science als Abfolge von sechs Aktivitäten.
Die erste Aktivität besteht in der Identifikation des Problems und der Motivation für dessen Lösung.
Die zweite Aktivität definiert die Ziele, die eine Lösung erreichen soll, und leitet daraus Anforderungen ab.
Die dritte Aktivität umfasst Design und Entwicklung des Artefakts.
Die vierte Aktivität demonstriert das Artefakt, indem es auf eine konkrete Instanz des Problems angewendet wird.
Die fünfte Aktivität evaluiert, wie gut das Artefakt die zuvor definierten Ziele erreicht.
Die sechste Aktivität kommuniziert Problem, Artefakt, Nutzen und methodische Strenge an Fachwelt und Praxis.

Entscheidend ist, dass dieser Prozess nicht linear, sondern iterativ angelegt ist.
Nach der Demonstration oder Evaluation kann in frühere Aktivitäten zurückgesprungen werden, etwa um das Design zu überarbeiten, wenn die Evaluation Schwächen offenbart.
Der Prozess kennt zudem mehrere mögliche Einstiegspunkte; die vorliegende Arbeit nutzt den problemzentrierten Einstieg, da ihr Ausgangspunkt das identifizierte Problem laienhaft KI-generierter Web-Prototypen ist.

## 3.2 Begründung der Methodenwahl

Die Wahl von Design Science Research folgt unmittelbar aus dem Charakter der Forschungsfragen.
Da alle vier Fragen darauf zielen, eine gestaltbare Lösung hervorzubringen, genügt ein rein deskriptiver Ansatz nicht: Eine Befragung oder Literaturanalyse könnte das Problem beschreiben, aber nicht die geforderte Transformationsmethodik erzeugen und belegen.
DSR ist dagegen genau darauf ausgerichtet, ein Artefakt zu entwickeln und dessen Nutzen nachzuweisen.

Gegenüber verwandten Ansätzen grenzt sich die Arbeit wie folgt ab.
Action Research zielt auf organisatorische Veränderung und Intervention im laufenden Betrieb, nicht auf ein übertragbares Design-Artefakt als Erkenntnisbeitrag.
Die Fallstudie ist eine Forschungsstrategie zur Untersuchung eines Phänomens in seinem Kontext, erzeugt aber für sich genommen kein Design-Artefakt.
Der konstruktive Forschungsansatz steht DSR zwar nahe, ist jedoch primär betriebswirtschaftlich geprägt.
DSR ist in der Informatik und Wirtschaftsinformatik der etablierte Rahmen für die Entwicklung und Bewertung von IT-Artefakten und wird daher gewählt.

Innerhalb von DSR fällt die Wahl auf den Prozess nach Peffers, weil er der am weitesten verbreitete und klar strukturierte DSR-Prozess ist.
Seine sechs Aktivitäten lassen sich unmittelbar auf die Kapitelstruktur der Arbeit abbilden, und er erhebt die Kommunikation der Ergebnisse zu einer eigenständigen Aktivität, was für eine Abschlussarbeit essenziell ist.

Die Fallstudie des Studienplaners ist in diesem Rahmen kein Selbstzweck.
Sie dient als Demonstrations- und Evaluationsvehikel: An ihr wird der framework-agnostische Migrationspfad angewendet und überprüft.
Als Einzelfall ist sie bewusst so gewählt, dass sie die Klasse „laienhaft in Auftrag gegebener, KI-generierter Web-Prototyp" authentisch repräsentiert.
Damit erlaubt sie die Validierung des Ansatzes an genau der Art von Ausgangsartefakt, auf die die Arbeit abzielt.

## 3.3 Skizzierung des eigenen Forschungsprozesses

### 3.3.1 Zuordnung der DSRM-Aktivitäten zu den Kapiteln

Die sechs Aktivitäten des Prozesses nach Peffers werden im Verlauf der Arbeit wie folgt auf die Kapitel abgebildet.
Die Problemidentifikation und Motivation findet sich in Kapitel 1, das die Motivation und Problemstellung entfaltet, und wird in Kapitel 4 durch die detaillierte Analyse des Ist-Zustands vertieft.
Die Zieldefinition erfolgt ebenfalls in Kapitel 4, wo aus der Problemanalyse die Anforderungen an das Artefakt abgeleitet werden.
Design und Entwicklung des Artefakts werden in Kapitel 5 beschrieben, das die konzeptionelle Architektur und die praktische Umsetzung behandelt.
Die Demonstration erfolgt in Kapitel 6 durch die Anwendung des Migrationspfads auf die Fallstudie.
Dort findet auch die Evaluation statt, indem das Ergebnis gegen die in Kapitel 4 definierten Anforderungen abgeglichen wird.
Die Kommunikation schließlich ist die Arbeit selbst und wird in Kapitel 7 durch die Diskussion des Beitrags sowie in Kapitel 8 durch Fazit und Ausblick abgeschlossen.
Diese Zuordnung macht den Forschungsprozess transparent und belegt, dass jede Aktivität des DSRM-Prozesses in der Arbeit tatsächlich durchlaufen wird.

### 3.3.2 Iterative Design-Zyklen in der Praxis

Der Forschungsprozess ist in der Praxis nicht linear, sondern vollzieht sich in mehreren Design-Zyklen.
Jede Teilaufgabe der Transformation wird dabei als Zyklus mit drei Phasen behandelt.
In der Planungsphase werden die Aufgabe, die betrachteten Alternativen und die Begründung der gewählten Variante festgehalten.
In der Implementierungsphase wird dokumentiert, was tatsächlich gebaut wurde und wo vom Plan abgewichen wurde.
In der Reflexionsphase wird festgehalten, was funktioniert hat, was nicht und welche Muster oder Sicherheitsaspekte sich dabei zeigten.

Diese Zyklen setzen die Iteration des DSRM-Prozesses konkret um und machen Rücksprünge sichtbar, etwa wenn eine Erkenntnis aus der Evaluation eine Überarbeitung des Designs erfordert.
Jeder Zyklus wird zudem einer der vier Forschungsfragen zugeordnet, sodass die Rückverfolgbarkeit zwischen Vorgehen und Forschungsfragen gewährleistet ist.

### 3.3.3 Dokumentation und Nachvollziehbarkeit

Für die methodische Strenge der Arbeit ist entscheidend, dass sämtliche Pläne, Designentscheidungen, aufgetretenen Hindernisse und Reflexionen dokumentiert werden.
Code allein genügt dafür nicht.
Jede Teilaufgabe wird deshalb in einer eigenen Dokumentationsdatei festgehalten, die Auskunft darüber gibt, welches Problem vorlag, welche Lösungswege erwogen wurden, welches Ergebnis erzielt wurde und was daraus gelernt wurde.
Gerade Blockaden und Kompromisse sind dabei die wertvollsten Einsichten, da sie die nicht offensichtlichen Entscheidungen der Arbeit sichtbar machen.

Diese Dokumentation erfüllt eine doppelte Funktion.
Zum einen bildet sie die Prüfspur der Arbeit und belegt, dass Entscheidungen nachvollziehbar begründet und Alternativen abgewogen wurden.
Zum anderen ist sie die primäre Quellenbasis für die Kapitel 4 bis 6, die aus diesen Aufzeichnungen die Analyse, den Entwurf und die Evaluation rekonstruieren.
Auf diese Weise wird der DSRM-Prozess nicht nur behauptet, sondern durch eine durchgängige Dokumentation nachvollziehbar belegt.
