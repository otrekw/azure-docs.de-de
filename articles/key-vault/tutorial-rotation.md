---
title: Tutorial zur Rotation eines einzelnen Benutzers/Kennworts
description: Verwenden Sie dieses Tutorial, um zu erfahren, wie Sie die Rotation eines Geheimnisses für Ressourcen automatisieren, die die Authentifizierung mit einem einzelnen Benutzer/einzelnen Kennwort verwenden.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: rotation
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 01/26/2020
ms.author: mbaldwin
ms.openlocfilehash: 684bfccd698217acdafdaac8c33f1e5531cdb4bf
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80998896"
---
# <a name="automate-the-rotation-of-a-secret-for-resources-that-use-single-usersingle-password-authentication"></a>Automatisieren der Rotation eines Geheimnisses für Ressourcen, die die Authentifizierung mit einem einzelnen Benutzer/einzelnen Kennwort verwenden

Die beste Möglichkeit zur Authentifizierung bei Azure-Diensten ist die Verwendung einer [verwalteten Identität](managed-identity.md). Es gibt jedoch einige Szenarien, in denen dies nicht möglich ist. In diesen Fällen werden Zugriffsschlüssel oder Geheimnisse verwendet. Sie sollten Zugriffsschlüssel und Geheimnisse regelmäßig rotieren.

In diesem Tutorial wird gezeigt, wie Sie die regelmäßige Rotation von Geheimnissen für Datenbanken und Dienste automatisieren, die die Authentifizierung mit einem einzelnen Benutzer/Kennwort verwenden. In diesem Tutorial werden in Azure Key Vault gespeicherte SQL Server-Kennwörter mithilfe einer Funktion rotiert, die durch eine Azure Event Grid-Benachrichtigung ausgelöst wird:

![Diagramm der Rotationslösung](./media/rotate1.png)

1. 30 Tage vor dem Ablaufdatum eines Geheimnisses veröffentlicht Key Vault das Ereignis „Läuft demnächst ab“ in Event Grid.
1. Event Grid überprüft die Ereignisabonnements und ruft mit HTTP POST den Funktions-App-Endpunkt auf, der dieses Ereignis abonniert hat.
1. Die Funktions-App empfängt die Geheimnisinformationen, generiert ein neues zufälliges Kennwort und erstellt für das Geheimnis eine neue Version mit dem neuen Kennwort in Key Vault.
1. Die Funktions-App aktualisiert SQL Server mit dem neuen Kennwort.

> [!NOTE]
> Zwischen den Schritten 3 und 4 kann eine Verzögerung auftreten. Während dieser Zeit kann sich das Geheimnis in Key Vault nicht bei SQL Server authentifizieren. Tritt in einem der Schritte ein Fehler auf, wiederholt Event Grid den Vorgang zwei Stunden lang.

## <a name="create-a-key-vault-and-sql-server-instance"></a>Erstellen einer Key Vault- und SQL Server-Instanz

Im ersten Schritt werden eine Key Vault-Instanz, eine SQL Server-Instanz sowie eine Datenbank erstellt und das SQL Server-Administratorkennwort in Key Vault gespeichert.

In diesem Tutorial wird eine vorhandene Azure Resource Manager-Vorlage zum Erstellen von Komponenten verwendet. Hier finden Sie den Code: [Einfaches Vorlagenbeispiel für die Geheimnisrotation](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/arm-templates)

1. Wählen Sie den Link zur Bereitstellung der Vorlage in Azure aus:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Finitial-setup%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus. Nennen Sie die Gruppe **simplerotation**.
1. Wählen Sie die Option **Kaufen**.

    ![Erstellen einer Ressourcengruppe](./media/rotate2.png)

Sie verfügen jetzt über einen Schlüsseltresor, eine SQL Server-Instanz und eine SQL-Datenbank-Instanz. Sie können dieses Setup in der Azure CLI überprüfen, indem Sie den folgenden Befehl ausführen:

```azurecli
az resource list -o table
```

Das Ergebnis sieht etwa wie die folgende Ausgabe aus:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
simplerotation-kv          simplerotation      eastus      Microsoft.KeyVault/vaults
simplerotation-sql         simplerotation      eastus      Microsoft.Sql/servers
simplerotation-sql/master  simplerotation      eastus      Microsoft.Sql/servers/databases
```

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Erstellen Sie als Nächstes zusätzlich zu den weiteren erforderlichen Komponenten eine Funktions-App mit einer systemseitig verwalteten Identität.

Eine Funktions-App benötigt diese Komponenten:
- Einen Azure App Service-Plan
- Ein Speicherkonto
- Eine Zugriffsrichtlinie für den Zugriff auf Geheimnisse in Key Vault mithilfe der verwalteten Identität der Funktions-App

1. Wählen Sie den Link zur Bereitstellung der Vorlage in Azure aus:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Ffunction-app%2Fazuredeploy.json" target="_blank"><img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. Wählen Sie in der Liste **Ressourcengruppe** die Gruppe **simplerotation** aus.
1. Wählen Sie die Option **Kaufen**.

   ![Auswählen von „Kaufen“](./media/rotate3.png)

Nachdem Sie die obigen Schritte ausgeführt haben, verfügen Sie über ein Speicherkonto, eine Serverfarm und eine Funktions-App. Sie können dieses Setup in der Azure CLI überprüfen, indem Sie den folgenden Befehl ausführen:

```azurecli
az resource list -o table
```

Das Ergebnis sieht etwa wie die folgende Ausgabe aus:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
simplerotation-kv          simplerotation       eastus      Microsoft.KeyVault/vaults
simplerotation-sql         simplerotation       eastus      Microsoft.Sql/servers
simplerotation-sql/master  simplerotation       eastus      Microsoft.Sql/servers/databases
simplerotationstrg         simplerotation       eastus      Microsoft.Storage/storageAccounts
simplerotation-plan        simplerotation       eastus      Microsoft.Web/serverFarms
simplerotation-fn          simplerotation       eastus      Microsoft.Web/sites
```

Informationen zum Erstellen einer Funktions-Apps und Zugreifen auf Key Vault mit einer verwalteten Identität finden Sie unter [Erstellen einer Funktions-App im Azure-Portal](../azure-functions/functions-create-function-app-portal.md) und [Bereitstellen der Key Vault-Authentifizierung mit einer verwalteten Identität](managed-identity.md).

### <a name="rotation-function"></a>Rotationsfunktion
Die Funktion verwendet ein Ereignis, um die Rotation eines Geheimnisses durch Aktualisieren von Key Vault und der SQL-Datenbank-Instanz zu initiieren.

#### <a name="function-trigger-event"></a>Funktionsstriggerereignis

Diese Funktion liest Ereignisdaten und führt die Rotationslogik aus:

```csharp
public static class SimpleRotationEventHandler
{
    [FunctionName("SimpleRotation")]
       public static void Run([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
       {
            log.LogInformation("C# Event trigger function processed a request.");
            var secretName = eventGridEvent.Subject;
            var secretVersion = Regex.Match(eventGridEvent.Data.ToString(), "Version\":\"([a-z0-9]*)").Groups[1].ToString();
            var keyVaultName = Regex.Match(eventGridEvent.Topic, ".vaults.(.*)").Groups[1].ToString();
            log.LogInformation($"Key Vault Name: {keyVaultName}");
            log.LogInformation($"Secret Name: {secretName}");
            log.LogInformation($"Secret Version: {secretVersion}");

            SeretRotator.RotateSecret(log, secretName, secretVersion, keyVaultName);
        }
}
```

#### <a name="secret-rotation-logic"></a>Logik für die Geheimnisrotation
Diese Rotationsmethode liest Datenbankinformationen aus dem Geheimnis, erstellt eine neue Version des Geheimnisses und aktualisiert die Datenbank mit dem neuen Geheimnis:

```csharp
public class SecretRotator
    {
       private const string UserIdTagName = "UserID";
       private const string DataSourceTagName = "DataSource";
       private const int SecretExpirationDays = 31;

    public static void RotateSecret(ILogger log, string secretName, string secretVersion, string keyVaultName)
    {
           //Retrieve current secret
           var kvUri = "https://" + keyVaultName + ".vault.azure.net";
           var client = new SecretClient(new Uri(kvUri), new DefaultAzureCredential());
           KeyVaultSecret secret = client.GetSecret(secretName, secretVersion);
           log.LogInformation("Secret Info Retrieved");
        
           //Retrieve secret info
           var userId = secret.Properties.Tags.ContainsKey(UserIdTagName) ?  
                        secret.Properties.Tags[UserIdTagName] : "";
           var datasource = secret.Properties.Tags.ContainsKey(DataSourceTagName) ? 
                            secret.Properties.Tags[DataSourceTagName] : "";
           log.LogInformation($"Data Source Name: {datasource}");
           log.LogInformation($"User Id Name: {userId}");
        
           //Create new password
           var randomPassword = CreateRandomPassword();
           log.LogInformation("New Password Generated");
        
           //Check DB connection using existing secret
           CheckServiceConnection(secret);
           log.LogInformation("Service Connection Validated");
                    
           //Create new secret with generated password
           CreateNewSecretVersion(client, secret, randomPassword);
           log.LogInformation("New Secret Version Generated");
        
           //Update DB password
           UpdateServicePassword(secret, randomPassword);
           log.LogInformation("Password Changed");
           log.LogInformation($"Secret Rotated Succesffuly");
    }
}
```
Den vollständigen Beispielcode finden Sie auf [GitHub](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/rotation-function).

#### <a name="function-deployment"></a>Bereitstellung der Funktion

1. Laden Sie die ZIP-Datei der Funktions-App von [GitHub](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-fn.zip) herunter.

1. Laden Sie die Datei „simplerotationsample-fn.zip“ in Azure Cloud Shell hoch.

   ![Hochladen der Datei](./media/rotate4.png)
1. Stellen Sie die ZIP-Datei mit dem folgenden Azure CLI-Befehl in der Funktions-App bereit:

   ```azurecli
   az functionapp deployment source config-zip -g simplerotation -n simplerotation-fn --src /home/{firstname e.g jack}/simplerotationsample-fn.zip
   ```

Nachdem die Funktion bereitgestellt wurde, sollten unter „simplerotations-fn“ zwei Funktionen angezeigt werden:

![Funktionen SimpleRotation und SimpleRotationHttpTest](./media/rotate5.png)

## <a name="add-an-event-subscription-for-the-secretnearexpiry-event"></a>Hinzufügen eines Ereignisabonnements für das SecretNearExpiry-Ereignis

Kopieren Sie den `eventgrid_extension`-Schlüssel der Funktions-App:

   ![„Funktionen-App-Einstellungen“ auswählen](./media/rotate6.png)

   ![eventgrid_extension-Schlüssel](./media/rotate7.png)

Geben Sie im folgenden Befehl den kopierten `eventgrid_extension`-Schlüssel und Ihre Abonnement-ID an, um ein Event Grid-Abonnement für `SecretNearExpiry`-Ereignisse zu erstellen:

```azurecli
az eventgrid event-subscription create --name simplerotation-eventsubscription --source-resource-id "/subscriptions/<subscription-id>/resourceGroups/simplerotation/providers/Microsoft.KeyVault/vaults/simplerotation-kv" --endpoint "https://simplerotation-fn.azurewebsites.net/runtime/webhooks/EventGrid?functionName=SimpleRotation&code=<extension-key>" --endpoint-type WebHook --included-event-types "Microsoft.KeyVault.SecretNearExpiry"
```

## <a name="add-the-secret-to-key-vault"></a>Hinzufügen des Geheimnisses zu Key Vault
Erteilen Sie Benutzern in Ihrer Zugriffsrichtlinie die Berechtigungen zum *Verwalten von Geheimnissen*:

```azurecli
az keyvault set-policy --upn <email-address-of-user> --name simplerotation-kv --secret-permissions set delete get list
```

Erstellen Sie einen neuen geheimen Schlüssel mit Tags, die die SQL-Datenbank-Datenquelle und die Benutzer-ID enthalten. Fügen Sie ein Ablaufdatum ein, das für morgen festgelegt ist.

```azurecli
$tomorrowDate = (get-date).AddDays(+1).ToString("yyy-MM-ddThh:mm:ssZ")
az keyvault secret set --name sqluser --vault-name simplerotation-kv --value "Simple123" --tags "UserID=azureuser" "DataSource=simplerotation-sql.database.windows.net" --expires $tomorrowDate
```

Wenn Sie ein Geheimnis mit einem kurzen Ablaufdatum erstellen, wird sofort ein `SecretNearExpiry`-Ereignis veröffentlicht, das wiederum die Funktion zum Rotieren des Geheimnisses auslöst.

## <a name="test-and-verify"></a>Testen und Überprüfen
Nach einigen Minuten sollte das `sqluser`-Geheimnis automatisch rotiert werden.

Um sicherzustellen, dass das Geheimnis rotiert wurde, wechseln Sie zu **Key Vault** > **Geheimnisse**:

![Wechseln zu Geheimnissen](./media/rotate8.png)

Öffnen Sie das Geheimnis **sqluser**, und überprüfen Sie die alte sowie die rotierte aktuelle Version:

![Öffnen des sqluser-Geheimnisses](./media/rotate9.png)

### <a name="create-a-web-app"></a>Erstellen einer Web-App

Erstellen Sie zum Überprüfen der SQL-Anmeldeinformationen eine Web-App. Diese Web-App ruft das Geheimnis aus Key Vault ab, extrahiert SQL-Datenbank- und -Anmeldeinformationen aus dem Geheimnis und testet die Verbindung mit SQL Server.

Die Web-App benötigt diese Komponenten:
- Eine Web-App mit systemseitig verwalteter Identität
- Eine Zugriffsrichtlinie für den Zugriff auf Geheimnisse in Key Vault mithilfe der verwalteten Identität der Web-App

1. Wählen Sie den Link zur Bereitstellung der Vorlage in Azure aus:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Fweb-app%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. Wählen Sie die Ressourcengruppe **simplerotation** aus.
1. Wählen Sie die Option **Kaufen**.

### <a name="deploy-the-web-app"></a>Bereitstellen der Web-App

Quellcode für die Web-App finden Sie auf [GitHub](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/test-webapp).

Führen Sie die folgenden Schritte aus, um die Web-App bereitzustellen:

1. Laden Sie die ZIP-Datei der Funktions-App von [GitHub](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-app.zip) herunter.
1. Laden Sie die Datei „simplerotationsample-app.zip“ in Azure Cloud Shell hoch.
1. Stellen Sie die ZIP-Datei mit dem folgenden Azure CLI-Befehl in der Funktions-App bereit:

   ```azurecli
   az webapp deployment source config-zip -g simplerotation -n simplerotation-app --src /home/{firstname e.g jack}/simplerotationsample-app.zip
   ```

### <a name="open-the-web-app"></a>Öffnen der Web-App

Wechseln Sie zur bereitgestellten App, und wählen Sie die URL aus:
 
![Auswählen der URL](./media/rotate10.png)

Der generierte Geheimniswert sollte mit dem Wert „true“ für „Mit Datenbank verbunden“ angezeigt werden.

## <a name="learn-more"></a>Weitere Informationen

- Übersicht: [Überwachen von Key Vault mit Azure Event Grid (Vorschau)](event-grid-overview.md)
- Gewusst wie: [Verwenden von Logic Apps zum Empfangen einer E-Mail bei Statusänderungen von Key Vault-Geheimnissen](event-grid-logicapps.md)
- [Azure Event Grid-Ereignisschema für Azure Key Vault (Vorschau)](../event-grid/event-schema-key-vault.md)
