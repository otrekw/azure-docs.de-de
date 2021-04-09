---
title: Programmgesteuertes Erstellen von Azure Service Bus-Entitäten | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie dynamisch oder programmgesteuert bereitgestellte Service Bus-Namespaces und -Entitäten verwendet werden.
ms.devlang: dotnet
ms.topic: article
ms.date: 01/13/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: 57192ab2ee1624cb18de832ac91c95290da727df
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98539879"
---
# <a name="dynamically-provision-service-bus-namespaces-and-entities"></a>Dynamisches Bereitstellen von Service Bus-Namespaces und -Entitäten 
Die Azure Service Bus-Verwaltungsbibliotheken können dynamisch Service Bus-Namespaces und -Entitäten bereitstellen. Dies ermöglicht komplexe Bereitstellungen und Messagingszenarios sowie eine programmgesteuerte Bestimmung der bereitzustellenden Entitäten. Die Bibliotheken sind derzeit für .NET verfügbar.

## <a name="overview"></a>Überblick
Zum Erstellen und Verwalten von Service Bus-Entitäten stehen drei Verwaltungsbibliotheken zur Verfügung. Sie lauten wie folgt:

- [Azure.Messaging.ServiceBus.Administration](#azuremessagingservicebusadministration)
- [Microsoft.Azure.ServiceBus.Management](#microsoftazureservicebusmanagement)
- [Microsoft.Azure.Management.ServiceBus](#microsoftazuremanagementservicebus)

All diese Pakete unterstützen Vorgänge zum Erstellen, Anfordern, Auflisten, Löschen und Aktualisieren für **Warteschlangen, Themen und Abonnements**. Aber nur [Microsoft.Azure.Management.ServiceBus](#microsoftazuremanagementservicebus) unterstützt Vorgänge zum Erstellen, Aktualisieren, Auflisten, Anfordern und Löschen für **Namespaces**, das Auflisten und erneute Generieren von SAS-Schlüsseln und vieles mehr. 

Die Bibliothek „Microsoft.Azure.Management.ServiceBus“ kann nur mit der Azure Active Directory-Authentifizierung (Azure AD) eingesetzt werden und bietet keine Unterstützung für die Verwendung einer Verbindungszeichenfolge. Die anderen beiden Bibliotheken („Azure.Messaging.ServiceBus“ und „Microsoft.Azure.ServiceBus“) unterstützen hingegen die Nutzung einer Verbindungszeichenfolge zur Authentifizierung beim Dienst und sind benutzerfreundlicher. Von diesen Bibliotheken ist „Azure.Messaging.ServiceBus“ die neueste, daher empfehlen wir Ihnen, diese Bibliothek zu verwenden.

Ausführlichere Informationen zu diesen Bibliotheken finden Sie in den folgenden Abschnitten. 

## <a name="azuremessagingservicebusadministration"></a>Azure.Messaging.ServiceBus.Administration
Die Klasse [servicebusadministrationclient](/dotnet/api/azure.messaging.servicebus.administration.servicebusadministrationclient) im Namespace [Azure.Messaging.ServiceBus.Administration](/dotnet/api/azure.messaging.servicebus.administration) eignet sich zum Verwalten von Namespaces, Warteschlangen, Themen und Abonnements. Sehen Sie sich den Beispielcode an. Ein vollständiges Beispiel finden Sie unter [CRUD-Beispiel](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/servicebus/Azure.Messaging.ServiceBus/tests/Samples/Sample07_CrudOperations.cs).

```csharp
using System;
using System.Threading.Tasks;

using Azure.Messaging.ServiceBus.Administration;

namespace adminClientTrack2
{
    class Program
    {
        public static void Main()
        {
            MainAsync().GetAwaiter().GetResult();
        }

        private static async Task MainAsync()
        {
            string connectionString = "SERVICE BUS NAMESPACE CONNECTION STRING";
            string QueueName = "QUEUE NAME";
            string TopicName = "TOPIC NAME";
            string SubscriptionName = "SUBSCRIPTION NAME";

            var adminClient = new ServiceBusAdministrationClient(connectionString);
            bool queueExists = await adminClient.QueueExistsAsync(QueueName);
            if (!queueExists)
            {
                var options = new CreateQueueOptions(QueueName)
                {
                    MaxDeliveryCount = 3                    
                };
                await adminClient.CreateQueueAsync(options);
            }


            bool topicExists = await adminClient.TopicExistsAsync(TopicName);
            if (!topicExists)
            {
                var options = new CreateTopicOptions(TopicName)
                {
                    MaxSizeInMegabytes = 1024
                };
                await adminClient.CreateTopicAsync(options);
            }

            bool subscriptionExists = await adminClient.SubscriptionExistsAsync(TopicName, SubscriptionName);
            if (!subscriptionExists)
            {
                var options = new CreateSubscriptionOptions(TopicName, SubscriptionName)
                {
                    DefaultMessageTimeToLive = new TimeSpan(2, 0, 0, 0)
                };
                await adminClient.CreateSubscriptionAsync(options);
            }
        }
    }
}

```


## <a name="microsoftazureservicebusmanagement"></a>Microsoft.Azure.ServiceBus.Management 
Die Klasse [managementclient](/dotnet/api/microsoft.azure.servicebus.management.managementclient) im Namespace [Microsoft.Azure.ServiceBus.Management](/dotnet/api/microsoft.azure.servicebus.management) eignet sich zum Verwalten von Namespaces, Warteschlangen, Themen und Abonnements. Sehen Sie sich den Beispielcode an: 

> [!NOTE]
> Es empfiehlt sich, die Klasse `ServiceBusAdministrationClient` aus der Bibliothek `Azure.Messaging.ServiceBus.Administration` zu verwenden, da es sich hierbei um das neueste SDK handelt. Weitere Details finden Sie im [ersten Abschnitt](#azuremessagingservicebusadministration). 

```csharp
using System;
using System.Threading.Tasks;

using Microsoft.Azure.ServiceBus.Management;

namespace SBusManagementClient
{
    class Program
    {
        public static void Main()
        {
            MainAsync().GetAwaiter().GetResult();
        }

        private static async Task MainAsync()
        {
            string connectionString = "SERVICE BUS NAMESPACE CONNECTION STRING";
            string QueueName = "QUEUE NAME";
            string TopicName = "TOPIC NAME";
            string SubscriptionName = "SUBSCRIPTION NAME";

            var managementClient = new ManagementClient(connectionString);
            bool queueExists = await managementClient.QueueExistsAsync(QueueName);
            if (!queueExists)
            {
                QueueDescription qd = new QueueDescription(QueueName);
                qd.MaxSizeInMB = 1024;
                qd.MaxDeliveryCount = 3;
                await managementClient.CreateQueueAsync(qd);
            }


            bool topicExists = await managementClient.TopicExistsAsync(TopicName);
            if (!topicExists)
            {
                TopicDescription td = new TopicDescription(TopicName);
                td.MaxSizeInMB = 1024;
                td.DefaultMessageTimeToLive = new TimeSpan(2, 0, 0, 0);
                await managementClient.CreateTopicAsync(td);
            }

            bool subscriptionExists = await managementClient.SubscriptionExistsAsync(TopicName, SubscriptionName);
            if (!subscriptionExists)
            {
                SubscriptionDescription sd = new SubscriptionDescription(TopicName, SubscriptionName);
                sd.DefaultMessageTimeToLive = new TimeSpan(2, 0, 0, 0);
                sd.MaxDeliveryCount = 3;
                await managementClient.CreateSubscriptionAsync(sd);
            }
        }
    }
}
```


## <a name="microsoftazuremanagementservicebus"></a>Microsoft.Azure.Management.ServiceBus 
Diese Bibliothek gehört zum Azure Resource Manager-basierten Steuerungsebenen-SDK. 

### <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Verwaltungsbibliothek müssen Sie sich zunächst beim Azure Active Directory-Dienst (Azure AD) authentifizieren. Azure AD erfordert, dass Sie sich als Dienstprinzipal authentifizieren. Dadurch erhalten Sie Zugriff auf Ihre Azure-Ressourcen. Informationen zum Erstellen eines Dienstprinzipals finden Sie in einem der folgenden Artikel:  

* [Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../active-directory/develop/howto-create-service-principal-portal.md)
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](../active-directory/develop/howto-authenticate-service-principal-powershell.md)
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle](/cli/azure/create-an-azure-service-principal-azure-cli)

In diesen Tutorials erhalten Sie Werte für `AppId` (Client-ID), `TenantId` und `ClientSecret` (Authentifizierungsschlüssel). Diese werden von den Verwaltungsbibliotheken für die Authentifizierung verwendet. Sie müssen mindestens über die Berechtigungen [**Datenbesitzer in Azure Service Bus**](../role-based-access-control/built-in-roles.md#azure-service-bus-data-owner) oder [**Mitwirkender**](../role-based-access-control/built-in-roles.md#contributor) für die Ressourcengruppe sein, für die die Ausführung erfolgen soll.

### <a name="programming-pattern"></a>Muster für die Programmierung

Das Muster zum Bearbeiten einer beliebigen Service Bus-Ressource folgt einem gemeinsamen Protokoll:

1. Rufen Sie mithilfe der **Microsoft.IdentityModel.Clients.ActiveDirectory**-Bibliothek ein Token aus Azure AD ab:
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.azure.com/", new ClientCredential(clientId, clientSecret));
   ```
2. Erstellen Sie das `ServiceBusManagementClient`-Objekt:

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```
3. Legen Sie die `CreateOrUpdate`-Parameter auf Ihre spezifischen Werte fest:

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```
4. Führen Sie den Aufruf aus:

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

### <a name="complete-code-to-create-a-queue"></a>Vollständiger Code für die Erstellung einer Warteschlange
Hier sehen Sie den Beispielcode für die Erstellung einer Service Bus-Warteschlange. Ein vollständiges Beispiel finden Sie unter dem [.NET Management-Beispiel auf GitHub](https://github.com/Azure-Samples/service-bus-dotnet-management/). 

```csharp
using System;
using System.Threading.Tasks;

using Microsoft.Azure.Management.ServiceBus;
using Microsoft.Azure.Management.ServiceBus.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;

namespace SBusADApp
{
    class Program
    {
        static void Main(string[] args)
        {
            CreateQueue().GetAwaiter().GetResult();
        }

        private static async Task CreateQueue()
        {
            try
            {
                var subscriptionID = "<SUBSCRIPTION ID>";
                var resourceGroupName = "<RESOURCE GROUP NAME>";
                var namespaceName = "<SERVICE BUS NAMESPACE NAME>";
                var queueName = "<NAME OF QUEUE YOU WANT TO CREATE>";

                var token = await GetToken();

                var creds = new TokenCredentials(token);
                var sbClient = new ServiceBusManagementClient(creds)
                {
                    SubscriptionId = subscriptionID,
                };

                var queueParams = new SBQueue();

                Console.WriteLine("Creating queue...");
                await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, queueName, queueParams);
                Console.WriteLine("Created queue successfully.");
            }
            catch (Exception e)
            {
                Console.WriteLine("Could not create a queue...");
                Console.WriteLine(e.Message);
                throw e;
            }
        }

        private static async Task<string> GetToken()
        {
            try
            {
                var tenantId = "<AZURE AD TENANT ID>";
                var clientId = "<APPLICATION/CLIENT ID>";
                var clientSecret = "<CLIENT SECRET>";

                var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

                var result = await context.AcquireTokenAsync(
                    "https://management.azure.com/",
                    new ClientCredential(clientId, clientSecret)
                );

                // If the token isn't a valid string, throw an error.
                if (string.IsNullOrEmpty(result.AccessToken))
                {
                    throw new Exception("Token result is empty!");
                }

                return result.AccessToken;
            }
            catch (Exception e)
            {
                Console.WriteLine("Could not get a token...");
                Console.WriteLine(e.Message);
                throw e;
            }
        }

    }
}
```

## <a name="fluent-library"></a>Fluent-Bibliothek
Ein Beispiel für die Verwendung der Fluent-Bibliothek zur Verwaltung von Service Bus-Entitäten finden Sie unter [diesem Beispiel](https://github.com/Azure/azure-libraries-for-net/tree/master/Samples/ServiceBus). 

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die folgenden Referenzthemen an: 

- [Azure.Messaging.ServiceBus.Administration](/dotnet/api/azure.messaging.servicebus.administration.servicebusadministrationclient)
- [Microsoft.Azure.ServiceBus.Management](/dotnet/api/microsoft.azure.servicebus.management.managementclient)
- [Microsoft.Azure.Management.ServiceBus](/dotnet/api/microsoft.azure.management.servicebus.servicebusmanagementclient)
- [Fluent](/dotnet/api/microsoft.azure.management.servicebus.fluent)