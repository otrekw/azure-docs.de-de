---
title: Vorteile der Migration der klassischen Bereitstellung in Azure AD Domain Services | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Vorteile der Migration von der klassischen Bereitstellung in Azure Active Directory Domain Services zum Resource Manager-Bereitstellungsmodell
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: iainfou
ms.openlocfilehash: 29d7772ef688134e8fc9009ec32cffacc3f23ca7
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84014450"
---
# <a name="benefits-of-migration-from-the-classic-to-resource-manager-deployment-model-in-azure-active-directory-domain-services"></a>Vorteile der Migration vom klassischen Bereitstellungsmodell in Azure Active Directory Domain Services zum Resource Manager-Bereitstellungsmodell

In Azure Active Directory Domain Services (AD DS) können Sie eine vorhandene verwaltete Domäne, die das klassische Bereitstellungsmodell verwendet, zum Resource Manager-Bereitstellungsmodell migrieren. Von Azure AD DS verwaltete Domänen, die das Resource Manager-Bereitstellungsmodell verwenden, bieten zusätzliche Features wie eine differenzierte Kennwortrichtlinie, Überwachungsprotokolle und einen Kontosperrschutz.

In diesem Artikel werden die Vorteile der Migration beschrieben. Lesen Sie zum Einstieg [Migrieren von Azure AD Domain Services vom klassischen VNET-Modell zu Resource Manager][howto-migrate].

> [!NOTE]
> Seit 2017 können Azure AD Domain Services in einem Azure Resource Manager-Netzwerk gehostet werden. Seitdem konnten wir mithilfe der modernen Möglichkeiten von Azure Resource Manager einen sichereren Dienst schaffen. Da Azure Resource Manager-Bereitstellungen klassische Bereitstellungen vollständig ersetzen, werden die klassischen Bereitstellungen virtueller Netzwerke mit Azure AD DS am 1. März 2023 eingestellt.
>
> Weitere Informationen finden Sie im [offiziellen Hinweis zur Einstellung](https://azure.microsoft.com/updates/we-are-retiring-azure-ad-domain-services-classic-vnet-support-on-march-1-2023/).

## <a name="migration-benefits"></a>Vorteile der Migration

Beim Migrationsvorgang wird eine vorhandene Azure AD DS-Instanz, die das klassische Bereitstellungsmodell verwendet, auf die Verwendung des Resource Manager-Bereitstellungsmodells umgestellt. Mit der Migration einer verwalteten Azure AD DS-Domäne vom klassischen zum Resource Manager-Bereitstellungsmodell vermeiden Sie die Notwendigkeit, Computer erneut in die verwaltete Domäne einzubinden oder die Azure AD DS-Instanz zu löschen und ganz neu zu erstellen. Nach Abschluss des Migrationsvorgangs bleiben virtuelle Computer in die verwaltete Azure AD DS-Domäne eingebunden.

Nach der Migration stellt Azure AD DS viele Features bereit, die nur für Domänen verfügbar sind, die das Resource Manager-Bereitstellungsmodell verwenden. Zu den Features zählen:

* [Unterstützung einer differenzierten Kennwortrichtlinie][password-policy]
* Höhere Synchronisierungsgeschwindigkeiten zwischen Azure AD und Azure AD Domain Services
* Zwei neue [Attribute, die von Azure AD synchronisiert werden][attributes] - *manager* und *employeeID*
* Zugriff auf leistungsstarke Domänencontroller, wenn Sie [ein SKU-Upgrade][skus] durchführen
* Schutz vor AD-Kontosperrung
* [E-Mail-Benachrichtigungen für Warnungen in Ihrer verwalteten Domäne][email-alerts]
* [Verwenden von Azure-Arbeitsmappen und Azure Monitor zum Anzeigen von Überwachungsprotokollen und Anmeldeaktivitäten][workbooks]
* [Azure-Verfügbarkeitszonen][availability-zones] in unterstützten Regionen
* Integration mit anderen Azure-Produkten wie [Azure Files][azure-files], [HD Insights][hd-insights] und [Windows Virtual Desktop][wvd]
* Zugriff des Supports auf weitere Telemetriedaten und effektivere Problembehebung
* Verschlüsselung ruhender Daten mit [Azure Managed Disks][managed-disks] für die Daten auf den verwalteten Domänencontrollern

Mit verwalteten Azure AD DS-Domänen, die ein Resource Manager-Bereitstellungsmodell verwenden, bleiben Sie in Bezug auf brandneue Features stets auf dem Laufenden. Für verwaltete Azure AD DS-Domänen mit dem klassischen Bereitstellungsmodell sind keine neuen Features verfügbar.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie zum Einstieg [Migrieren von Azure AD Domain Services vom klassischen VNET-Modell zu Resource Manager][howto-migrate].

<!-- LINKS - INTERNAL -->
[password-policy]: password-policy.md
[skus]: change-sku.md
[email-alerts]: notifications.md
[workbooks]: use-azure-monitor-workbooks.md
[azure-files]: ../storage/files/storage-files-identity-auth-active-directory-domain-service-enable.md
[hd-insights]: ../hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds.md
[wvd]: ../virtual-desktop/overview.md
[availability-zones]: ../availability-zones/az-overview.md
[howto-migrate]: migrate-from-classic-vnet.md
[attributes]: synchronization.md#attribute-synchronization-and-mapping-to-azure-ad-ds
[managed-disks]: ../virtual-machines/windows/managed-disks-overview.md
