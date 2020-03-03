---
title: 'Azure-Messagingdienste: Service Manager an Ressourcen-Manager'
description: Dieser Artikel bietet eine Zuordnung der veralteten Azure Service Manager-REST-API und PowerShell-Cmdlets zur Resource Manager-REST-API und den zugehörigen PowerShell-Cmdlets.
services: service-bus-messaging, event-hubs, event-grid
documentationcenter: na
author: spelluru
editor: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2020
ms.author: spelluru
ms.openlocfilehash: d263381667319b98a28ee6168e2de75c4041b58a
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77589661"
---
# <a name="deprecation-of-azure-service-manager-support-for-azure-service-bus-relay-and-event-hubs"></a>Einstellung der Azure Service Manager-Unterstützung für Azure Service Bus, Relay und Event Hubs

Resource Manager, der Cloudinfrastrukturstapel der nächsten Generation, ersetzt vollständig das „klassische“ Azure-Dienstverwaltungsmodell (klassisches Bereitstellungsmodell). Folglich werden die REST-APIs des klassischen Bereitstellungsmodells und die Unterstützung für Service Bus, Relay und Event Hubs am 1. November 2021 eingestellt. Diese Einstellung wurde erstmals in einer [Ankündigung der Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Service-Bus-blog/Deprecating-Service-Management-support-for-Azure-Service-Bus/ba-p/370909) mitgeteilt, aber wir haben uns vor kurzem entschieden, den Einstellungszeitraum um zwei weitere Jahre ab dem Zeitpunkt der ursprünglichen Ankündigung zu verlängern. Für eine einfache Identifikation weisen diese APIs `management.core.windows.net` in Ihrem URI auf. In der folgenden Tabelle finden Sie eine Liste der veralteten APIs und der Azure Resource Manager-API-Version, die Sie jetzt verwenden sollten.

Um Service Bus, Relay und Event Hubs weiterhin zu verwenden, wechseln Sie bis zum 31. Oktober 2021 zu Resource Manager. Wir empfehlen allen Kunden, die noch mit alten APIs arbeiten, die Umstellung bald vorzunehmen, um die zusätzlichen Vorteile von Resource Manager zu nutzen. Dazu gehören die Gruppierung von Ressourcen, Tags, ein optimierter Bereitstellungs- und Verwaltungsprozess und eine präzise Zugriffssteuerung mit rollenbasierter Zugriffssteuerung (Role-Based Access Control, RBAC).

Weitere Informationen zu Azure Resource Manager im Vergleich zu Azure Service Manager finden Sie im [TechNet-Blog](https://blogs.technet.microsoft.com/meamcs/2016/12/22/difference-between-azure-service-manager-and-azure-resource-manager/).

Weitere Informationen zu Service Manager- und Resource Manager-APIs für Azure Service Bus, Relay und Event Hubs finden Sie in unserer REST-API-Dokumentation:

- [Azure Service Bus](/rest/api/servicebus/)
- [Azure Event Hubs](/rest/api/eventhub/)
- [Azure Relay](/rest/api/relay/)

## <a name="service-manager-rest-api---resource-manager-rest-api"></a>Service Manager-REST-API – Resource Manager-REST-API

| Service Manager-APIs (veraltet) | Resource Manager – Service Bus-API | Resource Manager – Event Hub-API | Resource Manager – Relay-API |
| --------------- | ----------------- | ----------------- | ----------------- | 
| **Namespaces-GetNamespaceAsync** <br/>[Service Bus Get Namespace](/rest/api/servicebus/get-namespace)<br/>[Event Hub Get Namespace](/rest/api/eventhub/get-event-hub)<br/>[Relay Get Namespace](/rest/api/servicebus/get-relays)<br/> ```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [get](/rest/api/servicebus/namespaces/get) | [get](/rest/api/eventhub/namespaces/get) | [get](/rest/api/relay/namespaces/get) |
| **ConnectionDetails-GetConnectionDetails**<br/>Service Bus/Event Hub/Relay GetConnectionDetails<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/ConnectionDetails``` | [listkeys](/rest/api/servicebus/namespaces/listkeys) | [listkeys](/rest/api/eventhub/namespaces/listkeys) | [listkeys](/rest/api/relay/namespaces/listkeys) |
| **Topics-GetTopicsAsync**<br/>Service Bus<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/topics? $skip={skip}&$top={top}``` | [list](/rest/api/servicebus/topics/listbynamespace) | &nbsp; | &nbsp; | 
| **Queues-GetQueueAsync** <br/>Service Bus<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/queues/{queueName}``` | [get](/rest/api/servicebus/queues/get) | &nbsp; | &nbsp; | 
| **Relays-GetRelaysAsync**<br/>[Get Relays](/rest/api/servicebus/get-relays)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/relays? $skip={skip}&$top={top}```| &nbsp; | &nbsp; | [list](/rest/api/relay/wcfrelays/listbynamespace) | 
| **NamespaceAuthorizationRules-GetNamespaceAuthorizationRuleAsync**<br/>Service Bus/Event Hub/Relay GetNamespaceAuthRule<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/authorizationrules?``` | [getauthorizationrule](/rest/api/servicebus/namespaces/getauthorizationrule) |[getauthorizationrule](/rest/api/eventhub/namespaces/getauthorizationrule) | [getauthorizationrule](/rest/api/relay/namespaces/getauthorizationrule) |
| **Namespaces-DeleteNamespaceAsync**<br/>[Service Bus Delete Namespace](/rest/api/servicebus/delete-namespace)<br/>[Event Hubs Delete Namespace](/rest/api/eventhub/delete-event-hub)<br/>[Relays Delete Namespace](/rest/api/servicebus/delete-namespace)<br/> ```DELETE  https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [delete](/rest/api/servicebus/namespaces/delete) | [delete](/rest/api/eventhub/namespaces/delete) | [delete](/rest/api/relay/namespaces/delete) | 
| **MessagingSKUPlan-GetPlanAsync**<br/>Service Bus/Event Hub/Relay Get Namespace<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/MessagingPlan``` | [get](/rest/api/servicebus/namespaces/get) | [get](/rest/api/eventhub/namespaces/get) | [get](/rest/api/relay/namespaces/get) |
| **MessagingSKUPlan-UpdatePlanAsync**<br/>Service Bus/Event Hub/Relay Get Namespace<br/>```PUT https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/MessagingPlan``` | [createorupdate](/rest/api/servicebus/namespaces/createorupdate) | [createorupdate](/rest/api/eventhub/namespaces/createorupdate) | [createorupdate](/rest/api/relay/namespaces/createorupdate) |
| **NamespaceAuthorizationRules-UpdateNamespaceAuthorizationRuleAsync**<br/>Service Bus/Event Hub/Relay Get Namespace<br/>```PUT https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules/{rule name}``` | [createorupdate](/rest/api/servicebus/namespaces/createorupdate) | [createorupdateauthorizationrule](/rest/api/eventhub/namespaces/createorupdateauthorizationrule) | [createorupdateauthorizationrule](/rest/api/relay/namespaces/createorupdateauthorizationrule) | 
| **NamespaceAuthorizationRules-CreateNamespaceAuthorizationRuleAsync**<br/> 
Service Bus/Event Hub/Relay<br/>```PUT https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules/{rule name}``` |[createorupdate](/rest/api/servicebus/namespaces/createorupdate) | [createorupdateauthorizationrule](/rest/api/eventhub/namespaces/createorupdateauthorizationrule) | [createorupdateauthorizationrule](/rest/api/relay/namespaces/createorupdateauthorizationrule) |
| **NamespaceProperties-GetNamespacePropertiesAsync**<br/>[Service Bus Get Namespace](/rest/api/servicebus/get-namespace)<br/>[Event Hubs Get Namespace](/rest/api/eventhub/get-event-hub)<br/>[Relay Get Namespace](/rest/api/servicebus/get-relays)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [get](/rest/api/servicebus/namespaces/get) | [get](/rest/api/eventhub/namespaces/get) | [get](/rest/api/relay/namespaces/get) |
| **RegionCodes-GetRegionCodesAsync**<br/>Service Bus/Event Hub/Relay Get Namespace<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [listbysku](/rest/api/servicebus/regions/listbysku) | [listbysku](/rest/api/eventhub/regions/listbysku) | &nbsp; | 
| **NamespaceProperties-UpdateNamespacePropertyAsync**<br/>Service Bus/Event Hub/Relay<br/>```GET    https://management.core.windows.net/{subscription ID}/services/ServiceBus/Regions/``` | [createorupdate](/rest/api/servicebus/namespaces/createorupdate) | [createorupdate](/rest/api/eventhub/namespaces/createorupdate) | [createorupdate](/rest/api/relay/namespaces/createorupdate) |
| **EventHubsCrud-ListEventHubsAsync**<br/>[List Event Hubs](/rest/api/eventhub/list-event-hubs)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/eventhubs?$skip={skip}&$top={top}``` | &nbsp; | [list](/rest/api/eventhub/eventhubs/listbynamespace) | &nbsp; | 
| **EventHubsCrud-ListEventHubsAsync**<br/>[Get Event Hubs](/rest/api/eventhub/get-event-hub)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/eventhubs/{eventHubPath}``` | &nbsp; | [get](/rest/api/eventhub/eventhubs/get) | &nbsp; | 
| **NamespaceAuthorizationRules-DeleteNamespaceAuthorizationRuleAsync**<br/>Service Bus/Event Hub/Relay<br/>```DELETE https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules/{rule name}``` | [deleteauthorizationrule](/rest/api/servicebus/namespaces/deleteauthorizationrule) | [deleteauthorizationrule](/rest/api/eventhub/namespaces/deleteauthorizationrule) | [deleteauthorizationrule](/rest/api/relay/namespaces/deleteauthorizationrule) |
| **NamespaceAuthorizationRules-GetNamespaceAuthorizationRulesAsync**<br/>Service Bus/Event Hub/Relay<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/AuthorizationRules``` | [listauthorizationrules](/rest/api/servicebus/namespaces/listauthorizationrules) | [listauthorizationrules](/rest/api/eventhub/namespaces/listauthorizationrules) | [listauthorizationrules](/rest/api/relay/namespaces/listauthorizationrules) |
| **NamespaceAvailability-IsNamespaceAvailable**<br/>[Service Bus-Namespaceverfügbarkeit](/rest/api/servicebus/check-namespace-availability)<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/CheckNamespaceAvailability/?namespace=<namespaceValue>``` | [checknameavailability](/rest/api/servicebus/namespaces/checknameavailability) | [checknameavailability](/rest/api/eventhub/namespaces/checknameavailability) | [checknameavailability](/rest/api/relay/namespaces/checknameavailability) |
| **Namespaces-CreateOrUpdateNamespaceAsync**<br/>Service Bus/Event Hub/Relay<br/>```PUT https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}``` | [createorupdate](/rest/api/servicebus/namespaces/createorupdate) | [createorupdate](/rest/api/eventhub/namespaces/createorupdate) | [createorupdate](/rest/api/relay/namespaces/createorupdate) | 
| **Topics-GetTopicAsync**<br/>```GET https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/topics/{topicPath}``` | [get](/rest/api/servicebus/topics/get) | &nbsp; | &nbsp; |

## <a name="service-manager-powershell---resource-manager-powershell"></a>Service Manager PowerShell – Resource Manager PowerShell
| Service Manager PowerShell-Befehl (veraltet) | Neue Resource Manager-Befehle | Neuerer Resource Manager-Befehl |
| ----- | ----- | ----- | 
| [Get-AzureSBAuthorizationRule](/powershell/module/servicemanagement/azure/get-azuresbauthorizationrule?view=azuresmps-4.0.0) | [Get-AzureRmServiceBusAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusauthorizationrule?view=azurermps-6.13.0) | [Get-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/get-azservicebusauthorizationrule?view=azps-1.6.0) |
| [Get-AzureSBLocation](/powershell/module/servicemanagement/azure/get-azuresblocation?view=azuresmps-4.0.0) | [Get-AzureRmServiceBusGeoDRConfiguration](/powershell/module/azurerm.servicebus/get-azurermservicebusgeodrconfiguration?view=azurermps-6.13.0) | [Get-AzServiceBusGeoDRConfiguration](/powershell/module/az.servicebus/get-azservicebusgeodrconfiguration?view=azps-1.6.0) |
| [Get-AzureSBNamespace](/powershell/module/servicemanagement/azure/get-azuresbnamespace?view=azuresmps-4.0.0) | [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace?view=azurermps-6.13.0) | [Get-AzServiceBusNamespace](/powershell/module/az.servicebus/get-azservicebusnamespace?view=azps-1.6.0) |
| [New-AzureSBAuthorizationRule](/powershell/module/servicemanagement/azure/new-azuresbauthorizationrule?view=azuresmps-4.0.0) | [New-AzureRmServiceBusAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusauthorizationrule?view=azurermps-6.13.0) | [New-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/new-azservicebusauthorizationrule?view=azps-1.6.0) |
| [New-AzureSBNamespace](/powershell/module/servicemanagement/azure/new-azuresbnamespace?view=azuresmps-4.0.0) | [New-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace?view=azurermps-6.13.0) | [New-AzServiceBusNamespace](/powershell/module/az.servicebus/new-azservicebusnamespace?view=azps-1.6.0) |
| [Remove-AzureRmRelayAuthorizationRule](/powershell/module/azurerm.relay/remove-azurermrelayauthorizationrule?view=azurermps-6.13.0) | [Remove-AzureRmEventHubAuthorizationRule](/powershell/module/azurerm.eventhub/remove-azurermeventhubauthorizationrule?view=azurermps-6.13.0) | [Remove-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/remove-azservicebusauthorizationrule?view=azps-1.6.0) |
| [Remove-AzureSBNamespace](/powershell/module/servicemanagement/azure/remove-azuresbnamespace?view=azuresmps-4.0.0) | [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace?view=azurermps-6.13.0) | [Remove-AzServiceBusNamespace](/powershell/module/az.servicebus/remove-azservicebusnamespace?view=azps-1.6.0) |
| [Set-AzureSBAuthorizationRule](/powershell/module/servicemanagement/azure/set-azuresbauthorizationrule?view=azuresmps-4.0.0) | [Set-AzureRmServiceBusAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusauthorizationrule?view=azurermps-6.13.0) | [Set-AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/set-azservicebusauthorizationrule?view=azps-1.6.0) |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in der folgenden Dokumentation: 

- Neueste REST-API-Dokumentation
    - [Azure Service Bus](/rest/api/servicebus/)
    - [Azure Event Hubs](/rest/api/eventhub/)
    - [Azure Relay](/rest/api/relay/)
- Neueste PowerShell-Dokumentation
    - [Azure Service Bus](/powershell/module/azurerm.servicebus/?view=azurermps-6.13.0#service_bus)
    - [Azure Event Hubs](/powershell/module/azurerm.eventhub/?view=azurermps-6.13.0#event_hub)
    - [Azure Event Grid](/powershell/module/azurerm.eventgrid/?view=azurermps-6.13.0#event_grid)
