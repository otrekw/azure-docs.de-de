---
title: 'Schnellstart: Konfigurieren von NSG-Datenflussprotokollen per Azure Resource Manager-Vorlage'
description: Hier erfahren Sie, wie Sie NSG-Datenflussprotokolle programmgesteuert mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage) und unter Verwendung von Azure PowerShell aktivieren.
services: network-watcher
author: damendo
Customer intent: I need to enable the NSG flow logs using Azure Resource Manager Template
ms.service: network-watcher
ms.topic: quickstart
ms.date: 07/22/2020
ms.author: damendo
ms.custom: subject-armqs
ms.openlocfilehash: 96f30c05527754cbce3b7593c8d62fb56844d41e
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93042745"
---
# <a name="quickstart-configure-nsg-flow-logs-using-an-arm-template"></a>Schnellstart: Konfigurieren von NSG-Datenflussprotokollen mithilfe einer ARM-Vorlage

In dieser Schnellstartanleitung werden [NSG-Datenflussprotokolle](network-watcher-nsg-flow-logging-overview.md) mithilfe einer [Azure Resource Manager](../azure-resource-manager/management/overview.md)-Vorlage (ARM-Vorlage) und unter Verwendung von Azure PowerShell aktiviert.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Wir beginnen mit einer Übersicht über die Eigenschaften des NSG-Datenflussprotokoll-Objekts, gefolgt von einigen Beispielvorlagen. Anschließend stellen wir die Vorlage mithilfe einer lokalen PowerShell-Instanz bereit.

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-networkwatcher-flowLogs-create%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-networkwatcher-flowlogs-create).

:::code language="json" source="~/quickstart-templates/101-networkwatcher-flowlogs-create/azuredeploy.json":::

In der Vorlage sind mehrere Ressourcen definiert:

- [Microsoft.Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts)
- [Microsoft.Resources/deployments](/azure/templates/microsoft.resources/deployments)

## <a name="nsg-flow-logs-object"></a>NSG-Flussprotokollobjekt

Das NSG-Flussprotokollobjekt mit allen Parametern ist unten dargestellt. Eine umfassende Übersicht über die Eigenschaften finden Sie unter [Microsoft.Network networkWatchers/flowLogs](/azure/templates/microsoft.network/networkwatchers/flowlogs).

```json
{
  "name": "string",
  "type": "Microsoft.Network/networkWatchers/flowLogs",
  "location": "string",
  "apiVersion": "2019-09-01",
  "properties": {
    "targetResourceId": "string",
    "storageId": "string",
    "enabled": "boolean",
    "flowAnalyticsConfiguration": {
      "networkWatcherFlowAnalyticsConfiguration": {
        "enabled": "boolean",
        "workspaceResourceId": "string",
        "trafficAnalyticsInterval": "integer"
      },
      "retentionPolicy": {
        "days": "integer",
        "enabled": "boolean"
      },
      "format": {
        "type": "string",
        "version": "integer"
      }
    }
  }
}
```

Fügen Sie zum Erstellen einer Ressource vom Typ `Microsoft.Network/networkWatchers/flowLogs` den oben gezeigten JSON-Code im Ressourcenabschnitt der Vorlage hinzu.

## <a name="creating-your-template"></a>Erstellen der Vorlage

Wenn Sie zum ersten Mal ARM-Vorlagen verwenden, finden Sie unter den nachstehenden Links weitere Informationen.

- [Bereitstellen von Ressourcen mit ARM-Vorlagen und Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md#deploy-local-template)
- [Tutorial: Erstellen und Bereitstellen Ihrer ersten ARM-Vorlage](../azure-resource-manager/templates/template-tutorial-create-first-template.md)

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-networkwatcher-flowlogs-create).

Bei dem folgenden Beispiel einer vollständigen Vorlage handelt es sich um die einfachste Version mit minimalen Parametern, die zur Einrichtung von NSG-Datenflussprotokollen übergeben werden. Weitere Beispiele finden Sie in der [Schrittanleitung](network-watcher-nsg-flow-logging-azure-resource-manager.md).

**Beispiel:** Mit der nachstehenden Vorlage werden NSG-Datenflussprotokolle in einer Ziel-NSG aktiviert und in einem angegebenen Speicherkonto gespeichert.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "apiProfile": "2019-09-01",
  "resources": [
    {
      "name": "NetworkWatcher_centraluseuap/Microsoft.NetworkDalanDemoPerimeterNSG",
      "type": "Microsoft.Network/networkWatchers/FlowLogs/",
      "location": "centraluseuap",
      "apiVersion": "2019-09-01",
      "properties": {
        "targetResourceId": "/subscriptions/<subscription Id>/resourceGroups/DalanDemo/providers/Microsoft.Network/networkSecurityGroups/PerimeterNSG",
        "storageId": "/subscriptions/<subscription Id>/resourceGroups/MyCanaryFlowLog/providers/Microsoft.Storage/storageAccounts/storagev2ira",
        "enabled": true,
        "flowAnalyticsConfiguration": {},
        "retentionPolicy": {},
        "format": {}
      }
    }
  ]
}
```

> [!NOTE]
> - Der Name der Ressource hat das Format _übergeordnete Ressource_untergeordnete Ressource_. Hier ist die übergeordnete Ressource die regionale Network Watcher Instanz (Format: NetworkWatcher_RegionName. Beispiel: NetworkWatcher_centraluseuap)
> - `targetResourceId` ist die Ressourcen-ID der Ziel-NSG.
> - `storageId` ist die Ressourcen-ID des Zielspeicherkontos.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

In diesem Tutorial wird davon ausgegangen, dass Sie über eine Ressourcengruppe und eine NSG verfügen, in der Sie die Flussprotokollierung aktivieren können.
Sie können jede der oben aufgeführten Beispielvorlagen lokal als `azuredeploy.json` speichern. Aktualisieren Sie die Eigenschaftswerte so, dass sie auf gültige Ressourcen in Ihrem Abonnement verweisen.

Führen Sie den folgenden Befehl in PowerShell aus, um die Vorlage bereitzustellen.

```azurepowershell-interactive
$context = Get-AzSubscription -SubscriptionId <subscription Id>
Set-AzContext $context
New-AzResourceGroupDeployment -Name EnableFlowLog -ResourceGroupName NetworkWatcherRG `
    -TemplateFile "C:\MyTemplates\azuredeploy.json"
```

> [!NOTE]
> Mit den obigen Befehlen wird eine Ressource für die Ressourcengruppe „NetworkWatcherRG“ bereitgestellt, nicht für die Ressourcengruppe, die die NSG enthält.

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

Für die Überprüfung, ob die Bereitstellung erfolgreich war, gibt es mehrere Möglichkeiten. In der PowerShell-Konsole sollte `Succeeded` für `ProvisioningState` angezeigt werden. Darüber hinaus können Sie die Änderungen auf der [Portalseite für NSG-Datenflussprotokolle](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs) bestätigen. Falls bei der Bereitstellung Probleme aufgetreten sind, lesen Sie den Artikel [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../azure-resource-manager/templates/common-deployment-errors.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Azure ermöglicht das Löschen von Ressourcen über den Bereitstellungsmodus `Complete`. Zum Löschen einer Ressource für Datenflussprotokolle geben Sie eine Bereitstellung im Modus `Complete` ohne Angabe der zu löschenden Ressource an. Erfahren Sie mehr über den [Bereitstellungsmodus „Vollständig“](../azure-resource-manager/templates/deployment-modes.md#complete-mode).

Alternativ können Sie ein NSG-Datenflussprotokoll wie folgt über das Azure-Portal deaktivieren:

1. Melden Sie sich beim Azure-Portal an.
1. Wählen Sie oben links im Portal die Option **Alle Dienste** aus. Geben Sie im Feld **Filter** die Zeichenfolge _Network Watcher_ ein. Wählen Sie **Network Watcher** aus, wenn der Begriff in den Suchergebnissen angezeigt wird.
1. Wählen Sie unter **PROTOKOLLE** die Option **NSG-Datenflussprotokolle** aus.
1. Wählen Sie in der NSG-Liste die NSG aus, für die Sie Datenflussprotokolle deaktivieren möchten.
1. Legen Sie unter **Datenflussprotokolleinstellungen** den Datenflussprotokollstatus auf **Aus** fest.
1. Scrollen Sie nach unten, und wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie NSG-Datenflussprotokolle aktiviert. Informieren Sie sich als Nächstes darüber, wie Sie Ihre NSG-Datenflussprotokolle mit folgenden Hilfsmitteln visualisieren:

- [Microsoft Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)
- [Open-Source-Tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
- [Azure Traffic Analytics](traffic-analytics.md)
