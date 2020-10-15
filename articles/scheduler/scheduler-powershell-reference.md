---
title: Referenz zu PowerShell-Cmdlets
description: Informationen zu PowerShell-Cmdlets für den Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 5b82dba923db16e96cc0884b629723c4e8496c3a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87080912"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>PowerShell-Cmdlets-Referenz für den Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ersetzt den Microsoft Azure Scheduler, der [ eingestellt wird](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date). Wenn Sie weiterhin mit den Aufträgen arbeiten möchten, die Sie in Scheduler eingerichtet haben, sollten Sie so bald wie möglich [zu Azure Logic Apps migrieren](../scheduler/migrate-from-scheduler-to-logic-apps.md). 
>
> Scheduler ist nicht mehr in der Azure-Portal verfügbar, aber die [REST-API](/rest/api/scheduler) und [Azure Scheduler-PowerShell-Cmdlets](scheduler-powershell-reference.md) sind weiterhin verfügbar, damit Sie Ihre Aufträge und Auftragssammlungen verwalten können.

Um Skripts zum Erstellen und Verwalten von Scheduler-Aufträgen und Auftragssammlungen zu erstellen, können Sie PowerShell-Cmdlets verwenden. Dieser Artikel enthält die wichtigsten PowerShell-Cmdlets für den Azure Scheduler mit Links zu den jeweiligen Referenzartikeln. Um Azure PowerShell für Ihr Azure-Abonnement zu installieren, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/). Weitere Informationen zu [Azure Resource Manager-Cmdlets](/powershell/azure/) finden Sie unter [Verwenden von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

| Cmdlet | BESCHREIBUNG |
|--------|-------------|
| [Disable-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |Deaktiviert eine Auftragssammlung. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |Aktiviert eine Auftragssammlung. |
| [Get-AzSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Ruft Scheduler-Aufträge ab. |
| [Get-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |Ruft Auftragssammlungen ab. |
| [Get-AzSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |Ruft den Auftragsverlauf ab. |
| [New-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Erstellt einen HTTP-Auftrag. |
| [New-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |Erstellt eine Auftragssammlung. |
| [New-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) | Erstellt einen Service Bus-Warteschlangenauftrag. |
| [New-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Erstellt einen Service Bus-Themenauftrag. |
| [New-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Erstellt einen Storage-Warteschlangenauftrag. |
| [Remove-AzSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Entfernt einen Scheduler-Auftrag. |
| [Remove-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |Entfernt eine Auftragssammlung. |
| [Set-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Ändert einen Scheduler-HTTP-Auftrag. |
| [Set-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |Ändert eine Auftragssammlung. |
| [Set-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Ändert einen Service Bus-Warteschlangenauftrag. |
| [Set-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Ändert einen Service Bus-Themenauftrag. |
| [Set-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Ändert einen Storage-Warteschlangenauftrag. |
||| 

Um weitere Informationen zu erhalten, können Sie eines dieser Cmdlets ausführen: 

```text
Get-Help <cmdlet name> -Detailed
Get-Help <cmdlet name> -Examples
Get-Help <cmdlet name> -Full
```

## <a name="next-steps"></a>Nächste Schritte

* [Konzepte, Terminologie und Entitätshierarchie für Azure Scheduler](scheduler-concepts-terms.md)
* [Einschränkungen, Standardwerte und Fehlercodes für Azure Scheduler](scheduler-limits-defaults-errors.md)
* [Azure Scheduler-REST-API – Referenz](/rest/api/scheduler)