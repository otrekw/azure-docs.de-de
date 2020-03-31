---
title: include file
titleSuffix: Azure
description: include file
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 2a42ba8809e6895c9eea9f8762513b7fcaa9eb3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773965"
---
1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Konfiguration** die Felder wie unten gezeigt aus.

    * Wählen Sie für **Peeringtyp** *Direct* aus.
    * Wählen Sie für **Microsoft-Netzwerk** *AS8075* aus. Erstellen Sie kein Peering mit ASN 8069. Sie ist für Sonderanwendungen reserviert und wird nur von [Microsoft Peering](mailto:peering@microsoft.com) verwendet.
    * Wählen Sie unter **SKU** die Option *Basic Free* aus. Wählen Sie nicht *Premium Free* aus, da diese Option für Sonderanwendungen reserviert ist.
    * Wählen Sie unter **Metro** den Standort aus, an dem Sie Peering einrichten möchten.

        > [!NOTE]
        > Wenn Sie am unter **Metro** ausgewählten Standort bereits über Peeringverbindungen mit Microsoft verfügen und Sie das Portal zum ersten Mal zum Einrichten von Peering an diesem Standort verwenden, werden Ihre vorhandenen Peeringverbindungen im Abschnitt **Peeringverbindungen** aufgelistet, wie unten zu sehen. Microsoft konvertiert diese Peeringverbindungen automatisch in Azure-Ressourcen, sodass Sie sie zusammen mit den neuen Verbindungen an einem zentralen Ort verwalten können. Weitere Informationen finden Sie unter [Konvertieren einer Legacy-Instanz für Direct Peering in eine Azure-Ressource über das Portal](../howto-legacy-direct-portal.md).
        >

1. Klicken Sie unter **Peeringverbindungen** auf **Neu erstellen**, um für jede neue Verbindung, die Sie einrichten möchten, eine Zeile hinzuzufügen.

    * Klicken Sie für eine Zeile auf die Schaltfläche „Bearbeiten“, um Verbindungseinstellungen zu konfigurieren oder zu ändern.

        > [!div class="mx-imgBorder"]
        > ![Peeringkonfiguration – Direkte Bearbeitung](../media/setup-direct-conf-tab-edit.png)
    
    * Klicken Sie zum Löschen einer Zeile auf die Schaltfläche **...** und anschließend auf **Löschen**.

        > [!div class="mx-imgBorder"]
        > ![Peeringkonfiguration – Direkte Bearbeitung](../media/setup-direct-conf-tab-delete.png)

    * Pro Verbindung müssen alle Einstellungen wie unten gezeigt angegeben werden.

         > [!div class="mx-imgBorder"]
         > ![Peeringkonfiguration – direkte Verbindung](../media/setup-direct-conf-tab-connection.png)

        1. Wählen Sie die **Peeringeinrichtung** aus, in der die Verbindung eingerichtet werden soll.
        1. **Sitzungsadressanbieter** wird verwendet, um zu bestimmen, wer das erforderliche Subnetz bereitstellt, das zum Einrichten der BGP-Sitzung zwischen Ihrem Netzwerk und Microsoft benötigt wird. Wenn Sie das Subnetz bereitstellen können, wählen Sie *Peer* aus. Wählen Sie andernfalls **Microsoft** aus, dann wird [Microsoft Peering](mailto:peering@microsoft.com) sich mit Ihnen in Verbindung setzen. Beachten Sie, dass bei Auswahl dieser Option Microsoft mehr Zeit für die Verarbeitung der Peeringanforderung benötigt. In einigen Fällen ist Microsoft möglicherweise nicht in der Lage, Subnetze bereitzustellen, was zu einer Ablehnung der Anforderung führt.
        1. Wenn Sie für **Sitzungsadressanbieter** *Peer* ausgewählt haben, geben Sie die IPv4- und die IPv6-Adresse zusammen mit der Präfixmaske im Feld **IPv4-Sitzungspräfix** bzw. **IPv6-Sitzungspräfix** ein.
        1. Geben Sie im Feld **Maximum advertised IPv4 addresses** (Maximal angekündigte IPv4-Adressen) bzw. **Maximum advertised IPv6 addresses** (Maximal angekündigte IPv6-Adressen) die Anzahl von IPv4- und IPv6-Präfixen ein, die Sie ankündigen möchten.
        1. Passen Sie den Schieberegler **Bandbreite gesamt** an, sodass er die Bandbreite der Verbindung wiedergibt.
        1. Klicken Sie auf **OK**, um Ihre Verbindungseinstellungen zu speichern.

1. Wiederholen Sie den obigen Schritt, um weitere Verbindungen für Einrichtungen hinzuzufügen, bei denen sich Microsoft und Ihr Netzwerk am gleichen Standort befinden, den Sie zuvor unter **Metro** ausgewählt haben.

1. Nachdem Sie alle erforderlichen Verbindungen hinzugefügt haben, klicken Sie auf **Überprüfen + erstellen**.

    > [!div class="mx-imgBorder"]
    > ![Peeringkonfiguration: Letzte Registerkarte](../media/setup-direct-conf-tab-final.png)

1. Daraufhin wird vom Portal eine grundlegende Überprüfung der eingegebenen Informationen durchgeführt. Dies wird auf einem Menüband im oberen Bereich als *Abschließende Überprüfung wird ausgeführt...* angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „Peeringüberprüfung“](../media/setup-direct-review-tab-validation.png)

1. Nachdem sich die Anzeige in *Überprüfung bestanden* geändert hat, überprüfen Sie Ihre Informationen, und übermitteln Sie die Anforderung, indem Sie auf **Erstellen** klicken. Wenn Sie Ihre Anforderung ändern müssen, klicken Sie auf **Zurück**, und wiederholen Sie die obigen Schritte.

    > [!div class="mx-imgBorder"]
    > ![Peeringübermittlung](../media/setup-direct-review-tab-submit.png)

1. Nachdem Sie die Anforderung übermittelt haben, warten Sie, bis die Bereitstellung beendet ist. Sollte bei der Bereitstellung ein Fehler auftreten, wenden Sie sich an [Microsoft Peering](mailto:peering@microsoft.com). Eine erfolgreiche Bereitstellung sieht wie unten dargestellt aus.

    > [!div class="mx-imgBorder"]
    > ![Peering erfolgreich](../media/setup-direct-success.png)
