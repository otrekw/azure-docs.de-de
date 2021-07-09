---
title: Datei einfügen
description: include file
services: azure-communication-services
author: orwatson
manager: vravikumar
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 06/01/2021
ms.topic: include
ms.custom: include file
ms.author: orwatson
ms.openlocfilehash: f044b23d26bee71fc3f6a28f96a42c42a01f4d02
ms.sourcegitcommit: b11257b15f7f16ed01b9a78c471debb81c30f20c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2021
ms.locfileid: "111593104"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/): Active LTS- und Maintenance LTS-Versionen.
- Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource](../create-communication-resource.md).
- Erstellen Sie im Azure-Portal eine [Application Insights-Ressource](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource).

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-nodejs-app"></a>Erstellen einer neuen Node.js-App

Falls Sie bereits über eine App verfügen, für die eine ACS-Bibliothek genutzt wird, und Sie mit JavaScript vertraut sind, können Sie auch mit [Einrichten der Ablaufverfolgung](#setup-tracer) fortfahren.

Für den Einstieg benötigen Sie eine JS-App, für die eine unserer Clientbibliotheken verwendet wird. Wir verwenden die Bibliothek [@azure/communication-identity](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/communication/communication-identity), um unsere App zu erstellen. Führen Sie die folgenden Schritte aus, um Ihre Beispiel-App einzurichten:

Öffnen Sie Ihr Terminal- oder Befehlsfenster, erstellen Sie ein neues Verzeichnis, und navigieren Sie zu diesem Verzeichnis.

```console
mkdir export-js-telemetry && cd export-js-telemetry
```
Führen Sie `npm init -y` aus, um die Datei **package.json** mit den Standardeinstellungen zu erstellen.

```console
npm init -y
```

### <a name="install-dependencies"></a>Installieren von Abhängigkeiten

Verwenden Sie den Befehl `npm install`, um die Abhängigkeiten zu installieren, die für unsere App verwendet werden.

```bash
npm install @azure/communication-identity @azure/monitor-opentelemetry-exporter @opentelemetry/node@^0.14.0 @opentelemetry/plugins-node-core --save
```

Mit der Option `--save` werden die Bibliotheken als Abhängigkeiten in der Datei **package.json** hinzugefügt, die im vorherigen Schritt erstellt wurde.

Es ist wichtig, dass Sie nicht die spezifische Version von `@opentelemetry/node@^0.14.0` vergessen. Zum Zeitpunkt der Erstellung dieses Leitfadens ist dies die neueste Version des Pakets, die vollständig mit `@azure/monitor-opentelemetry-exporter` kompatibel ist.

### <a name="add-main-app-file"></a>Hinzufügen der Hauptdatei für die App

Erstellen Sie eine Datei mit dem Namen **token.js** im Stammverzeichnis des Verzeichnisses. Fügen Sie ihr folgenden Code hinzu:

```javascript
// token.js

const { CommunicationIdentityClient } = require("@azure/communication-identity");

async function main() {
  const ACS_CONNECTION_STRING = "<your-acs-connection-string>"
  const client = new CommunicationIdentityClient(ACS_CONNECTION_STRING);

  console.log("Creating user...")
  const user = await client.createUser();
  console.log(`User ${user.communicationUserId} was created successfully.\n`);

  console.log(`Issuing token for ${user.communicationUserId}...`);
  const { token } = await client.getToken(user, ["chat"]);
  console.log("Token issued successfully.\n")
  console.log(`${token}\n`);

  console.log(`Revoking token for ${user.communicationUserId}...`);
  await client.revokeTokens(user);
  console.log("Token revoked successfully.\n");

  console.log(`Deleting user ${user.communicationUserId}...`);
  await client.deleteUser(user);
  console.log("User deleted successfully.\n");
}

main().catch((error) => {
  console.log("Encountered an error:", error);
  process.exit(1);
});
```
## <a name="setup-tracer"></a>Einrichten der Ablaufverfolgung

Erstellen Sie eine Datei mit dem Namen **tracing.js** im Stammverzeichnis der Verzeichnisstruktur. Fügen Sie ihr folgenden Code hinzu:

```javascript
// tracing.js

const azureSdkTracing = require("@azure/core-tracing");
const { AzureMonitorTraceExporter } = require("@azure/monitor-opentelemetry-exporter");
const { NodeTracerProvider } = require("@opentelemetry/node");
const { BatchSpanProcessor } = require("@opentelemetry/tracing");

const AI_CONNECTION_STRING = "<your-application-insights-connection-string>";
const provider = new NodeTracerProvider();
const azExporter = new AzureMonitorTraceExporter({
  connectionString: AI_CONNECTION_STRING
});

provider.addSpanProcessor(
  new BatchSpanProcessor(azExporter, {
    bufferSize: 1000, // 1000 spans
    bufferTimeout: 5000 // 5 seconds
  })
);

provider.register();

const tracer = provider.getTracer("sample tracer");
azureSdkTracing.setTracer(tracer);

exports.default = tracer;
```

### <a name="add-tracer-to-main-app"></a>Hinzufügen der Ablaufverfolgung zur Haupt-App

Führen Sie für **token.js** die folgenden Updates durch:

Machen Sie die Verwendung von **tracing.js** obligatorisch:

```javascript
const { CommunicationIdentityClient } = require("@azure/communication-identity");
const tracer = require("./tracing.js");

async function main() {
```

Erstellen eines span-Elements auf Stammebene und Exportieren von Daten

```javascript
  console.log("User deleted successfully.\n");
}

const rootSpan = tracer.startSpan("Root Identity Span");
tracer.withSpan(rootSpan, async function() {
  try {
    await main();
  } catch (error) {
    console.error("Error running sample:", error);
  } finally {
    // End the optional root span on completion
    rootSpan.end();
  }
}).then(function() {
  console.log("Awaiting batched span processor to export batched spans...");

  setTimeout(function() {
    console.log("Spans exported.");
  }, 6000);
});
```

## <a name="run-the-code"></a>Ausführen des Codes

Achten Sie darauf, dass Sie die Platzhaltertexte durch gültige Verbindungszeichenfolgen-Werte ersetzen.

Verwenden Sie den node-Befehl, um den Code auszuführen, den Sie der Datei **token.js** hinzugefügt haben.

```console
node token.js
```