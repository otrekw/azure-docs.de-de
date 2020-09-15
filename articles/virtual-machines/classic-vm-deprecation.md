---
title: Klassische Azure-VMs werden ab dem 1. März 2023 nicht mehr unterstützt
description: Der Artikel bietet einen allgemeinen Überblick über das Auslaufen klassischer VMs.
author: tanmaygore
manager: vashan
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: tagore
ms.openlocfilehash: de590168ecf4febe3c18442a6dc6c1c2eba4dc51
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89297602"
---
# <a name="migrate-your-iaas-resources-to-azure-resource-manager-by-march-1-2023"></a>Migrieren Ihrer IaaS-Ressourcen zu Azure Resource Manager vor dem 1. März 2023 

2014 haben wir IaaS in Azure Resource Manager eingeführt und die Funktionen seitdem kontinuierlich weiter ausgebaut. Da [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/) jetzt über vollständige IaaS-Funktionen und andere Erweiterungen verfügt, haben wir am 28. Februar 2020 begonnen, die Verwaltung von IaaS-VMs über [Azure Service Manager](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-faq#what-is-azure-service-manager-and-what-does-it-mean-by-classic) einzustellen. Am 1. März 2023 wird die Unterstützung dieser Funktionen vollständig beendet. 

Derzeit nutzen etwa 90% der IaaS-VMs Azure Resource Manager. Wenn Sie IaaS-Ressourcen über Azure Service Manager (ASM) nutzen, sollten Sie ab sofort mit der Planung der Migration beginnen und diesen Vorgang bis zum 1. März 2023 abschließen, um zu [Azure Resource Manager](../azure-resource-manager/management/index.yml) zu wechseln.

Für klassische VMs gilt die [moderne Lebenszyklusrichtlinie](https://support.microsoft.com/help/30881/modern-lifecycle-policy) für die Außerbetriebnahme.

## <a name="how-does-this-affect-me"></a>Welche Folgen hat das für mich? 

- Seit dem 28. Februar 2020 können Kunden, die im Februar 2020 keine IaaS-VMs über Azure Service Manager (ASM) verwendet haben, keine klassischen VMs mehr erstellen. 
- Ab dem 1. März 2023 können Kunden keine IaaS-VMs mehr über Azure Service Manager starten. VMs, die zu diesem Zeitpunkt ausgeführt werden oder zugeordnet sind, werden beendet, und die Zuordnung wird aufgehoben. 
- Am 1. März 2023 werden alle Abonnements, die noch nicht zu Azure Resource Manager migriert wurden, über unseren Zeitplan für das Löschen der verbleibenden klassischen VMs informiert.  

Die folgenden Azure-Dienste und -Funktionen sind von dieser Aussonderung **NICHT** betroffen: 
- Cloud Services 
- Speicherkonten, die **nicht** von klassischen VMs genutzt werden 
- Virtuelle Netzwerke (VNETs), die **nicht** von klassischen VMs genutzt werden 
- Sonstige klassische Ressourcen

## <a name="what-actions-should-i-take"></a>Welche Aktionen zieht das für mich nach sich? 

- Beginnen Sie noch heute mit der Planung Ihre Migration zu Azure Resource Manager. 

- Erstellen Sie eine Liste aller betroffenen VMs. 
    - Der VM-Typ „Virtuelle Computer (klassisch)“ auf dem [VM-Blatt des Azure-Portals](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ClassicCompute%2FVirtualMachines) umfasst alle betroffenen VMs innerhalb des Abonnements. 
    - Sie können Azure Resource Graph auch mit dem [Portal](https://portal.azure.com/#blade/HubsExtension/ArgQueryBlade/query/resources%0A%7C%20where%20type%20%3D%3D%20%22microsoft.classiccompute%2Fvirtualmachines%22) oder [PowerShell](https://docs.microsoft.com/azure/governance/resource-graph/concepts/work-with-data) abfragen, um alle mit „Virtuelle Computer (klassisch)“ gekennzeichneten VMs und deren zugehörige Informationen für die ausgewählten Abonnements anzuzeigen. 
    - Wir haben E-Mails mit der Liste aller Abonnements, die diese klassischen virtuellen Computer enthalten, an Abonnementbesitzer gesendet. Verwenden Sie sie zum Erstellen dieser Liste. 

- Weitere Informationen zur Migration Ihrer klassischen virtuellen [Linux](./linux/migration-classic-resource-manager-plan.md)- und [Windows](./windows/migration-classic-resource-manager-plan.md)-Computer zu Azure Resource Manager finden Sie [hier](./windows/migration-classic-resource-manager-overview.md).

- Zusätzliche Informationen finden Sie unter [Häufig gestellte Fragen zur Migration vom klassischen Bereitstellungsmodell zum Azure Resource Manager-Bereitstellungsmodell](./migration-classic-resource-manager-faq.md).

- Es wird empfohlen, die Planung mit dem [plattformgestützten Migrationstool](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview) beginnen, um Ihre vorhandenen VMs in drei einfachen Schritten zu migrieren: „Überprüfen, Vorbereiten und Committen“. 
    - Das Tool wurde entwickelt, um Ihre VMs mit minimaler bis gar keiner Ausfallzeit zu migrieren. 
    - Der erste Schritt „Überprüfen“ hat keine Auswirkungen auf Ihre vorhandene Bereitstellung und enthält eine Liste aller nicht unterstützten Szenarien für die Migration. 
    - Gehen Sie die [Liste der Problemumgehungen](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview#unsupported-features-and-configurations) durch, um Ihre Bereitstellung zu korrigieren und für die Migration vorzubereiten. 
    - Nachdem im Idealfall alle Überprüfungsfehler behoben wurden, sollten Sie während der Vorbereitungs- und Commitschritte auf keine Probleme stoßen. Nach dem erfolgreichen Commit wird eine Livemigration Ihrer Bereitstellung zu Azure Resource Manager durchgeführt. Anschließend kann sie mithilfe neuer APIs verwaltet werden, die von Azure Resource Manager bereitgestellt werden. 
    - Wenn dieses Tool für Ihre Migration nicht geeignet ist, können Sie sich [weitere Computeangebote](https://docs.microsoft.com/azure/architecture/guide/technology-choices/compute-decision-tree) für diese Migration ansehen. Da zahlreiche Azure-Computeangebote sich jeweils stark voneinander unterscheiden, ist es nicht möglich, einen plattformgestützten Migrationspfad für diese Angebote bereitzustellen.  
- Bei technischen Fragen und Problemen sowie zum Hinzufügen von Abonnements zur Zulassungsliste [wenden Sie sich an den Support](https://ms.portal.azure.com/#create/Microsoft.Support/Parameters/{"pesId":"6f16735c-b0ae-b275-ad3a-03479cfa1396","supportTopicId":"8a82f77d-c3ab-7b08-d915-776b4ff64ff4"}).

- Schließen Sie die Migration so bald wie möglich ab, um Beeinträchtigungen des Geschäftsbetriebs zu vermeiden und von der verbesserten Leistung und Sicherheit sowie den neuen Features von Azure Resource Manager zu profitieren. 

## <a name="what-resources-are-provided-to-me-for-this-migration"></a>Welche Ressourcen werden mir für diese Migration zur Verfügung gestellt?

- [Microsoft Q&A (Fragen und Antworten):](https://docs.microsoft.com/answers/topics/azure-virtual-machines-migration.html) Microsoft- und Communityunterstützung für die Migration

- [Azure-Migrationsunterstützung:](https://ms.portal.azure.com/#create/Microsoft.Support/Parameters/{"pesId":"6f16735c-b0ae-b275-ad3a-03479cfa1396","supportTopicId":"1135e3d0-20e2-aec5-4ef0-55fd3dae2d58"}) Spezielles Supportteam zur technischen Unterstützung während der Migration

- [Microsoft Fast Track:](https://www.microsoft.com/fasttrack) Das Microsoft Fast Track-Team kann berechtigten Kunden während der Migration technische Unterstützung bieten. 

- Wenn Ihr Unternehmen oder Ihre Organisation eine Partnerschaft mit Microsoft eingegangen ist und/oder mit Microsoft-Vertretern wie Cloud Solution Architects (CSAs) oder Technical Account Managers (TAMs) zusammenarbeitet, erhalten Sie von diesen zusätzliche Ressourcen für die Migration. 

