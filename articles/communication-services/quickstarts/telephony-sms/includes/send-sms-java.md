---
title: include file
description: include file
services: azure-communication-services
author: pvicencio
manager: ankita
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 03/12/2021
ms.topic: include
ms.custom: include file
ms.author: pvicencio
ms.openlocfilehash: 2739079b67d80f3e4a9f367aaa58f6dcbbb650ca
ms.sourcegitcommit: 18a91f7fe1432ee09efafd5bd29a181e038cee05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103622036"
---
Steigen Sie in Azure Communication Services ein, indem Sie die Java-Clientbibliothek für SMS von Communication Services nutzen, um SMS-Nachrichten zu senden.

Im Rahmen dieser Schnellstartanleitung fallen in Ihrem Azure-Konto ggf. geringfügige Kosten im Centbereich an.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Java Development Kit (JDK)](/java/azure/jdk/), Version 8 oder höher.
- [Apache Maven](https://maven.apache.org/download.cgi).
- Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource.](../../create-communication-resource.md)
- Eine für SMS-Nachrichten geeignete Telefonnummer. [Beziehen Sie eine Telefonnummer.](../get-phone-number.md)

### <a name="prerequisite-check"></a>Prüfen der Voraussetzungen

- Führen Sie in einem Terminal- oder Befehlsfenster `mvn -v` aus, um sich zu vergewissern, dass Maven installiert ist.
- Melden Sie sich zum Anzeigen der Telefonnummern für Ihre Communication Services-Ressource beim [Azure-Portal](https://portal.azure.com/) an, suchen Sie nach Ihrer Communication Services-Ressource, und öffnen Sie im linken Navigationsbereich die Registerkarte **Telefonnummern**.

## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-java-application"></a>Erstellen einer neuen Java-Anwendung

Öffnen Sie Ihr Terminal- oder Befehlsfenster, und navigieren Sie zu dem Verzeichnis, in dem Sie Ihre Java-Anwendung erstellen möchten. Führen Sie den unten angegebenen Befehl aus, um das Java-Projekt auf der Grundlage der Vorlage „maven-archetype-quickstart“ zu generieren.

```console
mvn archetype:generate -DgroupId=com.communication.quickstart -DartifactId=communication-quickstart -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```

Durch das Ziel „generate“ wird ein Verzeichnis erstellt, das den gleichen Namen besitzt wie die Artefakt-ID (artifactId). In diesem Verzeichnis enthält das Verzeichnis **src/main/java** den Quellcode des Projekts. Das Verzeichnis **src/test/java** enthält die Testquelle, und die Datei **pom.xml** ist das Projektobjektmodell (POM) des Projekts.

### <a name="install-the-package"></a>Installieren des Pakets

Öffnen Sie die Datei **pom.xml** in Ihrem Text-Editor. Fügen Sie der Gruppe „dependencies“ das folgende Abhängigkeitselement hinzu.

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-sms</artifactId>
    <version>1.0.0-beta.4</version>
</dependency>
```

### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

Fügen Sie der Datei **pom.xml** die Abhängigkeit `azure-core-http-netty` hinzu.

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-core-http-netty</artifactId>
    <version>1.8.0</version>
</dependency>
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-core</artifactId>
    <version>1.13.0</version> <!-- {x-version-update;com.azure:azure-core;dependency} -->
</dependency>
```

Öffnen Sie **/src/main/java/com/communication/quickstart/App.java** in einem Text-Editor, fügen Sie Importanweisungen hinzu, und entfernen Sie die Anweisung `System.out.println("Hello world!");`:

```java
package com.communication.quickstart;

import com.azure.communication.sms.models.*;
import com.azure.core.credential.AzureKeyCredential;
import com.azure.communication.sms.*;
import com.azure.core.http.HttpClient;
import com.azure.core.http.netty.NettyAsyncHttpClientBuilder;
import com.azure.core.util.Context;
import java.util.Arrays;

public class App
{
    public static void main( String[] args )
    {
        // Quickstart code goes here
    }
}

```

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen werden für einige der wichtigsten Features der Java-Clientbibliothek für SMS von Azure Communication Services verwendet:

| Name                                                             | Beschreibung                                                                                     |
| ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| SmsClientBuilder              | Diese Klasse dient zum Erstellen des SMS-Clients (SmsClient). Für sie müssen ein Endpunkt, Anmeldeinformationen und ein HTTP-Client angegeben werden. |
| SmsClient                    | Diese Klasse ist für alle SMS-Funktionen erforderlich. Sie wird zum Senden von SMS-Nachrichten verwendet.                |
| SmsSendResult                | Diese Klasse enthält das Ergebnis des SMS-Diensts.                                          |
| SmsSendOptions               | Diese Klasse stellt Optionen zum Hinzufügen benutzerdefinierter Tags und zum Konfigurieren von Zustellberichten bereit. Ist „deliveryReportEnabled“ auf „true“ festgelegt, wird bei erfolgreicher Zustellung ein Ereignis ausgegeben.|                           |

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Instanziieren Sie einen SMS-Client (`SmsClient`) mit Ihrer Verbindungszeichenfolge. `Key` ist die Anmeldeinformation aus dem Azure-Portal. Informationen zur Verwaltung der Verbindungszeichenfolge Ihrer Ressource finden Sie [hier](../../create-communication-resource.md#store-your-connection-string).

Fügen Sie der `main`-Methode folgenden Code hinzu:

```java
// You can find your endpoint and access key from your resource in the Azure Portal
String endpoint = "https://<resource-name>.communication.azure.com/";
AzureKeyCredential azureKeyCredential = new AzureKeyCredential("<access-key-credential>");

// Create an HttpClient builder of your choice and customize it
HttpClient httpClient = new NettyAsyncHttpClientBuilder().build();

SmsClient smsClient = new SmsClientBuilder()
                .endpoint(endpoint)
                .credential(azureKeyCredential)
                .httpClient(httpClient)
                .buildClient();
```

Sie können den Client mit einem beliebigen benutzerdefinierten HTTP-Client initialisieren, der die Schnittstelle `com.azure.core.http.HttpClient` implementiert. Im obigen Code wird die Verwendung des von `azure-core` bereitgestellten [Azure Core-Netty-HTTP-Clients](/java/api/overview/azure/core-http-netty-readme) gezeigt.

Anstelle des Endpunkts und des Zugriffsschlüssels können Sie mithilfe der ConnectionString()-Funktion auch die gesamte Verbindungszeichenfolge bereitstellen. 
```java
// You can find your connection string from your resource in the Azure Portal
String connectionString = "https://<resource-name>.communication.azure.com/;<access-key>";

// Create an HttpClient builder of your choice and customize it
HttpClient httpClient = new NettyAsyncHttpClientBuilder().build();

SmsClient smsClient = new SmsClientBuilder()
            .connectionString(connectionString)
            .httpClient(httpClient)
            .buildClient();
```

## <a name="send-a-11-sms-message"></a>Senden einer SMS: 1:1

Wenn Sie eine SMS an einen einzelnen Empfänger senden möchten, rufen Sie über SmsClient die Methode `send` mit der Telefonnummer eines einzelnen Empfängers auf. Sie können auch optionale Parameter übergeben, um anzugeben, ob der Zustellbericht aktiviert werden soll, und um benutzerdefinierte Tags festzulegen.

```java
SmsSendResult sendResult = smsClient.send(
                "<from-phone-number>",
                "<to-phone-number>",
                "Weekly Promotion");

System.out.println("Message Id: " + sendResult.getMessageId());
System.out.println("Recipient Number: " + sendResult.getTo());
System.out.println("Send Result Successful:" + sendResult.isSuccessful());
```
## <a name="send-a-1n-sms-message-with-options"></a>Senden einer SMS mit Optionen: 1:N
Wenn Sie eine SMS an eine Empfängerliste senden möchten, rufen Sie die Methode `send` mit einer Liste mit den Telefonnummern der Empfänger auf. Sie können auch optionale Parameter übergeben, um anzugeben, ob der Zustellbericht aktiviert werden soll, und um benutzerdefinierte Tags festzulegen.
```java
SmsSendOptions options = new SmsSendOptions();
options.setDeliveryReportEnabled(true);
options.setTag("Marketing");

Iterable<SmsSendResult> sendResults = smsClient.send(
    "<from-phone-number>",
    Arrays.asList("<to-phone-number1>", "<to-phone-number2>"),
    "Weekly Promotion",
    options /* Optional */,
    Context.NONE);

for (SmsSendResult result : sendResults) {
    System.out.println("Message Id: " + result.getMessageId());
    System.out.println("Recipient Number: " + result.getTo());
    System.out.println("Send Result Successful:" + result.isSuccessful());
}
```

Ersetzen Sie `<from-phone-number>` durch eine für SMS geeignete Telefonnummer, die Ihrer Communication Services-Ressource zugeordnet ist, und ersetzen Sie `<to-phone-number>` durch die Telefonnummer oder Liste mit Telefonnummern, an die Sie eine Nachricht senden möchten.

## <a name="optional-parameters"></a>Optionale Parameter

Der Parameter `deliveryReportEnabled` ist ein optionaler Parameter zum Konfigurieren von Zustellberichten. Dies ist in Szenarien hilfreich, in denen Ereignisse ausgegeben werden sollen, wenn SMS-Nachrichten zugestellt wurden. Informationen zum Konfigurieren von Zustellberichten für SMS-Nachrichten finden Sie in der Schnellstartanleitung [Behandeln von SMS-Ereignissen](../handle-sms-events.md).

Der Parameter `tag` ist ein optionaler Parameter zum Anwenden eines Tags auf den Zustellbericht.

## <a name="run-the-code"></a>Ausführen des Codes

Navigieren Sie zum Verzeichnis mit der Datei **pom.xml**, und kompilieren Sie das Projekt mithilfe des Befehls `mvn`.

```console

mvn compile

```

Erstellen Sie dann das Paket.

```console

mvn package

```

Führen Sie die App mit dem folgenden `mvn`-Befehl aus.

```console

mvn exec:java -Dexec.mainClass="com.communication.quickstart.App" -Dexec.cleanupDaemonThreads=false

```