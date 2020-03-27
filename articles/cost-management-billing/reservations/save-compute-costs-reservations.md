---
title: Was sind Azure-Reservierungen?
description: Erfahren Sie mehr über Azure-Reservierungen und -Preise, um Kosten für virtuelle Computer, SQL-Datenbank-Instanzen, Azure Cosmos DB-Instanzen und andere Ressourcen zu sparen.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: banders
ms.openlocfilehash: 6277a7e7dc5891a3bc67c298a31344284c92e31d
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2020
ms.locfileid: "80235631"
---
# <a name="what-are-azure-reservations"></a>Was sind Azure-Reservierungen?

Mit Azure-Reservierungen können Sie Geld sparen, indem Sie sich bei mehreren Produkten für Pläne mit einer Laufzeit von einem Jahr oder drei Jahren entscheiden. Dadurch können Sie einen Rabatt für die von Ihnen genutzten Ressourcen in Anspruch nehmen. Reservierungen ermöglichen Kostensenkungen von bis zu 72 Prozent im Vergleich zur nutzungsbasierten Bezahlung. Reservierungen bieten einen Abrechnungsrabatt und wirken sich nicht auf den Laufzeitstatus Ihrer Ressourcen aus. Nach dem Kauf einer Reservierung gilt der Rabatt automatisch für die übereinstimmenden Ressourcen.

Sie können für eine Reservierung im Voraus oder monatlich bezahlen. Die Gesamtkosten für vorab bezahlte und monatliche Reservierungen sind gleich. Es fallen keine zusätzlichen Gebühren an, wenn Sie sich für die monatliche Zahlung entscheiden. Die monatliche Zahlung ist für Azure-Reservierungen verfügbar, nicht für Produkte von Drittanbietern.

Sie können eine Reservierung im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) erwerben.

## <a name="why-buy-a-reservation"></a>Warum eine Reservierung kaufen?

Bei einer konsistenten Ressourcennutzung, die Reservierungen unterstützt, können Sie durch den Erwerb einer Reservierung Ihre Kosten senken. Wenn Sie beispielsweise kontinuierlich Instanzen von einem Dienst ohne Reservierung ausführen, fallen die Gebühren für die nutzungsbasierte Bezahlung an. Wenn Sie eine Reservierung kaufen, erhalten Sie sofort den Reservierungsrabatt. Die Ressourcen werden nicht mehr mit den Gebühren für die nutzungsbasierte Bezahlung in Rechnung gestellt.

## <a name="how-reservation-discount-is-applied"></a>Wie der Reservierungsrabatt angewendet wird

Nach dem Kauf gilt der Reservierungsrabatt automatisch für die Ressourcennutzung, die den Attributen entspricht, die Sie beim Erwerb der Reservierung auswählen. Zu den Attributen zählen die SKU, Regionen (falls zutreffend) und der Bereich. Mit dem Reservierungsbereich wird ausgewählt, wo die Reservierungseinsparungen gelten.

Weitere Informationen zur Anwendung des Rabatts finden Sie unter [Anwendung des Rabatts für reservierte Instanzen](reservation-discount-application.md).

Weitere Informationen zur Funktionsweise des Reservierungsbereichs finden Sie unter [Bereichsreservierungen](prepare-buy-reservation.md#scope-reservations).


## <a name="flexibility-with-azure-reservations"></a>Flexibilität mit Azure-Reservierungen

Mit Azure-Reservierungen können Sie Ihre sich ändernden Anforderungen flexibel erfüllen. Sie können eine Reservierung gegen eine andere Reservierung des gleichen Typs umtauschen. Sie können auch eine Rückerstattung für eine Reservierung erhalten, wenn Sie sie nicht mehr benötigen (bis zu 50.000 US-Dollar in einem rollierenden Zeitfenster von zwölf Monaten). Das Maximum für die Rückerstattung gilt für alle Reservierungen im Rahmen Ihrer Vereinbarung mit Microsoft.

Weitere Informationen finden Sie unter [Self-Service-Umtausch und -Rückerstattungen für Azure-Reservierungen](exchange-and-refund-azure-reservations.md).


## <a name="charges-covered-by-reservation"></a>Gebühren, die durch Reservierung abgedeckt werden

- **Reservierte VM-Instanz**: Eine Reservierung deckt nur die Computekosten virtueller Computer ab. Eine Reservierung deckt keine zusätzlichen Kosten für Software, Windows, Netzwerke oder Speicher ab.
- **Reservierte Azure Storage-Kapazität**: Eine Reservierung deckt die Speicherkapazität für Storage Standard-Konten für Blobspeicher oder Azure Data Lake Gen2-Speicher ab. Die Reservierung deckt keine Bandbreite oder Transaktionsraten ab.
- **Reservierte Azure Cosmos DB-Kapazität**: Eine Reservierung deckt den für Ihre Ressourcen bereitgestellten Durchsatz ab. Die Speicher- und Netzwerkkosten werden nicht abgedeckt.
- **Reservierte virtuelle Kerne für SQL-Datenbank**: In einer Reservierung sind nur die Computekosten enthalten. Die SQL-Lizenz wird separat abgerechnet.
- **SQL Data Warehouse**: Eine Reservierung deckt die cDWU-Nutzung ab. Sie deckt keine mit der SQL Data Warehouse-Nutzung verbundenen Speicher- oder Netzwerkgebühren ab.
- **Azure Databricks**: Eine Reservierung deckt nur die DBU-Nutzung ab. Andere Gebühren (etwa Compute-, Speicher- und Netzwerkgebühren) werden separat angewendet.
- **App Service-Stempelgebühr**: Eine Reservierung deckt die Stempelnutzung ab. Sie gilt nicht für Worker. Jede andere dem Stempel zugeordnete Ressource wird also separat abgerechnet.
- **Azure Database for MySQL**: In einer Reservierung sind nur die Computekosten enthalten. Eine Reservierung deckt nicht die Software-, Netzwerk- oder Speichergebühren für den MySQL-Datenbankserver ab.
- **Azure Database for PostgreSQL**: In einer Reservierung sind nur die Computekosten enthalten. Eine Reservierung deckt nicht die Software-, Netzwerk- oder Speichergebühren für PostgreSQL-Datenbankserver ab.
- **Azure Database for MariaDB**: In einer Reservierung sind nur die Computekosten enthalten. Eine Reservierung deckt nicht die Software-, Netzwerk- oder Speichergebühren für den MariaDB-Datenbankserver ab.
- **Azure Data Explorer**: Eine Reservierung beinhaltet die Markupgebühren. Sie deckt keine mit den Clustern verbundenen Compute-, Netzwerk- oder Speichergebühren ab.
- **Azure Cache for Redis**: In einer Reservierung sind nur die Computekosten enthalten. Eine Reservierung deckt keine Netzwerk- oder Speichergebühren im Zusammenhang mit den Redis-Cache-Instanzen ab.
- **Azure Dedicated Host**: In Dedicated Host sind nur die Computekosten enthalten.
- **Azure Disk Storage-Reservierungen**: Eine Reservierung deckt nur SSD Premium-Datenträger der Größe P30 oder höher ab. Es werden keine anderen Datenträgertypen oder -größen abgedeckt, die kleiner als P30 sind.

Softwarepläne:

- **SUSE Linux**: Eine Reservierung deckt die Kosten für den Softwareplan ab. Die Rabatte gelten nur für SUSE-Verbrauchseinheiten und nicht für die Nutzung virtueller Computer.
- **Red Hat-Pläne**: Eine Reservierung deckt die Kosten für den Softwareplan ab. Die Rabatte gelten nur für RedHat-Verbrauchseinheiten und nicht für die Nutzung virtueller Computer.
- **Azure VMware Solution by CloudSimple**: Eine Reservierung deckt die VMWare CloudSimple-Knoten ab. Kosten für zusätzliche Softwarekosten fallen weiterhin an.
- **Azure Red Hat OpenShift**: Eine Reservierung gilt für die OpenShift-Kosten, nicht für die Azure-Infrastrukturkosten.

Bei virtuellen Windows-Computern und SQL-Datenbank wird der Reservierungsrabatt nicht auf die Softwarekosten angewendet. Die Lizenzkosten können Sie mit dem [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) abdecken.


## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:
    - [Verwalten von Azure-Reservierungen](manage-reserved-vm-instance.md)
    - [Grundlegendes zur Reservierungsnutzung bei einem Abonnement mit nutzungsbasierten Tarifen ](understand-reserved-instance-usage.md)
    - [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](understand-reserved-instance-usage-ea.md)
    - [Nicht in Azure-Reservierungen enthaltene Windows-Softwarekosten](reserved-instance-windows-software-costs.md)
    - [Verkaufen Microsoft Azure Reserved Instances](/partner-center/azure-reservations)

- Weitere Informationen zu Reservierungen für Diensttarife:
    - [Virtuelle Computer mit Azure Reserved VM Instances](../../virtual-machines/windows/prepay-reserved-vm-instances.md)
    - [Azure Cosmos DB-Ressourcen mit reservierter Azure Cosmos DB-Kapazität](../../cosmos-db/cosmos-db-reserved-capacity.md)
    - [SQL-Datenbank-Computeressourcen mit reservierter Azure SQL-Datenbank-Kapazität](../../sql-database/sql-database-reserved-capacity.md). Erfahren Sie mehr über Reservierungen für Softwarepläne:
    - [Red Hat-Softwarepläne aus Azure-Reservierungen](../../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [SUSE-Softwarepläne aus Azure-Reservierungen](../../virtual-machines/linux/prepay-suse-software-charges.md)
