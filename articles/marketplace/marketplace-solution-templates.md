---
title: Leitfaden für die Veröffentlichung von Lösungsvorlagenangeboten für Azure-Anwendungen – Azure Marketplace
description: In diesem Artikel werden die Anforderungen für das Veröffentlichen von Lösungsvorlagen im Azure Marketplace beschrieben.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: dsindona
ms.openlocfilehash: 4d395aa68c579b8e1271f4225ba57fa5f9932246
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2020
ms.locfileid: "85963783"
---
# <a name="publishing-guide-for-azure-applications-solution-template-offers"></a>Leitfaden für die Veröffentlichung von Lösungsvorlagenangeboten für Azure-Anwendungen

In diesem Artikel werden die Anforderungen für das Veröffentlichen von Lösungsvorlagenangeboten erläutert. Hierbei handelt es sich um eine Möglichkeit, Azure-Anwendungsangebote im Azure Marketplace zu veröffentlichen. Für den Angebotstyp „Lösungsvorlage“ ist eine [Azure Resource Manager-Vorlage (ARM-Vorlage](../azure-resource-manager/templates/overview.md)) erforderlich, um Ihre Lösungsinfrastruktur automatisch bereitzustellen.

Verwenden Sie den Angebotstyp *Lösungsvorlage* für Azure-Anwendungen unter den folgenden Bedingungen:

- Ihre Lösung erfordert eine zusätzliche Automatisierung der Bereitstellung und Konfiguration über einen einzelnen virtuellen Computer (Virtual Machine, VM) hinaus, z. B. eine Kombination aus VMs, Netzwerk- und Speicherressourcen.
- Ihre Kunden werden die Lösung selbst verwalten.

Einem Kunden wird für diesen Angebotstyp der Aktionsaufruf *Jetzt kaufen* angezeigt.

## <a name="requirements-for-solution-template-offers"></a>Anforderungen für Lösungsvorlagenangebote

| **Anforderungen** | **Details**  |
| ---------------  | -----------  |
|Abrechnung und Messung    |  Lösungsvorlagenangebote sind keine Transaktionsangebote. Sie können jedoch zum Bereitstellen bezahlter VM-Angebote verwendet werden, die über den kommerziellen Microsoft-Marketplace abgerechnet werden. Die über die ARM-Vorlage der Lösung bereitgestellten Ressourcen werden im Azure-Abonnement des Kunden eingerichtet. Bei virtuellen Computern mit nutzungsbasierter Bezahlung erfolgt die Transaktion mit dem Kunden über Microsoft, und die Abrechnung wird über das Azure-Abonnement des Kunden abgewickelt.<br/> Bei der Abrechnung nach dem Modell „Bring Your Own License“ (BYOL) rechnet Microsoft die angefallenen Infrastrukturkosten im Kundenabonnement ab, während Sie Ihre Softwarelizenzgebühren direkt mit dem Kunden abrechnen.   |
|Azure-kompatible virtuelle Festplatte (VHD)  |   VMs müssen unter Windows oder Linux erstellt werden. Weitere Informationen finden Sie unter <ul> <li>[Erstellen eines Azure-Anwendungsangebots](./partner-center-portal/create-new-azure-apps-offer.md) (für Windows-VHDs).</li><li>[Von Azure unterstützte Distributionen von Linux](../virtual-machines/linux/endorsed-distros.md) (für Linux-VHDs)</li></ul> |
| Zuordnen der Nutzung durch Kunden | Die Zuordnung der Nutzung durch Kunden muss für alle Lösungsvorlagen aktiviert werden, die im Azure Marketplace veröffentlicht werden. Weitere Informationen zur Zuordnung der Nutzung durch Kunden sowie zu deren Aktivierung finden Sie unter [Zuordnung der Nutzung durch Kunden von Azure-Partnern](./azure-partner-customer-usage-attribution.md).  |
| Verwenden von verwalteten Datenträgern | [Verwaltete Datenträger](../virtual-machines/windows/managed-disks-overview.md) sind die Standardoption für persistente Datenträger von Infrastructure-as-a-Service-VMs (IaaS-VMs) in Azure. In Lösungsvorlagen müssen Sie verwaltete Datenträger verwenden. <ul><li>Wenn Sie Ihre Lösungsvorlagen aktualisieren möchten, folgen Sie den Anweisungen unter [Verwenden verwalteter Datenträger mit Resource Manager-Vorlagen](../virtual-machines/windows/using-managed-disks-template-deployments.md), und verwenden Sie die bereitgestellten [Beispiele](https://github.com/Azure/azure-quickstart-templates).<br><br> </li><li>Wenn Sie die VHD als Image im Azure Marketplace veröffentlichen möchten, importieren Sie die zugrunde liegende VHD der verwalteten Datenträger in ein Speicherkonto. Verwenden Sie dazu eine der folgenden Methoden:<ul><li>[Azure PowerShell](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd.md) </li> <li> [Die Azure-CLI](../virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd.md) </li> </ul></ul> |

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich (falls noch nicht geschehen), wie Sie [Ihr Cloudgeschäft mit dem Azure Marketplace ausweiten](https://azuremarketplace.microsoft.com/sell).

So registrieren Sie sich in Partner Center und beginnen mit der Arbeit

- [Melden Sie sich bei Partner Center an](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership), um Ihr Angebot zu erstellen oder abzuschließen.
- Weitere Informationen finden Sie unter [Erstellen eines Azure-Anwendungsangebots](./partner-center-portal/create-new-azure-apps-offer.md).
