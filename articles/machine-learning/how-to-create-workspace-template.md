---
title: Erstellen eines Arbeitsbereichs anhand einer Azure Resource Manager-Vorlage
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage verwenden, um einen neuen Azure Machine Learning-Arbeitsbereich zu erstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 05/19/2020
ms.custom: seoapril2019
ms.openlocfilehash: eae10b7ae8cd14fd120e969c39c05a8ba2525003
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83681544"
---
# <a name="use-an-azure-resource-manager-template-to-create-a-workspace-for-azure-machine-learning"></a>Verwenden einer Azure Resource Manager-Vorlage zum Erstellen eines Arbeitsbereichs für Azure Machine Learning

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]
<br>

In diesem Artikel erlernen Sie verschiedene Möglichkeiten zum Erstellen eines Azure Machine Learning-Arbeitsbereichs mithilfe von Azure Resource Manager-Vorlagen. Eine Resource Manager-Vorlage erleichtert das Erstellen von Ressourcen in einem einzelnen, koordinierten Vorgang. Eine Vorlage ist ein JSON-Dokument, das die Ressourcen definiert, die für eine Bereitstellung erforderlich sind. Es kann außerdem bestimmte Bereitstellungsparameter angeben. Parameter werden verwendet, um Eingabewerte bereitzustellen, wenn die Vorlage verwendet wird.

Weitere Informationen finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Resource Manager-Vorlage](../azure-resource-manager/templates/deploy-powershell.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein **Azure-Abonnement**. Wenn Sie keins besitzen, probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) aus.

* Um eine Vorlage über eine Befehlszeilenschnittstelle zu verwenden, benötigen Sie entweder [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.2.0) oder die [Azure-Befehlszeilenschnittstelle (CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="resource-manager-template"></a>Resource Manager-Vorlage

Die folgende Resource Manager-Vorlage kann verwendet werden, um einen Azure Machine Learning-Arbeitsbereich mit zugeordneten Azure-Ressourcen zu erstellen:

[!code-json[create-azure-machine-learning-service-workspace](~/quickstart-templates/101-machine-learning-create/azuredeploy.json)]

Diese Vorlage erstellt die folgenden Azure-Dienste:

* Azure-Ressourcengruppe
* Azure Storage-Konto
* Azure-Schlüsseltresor
* Azure Application Insights
* Azure Container Registry
* Azure Machine Learning-Arbeitsbereich

Die Ressourcengruppe ist der Container, der die Dienste enthält. Die verschiedenen Dienste sind für den Azure Machine Learning-Arbeitsbereich erforderlich.

Die Beispielvorlage verfügt über zwei Parameter:

* Den **Ort**, an dem die Ressourcengruppe und die Dienste erstellt werden.

    Die Vorlage verwendet den von Ihnen ausgewählten Ort für die meisten Ressourcen. Die Ausnahme ist hierbei der Application Insights-Dienst, der nicht an allen Orten verfügbar ist, an denen die anderen Dienste verfügbar sind. Wenn Sie einen Ort auswählen, an dem er nicht verfügbar ist, wird der Dienst am Ort „USA, Süden-Mitte“ erstellt.

* Der **Arbeitsbereichsname**, wobei es sich um den Anzeigenamen des Azure Machine Learning-Arbeitsbereichs handelt.

    > [!NOTE]
    > Für den Namen des Arbeitsbereichs wird die Groß-/Kleinschreibung nicht beachtet.

    Die Namen der anderen Dienste werden nach dem Zufallsprinzip generiert.

> [!TIP]
> Während die Vorlage, die mit diesem Dokument verknüpft ist, eine neue Azure-Containerregistrierung erstellt, können Sie auch einen neuen Arbeitsbereich erstellen, ohne eine Containerregistrierung zu erstellen. Eine Containerregistrierung wird erstellt, wenn Sie einen Vorgang ausführen, der eine Containerregistrierung erfordert. Beispielsweise Trainieren oder Bereitstellen eines Modells.
>
> Sie können auch auf eine vorhandene Containerregistrierung oder ein Speicherkonto in der Azure Resource Manager-Vorlage verweisen, anstatt eine neue Registrierung zu erstellen.

[!INCLUDE [machine-learning-delete-acr](../../includes/machine-learning-delete-acr.md)]

Weitere Informationen zu Vorlagen finden Sie in den folgenden Artikeln:

* [Erstellen von Azure Resource-Manager-Vorlagen](../azure-resource-manager/templates/template-syntax.md)
* [Bereitstellen einer Anwendung mit Azure Resource Manager-Vorlagen](../azure-resource-manager/templates/deploy-powershell.md)
* [Microsoft.MachineLearningServices-Ressourcentypen](https://docs.microsoft.com/azure/templates/microsoft.machinelearningservices/allversions)

### <a name="advanced-template"></a>Erweiterte Vorlage

Die folgende Beispielvorlage zeigt, wie Sie einen Arbeitsbereich mit drei Einstellungen erstellen:

* Aktivierung einer hohen Vertraulichkeit für den Arbeitsbereich
* Aktivierung der Verschlüsselung für den Arbeitsbereich
* Verwendung einer vorhandenen Azure Key Vault-Instanz zum Abrufen von kundenseitig verwalteten Schlüsseln

Weitere Informationen finden Sie unter [Verschlüsselung ruhender Daten](concept-enterprise-security.md#encryption-at-rest).

> [!IMPORTANT]
> Es gibt einige bestimmte Anforderungen, die Ihr Abonnement erfüllen muss, bevor Sie diese Vorlage verwenden können:
> * Die __Azure Machine Learning__-Anwendung muss ein __Mitwirkender__ für Ihr Azure-Abonnement sein.
> * Sie müssen über einen vorhandenen Azure Key Vault verfügen, der einen Verschlüsselungsschlüssel enthält.
> * Sie müssen über eine Zugriffsrichtlinie im Azure Key Vault verfügen, die __get__-, __wrap__- und __unwrap__-Zugriff auf die __Azure Cosmos DB__-Anwendung gewährt.
> * Der Azure Key Vault muss sich in derselben Region befinden, in der Sie den Azure Machine Learning-Arbeitsbereich erstellen möchten.

Verwenden Sie die folgenden Befehle, __um die Azure Machine Learning-Anwendung als Mitwirkender hinzuzufügen__:

1. Verwenden Sie den folgenden Befehl, um sich über die Befehlszeilenschnittstelle bei Azure zu authentifizieren:

    ```azurecli-interactive
    az login
    ```
    
    [!INCLUDE [subscription-login](../../includes/machine-learning-cli-subscription.md)]

1. Verwenden Sie den folgenden Befehl, um die Objekt-ID der Azure Machine Learning-App abzurufen. Der Wert kann für jedes Ihrer Azure-Abonnements unterschiedlich sein:

    ```azurecli-interactive
    az ad sp list --display-name "Azure Machine Learning" --query '[].[appDisplayName,objectId]' --output tsv
    ```

    Dieser Befehl gibt die Objekt-ID zurück, bei der es sich um eine GUID handelt.

1. Verwenden Sie den folgenden Befehl, um die Objekt-ID als Mitwirkender zu Ihrem Abonnement hinzuzufügen. Ersetzen Sie `<object-ID>` durch die GUID aus dem vorherigen Schritt. Ersetzen Sie `<subscription-ID>` durch den Namen oder die ID Ihres Azure-Abonnements:

    ```azurecli-interactive
    az role assignment create --role 'Contributor' --assignee-object-id <object-ID> --subscription <subscription-ID>
    ```

__Zum Hinzufügen eines Schlüssels zu Ihrem Azure Key Vault__ verwenden Sie die Informationen im Abschnitt [Hinzufügen eines Schlüssels, Geheimnisses oder Zertifikats zum Schlüsseltresor](../key-vault/general/manage-with-cli2.md#adding-a-key-secret-or-certificate-to-the-key-vault) des Artikels __Verwalten von Key Vault mit der Azure-Befehlszeilenschnittstelle__.

__Verwenden Sie die folgenden Befehle, um dem Schlüsseltresor eine Zugriffsrichtlinie hinzuzufügen__:

1. Verwenden Sie den folgenden Befehl, um die Objekt-ID der Azure Cosmos DB-App abzurufen. Der Wert kann für jedes Ihrer Azure-Abonnements unterschiedlich sein:

    ```azurecli-interactive
    az ad sp list --display-name "Azure Cosmos DB" --query '[].[appDisplayName,objectId]' --output tsv
    ```
    
    Dieser Befehl gibt die Objekt-ID zurück, bei der es sich um eine GUID handelt.

1. Um eine Richtlinie festzulegen, verwenden Sie den folgenden Befehl. Ersetzen Sie `<keyvault-name>` durch den Namen des vorhandenen Azure Key Vault. Ersetzen Sie `<object-ID>` durch die GUID aus dem vorherigen Schritt:

    ```azurecli-interactive
    az keyvault set-policy --name <keyvault-name> --object-id <object-ID> --key-permissions get unwrapKey wrapKey
    ```

__Zum Abrufen der Werte__ für die Parameter `cmk_keyvault` (Key Vault-ID) und `resource_cmk_uri` (Schlüssel-URI), die für diese Vorlage erforderlich sind, verwenden Sie die folgenden Schritte:

1. Verwenden Sie den folgenden Befehl, um die Key Vault-ID abzurufen:

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query "id"
    ```

    Der Befehl gibt einen Wert zurück, der `/subscriptions/{subscription-guid}/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault` ähnelt.

1. Um den Wert für den URI für den kundenseitig verwalteten Schlüssel abzurufen, verwenden Sie folgenden Befehl:

    ```azurecli-interactive
    az keyvault key show --vault-name mykeyvault --name mykey --query "key.kid"
    ```

    Der Befehl gibt einen Wert zurück, der `https://mykeyvault.vault.azure.net/keys/mykey/{guid}` ähnelt.

__Beispielvorlage__

:::code language="json" source="~/quickstart-templates/201-machine-learning-encrypted-workspace/azuredeploy.json":::

> [!IMPORTANT]
> Nachdem ein Arbeitsbereich erstellt wurde, können Sie die Einstellungen für vertrauliche Daten, Verschlüsselung, Key Vault-ID oder Schlüsselbezeichner nicht mehr ändern. Um diese Werte zu ändern, müssen Sie einen neuen Arbeitsbereich erstellen und dabei neue Werte verwenden.

## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

1. Befolgen Sie die Schritte in [Bereitstellen von Ressourcen mithilfe einer benutzerdefinierten Vorlage](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal#deploy-resources-from-custom-template). Wenn Sie zum Bildschirm __Vorlage bearbeiten__ gelangen, fügen Sie die Vorlage aus diesem Dokument ein.
1. Wählen Sie __Speichern__ aus, um die Vorlage zu verwenden. Geben Sie die folgenden Informationen an, und stimmen Sie den aufgeführten Bestimmungen und Bedingungen zu:

   * Abonnement: Wählen Sie aus, welches Azure-Abonnement für diese Ressourcen verwendet werden soll.
   * Ressourcengruppe: Wählen Sie eine Ressourcengruppe für die Aufnahme der Dienste aus, oder erstellen Sie eine.
   * Arbeitsbereichsname: Der für den Azure Machine Learning-Arbeitsbereich, der erstellt wird, zu verwendende Name. Der Arbeitsbereichsname muss zwischen 3 und 33 Zeichen umfassen. Er darf nur alphanumerische Zeichen und Bindestriche („-“) enthalten.
   * Standort: Wählen Sie den Ort aus, an dem die Ressourcen erstellt werden.

Weitere Informationen finden Sie unter [Bereitstellen von Ressourcen mithilfe einer benutzerdefinierten Vorlage](../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template).

## <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell

In diesem Beispiel wird vorausgesetzt, dass Sie die Vorlagen in einer Datei namens `azuredeploy.json` im aktuellen Verzeichnis gespeichert haben:

```powershell
New-AzResourceGroup -Name examplegroup -Location "East US"
new-azresourcegroupdeployment -name exampledeployment `
  -resourcegroupname examplegroup -location "East US" `
  -templatefile .\azuredeploy.json -workspaceName "exampleworkspace" -sku "basic"
```

Weitere Informationen finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md) und [Bereitstellen privater Resource Manager-Vorlagen mit SAS-Token und Azure PowerShell](../azure-resource-manager/templates/secure-template-with-sas-token.md).

## <a name="use-the-azure-cli"></a>Verwenden der Azure-CLI

In diesem Beispiel wird vorausgesetzt, dass Sie die Vorlagen in einer Datei namens `azuredeploy.json` im aktuellen Verzeichnis gespeichert haben:

```azurecli-interactive
az group create --name examplegroup --location "East US"
az group deployment create \
  --name exampledeployment \
  --resource-group examplegroup \
  --template-file azuredeploy.json \
  --parameters workspaceName=exampleworkspace location=eastus sku=basic
```

Weitere Informationen finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und der Azure-Befehlszeilenschnittstelle](../azure-resource-manager/templates/deploy-cli.md) und [Bereitstellen privater Resource Manager-Vorlagen mit SAS-Token und der Azure-Befehlszeilenschnittstelle](../azure-resource-manager/templates/secure-template-with-sas-token.md).

## <a name="troubleshooting"></a>Problembehandlung

### <a name="resource-provider-errors"></a>Fehler der Ressourcenanbieter

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Azure Key Vault-Zugriffsrichtlinie und Azure Resource Manager-Vorlagen

Wenn Sie eine Azure Resource Manager-Vorlage mehrmals zum Erstellen des Arbeitsbereichs und der zugehörigen Ressourcen (einschließlich Azure Key Vault) verwenden. Dies ist beispielsweise der Fall, wenn Sie die Vorlage mehrere Male mit den gleichen Parametern in einer Continuous Integration- und Deployment-Pipeline verwenden.

Die meisten Vorgänge zum Erstellen von Ressourcen mit Vorlagen sind idempotent, Key Vault löscht die Zugriffsrichtlinien jedoch jedes Mal, wenn die Vorlage verwendet wird. Das Löschen der Zugriffsrichtlinien unterbricht den Zugriff auf den Schlüsseltresor für vorhandene Arbeitsbereiche, die ihn verwenden. Es kann beispielsweise sein, dass für die Beenden/Erstellen-Funktionalität der Azure Notebooks-VM ein Fehler auftritt.  

Folgende Ansätze werden empfohlen, um dieses Problem zu umgehen:

* Stellen Sie die Vorlage nicht mehrmals mit den gleichen Parametern bereit, oder löschen Sie die vorhandenen Ressourcen, bevor Sie die Vorlage verwenden, um sie neu zu erstellen.

* Untersuchen Sie die Key Vault-Zugriffsrichtlinien, und verwenden Sie sie dann zum Festlegen der `accessPolicies`-Eigenschaft der Vorlage. Zum Anzeigen der Zugriffsrichtlinien verwenden Sie den folgenden Azure CLI-Befehl:

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query properties.accessPolicies
    ```

    Weitere Informationen zur Verwendung des `accessPolicies`-Abschnitts der Vorlage finden Sie im [AccessPolicyEntry-Objektverweis](https://docs.microsoft.com/azure/templates/Microsoft.KeyVault/2018-02-14/vaults#AccessPolicyEntry).

* Überprüfen Sie, ob die Key Vault-Ressource bereits vorhanden ist. Wenn dies der Fall ist, erstellen Sie sie nicht mithilfe der Vorlage neu. Wenn Sie z. B. den vorhandenen Key Vault verwenden möchten, anstatt einen neuen zu erstellen, nehmen Sie die folgenden Änderungen an der Vorlage vor:

    * **Fügen Sie einen Parameter hinzu**, der die ID einer vorhandenen Key Vault-Ressource annimmt:

        ```json
        "keyVaultId":{
          "type": "string",
          "metadata": {
            "description": "Specify the existing Key Vault ID."
          }
        }
      ```

    * **Entfernen Sie** den Abschnitt, der eine Key Vault-Ressource erstellt:

        ```json
        {
          "type": "Microsoft.KeyVault/vaults",
          "apiVersion": "2018-02-14",
          "name": "[variables('keyVaultName')]",
          "location": "[parameters('location')]",
          "properties": {
            "tenantId": "[variables('tenantId')]",
            "sku": {
              "name": "standard",
              "family": "A"
            },
            "accessPolicies": [
            ]
          }
        },
        ```

    * **Entfernen Sie** die Zeile `"[resourceId('Microsoft.KeyVault/vaults', variables('keyVaultName'))]",` aus dem Abschnitt `dependsOn` des Arbeitsbereichs. **Ändern** Sie zudem ein Eintrag `keyVault` im Abschnitt `properties` des Arbeitsbereichs, um auf den Parameter `keyVaultId` zu verweisen:

        ```json
        {
          "type": "Microsoft.MachineLearningServices/workspaces",
          "apiVersion": "2019-11-01",
          "name": "[parameters('workspaceName')]",
          "location": "[parameters('location')]",
          "dependsOn": [
            "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
            "[resourceId('Microsoft.Insights/components', variables('applicationInsightsName'))]"
          ],
          "identity": {
            "type": "systemAssigned"
          },
          "sku": {
            "tier": "[parameters('sku')]",
            "name": "[parameters('sku')]"
          },
          "properties": {
            "friendlyName": "[parameters('workspaceName')]",
            "keyVault": "[parameters('keyVaultId')]",
            "applicationInsights": "[resourceId('Microsoft.Insights/components',variables('applicationInsightsName'))]",
            "storageAccount": "[resourceId('Microsoft.Storage/storageAccounts/',variables('storageAccountName'))]"
          }
        }
        ```

    Nachdem Sie diese Änderungen vorgenommen haben, können Sie beim Ausführen der Vorlage die ID der vorhandenen Key Vault-Ressource angeben. Die Vorlage verwendet den Schlüsseltresor anschließend erneut, indem die `keyVault`-Eigenschaft des Arbeitsbereichs auf die entsprechende ID festlegt wird.

    Um die ID des Key Vaults abzurufen, können Sie auf die Ausgabe der ursprünglichen Vorlagenausführung verweisen oder die Azure CLI verwenden. Der folgende Befehl ist ein Beispiel für die Verwendung der Azure CLI zum Abrufen der ID der Key Vault Ressource:

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query id
    ```

    Dieser Befehl gibt einen Wert zurück, der in etwa wie folgt aussieht:

    ```text
    /subscriptions/{subscription-guid}/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault
    ```

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und Resource Manager-REST-API](../azure-resource-manager/templates/deploy-rest.md).
* [Erstellen und Bereitstellen von Azure-Ressourcengruppen mit Visual Studio](../azure-resource-manager/templates/create-visual-studio-deployment-project.md).
