# 2.2.1 Referenzarchitektur KI-generierter Web-Anwendungen

Dieser Abschnitt beschreibt die in dieser Arbeit betrachtete Referenzarchitektur: einen KI-generierten, cloud-zentrierten Web-Prototyp auf Basis eines clientseitigen Frontend-Frameworks und eines entfernten Backends. Die dargestellten Merkmale sind daher nicht als allgemeingültige Eigenschaften sämtlicher moderner Web-Anwendungen zu verstehen. Moderne Web-Anwendungen können ebenso serverseitig gerendert, offlinefähig, local-first oder mit lokaler Persistenz über Browsertechnologien umgesetzt werden. Im Mittelpunkt steht hier vielmehr der Ausgangszustand, der im weiteren Verlauf systematisch in eine lokal ausgeführte Desktop-Anwendung transformiert wird.

## Request-Response-Modell und Zustandssemantik

Das Request-Response-Modell bildet eine wesentliche Grundlage vieler Web-Anwendungen: Eine Nutzerin oder ein Nutzer löst über den Browser eine HTTP-Anfrage aus, ein Server verarbeitet diese, greift gegebenenfalls auf eine Datenbank zu und liefert eine Antwort zurück, beispielsweise in Form einer HTML-Seite oder von JSON-Daten~\cite{MDN2026ClientServer}. Bei einer Single-Page Application (SPA) wird die Benutzeroberfläche nach dem initialen Laden typischerweise clientseitig aktualisiert. Weitere Daten werden asynchron über die Fetch API oder vergleichbare Schnittstellen abgerufen, ohne dass die gesamte Seite neu geladen werden muss~\cite{MDN2026JSFramework}.

HTTP stellt zwischen zwei Anfragen nicht automatisch einen Anwendungskontext bereit. Jeder Request muss daher grundsätzlich alle Informationen enthalten, die zu seiner Zuordnung und Verarbeitung erforderlich sind. Daraus folgt jedoch nicht, dass eine serverseitige Anwendung keinerlei Sitzungszustand verwalten kann. Ein Client kann beispielsweise ein Session-Cookie oder ein Token mitsenden, über das der Server auf einen zentral gespeicherten Sitzungszustand zugreift. Auf Anwendungsebene entsteht dadurch eine zustandsbehaftete Sitzung, obwohl HTTP selbst keinen sitzungsübergreifenden Zustand verwaltet~\cite{Wikipedia2026Stateless,Wikipedia2026State}.

Auch die Skalierbarkeit hängt nicht allein vom verwendeten Protokoll ab. Zustandslose Anwendungsinstanzen erleichtern die horizontale Skalierung, da Requests prinzipiell auf beliebige Instanzen verteilt werden können. Voraussetzung ist, dass sitzungsbezogener Zustand entweder im Request enthalten oder in einer gemeinsam erreichbaren Persistenzschicht abgelegt wird. Ein zentraler Session-Store vermeidet zwar eine Synchronisation zwischen den Anwendungsinstanzen, bleibt aber eine gemeinsam genutzte Abhängigkeit.

## Browser als Präsentations- und Anwendungsschicht

In klassischen serverzentrierten Web-Architekturen wird der Browser überwiegend als Präsentationsschicht eingesetzt. Moderne Browser stellen jedoch verschiedene Mechanismen zur lokalen Persistenz bereit, darunter Cookies, Web Storage, IndexedDB und die Cache API. Ob und in welchem Umfang diese Mechanismen genutzt werden, hängt von der jeweiligen Anwendungsarchitektur ab~\cite{Wikipedia2026ThinClient}.

SPAs haben die Aufgabenverteilung zwischen Client und Server zusätzlich verschoben. Frameworks wie React, Angular und Vue.js verlagern einen erheblichen Teil der Darstellungslogik in den Browser. Das Frontend verwaltet typischerweise UI-Zustand und kann darüber hinaus Teile der Validierungs-, Workflow- und Geschäftslogik ausführen. Hinzu kommen beispielsweise optimistische Aktualisierungen, clientseitige Berechnungen und lokale Caches.

Für die hier betrachtete Referenzarchitektur verbleibt die autoritative, dauerhaft persistierte Datenbasis jedoch im Cloud-Backend. Flüchtiger UI-Zustand oder ein clientseitiger Cache stellen dabei keine dauerhafte lokale Datenhaltung dar. Nach einem Seiten-Reload oder Neustart muss der für die Anwendung benötigte Datenbestand daher erneut aus dem Backend geladen werden, sofern keine zusätzliche Browserpersistenz implementiert wurde. Diese Abhängigkeit ist für die nachfolgende Transformation relevant, stellt aber keine allgemeine Eigenschaft von Web-Architekturen dar.

## Cloud-Backend und autoritative Datenquelle

In der hier betrachteten cloud-zentrierten Referenzarchitektur erfolgt die dauerhafte Datenhaltung überwiegend in einer entfernten Datenbank. Lese- und Schreiboperationen werden über Netzwerk-Schnittstellen ausgeführt, die beispielsweise nach einem REST- oder GraphQL-Muster strukturiert sein können. Die entfernte Datenbank dient dabei als autoritative Datenquelle für die dauerhaft gespeicherten Anwendungsdaten.

Im Kontext KI-generierter Web-Prototypen können Backend-as-a-Service-Angebote (BaaS) wie Supabase oder Firebase sowie integrierte Datenbankdienste von Entwicklungsplattformen zum Einsatz kommen. Diese Dienste reduzieren die initiale Entwicklungskomplexität, indem sie Datenbankbetrieb, Authentifizierung und teilweise auch clientseitige Zugriffsschnittstellen abstrahieren~\cite{lovable2026docs}. Das daraus entstehende Sicherheitsmodell beruht jedoch auf geteilter Verantwortung. Der Anbieter verantwortet unter anderem den Plattformbetrieb und technische Basismechanismen. Das Entwicklungsteam muss dagegen Datenmodell, Security Rules beziehungsweise Row-Level-Security-Regeln, Rollen, Zugriffskontrollen und die sichere Clientkonfiguration korrekt gestalten. Fehlkonfigurationen können dazu führen, dass Clients unzulässig auf Daten zugreifen~\cite{albergotti2025lovable}.

Für die Referenzarchitektur lassen sich folgende Merkmale zusammenfassen:

\begin{itemize}
  \item \textbf{Entfernte autoritative Persistenz}: Der dauerhaft gespeicherte Datenbestand liegt überwiegend in einer entfernten Datenbank. Eine lokale Browserpersistenz ist nicht zwingend vorhanden.
  \item \textbf{Client-Server-Kopplung}: Zentrale Daten und Funktionen sind über Backend-Dienste erreichbar. Ohne Netzwerkverbindung können die davon abhängigen Funktionen nicht oder nur eingeschränkt genutzt werden.
  \item \textbf{Bedingte Synchronisation}: Datenänderungen werden über das Backend persistiert. Andere Clients erhalten sie je nach technischer Umsetzung bei einer erneuten Abfrage oder über Echtzeitmechanismen wie WebSockets beziehungsweise Subscriptions. Eine unmittelbare Verfügbarkeit ist daher nicht automatisch gegeben.
  \item \textbf{Zentrale Identitätsverwaltung}: Benutzeridentitäten und Zugriffskontrollen werden typischerweise durch den Backend-Dienst verwaltet. Der Browser überträgt die zur Authentifizierung erforderlichen Informationen beispielsweise über Cookies oder Session-Tokens.
\end{itemize}

Der Begriff \emph{Single Source of Truth} ist in diesem Zusammenhang nur präzise zu verwenden. Er kann sich auf die autoritative Persistenz, den UI-Zustand, einen Cache oder die Identitätsverwaltung beziehen. Im Folgenden ist mit der autoritativen Datenquelle ausschließlich der dauerhaft gespeicherte, fachlich maßgebliche Datenbestand gemeint.

## Abhängigkeiten und Technologie-Stack

Die beschriebene Web-Anwendung beruht auf einer Kette spezialisierter Abhängigkeiten. Diese Auflistung dient nicht als vollständige Beschreibung moderner Webtechnologien, sondern typisiert die für den betrachteten Ausgangszustand relevanten Schichten:

\begin{itemize}
  \item \textbf{Netzwerkprotokolle und Datenformate}: HTTP beziehungsweise HTTPS dienen der Kommunikation. JSON ist ein verbreitetes Datenaustauschformat. GraphQL stellt eine Alternative zu REST dar, verändert jedoch nicht die grundsätzliche Abhängigkeit von einer erreichbaren Backend-Schnittstelle.
  \item \textbf{Frontend-Frameworks und UI-Bibliotheken}: React, Vue, Angular und Svelte unterstützen die Umsetzung der Benutzeroberfläche. Sie können durch Build- und Entwicklungswerkzeuge wie Vite oder durch Meta-Frameworks wie Next.js ergänzt werden.
  \item \textbf{Client-State-Management}: Redux, Zustand oder React Context verwalten flüchtigen UI- und Anwendungszustand innerhalb des Clients.
  \item \textbf{Server-State-Management}: Bibliotheken wie TanStack Query verwalten entfernte Daten im Client, puffern Antworten und unterstützen automatische Aktualisierungen. Der Cache ist standardmäßig flüchtig, kann jedoch durch zusätzliche Persistenzmechanismen über Seitenneuladungen hinweg gespeichert werden.
  \item \textbf{Persistenz und Backenddienste}: Firebase, Supabase oder verwaltete Datenbanken stellen entfernte Persistenz- und Backendfunktionen bereit. Die konkrete technische Einordnung einzelner Plattformdienste ist versionsabhängig und muss anhand der jeweiligen Dokumentation geprüft werden.
\end{itemize}

Die Unterscheidung zwischen Client-State und Server-State ist für die spätere Migration wesentlich. Ein lokaler UI-State beschreibt beispielsweise, ob ein Dialog geöffnet ist. Server-State bezeichnet dagegen Daten, deren fachliche Autorität zunächst beim Backend liegt. Eine lokale Desktop-Anwendung benötigt für letztere Kategorie eine eigene dauerhafte Persistenz- und Zugriffsschicht, wenn sie unabhängig vom Netzwerk funktionieren soll.

## Konsequenzen für die lokale Ausführung

Aus den beschriebenen Eigenschaften ergeben sich für die Transformation mehrere voneinander zu unterscheidende Aufgaben:

\begin{itemize}
  \item \textbf{Eingeschränkte Offlinefähigkeit}: Nicht das Request-Response-Modell selbst setzt eine permanente Netzwerkverbindung voraus. Problematisch ist die ausschließliche Abhängigkeit von entfernten Ressourcen. Sind Daten und zentrale Funktionen nur über Backend-Dienste verfügbar, können die betreffenden Funktionen ohne Netzwerkverbindung nicht oder nur eingeschränkt genutzt werden.
  \item \textbf{Fehlende dauerhafte lokale Persistenz}: Flüchtiger UI-State und clientseitige Caches ersetzen keine eingebettete Persistenzschicht. Für die lokale Ausführung muss daher ein lokales Datenmodell, beispielsweise für SQLite, entwickelt und in die Anwendung integriert werden.
  \item \textbf{Entkopplung vom Cloud-Backend}: Die direkte Kommunikation mit REST-, GraphQL- oder BaaS-Schnittstellen kann nicht ohne Weiteres durch eine lokale Datenbank ersetzt werden. Erforderlich sind Anpassungen der Datenzugriffsschicht, der Transaktionslogik und gegebenenfalls der Synchronisationsmechanismen.
  \item \textbf{Anpassung des Identitätsmodells}: Es ist zu entscheiden, ob die bestehende cloudbasierte Authentifizierung beibehalten, durch eine lokale Benutzerverwaltung ersetzt oder für einen Einzelbenutzerbetrieb vollständig entfernt wird. Eine lokale Ausführung erzwingt nicht automatisch eine lokale Benutzerregistrierung.
\end{itemize}

Die lokale Ausführung einer Anwendung ist nicht automatisch mit einer Local-First-Architektur gleichzusetzen. Eine Desktop-Anwendung kann weiterhin ausschließlich auf ein Cloud-Backend zugreifen. Local First bezeichnet dagegen ein Datenhaltungs- und Synchronisationsmodell, bei dem lokale Datenbestände die primäre Grundlage der Interaktion bilden und Netzwerkzugriffe nicht für jede Operation erforderlich sind. Umgekehrt kann auch eine Browseranwendung local-first und offlinefähig umgesetzt werden.

Die hier untersuchte Transformation bezeichnet somit den Übergang von einer cloud-zentrierten Webanwendung zu einer lokal ausgeführten Desktop-Anwendung mit dauerhafter lokaler Persistenz. Ob daraus zusätzlich ein Local-First-System entsteht, hängt von der konkreten Ausgestaltung der Datenhaltung und der Synchronisation ab. Unterabschnitt~\ref{sec:desktop-runtimes} stellt Electron als Brückentechnologie für die Ausführung von Webtechnologien auf dem Desktop vor. Unterabschnitt~\ref{sec:electron-security} diskutiert die daraus entstehenden sicherheitsrelevanten Konsequenzen. Kapitel~\ref{sec:local-first} führt anschließend das Local-First-Paradigma als mögliches architektonisches Zielbild ein.
