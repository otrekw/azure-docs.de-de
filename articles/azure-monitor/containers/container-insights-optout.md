---
title: Beenden der Überwachung Ihres Azure Kubernetes Service-Clusters | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Überwachung Ihres Azure AKS-Clusters mit Container Insights beenden können.
ms.topic: conceptual
ms.date: 08/19/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: eab34f27d1e33b166971203082cce99fb2b0e106
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101723536"
---
# <a name="how-to-stop-monitoring-your-azure-kubernetes-service-aks-with-container-insights"></a>Beenden der Überwachung Ihres Azure Kubernetes Service (AKS) mit Container Insights

Die Überwachung Ihres AKS-Clusters kann nach der Aktivierung wieder beendet werden, wenn Sie den Cluster nicht mehr überwachen möchten. In diesem Artikel erfahren Sie, wie Sie dies mithilfe der Azure CLI oder der bereitgestellten Azure Resource Manager-Vorlagen umsetzen.  


## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Verwenden Sie den Befehl [az aks disable-addons](/cli/azure/aks#az-aks-disable-addons), um Container Insights zu deaktivieren. Der Befehl entfernt den Agent von den Clusterknoten, er entfernt jedoch nicht die Lösung oder die bereits gesammelten und in Ihrer Azure Monitor-Ressource gespeicherten Daten.  

```azurecli
az aks disable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG
```

Informationen zum erneuten Aktivieren der Überwachung für Ihren Cluster finden Sie unter [Aktivieren der Überwachung mithilfe der Azure-Befehlszeilenschnittstelle](container-insights-enable-new-cluster.md#enable-using-azure-cli).

## <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage

Es stehen zwei Vorlagen für Azure Resource Manager zur Verfügung, die das konsistente und wiederholte Entfernen der Lösungsressourcen aus Ihrer Ressourcengruppe unterstützen. Eine ist eine JSON-Vorlage, die die Konfiguration zum Beenden der Überwachung angibt, während die andere die von Ihnen zu konfigurierenden Parameterwerte enthält, mit denen Sie die Ressourcen-ID des AKS-Clusters und die Ressourcengruppe festlegen, in der der Cluster bereitgestellt ist.

Wenn Sie mit der Bereitstellung von Ressourcen mithilfe einer Vorlage nicht vertraut sind, finden Sie weitere Informationen unter:
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../../azure-resource-manager/templates/deploy-powershell.md)
* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure CLI](../../azure-resource-manager/templates/deploy-cli.md)

>[!NOTE]
>Die Vorlage muss in derselben Ressourcengruppe des Clusters bereitgestellt werden. Wenn Sie bei Verwendung dieser Vorlage andere Eigenschaften oder Add-Ons weglassen, werden diese ggf. aus dem Cluster entfernt. Beispiel: *enableRBAC* für Kubernetes RBAC-Richtlinien, die in Ihrem Cluster implementiert werden, oder *aksResourceTagValues*, wenn Tags für den AKS-Cluster angegeben werden.  
>

Wenn Sie die Azure CLI verwenden möchten, müssen Sie sie zuerst installieren und lokal verwenden. Sie benötigen Azure CLI 2.0.27 oder höher. Um Ihre Version zu ermitteln, führen Sie `az --version` aus. Informationen zur Installation und zum Upgrade von Azure CLI finden Sie unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

### <a name="create-template"></a>Erstellen der Vorlage

1. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "aksResourceId": {
           "type": "string",
           "metadata": {
             "description": "AKS Cluster Resource ID"
           }
       },
      "aksResourceLocation": {
        "type": "string",
        "metadata": {
           "description": "Location of the AKS resource e.g. \"East US\""
         }
       },
    "aksResourceTagValues": {
      "type": "object",
      "metadata": {
        "description": "Existing all tags on AKS Cluster Resource"
        }
      }
     },
    "resources": [
      {
        "name": "[split(parameters('aksResourceId'),'/')[8]]",
        "type": "Microsoft.ContainerService/managedClusters",
        "location": "[parameters('aksResourceLocation')]",
        "tags": "[parameters('aksResourceTagValues')]",
        "apiVersion": "2018-03-31",
        "properties": {
          "mode": "Incremental",
          "id": "[parameters('aksResourceId')]",
          "addonProfiles": {
            "omsagent": {
              "enabled": false,
              "config": null
            }
           }
         }
       }
      ]
    }
    ```

2. Speichern Sie diese Datei als **OptOutTemplate.json** in einem lokalen Ordner.

3. Fügen Sie die folgende JSON-Syntax in Ihre Datei ein:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "aksResourceId": {
          "value": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
        },
        "aksResourceLocation": {
          "value": "<aksClusterRegion>"
        },
        "aksResourceTagValues": {
          "value": {
            "<existing-tag-name1>": "<existing-tag-value1>",
            "<existing-tag-name2>": "<existing-tag-value2>",
            "<existing-tag-nameN>": "<existing-tag-valueN>"
          }
        }
      }
    }
    ```

4. Aktualisieren Sie den Wert für **aksResourceId** und **aksResourceLocation** mit den Werten des AKS-Clusters, die auf der Seite **Eigenschaften** für den ausgewählten Cluster angegeben werden.

    ![Seite mit den Containereigenschaften](media/container-insights-optout/container-properties-page.png)

    Kopieren Sie auf der Seite **Eigenschaften** außerdem die **Arbeitsbereichsressourcen-ID**. Sie benötigen diesen Wert, falls Sie den Log Analytics-Arbeitsbereich später löschen möchten. Das Löschen des Log Analytics-Arbeitsbereichs wird nicht im Rahmen dieses Prozesses ausgeführt.

    Bearbeiten Sie die Werte für **aksResourceTagValues** so, dass sie den für den AKS-Cluster angegebenen vorhandenen Tagwerten entsprechen.

5. Speichern Sie diese Datei als **OptOutParam.json** in einem lokalen Ordner.

6. Nun können Sie die Vorlage bereitstellen.

### <a name="remove-the-solution-using-azure-cli"></a>Entfernen der Lösung mithilfe der Azure-Befehlszeilenschnittstelle

Führen Sie den folgenden Befehl der Azure CLI unter Linux aus, um die Lösung zu entfernen und die Konfiguration auf Ihrem AKS-Cluster zu bereinigen.

```azurecli
az login   
az account set --subscription "Subscription Name"
az deployment group create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
```

Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das zurückgegebene Ergebnis anzeigt:

```output
ProvisioningState       : Succeeded
```

### <a name="remove-the-solution-using-powershell"></a>Entfernen der Lösung mithilfe von PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Führen Sie die folgenden PowerShell-Befehle in dem Ordner aus, der die Vorlage enthält, um die Lösung zu entfernen und die Konfiguration auf Ihrem AKS-Cluster zu bereinigen.    

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName <yourSubscriptionName>
New-AzResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
```

Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das zurückgegebene Ergebnis anzeigt:

```output
ProvisioningState       : Succeeded
```


## <a name="next-steps"></a>Nächste Schritte

Wenn der Arbeitsbereich nur als Unterstützung für die Clusterüberwachung erstellt wurde und nicht mehr benötigt wird, müssen Sie ihn manuell löschen. Wie ein Arbeitsbereich gelöscht wird, erfahren Sie unter [Löschen eines Log Analytics-Arbeitsbereichs mit dem Azure-Portal](../logs/delete-workspace.md). Denken Sie an die **Arbeitsbereichsressourcen-ID**, die Sie weiter oben in Schritt 4 kopiert haben, da Sie sie später benötigen.