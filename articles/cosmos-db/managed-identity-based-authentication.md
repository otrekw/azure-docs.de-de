---
title: 'Gewusst wie: Verwenden der systemseitig zugewiesenen verwalteten Identität für den Zugriff auf Azure Cosmos DB'
description: Erfahren Sie, wie Sie eine systemseitig zugewiesene verwaltete Azure AD-Identität für den Zugriff auf Schlüssel aus Azure Cosmos DB konfigurieren. MSI, verwaltete Dienstidentität, AAD, Azure Active Directory, Identität
author: j-patrick
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: justipat
ms.reviewer: sngun
ms.openlocfilehash: 102efba5d028eef621f392ef1739ea9ebeca0b44
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2020
ms.locfileid: "80416975"
---
# <a name="how-to-use-a-system-assigned-managed-identity-to-access-azure-cosmos-db-data"></a>Gewusst wie: Verwenden der systemseitig zugewiesenen verwalteten Identität für den Zugriff auf Azure Cosmos DB

In diesem Artikel richten Sie eine **robuste, schlüsselrotationsunabhängige** Lösung für den Zugriff auf Azure Cosmos DB-Schlüssel durch Nutzung [verwalteter Identitäten](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md) ein. Das Beispiel in diesem Artikel verwendet eine Azure-Funktion. Sie können diese Lösung jedoch mit jedem Dienst erreichen, der verwaltete Identitäten unterstützt. 

Sie erfahren, wie Sie eine Azure-Funktion erstellen, die auf Azure Cosmos DB zugreifen kann, ohne Azure Cosmos DB-Schlüssel kopieren zu müssen. Die Funktion wird jede Minute aktiviert und zeichnet die aktuelle Temperatur eines Aquariums auf. Informationen zum Einrichten einer durch einen Timer ausgelösten Azure-Funktion finden Sie im Artikel [Erstellen einer Funktion in Azure, die von einem Timer ausgelöst wird](../azure-functions/functions-create-scheduled-function.md).

Zur Vereinfachung des Szenarios erfolgt die Bereinigung älterer Temperaturdokumente durch eine bereits konfigurierte Einstellung für die [Gültigkeitsdauer](./time-to-live.md). 

## <a name="assign-a-system-assigned-managed-identity-to-an-azure-function"></a>Zuweisen einer vom System zugewiesenen verwalteten Identität zu einer Azure-Funktion

In diesem Schritt weisen Sie Ihrer Azure-Funktion eine vom System zugewiesene verwaltete Identität zu.

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) den Bereich **Azure-Funktion** und navigieren Sie zu Ihrer Funktions-App. 

1. Öffnen Sie die Registerkarte **Plattformfeatures** > **Identität**: 

   ![Registerkarte „Identität“](./media/managed-identity-based-authentication/identity-tab-selection.png)

1. Legen Sie auf der Registerkarte **Identität** den Status der **Systemidentität** auf **Ein** fest. Wählen Sie **Speichern** aus, und bestätigen Sie, dass Sie die Systemidentität aktivieren möchten. Danach sollte der Bereich **Systemidentität** folgendermaßen aussehen:  

   ![Systemidentität aktiviert](./media/managed-identity-based-authentication/identity-tab-system-managed-on.png)

## <a name="grant-the-managed-identity-access-to-your-azure-cosmos-account"></a>Gewähren der verwalteten Identität Zugriff auf Ihr Azure Cosmos-Konto

In diesem Schritt weisen Sie der vom System zugewiesenen verwalteten Identität der Azure-Funktion eine Rolle zu. Azure Cosmos DB umfasst mehrere integrierte Rollen, die Sie der verwalteten Identität zuweisen können. Im Rahmen dieser Lösung verwenden Sie die beiden folgenden Rollen:

|Integrierte Rolle  |BESCHREIBUNG  |
|---------|---------|
|[Mitwirkender von DocumentDB-Konto](../role-based-access-control/built-in-roles.md#documentdb-account-contributor)|Kann Azure Cosmos DB-Konten verwalten. Ermöglicht das Abrufen von Lese-/Schreibschlüsseln. |
|[Cosmos DB-Kontoleser](../role-based-access-control/built-in-roles.md#cosmos-db-account-reader-role)|Kann Azure Cosmos DB-Kontodaten lesen. Ermöglicht das Abrufen von Leseschlüsseln. |

> [!IMPORTANT]
> Die RBAC-Unterstützung in Azure Cosmos DB gilt nur für Vorgänge auf Steuerungsebene. Vorgänge auf Datenebene werden mithilfe von Hauptschlüsseln oder Ressourcentoken abgesichert. Weitere Informationen finden Sie im Artikel [Sicherer Zugriff auf Daten](secure-access-to-data.md).

> [!TIP] 
> Weisen Sie bei der Zuweisung von Rollen nur die erforderlichen Zugriffsrechte zu. Wenn Ihr Dienst nur Daten lesen muss, weisen Sie der verwalteten Identität die Rolle **Cosmos DB-Kontoleser** zu. Weitere Informationen zur Bedeutung des Zugriffs mit geringsten Rechten finden Sie im Artikel [Senken der Gefährdung privilegierter Konten](../security/fundamentals/identity-management-best-practices.md#lower-exposure-of-privileged-accounts).

In Ihrem Szenario lesen Sie die Temperatur und schreiben die betreffenden Daten dann in einen Container in Azure Cosmos DB. Da Sie die Daten schreiben müssen, verwenden Sie die Rolle **DocumentDB-Kontomitwirkender**. 

1. Melden Sie sich beim Azure-Portal an, und navigieren Sie zu Ihrem Azure Cosmos DB-Konto. Öffnen Sie den Bereich **Zugriffsverwaltung (IAM)** und dann die Registerkarte **Rollenzuweisungen**:

   ![IAM-Bereich](./media/managed-identity-based-authentication/cosmos-db-iam-tab.png)

1. Klicken Sie auf die Schaltfläche **+ Hinzufügen** und dann auf **Rollenzuweisung hinzufügen**.

1. Der Bereich **Rollenzuweisung hinzufügen** wird rechts geöffnet:

   ![Rolle hinzufügen](./media/managed-identity-based-authentication/cosmos-db-iam-tab-add-role-pane.png)

   * **Rolle** – Wählen Sie **DocumentDB-Kontomitwirkender** aus.
   * **Zugriff zuweisen zu** – Wählen Sie im Unterabschnitt **Systemseitig zugewiesene verwaltete Identität** die Option **Funktions-App** aus.
   * **Auswählen** – Im Bereich werden alle Funktions-Apps in Ihrem Abonnement eingetragen, die über eine **verwaltete Systemidentität** verfügen. In unserem Fall wird die Funktions-App **SummaryService** ausgewählt: 

      ![Zuweisung auswählen](./media/managed-identity-based-authentication/cosmos-db-iam-tab-add-role-pane-filled.png)

1. Klicken Sie nach Auswahl der Identität der Funktions-App auf **Speichern**.

## <a name="programmatically-access-the-azure-cosmos-db-keys-from-the-azure-function"></a>Programmgesteuerter Zugriff auf die Azure Cosmos DB-Schlüssel in der Azure-Funktion

Jetzt verfügen wir über eine Funktions-App mit einer vom System zugewiesenen verwalteten Identität. Dieser Identität wurde die Rolle **DocumentDB-Kontomitwirkender** in den Azure Cosmos DB-Berechtigungen zugewiesen. Der folgende Funktions-App-Code ruft die Azure Cosmos DB-Schlüssel ab, erstellt ein CosmosClient-Objekt, ruft die Temperatur ab und speichert diese dann in Cosmos DB.

In diesem Beispiel wird die [List Keys-API](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/DatabaseAccounts/ListKeys) verwendet, um auf die Azure Cosmos DB Kontoschlüssel zuzugreifen.

> [!IMPORTANT] 
> Wenn Sie die [Rolle **Cosmos DB-Kontoleser** zuweisen](#grant-the-managed-identity-access-to-your-azure-cosmos-account) möchten, müssen Sie die schreibgeschützte [List Keys-API](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/DatabaseAccounts/ListReadOnlyKeys) verwenden. Dadurch werden nur die schreibgeschützten Schlüssel abgerufen.

Die List Keys-API gibt das `DatabaseAccountListKeysResult`-Objekt zurück. Dieser Typ ist in den C# Bibliotheken nicht definiert. Im folgenden Codebeispiel wird die Implementierung dieser Klasse veranschaulicht:  

```csharp 
namespace SummarizationService 
{
  public class DatabaseAccountListKeysResult
  {
      public string primaryMasterKey {get;set;}
      public string primaryReadonlyMasterKey {get; set;}
      public string secondaryMasterKey {get; set;}
      public string secondaryReadonlyMasterKey {get;set;}
  }
}
```

Im Beispiel wird außerdem ein einfaches Dokument mit dem Namen „TemperatureRecord“ verwendet, das wie folgt definiert ist:

```csharp
using System;

namespace Monitor
{
    public class TemperatureRecord
    {
        public string id { get; set; } = Guid.NewGuid().ToString();
        public DateTime RecordTime { get; set; }
        public int Temperature { get; set; }

    }
}
```

Sie verwenden die [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)-Bibliothek, um das vom System zugewiesene verwaltete Identitätstoken abzurufen. Weitere Möglichkeiten zum Abrufen des Tokens und weitere Informationen zur `Microsoft.Azure.Service.AppAuthentication`-Bibliothek finden Sie im Artikel [Dienst-zu-Dienst-Authentifizierung](../key-vault/service-to-service-authentication.md).

```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;
using Microsoft.Azure.Cosmos;
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;

namespace Monitor
{
    public static class TemperatureMonitor
    {
        private static string subscriptionId =
        "<azure subscription id>";
        private static string resourceGroupName =
        "<name of your azure resource group>";
        private static string accountName =
        "<Azure Cosmos DB account name>";
        private static string cosmosDbEndpoint =
        "<Azure Cosmos DB endpoint>";
        private static string databaseName =
        "<Azure Cosmos DB name>";
        private static string containerName =
        "<container to store the temperature in>";

        [FunctionName("TemperatureMonitor")]
        public static async Task Run([TimerTrigger("0 * * * * *")]TimerInfo myTimer, ILogger log)
        {
            log.LogInformation($"Starting temperature monitoring: {DateTime.Now}");

            // AzureServiceTokenProvider will help us to get the Service Managed token.
            var azureServiceTokenProvider = new AzureServiceTokenProvider();

            // In order to get the Service Managed token we need to authenticate to the Azure Resource Manager.
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://management.azure.com/");

            // To get the Azure Cosmos DB keys setup the List Keys API:
            string endpoint = $"https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DocumentDB/databaseAccounts/{accountName}/listKeys?api-version=2019-12-12";

            // setup an HTTP Client and add the access token.
            HttpClient httpClient = new HttpClient();
            httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

            // Post to the endpoint to get the keys result.
            var result = await httpClient.PostAsync(endpoint, new StringContent(""));

            // Get the Result back as a DatabaseAccountListKeysResult.
            DatabaseAccountListKeysResult keys = await result.Content.ReadAsAsync<DatabaseAccountListKeysResult>();

            log.LogInformation("Starting to create the client");

            CosmosClient client = new CosmosClient(cosmosDbEndpoint, keys.primaryMasterKey);

            log.LogInformation("Client created");

            var database = client.GetDatabase(databaseName);
            var container = database.GetContainer(containerName);

            log.LogInformation("Get the temperature.");

            var tempRecord = new TemperatureRecord() { RecordTime = DateTime.UtcNow, Temperature = GetTemperature() };

            log.LogInformation("Store temperature");

            await container.CreateItemAsync<TemperatureRecord>(tempRecord);

            log.LogInformation($"Ending temperature monitor: {DateTime.Now}");
        }

        private static int GetTemperature()
        {
            // fake the temperature sensor for this demo
            Random r = new Random(DateTime.UtcNow.Second);
            return r.Next(0, 120);
        }
    }
}
```

Sie können jetzt Ihre [Azure-Funktion bereitstellen](../azure-functions/functions-create-first-function-vs-code.md).

## <a name="next-steps"></a>Nächste Schritte

* [Zertifikatbasierte Authentifizierung mit Azure Cosmos DB und Active Directory](certificate-based-authentication.md)
* [Sichern von Azure Cosmos-Schlüsseln mit Azure Key Vault](access-secrets-from-keyvault.md)
* [Sicherheitsbaseline für Azure Cosmos DB](security-baseline.md)
