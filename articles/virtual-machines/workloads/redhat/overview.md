---
title: Red Hat-Workloads in Azure – Übersicht | Microsoft-Dokumentation
description: Informationen über die in Azure verfügbaren Red Hat-Produktangebote
services: virtual-machines-linux
author: asinn826
manager: borisb2015
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: overview
ms.date: 02/10/2020
ms.author: alsin
ms.openlocfilehash: daba49e6861eb67fd07c6fcf618b2b2d6cdd8c89
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77133821"
---
# <a name="red-hat-workloads-on-azure"></a>Red Hat-Workloads in Azure
Red Hat-Workloads werden in einer Reihe von Angeboten in Azure unterstützt. Red Hat Enterprise Linux-Images (RHEL) gehören ebenso wie die Red Hat Update Infrastructure (RHUI) zum Kern von RHEL-Workloads.

## <a name="red-hat-enterprise-linux-rhel-images"></a>RHEL-Images (Red Hat Enterprise Linux)
Azure bietet eine umfassende Palette an RHEL-Images. Diese Images werden über zwei verschiedene Lizenzierungsmodelle zur Verfügung gestellt: nutzungsbasierte Bezahlung (Pay-As-You-Go, PAYG) und Nutzung eigener Abonnements (Bring-Your-Own-Subscription, BYOS). Neue RHEL-Images werden bei Erscheinen neuer RHEL-Versionen in Azure veröffentlicht und während ihres gesamten Lebenszyklus bei Bedarf aktualisiert.

### <a name="pay-as-you-go-payg-images"></a>PAYG-Images (Pay-As-You-Go, nutzungsbasierte Bezahlung)
Azure bietet eine Vielzahl von RHEL-PAYG-Images. Diese Images umfassen die Berechtigung zur ordnungsgemäßen Nutzung von RHEL und sind einer Updatequelle (Red Hat Update Infrastructure) zugeordnet. Für diese Images wird eine Premiumgebühr für die RHEL-Berechtigung und -Updates berechnet. Folgende RHEL-PAYG-Imagevarianten sind verfügbar:
* RHEL Standard
* RHEL für SAP
* RHEL für SAP mit Hochverfügbarkeits- und Updatediensten

Die Verwendung von PAYG-Images empfiehlt sich, wenn Sie sich keine Gedanken um die separate Bezahlung der entsprechenden Anzahl von Abonnements machen möchten.

### <a name="red-hat-gold-images-rhel-byos"></a>Red Hat Gold-Images (`rhel-byos`)
Azure bietet auch Red Hat Gold-Images an. Diese Images eignen sich für Kunden, die bereits über Red Hat-Abonnements verfügen und diese in Azure verwenden möchten. Sie müssen Ihre vorhandenen Red Hat-Abonnements für Red Hat Cloud Access aktivieren, bevor Sie sie in Azure nutzen können. Der Zugriff auf diese Images wird automatisch gewährt, wenn Ihre Red Hat-Abonnements für Cloud Access aktiviert sind und die Qualifizierungsanforderungen erfüllen. Mit diesen Images können Kunden eine doppelte Fakturierung vermeiden, die bei PAYG-Images auftreten kann.
* [Lesen Sie, wie Sie Ihre Red Hat-Abonnements für Cloud Access mit Azure aktivieren.](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/con-enable-subs)
* [Erfahren Sie, wie Sie Red Hat Gold-Images über das Azure-Portal, die CLI oder ein PowerShell-Cmdlet suchen.](./byos.md)

> [!NOTE]
> Hinweis zur doppelten Fakturierung: Als doppelte Fakturierung wird eine Situation bezeichnet, in der ein Benutzer zweimal für RHEL-Abonnements bezahlt. Diese Situation tritt gewöhnlich dann auf, wenn ein Kunde „subscription-manager“ verwendet, um eine Berechtigung an eine RHEL-PAYG-VM anzufügen. Wenn ein Kunde beispielsweise „subscription-manager“ verwendet, um eine Berechtigung für SAP-Pakete an ein RHEL-PAYG-Image anzufügen, wird dieser Kunde indirekt doppelt belastet, weil er zweimal für RHEL bezahlt: einmal über die PAYG-Premiumgebühr und einmal über sein SAP-Abonnement. Dies passiert bei BYOS-Benutzern nicht.

### <a name="generation-2-images"></a>Images der Generation 2
Virtuelle Computer der 2. Generation bieten im Vergleich zu virtuellen Computer der 1. Generation einige neuere Features. Details finden Sie unter [Unterstützung für VMs der Generation 2 in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/generation-2). Der Hauptunterschied aus der Perspektive eines RHEL-Images besteht darin, dass VMs der Generation 2 eine UEFI statt einer BIOS-Firmwareschnittstelle verwenden und beim Start eine GUID-Partitionstabelle (GPT) anstelle eines Master Boot Record (MBR) nutzen. Dadurch können Betriebssystemdatenträger u. a. größer als 2 TB sein. Darüber hinaus werden [virtuelle Computer der Mv2-Serie](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory#mv2-series) nur mit Images der Generation 2 ausgeführt.

RHEL-Images der Generation 2 sind im Marketplace verfügbar. Suchen Sie in der Image-SKU nach „Gen2“, wenn Sie alle Images mit der Azure CLI auflisten. Navigieren Sie beim Bereitstellen des virtuellen Computers zur Registerkarte „Erweitert“, um einen virtuellen Computer der Generation 2 bereitzustellen.

## <a name="red-hat-update-infrastructure-rhui"></a>Red Hat Update Infrastructure (RHUI)
Azure stellt die Red Hat Update Infrastructure nur für PAYG-RHEL-VMs bereit. Die RHUI ist im Grunde ein Spiegel der Red Hat-CDNs, es kann aber nur über die Azure-PAYG-RHEL-VMs darauf zugegriffen werden. Je nachdem, welches RHEL-Image Sie bereitgestellt haben, erhalten Sie Zugriff auf die geeigneten Pakete. Ein RHEL für SAP-Image beispielsweise erhält neben den grundlegenden RHEL-Paketen auch Zugriff auf die SAP-Pakete.

### <a name="rhui-update-behavior"></a>Updateverhalten der RHUI
RHEL-Images, die mit der RHUI verbunden sind, werden standardmäßig auf die neueste Nebenversion von RHEL aktualisiert, wenn ein `yum update`-Vorgang ausgeführt wird. Das bedeutet, dass eine VM mit RHEL 7.4 auf RHEL 7.7 aktualisiert wird, wenn ein `yum update`-Vorgang auf der VM ausgeführt wird. Dies ist ein beabsichtigtes Verhalten. Dieses Upgradeverhalten kann jedoch eingeschränkt werden, indem Benutzer von regulären RHEL-Repositorys zu [EUS-Repositorys (Extended Update Support)](./redhat-rhui.md#rhel-eus-and-version-locking-rhel-vms) wechseln.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [RHEL-Images in Azure](./redhat-images.md).
* Erfahren Sie mehr über die [Red Hat Update Infrastructure](./redhat-rhui.md).
* Erfahren Sie mehr über das [Angebot für Red Hat Gold-Images (`rhel-byos`)](./byos.md).
