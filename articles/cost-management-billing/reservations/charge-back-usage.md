---
title: Rückbuchen von Azure-Reservierungskosten
description: Erfahren Sie, wie Sie Azure-Reservierungskosten für die Rückbuchung anzeigen.
author: yashesvi
ms.service: cost-management-billing
ms.topic: how-to
ms.date: 07/24/2020
ms.author: banders
ms.openlocfilehash: 04e4e8dfce8549494eb9d2984d6307c18b88dc73
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87287330"
---
# <a name="charge-back-azure-reservation-costs"></a>Rückbuchen von Azure-Reservierungskosten

Abrechnungsleser für Enterprise Agreement und Microsoft-Kundenvereinbarung können amortisierte Kostendaten für Reservierungen anzeigen. Sie können die Kostendaten verwenden, um den Geldwert für ein Abonnement, eine Ressourcengruppe, eine Ressource oder ein Tag an ihre Partner zurückzubuchen. Bei amortisierten Daten entspricht der effektive Preis den anteiligen Reservierungskosten pro Stunde. Die Kosten sind die Gesamtkosten der Reservierungsnutzung durch die Ressource an diesem Tag.

Benutzer mit einem einzelnen Abonnement können die amortisierten Kostendaten aus ihrer Nutzungsdatei abrufen. Wenn eine Ressource einen Reservierungsrabatt erhält, enthält der Abschnitt *AdditionalInfo* in der Nutzungsdatei die Reservierungsdetails. Weitere Informationen finden Sie unter [Herunterladen der Nutzung aus dem Azure-Portal](https://docs.microsoft.com/azure/cost-management-billing/understand/download-azure-daily-usage#download-usage-from-the-azure-portal-csv).

## <a name="get-reservation-charge-back-data-for-chargeback"></a>Abrufen von Daten zur Rückbuchung von Reservierungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu **Kostenverwaltung + Abrechnung**.
1. Wählen Sie unter **Tatsächliche Kosten** die Metrik **Amortisierte Kosten** aus.
1. Um zu sehen, welche Ressourcen von einer Reservierung verwendet wurden, wenden Sie einen Filter für **Reservierung** an, und wählen Sie dann Reservierungen aus.
1. Legen Sie die **Granularität** auf **Monatlich** oder **Täglich** fest.
1. Legen Sie den Diagrammtyp auf **Tabelle** fest.
1. Legen Sie die Option **Gruppieren nach** auf **Ressource** fest.

[![Beispiel für Ressourcenkosten einer Reservierung, die Sie für Rückbuchungen verwenden können](./media/charge-back-usage/amortized-reservation-costs.png)](./media/charge-back-usage/amortized-reservation-costs.png#lightbox)

Im folgenden Video wird gezeigt, wie Sie Kosten für die Reservierungsnutzung im Azure-Portal anzeigen können.

 > [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4sQOw] 

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Verwalten einer Reservierung finden Sie unter [Verwalten von Azure-Reservierungen](manage-reserved-vm-instance.md).
- Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:
  - [Was sind Azure-Reservierungen?](save-compute-costs-reservations.md)
  - [Verwalten von Reservierungen für Ressourcen in Azure](manage-reserved-vm-instance.md)
