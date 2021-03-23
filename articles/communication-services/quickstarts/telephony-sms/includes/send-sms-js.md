---
title: include file
description: include file
services: azure-communication-services
author: bertong
manager: ankita
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 03/11/2021
ms.topic: include
ms.custom: include file
ms.author: bertong
ms.openlocfilehash: 0d142c477e1de2a2a34a8abfd948800cc0b607ee
ms.sourcegitcommit: 27cd3e515fee7821807c03e64ce8ac2dd2dd82d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103622359"
---
Steigen Sie in Azure Communication Services ein, indem Sie die JavaScript-Clientbibliothek für SMS von Communication Services nutzen, um SMS-Nachrichten zu senden.

Im Rahmen dieser Schnellstartanleitung fallen in Ihrem Azure-Konto ggf. geringfügige Kosten im Centbereich an.

<!--**TODO: update all these reference links as the resources go live**

[API reference documentation](../../../references/overview.md) | [Library source code](https://github.com/Azure/azure-sdk-for-js-pr/tree/feature/communication/sdk/communication/communication-sms) | [Package (NPM)](https://www.npmjs.com/package/@azure/communication-sms) | [Samples](#todo-samples)-->

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org/): Active LTS- und Maintenance LTS-Versionen (8.11.1 und 10.14.1 empfohlen).
- Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource.](../../create-communication-resource.md)
- Eine für SMS-Nachrichten geeignete Telefonnummer. [Beziehen Sie eine Telefonnummer.](../get-phone-number.md)

### <a name="prerequisite-check"></a>Prüfen der Voraussetzungen

- Führen Sie in einem Terminal- oder Befehlsfenster `node --version` aus, um sich zu vergewissern, dass Node.js installiert ist.
- Melden Sie sich zum Anzeigen der Telefonnummern für Ihre Communication Services-Ressource beim [Azure-Portal](https://portal.azure.com/) an, suchen Sie nach Ihrer Communication Services-Ressource, und öffnen Sie im linken Navigationsbereich die Registerkarte **Telefonnummern**.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-nodejs-application"></a>Erstellen einer neuen Node.js-Anwendung

Öffnen Sie zunächst Ihr Terminal- oder Befehlsfenster, erstellen Sie ein neues Verzeichnis für Ihre App, und navigieren Sie zu diesem Verzeichnis.

```console
mkdir sms-quickstart && cd sms-quickstart
```

Führen Sie `npm init -y` aus, um die Datei **package.json** mit den Standardeinstellungen zu erstellen.

```console
npm init -y
```

Verwenden Sie einen Text-Editor, um im Stammverzeichnis des Projekts die Datei **send-sms.js** zu erstellen. In den folgenden Abschnitten wird dieser Datei der gesamte Quellcode für diese Schnellstartanleitung hinzugefügt.

### <a name="install-the-package"></a>Installieren des Pakets

Verwenden Sie den Befehl `npm install`, um die JavaScript-Clientbibliothek für SMS von Azure Communication Services zu installieren.

```console
npm install @azure/communication-sms --save
```

Durch die Option `--save` wird die Bibliothek als Abhängigkeit in der Datei **package.json** aufgeführt.

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen werden für einige der wichtigsten Features der Node.js-Clientbibliothek für SMS von Azure Communication Services verwendet:

| Name                                  | Beschreibung                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| SmsClient | Diese Klasse ist für alle SMS-Funktionen erforderlich. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Senden von SMS-Nachrichten. |
| SmsSendResult               | Diese Klasse enthält das Ergebnis des SMS-Diensts.                                          |
| SmsSendOptions | Diese Schnittstelle bietet Optionen zum Konfigurieren von Zustellberichten. Ist `enableDeliveryReport` auf `true` festgelegt, wird bei erfolgreicher Zustellung ein Ereignis ausgegeben. |
| SmsSendRequest | Bei dieser Schnittstelle handelt es sich um das Modell für die Erstellung der SMS-Anforderung (beispielsweise zum Konfigurieren der Ausgangs- und Zieltelefonnummer und des SMS-Inhalts). |

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Importieren Sie den SMS-Client (**SmsClient**) aus der Clientbibliothek, und instanziieren Sie ihn mit Ihrer Verbindungszeichenfolge. Im folgenden Code wird die Verbindungszeichenfolge für die Ressource aus einer Umgebungsvariablen namens `COMMUNICATION_SERVICES_CONNECTION_STRING` abgerufen. Informationen zur Verwaltung der Verbindungszeichenfolge Ihrer Ressource finden Sie [hier](../../create-communication-resource.md#store-your-connection-string).

Fügen Sie **send-sms.js** den folgenden Code hinzu:

```javascript
const { SmsClient } = require('@azure/communication-sms');

// This code demonstrates how to fetch your connection string
// from an environment variable.
const connectionString = process.env['COMMUNICATION_SERVICES_CONNECTION_STRING'];

// Instantiate the SMS client
const smsClient = new SmsClient(connectionString);
```

## <a name="send-a-1n-sms-message"></a>Senden einer SMS: 1:N

Zum Senden einer SMS-Nachricht an eine Empfängerliste rufen Sie über SmsClient die Funktion `send` mit einer Liste der Telefonnummern der Empfänger auf. (Wenn Sie eine Nachricht an einen einzelnen Empfänger senden möchten, fügen Sie nur eine Telefonnummer in die Liste ein.) Fügen Sie am Ende der Methode **send-sms.js** den folgenden Code hinzu:

```javascript
async function main() {
  const sendResults = await smsClient.send({
    from: "<from-phone-number>",
    to: ["<to-phone-number-1>", "<to-phone-number-2>"],
    message: "Hello World 👋🏻 via SMS"
  });

  // individual messages can encounter errors during sending
  // use the "successful" property to verify
  for (const sendResult of sendResults) {
    if (sendResult.successful) {
      console.log("Success: ", sendResult);
    } else {
      console.error("Something went wrong when trying to send this message: ", sendResult);
    }
  }
}

main();
```
Ersetzen Sie `<from-phone-number>` durch eine für SMS geeignete Telefonnummer, die Ihrer Communication Services-Ressource zugeordnet ist, und ersetzen Sie `<to-phone-number>` durch die Telefonnummer, an die Sie eine Nachricht senden möchten.

## <a name="send-a-1n-sms-message-with-options"></a>Senden einer SMS mit Optionen: 1:N

Sie können auch ein options-Objekt übergeben, um anzugeben, ob der Zustellbericht aktiviert werden soll, und um benutzerdefinierte Tags festzulegen.

```javascript

async function main() {
  await smsClient.send({
    from: "<from-phone-number>",
    to: ["<to-phone-number-1>", "<to-phone-number-2>"],
    message: "Weekly Promotion!"
  }, {
    //Optional parameter
    enableDeliveryReport: true,
    tag: "marketing"
  });

  // individual messages can encounter errors during sending
  // use the "successful" property to verify
  for (const sendResult of sendResults) {
    if (sendResult.successful) {
      console.log("Success: ", sendResult);
    } else {
      console.error("Something went wrong when trying to send this message: ", sendResult);
    }
  }
}

main();
```

Der Parameter `enableDeliveryReport` ist ein optionaler Parameter zum Konfigurieren von Zustellberichten. Dies ist in Szenarien hilfreich, in denen Ereignisse ausgegeben werden sollen, wenn SMS-Nachrichten zugestellt wurden. Informationen zum Konfigurieren von Zustellberichten für SMS-Nachrichten finden Sie in der Schnellstartanleitung [Behandeln von SMS-Ereignissen](../handle-sms-events.md).
`tag` ist ein optionaler Parameter zum Anwenden eines Tags auf den Zustellbericht.

## <a name="run-the-code"></a>Ausführen des Codes

Verwenden Sie den Befehl `node`, um den Code auszuführen, den Sie der Datei **send-sms.js** hinzugefügt haben.

```console

node ./send-sms.js

```
