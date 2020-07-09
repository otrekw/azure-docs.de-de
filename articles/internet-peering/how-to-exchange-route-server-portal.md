---
title: Peeringverbindung für Exchange-Partner mit Routenserver mithilfe des Portals
titleSuffix: Azure
description: Erstellen oder Ändern eines Exchange-Peerings mit Routenserver mithilfe des Azure-Portals
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: how-to
ms.date: 5/19/2020
ms.author: derekol
ms.openlocfilehash: 1eaf7413b01bceacbcbf3640bfe654fdad026672
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84700667"
---
# <a name="create-or-modify-an-exchange-peering-with-route-server-in-azure-portal"></a>Erstellen oder Ändern eines Exchange-Peerings mit Routenserver im Azure-Portal

In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal mit einem Routenserver ein Microsoft-Exchange-Peering erstellen. Der Artikel veranschaulicht darüber hinaus, wie Sie den Status der Ressource prüfen und die Ressource aktualisieren, löschen oder ihre Bereitstellung aufheben.


## <a name="before-you-begin"></a>Voraussetzungen
* Lesen Sie vor dem Konfigurieren die Informationen zu den [Voraussetzungen](prerequisites.md) sowie die [Exemplarische Vorgehensweise für das Exchange Peering](walkthrough-exchange-all.md).
* Falls Sie bereits Exchange Peerings mit Microsoft eingerichtet haben, die nicht in Azure-Ressourcen konvertiert werden, lesen Sie die Informationen unter [Konvertieren eines Legacy-Exchange Peerings in eine Azure-Ressource mithilfe des Portals](howto-legacy-exchange-portal.md).

## <a name="create-and-provision-an-exchange-peering"></a>Erstellen und Bereitstellen eines Exchange Peerings

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Melden Sie sich beim Portal an, und wählen Sie Ihr Abonnement aus.
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-an-exchange-peering-with-route-server"></a><a name=create></a>Erstellen eines Exchange-Peerings mit einem Routenserver


Als Internet Exchange-Anbieter können Sie durch [Erstellen eines Peerings]( https://go.microsoft.com/fwlink/?linkid=2129593) eine Anforderung für ein Exchange Peering erstellen.

1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Grundlagen** die Felder wie hier gezeigt aus:

    > [!div class="mx-imgBorder"] 
    > ![Registrieren eines Peering Service](./media/setup-basics-tab.png)

* Wählen Sie Ihr Azure-Abonnement aus.

* Als Ressourcengruppe können Sie in der Dropdownliste eine vorhandene Ressourcengruppe auswählen oder eine neue Gruppe erstellen, indem Sie „Neu erstellen“ auswählen. Im Rahmen dieses Beispiels erstellen Sie eine neue Ressourcengruppe.

* Der Name entspricht dem Ressourcennamen und ist frei wählbar.

* Die Region wird automatisch ausgewählt, wenn Sie eine vorhandene Ressourcengruppe ausgewählt haben. Wenn Sie eine neue Ressourcengruppe erstellen möchten, müssen Sie auch die Azure-Region auswählen, in der sich die Ressource befinden soll.

    >[!NOTE]
    >Die Region, in der sich die Ressourcengruppe befindet, ist unabhängig vom Standort, an dem Sie das Peering mit Microsoft erstellen möchten. Es empfiehlt sich jedoch, Ihre Peeringressourcen innerhalb von Ressourcengruppen zu organisieren, die sich in nahegelegenen Azure-Regionen befinden. Beispiel: Für Peerings in Ashburn können Sie eine Ressourcengruppe in „USA, Osten“ oder in „USA, Osten 2“ erstellen.

* Wählen Sie Ihre ASN im Feld **PeerASN** aus.

    >[!IMPORTANT] 
    >Sie können nur eine ASN mit dem ValidationState „Genehmigt“ auswählen, bevor Sie eine Peeringanforderung übermitteln. Wenn Sie Ihre PeerAsn-Anforderung gerade erst übermittelt haben, warten Sie ca. 12 Stunden, damit die ASN-Zuordnung genehmigt werden kann. Wenn die Genehmigung der von Ihnen ausgewählten ASN noch aussteht, wird eine Fehlermeldung angezeigt. Wenn die benötigte ASN nicht angezeigt wird, überprüfen Sie, ob Sie das richtige Abonnement ausgewählt haben. Ist dies der Fall, überprüfen Sie mithilfe von **[Zuordnen der Peer-ASN zum Azure-Abonnement](https://go.microsoft.com/fwlink/?linkid=2129592)** , ob Sie PeerAsn bereits erstellt haben.

* Klicken Sie auf **Weiter: Konfiguration**, um fortzufahren.

#### <a name="configure-connections-and-submit"></a>Konfigurieren von Verbindungen und Übermitteln

1. Füllen Sie auf der Seite „Peering erstellen“ auf der Registerkarte „Konfiguration“ die Felder wie im Folgenden gezeigt aus:

    > [!div class="mx-imgBorder"]
    > ![Konfigurieren von Routenservern](./media/setup-exchange-conf-tab-routeserver.png)
 
    * Wählen Sie den Peeringtyp **Direkt** aus.
    * Wählen Sie für Microsoft-Netzwerk die Option **AS8075 with exchange route server** (AS8075 mit Exchange-Routenserver) aus. 
    * Wählen Sie unter SKU die Option **Basic Free** aus. Wählen Sie nicht „Premium Free“ aus, da diese Option für spezielle Anwendungen vorgesehen ist.
    * Wählen Sie den **Metro**-Standort aus, an dem Sie das Peering einrichten möchten.

1. Klicken Sie unter **Peeringverbindungen** auf **Neu erstellen**.

1.  Geben Sie unter **Direct Peering Connection** (Direkte Peeringverbindung) die folgenden BGP-Sitzunginformationen ein:

    > [!div class="mx-imgBorder"]
    > ![Konfigurieren des Routenservers für das direkte Peering](./media/setup-exchange-conf-tab-direct-route.png)


     * Wählen Sie unter „Peering Facility“ (Peeringeinrichtung) den entsprechenden physischen Standort für das Peering aus.
     * Wählen Sie unter „Session Address Provider“ (Sitzungsadressanbieter) die Option „Peer“ aus.
     * Das IPv4-Sitzungspräfix wird vom Peer des Exchange-Anbieters bereitgestellt.
     * Die IPv4-Adresse für die Peeringsitzung wird anhand des IP-Adressbereichs für das Präfix vom Exchange-Peering für den Routenserver ausgewählt.
     * Die IPv4-Adresse für die Microsoft-Sitzung entspricht der Router-IP, die vom IP-Adressbereich des Präfix zugewiesen wird.
     * Das IPv6-Sitzungspräfix ist derzeit optional.
     * Der Maximalwert für das angekündigte IPv4-Präfix kann bis zu 20000 betragen. 
     * Die Verwendung für Peering Service ist standardmäßig deaktiviert. Sie kann aktiviert werden, sobald der Exchange-Anbieter einen Servicevertrag für das Peering mit Microsoft vereinbart hat.

1. Klicken Sie nach Abschluss auf **Speichern**. 

1. Unter „Create a peering“ (Peering erstellen) wird angezeigt, dass die Überprüfung erfolgreich abgeschlossen wurde. Klicken Sie nach Abschluss der Überprüfung auf **Erstellen**.

    > [!div class="mx-imgBorder"]
    > ![Überprüfung der Einstellungen](./media/setup-exchange-conf-tab-validation.png)

    >[!NOTE]
    >Für normale Internetdienstanbieter (ISPs), die Peering Service-Partner von Microsoft sind, ist die Registrierung von Kunden-IP-Präfixen erforderlich. Für Exchange-Partner mit einem Routenserver ist es jedoch erforderlich, die ASN-Schlüssel der Kunden anstelle der Präfixe zu registrieren. Derselbe ASN-Schlüssel wäre für die Präfixregistrierung des Kunden gültig.

1. Klicken Sie im Abschnitt „Einstellungen“ auf **Registered ASNs** (Registrierte ASNs).

    > [!div class="mx-imgBorder"]
    > ![Konfigurieren des Routenservers für direktes Peering](./media/setup-exchange-registered-asn.png)

1. Klicken Sie auf **Add registered ASN** (Registrierte ASN hinzufügen), um eine neue Kunden-ASN in Ihrem Exchange-Abonnement zu erstellen.

    > [!div class="mx-imgBorder"]
    > ![Konfigurieren des Routenservers für direktes Peering](./media/setup-exchange-register-new-asn.png)

1. Geben Sie unter „Register an ASN“ (ASN registrieren) einen Namen an, füllen Sie die Kunden-ASN auf, und klicken Sie dann auf „Speichern“.

1. Unter „Registered ASNs“ (Registrierte ASNs) wird allen ASNs ein Präfixschlüssel zugeordnet. Als Exchange-Anbieter müssen Sie diesen Präfixschlüssel Ihrem Kunden bereitstellen, damit dieser Peering Service für sein Abonnement registrieren kann.

    > [!div class="mx-imgBorder"]
    > ![Konfigurieren des Routenservers für direktes Peering](./media/setup-exchange-register-asn-prefixkey.png)




### <a name="verify-an-exchange-peering"></a><a name=get></a>Überprüfen eines Austauschpeerings
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="modify-an-exchange-peering"></a><a name="modify"></a>Ändern eines Austauschpeerings
[!INCLUDE [peering-exchange-modify-portal](./includes/exchange-portal-modify.md)]

## <a name="deprovision-an-exchange-peering"></a><a name="delete"></a>Aufheben der Bereitstellung eines Exchange Peerings
[!INCLUDE [peering-exchange-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen oder Ändern einer Instanz für Direct Peering über das Portal](howto-direct-portal.md)
* [Konvertieren einer älteren Instanz für Direct Peering in eine Azure-Ressource mit dem Portal](howto-legacy-direct-portal.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen finden Sie unter [Internetpeering: häufig gestellte Fragen](faqs.md).
