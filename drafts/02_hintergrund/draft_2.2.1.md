# 2.2.1 Charakteristika moderner Web-Architekturen

Dieser Abschnitt beschreibt die architektonischen Grundlagen moderner Web-Anwendungen, wie sie typischerweise von KI-gestützten Entwicklungswerkzeugen erzeugt werden. Die in diesem Abschnitt identifizierten Merkmale bilden den Ausgangszustand, der im weiteren Verlauf systematisch transformiert wird.

## Request-Response-Modell und Statelessness

Das Fundament jeder Web-Anwendung ist das Request-Response-Modell: eine Nutzerin oder ein Nutzer löst über den Browser eine HTTP-Anfrage aus, ein Server verarbeitet diese, greift ggf.\ auf eine Datenbank zu und liefert eine Antwort zurück — typischerweise als HTML-Seite, JSON-Daten oder eine Kombination beider Formate~\cite{MDN2026ClientServer}.

Dieses Modell basiert auf HTTP als zustandslosem (\emph{stateless}) Protokoll. Ein zustandsloses Protokoll bedeutet, dass jeder Request als eigenständiger Vorgang behandelt wird, der nicht auf vorherigen Interaktionen aufbaut~\cite{Wikipedia2026Stateless}. Das Server-seitige Programm hat daher keine Information über vorher von diesem Client gesendete Anfragen und startet mit jedem Request quasi bei Null. Um dennoch Benutzersessions zu ermöglichen, werden Zustandsinformationen in Form von Cookies oder Tokens auf der Client-Seite gespeichert und bei jedem Request erneut an den Server gesendet — ein Muster, das als \emph{stateful protocol} auf Application-Level bezeichnet wird, obwohl das zugrunde liegende HTTP-Protokoll stateless bleibt~\cite{Wikipedia2026State}.

Diese Statelessness hat wesentliche Vorteile für die Skalierbarkeit, da Server-Anfragen unabhängig voneinander auf beliebige Server-Instanzen verteilt werden können, ohne dass Session-State synchronisiert werden muss. Sie bedeutet zugleich, dass sämtliche anwendungsspezifische Zustände — Benutzerpräferenzen, Bearbeitungsschritte, Formulardaten — externalisiert werden müssen: Entweder auf dem Client (via Cookies, Local Storage oder Session Storage) oder server-seitig in einer Datenbank oder einem In-Memory-Cache wie Redis.

## Der Browser als dünne Präsentationsschicht

In der klassischen Web-Architektur erfüllt der Browser die Rolle eines \emph{Thin Clients}: Er ist primär für die Darstellung von Inhalten und die Abwicklung von Netzwerkkommunikation zuständig, während die Geschäftslogik und die Datenpersistenz vollständig auf der Server-Seite resideieren~\cite{Wikipedia2026ThinClient}. Der Browser selbst enthält keinen persistenten Zustand — beim Schließen oder Neuladen einer Seite verschwindet alle lokale State-Information, außer den explizit persistierten Daten wie Cookies oder Local Storage-Einträge.

Dieses Muster hat sich durch die Einführung von \emph{Single-Page Applications} (\emph{SPA}) im Laufe der 2010er-Jahre grundlegend gewandelt. Frameworks wie React, Angular und Vue.js verlagern einen erheblichen Teil der Darstellungslogik in den Browser und laden zunächst ein minimales HTML-Gerüst, das anschließend durch JavaScript dynamisch mit Inhalten gefüllt wird~\cite{MDN2026JSFramework}. Die Datenanfrage an den Server erfolgt seither asynchron über \emph{AJAX} (\emph{Asynchronous JavaScript and XML}) oder modernere Schnittstellen wie die Fetch API, wodurch die Seite nicht bei jeder Datenaktualisierung komplett neu geladen werden muss.

Trotz dieser Client-seitigen Render-Logik bleibt die Architektur im Kern stateless: Browser und Frontend-Framework verwalten nur eine visuelle Repräsentation des Applikationszustands, während die autoritative Datenquelle ausschließlich auf dem Server existiert. Bei einem Seiten-Reload oder Neustart des Browsers muss dieser Zustand vollständig aus dem Server neu aufgebaut werden — ein Umstand, der für die nachfolgende Transformation in eine lokal ausgeführte Anwendung von zentraler Bedeutung ist, da er die Cloud-Abhängigkeit als konstitutiv für die Web-Architektur outet.

## Cloud-Backend als Single Source of Truth

Die Datenhaltung moderner Web-Anwendungen erfolgt durchgängig server-seitig in einer Datenbank, die als \emph{Single Source of Truth} fungiert. Alle Lese- und Schreiboperationen werden über Netzwerk-Requests erledigt, die typischerweise nach einem RESTful- oder GraphQL-API-Muster strukturiert sind.

Im Kontext KI-generierter Web-Prototypen kommt dabei häufig ein \gls{baas} (\emph{Backend as a Service}) wie Supabase, Firebase oder die integrierten Datenbankservices von Replit zum Einsatz. Diese Dienste abstrahieren die Datenbankadministration vollständig und stellen direkte Client-seitige Schnittstellen bereit, die ein Frontend-Framework ohne explizite server-seitige API-Schicht direkt ansprechen kann~\cite{lovable2026docs}. Dieses Muster reduziert die initiale Entwicklingskomplexität erheblich, verlagert jedoch Sicherheitsverantwortung — insbesondere bzgl.\ Zugriffskontrollen und Datenvalidierung — vollständig auf die Nutzenden, was empirisch wiederholt als Schwachstelle identifiziert wurde~\cite{albergotti2025lovable}.

Unabhängig von der konkreten Implementierung teilt dieses Architekturmuster folgende charakteristische Eigenschaften:

\begin{itemize}
  \item \textbf{Zentrale Datenhaltung}: Alle Daten resideieren auf einem entfernten Server, der Browser kennt keine lokale Persistenzschicht.
  \item \textbf{Client-Server-Kopplung}: Die Applikation ist funktional nur so lange nutzbar, wie eine verbindliche Netzwerkverbindung zum Server besteht.
  \item \textbf{Echtzeit-Synchronisation}: Jede Datenänderung durch eine Nutzerin oder einen Nutzer wird sofort auf dem Server persistiert und steht damit allen anderen Nutzerinnen und Nutzern unmittelbar zur Verfügung.
  \item \textbf{Authentifizierung als Gateway}: Der Server verwaltet Benutzeridentitäten und Access-Controls; der Browser überträgt diese Informationen reprimär durch Weitergabe von Session-Tokens.
\end{itemize}

## Abhängigkeiten und Technologie-Stack

Eine moderne Web-Anwendung ist typischerweise durch eine Kette spezialisierter Abhängigkeiten geprägt, die sich über mehrere Schichten erstrecken. Für den in dieser Arbeit betrachteten Ausgangszustand (KI-generierter Web-Prototyp auf Basis von React und einem Cloud-Backend) lässt sich diese Typisierung wie folgt zusammenfassen:

\begin{itemize}
  \item \textbf{Netzwerkprotokolle}: HTTP/HTTPS als Transport, JSON als Datenaustauschformat. GraphQL ist eine verbreitete Alternative zum REST-Paradigma, die eine flexiblere Datenabfrage ermöglicht, aber ähnliche architecturische Konsequenzen hat.
  \item \textbf{Cloud-Datenbanken}: Firebase (NoSQL), Supabase (PostgreSQL), Replit DB (MongoDB-ähnlich) oder verwaltete PostgreSQL/MySQL-Instanzen. Alle bieten identifikationsbasierte Authentifizierung und cloudseitige Datenpersistenz.
  \item \textbf{Frontend-Frameworks}: React, Vue, Angular oder Svelte, typischerweise ausgeführt über Vite oder Next.js als Build-Tool bzw. Full-Stack-Framework. Diese Frameworks verwalten lokalen UI-State via State-Management-Lösungen wie Redux, Zustand oder React Context.
  \item \textbf{State-Management im Browser}: TanStack Query (früher React Query) als beliebte Bibliothek zur Client-seitigen Daten-Cache-Verwaltung, die server-seitige Daten im Browser puffert und automatische Refetch-Logik bietet. Dieser Cache ist flüchtig — er überlebt einen Seiten-Reload nicht.
\end{itemize}

Diese Abhängigkeitskette ist nicht willkürlich, sondern resultiert aus der grundlegenden Architektur-Entscheidung, Datenhaltung und Geschäftslogik server-seitig zu zentralisieren und den Browser als reine Präsentationsschicht zu behandeln.

## Probleme für die lokale Ausführung

Die in den vorangegangenen Abschnitten beschriebenen architektonischen Merkmale stellen für eine Migration in den lokalen Desktop-Kontext ein konsistentes Muster von Herausforderungen dar, die im weiteren Verlauf dieser Arbeit adressiert werden:

\begin{itemize}
  \item \textbf{Keine Offline-Fähigkeit}: Das Request-Response-Modell setzt eine permanente Netzwerkverbindung voraus. Ohne Server-Konnektivität ist die Applikation nicht funktionsfähig — ein gravierendes Defizit im mobilen und heterogenen Desktop-Umfeld.
  \item \textbf{Keine lokale State-Verwaltung}: Zustandsinformationen, die sich typischerweise im Browser-Cache oder auf dem Server befinden, müssen bei einer lokalen Ausführung explizit in einer eingebetteten Datenbank wie SQLite verwaltet werden.
  \item \textbf{Cloud-Backend-Kopplung}: Die direkte Kommunikation mit einem Cloud-Backend über REST/GraphQL-APIs kann nicht einfach durch eine lokale Datenbank ersetzt werden — dafür sind architektonische Anpassungen der Datenzugriffsschicht erforderlich.
  \item \textbf{Authentifizierungsansatz}: Die server-seitige Authentifizierung über Session-Tokens oder OAuth-Flows muss durch lokale Benutzerregistrierung und -verwaltung ersetzt werden.
\end{itemize}

Diese Transformation — also der Übergang von einer cloud-zentrierten, stateless Web-Architektur zu einer lokal ausgeführten, persistenten Desktop-Anwendung — ist das zentrale Themenfeld der folgenden Abschnitte dieses Kapitels. Unterabschnitt~\ref{sec:desktop-runtimes} stellt Electron als Brückentechnologie vor, die Web-Technologien auf dem Desktop ermöglicht. Unterabschnitt~\ref{sec:electron-security} diskutiert die daraus resultierenden sicherheitsrelevanten Konsequenzen und das Transformationsziel Kapitel~\ref{sec:local-first} führt das Local-First-Paradigma als Architekturmuster ein, das explizit die Probleme der Cloud-Web-Architektur adressiert.