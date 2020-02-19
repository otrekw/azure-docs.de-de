---
title: Azure-Ausgabenlimit | Microsoft-Dokumentation
description: In diesem Artikel werden die Funktionsweise und das Entfernen des Azure-Ausgabenlimits beschrieben.
author: bandersmsft
manager: judupont
tags: billing
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: banders
ms.openlocfilehash: bde3c142fa0f4f69948a9ff1df61d77f06d2b430
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77188269"
---
# <a name="azure-spending-limit"></a>Azure-Ausgabenlimit

Mit dem Ausgabenlimit in Azure wird verhindert, dass Sie Ihr Guthaben überschreiten. Für alle neuen Kunden, die sich für ein kostenloses Azure-Konto oder für Abonnementtypen registrieren, die Guthaben für mehrere Monate beinhalten, ist das Ausgabenlimit standardmäßig aktiviert. Das Ausgabenlimit entspricht dem Guthaben und kann nicht geändert werden. Wenn Sie sich also beispielsweise für ein kostenloses Azure-Konto registriert haben, liegt Ihr Ausgabenlimit bei 200 USD und kann nicht auf 500 USD angehoben werden. Sie können das Ausgabenlimit jedoch entfernen. Sie haben also die Wahl zwischen keinem Limit oder einem Limit in Höhe des Guthabens. Dadurch werden die meisten Arten von Ausgaben unterbunden. Für Abonnements mit Verpflichtungsplänen oder mit nutzungsbasierter Bezahlung ist kein Ausgabenlimit verfügbar. Die vollständige Liste mit Azure-Abonnementtypen sowie Informationen zur Verfügbarkeit des Ausgabenlimits finden Sie [hier](https://azure.microsoft.com/support/legal/offer-details/).

## <a name="reaching-a-spending-limit"></a>Erreichen eines Ausgabenlimits

Wenn durch Ihre Nutzung Gebühren anfallen, die über Ihr Ausgabenlimit hinausgehen, werden die von Ihnen bereitgestellten Dienste für den restlichen Abrechnungszeitraum deaktiviert.

Wenn Sie also beispielsweise das gesamte Guthaben Ihres kostenlosen Azure-Kontos verbrauchen, werden die von Ihnen bereitgestellten Azure-Ressourcen aus der Produktion entfernt. Außerdem werden Ihre virtuellen Azure-Computer beendet, und deren Zuordnung wird aufgehoben. Die Daten in Ihren Speicherkonten sind in diesem Fall nur schreibgeschützt verfügbar.

Falls Ihr Abonnementtyp Guthaben für mehrere Monate umfasst, wird Ihr Abonnement zu Beginn des nächsten Abrechnungszeitraums automatisch reaktiviert. Sie können dann Ihre Azure-Ressourcen erneut bereitstellen und haben Vollzugriff auf Ihre Speicherkonten und Datenbanken.

Bei Erreichen des Ausgabenlimits sendet Azure entsprechende E-Mail-Benachrichtigungen. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) an, um Benachrichtigungen zu Abonnements anzuzeigen, deren Ausgabenlimit erreicht wurde.

Wenn Sie sich für ein kostenloses Azure-Konto registriert haben und das Ausgabenlimit erreichen, können Sie auf die [nutzungsbasierte Bezahlung](upgrade-azure-subscription.md) upgraden, um das Ausgabenlimit zu entfernen und Ihr Abonnement automatisch zu reaktivieren.

## <a name="remove-the-spending-limit-in-azure-portal"></a>Entfernen des Ausgabenlimits im Azure-Portal

Wenn für Ihr Abonnement eine gültige Zahlungsmethode festgelegt wurde, können Sie das Ausgabenlimit jederzeit entfernen. Bei Abonnementtypen mit Guthaben für mehrere Monate (beispielsweise Visual Studio Enterprise und Visual Studio Professional) kann das Ausgabenlimit dauerhaft oder nur für den aktuellen Abrechnungszeitraum aufgehoben werden. Wenn Sie nur den aktuellen Abrechnungszeitraum auswählen, wird das Ausgabenlimit zu Beginn des nächsten Abrechnungszeitraums automatisch wieder aktiviert.

Falls Sie über ein kostenloses Azure-Konto verfügen, erfahren Sie unter [Durchführen eines Upgrades für Ihr kostenloses Azure-Konto oder Ihr Azure for Students Starter-Konto](upgrade-azure-subscription.md), wie Sie Ihr Ausgabenlimit entfernen. Führen Sie andernfalls die folgenden Schritte aus, um das Ausgabenlimit zu entfernen:

<a id="remove"></a>

1. Melden Sie sich als Kontoadministrator beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot: Suchen nach „Kostenverwaltung + Abrechnung“ ](./media/spending-limit/search-bar.png)

1. Wählen Sie in der Liste **Meine Abonnements** Ihr Abonnement aus. Beispielsweise *Visual Studio Enterprise*. 

   ![Screenshot: Raster „Meine Abonnements“ in der Übersicht](./media/spending-limit/cost-management-overview-msdn-x.png)

    > [!NOTE]
    > Wenn einige Ihrer Visual Studio-Abonnements hier nicht angezeigt werden, liegt dies möglicherweise daran, dass Sie ein Abonnementverzeichnis zu irgendeinem Zeitpunkt geändert haben. Für diese Abonnements müssen Sie das Verzeichnis in das ursprüngliche Verzeichnis ändern (das Verzeichnis, in dem Sie sich anfänglich registriert haben). Wiederholen Sie anschließend Schritt 2.
    
1. Klicken Sie in der Abonnementübersicht auf das orangefarbene Banner, um das Ausgabenlimit zu entfernen.
    
    ![Screenshot: Banner zum Entfernen des Ausgabenlimits](./media/spending-limit/msdn-remove-spending-limit-banner-x.png)

1. Wählen Sie aus, ob Sie das Ausgabenlimit dauerhaft oder nur für den aktuellen Abrechnungszeitraum entfernen möchten.
    
      ![Screenshot: Blatt zum Entfernen des Ausgabenlimits](./media/spending-limit/remove-spending-limit-blade-x.png)
    
      | Option | Wirkung |
      | --- | --- |
      | Ausgabenlimit auf unbestimmte Zeit deaktivieren | Das Ausgabenlimit wird zu Beginn des nächsten Abrechnungszeitraums nicht automatisch wieder aktiviert. Sie können es aber jederzeit selbst wieder aktivieren. |
      | Ausgabenlimit für den aktuellen Abrechnungszeitraum deaktivieren | Das Ausgabenlimit wird zu Beginn des nächsten Abrechnungszeitraums automatisch wieder aktiviert. |


1. Klicken Sie auf **Zahlungsmethode auswählen**, um eine Zahlungsmethode für Ihr Abonnement auszuwählen. Diese wird als aktive Zahlungsmethode für Ihr Abonnement verwendet.

1. Klicken Sie auf **Fertig stellen**.


## <a name="why-you-might-want-to-remove-the-spending-limit"></a>Warum sollten Sie das Ausgabenlimit entfernen?

Das Ausgabenlimit könnte verhindern, dass Sie bestimmte Drittanbieterdienste und Microsoft-Dienste bereitstellen und verwenden können. Im Folgenden sind Situationen aufgeführt, in denen Sie das Ausgabenlimit für Ihr Abonnement entfernen sollten.

-  Sie planen, Images von Drittanbietern (beispielsweise Oracle) oder Dienste wie Azure DevOps Services bereitzustellen. In dieser Situation erreichen Sie praktisch sofort das Ausgabenlimit, woraufhin Ihr Abonnement deaktiviert wird.
- Sie haben Dienste, die nicht unterbrochen werden sollen. Bei Erreichen Ihres Ausgabenlimits werden von Ihnen bereitgestellte Azure-Ressourcen aus der Produktion entfernt, Ihre virtuellen Azure-Computer werden beendet, und deren Zuordnung wird aufgehoben. Wenn Sie über Dienste verfügen, die nicht unterbrochen werden sollen, müssen Sie Ihr Ausgabenlimit entfernen.
- Sie verfügen über Dienste und Ressourcen mit Einstellungen wie virtuellen IP-Adressen, die nicht verloren gehen sollen. Wenn Sie Ihr Ausgabenlimit erreichen und die Zuordnung der Dienste und Ressourcen aufgehoben wird, gehen diese Einstellungen verloren.

## <a name="turn-on-the-spending-limit-after-removing"></a>Aktivieren des Ausgabenlimits nach dem Entfernen

Dieses Feature ist nur verfügbar, wenn das Ausgabenlimit für Abonnementtypen, die über Guthaben für mehrere Monate verfügen, ohne zeitliche Beschränkung entfernt wurde. Mithilfe dieses Features kann das Ausgabenlimit automatisch zu Beginn des nächsten Abrechnungszeitraums aktiviert werden.


1. Melden Sie sich als Kontoadministrator beim [Azure-Portal](https://portal.azure.com) an.
1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

    ![Screenshot: Suchen nach „Kostenverwaltung + Abrechnung“ ](./media/spending-limit/search-bar.png)

1. Wählen Sie in der Liste **Meine Abonnements** Ihr Abonnement aus. Beispielsweise *Visual Studio Enterprise*. 

   ![Screenshot: Raster „Meine Abonnements“ in der Übersicht](./media/spending-limit/cost-management-overview-msdn-x.png)

    > [!NOTE]
    > Wenn einige Ihrer Visual Studio-Abonnements hier nicht angezeigt werden, liegt dies möglicherweise daran, dass Sie ein Abonnementverzeichnis zu irgendeinem Zeitpunkt geändert haben. Für diese Abonnements müssen Sie das Verzeichnis in das ursprüngliche Verzeichnis ändern (das Verzeichnis, in dem Sie sich anfänglich registriert haben). Wiederholen Sie anschließend Schritt 2.
    
1. Klicken Sie in der Abonnementübersicht auf das Banner am oberen Seitenrand, um das Ausgabenlimit wieder zu aktivieren.

## <a name="custom-spending-limit"></a>Benutzerdefiniertes Ausgabenlimit

Ausgabenlimits von Kunden sind nicht verfügbar.

## <a name="a-spending-limit-doesnt-prevent-all-charges"></a>Ein Ausgabenlimit verhindert nicht alle Gebühren

[Einige im Azure Marketplace veröffentlichte externe Dienste](../understand/understand-azure-marketplace-charges.md) können nicht mit Ihrem Abonnementguthaben verwendet werden und separate Gebühren verursachen, selbst wenn ein Ausgabenlimit festgelegt ist. Beispiele sind Visual Studio-Lizenzen, Azure Active Directory Premium, Supportpläne und die meisten Drittanbieterdienste. Wenn Sie einen neuen externen Dienst bereitstellen, wird eine Warnung angezeigt, um darauf hinzuweisen, dass die Dienste separat in Rechnung gestellt werden:

![Warnung beim Erwerb von Marketplace-Angeboten](./media/spending-limit/marketplace-warning01.png)

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte
- Steigen Sie auf einen Plan mit [nutzungsbasierter Bezahlung](upgrade-azure-subscription.md) um.
