---
title: Automatisches Sichern von Schlüsselwerten aus dem Azure App Configuration-Speicher
description: Erfahren Sie, wie Sie eine automatische Sicherung von Schlüsselwerten zwischen App Configuration-Speichern einrichten.
services: azure-app-configuration
author: avanigupta
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: how-to
ms.date: 04/27/2020
ms.author: avgupta
ms.openlocfilehash: 0b057172c822f893e602d60f77a285f0867cf368
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87367757"
---
# <a name="backup-app-configuration-stores-automatically"></a>Automatisches Sichern von App Configuration-Speichern

In diesem Artikel erfahren Sie, wie Sie eine automatische Sicherung von Schlüsselwerten aus einem primären App Configuration-Speicher in einem sekundären Speicher einrichten. Dabei wird die Integration von Azure Event Grid in App Configuration genutzt. Nach der Einrichtung veröffentlicht App Configuration bei allen Änderungen, die an Schlüsselwerten in einem Konfigurationsspeicher vorgenommen wurden, Ereignisse in Event Grid. Event Grid unterstützt eine Vielzahl von Azure-Diensten, aus denen Benutzer die Ereignisse abonnieren können, die auftreten, wenn Schlüsselwerte erstellt, aktualisiert oder gelöscht werden.

## <a name="overview"></a>Übersicht

In diesem Tutorial empfangen Sie mit einer Azure Storage-Warteschlange Ereignisse von Event Grid und verarbeiten mit einem Timertrigger von Azure Functions Ereignisse in der Storage-Warteschlange in Batches. Wenn die Funktion aufgrund der Ereignisse ausgelöst wird, ruft sie die neuesten Werte der Schlüssel, die sich geändert haben, aus dem primären App Configuration-Speicher ab und aktualisiert den sekundären Speicher entsprechend. Dieses Setup unterstützt das Kombinieren mehrerer, in einem kurzen Zeitraum in einem Sicherungsvorgang stattfindender Änderungen und vermeidet übermäßige Anforderungen an Ihre App Configuration-Speicher.

![Architektur der App Configuration-Speichersicherung](./media/config-store-backup-architecture.png)

## <a name="resource-provisioning"></a>Bereitstellung von Ressourcen

Hinter der Sicherung von App Configuration-Speichern steht die Motivation, mehrere Konfigurationsspeicher in verschiedenen Azure-Regionen zu verwenden, um die Georesilienz Ihrer Anwendung zu erhöhen. Um dies zu erreichen, sollten sich Ihre primären und sekundären Speicher in unterschiedlichen Azure-Regionen befinden. Alle anderen Ressourcen, die in diesem Tutorial erstellt werden, können in einer beliebigen Region Ihrer Wahl bereitgestellt werden. Der Grund ist: Wenn die primäre Region ausgefallen ist, gibt es nichts Neues zum Sichern, solange nicht wieder auf die primäre Region zugegriffen werden kann.

In diesem Tutorial erstellen Sie einen sekundären Speicher in der Region `centralus` und alle anderen Ressourcen in der Region `westus`.

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/) Sie können optional Azure Cloud Shell verwenden.
- [Visual Studio 2019](https://visualstudio.microsoft.com/vs) mit der Workload Azure-Entwicklung.
- Laden Sie das [.NET Core SDK](https://dotnet.microsoft.com/download) herunter, und installieren Sie es.
- Aktuelle Version von Azure CLI (2.3.1 oder höher). Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli). Falls Sie die Azure CLI verwenden, müssen Sie sich erst mithilfe von `az login` anmelden. Sie können optional Azure Cloud Shell verwenden.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Die Azure-Ressourcengruppe ist eine logische Sammlung, in der Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mithilfe des Befehls [az group create](/cli/azure/group) eine Ressourcengruppe.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `<resource_group_name>` am Standort `westus` erstellt.  Ersetzen Sie `<resource_group_name>` durch einen eindeutigen Namen für Ihre Ressourcengruppe.

```azurecli-interactive
resourceGroupName="<resource_group_name>"
az group create --name $resourceGroupName --location westus
```

## <a name="create-app-configuration-stores"></a>Erstellen eines App Configuration-Speichers

Erstellen Sie Ihren primären und sekundären App Configuration-Speicher in unterschiedlichen Regionen.
Ersetzen Sie  `<primary_appconfig_name>` und `<secondary_appconfig_name>` durch eindeutige Namen für ihre Konfigurationsspeicher. Der Speichername muss eindeutig sein, da er als DNS-Name verwendet wird.

```azurecli-interactive
primaryAppConfigName="<primary_appconfig_name>"
secondaryAppConfigName="<secondary_appconfig_name>"
az appconfig create \
  --name $primaryAppConfigName \
  --location westus \
  --resource-group $resourceGroupName\
  --sku standard

az appconfig create \
  --name $secondaryAppConfigName \
  --location centralus \
  --resource-group $resourceGroupName\
  --sku standard
```

## <a name="create-azure-storage-queue"></a>Erstellen der Azure Storage-Warteschlange

Erstellen Sie ein Speicherkonto und eine Speicherwarteschlange zum Empfangen von Ereignissen, die von Event Grid veröffentlicht werden.

```azurecli-interactive
storageName="<unique_storage_name>"
queueName="<queue_name>"
az storage account create -n $storageName -g $resourceGroupName -l westus --sku Standard_LRS
az storage queue create --name $queueName --account-name $storageName --auth-mode login
```

[!INCLUDE [event-grid-register-provider-cli.md](../../includes/event-grid-register-provider-cli.md)]

## <a name="subscribe-to-your-app-configuration-store-events"></a>Abonnieren Ihrer App Configuration-Speicherereignisse

Sie abonnieren diese beiden Ereignisse aus dem primären App Configuration-Speicher:

- `Microsoft.AppConfiguration.KeyValueModified`
- `Microsoft.AppConfiguration.KeyValueDeleted`

Mit dem folgenden Befehl wird ein Event Grid-Abonnement für die beiden Ereignisse erstellt, die an die Storage-Warteschlange gesendet werden, wobei der Endpunkttyp auf `storagequeue` und der Endpunkt auf die Warteschlangen-ID festgelegt ist. Ersetzen Sie `<event_subscription_name>` durch den Namen Ihrer Wahl für das Ereignisabonnement.

```azurecli-interactive
storageId=$(az storage account show --name $storageName --resource-group  $resourceGroupName --query id --output tsv)
queueId="$storageId/queueservices/default/queues/$queueName"
appconfigId=$(az appconfig show --name $primaryAppConfigName --resource-group $resourceGroupName --query id --output tsv)
eventSubscriptionName="<event_subscription_name>"
az eventgrid event-subscription create \
  --source-resource-id $appconfigId \
  --name $eventSubscriptionName \
  --endpoint-type storagequeue \
  --endpoint $queueId \
  --included-event-types Microsoft.AppConfiguration.KeyValueModified Microsoft.AppConfiguration.KeyValueDeleted 
```

## <a name="create-azure-functions-for-handling-events-from-storage-queue"></a>Erstellen von Azure-Funktionen für die Behandlung von Ereignissen aus der Storage-Warteschlange

### <a name="setup-with-ready-to-use-azure-functions"></a>Setup mit sofort einsatzbereiten Azure-Funktionen

In diesem Tutorial arbeiten Sie mit C# Azure Functions mit den folgenden Eigenschaften:
- Runtime Stack .NET Core 3.1
- Azure Functions Runtimeversion 3.x
- Funktion wird vom Timer alle 10 Minuten ausgelöst

Um Ihnen die Sicherung Ihrer Daten zu erleichtern, haben wir [Azure-Funktionen](https://github.com/Azure/AppConfiguration/tree/master/examples/ConfigurationStoreBackup) getestet und veröffentlicht, die Sie verwenden können, ohne Änderungen am Code vorzunehmen. Laden Sie die Projektdateien herunter, und [veröffentlichen Sie sie in ihrer eigenen Azure-Funktions-App aus Visual Studio](/azure/azure-functions/functions-develop-vs#publish-to-azure).

> [!IMPORTANT]
> Nehmen Sie keine Änderungen an den Umgebungsvariablen im heruntergeladen Code vor. Im nächsten Abschnitt erstellen Sie die erforderlichen App-Einstellungen.
>

### <a name="build-your-own-azure-functions"></a>Erstellen Sie Ihre eigenen Azure-Funktionen

Wenn der oben angegebene Beispielcode nicht Ihren Anforderungen entspricht, können Sie auch eigene Azure-Funktionen erstellen. Die Funktion muss die folgenden Aufgaben ausführen können, um die Sicherung abzuschließen:
- Lesen Sie regelmäßig Inhalte Ihrer Speicherwarteschlange, um zu ermitteln, ob sie Benachrichtigungen von Event Grid enthält. Implementierungsdetails finden Sie unter [Schnellstart: Azure Queue Storage-Clientbibliothek v12 für .NET](/azure/storage/queues/storage-quickstart-queues-dotnet).
- Wenn die Speicherwarteschlange [Ereignisbenachrichtigungen von Event Grid](/azure/azure-app-configuration/concept-app-configuration-event?branch=pr-en-us-112982#event-schema) enthält, extrahieren Sie alle eindeutigen <Schlüssel, Bezeichnungen> aus den Ereignisbenachrichtigungen. Die Kombination aus Schlüssel und Bezeichnung ist der eindeutige Bezeichner für Schlüssel-Wert-Änderungen im primären Speicher.
- Lesen Sie alle Einstellungen aus dem primären Speicher. Aktualisieren Sie nur die Einstellungen im sekundären Speicher, für die ein entsprechendes Ereignis in der Speicherwarteschlange vorliegt. Löschen Sie alle Einstellungen aus dem sekundären Speicher, die in der Speicherwarteschlange, aber nicht im primären Speicher vorhanden waren. Sie können das [App Configuration-SDK](https://github.com/Azure/AppConfiguration#sdks) nutzen, um programmgesteuert auf ihre Konfigurationsspeicher zuzugreifen.
- Löschen Sie Nachrichten aus der Speicherwarteschlange, wenn während der Verarbeitung keine Ausnahmen aufgetreten sind.
- Stellen Sie sicher, dass Sie die Fehlerbehandlung entsprechend Ihren Anforderungen implementieren. Im obigen Codebeispiel finden Sie einige allgemeine Ausnahmen, die Sie möglicherweise behandeln möchten.

Weitere Informationen zum Erstellen von Azure-Funktionen finden Sie unter: [Erstellen einer Funktion in Azure, die von einem Timer ausgelöst wird](/azure/azure-functions/functions-create-scheduled-function) und [Entwickeln von Azure Functions mithilfe von Visual Studio](/azure/azure-functions/functions-develop-vs).


> [!IMPORTANT]
> Wählen Sie den Timerzeitplan abhängig davon, wie oft Sie Änderungen an Ihrem primären Konfigurationsspeicher vornehmen, nach bestem Wissen aus. Beachten Sie, dass das zu häufige Ausführen der Funktion zu einer Drosselung von Anforderungen an den Speicher führen kann.
>


## <a name="create-azure-function-app-settings"></a>Erstellen von Azure-Funktions-App-Einstellungen

Wenn Sie die von uns bereitgestellte Azure Functions-Version verwenden, sind die folgenden App-Einstellungen in Ihrer Azure-Funktions-App erforderlich:
- `PrimaryStoreEndpoint`: Endpunkt für den primären App Configuration-Speicher. Zum Beispiel, `https://{primary_appconfig_name}.azconfig.io`
- `SecondaryStoreEndpoint`: Endpunkt für den sekundären App Configuration-Speicher. Zum Beispiel, `https://{secondary_appconfig_name}.azconfig.io`
- `StorageQueueUri`: Speicherwarteschlangen-URI. Zum Beispiel, `https://{unique_storage_name}.queue.core.windows.net/{queue_name}`

Der folgende Befehl erstellt die erforderlichen App-Einstellungen in Ihrer Azure-Funktions-App. Ersetzen Sie `<function_app_name>` durch den Namen der Azure Funktions-App.

```azurecli-interactive
functionAppName="<function_app_name>"
primaryStoreEndpoint="https://$primaryAppConfigName.azconfig.io"
secondaryStoreEndpoint="https://$secondaryAppConfigName.azconfig.io"
storageQueueUri="https://$storageName.queue.core.windows.net/$queueName"
az functionapp config appsettings set --name $functionAppName --resource-group $resourceGroupName --settings StorageQueueUri=$storageQueueUri PrimaryStoreEndpoint=$primaryStoreEndpoint SecondaryStoreEndpoint=$secondaryStoreEndpoint
```


## <a name="grant-access-to-the-managed-identity-of-azure-function-app"></a>Gewähren des Zugriffs auf die verwaltete Identität der Azure-Funktions-App

Fügen Sie mit dem folgenden Befehl oder über das [Azure-Portal](/azure/app-service/overview-managed-identity#add-a-system-assigned-identity) eine vom System zugewiesene verwaltete Identität für Ihre Azure-Funktions-App hinzu.

```azurecli-interactive
az functionapp identity assign --name $functionAppName --resource-group $resourceGroupName
```

> [!NOTE]
> Ihr Konto muss über die Berechtigung `'Owner'` für den geeigneten Bereich (Ihr Abonnement oder die Ressourcengruppe) verfügen, um die erforderliche Ressourcenerstellung und eine Rollenverwaltung durchführen zu können. Hilfe zu Rollenzuweisungen finden Sie ggf. unter [Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und dem Azure-Portal](/azure/role-based-access-control/role-assignments-portal).

Gewähren Sie mit den folgenden Befehlen oder über das [Azure-Portal](/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity#grant-access-to-app-configuration) der verwalteten Identität Ihrer Azure-Funktions-App Zugriff auf Ihre App Configuration-Speicher.
- Weisen Sie die `App Configuration Data Reader`-Rolle im primären App Configuration-Speicher zu.
- Weisen Sie die `App Configuration Data Owner`-Rolle im sekundären App Configuration-Speicher zu.

```azurecli-interactive
functionPrincipalId=$(az functionapp identity show --name $functionAppName --resource-group  $resourceGroupName --query principalId --output tsv)
primaryAppConfigId=$(az appconfig show -n $primaryAppConfigName --query id --output tsv)
secondaryAppConfigId=$(az appconfig show -n $secondaryAppConfigName --query id --output tsv)

az role assignment create \
    --role "App Configuration Data Reader" \
    --assignee $functionPrincipalId \
    --scope $primaryAppConfigId

az role assignment create \
    --role "App Configuration Data Owner" \
    --assignee $functionPrincipalId \
    --scope $secondaryAppConfigId
```

Gewähren Sie mit dem folgenden Befehl oder über das [Azure-Portal](/azure/storage/common/storage-auth-aad-rbac-portal#assign-rbac-roles-using-the-azure-portal) der verwalteten Identität Ihrer Azure-Funktions-App Zugriff auf Ihre Speicherwarteschlange. 
- Weisen Sie die `Storage Queue Data Contributor`-Rolle in der Speicherwarteschlange zu.

```azurecli-interactive
az role assignment create \
    --role "Storage Queue Data Contributor" \
    --assignee $functionPrincipalId \
    --scope $queueId
```

## <a name="trigger-an-app-configuration-event"></a>Auslösen eines App Configuration-Ereignisses

Um zu testen, ob alles funktioniert, können Sie einen Schlüsselwert aus dem primären Speicher erstellen/aktualisieren/löschen. Diese Änderung sollte einige Sekunden nach dem Auslösen von Azure Functions durch den Timer automatisch im sekundären Speicher angezeigt werden.

```azurecli-interactive
az appconfig kv set --name $primaryAppConfigName --key Foo --value Bar --yes
```

Sie haben das Ereignis ausgelöst, und in wenigen Augenblicken wird Event Grid die Ereignisbenachrichtigung an Ihre Azure Storage-Warteschlange senden. Zeigen Sie ***nach der nächsten geplanten Ausführung Ihrer Azure-Funktion*** die Konfigurationseinstellungen im sekundären Speicher an, um festzustellen, ob er den aktualisierten Schlüsselwert aus dem primären Speicher enthält.

> [!NOTE]
> Sie können [Ihre Azure-Funktion während der Tests und Problembehandlung manuell auslösen](/azure/azure-functions/functions-manually-run-non-http), ohne auf den geplanten Timertrigger zu warten.

Nachdem Sie sichergestellt haben, dass die Sicherungsfunktion erfolgreich ausgeführt wird, können Sie sehen, dass der Schlüssel jetzt im sekundären Speicher vorhanden ist.

```azurecli-interactive
az appconfig kv show --name $secondaryAppConfigName --key Foo
```

```json
{
  "contentType": null,
  "etag": "eVgJugUUuopXnbCxg0dB63PDEJY",
  "key": "Foo",
  "label": null,
  "lastModified": "2020-04-27T23:25:08+00:00",
  "locked": false,
  "tags": {},
  "value": "Bar"
}
```

## <a name="troubleshooting"></a>Problembehandlung

Wenn die neue Einstellung nicht im sekundären Speicher angezeigt wird:

- Stellen Sie sicher, dass die Sicherungsfunktion ausgelöst wurde, ***nachdem*** Sie die Einstellung im primären Speicher erstellt haben.
- Es ist möglich, dass Event Grid die Ereignisbenachrichtigung nicht rechtzeitig an die Speicherwarteschlange senden konnte. Überprüfen Sie, ob die Speicherwarteschlange weiterhin die Ereignisbenachrichtigung aus dem primären Speicher enthält. Wenn dies der Fall ist, führen Sie die Sicherungsfunktion erneut aus.
- Überprüfen Sie die [Azure Functions-Protokolle](/azure/azure-functions/functions-create-scheduled-function#test-the-function) auf Fehler oder Warnungen.
- Stellen Sie im [Azure-Portal](/azure/azure-functions/functions-how-to-use-azure-function-app-settings#get-started-in-the-azure-portal) sicher, dass die Azure-Funktions-App korrekte Werte für die Anwendungseinstellungen enthält, die Ihre Azure-Funktion zu lesen versucht.
- Sie können auch die Überwachung und Warnungen für Ihre Azure-Funktion mit [Azure Application Insights](/azure/azure-functions/functions-monitoring?tabs=cmd) einrichten. 


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie diese App Configuration-Instanz und das Ereignisabonnement weiterverwenden möchten, überspringen Sie die Bereinigung der in diesem Artikel erstellten Ressourcen. Führen Sie andernfalls den folgenden Befehl aus, um die in diesem Artikel erstellten Ressourcen zu löschen.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte

Da Sie nun wissen, wie Sie die automatische Sicherung ihrer Schlüsselwerte einrichten, sollten Sie erfahren, wie Sie die Georesilienz Ihrer Anwendung erhöhen können:

- [Resilienz und Notfallwiederherstellung](concept-disaster-recovery.md)
