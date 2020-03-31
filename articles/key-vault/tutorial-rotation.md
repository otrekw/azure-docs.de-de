---
title: Tutorial zur Rotation eines einzelnen Benutzers/Kennworts
description: In diesem Tutorial erfahren Sie, wie Sie die Rotation eines einzelnen Benutzers/Kennworts automatisieren.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: rotation
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 01/26/2020
ms.author: mbaldwin
ms.openlocfilehash: 890932f7c0e46a2c9c0b6e1cf1461e4d7d25b409
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "79223357"
---
# <a name="automate-the-rotation-of-a-secret-for-resources-with-single-userpassword-authentication"></a>Automatisieren der Rotation eines Geheimnisses für Ressourcen mit Authentifizierung mit einem einzelnen Benutzer/Kennwort

Die beste Möglichkeit zur Authentifizierung bei Azure-Diensten ist die Verwendung einer [verwalteten Identität](managed-identity.md). Es gibt jedoch einige Szenarien, in denen dies nicht möglich ist. In diesen Fällen werden Zugriffsschlüssel oder Geheimnisse verwendet. Zugriffsschlüssel und Geheimnisse sollten regelmäßig rotiert werden.

In diesem Tutorial wird gezeigt, wie Sie die regelmäßige Rotation von Geheimnissen für Datenbanken und Dienste mit Authentifizierung mit einem einzelnen Benutzer/Kennwort automatisieren. In diesem Szenario werden in Key Vault gespeicherte SQL Server-Kennwörter mithilfe einer Funktion rotiert, die durch eine Event Grid-Benachrichtigung ausgelöst wird:

![Diagramm zur Rotation](./media/rotate1.png)

1. 30 Tage vor dem Ablaufdatum eines Geheimnisses veröffentlicht Key Vault das Ereignis „Läuft demnächst ab“ in Event Grid.
1. Event Grid überprüft die Ereignisabonnements und ruft mit HTTP POST den Funktions-App-Endpunkt auf, der dieses Ereignis abonniert hat.
1. Die Funktions-App empfängt die Geheimnisinformationen, generiert ein neues zufälliges Kennwort und erstellt für das Geheimnis eine neue Version mit einem neuen Kennwort in Key Vault.
1. Die Funktions-App aktualisiert SQL mit dem neuen Kennwort.

> [!NOTE]
> Zwischen Schritt 3 und 4 kann eine Verzögerung auftreten, während der das Geheimnis in Key Vault nicht für die Authentifizierung bei SQL gültig ist. Tritt in einem der Schritte ein Fehler auf, wiederholt Event Grid den Vorgang zwei Stunden lang.

## <a name="setup"></a>Einrichten

## <a name="create-a-key-vault-and-sql-server"></a>Erstellen einer Key Vault- und SQL Server-Instanz

Bevor wir beginnen, müssen wir eine Key Vault-Instanz, eine SQL Server-Instanz sowie eine Datenbank erstellen und das SQL Server-Administratorkennwort in Key Vault speichern.

In diesem Tutorial wird eine vorab erstellte Azure Resource Manager-Vorlage zum Erstellen von Komponenten verwendet. Den vollständigen Code finden Sie hier: [Einfaches Vorlagenbeispiel für die Geheimnisrotation](https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/arm-templates)

1. Klicken Sie auf den Link zur Bereitstellung der Vorlage in Azure:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Finitial-setup%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. Wählen Sie unter „Ressourcengruppe“ die Option „Neu erstellen“ aus, und geben Sie den Namen „simplerotation“ ein.
1. Wählen Sie „Kaufen“ aus.

    ![Erstellen einer neuen Ressourcengruppe](./media/rotate2.png)

Nachdem Sie diese Schritte ausgeführt haben, verfügen Sie über eine Key Vault-Instanz, eine SQL Server-Instanz und eine SQL-Datenbank. Sie können überprüfen, ob diese Komponenten erstellt wurden, indem Sie in einem Azure CLI-Terminal den folgenden Befehl ausführen:

```azurecli
az resource list -o table
```

Die Ergebnisse sehen in etwa wie folgt aus:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
simplerotation-kv             simplerotation             eastus      Microsoft.KeyVault/vaults
simplerotation-sql            simplerotation             eastus      Microsoft.Sql/servers
simplerotation-sql/master     simplerotation             eastus      Microsoft.Sql/servers/databases
```

## <a name="create-function-app"></a>Erstellen einer Funktions-App

Erstellen Sie eine Funktions-App mit einer systemseitig verwalteten Identität sowie die weiteren erforderlichen Komponenten: 

Die Funktions-App erfordert die folgenden Komponenten und die unten stehende Konfiguration:
- App Service-Plan
- Speicherkonto
- Zugriffsrichtlinie für den Zugriff auf Geheimnisse in Key Vault mithilfe der verwalteten Identität der Funktions-App

1. Klicken Sie auf den Link zur Bereitstellung der Vorlage in Azure:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Ffunction-app%2Fazuredeploy.json" target="_blank"><img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. Wählen Sie die Ressourcengruppe „simplerotation“ aus.
1. Wählen Sie „Kaufen“ aus.

   ![Bildschirm „Kaufen“](./media/rotate3.png)

Nachdem Sie die obigen Schritte ausgeführt haben, verfügen Sie über ein Speicherkonto, eine Serverfarm und eine Funktions-App.  Sie können überprüfen, ob diese Komponenten erstellt wurden, indem Sie in einem Azure CLI-Terminal den folgenden Befehl ausführen:

```azurecli
az resource list -o table
```

Die Ergebnisse sehen in etwa wie folgt aus:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
simplerotation-kv             simplerotation             eastus      Microsoft.KeyVault/vaults
simplerotation-sql            simplerotation             eastus      Microsoft.Sql/servers
simplerotation-sql/master     simplerotation             eastus      Microsoft.Sql/servers/databases
simplerotationstrg            simplerotation             eastus      Microsoft.Storage/storageAccounts
simplerotation-plan           simplerotation             eastus      Microsoft.Web/serverFarms
simplerotation-fn             simplerotation             eastus      Microsoft.Web/sites
```

Informationen zum Erstellen von Funktions-Apps und Zugreifen auf Key Vault mit einer verwalteten Identität finden Sie unter [Erstellen einer Funktions-App im Azure-Portal](../azure-functions/functions-create-function-app-portal.md) und [Bereitstellen der Key Vault-Authentifizierung mit einer verwalteten Identität](managed-identity.md).

### <a name="rotation-function-and-deployment"></a>Rotationsfunktion und Bereitstellung
Die Funktion verwendet ein Ereignis als Auslöser und rotiert ein Geheimnis, indem Key Vault und die SQL-Datenbank aktualisiert werden.

#### <a name="function-event-trigger-handler"></a>Ereignisauslöserhandler der Funktion

Die folgende Funktion liest Ereignisdaten und führt eine Rotationslogik aus.

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
Diese Rotationsmethode liest Datenbankinformationen aus dem Geheimnis, erstellt eine neue Version des Geheimnisses und aktualisiert die Datenbank mit einem neuen Geheimnis.

```csharp
public class SecretRotator
    {
       private const string UserIdTagName = "UserID";
       private const string DataSourceTagName = "DataSource";
       private const int SecretExpirationDays = 31;

    public static void RotateSecret(ILogger log, string secretName, string secretVersion, string keyVaultName)
    {
           //Retrieve Current Secret
           var kvUri = "https://" + keyVaultName + ".vault.azure.net";
           var client = new SecretClient(new Uri(kvUri), new DefaultAzureCredential());
           KeyVaultSecret secret = client.GetSecret(secretName, secretVersion);
           log.LogInformation("Secret Info Retrieved");
        
           //Retrieve Secret Info
           var userId = secret.Properties.Tags.ContainsKey(UserIdTagName) ?  
                        secret.Properties.Tags[UserIdTagName] : "";
           var datasource = secret.Properties.Tags.ContainsKey(DataSourceTagName) ? 
                            secret.Properties.Tags[DataSourceTagName] : "";
           log.LogInformation($"Data Source Name: {datasource}");
           log.LogInformation($"User Id Name: {userId}");
        
           //create new password
           var randomPassword = CreateRandomPassword();
           log.LogInformation("New Password Generated");
        
           //Check db connection using existing secret
           CheckServiceConnection(secret);
           log.LogInformation("Service Connection Validated");
                    
           //Create new secret with generated password
           CreateNewSecretVersion(client, secret, randomPassword);
           log.LogInformation("New Secret Version Generated");
        
           //Update db password
           UpdateServicePassword(secret, randomPassword);
           log.LogInformation("Password Changed");
           log.LogInformation($"Secret Rotated Succesffuly");
    }
}
```
Den vollständigen Code finden Sie hier: https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/rotation-function

#### <a name="function-deployment"></a>Bereitstellung der Funktion

1. Laden Sie die ZIP-Datei der Funktions-App herunter: https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-fn.zip

1. Laden Sie die Datei „simplerotationsample-fn.zip“ in Azure Cloud Shell hoch.
 
1. Stellen Sie die ZIP-Datei mit dem folgenden CLI-Befehl in der Funktions-App bereit:

```azurecli
az functionapp deployment source config-zip -g simplerotation -n simplerotation-fn --src /home/{firstname e.g jack}/simplerotationsample-fn.zip
```
![Bildschirm „Kaufen“](./media/rotate4.png)

Nach der Bereitstellung sollten unter „simplerotation-fn“ zwei Funktionen angezeigt werden:

![Azure Cloud Shell](./media/rotate5.png)

### <a name="add-event-subscription-for-secretnearexpiry-event"></a>Hinzufügen des Ereignisabonnements für „SecretNearExpiry“

Kopieren Sie den Schlüssel „eventgrid_extension“ der Funktions-App.

![Azure Cloud Shell](./media/rotate6.png)

![Testen und Überprüfen](./media/rotate7.png)

Geben Sie im folgenden Befehl den kopierten Schlüssel „eventgrid_extension“ und Ihre Abonnement-ID an, um ein Event Grid-Abonnement für SecretNearExpiry-Ereignisse zu erstellen.

```azurecli
az eventgrid event-subscription create --name simplerotation-eventsubscription --source-resource-id "/subscriptions/<subscription-id>/resourceGroups/simplerotation/providers/Microsoft.KeyVault/vaults/simplerotation-kv" --endpoint "https://simplerotation-fn.azurewebsites.net/runtime/webhooks/EventGrid?functionName=SimpleRotation&code=<extension-key>" --endpoint-type WebHook --included-event-types "Microsoft.KeyVault.SecretNearExpiry"
```

### <a name="add-secret-to-key-vault"></a>Hinzufügen des Geheimnisses zu Key Vault
Erteilen Sie Benutzern in Ihrer Zugriffsrichtlinie die Berechtigung „Geheimnisse verwalten“.

```azurecli
az keyvault set-policy --upn <email-address-of-user> --name simplerotation-kv --secret-permissions set delete get list
```

Erstellen Sie jetzt ein neues Geheimnis mit Tags, die die Datenquelle der SQL-Datenbank und die Benutzer-ID enthalten, und legen Sie das Ablaufdatum auf morgen fest.

```azurecli
$tomorrowDate = (get-date).AddDays(+1).ToString("yyy-MM-ddThh:mm:ssZ")
az keyvault secret set --name sqluser --vault-name simplerotation-kv --value "Simple123" --tags "UserID=azureuser" "DataSource=simplerotation-sql.database.windows.net" --expires $tomorrowDate
```

Wenn Sie ein Geheimnis mit einem kurzen Ablaufdatum erstellen, wird sofort ein SecretNearExpiry-Ereignis veröffentlicht, das wiederum die Funktion zum Rotieren des Geheimnisses auslöst.

### <a name="test-and-verify"></a>Testen und Überprüfen
Nach einigen Minuten sollte das Geheimnis „sqluser“ automatisch rotiert werden.

Navigieren Sie zu „Key Vault“ > „Geheimnisse“, um die Geheimnisrotation zu überprüfen.

![Testen und Überprüfen](./media/rotate8.png)

Öffnen Sie das Geheimnis „sqluser“, und überprüfen Sie die alte sowie die rotierte aktuelle Version.

![Testen und Überprüfen](./media/rotate9.png)

## <a name="create-web-app"></a>Web-App erstellen

Erstellen Sie zum Überprüfen der SQL-Anmeldeinformationen eine Web-App. Diese Web-App ruft das Geheimnis aus Key Vault ab, extrahiert SQL-Datenbank- und -Anmeldeinformationen aus dem Geheimnis und testet die Verbindung mit SQL.

Die Web-App erfordert die folgenden Komponenten und die unten stehende Konfiguration:
- Web-App mit systemseitig verwalteter Identität
- Zugriffsrichtlinie für den Zugriff auf Geheimnisse in Key Vault mithilfe der verwalteten Identität der Web-App

1. Klicken Sie auf den Link zur Bereitstellung der Vorlage in Azure:
<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2Fazure-keyvault-basicrotation-tutorial%2Fmaster%2Farm-templates%2Fweb-app%2Fazuredeploy.json" target="_blank"> <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/></a>
1. Wählen Sie die Ressourcengruppe **simplerotation** aus.
1. Klicken Sie auf „Kaufen“.

### <a name="deploy-web-app"></a>Bereitstellen der Web-App

Den Quellcode für die Web-App finden Sie unter https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/tree/master/test-webapp. Gehen Sie zum Bereitstellen der Web-App wie folgt vor:

1. Laden Sie die ZIP-Datei der Funktions-App von https://github.com/jlichwa/azure-keyvault-basicrotation-tutorial/raw/master/simplerotationsample-app.zip herunter.
1. Laden Sie die Datei `simplerotationsample-app.zip` in Azure Cloud Shell hoch.
1. Stellen Sie die ZIP-Datei mit dem folgenden Azure CLI-Befehl in der Funktions-App bereit:

   ```azurecli
   az webapp deployment source config-zip -g simplerotation -n simplerotation-app --src /home/{firstname e.g jack}/simplerotationsample-app.zip
   ```

#### <a name="open-web-application"></a>Öffnen der Web-App

Wechseln Sie zur bereitgestellten App, und klicken Sie auf „URL“:
 
![Testen und Überprüfen](./media/rotate10.png)

Der generierte Geheimniswert sollte mit dem Wert „true“ für „Database Connected“ angezeigt werden.

## <a name="learn-more"></a>Weitere Informationen:

- Übersicht: [Überwachen von Key Vault mit Azure Event Grid (Vorschau)](event-grid-overview.md)
- Gewusst wie: [Verwenden von Logic Apps zum Empfangen einer E-Mail bei Statusänderungen von Key Vault-Geheimnissen](event-grid-logicapps.md)
- [Azure Event Grid-Ereignisschema für Azure Key Vault (Vorschau)](../event-grid/event-schema-key-vault.md)
