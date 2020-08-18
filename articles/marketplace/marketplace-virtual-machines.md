---
title: Leitfaden für die Veröffentlichung von Virtual Machine-Angeboten im Azure Marketplace
description: In diesem Artikel werden die Anforderungen zum Veröffentlichen eines virtuellen Computers (Virtual Machine, VM) und einer kostenlosen Softwaretestversion beschrieben, die über den Azure Marketplace bereitgestellt werden.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 04/15/2020
ms.openlocfilehash: 9f408174b9a8dff925a9c834ea75bf725ec6f913
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87837343"
---
# <a name="publishing-guide-for-virtual-machine-offers"></a>Leitfaden für die Veröffentlichung von Virtual Machine-Angeboten

Das Veröffentlichen von VM-Images ist eine der wichtigsten Möglichkeiten, eine Lösung im Azure Marketplace zu veröffentlichen. Lesen Sie diesen Leitfaden, um die Anforderungen für diesen Angebotstyp zu verstehen. 

Virtual Machine-Angebote sind Transaktionsangebote, die über den Azure Marketplace bereitgestellt und abgerechnet werden. Den Benutzern wird der Aktionsaufruf *Jetzt kaufen* angezeigt.

## <a name="free-trial"></a>Kostenlose Testversion 

Bei Verwendung des Abrechnungsmodells „Bring Your Own License“ (BYOL) können Sie Benutzern das Testen Ihres Angebots mit befristeten Softwarelizenzen ermöglichen. 

## <a name="test-drive"></a>Testversion

Sie können einen oder mehrere virtuelle Computer über Infrastructure-as-a-Service- (IaaS-) oder Software-as-a-Service-Apps (SaaS-Apps) bereitstellen. Ein Vorteil der Veröffentlichungsoption *Testversion* ist die automatisierte Einrichtung eines virtuellen Computers oder einer gesamten Lösung mit einer von einem Partner gehosteten geführten Tour. Mit einer Testversion können Ihre Kunden VMs ohne zusätzliche Kosten evaluieren. Ein Kunde muss kein bestehender Azure-Kunde sein, um die Testversion nutzen zu können. 

Weitere Informationen zu Testversionen finden Sie unter [Was ist eine Testversion?](what-is-test-drive.md)

|Requirements (Anforderungen)  |Details |
|---------|---------|
| Sie verfügen über eine Azure Marketplace-App.   |  Mindestens eine VM über IaaS oder SaaS.      |

## <a name="interactive-demo"></a>Interaktive Demo

Mit diesem Angebot bieten Sie Ihren Kunden anhand einer interaktiven Demo eine Einführung in Ihre Lösung. Der Vorteil der Veröffentlichungsoption einer interaktiven Demo liegt darin, dass Sie eine Testversion anbieten können, ohne eine komplizierte Einrichtung Ihrer komplexen Lösung bereitstellen zu müssen. 

## <a name="virtual-machine-offer"></a>VM-Angebot

Verwenden Sie den Angebotstyp *Virtueller Computer*, wenn Sie ein virtuelles Gerät für das Ihrem Kunden zugeordnete Abonnement bereitstellen. VMs können vollständig kommerziell genutzt werden, wobei dafür das Modell der nutzungsbasierten Bezahlung oder das Bring-Your-Own-Licence-Lizenzierungsmodell (BYOL) verwendet wird. Microsoft hostet die kommerzielle Transaktion und führt für Sie die Abrechnung mit Ihrem Kunden durch. Sie haben den Vorteil, dass Sie die bevorzugte Zahlungsmethoden zwischen Ihrem Kunden und Microsoft, einschließlich des Enterprise Agreements, nutzen können.

> [!NOTE]
> Zum jetzigen Zeitpunkt kann der mit einem Enterprise Agreement verbundene Mindestverbrauch für die Azure-Nutzung Ihres virtuellen Computers genutzt, aber nicht auf Ihre Softwarelizenzgebühren angewendet werden.  
> 
> [!NOTE]
> Sie können die Ermittlung und Bereitstellung Ihres virtuellen Computers auf eine bestimmte Gruppe von Kunden beschränken, indem Sie das Image und die Preise als privates Angebot veröffentlichen. Durch private Angebote haben Sie die Möglichkeit, exklusive Angebote für Ihre wertvollsten Kunden zu erstellen und benutzerdefinierte Software und Bestimmungen anzubieten. Benutzerdefinierte Bestimmungen ermöglichen Ihnen die Schaffung vielfältiger Szenarios, wie etwa vom Vertrieb erzielte Geschäftsabschlüsse mit Sonderpreisen und -bestimmungen sowie ein frühzeitiger Zugang zu Software, die in limitierter Anzahl veröffentlicht wird. Durch private Angebote können Sie einer begrenzten Anzahl von Kunden Sonderpreise oder bestimmte Produkte anbieten, indem Sie einen neuen Plan mit diesen Konditionen erstellen.  
>
> Weitere Informationen finden Sie unter [Private Angebote im Azure Marketplace](https://azure.microsoft.com/blog/private-offers-on-azure-marketplace).  

| Anforderung | Details |  
|:--- |:--- | 
| Abrechnung und Messung | Ihr virtueller Computer muss entweder die Abrechnung nach dem BYOL-Modell oder die monatliche nutzungsbasierte Bezahlung unterstützen. |  
| Azure-kompatible virtuelle Festplatte (VHD) | VMs müssen unter Windows oder Linux erstellt werden. Weitere Informationen zum Erstellen einer VHD finden Sie unter: <ul> <li>[Von Azure unterstützte Distributionen von Linux](../virtual-machines/linux/endorsed-distros.md) (für Linux-VHDs)</li> <li>[Erstellen einer Azure-kompatiblen VHD](./partner-center-portal/azure-vm-create-offer.md) (für Windows-VHDs).</li> </ul> |  

>[!Note]
>Die Option zur Nutzung des CSP-Partnerkanals (Cloud Solution Provider) ist jetzt verfügbar. Weitere Informationen zum Vermarkten Ihres Angebots über Microsoft CSP-Partnerkanäle finden Sie unter [Cloud Solution Provider](./cloud-solution-providers.md).

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich (falls noch nicht geschehen), wie Sie [Ihr Cloudgeschäft mit dem Azure Marketplace ausweiten](https://azuremarketplace.microsoft.com/sell).

So registrieren Sie sich in Partner Center und beginnen mit der Arbeit

- [Melden Sie sich bei Partner Center an](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership), um Ihr Angebot zu erstellen oder abzuschließen.
- Weitere Informationen finden Sie unter [Erstellen eines VM-Angebots](./partner-center-portal/azure-vm-create-offer.md).
