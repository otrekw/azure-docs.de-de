---
title: Kündigen Ihres Azure-Abonnements
description: Beschreibt, wie Sie Ihr Azure-Abonnement, etwa das Abonnement für die kostenlose Testversion, kündigen.
author: bandersmsft
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 08/10/2020
ms.author: banders
ms.openlocfilehash: 592a5d9573cb4e475c5d1ca6b4f06002f949e197
ms.sourcegitcommit: 1aef4235aec3fd326ded18df7fdb750883809ae8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2020
ms.locfileid: "88136251"
---
# <a name="cancel-your-azure-subscription"></a>Kündigen Ihres Azure-Abonnements

Sie können Ihr Azure-Abonnement im Azure-Portal kündigen, wenn Sie das Abonnement nicht länger benötigen.

Die folgenden Schritte sind zwar nicht obligatorisch, aber Microsoft *empfiehlt* Ihnen, sie vor dem Kündigen Ihres Abonnements auszuführen:

* Sichern Sie Ihre Daten. Wenn Sie Ihre Daten beispielsweise im Azure-Speicher oder SQL speichern, laden Sie eine Kopie herunter. Wenn Sie einen virtuellen Computer haben, speichern Sie lokal ein Image davon.
* Beenden Sie Ihre Dienste. Wechseln Sie zur [Ressourcenseite im Verwaltungsportal](https://ms.portal.azure.com/?flight=1#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2Fresources), und **beenden** Sie alle ausgeführten virtuellen Computer, Anwendungen oder anderen Dienste.
* Ziehen Sie die Migration Ihrer Daten in Betracht. Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../../azure-resource-manager/management/move-resource-group-and-subscription.md).
* Löschen Sie alle Ressourcen und Ressourcengruppen.
* Wenn Sie über benutzerdefinierte Rollen verfügen, die in `AssignableScopes` auf das Abonnement verweisen, sollten Sie die benutzerdefinierten Rollen aktualisieren, um das Abonnement zu entfernen. Wenn Sie versuchen, eine benutzerdefinierte Rolle zu aktualisieren, nachdem Sie ein Abonnement gekündigt haben, wird möglicherweise ein Fehler ausgegeben. Weitere Informationen finden Sie unter [Probleme mit benutzerdefinierten Rollen](../../role-based-access-control/troubleshooting.md#problems-with-custom-roles) und [Benutzerdefinierte Azure-Rollen](../../role-based-access-control/custom-roles.md).

Wenn Sie einen kostenpflichtigen Azure-Supportplan kündigen, werden Ihnen die Gebühren für die verbleibende Abonnementlaufzeit in Rechnung gestellt. Weitere Informationen finden Sie unter [Azure-Supportpläne](https://azure.microsoft.com/support/plans/).

## <a name="cancel-subscription-in-the-azure-portal"></a>Kündigen eines Abonnements im Azure-Portal

1. Wählen Sie auf der [Seite „Abonnements“ im Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Ihr Abonnement aus.
1. Wählen Sie das Abonnement aus, das Sie kündigen möchten.
1. Wählen Sie **Übersicht** und dann **Abonnement kündigen** aus.
    ![Screenshot mit der Schaltfläche zum Kündigen](./media/cancel-azure-subscription/cancel_ibiza.png)
1. Befolgen Sie die Anweisungen, und schließen Sie die Kündigung ab.

## <a name="who-can-cancel-a-subscription"></a>Wer kann ein Abonnement kündigen?

Die Tabelle unten beschreibt die Berechtigungen, die zum Kündigen eines Abonnements benötigt werden.

|Abonnementtyp     |Wer kann kündigen?  |
|---------|---------|
|Abonnements, die bei der Registrierung für Azure über die Azure-Website erstellt wurden. Beispiele hierfür sind die Registrierung für ein [Kostenloses Azure-Konto](https://azure.microsoft.com/offers/ms-azr-0044p/), ein [Angebot mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) oder als [Visual Studio-Abonnent](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/). |  Kontoadministrator und Besitzer des Abonnements  |
|[Microsoft Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/) und [Enterprise Dev/Test](https://azure.microsoft.com/offers/ms-azr-0148p/)     |  Kontobesitzer und Besitzer des Abonnements       |
|[Azure-Plan](https://azure.microsoft.com/offers/ms-azr-0017g/) und [Azure-Plan für DevTest](https://azure.microsoft.com/offers/ms-azr-0148g/)     |  Besitzer des Abonnements      |

## <a name="what-happens-after-i-cancel-my-subscription"></a>Was geschieht, nachdem ich mein Abonnement gekündigt habe?

Nach Ihrer Kündigung wird die Abrechnung sofort beendet. Bis die Kündigung im Portal angezeigt wird, kann es allerdings bis zu zehn Minuten dauern. Wenn Sie das Abonnement mitten in einem Abrechnungszeitraum kündigen, senden wir Ihnen die Schlussrechnung zum üblichen Rechnungsdatum nach Ende des Abrechnungszeitraums zu.

Nach der Kündigung werden Ihre Dienste deaktiviert. Dies bedeutet, dass die Zuordnung Ihrer virtuellen Computer aufgehoben wird, die vorübergehenden IP-Adressen freigegeben werden und der Speicher nur noch im Lesezugriff verfügbar ist.

Nach der Kündigung Ihres Abonnements warten wir noch 30 bis 90 Tage mit der endgültigen Löschung Ihrer Daten, falls Sie doch noch einmal darauf zugreifen müssen oder Ihre Meinung ändern sollten. Für die Aufbewahrung der Daten rechnen wir keine Gebühr ab. Weitere Informationen finden Sie in [Microsoft Trust Center – Wie wir Ihre Daten verwalten](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409).

## <a name="delete-free-trial-subscription"></a>Löschen eines kostenlosen Testabonnements

Wenn Sie über ein kostenloses Testabonnement verfügen, müssen Sie nicht 30 Tage warten, bis das Abonnement automatisch gelöscht wird. Sie können Ihr Abonnement *drei Tage* nach der Kündigung löschen. Die Option **Abonnement löschen** wird erst drei Tage nach der Kündigung Ihres Abonnements verfügbar.

1. Warten Sie drei Tage, nachdem Sie das Abonnement gekündigt haben.
1. Wählen Sie im Azure-Portal auf der Seite [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Ihr Abonnement aus.
1. Wählen Sie das Abonnement aus, das Sie löschen möchten.
1. Wählen Sie **Übersicht** und anschließend **Abonnement löschen** aus.

## <a name="reactivate-subscription"></a>Erneutes Aktivieren von Abonnements

Wenn Sie Ihr Abonnement mit nutzungsbasierter Bezahlung versehentlich gekündigt haben, können Sie es [im Kontocenter reaktivieren](subscription-disabled.md).

Wenn es sich bei Ihrem Abonnement nicht um ein Abonnement mit nutzungsbasierter Bezahlung handelt, nehmen Sie bitte innerhalb von 90 Tagen nach der Kündigung Kontakt zum Support auf, um Ihr Abonnement zu reaktivieren.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

- Bei Bedarf können Sie ein Abonnement mit nutzungsbasierter Bezahlung im [Azure-Kontocenter](subscription-disabled.md) reaktivieren.