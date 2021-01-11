---
title: Zonenredundante Registrierung für Hochverfügbarkeit
description: Erfahren Sie mehr über das Aktivieren der Zonenredundanz in Azure Container Registry durch Erstellen einer Containerregistrierung oder Replikation in einer Azure-Verfügbarkeitszone. Zonenredundanz ist ein Feature der Dienstebene „Premium“.
ms.topic: article
ms.date: 12/11/2020
ms.openlocfilehash: 1553beef47a3d493f066e47cd39751093d83fc24
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/29/2020
ms.locfileid: "97803509"
---
# <a name="enable-zone-redundancy-in-azure-container-registry-for-resiliency-and-high-availability"></a>Aktivieren von Zonenredundanz in Azure Container Registry für Resilienz und Hochverfügbarkeit

Zusätzlich zur [Georeplikation](container-registry-geo-replication.md), bei der Registrierungsdaten in einer oder zwischen mehreren Azure-Regionen repliziert werden, um die Verfügbarkeit zu gewährleisten und Latenzzeiten für regionale Vorgänge zu verringern, unterstützt Azure Container Registry optionale *Zonenredundanz*. [Zonenredundanz](../availability-zones/az-overview.md#availability-zones) bietet Resilienz und Hochverfügbarkeit für eine Registrierung oder Replikationsressource (Replikat) in einer bestimmten Region.

In diesem Artikel wird gezeigt, wie Sie die eine zonenredundante Containerregistrierung oder ein zonenredundantes Replikat mithilfe des Azure-Portals oder einer Azure Resource Manager-Vorlage einrichten. 

Zonenredundanz ist eine **Preview** funktion der Containerregistrierungs-Dienstebene „Premium“. Informationen zu den Tarifen und Einschränkungen des Registrierungsdiensts finden Sie unter [Azure Container Registry-Tarife](container-registry-skus.md).

## <a name="preview-limitations"></a>Einschränkungen der Vorschau

* Derzeit nur in den folgenden Regionen unterstützt: „USA, Osten“, „USA, Osten 2“ und „USA, Westen 2“.
* Regionskonvertierungen in Verfügbarkeitszonen werden derzeit nicht unterstützt. Um die Unterstützung für Verfügbarkeitszonen in einer Region zu aktivieren, muss die Registrierung entweder in der gewünschten Region mit aktivierter Verfügbarkeitszonenunterstützung erstellt werden, oder sie muss mit aktivierter Unterstützung für Verfügbarkeitszonen hinzugefügt werden.
* Zonenredundanz kann in einer Region nicht deaktiviert werden.
* [ACR Tasks](container-registry-tasks-overview.md) unterstützt noch keine Verfügbarkeitszonen.
* Sie werden zurzeit durch Azure Resource Manager-Vorlagen oder das Azure-Portal unterstützt. Azure CLI-Unterstützung wir in einer zukünftigen Version aktiviert.

## <a name="about-zone-redundancy"></a>Informationen zur Zonenredundanz

Verwenden Sie Azure-[Verfügbarkeitszonen](../availability-zones/az-overview.md), um eine robuste und hochverfügbare Azure-Containerregistrierung innerhalb einer Azure-Region zu erstellen. Organisationen können beispielsweise eine zonenredundante Azure-Containerregistrierung mit anderen [unterstützten Azure-Ressourcen](../availability-zones/az-region.md) einrichten, um Datenresidenz- oder andere Complianceanforderungen zu erfüllen und gleichzeitig Hochverfügbarkeit innerhalb einer Region zu gewährleisten.

Azure Container Registry unterstützt außerdem [Georeplikation](container-registry-geo-replication.md), die den Dienst über mehrere Regionen hinweg repliziert, sodass Redundanz und Lokalität zu Compute-Ressourcen an anderen Standorten ermöglicht werden. Die Kombination aus Verfügbarkeitszonen für Redundanz innerhalb einer Region und Georeplikation über mehrere Regionen hinweg verbessert sowohl die Zuverlässigkeit als auch die Leistung einer Registrierung.

Verfügbarkeitszonen sind eindeutige physische Standorte in einer Azure-Region. Zur Gewährleistung der Resilienz sind in allen aktivierten Regionen mindestens drei separate Zonen vorhanden. Jede Zone enthält mindestens ein Rechenzentrum, das mit eigener unabhängiger Stromversorgung, Kühlung und Netzwerk ausgestattet ist. Wenn eine Registrierung (oder ein Registrierungsreplikat in einer anderen Region) für Zonenredundanz konfiguriert ist, wird sie über alle Verfügbarkeitszonen in der Region hinweg repliziert, sodass sie verfügbar bleibt, wenn es zu Rechenzentrumsausfällen kommt.

## <a name="create-a-zone-redundant-registry---portal"></a>Erstellen einer zonenredundanten Registrierung – Portal

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.
1. Klicken Sie auf **Ressource erstellen** > **Container** > **Container Registry**.
1. Wählen Sie auf der Registerkarte **Grundlagen** eine Ressourcengruppe aus, oder erstellen Sie eine, und geben Sie einen eindeutigen Registrierungsnamen ein. 
1. Wählen Sie unter **Standort** eine Region aus, die Zonenredundanz für Azure Container Registry unterstützt, z. B. *USA, Osten*.
1. Wählen Sie unter **SKU** die Option **Premium** aus.
1. Wählen Sie unter **Verfügbarkeitszonen** die Option **Aktiviert** aus.
1. Konfigurieren Sie optional zusätzliche Registrierungseinstellungen, und wählen Sie dann **Überprüfen und erstellen** aus.
1. Wählen Sie **Erstellen**, um die Registrierungsinstanz bereitzustellen.

    :::image type="content" source="media/zone-redundancy/enable-availability-zones-portal.png" alt-text="Aktivieren von Zonenredundanz im Azure-Portal":::

So erstellen Sie eine zonenredundante Replikation

1. Navigieren Sie zu Ihrer Containerregistrierung der Premium-Dienstebene, und wählen Sie **Replikationen** aus.
1. Wählen Sie auf der angezeigten Karte ein grünes Sechseck in einer Region aus, die Zonenredundanz für Azure Container Registry unterstützt, z. B. **USA, Westen 2**. Klicken Sie anschließend auf **Erstellen**.
1. Wählen Sie im Fenster **Replikation erstellen** in **Verfügbarkeitszonen** die Option **Aktiviert** aus, und wählen Sie dann **Erstellen** aus.

## <a name="create-a-zone-redundant-registry---template"></a>Erstellen einer zonenredundanten Registrierung – Vorlage

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Führen Sie bei Bedarf den Befehl [az group create](/cli/azure/group) aus, um eine Ressourcengruppe für die Registrierung in einer Region zu erstellen, die [Verfügbarkeitszonen](../availability-zones/az-region.md) für Azure Container Registry unterstützt, z. B. „USA, Osten“ (*eastus*).

```azurecli
az group create --name <resource-group-name> --location <location>
```

### <a name="deploy-the-template"></a>Bereitstellen der Vorlage 

Sie können die folgende Resource Manager-Vorlage verwenden, um eine zonenredundante, georeplizierte Registrierung zu erstellen. Die Vorlage aktiviert standardmäßig die Zonenredundanz in der Registrierung sowie ein zusätzliches regionales Replikat. 

Kopieren Sie den folgenden Inhalt in eine neue Datei, und speichern Sie sie unter einem Dateinamen, z. B. `registryZone.json`.

```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "acrName": {
        "type": "string",
        "defaultValue": "[concat('acr', uniqueString(resourceGroup().id))]",
        "minLength": 5,
        "maxLength": 50,
        "metadata": {
          "description": "Globally unique name of your Azure Container Registry"
        }
      },
      "location": {
        "type": "string",
        "defaultValue": "[resourceGroup().location]",
        "metadata": {
          "description": "Location for registry home replica."
        }
      },
      "acrSku": {
        "type": "string",
        "defaultValue": "Premium",
        "allowedValues": [
          "Premium"
        ],
        "metadata": {
          "description": "Tier of your Azure Container Registry. Geo-replication and zone redundancy require Premium SKU."
        }
      },
      "acrZoneRedundancy": {
        "type": "string",
        "defaultValue": "Enabled",
        "metadata": {
          "description": "Enable zone redundancy of registry's home replica. Requires registry location to support availability zones."
        }
      },
      "acrReplicaLocation": {
        "type": "string",
        "metadata": {
          "description": "Short name for registry replica location."
        }
      },
      "acrReplicaZoneRedundancy": {
        "type": "string",
        "defaultValue": "Enabled",
        "metadata": {
          "description": "Enable zone redundancy of registry replica. Requires replica location to support availability zones."
        }
      }
    },
    "resources": [
      {
        "comments": "Container registry for storing docker images",
        "type": "Microsoft.ContainerRegistry/registries",
        "apiVersion": "2020-11-01-preview",
        "name": "[parameters('acrName')]",
        "location": "[parameters('location')]",
        "sku": {
          "name": "[parameters('acrSku')]",
          "tier": "[parameters('acrSku')]"
        },
        "tags": {
          "displayName": "Container Registry",
          "container.registry": "[parameters('acrName')]"
        },
        "properties": {
          "adminUserEnabled": "[parameters('acrAdminUserEnabled')]",
          "zoneRedundancy": "[parameters('acrZoneRedundancy')]"
        }
      },
      {
        "type": "Microsoft.ContainerRegistry/registries/replications",
        "apiVersion": "2020-11-01-preview",
        "name": "[concat(parameters('acrName'), '/', parameters('acrReplicaLocation'))]",
        "location": "[parameters('acrReplicaLocation')]",
          "dependsOn": [
          "[resourceId('Microsoft.ContainerRegistry/registries/', parameters('acrName'))]"
        ],
        "properties": {
          "zoneRedundancy": "[parameters('acrReplicaZoneRedundancy')]"
        }
      }
    ],
    "outputs": {
      "acrLoginServer": {
        "value": "[reference(resourceId('Microsoft.ContainerRegistry/registries',parameters('acrName')),'2019-12-01-preview').loginServer]",
        "type": "string"
      }
    }
  }
```

Führen Sie den folgenden Befehl [az deployment group create](/cli/azure/deployment?view=azure-cli-latest) aus, um die Registrierung mithilfe der vorherigen Vorlagendatei zu erstellen. Geben Sie, wenn angezeigt, Folgendes an:

* Einen eindeutigen Registrierungsnamen, oder stellen Sie die Vorlage ohne Parameter bereit, und sie erstellt einen eindeutigen Name für Sie.
* Einen Standort für das Replikat, der Verfügbarkeitszonen unterstützt, z. B. „USA, Westen 2“ (*westus2*).

```azurecli
az deployment group create \
  --resource-group <resource-group-name> \
  --template-file registryZone.json \
  --parameters acrName=<registry-name> acrReplicaLocation=<replica-location>
```

Beachten Sie in der Befehlsausgabe die Eigenschaft `zoneRedundancy` für die Registrierung und das Replikat. Wenn diese Option aktiviert ist, ist jede Ressource zonenredundant:

```JSON
{
 [...]
"zoneRedundancy": "Enabled",
}
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Regionen, die Verfügbarkeitszonen unterstützen](../availability-zones/az-region.md).
* Weitere Informationen zum Erstellen für [Zuverlässigkeit](/azure/architecture/framework/resiliency/overview) in Azure.
