---
title: Anzeigen von Reservierungen für Azure-Ressourcen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure-Reservierungen im Azure-Portal anzeigen.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: how-to
ms.date: 07/24/2020
ms.author: banders
ms.openlocfilehash: fd5d4248e9b1e645628ac8b87df3b4b0a16e2049
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288966"
---
# <a name="view-azure-reservations"></a>Anzeigen von Azure-Reservierungen

Sie können die erworbene Reservierung im Azure-Portal anzeigen und verwalten.

## <a name="permissions-to-view-a-reservation"></a>Berechtigungen zum Anzeigen einer Reservierung

Zum Anzeigen oder Verwalten einer Reservierung benötigen Sie die Berechtigung „Leser“ oder „Besitzer“ dafür. Beim Kauf einer Reservierung wird Ihnen und dem Kontoadministrator automatisch die Rolle „Besitzer“ für den Reservierungsauftrag und die Reservierung zugewiesen. Damit andere Personen die Reservierung anzeigen können, müssen Sie sie als **Besitzer** oder **Leser** für den Reservierungsauftrag und die Reservierung hinzufügen. Beim Hinzufügen eines Benutzers zu dem Abonnement, das für die Abrechnung der Reservierung bereitgestellt wird, wird der Benutzer nicht automatisch zur Reservierung hinzugefügt. 

Weitere Informationen finden Sie unter [Hinzufügen oder Ändern von Benutzern, die eine Reservierung verwalten können](manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).

## <a name="view-reservation-and-utilization-in-the-azure-portal"></a>Anzeigen der Reservierung und Nutzung im Azure-Portal

So zeigen Sie eine Reservierung als Besitzer oder Leser an:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Navigieren Sie zu [Reservierungen](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).
3. Die Liste zeigt alle Reservierungen, für die Sie die Rolle „Besitzer“ oder „Leser“ ausüben. Jede Reservierung zeigt den letzten bekannten Nutzungsprozentsatz.
4. Klicken Sie auf den Nutzungsprozentsatz, um den Verlauf und Details der Nutzung anzuzeigen. Das folgende Video enthält ausführliche Informationen.
   > [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4sYwk] 

## <a name="get-reservations-and-utilization-using-apis-powershell-cli"></a>Abrufen der Reservierungen und Nutzung mithilfe von APIs, PowerShell und der CLI

Abrufen einer Liste aller Reservierungen mithilfe der folgenden Ressourcen
- [API: Reservierungsauftrag: Liste](/rest/api/reserved-vm-instances/reservationorder/list)
- [PowerShell: Reservierungsauftrag: Liste](/powershell/module/azurerm.reservations/get-azurermreservationorder)
- [CLI: Reservierungsauftrag: Liste](/cli/azure/reservations/reservation-order#az-reservations-reservation-order-list)

Sie können die [Reservierungsnutzung](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) auch mithilfe der API für die Nutzung reservierter Instanzen abrufen. 

## <a name="see-reservations-and-utilization-in-power-bi"></a>Anzeigen der Reservierungen und Nutzung in Power BI

Es gibt zwei Optionen für Power BI-Benutzer:
- Inhaltspaket: Reservierungskäufe und Nutzungsdaten sind im [Consumption Insights Power BI-Inhaltspaket](/power-bi/desktop-connect-azure-cost-management) verfügbar. Erstellen Sie die gewünschten Berichte mit diesem Inhaltspaket. 
- Cost Management-App: Verwenden Sie die [Cost Management-App](https://appsource.microsoft.com/product/power-bi/costmanagement.azurecostmanagementapp) für vorgefertigte Berichte, die Sie weiter anpassen können.

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Azure-Reservierungen](manage-reserved-vm-instance.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](understand-reserved-instance-usage.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](understand-reserved-instance-usage-ea.md)
- [Grundlegendes zur Verwendung von Azure-Reservierungen für CSP-Abonnements](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).
