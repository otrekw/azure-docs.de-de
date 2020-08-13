---
title: Azure Service Bus-Verwaltungsbibliotheken| Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie mit den Azure Service Bus-Verwaltungsbibliotheken dynamisch Service Bus-Namespaces und -Entitäten bereitgestellt werden.
ms.devlang: dotnet
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 63a8f0f8ef299f9d27d3c1be2746052536c4e5e8
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88066167"
---
# <a name="service-bus-management-libraries"></a>Service Bus-Verwaltungsbibliotheken

Die Azure Service Bus-Verwaltungsbibliotheken können dynamisch Service Bus-Namespaces und -Entitäten bereitstellen. Dies ermöglicht komplexe Bereitstellungen und Messagingszenarios sowie eine programmgesteuerte Bestimmung der bereitzustellenden Entitäten. Die Bibliotheken sind derzeit für .NET verfügbar.

## <a name="supported-functionality"></a>Unterstützte Funktionen

* Erstellen, Aktualisieren und Löschen von Namespaces
* Erstellen, Aktualisieren und Löschen von Warteschlangen
* Erstellen, Aktualisieren und Löschen von Themen
* Erstellen, Aktualisieren und Löschen von Abonnements

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung der Service Bus-Verwaltungsbibliotheken müssen Sie sich zunächst beim Azure Active Directory-Dienst (Azure AD) authentifizieren. Azure AD erfordert, dass Sie sich als Dienstprinzipal authentifizieren. Dadurch erhalten Sie Zugriff auf Ihre Azure-Ressourcen. Informationen zum Erstellen eines Dienstprinzipals finden Sie in einem der folgenden Artikel:  

* [Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../active-directory/develop/howto-create-service-principal-portal.md)
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](../active-directory/develop/howto-authenticate-service-principal-powershell.md)
* [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)

In diesen Tutorials erhalten Sie Werte für `AppId` (Client-ID), `TenantId` und `ClientSecret` (Authentifizierungsschlüssel). Diese werden von den Verwaltungsbibliotheken für die Authentifizierung verwendet. Sie müssen mindestens über die Berechtigungen [**Datenbesitzer in Azure Service Bus**](../role-based-access-control/built-in-roles.md#azure-service-bus-data-owner) oder [**Mitwirkender**](../role-based-access-control/built-in-roles.md#contributor) für die Ressourcengruppe sein, für die die Ausführung erfolgen soll.

## <a name="programming-pattern"></a>Muster für die Programmierung

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

## <a name="complete-code-to-create-a-queue"></a>Vollständiger Code für die Erstellung einer Warteschlange
Hier ist der vollständige Code für die Erstellung einer Service Bus-Warteschlange angegeben: 

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

> [!IMPORTANT]
> Ein vollständiges Beispiel finden Sie unter dem [.NET Management-Beispiel auf GitHub](https://github.com/Azure-Samples/service-bus-dotnet-management/). 

## <a name="next-steps"></a>Nächste Schritte
[Microsoft.Azure.Management.ServiceBus-API-Referenz](/dotnet/api/Microsoft.Azure.Management.ServiceBus)