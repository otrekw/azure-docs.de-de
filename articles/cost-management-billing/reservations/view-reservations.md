---
title: Anzeigen von Reservierungen für Azure-Ressourcen
description: Erfahren Sie, wie Sie Azure-Reservierungen im Azure-Portal anzeigen. Informieren Sie sich über Reservierungen und Nutzung mithilfe von APIs, PowerShell, der CLI und von Power BI.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: how-to
ms.date: 02/24/2021
ms.author: banders
ms.openlocfilehash: 477dff9db28672f8231710af34786ac387f43b71
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102050340"
---
# <a name="view-azure-reservations"></a>Anzeigen von Azure-Reservierungen

In diesem Artikel wird erläutert, wie Sie Azure-Reservierungen im Azure-Portal anzeigen. Sie können eine erworbene Reservierung im Azure-Portal anzeigen und verwalten.

## <a name="who-can-manage-a-reservation-by-default"></a>Wer kann eine Reservierung standardmäßig verwalten?

Die folgenden Benutzer können Reservierungen standardmäßig anzeigen und verwalten:

- Die Person, die eine Reservierung erwirbt, und der Kontoadministrator des Abrechnungsabonnements, mit dem die Reservierung erworben wird, werden der Reservierungsreihenfolge hinzugefügt.
- Abrechnungsadministratoren für Konzernvertrag (Enterprise Agreement, EA) und Microsoft-Kundenvereinbarung.

Damit andere Personen Reservierungen verwalten können, haben Sie zwei Optionen:

- Delegieren der Zugriffsverwaltung für eine individuelle Reservierungsreihenfolge:
    1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
    1. Wählen Sie **Alle Dienste** > **Reservierungen** aus, um Reservierungen aufzulisten, auf die Sie Zugriff haben.
    1. Wählen Sie die Reservierung aus, für die Sie den Zugriff an andere Benutzer delegieren möchten.
    1. Wählen Sie unter „Reservierungsdetails“ die gewünschte Reservierungsreihenfolge aus.
    1. Wählen Sie die Option **Zugriffssteuerung (IAM)** aus.
    1. Wählen Sie **Rollenzuweisung hinzufügen** > **Rolle** > **Besitzer** aus. Wenn Sie eingeschränkten Zugriff gewähren möchten, wählen Sie eine andere Rolle aus.
    1. Geben Sie die E-Mail-Adresse des Benutzers ein, den Sie als Besitzer hinzufügen möchten.
    1. Wählen Sie den Benutzer und dann **Speichern** aus.

- Hinzufügen eines Benutzers als Abrechnungsadministrator zu einem Konzernvertrag oder einer Microsoft-Kundenvereinbarung:
    - Fügen Sie bei einem Konzernvertrag Benutzer mit der Rolle _Unternehmensadministrator_ hinzu, um alle für diesen Vertrag geltenden Reservierungsaufträge anzuzeigen und zu verwalten. Benutzer mit der Rolle _Unternehmensadministrator (schreibgeschützt)_ können die Reservierung nur anzeigen. Abteilungsadministratoren und Kontobesitzer können Reservierungen nicht anzeigen, _außer_ wenn sie ihnen mithilfe der Zugriffssteuerung (Access Control, IAM) explizit hinzugefügt werden. Weitere Informationen finden Sie unter [Verwalten von Azure Enterprise-Rollen](../manage/understand-ea-roles.md).

        _Unternehmensadministratoren können den Besitz einer Reservierungsreihenfolge übernehmen und andere Benutzer mithilfe der Zugriffssteuerung (Identity & Access Management, IAM) zu einer Reservierung hinzufügen._
    - Bei einer Microsoft-Kundenvereinbarung können Benutzer mit der Rolle „Besitzer des Abrechnungsprofils“ oder „Mitwirkender am Abrechnungsprofil“ alle mit dem Abrechnungsprofil getätigten Reservierungseinkäufe verwalten. Benutzer mit Leseberechtigung für das Abrechnungsprofil und Rechnungs-Manager können alle Reservierungen anzeigen, die für das Abrechnungsprofil bezahlt werden. Sie können allerdings keine Änderungen an den Reservierungen vornehmen.
    Weitere Informationen finden Sie unter [Rollen und Aufgaben für ein Abrechnungsprofil](../manage/understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="how-billing-administrators-view-or-manage-reservations"></a>Anzeigen oder Verwalten von Reservierungen durch Abrechnungsadministratoren

Wenn Sie über Azure RBAC Zugriff auf Reservierungen oder Reservierungsaufträge haben, wird unter Umständen nur eine Teilmenge der Reservierungstransaktionen oder keine Reservierungstransaktion angezeigt, wenn Sie zu Reservierungen navigieren. Führen Sie die folgenden Schritte aus, um alle Reservierungen und Reservierungstransaktionen anzuzeigen und zu verwalten:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu **Kostenverwaltung + Abrechnung**.
    - Wenn Sie EA-Administrator sind, wählen Sie im Menü auf der linken Seite die Option **Abrechnungsbereiche** und dann in der Liste der Abrechnungsbereiche einen Bereich aus.
    - Wenn Sie Besitzer eines Abrechnungsprofils für eine Microsoft-Kundenvereinbarung sind, wählen Sie im linken Menü **Abrechnungsprofile** aus. Wählen Sie in der Liste der Abrechnungsprofile ein Profil aus.
1. Wählen Sie im Menü auf der linken Seite die Option **Reservierungstransaktionen** aus. Die Liste der Reservierungstransaktionen wird angezeigt.
1. Am oberen Seitenrand wird ein Banner mit folgendem Hinweis angezeigt: *Abrechnungsadministratoren können jetzt Reservierungen verwalten. Klicken Sie hier, um Reservierungen anzuzeigen und zu verwalten.* Wählen Sie das Banner aus.
1. Die vollständige Liste der Reservierungen für Ihr EA-Registrierungsprofil oder -Abrechnungsprofil wird angezeigt.
1. Wenn Sie den Besitz einer Reservierung übernehmen möchten, wählen Sie die entsprechende Reservierung aus. Wählen Sie dann auf der Seite „Einrichten von Berechtigungen“ die Option **Zugriff gewähren** aus. Sie erhalten Besitzerzugriff auf die Reservierung und den Reservierungsauftrag.

## <a name="view-reservation-and-utilization-in-the-azure-portal"></a>Anzeigen der Reservierung und Nutzung im Azure-Portal

So zeigen Sie eine Reservierung als Besitzer oder Leser an:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Navigieren Sie zu [Reservierungen](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).
3. Diese Liste zeigt alle Reservierungen, für die Sie die Rolle „Besitzer“ oder „Leser“ ausüben. Jede Reservierung zeigt den letzten bekannten Nutzungsprozentsatz.
4. Wählen Sie den Nutzungsprozentsatz aus, um den Verlauf und Details der Nutzung anzuzeigen. Das folgende Video enthält ausführliche Informationen.
   > [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4sYwk] 

## <a name="get-reservations-and-utilization-using-apis-powershell-and-cli"></a>Abrufen der Reservierungen und Nutzung mithilfe von APIs, PowerShell und der CLI

Abrufen einer Liste aller Reservierungen mithilfe der folgenden Ressourcen:

- [API: Reservierungsauftrag: Liste](/rest/api/reserved-vm-instances/reservationorder/list)
- [PowerShell: Reservierungsauftrag: Liste](/powershell/module/azurerm.reservations/get-azurermreservationorder)
- [CLI: Reservierungsauftrag: Liste](/cli/azure/reservations/reservation-order#az-reservations-reservation-order-list)

Sie können die [Reservierungsnutzung](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) auch mithilfe der API für die Nutzung reservierter Instanzen abrufen. 

## <a name="see-reservations-and-utilization-in-power-bi"></a>Anzeigen der Reservierungen und Nutzung in Power BI

Es gibt zwei Optionen für Power BI-Benutzer:
- Inhaltspaket: Reservierungskaufdatum und Nutzungsdaten sind im [Consumption Insights Power BI-Inhaltspaket](/power-bi/desktop-connect-azure-cost-management) verfügbar. Erstellen Sie die gewünschten Berichte mithilfe des Inhaltspakets. 
- Cost Management-App: Verwenden Sie die [Cost Management-App](https://appsource.microsoft.com/product/power-bi/costmanagement.azurecostmanagementapp) für vorgefertigte Berichte, die Sie weiter anpassen können.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Azure-Reservierungen](manage-reserved-vm-instance.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](understand-reserved-instance-usage.md).
- [Grundlegendes zur Nutzung von Azure-Reservierungen für die Enterprise-Registrierung](understand-reserved-instance-usage-ea.md).
- [Grundlegendes zur Nutzung von Azure-Reservierungen für CSP-Abonnements](/partner-center/azure-reservations).

