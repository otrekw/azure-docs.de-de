---
title: Zuordnen der Peer-ASN zum Azure-Abonnement über das Portal
titleSuffix: Azure
description: Zuordnen der Peer-ASN zum Azure-Abonnement über das Portal
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: how-to
ms.date: 5/18/2020
ms.author: derekol
ms.openlocfilehash: 57c38d3b6eeb26a38a36fb67da25bcf960cee2dc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84710846"
---
# <a name="associate-peer-asn-to-azure-subscription-using-the-portal"></a>Zuordnen der Peer-ASN zum Azure-Abonnement über das Portal

Als Internetdienstanbieter oder Internet Exchange-Anbieter sollten Sie vor dem Einreichen einer Peeringanforderung zunächst Ihre ASN mithilfe der unten dargestellten Schritte einem Azure-Abonnement zuordnen.

Falls Sie es vorziehen, können Sie diese Anleitung auch mithilfe von [PowerShell](howto-subscription-association-powershell.md) ausführen.

## <a name="create-peerasn-to-associate-your-asn-with-azure-subscription"></a>Erstellen einer PeerAsn zum Zuordnen Ihrer ASN zum Azure-Abonnement

### <a name="sign-in-to-the-portal"></a>Anmelden beim Portal
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="register-for-peering-resource-provider"></a>Für Peeringressourcenanbieter registrieren
Registrieren Sie sich in Ihrem Abonnement für den Peeringressourcenanbieter, indem Sie die Schritte unten ausführen. Wenn Sie diese Schritte nicht ausführen, sind die zum Einrichten des Peerings erforderlichen Azure-Ressourcen nicht verfügbar.

1. Klicken Sie in der oberen linken Ecke des Portals auf **Abonnements**. Wenn dieser Eintrag nicht angezeigt wird, klicken Sie auf **Weitere Dienste**, und suchen Sie danach.

    > [!div class="mx-imgBorder"]
    > ![Öffnen von Abonnements](./media/rp-subscriptions-open.png)

1. Klicken Sie auf das Abonnement, das Sie für das Peering verwenden möchten.

    > [!div class="mx-imgBorder"]
    > ![Starten des Abonnements](./media/rp-subscriptions-launch.png)

1. Klicken Sie nach dem Öffnen des Abonnements auf der linken Seite auf **Ressourcenanbieter**. Suchen Sie dann im rechten Bereich im Suchfenster nach *Peering*, oder verwenden Sie die Scrollleiste, um **Microsoft.Peering** zu finden, und sehen Sie sich den **Status** an. Wenn der Status ***Registriert*** angezeigt wird, überspringen Sie die Schritte unten, und fahren Sie mit dem Abschnitt **PeerAsn erstellen** fort. Wenn der Status ***NIcht registriert*** ist, wählen Sie **Microsoft.Peering** aus, und klicken Sie auf **Registrieren**.

    > [!div class="mx-imgBorder"]
    > ![Beginn der Registrierung](./media/rp-register-start.png)

1. Beachten Sie, dass sich der Status in ***Wird registriert*** ändert.

    > [!div class="mx-imgBorder"]
    > ![Registrierung wird ausgeführt](./media/rp-register-progress.png)

1. Warten Sie etwa eine Minute lang bis zum Abschluss der Registrierung. Klicken Sie dann auf **Aktualisieren**, und überprüfen Sie, ob der Status jetzt ***Registriert*** ist.

    > [!div class="mx-imgBorder"]
    > ![Registrierung abgeschlossen](./media/rp-register-completed.png)

### <a name="create-peerasn"></a>PeerAsn erstellen
Als Internetdienstanbieter oder Internet Exchange-Anbieter können Sie eine neue PeerAsn-Ressource zum Zuordnen einer ASN (Autonome Systemnummer) zu einem Azure-Abonnement auf der Seite [Peer-ASN zuordnen](https://go.microsoft.com/fwlink/?linkid=2129592) erstellen. Sie können einem Abonnement mehrere ASNs zuordnen, indem Sie eine a **PeerAsn** für jede ASN erstellen, die Sie zuordnen möchten.

1. Füllen Sie auf der Seite **Peer-ASN zuordnen** auf der Registerkarte **Grundlagen** die Felder wie unten dargestellt aus.

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „PeerAsn-Grundlagen“](./media/peerasn-basics-tab.png)

    * **Name** entspricht dem Ressourcennamen und ist frei wählbar.  
    * Wählen Sie das **Abonnement** aus, dem Sie den ASN zuordnen möchten.
    * **Peername** entspricht dem Namen Ihres Unternehmens und muss so nah wie möglich an Ihrem PeeringDB-Profil sein. Beachten Sie, dass der Wert nur die Zeichen a–z, A–Z und Leerzeichen unterstützt.
    * Geben Sie Ihre ASN im Feld **Peer-ASN** ein.
    * Klicken Sie auf **Neue erstellen**, und geben Sie die **E-MAIL-ADRESSE** und **TELEFONNUMMER** Ihres Network Operations Center (NOC) ein.
1. Klicken Sie dann auf **Überprüfen + Erstellen**, und beachten Sie, dass das Portal eine grundlegende Überprüfung der eingegebenen Informationen ausführt. Dies wird in einem Menüband oben auf der Seite angezeigt, als *Abschließende Überprüfung wird ausgeführt...* .

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „PeerAsn überprüfen“](./media/peerasn-review-tab-validation.png)

1. Sobald die Nachricht im Menüband zu *Überprüfung bestanden* wechselt, überprüfen Sie Ihre Informationen, und reichen Sie die Anforderung ein, indem Sie auf **Erstellen** klicken. Wenn die Überprüfung nicht bestanden wird, klicken Sie auf **Zurück**, und wiederholen Sie die Schritte oben, um Ihre Anforderung zu ändern und sicherzustellen, dass die eingegebenen Werte frei von Fehlern sind.

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „PeerAsn überprüfen“](./media/peerasn-review-tab.png)

1. Nachdem Sie die Anforderung übermittelt haben, warten Sie, bis die Bereitstellung beendet ist. Wenn bei der Bereitstellung ein Fehler auftritt, wenden Sie sich an [Microsoft Peering](mailto:peering@microsoft.com). Eine erfolgreiche Bereitstellung wird wie unten dargestellt angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Erfolg von „PeerAsn“](./media/peerasn-success.png)

### <a name="view-status-of-a-peerasn"></a>Anzeigestatus einer PeerAsn
Nach der erfolgreichen Bereitstellung der PeerAsn-Ressource müssen Sie warten, dass Microsoft die Assoziierungsanforderung genehmigt. Die Genehmigung kann bis zu 12 Stunden dauern. Nach erfolgter Genehmigung erhalten Sie eine Benachrichtigung an die E-Mail-Adresse, die im Abschnitt oben eingegeben wurde.

> [!IMPORTANT]
> Warten Sie, bis der ValidationState sich in „Genehmigt“ geändert hat, bevor Sie eine Peeringanforderung einreichen. Diese Genehmigung kann bis zu 12 Stunden dauern.

## <a name="modify-peerasn"></a>PeerAsn ändern
Das Ändern der PeerAsn wird zurzeit nicht unterstützt. Wenn Sie Änderungen vornehmen müssen, wenden Sie sich an [Microsoft Peering](mailto:peering@microsoft.com).

## <a name="delete-peerasn"></a>PeerAsn löschen
Das Löschen einer PeerAsn wird zurzeit nicht unterstützt. Wenn Sie die PeerAsn löschen müssen, wenden Sie sich an [Microsoft Peering](mailto:peering@microsoft.com).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern einer Instanz für Direct Peering über das Portal](howto-direct-portal.md)
* [Konvertieren einer Legacy-Instanz für Direct Peering in eine Azure-Ressource über das Portal](howto-legacy-direct-portal.md)
* [Erstellen oder Ändern einer Instanz für Exchange Peering über das Portal](howto-exchange-portal.md)
* [Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource über das Portal](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen finden Sie unter [Häufig gestellte Fragen zum Internetpeering](faqs.md).
