---
title: Aktivieren von VM Insights mithilfe von Resource Manager-Vorlagen
description: In diesem Artikel wird beschrieben, wie Sie VM Insights mithilfe von Azure PowerShell oder Azure Resource Manager-Vorlagen für mindestens einen virtuellen Azure-Computer oder eine Virtual Machine Scale Sets-Instanz aktivieren.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: fc0c304a3fea81f44e01d3e815f34e44728ea42e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102031865"
---
# <a name="enable-vm-insights-using-resource-manager-templates"></a>Aktivieren von VM Insights mithilfe von Resource Manager-Vorlagen
In diesem Artikel wird beschrieben, wie Sie VM Insights für einen virtuellen Computer oder eine Virtual Machine Scale Sets-Instanz mithilfe von Resource Manager-Vorlagen aktivieren. Dieses Verfahren kann für Folgendes verwendet werden:

- Virtueller Azure-Computer
- Azure-VM-Skalierungsgruppe
- Hybrid-VM, die mit Azure Arc verbunden ist

## <a name="prerequisites"></a>Voraussetzungen

- [Erstellen und konfigurieren Sie einen Log Analytics-Arbeitsbereich](./vminsights-configure-workspace.md). 
- Überprüfen Sie anhand der Liste unter [Unterstützte Betriebssysteme](./vminsights-enable-overview.md#supported-operating-systems), ob das Betriebssystem der VM oder VM-Skalierungsgruppe, die aktiviert wird, unterstützt wird. 

## <a name="resource-manager-templates"></a>Resource Manager-Vorlagen

Für das Onboarding Ihrer VMs oder VM-Skalierungsgruppen stehen Azure Resource Manager-Beispielvorlagen zur Verfügung. Diese Vorlagen umfassen Szenarios, mit denen Sie die Überwachung für eine vorhandene Ressource aktivieren und eine neue Ressource mit aktivierter Überwachung erstellen können.

>[!NOTE]
>Die Vorlage muss in derselben Ressourcengruppe bereitgestellt werden wie die VM oder VM-Skalierungsgruppe, die aktiviert wird.


Die Azure Resource Manager-Vorlagen werden in einer Archivdatei (.zip) bereitgestellt, die Sie aus dem GitHub-Repository [herunterladen](https://aka.ms/VmInsightsARMTemplates) können. Zu den Inhalten der Datei gehören Ordner, die einzelne Bereitstellungsszenarios darstellen und eine Vorlage sowie eine Parameterdatei enthalten. Vor der Ausführung passen Sie die Parameterdatei an, und legen Sie die erforderlichen Werte fest. 

Die herunterladbare Datei enthält die folgenden Vorlagen für verschiedene Szenarios:

- Mit der Vorlage **ExistingVmOnboarding** wird VM Insights aktiviert, wenn die VM bereits vorhanden ist.
- Mit der Vorlage **NewVmOnboarding** wird eine VM erstellt und VM Insights aktiviert, um diese zu überwachen.
- Mit der Vorlage **ExistingVmssOnboarding** wird VM Insights aktiviert, wenn die Virtual Machine Scale Sets-Instanz bereits vorhanden ist.
- Mit der Vorlage **NewVmssOnboarding** werden Virtual Machine Scale Sets-Instanzen erstellt und VM Insights aktiviert, um diese zu überwachen.
- Mit der Vorlage **ConfigureWorkspace** wird Ihr Log Analytics-Arbeitsbereich für die Unterstützung von VM Insights konfiguriert, indem die Lösungen und die Erfassung von Leistungsindikatoren der Betriebssysteme Linux und Windows aktiviert werden.

>[!NOTE]
>Wenn bereits Virtual Machine Scale Sets-Instanzen vorhanden sind und die Upgraderichtlinie auf **Manuell** festgelegt ist, wird VM Insights für diese Instanzen nicht standardmäßig aktiviert, wenn die Azure Resource Manager-Vorlage **ExistingVmssOnboarding** ausgeführt wird. Sie müssen ein manuelles Upgrade für diese Instanzen durchführen.

## <a name="deploy-templates"></a>Bereitstellen von Vorlagen
Die Vorlagen können über eine [beliebige Bereitstellungsmethode für Resource Manager-Vorlagen](../../azure-resource-manager/templates/deploy-powershell.md) bereitgestellt werden. Dazu zählen u. a. die folgenden Beispiele, bei denen PowerShell und die CLI verwendet werden.

```powershell
New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile <Template.json> -TemplateParameterFile <Parameters.json>
```


```azurecli
az deployment group create --resource-group <ResourceGroupName> --template-file <Template.json> --parameters <Parameters.json>
```



## <a name="next-steps"></a>Nächste Schritte

Nachdem die Überwachung für Ihre virtuellen Computer aktiviert wurde, stehen diese Informationen nun für die Analyse mit VM Insights zur Verfügung.

- Informationen zu ermittelten Anwendungsabhängigkeiten finden Sie unter [Anzeigen der Zuordnung in VM Insights](vminsights-maps.md).

- Informationen zum Erkennen von Engpässen und der Gesamtauslastung im Hinblick auf die Leistung Ihrer VM finden Sie unter [Anzeigen der Leistung von Azure-VMs](vminsights-performance.md).