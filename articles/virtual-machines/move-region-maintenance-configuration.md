---
title: Verschieben einer Wartungskonfiguration in eine andere Azure-Region
description: Erfahren Sie, wie eine Wartungskonfiguration in eine andere Azure-Region verschoben wird.
author: shants123
ms.service: virtual-machines
ms.topic: how-to
ms.tgt_pltfrm: vm
ms.date: 03/04/2020
ms.author: shants
ms.openlocfilehash: 4cff7eb4a69005f2e74747b6e58447f100c69b60
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86501601"
---
# <a name="move-a-maintenance-control-configuration-to-another-region"></a>Verschieben einer Wartungssteuerungskonfiguration in eine andere Region

In diesem Artikel wird beschrieben, wie Sie eine Wartungssteuerungskonfiguration in eine andere Azure-Region verschieben. Eine Konfiguration kann aus verschiedenen Gründen verschoben werden. Beispielsweise, um eine neue Region zu nutzen, um Features oder Dienste bereitzustellen, die in einer bestimmten Region verfügbar sind, um interne Richtlinien- und Governanceanforderungen zu erfüllen oder als Reaktion auf die Kapazitätsplanung.

Mit der Wartungssteuerung können Sie mit angepassten Wartungskonfigurationen steuern, wie Plattformupdates auf [Windows](./maintenance-control-cli.md?toc=/azure/virtual-machines/windows/toc.json&bc=/azure/virtual-machines/windows/breadcrumb/toc.json)- und [Linux](./maintenance-control-cli.md?toc=%2Fazure%2Fvirtual-machines%2Flinux%2Ftoc.json&bc=%2Fazure%2Fvirtual-machines%2Flinux%2Fbreadcrumb%2Ftoc.json&view=azure-java-stable)-VMs und auf Azure Dedicated Hosts angewendet werden. Es gibt eine Reihe von Szenarien, in denen die Wartungssteuerung regionsübergreifend verschoben wird:

- Wenn Sie Ihre Wartungssteuerungskonfiguration, nicht aber die der Konfiguration zugeordneten Ressourcen verschieben möchten, befolgen Sie die Anweisungen in diesem Artikel.
- Befolgen Sie die [diese Anweisungen](move-region-maintenance-configuration-resources.md), um die einer Wartungskonfiguration zugeordneten Ressourcen, nicht aber die Konfiguration selbst zu verschieben.
- Wenn Sie Ihre Wartungssteuerungskonfiguration, nicht aber die der Konfiguration zugeordneten Ressourcen verschieben möchten, befolgen Sie die Anweisungen in diesem Artikel. Befolgen Sie dann [diese Anweisungen](move-region-maintenance-configuration-resources.md).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit dem Verschieben einer Wartungssteuerungskonfiguration beginnen:

- Wartungskonfigurationen sind Azure-VMs oder Azure Dedicated Hosts zugeordnet. Stellen Sie sicher, dass die VM-/Hostressourcen in der neuen Region vorhanden sind, bevor Sie beginnen.
- Identifizieren Sie Folgendes: 
    - Vorhandene Wartungssteuerungskonfigurationen.
    - Die Ressourcengruppen, in denen sich vorhandene Konfigurationen derzeit befinden. 
    - Die Ressourcengruppen, denen die Konfigurationen nach dem Verschieben in die neue Region hinzugefügt werden. 
    - Die Ressourcen, die der zu verschiebenden Wartungskonfiguration zugeordnet sind.
    - Überprüfen Sie, ob die Ressourcen in der neuen Region mit denen identisch sind, die mit den aktuellen Wartungskonfigurationen verknüpft sind. Die Konfigurationen können die gleichen Namen in der neuen Region wie in der alten Region aufweisen. Dies ist jedoch nicht erforderlich.

## <a name="prepare-and-move"></a>Vorbereiten und Verschieben 

1. Rufen Sie alle Wartungskonfigurationen in den einzelnen Abonnements ab. Führen Sie den CLI-Befehl [az maintenance configuration list](/cli/azure/ext/maintenance/maintenance/configuration?view=azure-cli-latest#ext-maintenance-az-maintenance-configuration-list) aus, und ersetzen Sie dabei $subId durch Ihre Abonnement-ID.

    ```
    az maintenance configuration list --subscription $subId --query "[*].{Name:name, Location:location, ResGroup:resourceGroup}" --output table
    ```
2. Überprüfen Sie die zurückgegebene Tabellenliste der Konfigurationsdatensätze innerhalb des Abonnements. Hier sehen Sie ein Beispiel. Ihre Liste enthält Werte für ihre jeweilige Umgebung.

    **Name** | **Location** | **Ressourcengruppe**
    --- | --- | ---
    Wartung überspringen | eastus2 | configuration-resource-group
    IgniteDemoConfig | eastus2 | configuration-resource-group
    defaultMaintenanceConfiguration-eastus | eastus | test-configuration
    

3. Speichern Sie die Liste als Referenz. Wenn Sie die Konfigurationen verschieben, können Sie anhand der Liste überprüfen, ob alles verschoben wurde.
4. Ordnen Sie als Referenz jede Konfiguration/Ressourcengruppe der neuen Ressourcengruppe in der neuen Region zu.
5. Erstellen Sie mithilfe von [PowerShell](../virtual-machines/maintenance-control-powershell.md#create-a-maintenance-configuration) oder der [CLI](../virtual-machines/maintenance-control-cli.md#create-a-maintenance-configuration) neue Wartungskonfigurationen in der neuen Region.
6. Ordnen Sie die Konfigurationen den Ressourcen in der neuen Region zu, und verwenden Sie dazu [PowerShell](../virtual-machines/maintenance-control-powershell.md#assign-the-configuration) oder die [CLI](../virtual-machines/maintenance-control-cli.md#assign-the-configuration).


## <a name="verify-the-move"></a>Überprüfen der Verschiebung

Vergleichen Sie nach dem Verschieben der Konfigurationen die Konfigurationen und Ressourcen in der neuen Region mit der von Ihnen vorbereiteten Tabellenliste.


## <a name="clean-up-source-resources"></a>Bereinigen der Quellressourcen

Ziehen Sie nach dem Verschieben in Erwägung, die verschobenen Wartungskonfigurationen in der Quellregion, in [PowerShell](../virtual-machines/maintenance-control-powershell.md#remove-a-maintenance-configuration) oder in der [CLI](../virtual-machines/maintenance-control-cli.md#delete-a-maintenance-configuration) zu löschen.


## <a name="next-steps"></a>Nächste Schritte

Befolgen Sie [diese Anweisungen](move-region-maintenance-configuration-resources.md), wenn Sie den Wartungskonfigurationen zugeordnete Ressourcen verschieben müssen. 
