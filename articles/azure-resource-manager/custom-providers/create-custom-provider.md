---
title: Erstellen eines Ressourcenanbieters
description: Beschreibt das Erstellen eines Ressourcenanbieters und die Bereitstellung seiner benutzerdefinierten Ressourcentypen.
author: MSEvanhi
ms.topic: tutorial
ms.date: 06/19/2020
ms.author: evanhi
ms.openlocfilehash: ce547c010d3cc814d4e6f6182c19572248228fc3
ms.sourcegitcommit: 398fecceba133d90aa8f6f1f2af58899f613d1e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2020
ms.locfileid: "85125003"
---
# <a name="quickstart-create-custom-provider-and-deploy-custom-resources"></a>Schnellstart: Erstellen eines benutzerdefinierten Anbieters und Bereitstellen von benutzerdefinierten Ressourcen

In dieser Schnellstartanleitung erstellen Sie Ihren eigenen Ressourcenanbieter und stellen benutzerdefinierte Ressourcentypen für diesen Ressourcenanbieter bereit. Weitere Informationen zu benutzerdefinierten Anbietern finden Sie unter [Azure Custom Providers (Vorschau): Übersicht](overview.md).

## <a name="prerequisites"></a>Voraussetzungen

In dieser Schnellstartanleitung müssen `REST`-Vorgänge aufgerufen werden. Es gibt [verschiedene Möglichkeiten zum Senden von REST-Anforderungen](/rest/api/azure/).

Um Azure CLI-Befehle auszuführen, verwenden Sie [Bash in der Azure Cloud Shell](/azure/cloud-shell/quickstart). Die [custom-providers](/cli/azure/ext/custom-providers/custom-providers/resource-provider)-Befehle erfordern eine Erweiterung. Weitere Informationen finden Sie unter [Verwenden von Erweiterungen mit der Azure CLI](/cli/azure/azure-cli-extensions-overview).

Verwenden Sie PowerShell 7 oder höher und die Azure PowerShell-Module, um PowerShell-Befehle lokal auszuführen. Weitere Informationen finden Sie unter [Installieren von Azure PowerShell](/powershell/azure/install-az-ps). Wenn Sie noch nicht über ein Tool für `REST`-Vorgänge verfügen, installieren Sie [ARMClient](https://github.com/projectkudu/ARMClient). Es handelt sich um ein Open-Source-Befehlszeilentool, das den Aufruf der Azure Resource Manager-API vereinfacht.

## <a name="deploy-custom-provider"></a>Bereitstellen eines benutzerdefinierten Anbieters

Um den benutzerdefinierten Anbieter einzurichten, stellen Sie eine [Beispielvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/custom-providers/customprovider.json) für Ihr Azure-Abonnement bereit.

Nach dem Bereitstellen der Vorlage weist Ihr Abonnement die folgenden Ressourcen auf:

* Funktions-App mit den Vorgängen für die Ressourcen und Aktionen.
* Speicherkonto zum Speichern von Benutzern, die durch den benutzerdefinierten Anbieter erstellt werden.
* Benutzerdefinierten Anbieter, der die benutzerdefinierten Ressourcentypen und Aktionen definiert. Der Endpunkt der Funktions-App wird zum Senden von Anforderungen verwendet.
* Benutzerdefinierte Ressource vom benutzerdefinierten Anbieter.

Verwenden Sie zum Bereitstellen des benutzerdefinierten Anbieters Azure CLI oder PowerShell:

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
read -p "Enter a resource group name:" rgName &&
read -p "Enter the location (i.e. eastus):" location &&
read -p "Enter the provider's function app name:" funcName &&
templateUri="https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/custom-providers/customprovider.json" &&
az group create --name $rgName --location "$location" &&
az deployment group create --resource-group $rgName --template-uri $templateUri --parameters funcName=$funcName &&
echo "Press [ENTER] to continue ..." &&
read
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
$rgName = Read-Host -Prompt "Enter a resource group name"
$location = Read-Host -Prompt "Enter the location (i.e. eastus)"
$funcName = Read-Host -Prompt "Enter the provider's function app name"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/custom-providers/customprovider.json"
New-AzResourceGroup -Name $rgName -Location "$location"
New-AzResourceGroupDeployment -ResourceGroupName $rgName -TemplateUri $templateUri -funcName $funcName
Read-Host -Prompt "Press [ENTER] to continue ..."
```

---

Sie können die Lösung auch mithilfe der folgenden Schaltfläche aus dem Azure-Portal bereitstellen:

[![In Azure bereitstellen](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-docs-json-samples%2Fmaster%2Fcustom-providers%2Fcustomprovider.json)

## <a name="view-custom-provider-and-resource"></a>Anzeigen des benutzerdefinierten Anbieters und der Ressource

Im Portal ist der benutzerdefinierte Anbieter ein ausgeblendeten Ressourcentyp. Um zu bestätigen, dass der Ressourcenanbieter bereitgestellt wurde, navigieren Sie zur Ressourcengruppe. Wählen Sie die Option **Ausgeblendete Typen anzeigen** aus.

![Anzeigen von ausgeblendeten Ressourcentypen](./media/create-custom-provider/show-hidden.png)

Um den benutzerdefinierten Ressourcentyp anzuzeigen, den Sie bereitgestellt haben, verwenden Sie den `GET`-Vorgang für Ihren Ressourcentyp.

```http
GET https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users?api-version=2018-09-01-preview
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
subID=$(az account show --query id --output tsv)
requestURI="https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/users?api-version=2018-09-01-preview"
az rest --method get --uri $requestURI
```

Sie erhalten die folgende Antwort:

```json
{
  "value": [
    {
      "id": "/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/santa",
      "name": "santa",
      "properties": {
        "FullName": "Santa Claus",
        "Location": "NorthPole",
        "provisioningState": "Succeeded"
      },
      "resourceGroup": "<rg-name>",
      "type": "Microsoft.CustomProviders/resourceProviders/users"
    }
  ]
}
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
$subID = (Get-AzContext).Subscription.Id
$requestURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/users?api-version=2018-09-01-preview"

armclient GET $requestURI
```

Sie erhalten die folgende Antwort:

```json
{
  "value": [
    {
      "properties": {
        "provisioningState": "Succeeded",
        "FullName": "Santa Claus",
        "Location": "NorthPole"
      },
      "id": "/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/santa",
      "name": "santa",
      "type": "Microsoft.CustomProviders/resourceProviders/users"
    }
  ]
}
```

---

## <a name="call-action"></a>Aufrufaktion

Der benutzerdefinierte Anbieter verfügt auch über eine Aktion namens `ping`. Der Code, der die Anforderung verarbeitet, wird in der Funktions-App implementiert. Die `ping`-Aktion antwortet mit einer Begrüßung.

Um eine `ping`-Anforderung zu senden, verwenden Sie den `POST`-Vorgang für Ihren benutzerdefinierten Anbieter.

```http
POST https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/ping?api-version=2018-09-01-preview
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
pingURI="https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/ping?api-version=2018-09-01-preview"
az rest --method post --uri $pingURI
```

Sie erhalten die folgende Antwort:

```json
{
  "message": "hello <function-name>.azurewebsites.net",
  "pingcontent": {
    "source": "<function-name>.azurewebsites.net"
  }
}
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
$pingURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/ping?api-version=2018-09-01-preview"

armclient POST $pingURI
```

Sie erhalten die folgende Antwort:

```json
{
  "pingcontent": {
    "source": "<function-name>.azurewebsites.net"
  },
  "message": "hello <function-name>.azurewebsites.net"
}
```

---

## <a name="create-a-resource-type"></a>Erstellen eines Ressourcentyps

Um den benutzerdefinierten Ressourcentyp zu erstellen, können Sie die Ressource in einer Vorlage bereitstellen. Dieser Ansatz wird in der Vorlage gezeigt, die Sie in dieser Schnellstartanleitung bereitgestellt haben. Sie können auch eine `PUT`-Anforderung für den Ressourcentyp senden.

```http
PUT https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/<resource-name>?api-version=2018-09-01-preview

{"properties":{"FullName": "Test User", "Location": "Earth"}}
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
addURI="https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/users/testuser?api-version=2018-09-01-preview"
az rest --method put --uri $addURI --body "{'properties':{'FullName': 'Test User', 'Location': 'Earth'}}"
```

Sie erhalten die folgende Antwort:

```json
{
  "id": "/subscriptions/<sub-ID>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/testuser",
  "name": "testuser",
  "properties": {
    "FullName": "Test User",
    "Location": "Earth",
    "provisioningState": "Succeeded"
  },
  "resourceGroup": "<rg-name>",
  "type": "Microsoft.CustomProviders/resourceProviders/users"
}
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
$addURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/users/testuser?api-version=2018-09-01-preview"
$requestBody = "{'properties':{'FullName': 'Test User', 'Location': 'Earth'}}"

armclient PUT $addURI $requestBody
```

Sie erhalten die folgende Antwort:

```json
{
  "properties": {
    "provisioningState": "Succeeded",
    "FullName": "Test User",
    "Location": "Earth"
  },
  "id": "/subscriptions/<sub-ID>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/testuser",
  "name": "testuser",
  "type": "Microsoft.CustomProviders/resourceProviders/users"
}
```

---

## <a name="custom-resource-provider-commands"></a>Befehle für benutzerdefinierte Ressourcenanbieter

Verwenden Sie die [custom-providers](/cli/azure/ext/custom-providers/custom-providers/resource-provider)-Befehle, um mit Ihrem benutzerdefinierten Ressourcenanbieter zu arbeiten.

### <a name="list-custom-resource-providers"></a>Auflisten benutzerdefinierter Ressourcenanbieter

Listen Sie alle benutzerdefinierten Ressourcenanbieter in einem Abonnement auf. In der Standardeinstellung werden benutzerdefinierte Ressourcenanbieter für das aktuelle Abonnement aufgelistet, oder Sie können den `--subscription`-Parameter angeben. Um für eine Ressourcengruppe aufzulisten, verwenden Sie den Parameter `--resource-group`.

```azurecli-interactive
az custom-providers resource-provider list --subscription $subID
```

```json
[
  {
    "actions": [
      {
        "endpoint": "https://<provider-name>.azurewebsites.net/api/{requestPath}",
        "name": "ping",
        "routingType": "Proxy"
      }
    ],
    "id": "/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceproviders/<provider-name>",
    "location": "eastus",
    "name": "<provider-name>",
    "provisioningState": "Succeeded",
    "resourceGroup": "<rg-name>",
    "resourceTypes": [
      {
        "endpoint": "https://<provider-name>.azurewebsites.net/api/{requestPath}",
        "name": "users",
        "routingType": "Proxy, Cache"
      }
    ],
    "tags": {},
    "type": "Microsoft.CustomProviders/resourceproviders",
    "validations": null
  }
]
```

### <a name="show-the-properties"></a>Anzeigen der Eigenschaften

Zeigen Sie die Eigenschaften für einen benutzerdefinierten Ressourcenanbieter an. Das Ausgabeformat ähnelt der Ausgabe von `list`.

```azurecli-interactive
az custom-providers resource-provider show --resource-group $rgName --name $funcName
```

### <a name="create-a-new-resource"></a>Neue Ressource erstellen

Verwenden Sie den Befehl `create`, um einen benutzerdefinierten Ressourcenanbieter zu erstellen oder zu aktualisieren. In diesem Beispiel werden `actions` und `resourceTypes` aktualisiert.

```azurecli-interactive
az custom-providers resource-provider create --resource-group $rgName --name $funcName \
--action name=ping endpoint=https://myTestSite.azurewebsites.net/api/{requestPath} routing_type=Proxy \
--resource-type name=users endpoint=https://myTestSite.azurewebsites.net/api{requestPath} routing_type="Proxy, Cache"
```

```json
"actions": [
  {
    "endpoint": "https://myTestSite.azurewebsites.net/api/{requestPath}",
    "name": "ping",
    "routingType": "Proxy"
  }
],

"resourceTypes": [
  {
    "endpoint": "https://myTestSite.azurewebsites.net/api{requestPath}",
    "name": "users",
    "routingType": "Proxy, Cache"
  }
],
```

### <a name="update-the-providers-tags"></a>Aktualisieren der Tags des Anbieters

Der Befehl `update` aktualisiert nur Tags für einen benutzerdefinierten Ressourcenanbieter. Im Azure-Portal zeigt der App Service des benutzerdefinierten Ressourcenanbieters das Tag an.

```azurecli-interactive
az custom-providers resource-provider update --resource-group $rgName --name $funcName --tags new=tag
```

```json
"tags": {
  "new": "tag"
},
```

### <a name="delete-a-custom-resource-provider"></a>Löschen eines benutzerdefinierten Ressourcenanbieters

Bei Verwendung des Befehls `delete` werden Sie zur Bestätigung aufgefordert, und es wird nur der benutzerdefinierte Ressourcenanbieter gelöscht. Das Speicherkonto, der App Service und der App Service-Plan werden nicht gelöscht. Nachdem der Anbieter gelöscht wurde, wechseln Sie zu einer Eingabeaufforderung zurück.

```azurecli-interactive
az custom-providers resource-provider delete --resource-group $rgName --name $funcName
```

## <a name="next-steps"></a>Nächste Schritte

Eine Einführung in benutzerdefinierte Anbieter finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Vorschauversion zu benutzerdefinierten Azure-Anbietern – Übersicht](overview.md)
