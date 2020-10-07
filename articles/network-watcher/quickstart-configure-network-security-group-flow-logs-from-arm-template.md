---
title: 'Schnellstart: Konfigurieren von NSG-Datenflussprotokollen per Azure Resource Manager-Vorlage'
description: Hier erfahren Sie, wie Sie NSG-Datenflussprotokolle programmgesteuert mithilfe einer Azure Resource Manager-Vorlage und unter Verwendung von Azure PowerShell aktivieren.
services: network-watcher
author: damendo
Customer intent: I need to enable the NSG Flow Logs using Azure Resource Manager Template
ms.service: network-watcher
ms.topic: quickstart
ms.date: 07/22/2020
ms.author: damendo
ms.custom: subject-armqs
ms.openlocfilehash: 7d8cb89b1187bb15e7b361e1b6b9505400c612b5
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "87986316"
---
# <a name="quickstart-configure-nsg-flow-logs-from-arm-template"></a>Schnellstart: Konfigurieren von NSG-Datenflussprotokollen über eine ARM-Vorlage

In dieser Schnellstartanleitung werden [NSG-Datenflussprotokolle](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) programmgesteuert mithilfe einer [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/overview)-Vorlage (ARM-Vorlage) und unter Verwendung von Azure PowerShell aktiviert. 

Wir beginnen mit einer Übersicht über die Eigenschaften des NSG-Datenflussprotokoll-Objekts, gefolgt von einigen Beispielvorlagen. Anschließend stellen wir die Vorlage mithilfe einer lokalen PowerShell-Instanz bereit.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="nsg-flow-logs-object"></a>NSG-Flussprotokollobjekt

Das NSG-Flussprotokollobjekt mit allen Parametern ist unten dargestellt.
Eine umfassende Übersicht über die Eigenschaften finden Sie in der [Vorlagenreferenz zu NSG-Flussprotokollen](https://docs.microsoft.com/azure/templates/microsoft.network/networkwatchers/flowlogs).

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
Fügen Sie zum Erstellen einer Ressource vom Typ Microsoft.Network/networkWatchers/flowLogs dem Abschnitt „resources“ Ihrer Vorlage den obigen JSON-Code hinzu.


## <a name="creating-your-template"></a>Erstellen der Vorlage

Wenn Sie zum ersten Mal Azure Resource Manager-Vorlagen verwenden, finden Sie unter den nachstehenden Links weitere Informationen.

* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-powershell#deploy-local-template)
* [Tutorial: Erstellen und Bereitstellen Ihrer ersten Azure Resource Manager-Vorlage](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-tutorial-create-first-template?tabs=azure-powershell)

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-networkwatcher-flowlogs-create).

Bei dem folgenden Beispiel einer vollständigen Vorlage handelt es sich um die einfachste Version mit minimalen Parametern, die zur Einrichtung von NSG-Datenflussprotokollen übergeben werden. Weitere Beispiele finden Sie unter [diesem Link](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-azure-resource-manager).

**Beispiel:** Mit der nachstehenden Vorlage werden NSG-Flussprotokolle in einer Ziel-NSG aktiviert und in einem angegebenen Speicherkonto gespeichert.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "apiProfile": "2019-09-01",
  "resources": [
 {
    "name": "NetworkWatcher_centraluseuap/Microsoft.NetworkDalanDemoPerimeterNSG",
    "type": "Microsoft.Network/networkWatchers/FlowLogs/",
    "location": "centraluseuap",
    "apiVersion": "2019-09-01",
    "properties": {
      "targetResourceId": "/subscriptions/56abfbd6-ec72-4ce9-831f-bc2b6f2c5505/resourceGroups/DalanDemo/providers/Microsoft.Network/networkSecurityGroups/PerimeterNSG",
      "storageId": "/subscriptions/56abfbd6-ec72-4ce9-831f-bc2b6f2c5505/resourceGroups/MyCanaryFlowLog/providers/Microsoft.Storage/storageAccounts/storagev2ira",
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
> * Der Name der Ressource weist das Format „übergeordnete Ressource_untergeordnete Ressource“ auf. Hier ist die übergeordnete Ressource die regionale Network Watcher Instanz (Format: NetworkWatcher_RegionName. Beispiel: NetworkWatcher_centraluseuap)
> * targetResourceId ist die Ressourcen-ID der Ziel-NSG.
> * storageId ist die Ressourcen-ID des Zielspeicherkontos.


## <a name="deploying-your-azure-resource-manager-template"></a>Bereitstellen der Azure Resource Manager-Vorlage

In diesem Tutorial wird davon ausgegangen, dass Sie über eine Ressourcengruppe und eine NSG verfügen, in der Sie die Flussprotokollierung aktivieren können.
Sie können jede der oben aufgeführten Beispielvorlagen lokal als `azuredeploy.json` speichern. Aktualisieren Sie die Eigenschaftswerte so, dass sie auf gültige Ressourcen in Ihrem Abonnement verweisen.

Führen Sie den folgenden Befehl in PowerShell aus, um die Vorlage bereitzustellen.
```azurepowershell-interactive
$context = Get-AzSubscription -SubscriptionId 56acfbd6-vc72-43e9-831f-bcdb6f2c5505
Set-AzContext $context
New-AzResourceGroupDeployment -Name EnableFlowLog -ResourceGroupName NetworkWatcherRG `
    -TemplateFile "C:\MyTemplates\azuredeploy.json"
```

> [!NOTE]
> Mit den obigen Befehlen wird eine Ressource für die Ressourcengruppe „NetworkWatcherRG“ bereitgestellt, nicht für die Ressourcengruppe, die die NSG enthält.


## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

Für die Überprüfung, ob die Bereitstellung erfolgreich war, gibt es mehrere Möglichkeiten. In Ihrer PowerShell-Konsole sollte „ProvisioningState“ als „erfolgreich“ angezeigt werden. Darüber hinaus können Sie die Änderungen auf der [Portalseite für NSG-Flussprotokolle](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs) bestätigen. Falls bei der Bereitstellung Probleme aufgetreten sind, lesen Sie den Artikel [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/templates/common-deployment-errors).

## <a name="deleting-your-resource"></a>Löschen Ihrer Ressource
Azure ermöglicht das Löschen von Ressourcen über den Bereitstellungsmodus „Vollständig“. Zum Löschen einer Ressource des Flowprotokolls geben Sie eine Bereitstellung im Modus „Vollständig“ ohne Angabe der zu löschenden Ressource an. Erfahren Sie mehr über den [Bereitstellungsmodus „Vollständig“](https://docs.microsoft.com/azure/azure-resource-manager/templates/deployment-modes#complete-mode).

Alternativ können Sie ein NSG-Datenflussprotokoll wie folgt über das Azure-Portal deaktivieren:
1. Melden Sie sich beim Azure-Portal an.
2. Wählen Sie oben links im Portal die Option **Alle Dienste** aus. Geben Sie im Feld **Filter** die Zeichenfolge *Network Watcher* ein. Wählen Sie **Network Watcher** aus, wenn der Begriff in den Suchergebnissen angezeigt wird.
3. Wählen Sie unter **PROTOKOLLE** die Option **NSG-Datenflussprotokolle** aus.
4. Wählen Sie in der NSG-Liste die NSG aus, für die Sie Datenflussprotokolle deaktivieren möchten.
5. Legen Sie unter **Datenflussprotokolleinstellungen** den Datenflussprotokollstatus auf **Aus** fest.
6. Scrollen Sie nach unten, und wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie NSG-Datenflussprotokolle aktiviert. Informieren Sie sich als Nächstes darüber, wie Sie Ihre NSG-Datenflussprotokolle mit folgenden Hilfsmitteln visualisieren: 

* [Microsoft Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)
* [Open-Source-Tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
* [Azure Traffic Analytics](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)
