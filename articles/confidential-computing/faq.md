---
title: Häufig gestellte Fragen zum Azure Confidential Computing
description: Hier finden Sie Antworten auf häufig gestellte Fragen zum Azure Confidential Computing.
author: JBCook
ms.topic: troubleshooting
ms.workload: infrastructure
ms.service: virtual-machines
ms.subservice: workloads
ms.date: 4/17/2020
ms.author: jencook
ms.openlocfilehash: e9cb691ef60f612672078a9ef84db904c79cbc87
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82189444"
---
# <a name="frequently-asked-questions-for-azure-confidential-computing"></a>Häufig gestellte Fragen zum Azure Confidential Computing

Dieser Artikel bietet Antworten auf einige der häufigsten Fragen zur Ausführung von [Confidential Computing-Workloads in Azure](overview.md).

Suchen Sie in den Azure-Foren bei [MSDN und Stack Overflow](https://azure.microsoft.com/support/forums/), falls Sie Ihr Azure-Problem mit diesem Artikel nicht beheben konnten. Sie können Ihr Problem in diesen Foren veröffentlichen oder [auf Twitter an @AzureSupport senden](https://twitter.com/AzureSupport). Sie können auch eine Azure-Supportanfrage senden. Um eine Supportanfrage zu senden, wählen Sie auf der [Azure-Supportseite](https://azure.microsoft.com/support/options/) die Option „Support erhalten“ aus.

## <a name="confidential-computing-virtual-machines"></a>Virtuelle Computer für Confidential Computing <a id="vm-faq"></a>

1. **Wie können Sie mit der Bereitstellung virtueller Computer der DCsv2-Serie beginnen?**

   Im Folgenden finden Sie einige Möglichkeiten zum Bereitstellen einer DCsv2-VM:
   - Verwenden einer [Azure Resource Manager-Vorlage](../virtual-machines/windows/template-description.md)
   - Im [Azure-Portal](https://portal.azure.com/#create/hub)
   - In der Marketplace-Lösungsvorlage [Azure Confidential Compute](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.acc-virtual-machine-v2?tab=overview). Mithilfe der Marketplace-Lösungsvorlage können Kunden auf unterstützte Szenarien (Regionen, Images, Verfügbarkeit, Datenträgerverschlüsselung) beschränkt werden. 

1. **Funktionieren alle Betriebssystemimages mit Azure Confidential Computing?**

   Nein. Die virtuellen Computer können nur auf virtuellen Computern der Generation 2 bereitgestellt werden. Wir bieten Unterstützung für die Generation 2 für Ubuntu Server 18.04, Ubuntu Server 16.04 und Windows Server 2016 Datacenter an. Weitere Informationen zu virtuellen Computern der Generation 2 finden Sie unter [Linux-](../virtual-machines/linux/generation-2.md) und [Windows](../virtual-machines/windows/generation-2.md).

1. **DCsv2-VMs sind im Portal abgeblendet, und ich kann keine auswählen**

   Basierend auf der Informationssprechblase neben der VM können verschiedene Maßnahmen ergriffen werden:
    -   **UnsupportedGeneration**: Ändern Sie die Generation des virtuellen Computerimages in „Gen2“.
    -   **NotAvailableForSubscription**: Die Region ist für Ihr Abonnement noch nicht verfügbar. Wählen Sie eine verfügbare Region aus.
    -   **InsufficientQuota**: [Erstellen Sie eine Supportanfrage zur Erhöhung Ihres Kontingents](../azure-portal/supportability/per-vm-quota-requests.md). Kostenlose Testabonnements verfügen über kein Kontingent für Confidential Computing-VMs. 

1. **DCsv2-VMs werden nicht angezeigt, wenn ich nach ihnen in der Größenauswahl des Portals suche**

   Stellen Sie sicher, dass Sie eine verfügbare Region ausgewählt haben. Stellen Sie außerdem sicher, dass Sie in der Größenauswahl „Alle Filter löschen“ ausgewählt haben. 

1. **Worin besteht der Unterschied zwischen VMs der DCsv2-Serie und der DC-Serie?**

   VMs der DC-Serie werden auf älteren Intel-Prozessoren mit 6 Kernen mit Intel SGX ausgeführt. Diese verfügen über weniger Gesamtarbeitsspeicher, weniger EPC-Speicher (Enclave Page Cage) und sind in weniger Regionen verfügbar. Diese VMs sind nur in den Regionen „USA, Osten“ und „Europa, Westen“ in zwei Größen verfügbar: „Standard_DC2s“ und „Standard_DC4s“. Sie werden nicht allgemein verfügbar sein und können nur in der Marketplaceinstanz der [Confidential Compute-VM der DC-Serie [Vorschau]](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.confidentialcompute?tab=Overview) bereitgestellt werden.

1. **Sind DCsv2-VMs global verfügbar?**

   Nein, diese virtuellen Computer sind nur in ausgewählten Regionen verfügbar. Überprüfen Sie die Seite [Produkte nach Regionen](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) auf die neuesten verfügbaren Regionen. 

1. **Wie installiert man das Open Enclave SDK?**
   
   Anweisungen zum Installieren des OE SDK auf einem Computer, ob in Azure oder lokal, finden Sie in den Anleitungen im [Open Enclave SDK GitHub](https://github.com/openenclave/openenclave).
     
   Sie können auch zum Open Enclave SDK GitHub navigieren, um betriebssystemspezifische Installationsanweisungen zu erhalten:
     - [Installieren des OE SDK unter Windows](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Windows.md)
     - [Installieren des OE SDK unter Ubuntu 18.04](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Ubuntu_18.04.md)
     - [Installieren des OE SDK unter Ubuntu 16.04](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Ubuntu_16.04.md)
