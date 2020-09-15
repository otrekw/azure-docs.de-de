---
title: Übertragen von Azure-Abonnements von einem Partner auf einen anderen (Vorschau)
description: In diesem Artikel erfahren Sie, was Sie wissen sollten, bevor und nachdem Sie den Abrechnungsbesitz Ihres Azure-Abonnements übertragen haben.
author: bandersmsft
ms.reviewer: mcville
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 09/07/2020
ms.author: banders
ms.openlocfilehash: 246128ec3d26e4bf3f2a5a8f0660aab7d9daca3d
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89554197"
---
# <a name="transfer-azure-subscriptions-from-one-partner-to-another-preview"></a>Übertragen von Azure-Abonnements von einem Partner auf einen anderen (Vorschau)

In diesem Artikel erfahren Sie, was Sie wissen sollten, bevor und nachdem Sie den Abrechnungsbesitz Ihres Azure-Abonnements übertragen haben. Um die Übertragung eines Azure-Abonnements unter einem Azure-Plan von einem Microsoft-Partner auf einen anderen zu initiieren, müssen Sie sich an Ihren Partner wenden. Der Partner sendet Ihnen Anweisungen zu den ersten Schritten. Nachdem die Übertragung abgeschlossen ist, wird der Abrechnungsbesitz Ihres Abonnements geändert.

## <a name="user-access"></a>Benutzerzugriff

Der Zugriff auf vorhandene Benutzer, Gruppen oder Dienstprinzipale, die per rollenbasierter Azure-Zugriffssteuerung (Azure Role-Based Access Control, Azure RBAC) zugewiesen wurden, ist von der Übertragung nicht betroffen. Mit [Azure RBAC](../../role-based-access-control/overview.md) können Sie verwalten, welche Benutzer Zugriff auf Azure-Ressourcen haben, welche Aktionen Benutzer für diese Ressourcen ausführen können und auf welche Bereiche sie zugreifen können. Der neue Partner erhält durch die Abonnementübertragung keinen RBAC-Zugriff auf Ihre Ressourcen. Ihr vorheriger Partner behält seinen RBAC-Zugriff.

Daher ist es wichtig, dass Sie den Azure RBAC-Zugriff des alten Partners entfernen und dem neuen Partner Zugriff gewähren. Weitere Informationen dazu, wie Sie Ihrem neuen Partner Zugriff gewähren, finden Sie unter [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../../role-based-access-control/overview.md) Weitere Informationen dazu, wie Sie den RBAC-Zugriff Ihres vorherigen Partners entfernen, finden Sie unter [Entfernen einer Rollenzuweisung](../../role-based-access-control/role-assignments-portal.md#remove-a-role-assignment).

Außerdem erhält der neue Partner nicht automatisch Zugriff vom Typ [Administrator im Namen von](https://channel9.msdn.com/Series/cspdev/Module-11-Admin-On-Behalf-Of-AOBO) (Admin on Behalf Of, AOBO) auf Ihre Abonnements. AOBO ist für Ihren Partner erforderlich, um die Azure-Abonnements in Ihrem Namen zu verwalten. Weitere Informationen dazu, wie Sie Ihrem neuen Partner AOBO-Zugriff gewähren, finden Sie unter [Übertragen des Abrechnungsbesitzes eines Azure-Abonnements an ein anderes Konto](billing-subscription-transfer.md).

## <a name="stop-a-transfer"></a>Abbrechen einer Übertragung

Nachdem Sie die Bestätigung erhalten haben, dass eine Übertragungsanforderung übermittelt wurde, können Sie mithilfe einer der folgenden Optionen verhindern, **dass die Übertragung fortgesetzt wird**.

- Wenn die Anforderung noch nicht von Ihrem aktuellen Partner akzeptiert wurde, können Sie den neuen Partner bitten, die von Ihnen gestartete Übertragungsanforderung abzubrechen (wenn der Status „Ausstehend“ ist).
- Bitten Sie den aktuellen Partner, beim Erhalt der Übertragungsanforderung nichts zu unternehmen. Ohne Zustimmung Ihres aktuellen Partners kann die Übertragung nicht fortgesetzt werden. Die Anforderung läuft ab.
- Sie können _Ihre Vertriebspartnerschaft_ mit dem neuen Partner entfernen. Durch diese Aktion wird die Möglichkeit aufgehoben, Ihr Abonnement zu übertragen. Dadurch wird die Anforderung tatsächlich abgebrochen.

Mithilfe einer der Optionen auf der [Microsoft-Website für rechtliche Angelegenheiten](https://www.microsoft.com/legal/) können Sie auch Unterstützung anfordern sowie Fehlverhalten oder verdächtige Aktivitäten melden. Die Option zum Melden eines Anliegens finden Sie unter „Compliance & ethics“ (Compliance und Ethik).

## <a name="next-steps"></a>Nächste Schritte

- Wie Sie Ihrem neuen Partner RBAC-Zugriff gewähren, erfahren Sie unter [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../../role-based-access-control/overview.md)
- Wie Sie Ihrem neuen Partner AOBO-Zugriff gewähren, finden Sie unter [Übertragen des Abrechnungsbesitzes eines Azure-Abonnements an ein anderes Konto](billing-subscription-transfer.md).