---
title: Leitfaden für die Veröffentlichung von Lösungsvorlagenangeboten für Azure-Anwendungen | Azure Marketplace
description: In diesem Artikel werden die Anforderungen für die Veröffentlichung von Lösungsvorlagen im Azure Marketplace beschrieben.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/22/2020
ms.author: dsindona
ms.openlocfilehash: 4cce509b8f4db7549a05eccc3127d2c437f9c037
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82594575"
---
# <a name="azure-applications-solution-template-offer-publishing-requirements"></a>Azure-Anwendungen: Anforderungen für die Veröffentlichung von Lösungsvorlagenangeboten

In diesem Artikel werden die Anforderungen für den Angebotstyp „Lösungsvorlage“ erläutert. Hierbei handelt es sich um eine Möglichkeit, ein Azure-Anwendungsangebot im Azure Marketplace zu veröffentlichen. Für den Angebotstyp „Lösungsvorlage“ ist eine [Azure Resource Manager-Vorlage (ARM-Vorlage](../azure-resource-manager/templates/overview.md)) erforderlich, um Ihre Lösungsinfrastruktur automatisch bereitzustellen.

Verwenden Sie den Angebotstyp „Lösungsvorlage“ für Azure-Anwendungen, wenn die folgenden Bedingungen erforderlich sind:

- Ihre Lösung erfordert eine zusätzliche Automatisierung von Bereitstellung und Konfiguration über eine einzelne VM hinaus, z. B. eine Kombination aus VMs, ein Netzwerk und Speicherressourcen.
- Ihr Kunde wird die Lösung selbst verwalten.

Den Benutzern wird für diesen Angebotstyp der Aktionsaufruf „Jetzt kaufen“ angezeigt.

## <a name="requirements-for-solution-template-offers"></a>Anforderungen für Lösungsvorlagenangebote

| **Anforderungen** | **Details**  |
| ---------------  | -----------  |
|Abrechnung und Messung    |  Lösungsvorlagenangebote sind keine Transaktionsangebote, sondern können zur Bereitstellung bezahlter VM-Angebote verwendet werden, die über den kommerziellen Microsoft-Marketplace abgerechnet werden. Die Ressourcen, die mit der ARM-Vorlage der Lösung bereitgestellt werden, stehen dann im Azure-Abonnement des Kunden zur Verfügung. Für virtuelle Computer mit nutzungsbasierter Bezahlung (Pay-As-You-Go, PAYGO) wird die Transaktion mit dem Kunden über Microsoft und die Abrechnung über das Azure-Abonnement des Kunden abgewickelt.<br/> Im Falle von Bring-Your-Own-License (BYOL) rechnet Microsoft die im Kundenabonnement angefallenen Infrastrukturkosten ab, während Sie Ihre Softwarelizenzgebühren direkt mit dem Kunden abrechnen.   |
|Azure-kompatible virtuelle Festplatte (VHD)  |   VMs müssen unter Windows oder Linux erstellt werden. Weitere Informationen finden Sie unter <ul> <li>[Erstellen eines Azure-Anwendungsangebots](./partner-center-portal/create-new-azure-apps-offer.md) (für Windows-VHDs)</li><li>[Von Azure unterstützte Distributionen von Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) (für Linux-VHDs)</li></ul> |
| Zuordnen der Nutzung durch Kunden | Die Zuordnung der Nutzung durch Kunden muss für alle Lösungsvorlagen aktiviert werden, die über den Azure Marketplace veröffentlicht werden. Weitere Informationen zur Zuordnung der Nutzung durch Kunden sowie zu deren Aktivierung finden Sie unter [Zuordnen der Nutzung durch Kunden von Azure-Partnern](./azure-partner-customer-usage-attribution.md).  |
| Verwenden von verwalteten Datenträgern | Die Standardoption für persistierte Datenträger virtueller IaaS-Computer in Azure lautet [Managed Disks](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview). In Lösungsvorlagen muss Managed Disks verwendet werden. <br> <br> 1. Halten Sie sich an die [Anweisungen](https://docs.microsoft.com/azure/virtual-machines/windows/using-managed-disks-template-deployments) und [Beispiele](https://github.com/Azure/azure-quickstart-templates) für die Verwendung von Managed Disks in den Azure ARM-Vorlagen, um Ihre Lösungsvorlagen zu aktualisieren. <br> <br> 2. Befolgen Sie die nachfolgenden Anweisungen, um die zugrunde liegende VHD von Managed Disks in ein Speicherkonto zu importieren und die VHD als Image im Marketplace zu veröffentlichen: <br> <ul> <li> [PowerShell](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd?toc=%2fpowershell%2fmodule%2ftoc.json) </li> <li> [BEFEHLSZEILENSCHNITTSTELLE (CLI)](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd?toc=%2fcli%2fmodule%2ftoc.json) </li> </ul> |

## <a name="next-steps"></a>Nächste Schritte

- [Informieren Sie sich](https://azuremarketplace.microsoft.com/sell) über den Azure Marketplace, sofern noch nicht geschehen.
- [Melden Sie sich bei Partner Center an](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership), um Ihr Angebot zu erstellen oder abzuschließen.
- Weitere Informationen finden Sie unter [Erstellen eines Azure-Anwendungsangebots](./partner-center-portal/create-new-azure-apps-offer.md).
