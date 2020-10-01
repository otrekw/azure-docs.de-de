---
title: Azure-VMs (klassisch) werden ab dem 1. März 2023 nicht mehr unterstützt.
description: Dieser Artikel bietet eine allgemeine Übersicht über die Außerbetriebnahme von virtuellen Computern (VMs), die mit dem klassischen Bereitstellungsmodell erstellt wurden.
author: tanmaygore
manager: vashan
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: tagore
ms.openlocfilehash: 7f2db507176d65e7794607e83db8605b2f892c1c
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89646638"
---
# <a name="migrate-your-iaas-resources-to-azure-resource-manager-by-march-1-2023"></a>Migrieren Ihrer IaaS-Ressourcen zu Azure Resource Manager vor dem 1. März 2023 

Im Jahr 2014 haben wir mit Infrastructure-as-a-Service (IaaS) auf [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/) begonnen. Die zugehörigen Funktionen wurden seitdem ständig erweitert. Da Azure Resource Manager jetzt über vollständige IaaS-Funktionen und andere Erweiterungen verfügt, haben wir am 28. Februar 2020 begonnen, die Verwaltung von IaaS-VMs über [Azure Service Manager](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-faq#what-is-azure-service-manager-and-what-does-it-mean-by-classic) (ASM) einzustellen. Diese Funktionalität wird am 1. März 2023 vollständig eingestellt. 

Derzeit wird für etwa 90 Prozent der IaaS-VMs Azure Resource Manager verwendet. Wenn Sie IaaS-Ressourcen über ASM verwenden, sollten Sie jetzt mit der Planung der Migration beginnen. Schließen Sie die Planung bis zum 1. März 2023 ab, um [Azure Resource Manager](../azure-resource-manager/management/index.yml) zu nutzen.

VMs, die mit dem klassischen Bereitstellungs Modell erstellt wurden, werden gemäß der [Modern Lifecycle-Richtlinie](https://support.microsoft.com/help/30881/modern-lifecycle-policy) außer Betrieb genommen.

## <a name="how-does-this-affect-me"></a>Wie wirkt sich das auf mich aus? 

- Ab dem 28. Februar 2020 können Kunden, die im Monat Februar 2020 keine IaaS-VMs über ASM verwendet haben, keine VMs (klassisch) mehr erstellen. 
- Ab dem 1. März 2023 können Kunden IaaS-VMs nicht mehr über ASM starten. Alle VMs, die noch ausgeführt werden oder zugeordnet sind, werden beendet, und deren Zuordnungen werden aufgehoben. 
- Am 1. März 2023 werden alle Abonnements, die noch nicht zu Azure Resource Manager migriert wurden, über unseren Zeitplan für das Löschen der verbleibenden VMs (klassisch) informiert.  

Diese Außerbetriebnahme wirkt sich *nicht* auf die folgenden Azure-Dienste und -Funktionen aus: 
- Azure Cloud Services 
- Speicherkonten, die *nicht* von VMs (klassisch) genutzt werden 
- Virtuelle Netzwerke, die *nicht* von VMs (klassisch) genutzt werden 
- Sonstige klassische Ressourcen

## <a name="what-actions-should-i-take"></a>Welche Aktionen zieht das für mich nach sich? 

Beginnen Sie noch heute mit der Planung Ihre Migration zu Azure Resource Manager. 

1. Erstellen Sie eine Liste aller betroffenen VMs: 

   - Die VMs mit dem Typ **Virtuelle Computer (klassisch)** im [VM-Bereich des Azure-Portals](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ClassicCompute%2FVirtualMachines) entsprechen allen betroffenen VMs innerhalb des Abonnements. 
   - Sie können Azure Resource Graph auch über das [Portal](https://portal.azure.com/#blade/HubsExtension/ArgQueryBlade/query/resources%0A%7C%20where%20type%20%3D%3D%20%22microsoft.classiccompute%2Fvirtualmachines%22) oder [PowerShell](https://docs.microsoft.com/azure/governance/resource-graph/concepts/work-with-data) abfragen, um die Liste aller mit „Virtueller Computer (klassisch)“ gekennzeichneten VMs und deren zugehörige Informationen für die ausgewählten Abonnements anzuzeigen. 
   - Am 8. Februar und am 2. September 2020 haben wir E-Mails an Abonnementbesitzer mit einer Liste aller Abonnements gesendet, die diese VMs (klassisch) enthalten. Verwenden Sie sie zum Erstellen dieser Liste. 

1. Weitere Informationen zur Migration Ihrer [Linux](./linux/migration-classic-resource-manager-plan.md)- und [Windows](./windows/migration-classic-resource-manager-plan.md)-VMs (klassisch) zu Azure Resource Manager finden Sie [hier](./windows/migration-classic-resource-manager-overview.md). Weitere Informationen finden Sie unter [Häufig gestellte Fragen zur Migration vom klassischen Bereitstellungsmodell zum Azure Resource Manager-Bereitstellungsmodell](./migration-classic-resource-manager-faq.md).

1. Es empfiehlt sich, die Planung mit dem [plattformgestützten Migrationstool](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview) zu beginnen, um Ihre vorhandenen VMs in drei einfachen Schritten zu migrieren: Überprüfen, Vorbereiten und Committen. Das Tool wurde entwickelt, um Ihre VMs mit minimaler bis gar keiner Ausfallzeit zu migrieren. 

   1. Der erste Schritt, Überprüfen, hat keine Auswirkungen auf Ihre vorhandene Bereitstellung und enthält eine Liste aller nicht unterstützten Szenarien für die Migration. 
   1. Gehen Sie die [Liste der Problemumgehungen](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview#unsupported-features-and-configurations) durch, um Ihre Bereitstellung zu korrigieren und für die Migration vorzubereiten. 
   1. Nachdem im Idealfall alle Überprüfungsfehler behoben wurden, sollten Sie während der Vorbereitungs- und Commitschritte auf keine Probleme stoßen. Nach dem erfolgreichen Commit wird eine Livemigration Ihrer Bereitstellung zu Azure Resource Manager durchgeführt. Die Bereitstellung kann dann über neue APIs verwaltet werden, die von Azure Resource Manager bereitgestellt werden. 

   Wenn das Migrationstool nicht für Ihre Migration geeignet ist, können Sie sich [weitere Computeangebote](https://docs.microsoft.com/azure/architecture/guide/technology-choices/compute-decision-tree) für die Migration ansehen. Da es zahlreiche Azure-Computeangebote gibt und diese sich voneinander unterscheiden, ist es nicht möglich, einen plattformgestützten Migrationspfad für sie bereitzustellen.  

1. Bei technischen Fragen und Problemen sowie für Unterstützung beim Hinzufügen von Abonnements zur Zulassungsliste [wenden Sie sich an den Support](https://ms.portal.azure.com/#create/Microsoft.Support/Parameters/{"pesId":"6f16735c-b0ae-b275-ad3a-03479cfa1396","supportTopicId":"8a82f77d-c3ab-7b08-d915-776b4ff64ff4"}).

1. Schließen Sie die Migration so bald wie möglich ab, um Beeinträchtigungen Ihres Geschäftsbetriebs zu vermeiden und von der verbesserten Leistung und Sicherheit sowie den neuen Features von Azure Resource Manager zu profitieren. 

## <a name="what-resources-are-available-for-this-migration"></a>Welche Ressourcen sind für diese Migration verfügbar?

- [Microsoft Q&A (Fragen und Antworten):](https://docs.microsoft.com/answers/topics/azure-virtual-machines-migration.html) Microsoft- und Communityunterstützung für die Migration.

- [Azure-Migrationsunterstützung:](https://ms.portal.azure.com/#create/Microsoft.Support/Parameters/{"pesId":"6f16735c-b0ae-b275-ad3a-03479cfa1396","supportTopicId":"1135e3d0-20e2-aec5-4ef0-55fd3dae2d58"}) Spezielles Supportteam zur technischen Unterstützung während der Migration.

- [Microsoft Fast Track:](https://www.microsoft.com/fasttrack) Das Team kann berechtigten Kunden während der Migration technische Unterstützung bieten. 

Wenn Ihr Unternehmen oder Ihre Organisation eine Partnerschaft mit Microsoft eingegangen ist oder mit Microsoft-Vertretern (etwa Cloud Solution Architects (CSAs) oder Technical Account Managers (TAMs)) zusammenarbeitet, erhalten Sie von diesen zusätzliche Ressourcen für die Migration. 

