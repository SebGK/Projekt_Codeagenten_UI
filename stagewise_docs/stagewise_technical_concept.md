# Technisches Konzept: ThreadBridge
# Gemeinsames Frontend für Cursor und Windsurf mit Threading

## 1. Einführung

Dieses Dokument beschreibt das technische Konzept für "ThreadBridge", ein Frontend-Tool, das die Zusammenarbeit der KI-Code-Agenten Cursor und Windsurf in einem gemeinsamen Thread ermöglicht. Es baut auf der Analyse des Stagewise-Projekts auf und erweitert dessen Architektur, um Multi-Agenten-Fähigkeiten und Thread-Synchronisation zu integrieren.

## 2. Architekturübersicht

ThreadBridge folgt einer modularen Architektur, die aus folgenden Hauptkomponenten besteht:

1.  **Browser-Toolbar (Client)**: Eine in Web-Anwendungen integrierbare UI-Komponente zur Interaktion mit dem Benutzer (DOM-Auswahl, Kommentare, Agenten-Steuerung).
2.  **VSCode-Extension (Server/Proxy)**: Läuft in VSCode, dient als zentrale Kommunikationsschnittstelle und Proxy zu den KI-Agenten. Verwaltet die Agenten-Verbindungen und den gemeinsamen Thread.
3.  **Kommunikationsschicht (SRPC-Erweiterung)**: Basiert auf dem SRPC-Framework von Stagewise, erweitert um Multi-Agenten-Unterstützung und Thread-Management-Funktionen.
4.  **Thread-Manager**: Eine Komponente innerhalb der VSCode-Extension, verantwortlich für die Verwaltung des gemeinsamen Threads, die Kontextsynchronisation und das Routing von Nachrichten.
5.  **Agenten-Adapter**: Spezifische Module innerhalb der VSCode-Extension zur Kommunikation mit den APIs von Cursor und Windsurf.
6.  **Kontext-Speicher**: Verwaltet die Speicherung und Versionierung von Kontexten und Thread-Daten.

```mermaid
graph LR
    subgraph Browser
        Toolbar[Browser-Toolbar]
    end
    subgraph VSCode
        Extension[VSCode-Extension]
        subgraph Extension Components
            CommLayer[Kommunikationsschicht (SRPC)]
            ThreadMgr[Thread-Manager]
            CtxStore[Kontext-Speicher]
            AdapterCursor[Agenten-Adapter: Cursor]
            AdapterWindsurf[Agenten-Adapter: Windsurf]
        end
    end
    subgraph KI-Agenten
        Cursor[Cursor Agent]
        Windsurf[Windsurf Agent]
    end

    Toolbar -- SRPC --> CommLayer
    CommLayer <--> ThreadMgr
    ThreadMgr -- Verwaltet --> CtxStore
    ThreadMgr -- Routet an --> AdapterCursor
    ThreadMgr -- Routet an --> AdapterWindsurf
    AdapterCursor -- API Call --> Cursor
    AdapterWindsurf -- API Call --> Windsurf
```

## 3. Komponenten im Detail

### 3.1 Browser-Toolbar
- **Technologie**: TypeScript, Preact/React (oder Framework-spezifische Wrapper wie bei Stagewise), CSS.
- **Funktionen**: DOM-Inspektion, Elementauswahl, Kommentarfunktion, Agenten-Auswahl-UI, Thread-Anzeige, Kontext-Management-UI.
- **Kommunikation**: Nutzt den erweiterten SRPC-Client zur Kommunikation mit der VSCode-Extension über WebSockets.
- **Integration**: Wird als NPM-Paket bereitgestellt und in die Entwicklungsversion der Zielanwendung injiziert.

### 3.2 VSCode-Extension
- **Technologie**: TypeScript, Node.js (im VSCode Extension Host).
- **Funktionen**: Stellt den SRPC-Server bereit, hostet den Thread-Manager, die Agenten-Adapter und den Kontext-Speicher. Verwaltet den Lebenszyklus der Agenten-Verbindungen.
- **Kommunikation**: Implementiert den erweiterten SRPC-Server. Kommuniziert mit Cursor (vermutlich über MCP oder eine spezifische API) und Windsurf (über deren jeweilige API).

### 3.3 Kommunikationsschicht (SRPC-Erweiterung)
- **Basis**: `@stagewise/srpc` und `@stagewise/extension-toolbar-srpc-contract`.
- **Erweiterungen**: 
    - Definition neuer RPC-Methoden für Multi-Agenten-Steuerung (z.B. `sendToAgent(agentId, payload)`, `sendToAllAgents(payload)`).
    - Methoden für Thread-Management (z.B. `createThread`, `getThreadHistory`, `postMessageToThread`).
    - Methoden für Kontext-Synchronisation (z.B. `updateContext`, `getContext`).
    - Unterstützung für Broadcast-Nachrichten vom Server an den Client (Toolbar) für Thread-Updates.
- **Protokoll**: WebSocket-basiert, mit Zod für Schema-Validierung.

### 3.4 Thread-Manager
- **Verantwortlichkeiten**: 
    - Erstellung und Verwaltung von Thread-Instanzen.
    - Speicherung der Thread-Historie (Nachrichten, Antworten, Kontextänderungen).
    - Routing von Benutzeranfragen an die ausgewählten Agenten (einzeln oder beide).
    - Empfang von Antworten der Agenten und Weiterleitung an die Toolbar.
    - Orchestrierung der Kontextsynchronisation zwischen Agenten.
- **Implementierung**: Als Klasse oder Modul innerhalb der VSCode-Extension.

### 3.5 Agenten-Adapter
- **Zweck**: Abstraktion der spezifischen Kommunikationsdetails für jeden Agenten.
- **Implementierung**: Separate Module für Cursor und Windsurf.
    - **Cursor-Adapter**: Implementiert die Kommunikation über das MCP-Protokoll oder eine andere verfügbare Cursor-API.
    - **Windsurf-Adapter**: Implementiert die Kommunikation über die Windsurf-API.
- **Funktionen**: Senden von Anfragen, Empfangen von Antworten, Behandlung von agentenspezifischen Fehlern, Statusüberwachung.

### 3.6 Kontext-Speicher
- **Verantwortlichkeiten**: Speicherung, Versionierung und Abruf von Kontextdaten (DOM-Ausschnitte, Kommentare, Screenshots, Thread-Verlauf).
- **Speicherort**: 
    - Lokal: Im Workspace-Storage von VSCode oder in einem dedizierten Verzeichnis.
    - Optional Cloud: Integration mit Cloud-Speicherdiensten (z.B. über eine separate API).
- **Datenmodell**: Definiert Strukturen für Kontexte, Threads, Nachrichten und Versionen.

## 4. Kernmechanismen

### 4.1 Gemeinsamer Thread
1.  **Erstellung**: Benutzer initiiert einen neuen Thread über die Toolbar.
2.  **Nachricht senden**: Benutzer sendet eine Nachricht über die Toolbar. SRPC-Call an `postMessageToThread`.
3.  **Routing**: Thread-Manager empfängt die Nachricht und leitet sie basierend auf der Agenten-Auswahl an die entsprechenden Agenten-Adapter weiter.
4.  **Agenten-Antwort**: Agenten-Adapter senden die Anfrage an die Agenten und empfangen die Antworten.
5.  **Update**: Agenten-Adapter leiten die Antworten an den Thread-Manager weiter.
6.  **Broadcast**: Thread-Manager speichert die Antwort im Thread-Verlauf und sendet ein Update (Broadcast) über SRPC an die Toolbar.
7.  **Anzeige**: Toolbar empfängt das Update und zeigt die neue Nachricht/Antwort im Thread an.

### 4.2 Kontextsynchronisation
1.  **Kontextänderung**: Benutzer wählt ein neues Element aus oder fügt einen Kommentar hinzu.
2.  **Update senden**: Toolbar sendet ein `updateContext`-Event über SRPC an die Extension.
3.  **Verarbeitung**: Thread-Manager empfängt das Update, aktualisiert den Kontext im Kontext-Speicher und versioniert ihn.
4.  **Weiterleitung**: Thread-Manager leitet den aktualisierten Kontext an die Agenten-Adapter der aktiven Agenten weiter.
5.  **Agenten-Update**: Agenten-Adapter senden den aktualisierten Kontext an die jeweiligen Agenten über deren APIs.

### 4.3 Agenten-Management
- Die VSCode-Extension überwacht die Verfügbarkeit von Cursor und Windsurf (z.B. durch Prüfung laufender Prozesse oder API-Pings).
- Die Toolbar fragt den Status über einen SRPC-Call ab und zeigt ihn an.
- Bei der Auswahl von Agenten in der Toolbar wird die Konfiguration an den Thread-Manager gesendet, der das Routing entsprechend anpasst.

## 5. Datenmodell (Beispiele)

```typescript
interface Thread {
  id: string;
  name: string;
  createdAt: Date;
  participants: ('user' | 'cursor' | 'windsurf')[];
  history: Message[];
  currentContextId: string;
}

interface Message {
  id: string;
  threadId: string;
  timestamp: Date;
  sender: 'user' | 'cursor' | 'windsurf';
  content: string; // Text, Markdown, Code
  contextSnapshotId?: string; // Verweis auf Kontext zum Zeitpunkt der Nachricht
}

interface Context {
  id: string;
  version: number;
  timestamp: Date;
  url: string;
  selectedElements: DOMElementInfo[];
  comments: Comment[];
  screenshot?: string; // Base64 encoded oder URL
  // Weitere Metadaten
}

interface DOMElementInfo {
  // ... Details zum DOM-Element (Selector, HTML, CSS)
}

interface Comment {
  elementSelector: string;
  text: string;
}
```

## 6. Technologie-Stack

- **Frontend (Toolbar)**: TypeScript, Preact/React, CSS, WebSocket Client
- **Backend (VSCode Extension)**: TypeScript, Node.js, VSCode API, WebSocket Server
- **Kommunikation**: Erweitertes SRPC (basiert auf WebSockets, Zod)
- **Agenten-APIs**: Abhängig von Cursor und Windsurf (MCP, REST, etc.)
- **Build-System**: PNPM, Turborepo (wie bei Stagewise), Webpack/esbuild

## 7. Skalierbarkeit und Leistung

- **Kommunikation**: WebSockets sind für bidirektionale Echtzeitkommunikation gut geeignet. SRPC sorgt für Struktur.
- **Toolbar**: Leichtgewichtiges Framework (Preact) und effiziente DOM-Interaktion sind wichtig.
- **Extension**: Asynchrone Verarbeitung von Anfragen und Antworten. Effiziente Speicherung und Abruf von Kontextdaten.
- **Grenzen**: Die Leistung hängt stark von der Komplexität der Zielanwendung und der Reaktionszeit der KI-Agenten ab.

## 8. Sicherheit

- **Kommunikation**: Lokale Kommunikation über `localhost` minimiert externe Angriffsvektoren. Dennoch sollte die WebSocket-Verbindung gesichert werden (obwohl dies bei reiner localhost-Kommunikation weniger kritisch ist).
- **Daten**: Sensible Daten (Code, Kommentare) werden übertragen. Sicherstellen, dass keine unnötigen Daten gesendet werden. Lokale Speicherung sollte gesichert sein (VSCode Storage API bietet grundlegenden Schutz).
- **Berechtigungen**: Die Extension läuft mit den Berechtigungen des Benutzers in VSCode.

## 9. Risiken und Herausforderungen

- **Agenten-APIs**: Instabilität oder Änderungen in den APIs von Cursor und Windsurf.
- **Synchronisation**: Komplexität der zuverlässigen Kontextsynchronisation zwischen zwei unabhängigen Agenten.
- **Konfliktlösung**: Handhabung von widersprüchlichen Vorschlägen oder Aktionen der Agenten.
- **Leistung**: Sicherstellung der Performance in komplexen Webanwendungen.
- **Benutzererfahrung**: Gestaltung einer intuitiven UI für die Verwaltung von zwei Agenten und einem gemeinsamen Thread.

## 10. Fazit

Dieses technische Konzept skizziert eine erweiterte Architektur basierend auf Stagewise, um ein gemeinsames Frontend für Cursor und Windsurf mit Threading zu realisieren. Die Kernherausforderungen liegen in der robusten Implementierung des Thread-Managements, der Kontextsynchronisation und der Abstraktion der Agenten-APIs. Die Verwendung des SRPC-Frameworks bietet eine solide Grundlage für die Kommunikation.
