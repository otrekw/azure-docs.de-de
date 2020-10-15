---
title: Aktivieren von Azure Monitor für VMs mit Resource Manager-Vorlagen
description: In diesem Artikel wird beschrieben, wie Sie Azure Monitor für VMs mithilfe von Azure PowerShell oder Azure Resource Manager-Vorlagen für mindestens einen virtuellen Azure-Computer oder eine VM-Skalierungsgruppe aktivieren.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: 89a9a1b762e02237a8ee08dca5d6eedefabaafbb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87327995"
---
# <a name="enable-azure-monitor-for-vms-using-resource-manager-templates"></a>Aktivieren von Azure Monitor für VMs mit Resource Manager-Vorlagen
In diesem Artikel wird beschrieben, wie Sie Azure Monitor für VMs für einen virtuellen Computer oder eine VM-Skalierungsgruppe mithilfe von Resource Manager-Vorlagen aktivieren. Dieses Verfahren kann für Folgendes verwendet werden:

- Virtueller Azure-Computer
- Azure-VM-Skalierungsgruppe
- Hybrid-VM, die mit Azure Arc verbunden ist

## <a name="prerequisites"></a>Voraussetzungen

- [Erstellen und Konfigurieren eines Log Analytics-Arbeitsbereichs](vminsights-configure-workspace.md). 
- Überprüfen Sie anhand der Liste unter [Unterstützte Betriebssysteme](vminsights-enable-overview.md#supported-operating-systems), ob das Betriebssystem der VM oder VM-Skalierungsgruppe, die aktiviert wird, unterstützt wird. 

## <a name="resource-manager-templates"></a>Resource Manager-Vorlagen

Für das Onboarding Ihrer VMs oder VM-Skalierungsgruppen stehen Azure Resource Manager-Beispielvorlagen zur Verfügung. Diese Vorlagen umfassen Szenarios, mit denen Sie die Überwachung für eine vorhandene Ressource aktivieren und eine neue Ressource mit aktivierter Überwachung erstellen können.

>[!NOTE]
>Die Vorlage muss in derselben Ressourcengruppe bereitgestellt werden wie die VM oder VM-Skalierungsgruppe, die aktiviert wird.


Die Azure Resource Manager-Vorlagen werden in einer Archivdatei (.zip) bereitgestellt, die Sie aus dem GitHub-Repository [herunterladen](https://aka.ms/VmInsightsARMTemplates) können. Zu den Inhalten der Datei gehören Ordner, die einzelne Bereitstellungsszenarios darstellen und eine Vorlage sowie eine Parameterdatei enthalten. Vor der Ausführung passen Sie die Parameterdatei an, und legen Sie die erforderlichen Werte fest. 

Die herunterladbare Datei enthält die folgenden Vorlagen für verschiedene Szenarios:

- Mit der Vorlage **ExistingVmOnboarding** wird Azure Monitor für VMs aktiviert, wenn die VM bereits vorhanden ist.
- Mit der Vorlage **NewVmOnboarding** wird eine VM erstellt und Azure Monitor für VMs wird aktiviert, um diese zu überwachen.
- Mit der Vorlage **ExistingVmssOnboarding** wird Azure Monitor für VMs aktiviert, wenn die VM-Skalierungsgruppe bereits vorhanden ist.
- Mit der Vorlage **NewVmssOnboarding** werden VM-Skalierungsgruppen erstellt und Azure Monitor für VMs wird aktiviert, um diese zu überwachen.
- Mit der Vorlage **ConfigureWorkspace** wird Ihr Log Analytics-Arbeitsbereich für die Unterstützung von Azure Monitor für VMs konfiguriert, indem die Lösungen und die Erfassung von Leistungsindikatoren der Betriebssysteme Linux und Windows aktiviert werden.

>[!NOTE]
>Wenn bereits VM-Skalierungsgruppen vorhanden sind und die Upgraderichtlinie auf **Manuell** festgelegt ist, wird Azure Monitor für VMs für diese Instanzen nicht standardmäßig aktiviert, wenn die Azure Resource Manager-Vorlage **ExistingVmssOnboarding** ausgeführt wird. Sie müssen ein manuelles Upgrade für diese Instanzen durchführen.

## <a name="deploy-templates"></a>Bereitstellen von Vorlagen
Die Vorlagen können über eine [beliebige Bereitstellungsmethode für Resource Manager-Vorlagen](../../azure-resource-manager/templates/deploy-powershell.md) bereitgestellt werden. Dazu zählen u. a. die folgenden Beispiele, bei denen PowerShell und die CLI verwendet werden.

```powershell
New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile <Template.json> -TemplateParameterFile <Parameters.json>
```


```azurecli
az group deployment create --resource-group <ResourceGroupName> --template-file <Template.json> --parameters <Parameters.json>
```



## <a name="next-steps"></a>Nächste Schritte

Nachdem die Überwachung für Ihre virtuellen Computer aktiviert wurde, stehen diese Informationen für die Analyse mit Azure Monitor für VMs zur Verfügung.

- Informationen zu ermittelten Anwendungsabhängigkeiten finden Sie unter [Azure Monitor für VMs – Zuordnung anzeigen](vminsights-maps.md).

- Informationen zum Erkennen von Engpässen und der Gesamtauslastung im Hinblick auf die Leistung Ihrer VM finden Sie unter [Anzeigen der Leistung von Azure-VMs](vminsights-performance.md).
