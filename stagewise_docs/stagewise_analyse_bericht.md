# Technische Analyse: Stagewise als Frontend für Cursor und Windsurf

## Zusammenfassung

Stagewise ist ein Open-Source-Tool, das als Browser-Toolbar fungiert und eine Brücke zwischen Frontend-UI und Code-AI-Agenten in Code-Editoren wie Cursor und Windsurf schlägt. Die Analyse des GitHub-Repositories zeigt, dass Stagewise eine bidirektionale Kommunikation zwischen Browser und IDE ermöglicht und somit als gemeinsames Frontend für beide AI-Agenten dienen kann.

## Architektur und Komponenten

### Hauptkomponenten

1. **Browser-Toolbar**: 
   - Implementiert als JavaScript-Bibliothek (`@stagewise/toolbar`)
   - Kann in verschiedene Frontend-Frameworks integriert werden (React, Next.js, Vue, Nuxt.js)
   - Ermöglicht die Auswahl von DOM-Elementen und das Hinzufügen von Kommentaren

2. **VSCode-Extension**:
   - Hauptschnittstelle zur IDE
   - Stellt einen Server bereit, der mit der Toolbar kommuniziert
   - Leitet Anfragen an die jeweiligen AI-Agenten weiter

3. **SRPC-Framework**:
   - Schema-validiertes RPC-System über WebSocket
   - Ermöglicht typsichere, bidirektionale Kommunikation
   - Unterstützt Fortschrittsbenachrichtigungen für lang laufende Operationen

4. **Extension-Toolbar-SRPC-Contract**:
   - Definiert das Kommunikationsprotokoll zwischen VSCode-Extension und Toolbar
   - Stellt sicher, dass Nachrichten typsicher und validiert sind

### Kommunikationsmechanismen

Die Kommunikation zwischen Stagewise und den Code-AI-Agenten (Cursor, Windsurf) erfolgt über mehrere Schichten:

1. **Browser zu VSCode-Extension**:
   - WebSocket-basierte Kommunikation über das SRPC-Framework
   - Die Toolbar entdeckt automatisch die laufende Extension
   - Port 5746 wird für die Kommunikation verwendet (siehe `.cursor/mcp.json`)

2. **VSCode-Extension zu AI-Agenten**:
   - Die Extension kommuniziert mit Cursor über das MCP-Protokoll (Machine Communication Protocol)
   - Die Verbindung zu Windsurf erfolgt über ähnliche Mechanismen

3. **Datenfluss**:
   - DOM-Elemente, Screenshots und Metadaten werden vom Browser an die AI-Agenten gesendet
   - Antworten und Aktionen der AI-Agenten werden zurück an die Toolbar übermittelt

## Stagewise als gemeinsames Frontend für Cursor und Windsurf

### Funktionsweise

Stagewise kann als gemeinsames Frontend für beide IDEs (Cursor und Windsurf) dienen, indem es:

1. Eine einheitliche UI-Schnittstelle (Toolbar) bereitstellt
2. Kontextinformationen aus dem Browser an die AI-Agenten sendet
3. Die Kommunikation mit beiden Agenten über die VSCode-Extension koordiniert

### Einschränkungen

Die Analyse des Repositories zeigt jedoch einige Einschränkungen:

1. **Gleichzeitige Nutzung**: 
   - In der README wird darauf hingewiesen, dass bei mehreren geöffneten Cursor-Fenstern Probleme auftreten können
   - "If you have multiple Cursor windows open, the toolbar may send prompts to the wrong window"
   - Dies deutet darauf hin, dass die gleichzeitige Nutzung mehrerer Agenten noch nicht optimal unterstützt wird

2. **Gemeinsamer Thread**:
   - Es gibt keine explizite Implementierung eines gemeinsamen Threads für beide Agenten
   - Die Kommunikation erfolgt primär zwischen Toolbar und jeweils einem Agenten

## Implementierung eines gemeinsamen Threads für Agenten

### Aktueller Stand

Die Analyse des Quellcodes zeigt, dass Stagewise derzeit keinen nativen Mechanismus für einen gemeinsamen Thread oder Kontext zwischen Cursor und Windsurf implementiert. Die Kommunikation ist hauptsächlich darauf ausgerichtet, Kontextinformationen vom Browser an einen einzelnen AI-Agenten zu senden.

### Potenzielle Lösungsansätze

Um einen gemeinsamen Thread für beide Agenten zu implementieren, könnten folgende Ansätze verfolgt werden:

1. **Erweiterung des SRPC-Contracts**:
   - Erweiterung des Kommunikationsprotokolls, um Nachrichten an mehrere Agenten gleichzeitig zu senden
   - Implementierung eines Broadcast-Mechanismus in der VSCode-Extension

2. **Gemeinsamer Kontext-Speicher**:
   - Einführung eines gemeinsamen Kontextspeichers in der VSCode-Extension
   - Synchronisierung des Kontexts zwischen beiden Agenten

3. **UI-Erweiterung**:
   - Erweiterung der Toolbar-UI, um explizit zwischen Agenten zu wechseln oder beide gleichzeitig anzusprechen
   - Visualisierung des gemeinsamen Threads in der UI

## Technische Details der Kommunikation

### SRPC-Framework

Das SRPC-Framework (Schema-validated RPC) ist das Herzstück der Kommunikation und bietet:

```typescript
// Beispiel eines RPC-Contracts
const contract = createBridgeContract({
  server: {
    triggerAgentPrompt: {
      request: z.object({
        prompt: z.string(),
        options: z.object({
          temperature: z.number().min(0).max(1).optional(),
        }).optional(),
      }),
      response: z.object({
        result: z.object({
          success: z.boolean(),
          output: z.string().optional(),
        }),
      }),
      update: z.object({
        updateText: z.string(),
        progress: z.number().min(0).max(100).optional(),
      }),
    },
  },
  client: {
    getCurrentUrl: {
      request: z.object({}).optional(),
      response: z.object({
        url: z.string().url(),
        title: z.string().optional(),
      }),
    },
  },
});
```

### Extension-Toolbar-Kommunikation

Die Kommunikation zwischen Extension und Toolbar wird über WebSockets abgewickelt:

```typescript
// Extension (server) side
import { getExtensionBridge } from '@stagewise/extension-toolbar-srpc-contract';
import http from 'node:http';

const httpServer = http.createServer();
const bridge = getExtensionBridge(httpServer);

bridge.register({
  triggerAgentPrompt: async (request, { sendUpdate }) => {
    // Implementation
  }
});

// Toolbar (client) side
import { getToolbarBridge } from '@stagewise/extension-toolbar-srpc-contract';

const bridge = getToolbarBridge('ws://localhost:5746');
await bridge.connect();

// Call methods on the extension
const result = await bridge.call.triggerAgentPrompt({
  prompt: 'Make this button green'
});
```

### MCP-Protokoll für Cursor

Die Kommunikation mit Cursor erfolgt über das MCP-Protokoll, wie in `.cursor/mcp.json` definiert:

```json
{
  "mcpServers": {
    "stagewise": {
      "url": "http://localhost:5746/sse",
      "env": {}
    }
  }
}
```

## Fazit und Empfehlungen

### Aktueller Stand

Stagewise bietet eine solide Grundlage für die Kommunikation zwischen Browser-UI und Code-AI-Agenten. Es ermöglicht:

1. Die Auswahl von DOM-Elementen im Browser
2. Das Senden von Kontextinformationen an AI-Agenten
3. Die Integration mit verschiedenen Frontend-Frameworks

### Empfehlungen für einen gemeinsamen Thread

Um einen gemeinsamen Thread für beide Agenten zu implementieren, wären folgende Erweiterungen notwendig:

1. **Erweiterung des SRPC-Contracts**:
   - Hinzufügen von Methoden für Multi-Agent-Kommunikation
   - Implementierung von Broadcast-Mechanismen

2. **Erweiterung der VSCode-Extension**:
   - Implementierung eines Proxys, der Anfragen an beide Agenten weiterleitet
   - Zusammenführung der Antworten beider Agenten

3. **UI-Erweiterungen**:
   - Anzeige des aktiven Agenten oder beider Agenten
   - Visualisierung des gemeinsamen Kontexts

### Nächste Schritte

Für die Implementierung eines gemeinsamen Frontends mit Thread-Unterstützung für beide Agenten empfehle ich:

1. Erweiterung der VSCode-Extension, um gleichzeitig mit Cursor und Windsurf zu kommunizieren
2. Implementierung eines gemeinsamen Kontextspeichers in der Extension
3. Erweiterung der Toolbar-UI, um explizit zwischen Agenten zu wechseln oder beide gleichzeitig anzusprechen
4. Entwicklung eines Synchronisationsmechanismus für den Kontext zwischen beiden Agenten

Mit diesen Erweiterungen könnte Stagewise als vollwertiges Frontend für die kollaborative Arbeit von Cursor und Windsurf dienen und einen gemeinsamen Thread für beide Agenten bereitstellen.
