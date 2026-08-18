# 2.4 Das Local-First-Paradigma

Local First bezeichnet keine einzelne Technologie und auch keine bestimmte Desktop-Runtime.
Das Paradigma beschreibt vielmehr eine architektonische Priorisierung: Daten und zentrale Anwendungsoperationen sollen zunächst auf dem Gerät der nutzenden Person stattfinden, während ein Netzwerkdienst lediglich ergänzend für Synchronisation, Sicherung oder die Zusammenarbeit mehrerer Personen eingesetzt wird.
Damit unterscheidet sich Local First grundlegend von einer cloud-zentrierten Anwendung, in der die entfernte Datenbank die autoritative Datenquelle darstellt und der Client überwiegend als Repräsentation oder Cache dieser Daten dient~\cite{kleppmann2019localfirst}.

Die lokale Ausführung einer Anwendung ist dabei eine mögliche, aber keine hinreichende Voraussetzung für ein Local-First-Zielbild.
Eine Electron-Anwendung kann beispielsweise lediglich eine cloudbasierte Webanwendung verpacken und weiterhin für jede fachliche Operation ein entferntes Backend benötigen.
Umgekehrt kann eine Webanwendung mit Browserpersistenz und einem Offline-Datenmodell local-first-Eigenschaften besitzen, ohne als klassische Desktop-Anwendung installiert zu werden.
Für die vorliegende Arbeit ist daher zwischen der Desktop-Runtime, der lokalen Persistenz und dem Local-First-Paradigma zu unterscheiden.

## 2.4.1 Kernprinzipien nach Kleppmann et al.

Kleppmann et al. schlagen Local First als Gegenentwurf zu einem ausschließlich serverzentrierten Datenmodell vor.
Die lokale Kopie der Daten wird dabei als primäre Kopie behandelt.
Ein Server kann weiterhin existieren und sekundäre Kopien für Synchronisation, Datensicherung oder den Zugriff von weiteren Geräten bereitstellen.
Die Existenz eines Servers widerspricht dem Paradigma somit nicht, solange die lokale Anwendung nicht von dessen ständiger Verfügbarkeit abhängt und die lokale Kopie nicht lediglich als entbehrlicher Cache betrachtet wird~\cite{kleppmann2019localfirst}.

Die Autorinnen und Autoren formulieren sieben Ideale, anhand derer local-first-orientierte Anwendungen beurteilt werden können.
Diese Ideale sind nicht als technische Checkliste zu verstehen, die durch den Einsatz einer bestimmten Datenbank automatisch erfüllt wird.
Sie beschreiben vielmehr ein Zielbild für die Interaktion mit den eigenen Daten und machen sichtbar, welche Eigenschaften durch eine serverzentrierte Architektur typischerweise beeinträchtigt werden.

Das erste Ideal wird als „No spinners“ beziehungsweise als unmittelbare Verfügbarkeit der eigenen Arbeit beschrieben.
Lese- und Schreiboperationen sollen nicht von der Antwortzeit eines entfernten Servers abhängen.
Da die primäre Datenkopie lokal vorliegt, können Änderungen zunächst lokal verarbeitet und anschließend im Hintergrund synchronisiert werden.
Das reduziert die wahrgenommene Latenz und verhindert, dass alltägliche Interaktionen durch Netzwerkanfragen blockiert werden.
Die lokale Datenhaltung garantiert dabei nicht automatisch eine schnelle Anwendung, schafft jedoch die architektonische Voraussetzung dafür, dass die Benutzeroberfläche auf lokale Transaktionen reagieren kann.

Das zweite Ideal betrifft die Verfügbarkeit der Daten auf mehreren Geräten.
Local First bedeutet nicht, dass Daten dauerhaft auf ein einzelnes Endgerät eingeschlossen bleiben.
Eine Anwendung sollte die lokale Primärkopie auf weiteren Geräten verfügbar machen können, ohne die lokale Autonomie aufzugeben.
Dazu müssen Änderungen zwischen den Geräten übertragen und gegebenenfalls zusammengeführt werden.
Die Synchronisation ist deshalb nicht bloß ein technischer Kopiervorgang, sondern muss die Möglichkeit berücksichtigen, dass mehrere Geräte unabhängig voneinander Änderungen vornehmen.

Das dritte Ideal beschreibt das Netzwerk als optionale Infrastruktur.
Die nutzende Person soll Daten lesen und verändern können, wenn keine Internetverbindung besteht.
Nach Wiederherstellung der Verbindung können ausstehende Änderungen synchronisiert werden.
Dabei ist zwischen einer kurzen Unterbrechung der Verbindung und einem vollständig offlinefähigen System zu unterscheiden.
Eine Anwendung, die zwar bereits geladene Inhalte anzeigt, aber keine neuen Änderungen dauerhaft lokal speichern kann, erfüllt dieses Ideal nur eingeschränkt.
Offlinefähigkeit setzt daher ein lokales Datenmodell, lokale Geschäftsoperationen und eine Strategie für die spätere Verarbeitung ausstehender Änderungen voraus.

Das vierte Ideal ist die nahtlose Zusammenarbeit mehrerer Personen.
Local First soll nicht die Vorteile kollaborativer Cloud-Anwendungen aufgeben, sondern lokale Datenhoheit mit gemeinsamer Bearbeitung verbinden.
Hierfür müssen parallele Änderungen erkannt und möglichst automatisch zusammengeführt werden.
Bei strukturierten Daten ist eine rein textbasierte Zusammenführung oft unzureichend, weil die fachliche Bedeutung einer Änderung nicht allein aus der Zeichenfolge oder dem gespeicherten Datensatz hervorgeht.
Kollaboration ist deshalb eine zusätzliche Komplexitätsstufe und keine zwingende Voraussetzung für jede local-first-Anwendung.

Das fünfte Ideal, „The Long Now“, richtet den Blick auf die langfristige Erhaltung der Daten.
Eine nutzende Person sollte auf die eigenen Daten auch dann noch zugreifen können, wenn ein Anbieter seinen Dienst einstellt, Geschäftsbedingungen ändert oder eine bestimmte Serverkomponente nicht mehr betreibt.
Dafür müssen Daten in einem nachvollziehbaren Format gespeichert, exportiert und gegebenenfalls in zukünftige Schema-Versionen überführt werden können.
Die langfristige Verfügbarkeit hängt daher nicht nur von einer lokalen Datei ab, sondern auch von der Verfügbarkeit geeigneter Software, dokumentierter Formate und reproduzierbarer Migrationspfade.

Das sechste Ideal fordert Sicherheit und Datenschutz als standardmäßige Eigenschaften.
Durch die lokale Primärkopie muss nicht zwangsläufig eine zentrale Datenbank mit den Daten sämtlicher Nutzender betrieben werden.
Bei einer optionalen Synchronisation können zusätzliche Verfahren wie Ende-zu-Ende-Verschlüsselung die Einsichtsmöglichkeiten eines Servers begrenzen.
Local First ist jedoch nicht automatisch sicherer.
Die Daten liegen auf den Endgeräten und müssen dort gegen unberechtigten Zugriff, Schadsoftware, Verlust und unsichere Backups geschützt werden.
Die Verlagerung von Daten aus der Cloud auf das Gerät verschiebt daher einen Teil der Sicherheitsverantwortung in die Anwendung und auf das Betriebssystem des Endgeräts.

Das siebte Ideal beschreibt die letztendliche Kontrolle und den Besitz der Daten durch die nutzende Person.
Mit Besitz ist hier nicht das juristische Eigentum an den Inhalten gemeint, sondern die Möglichkeit, Daten unabhängig von den Vorgaben eines Dienstanbieters zu lesen, zu kopieren, zu exportieren, zu sichern und weiterzuverarbeiten.
Ein eingeschränktes Exportformat oder eine ausschließlich über proprietäre APIs erreichbare Datenbank kann diese Kontrolle trotz lokaler Zwischenspeicherung begrenzen.
Ein local-first-orientiertes System muss deshalb auch die Datenportabilität und die Nachvollziehbarkeit der Persistenz berücksichtigen.

Die sieben Ideale verdeutlichen, dass Local First über die Forderung „offline verfügbar“ hinausgeht.
Offlinefähigkeit, geringe Latenz und lokale Persistenz bilden die technische Grundlage.
Datensouveränität, langfristige Erhaltung, Datenschutz und gegebenenfalls Kollaboration beschreiben darüber hinaus die Auswirkungen auf die Nutzung und den Lebenszyklus der Daten.
Nicht jede Anwendung muss alle Ideale in gleichem Umfang erfüllen.
Ein lokaler Einzelbenutzer-Studienplaner benötigt beispielsweise keine Echtzeitkollaboration zwischen mehreren Personen, kann aber dennoch von lokaler Verfügbarkeit, kurzer Reaktionszeit, Datenkontrolle und langfristig lesbaren Daten profitieren.

Für die technische Umsetzung verteilter local-first-Anwendungen sind insbesondere Verfahren zur Zusammenführung paralleler Änderungen relevant.
Bei der Operational Transformation werden konkurrierende Operationen so angepasst, dass sie in einem gemeinsamen Zustand angewendet werden können.
Das Verfahren wurde insbesondere für kollaborative Editoren untersucht, setzt aber geeignete Operationsmodelle und Transformationsregeln voraus.
Conflict-free Replicated Data Types (CRDTs) verwenden dagegen Datenstrukturen und Änderungsoperationen, deren Zusammenführung unter bestimmten Voraussetzungen unabhängig von der Reihenfolge der Zustellung zu einem konvergenten Zustand führt~\cite{kleppmann2019localfirst}.
Dadurch kann ein Gerät Änderungen zunächst unabhängig von anderen Replikaten lokal ausführen.
Der Einsatz von CRDTs beseitigt jedoch weder jede fachliche Mehrdeutigkeit noch die Notwendigkeit eines geeigneten Datenmodells.
Wenn beispielsweise zwei Personen denselben fachlichen Wert auf inkompatible Weise verändern, muss die Anwendung festlegen, ob eine Änderung priorisiert, zusammengeführt oder der nutzenden Person zur Entscheidung vorgelegt wird.
Für eine reine Einzelplatzanwendung ohne Synchronisation sind CRDTs oder Operational Transformation dagegen nicht erforderlich.

## 2.4.2 Lokale Datenhaltung und Offline-First

Die zentrale technische Konsequenz des Local-First-Paradigmas ist die lokale Primärpersistenz.
Die Anwendung muss die für ihre Kernfunktionen erforderlichen Daten auf dem Endgerät dauerhaft speichern und lokale Lese- und Schreiboperationen ausführen können.
Ein flüchtiger UI-Zustand, ein Browser-Cache oder eine optimistische Darstellung einer noch nicht bestätigten Serveränderung genügt dafür nicht.
Erst eine lokale Persistenzschicht ermöglicht es, dass eine Änderung auch ohne Netzwerkverbindung als dauerhafter Bestandteil des lokalen Datenbestands behandelt wird.

Offline-First und Local First sind eng verwandte, aber nicht vollständig deckungsgleiche Begriffe.
Offline-First bezeichnet vor allem die Anforderung, dass die Anwendung auch bei fehlender Verbindung funktionsfähig bleibt und Netzwerkzugriffe nicht den primären Interaktionspfad bilden.
Local First umfasst zusätzlich die Frage, wem die Daten unterstehen, wie sie langfristig erhalten werden und wie mehrere lokale Kopien zusammenarbeiten können.
Eine Anwendung kann daher offlinefähig sein, ohne ihren Datenbestand in einer für die nutzende Person kontrollierbaren Form bereitzustellen.
Umgekehrt kann eine lokale Anwendung die Datenhoheit stärken, ohne eine Synchronisation mit mehreren Geräten anzubieten.

Für die lokale relationale Datenhaltung eignet sich SQLite als eingebettete Datenbank.
SQLite arbeitet als serverlose und konfigurationsarme Bibliothek innerhalb des Anwendungsprozesses und speichert eine vollständige relationale Datenbank in einer Datei.
Dadurch entfällt ein separat zu installierender und zu betreibender Datenbankserver.
SQL, Tabellen, Indizes, Fremdschlüssel und Transaktionen ermöglichen es, auch komplexere fachliche Beziehungen strukturiert abzubilden.
Die Datenbank unterstützt ACID-Transaktionen, sodass mehrere zusammengehörige Änderungen atomar verarbeitet und bei einem Fehler zurückgerollt werden können~\cite{sqlite2025about}.
Diese Eigenschaften machen SQLite für eine lokal ausgeführte Electron-Anwendung attraktiv, in der die Datenbank als Teil des installierten Anwendungssystems betrieben wird.

Die Eigenschaften von SQLite sind nicht mit einer vollständigen Datensicherungsstrategie gleichzusetzen.
Die Speicherung in einer einzelnen Datei vereinfacht zwar Kopieren, Export und Backup, schützt aber nicht automatisch vor Dateibeschädigung, versehentlichem Löschen, Ransomware oder dem Verlust des gesamten Endgeräts.
Backups müssen konsistent erstellt, gegen unberechtigten Zugriff geschützt und wiederherstellbar sein.
Ebenso ist zu entscheiden, ob die lokale Datenbank sensible Inhalte unverschlüsselt auf dem Datenträger ablegt oder ob zusätzliche Verschlüsselungs- und Schlüsselverwaltungsmechanismen erforderlich sind.
Diese Fragen gehören zur Sicherheits- und Betriebsbetrachtung der Zielanwendung und sind nicht allein durch die Wahl von SQLite gelöst.

Eine relationale lokale Datenbank benötigt außerdem ein explizit verwaltetes Schema.
Das Schema definiert Tabellen, Datentypen, Beziehungen, Integritätsbedingungen und Indizes, die von der Anwendung vorausgesetzt werden.
Wird die Anwendung weiterentwickelt, müssen bestehende Datenbestände auf die neue Struktur überführt werden.
Ein leerer Datenbankstand bei der Erstinstallation ist daher nicht der einzige relevante Fall.
Ebenso müssen Aktualisierungen mit bereits vorhandenen Studienplänen, Kursen, Prüfungen und weiteren relational verknüpften Datensätzen funktionieren.

Schema-Migrationen beschreiben diese Entwicklung als versionierte und reproduzierbare Änderungen.
Eine Migration kann beispielsweise eine Tabelle anlegen, eine Spalte ergänzen, einen Index erzeugen oder vorhandene Werte in ein neues Format überführen.
Bei komplexeren Änderungen sind Reihenfolge, Abhängigkeiten, Default-Werte, Datenübernahmen und die Wiederholbarkeit der einzelnen Schritte zu berücksichtigen.
Migrationen sollten möglichst in transaktionalen Grenzen ausgeführt werden, damit ein Fehler nicht zu einem teilweise aktualisierten Schema führt.
Die konkrete technische Umsetzung kann durch ein typgesichertes ORM wie Drizzle unterstützt werden, ersetzt aber nicht die fachliche Prüfung der erzeugten SQL-Änderungen.
Insbesondere bei lokalen Anwendungen müssen Migrationen mit dem Anwendungspaket ausgeliefert, beim Start kontrolliert angewendet und für alle unterstützten Ausgangsversionen getestet werden.

Damit wird die Schemaentwicklung zu einem Bestandteil des Anwendungslaufzeitsystems und nicht zu einer einmaligen Einrichtung durch eine Administration.
Die Zielanwendung muss erkennen können, welche Migrationen bereits ausgeführt wurden, und darf eine bestehende Datenbank nicht ohne nachvollziehbaren Grund durch eine leere Datenbank ersetzen.
Für die Fallstudie ist diese Eigenschaft relevant, weil die Ablösung der cloudbasierten Persistenz nicht nur eine Änderung des Datenbanktreibers bedeutet.
Sie erfordert ein lokales relationales Modell, eine Übernahme des bestehenden Datenbestands und einen kontrollierten Lebenszyklus für spätere Schemaänderungen.

Eine local-first-orientierte Anwendung kann unterschiedliche Synchronisationsstrategien verwenden.
Bei einem reinen Einzelbenutzer- und Einzelgerätszenario ist keine Synchronisation erforderlich.
Die lokale Datenbank bleibt die einzige autoritative Persistenz und kann durch lokale oder manuell angestoßene Backups ergänzt werden.
Bei mehreren Geräten kann ein Dienst als sekundäre Ablage eingesetzt werden, über den Änderungen zwischen den lokalen Datenbanken übertragen werden.
Alternativ sind Synchronisationen über ein lokales Netzwerk oder direkte Peer-to-Peer-Verbindungen denkbar.
Welche Strategie geeignet ist, hängt von Anforderungen an Geräteanzahl, Kollaboration, Datenschutz, Verfügbarkeit und Wiederherstellbarkeit ab.

Die Synchronisation darf nicht mit einer bloßen Kopie der gesamten Datenbank gleichgesetzt werden.
Werden zwei lokale Datenbanken unabhängig verändert, müssen Änderungen erkannt, übertragen und bei Bedarf zusammengeführt werden.
Ein einfaches „Last Write Wins“-Verfahren kann zwar technische Konflikte reduzieren, kann aber fachlich relevante Änderungen unbemerkt überschreiben.
Bei relationalen Daten muss deshalb festgelegt werden, ob auf Ebene ganzer Datensätze, einzelner Attribute oder fachlicher Operationen synchronisiert wird.
Für den Studienplaner ist zunächst zu begründen, ob eine solche Mehrgeräte- oder Mehrbenutzersynchronisation überhaupt Bestandteil des Zielartefakts ist.
Das Local-First-Paradigma schreibt diese Funktion nicht zwingend vor.

## 2.4.3 Abgrenzung zu Cloud-Native und traditionellen Desktop-Anwendungen

Die Unterschiede zwischen Local First, cloudzentrierten Anwendungen und traditionellen Desktop-Anwendungen lassen sich vor allem anhand der autoritativen Datenquelle und der Abhängigkeit vom Netzwerk erklären.
In der betrachteten cloudzentrierten Web-Referenzarchitektur liegt die dauerhaft maßgebliche Datenbasis im Backend.
Der Browser oder Web-Client verwaltet zwar UI-Zustand und möglicherweise einen Cache, fachliche Änderungen werden jedoch grundsätzlich an den Server übermittelt und dort persistiert.
Die Vorteile dieses Modells liegen unter anderem in der zentralen Verwaltung, der einfachen Bereitstellung neuer Versionen, der gemeinsamen Datenbasis und der unkomplizierten Zusammenarbeit über verschiedene Geräte.
Dem stehen Netzwerkabhängigkeit, Anbieterabhängigkeit, zentrale Angriffsziele und eingeschränkte Kontrolle über die gespeicherten Daten gegenüber.

Eine traditionelle Desktop-Anwendung führt Programm und Daten überwiegend lokal aus.
Sie kann deshalb ohne Netzwerkverbindung funktionieren und unmittelbar auf lokale Dateien oder eine eingebettete Datenbank zugreifen.
Historisch werden solche Anwendungen häufig mit nativen Technologien wie WinForms oder Windows Presentation Foundation umgesetzt.
Diese Technologien unterscheiden sich vom browserbasierten HTML-, CSS- und JavaScript-Modell der Ausgangsanwendung.
Eine traditionelle Desktop-Anwendung ist außerdem nicht automatisch local-first im umfassenden Sinn.
Sie kann zwar lokale Datenhoheit und Offlinefähigkeit bieten, aber weder mehrere Geräte synchronisieren noch eine nutzerfreundliche Zusammenarbeit oder einen transparenten Export unterstützen.

Local First kann daher als Hybrid zwischen den beiden Modellen verstanden werden.
Die lokale Anwendung verbindet die unmittelbare Interaktion und Datenkontrolle einer Desktop-Anwendung mit ausgewählten Vorteilen cloudbasierter Dienste, insbesondere Synchronisation, Backup und Kollaboration.
Der entscheidende Unterschied zur klassischen Cloud-Architektur liegt in der Reihenfolge und Autorität der Operationen.
Die lokale Kopie ist nicht nur ein Cache, der bei der nächsten Serverantwort verworfen werden kann, sondern die primäre Grundlage der fachlichen Interaktion.
Der Server ist optional und unterstützt die lokalen Kopien, anstatt sie vollständig zu ersetzen.

Für das Transformationsszenario bedeutet dies, dass die Wahl von Electron allein noch keine Local-First-Architektur erzeugt.
Electron stellt die Ausführungsumgebung und den kontrollierten Zugriff auf Betriebssystemressourcen bereit.
Erst die Kombination aus lokaler Datenbank, lokalem Datenzugriff, offlinefähiger Geschäftslogik und einem bewusst optionalen Netzwerkpfad verwirklicht die relevanten Local-First-Eigenschaften.
Die Preload- und IPC-Schicht ist dabei insbesondere für die sichere Vermittlung zwischen Web-UI und lokaler Persistenz verantwortlich.

## 2.4.4 Relevanz für das Transformationsszenario

Die Überführung des KI-generierten Web-Prototyps in eine local-first-orientierte Desktop-Anwendung verändert nicht nur den Speicherort der Daten.
Sie verändert die Autorität der Datenquelle, die Ausführungsreihenfolge der Geschäftsoperationen und die Verantwortung für Datenintegrität, Migration, Backup und Wiederherstellung.
Aus einer Anfrage an einen entfernten Dienst wird eine lokale Transaktion, die innerhalb der Anwendung ausgeführt und dauerhaft in SQLite gespeichert wird.
Der Netzwerkzugriff wird dadurch von einer notwendigen Voraussetzung zu einer optionalen Erweiterung, sofern keine Funktion ausdrücklich auf einen externen Dienst angewiesen ist.

Für die Transformation lassen sich mehrere architektonische Verschiebungen ableiten.
Erstens muss die bisher entfernte autoritative Persistenz durch eine lokale Primärpersistenz ersetzt werden.
Zweitens benötigt die Anwendung eine Datenzugriffsschicht, die fachliche Operationen auf das lokale relationale Modell abbildet, anstatt die Benutzeroberfläche direkt mit HTTP- oder BaaS-Aufrufen zu koppeln.
Drittens müssen Transaktionen und Integritätsbedingungen lokal nachvollziehbar umgesetzt werden.
Viertens muss entschieden werden, ob Identitäts-, Berechtigungs- und Mandantenkonzepte aus dem Cloud-System für das lokale Nutzungsszenario weiterhin erforderlich sind.
Fünftens sind Migrationen und Backups als Bestandteil des Anwendungslifecycles zu behandeln.

Auch die Benutzeroberfläche muss an die neue Zustandssemantik angepasst werden.
Ein Ladezustand, der in der Webanwendung auf die Antwort eines Servers wartet, ist bei einer lokalen Transaktion möglicherweise nicht mehr der primäre Interaktionsablauf.
Stattdessen muss die Anwendung lokale Erfolge, Validierungsfehler und gegebenenfalls ausstehende Synchronisationsvorgänge unterscheiden.
Optimistische UI-Aktualisierungen können bei einer lokalen Datenbanktransaktion teilweise durch eine unmittelbare lokale Persistierung ersetzt werden.
Sie bleiben jedoch relevant, wenn Änderungen später mit weiteren Geräten oder einem entfernten Dienst abgeglichen werden.

Die lokale Persistenz darf nicht direkt aus dem Renderer-Prozess heraus angesprochen werden.
Der Renderer verarbeitet weiterhin Web-Code und sollte keinen allgemeinen Dateisystem- oder Datenbankzugriff erhalten.
Stattdessen werden fachlich begrenzte Operationen über die in Abschnitt 2.2.3 beschriebene Preload- und IPC-Schnittstelle an den privilegierten Anwendungsteil vermittelt.
Die Eingaben müssen dort validiert und auf die erlaubten lokalen Operationen begrenzt werden.
Damit verbindet das Local-First-Zielbild die Datenarchitektur mit der Sicherheitsarchitektur der Electron-Anwendung.

Daraus folgt, dass die lokale Primärpersistenz, Offlinefähigkeit, schnelle lokale Interaktion, nachvollziehbare Schema-Migrationen und die Kontrolle über die eigenen Studienplandaten zentrale Ziele sind.
Die Arbeit kann somit die Local-First-Prinzipien auf die für den Anwendungsfall relevanten Eigenschaften anwenden.

Gleichzeitig entstehen durch die lokale Ausführung neue Verantwortlichkeiten.
Die Anwendung muss Datenbanken sicher anlegen und aktualisieren, Fehler bei Schreibvorgängen kontrolliert behandeln, Backups ermöglichen und mit beschädigten oder nicht unterstützten Datenbankständen umgehen.
Bei einer späteren Synchronisation müssten zusätzlich Konfliktregeln, Authentifizierung, Verschlüsselung und Wiederholbarkeit von Änderungen entworfen werden.
Die Transformation reduziert damit zwar die direkte Abhängigkeit vom Cloud-Anbieter, sie beseitigt aber nicht die Komplexität der Datenhaltung.
Sie verlagert einen Teil dieser Komplexität in die lokale Anwendung, wo sie explizit modelliert, getestet und abgesichert werden muss.

Das Local-First-Paradigma liefert für die vorliegende Arbeit somit ein architektonisches Zielbild und zugleich Bewertungskriterien für die Transformation.
Eine erfolgreiche Überführung besteht nicht darin, das Web-Frontend lediglich als installierbare Datei zu verpacken.
Erforderlich ist vielmehr eine lokale Daten- und Ausführungsarchitektur, in der die zentralen fachlichen Funktionen ohne Netzwerkverbindung verfügbar sind, Daten kontrollierbar gespeichert werden und spätere Erweiterungen wie Synchronisation nicht durch eine unklare Datenhaltung verhindert werden.
Die daraus entstehenden Sicherheitsanforderungen an lokale Daten, IPC, Dateisystemzugriffe und Abhängigkeiten werden in Abschnitt 2.3 aus Sicht des Bedrohungsmodells weiter untersucht.

## Quellenhinweise für die spätere Überführung

Die zentrale Quelle für die sieben Ideale, die Abgrenzung zu cloudzentrierten Anwendungen sowie die Einordnung von CRDTs ist:

Kleppmann, M., Wiggins, A., van Hardenberg, P. und McGranaghan, M. (2019): „Local-first software: You own your data, in spite of the cloud“, Proceedings of the 2019 ACM SIGPLAN International Symposium on New Ideas, New Paradigms, and Reflections on Programming and Software, S. 154--178, DOI: 10.1145/3359591.3359737.

Für die technischen Eigenschaften von SQLite sollte die offizielle SQLite-Dokumentation als Primärquelle in `thesis/refs.bib` ergänzt werden.
Die im Draft verwendeten Schlüssel `kleppmann2019localfirst` und `sqlite2025about` sind daher als vorläufige Schlüssel zu verstehen und müssen vor der Überführung in die LaTeX-Fassung mit den tatsächlich angelegten Bibliographieeinträgen abgeglichen werden.
