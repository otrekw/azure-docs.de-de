---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 03/10/2021
ms.author: mikben
ms.openlocfilehash: 22a9cf3338f422341928a77f2bf14c497aa2ba31
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105563774"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/dotnet/).
- [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?tabs=azure-cli) 

## <a name="create-azure-communication-resource"></a>Erstellen von Azure Communication-Ressourcen

Melden Sie sich bei der [Azure-Befehlszeilenschnittstelle](/cli/azure/authenticate-azure-cli) an, um eine Azure Communication Services-Ressource zu erstellen. Verwenden Sie hierzu am Terminal den Befehl ```az login```, und geben Sie Ihre Anmeldeinformationen ein. Führen Sie den folgenden Befehl aus, um die Ressource zu erstellen:

```azurecli
az communication create --name "<communicationName>" --location "Global" --data-location "United States" --resource-group "<resourceGroup>"
```

Wenn Sie ein bestimmtes Abonnement auswählen möchten, können Sie auch das Flag ```--subscription``` und die Abonnement-ID angeben.
```
az communication create --name "<communicationName>" --location "Global" --data-location "United States" --resource-group "<resourceGroup> --subscription "<subscriptionID>"
```

Sie können Ihre Communication Services-Ressource mit den folgenden Optionen konfigurieren:

* Die Ressourcengruppe
* Den Namen der Communication Services-Ressource
* Die Geografie, der die Ressource zugeordnet wird

Im nächsten Schritt können Sie der Ressource Tags zuweisen. Tags können verwendet werden, um Azure-Ressourcen zu organisieren. Weitere Informationen zu Tags finden Sie in der [Dokumentation zum Anwenden von Tags auf Ressourcen](../../../azure-resource-manager/management/tag-resources.md).

## <a name="manage-your-communication-services-resource"></a>Verwalten Ihrer Communication Services-Ressource

Führen Sie die folgenden Befehle aus, um Tags zu Ihrer Communication Services-Ressource hinzuzufügen. Sie können auch ein bestimmtes Abonnement als Ziel haben.

```azurecli
az communication update --name "<communicationName>" --tags newTag="newVal1" --resource-group "<resourceGroup>"

az communication update --name "<communicationName>" --tags newTag="newVal2" --resource-group "<resourceGroup>" --subscription "<subscriptionID>"

az communication show --name "<communicationName>" --resource-group "<resourceGroup>"

az communication show --name "<communicationName>" --resource-group "<resourceGroup>" --subscription "<subscriptionID>"
```

Informationen zu weiteren Befehlen finden Sie unter [az communication](/cli/azure/ext/communication/communication).
