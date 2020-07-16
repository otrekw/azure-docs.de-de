---
title: Upgraden der Konfiguration eines Azure Service Fabric-Clusters
description: Erfahren Sie, wie Sie mithilfe einer Resource Manager-Vorlage die Konfiguration upgraden, die einen Service Fabric-Cluster in Azure ausführt.
author: dkkapur
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 010f2b5d6dee3a5985c421e468bcd28b18148e5a
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86247726"
---
# <a name="upgrade-the-configuration-of-a-cluster-in-azure"></a>Upgraden der Konfiguration eines Clusters in Azure 

In diesem Artikel wird beschrieben, wie Sie die verschiedenen Fabric-Einstellungen für Ihren Service Fabric-Cluster anpassen. Für in Azure gehostete Cluster können Sie Einstellungen über das [Azure-Portal](https://portal.azure.com) oder mithilfe einer Azure Resource Manager-Vorlage anpassen.

> [!NOTE]
> Im Portal sind nicht alle Einstellungen verfügbar, [und es empfiehlt sich die Anpassung mithilfe einer Azure Resource Manager-Vorlage](./service-fabric-best-practices-infrastructure-as-code.md). Das Portal kann nur für das Szenario „Service Fabric Dev\Test“ verwendet werden.
> 


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Anpassen von Clustereinstellungen mithilfe von Resource Manager-Vorlagen
Azure-Cluster können über die JSON-Resource Manager-Vorlage konfiguriert werden. Weitere Informationen zu den verschiedenen Einstellungen finden Sie unter [Konfigurationseinstellungen für Cluster](service-fabric-cluster-fabric-settings.md). In den folgenden Schritten wird beispielhaft gezeigt, wie die neue Einstellung *MaxDiskQuotaInMB* mit dem Azure-Ressourcen-Explorer dem Abschnitt *Diagnose* hinzugefügt wird.

1. Besuchen Sie https://resources.azure.com.
2. Navigieren Sie zu Ihrem Abonnement, indem Sie **subscriptions** ->  **\<Your Subscription>**  -> **resourceGroups** ->  **\<Your Resource Group>**  -> **providers** -> **Microsoft.ServiceFabric** -> **clusters** ->  **\<Your Cluster Name>** erweitern.
3. Wählen Sie in der oberen rechten Ecke **Lesen/Schreiben** aus.
4. Wählen Sie **Bearbeiten** aus, aktualisieren Sie das JSON-Element `fabricSettings`, und fügen Sie ein neues Element hinzu:

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

Sie können auch auf eine der folgenden Arten mit Azure Resource Manager Clustereinstellungen anpassen:

- Verwenden Sie das [Azure-Portal](../azure-resource-manager/templates/export-template-portal.md), um die Resource Manager-Vorlage zu exportieren und zu aktualisieren.
- Verwenden Sie [PowerShell](../azure-resource-manager/management/manage-resources-powershell.md), um die Resource Manager-Vorlage zu exportieren und zu aktualisieren.
- Verwenden Sie die [Azure CLI](../azure-resource-manager/management/manage-resources-cli.md), um die Resource Manager-Vorlage zu exportieren und zu aktualisieren.
- Verwenden Sie die Azure PowerShell-Befehle [Set-AzServiceFabricSetting](/powershell/module/az.servicefabric/set-azservicefabricsetting) und [Remove-AzServiceFabricSetting](/powershell/module/az.servicefabric/remove-azservicefabricsetting), um die Einstellung direkt zu ändern.
- Verwenden Sie die [az sf cluster setting](/cli/azure/sf/cluster/setting)-Befehle der Azure CLI, um die Einstellung direkt zu ändern.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über die [Service Fabric-Clustereinstellungen](service-fabric-cluster-fabric-settings.md).
* Machen Sie sich mit der Vorgehensweise zum [Skalieren Ihres Clusters](service-fabric-cluster-scale-in-out.md) vertraut.
