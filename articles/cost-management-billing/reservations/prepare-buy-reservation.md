---
title: Vorbereiten des Kaufs einer Azure-Reservierung
description: Informieren Sie sich über wichtige Aspekte, bevor Sie eine Azure-Reservierung erwerben.
author: bandersmsft
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: banders
ms.openlocfilehash: 1f5ca2d43356eab98cffe8414c00d97e5744739a
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2020
ms.locfileid: "80235649"
---
# <a name="prepare-to-buy-a-reservation"></a>Vorbereiten des Kaufs einer Reservierung

Mit Azure-Reservierungen können Sie Geld sparen, indem Sie sich bei zahlreichen Azure-Ressourcen für Pläne mit einer Laufzeit von einem Jahr oder drei Jahren entscheiden. Bevor Sie eine Verpflichtung zum Kauf einer Reservierung eingehen, lesen Sie unbedingt die folgenden Abschnitte, um den Kauf vorzubereiten.

## <a name="who-can-buy-a-reservation"></a>Wer kann eine Reservierung kaufen?

Um einen Plan zu kaufen, müssen Sie eine Rolle als Abonnementverantwortlicher bei einem Enterprise-Abonnement (MS-AZR-0017P oder MS-AZR-0148P) oder einem Abonnement mit nutzungsbasierter Zahlung (MS-AZR-0003P oder MS-AZR-0023P) oder einem Abonnement im Rahmen der Microsoft-Kundenvereinbarung haben. Cloudlösungsanbieter können Azure-Reservierungen über das Azure-Portal oder  [Partner Center](/partner-center/azure-reservations)  erwerben.

EA-Kunden (Enterprise Agreement) können Käufe für EA-Administratoren beschränken, indem sie die Option **Reservierte Instanzen hinzufügen** im EA-Portal deaktivieren. EA-Administratoren müssen der Besitzer für mindestens ein EA-Abonnement sein, um eine Reservierung zu erwerben. Die Option ist nützlich für Unternehmen, die ein zentralisiertes Team benötigen, um Reservierungen für verschiedene Kostenstellen einzukaufen. Nach dem Kauf können zentralisierte Teams die Reservierungen um Kostenstellenbesitzer erweitern. Die Besitzer können dann die Reservierung auf ihre Abonnements ausdehnen. Das zentrale Team muss keinen Zugriff des Abonnenten haben, wenn die Reservierung gekauft wird.

Ein Reservierungsrabatt gilt nur für Ressourcen in Verbindung mit Abonnements, die über Enterprise, einen Cloud Solution Provider (CSP), eine Microsoft-Kundenvereinbarung und individuelle Pläne mit nutzungsbasierten Tarifen erworben wurden.

## <a name="scope-reservations"></a>Bereichsreservierungen

Sie können den Bereich für eine Reservierung auf ein Abonnement oder eine Ressourcengruppe festlegen. Wenn Sie den Bereich für eine Reservierung festlegen, wird ausgewählt, wo die Reservierungseinsparungen gelten. Wenn Sie den Bereich der Reservierung auf eine Ressourcengruppe festlegen, gelten die Reservierungsrabatte nur für die Ressourcengruppe und nicht für das gesamte Abonnement.

### <a name="reservation-scoping-options"></a>Optionen für Reservierungsbereiche

Ihnen stehen je nach Bedarf drei Optionen zur Verfügung, mit denen Sie den Bereich einer Reservierung definieren können:

- **Einzelne Ressourcengruppe**: Wendet den Reservierungsrabatt nur auf die entsprechenden Ressourcen in der ausgewählten Ressourcengruppe an.
- **Einzelabonnement**: Wendet den Reservierungsrabatt auf die entsprechenden Ressourcen im ausgewählten Abonnement an.
- **Gemeinsam genutzt**: Wendet den Reservierungsrabatt auf die entsprechenden Ressourcen in berechtigten Abonnements innerhalb des Abrechnungskontexts an. Für Kunden mit einem Enterprise Agreement ist der Abrechnungskontext die Registrierung. Für Kunden im Rahmen der Microsoft-Kundenvereinbarung ist der Abrechnungsbereich das Abrechnungsprofil. Für Kunden mit individuellen Abonnements mit nutzungsbasierten Tarifen handelt es sich beim Abrechnungsbereich um alle berechtigten Abonnements, die vom Kontoadministrator erstellt wurden.

Während Reservierungsrabatte auf Ihre Nutzung angewendet werden, verarbeitet Azure die Reservierung in der folgenden Reihenfolge:

1. Reservierungen mit einem Bereich für eine Ressourcengruppe
2. Reservierungen mit einem einzelnen Bereich
3. Reservierungen mit einem gemeinsam genutzten Bereich

Eine einzelne Ressourcengruppe kann abhängig davon, wie Sie Ihre Reservierungen festlegen, Reservierungsrabatte von mehreren Reservierungen erhalten.

Sie können den Bereich nach dem Erwerb einer Reservierung immer aktualisieren. Navigieren Sie zu diesem Zweck zu der Reservierung, klicken Sie auf **Konfiguration**, und legen Sie den Bereich für die Reservierung erneut fest. Das Neuzuweisen eines Bereichs für eine Reservierung ist keine kommerzielle Transaktion. Die Reservierungsbedingungen ändern sich nicht. Weitere Informationen zum Aktualisieren des Bereichs finden Sie unter [Aktualisieren des Bereichs nach dem Erwerb einer Reservierung](manage-reserved-vm-instance.md#change-the-reservation-scope).

![Beispiel für eine Änderung des Reservierungsbereichs](./media/prepare-buy-reservation/rescope-reservation-resource-group.png)

## <a name="purchase-reservations"></a>Erwerben von Reservierungen

Sie können Reservierungen über das Azure-Portal, APIs, PowerShell und die CLI erwerben. Lesen Sie die folgenden Artikel, wenn Sie Reservierungen kaufen möchten:

- [App Service](prepay-app-service-isolated-stamp.md)
- [Azure Cache for Redis](../../azure-cache-for-redis/cache-reserved-pricing.md)
- [Cosmos DB](../../cosmos-db/cosmos-db-reserved-capacity.md)
- [Databricks](prepay-databricks-reserved-capacity.md)
- [Data Explorer](../../data-explorer/pricing-reserved-capacity.md)
- [Disk Storage](../../virtual-machines/linux/disks-reserved-capacity.md)
- [Dedicated Host](../../virtual-machines/prepay-dedicated-hosts-reserved-instances.md)
- [Softwarepläne](../../virtual-machines/linux/prepay-suse-software-charges.md)
- [Storage](../../storage/blobs/storage-blob-reserved-capacity.md)
- [SQL-Datenbank](../../sql-database/sql-database-reserved-capacity.md)
- [SQL Data Warehouse](prepay-sql-data-warehouse-charges.md)
- [Virtuelle Computer](../../virtual-machines/windows/prepay-reserved-vm-instances.md)

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Reservierungen für Azure-Ressourcen](manage-reserved-vm-instance.md)
