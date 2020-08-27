---
title: Anzeigen von Transaktionen für Azure-Reservierungseinkäufe und -erstattungen
description: Erfahren Sie, wie Sie Transaktionen für Azure-Reservierungseinkäufe und -erstattungen anzeigen können.
author: yashesvi
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: how-to
ms.date: 07/24/2020
ms.author: banders
ms.openlocfilehash: d9e5269468f7cd4571e7ae686af7f1ef159b4ef3
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88681701"
---
# <a name="view-reservation-purchase-and-refund-transactions"></a>Anzeigen von Transaktionen für Reservierungseinkäufe und -erstattungen

Es gibt verschiedene Möglichkeiten zum Anzeigen von Transaktionen für Reservierungseinkäufe und -erstattungen. Sie können das Azure-Portal, Power BI und REST-APIs verwenden.

## <a name="view-reservation-transactions-in-the-azure-portal"></a>Anzeigen von Reservierungstransaktionen im Azure-Portal

Ein Rechnungsadministrator für die Unternehmensregistrierung oder Microsoft-Kundenvereinbarung kann Reservierungstransaktionen unter der Kostenverwaltung und Abrechnung anzeigen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.
1. Wählen Sie **Reservierungstransaktionen** aus.
1. Wählen Sie zum Filtern der Ergebnisse **Zeitraum**, **Typ** oder **Beschreibung** aus.
1. Wählen Sie **Übernehmen**.

[![Screenshot: Anzeigen von Reservierungstransaktionen im Azure-Portal](./media/view-purchase-refunds/azure-portal-reservation-transactions.png)](./media/view-purchase-refunds/azure-portal-reservation-transactions.png#lightbox)

## <a name="view-reservation-transactions-in-power-bi"></a>Anzeigen von Reservierungstransaktionen in Power BI

Ein Rechnungsadministrator für die Unternehmensregistrierung oder Microsoft-Kundenvereinbarung kann Reservierungstransaktionen mit der Cost Management-Power BI-App anzeigen.

1. Rufen Sie die [Cost Management-Power BI-App](https://appsource.microsoft.com/product/power-bi/costmanagement.azurecostmanagementapp) ab.
1. Navigieren Sie zum Bericht für RI-Käufe.

[![Beispiel mit Reservierungstransaktionen](./media/view-purchase-refunds/power-bi-reservation-transactions.png)](./media/view-purchase-refunds/power-bi-reservation-transactions.png#lightbox)

Weitere Informationen finden Sie unter [Azure Cost Management-Power BI-App für Enterprise Agreements](https://docs.microsoft.com/azure/cost-management-billing/costs/analyze-cost-data-azure-cost-management-power-bi-template-app).

## <a name="use-apis-to-get-reservation-transactions"></a>Verwenden von APIs zum Abrufen von Reservierungstransaktionen

Benutzer mit Enterprise Agreement (EA) und Microsoft-Kundenvereinbarung können Daten zu Reservierungstransaktionen mithilfe der [API für Reservierungstransaktionen – Liste](https://docs.microsoft.com/rest/api/consumption/reservationtransactions/list) abrufen.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Verwalten einer Reservierung finden Sie unter [Verwalten von Azure-Reservierungen](manage-reserved-vm-instance.md).
- Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:
  - [Was sind Azure-Reservierungen?](save-compute-costs-reservations.md)
  - [Verwalten von Reservierungen für Ressourcen in Azure](manage-reserved-vm-instance.md)
