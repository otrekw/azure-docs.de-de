---
title: Bereitstellen von Ressourcen mit REST-API und Vorlagen
description: Verwenden Sie Azure Resource Manager und Resource Manager-REST-API, um Ressourcen in Azure bereitzustellen. Die Ressourcen werden in einer Resource Manager-Vorlage definiert.
ms.topic: conceptual
ms.date: 06/04/2019
ms.openlocfilehash: 9cdb7b668e5170917b41ef49639bd9a17e538766
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80153232"
---
# <a name="deploy-resources-with-arm-templates-and-resource-manager-rest-api"></a>Bereitstellen von Ressourcen mit ARM-Vorlagen und Resource Manager-REST-API

In diesem Artikel wird erläutert, wie Ihre Ressourcen mithilfe der Resource Manager-REST-API und von Azure Resource Manager-Vorlagen (ARM) in Azure bereitgestellt werden.

Sie können entweder Ihre Vorlage in den Anforderungstext einschließen oder eine Verknüpfung zu einer Datei erstellen. Bei einer Datei kann es sich um eine lokale Datei oder eine externe Datei handeln, die über einen URI verfügbar ist. Wenn sich Ihre Vorlage in einem Speicherkonto befindet, können Sie den Zugriff auf die Vorlage beschränken und während der Bereitstellung ein SAS-Token (Shared Access Signature) angeben.

## <a name="deployment-scope"></a>Bereitstellungsumfang

Sie können als Ziel für Ihre Bereitstellung eine Verwaltungsgruppe, ein Azure-Abonnement oder eine Ressourcengruppe auswählen. In den meisten Fällen wählen Sie eine Ressourcengruppe als Ziel für Bereitstellungen aus. Verwenden Sie Verwaltungsgruppen- oder Abonnementbereitstellungen, um Richtlinien und Rollenzuweisungen im angegebenen Bereich anzuwenden. Sie verwenden Abonnementbereitstellungen auch, um eine Ressourcengruppe zu erstellen und Ressourcen für sie bereitzustellen. Abhängig vom Umfang der Bereitstellung verwenden Sie unterschiedliche Befehle.

Für die Bereitstellung in einer **Ressourcengruppe** verwenden Sie [Bereitstellungen – Erstellen](/rest/api/resources/deployments/createorupdate). Die Anforderung wird gesendet an:

```HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

Für die Bereitstellung in einem **Abonnement** verwenden Sie [Bereitstellungen – Erstellen im Abonnementbereich](/rest/api/resources/deployments/createorupdateatsubscriptionscope). Die Anforderung wird gesendet an:

```HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

Weitere Informationen zu Bereitstellungen auf Abonnementebene finden Sie unter [Erstellen von Ressourcengruppen und Ressourcen auf Abonnementebene](deploy-to-subscription.md).

Für die Bereitstellung in einer **Verwaltungsgruppe** verwenden Sie [Bereitstellungen – Erstellen im Verwaltungsgruppenbereich](/rest/api/resources/deployments/createorupdateatmanagementgroupscope). Die Anforderung wird gesendet an:

```HTTP
PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{groupId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

Weitere Informationen zu Bereitstellungen auf Verwaltungsgruppenebene finden Sie unter [Erstellen von Ressourcen auf der Verwaltungsgruppenebene](deploy-to-management-group.md).

Die Beispiele in diesem Artikel verwenden Ressourcengruppenbereitstellungen.

## <a name="deploy-with-the-rest-api"></a>Bereitstellen mit der REST-API

1. Legen Sie [allgemeine Parameter und Header](/rest/api/azure/) fest, einschließlich Authentifizierungstoken.

1. Erstellen Sie eine Ressourcengruppe, wenn noch keine vorhanden ist. Geben Sie Ihre Abonnement-ID, den Namen der neuen Ressourcengruppe und den Speicherort für Ihre Lösung an. Weitere Informationen finden Sie unter [Erstellen einer Ressourcengruppe](/rest/api/resources/resourcegroups/createorupdate).

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2019-05-01
   ```

   Mit dem Anforderungstext ähnlich dem folgenden:

   ```json
   {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
   }
   ```

1. Überprüfen Sie die Bereitstellung vor der Implementierung, indem Sie den Vorgang zum [Überprüfen einer Vorlagenbereitstellung](/rest/api/resources/deployments/validate) ausführen. Geben Sie die Parameter beim Testen der Bereitstellung genauso an wie beim Ausführen der Bereitstellung (wie im nächsten Schritt zu sehen).

1. Geben Sie zum Bereitstellen einer Vorlage Ihre Abonnement-ID, den Namen der Ressourcengruppe und den Namen der Bereitstellung im Anforderungs-URI an.

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2019-05-01
   ```

   Geben Sie im Anforderungstext einen Link zu Ihrer Vorlage und Parameterdatei an. Weitere Informationen zur Parameterdatei finden Sie unter [Erstellen einer Resource Manager-Parameterdatei](parameter-files.md).

   Beachten Sie, dass **mode** auf **Incremental** festgelegt ist. Legen Sie zum Ausführen einer vollständigen Bereitstellung **mode** auf **Complete** fest. Gehen Sie bei Verwendung des Modus „Complete“ sehr umsichtig vor, da Sie versehentlich Ressourcen löschen können, die nicht in Ihrer Vorlage enthalten sind.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental"
    }
   }
   ```

    Wenn Sie den Antwortinhalt und/oder den Anforderungsinhalt protokollieren möchten, fügen Sie **debugSetting** in die Anforderung ein.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "debugSetting": {
        "detailLevel": "requestContent, responseContent"
      }
    }
   }
   ```

    Sie können das Speicherkonto so einrichten, das ein SAS-Token (Shared Access Signature) verwendet wird. Weitere Informationen finden Sie unter [Delegieren des Zugriffs mit einer SAS (Shared Access Signature)](/rest/api/storageservices/delegating-access-with-a-shared-access-signature).

    Wenn Sie einen vertraulichen Wert für einen Parameter (z.B. ein Kennwort) angeben müssen, fügen Sie den Wert einem Schlüsseltresor hinzu. Rufen Sie den Schlüsseltresor während der Bereitstellung wie im vorherigen Beispiel gezeigt ab. Weitere Informationen finden Sie unter [Übergeben sicherer Werte während der Bereitstellung](key-vault-parameter.md).

1. Anstatt für die Vorlage und die Parameter eine Verknüpfung mit Dateien zu erstellen, können Sie diese auch in den Anforderungstext einschließen. Im folgenden Beispiel ist der Anforderungstext mit der inline angegebenen Vorlage und den Parametern dargestellt:

   ```json
   {
      "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
              "Standard_LRS",
              "Standard_GRS",
              "Standard_ZRS",
              "Premium_LRS"
            ],
            "metadata": {
              "description": "Storage Account type"
            }
          },
          "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
              "description": "Location for all resources."
            }
          }
        },
        "variables": {
          "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2018-02-01",
            "name": "[variables('storageAccountName')]",
            "location": "[parameters('location')]",
            "sku": {
              "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
          }
        ],
        "outputs": {
          "storageAccountName": {
            "type": "string",
            "value": "[variables('storageAccountName')]"
          }
        }
      },
      "parameters": {
        "location": {
          "value": "eastus2"
        }
      }
    }
   }
   ```

1. Rufen Sie den Status der Vorlagenbereitstellung mit [Deployments - Get](/rest/api/resources/deployments/get) ab.

   ```HTTP
   GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
   ```

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Rollback zu einer erfolgreiche Bereitstellung, wenn ein Fehler auftritt, finden Sie unter [Rollback bei Fehler zu erfolgreicher Bereitstellung](rollback-on-error.md).
- Wenn Sie angeben möchten, wie Ressourcen behandelt werden sollen, die in der Ressourcengruppe enthalten sind, aber nicht in der Vorlage definiert wurden, lesen Sie die Informationen unter [Azure Resource Manager-Bereitstellungsmodi](deployment-modes.md).
- Informationen zum Arbeiten mit asynchronen REST-Vorgängen finden Sie unter [Nachverfolgen asynchroner Vorgänge in Azure](../management/async-operations.md).
- Weitere Informationen zu Vorlagen finden Sie unter [Verstehen der Struktur und Syntax von ARM-Vorlagen](template-syntax.md).

