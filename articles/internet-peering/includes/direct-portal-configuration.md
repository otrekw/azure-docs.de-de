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
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773965"
---
1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Konfiguration** die Felder wie unten dargestellt aus.

    * Wählen Sie für **Peeringtyp** *Direct* aus.
    * Wählen Sie für **Microsoft-Netzwerk** *AS8075* aus. Erstellen Sie kein Peering mit ASN 8069. Sie ist für Sonderanwendungen reserviert und wird nur von [Microsoft Peering](mailto:peering@microsoft.com) verwendet.
    * Wählen Sie für **SKU** *Basic Free* aus. Wählen Sie nicht *Premium Free* aus, da diese Option für Sonderanwendungen reserviert ist.
    * Wählen Sie den **Metro**-Standort aus, an dem Sie Peering einrichten möchten.

        > [!NOTE]
        > Wenn Sie bereits über Peeringverbindungen mit Microsoft am ausgewählten **Metro**-Standort verfügen und Sie das Portal zum ersten Mal zum Einrichten von Peering an diesem Speicherort verwenden, werden Ihre vorhandenen Peeringverbindungen im Abschnitt **Peeringverbindungen** aufgelistet, wie unten dargestellt. Microsoft konvertiert diese Peeringverbindungen automatisch in Azure-Ressourcen, sodass Sie sie zusammen mit den neuen Verbindungen an einem zentralen Ort verwalten können. Weitere Informationen finden Sie unter [Konvertieren einer Legacy-Instanz für Direct Peering in eine Azure-Ressource über das Portal](../howto-legacy-direct-portal.md).
        >

1. Klicken Sie unter **Peeringverbindungen** auf **Neue erstellen**, um eine Zeile für jede neue Verbindung hinzuzufügen, die Sie einrichten möchten.

    * Zum Konfigurieren/Ändern von Verbindungseinstellungen klicken Sie auf die Schaltfläche „Bearbeiten“ für eine Zeile.

        > [!div class="mx-imgBorder"]
        > ![Peeringkonfiguration – direkte Bearbeitung](../media/setup-direct-conf-tab-edit.png)
    
    * Um eine Zeile zu löschen, klicken Sie auf die **...** -Schaltfläche > **Löschen**.

        > [!div class="mx-imgBorder"]
        > ![Peeringkonfiguration – direkte Bearbeitung](../media/setup-direct-conf-tab-delete.png)

    * Sie müssen alle Einstellungen für eine Verbindung angeben, wie unten dargestellt.

         > [!div class="mx-imgBorder"]
         > ![Peeringkonfiguration – direkte Verbindung](../media/setup-direct-conf-tab-connection.png)

        1. Wählen Sie die **Peeringeinrichtung** aus, in der die Verbindung eingerichtet werden soll.
        1. **Sitzungsadressanbieter** wird verwendet, um zu bestimmen, wer das erforderliche Subnetz bereitstellt, das zum Einrichten der BGP-Sitzung zwischen Ihrem Netzwerk und Microsoft benötigt wird. Wenn Sie das Subnetz bereitstellen können, wählen Sie *Peer* aus. Wählen Sie andernfalls **Microsoft** aus, dann wird [Microsoft Peering](mailto:peering@microsoft.com) sich mit Ihnen in Verbindung setzen. Beachten Sie, dass bei Auswahl dieser Option Microsoft mehr Zeit für die Verarbeitung der Peeringanforderung benötigt. In einigen Fällen ist Microsoft möglicherweise nicht in der Lage, Subnetze bereitzustellen, was zu einer Ablehnung der Anforderung führt.
        1. Wenn Sie für **Sitzungsadressanbieter** *Peer* ausgewählt haben, geben Sie die IPv4- und die IPv6-Adresse zusammen mit der Präfixmaske im Feld **IPv4-Sitzungspräfix** bzw. **IPv6-Sitzungspräfix** ein.
        1. Geben Sie die Anzahl der IPv4- und IPv6-Präfixe ein, die Sie im Feld **Maximum advertised IPv4 addresses** (Maximal bekanntgegebene IPv4-Adressen) bzw. **Maximum advertised IPv6 addresses** (Maximal bekanntgegebene IPv6-Adressen) bekanntgeben möchten.
        1. Passen Sie den Schieberegler **Bandbreite gesamt** an, sodass er die Bandbreite der Verbindung wiedergibt.
        1. Klicken Sie auf **OK**, um Ihre Verbindungseinstellungen zu speichern.

1. Wiederholen Sie den Schritt oben, um weitere Verbindungen in jeder Einrichtung hinzuzufügen, in der Microsoft gemeinsam mit Ihrem Netzwerk präsent ist, innerhalb der zuvor ausgewählten **Metro** (Metropolregion).

1. Nachdem Sie alle erforderlichen Verbindungen hinzugefügt haben, klicken Sie auf **Überprüfen + Erstellen**.

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „Peeringkonfiguration“: Abschluss](../media/setup-direct-conf-tab-final.png)

1. Beachten Sie, dass das Portal eine einfache Überprüfung der eingegebenen Informationen ausführt. Dies wird in einem Menüband oben auf der Seite angezeigt, als *Abschließende Überprüfung wird ausgeführt...* .

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „Peeringüberprüfung“](../media/setup-direct-review-tab-validation.png)

1. Nachdem sich die Anzeige in *Überprüfung bestanden* geändert hat, überprüfen Sie Ihre Informationen, und reichen Sie die Anforderung ein, indem Sie auf **Erstellen** klicken. Wenn Sie Ihre Anforderung ändern müssen, klicken Sie auf **Zurück**, und wiederholen Sie die Schritte oben.

    > [!div class="mx-imgBorder"]
    > ![Peering einreichen](../media/setup-direct-review-tab-submit.png)

1. Nachdem Sie die Anforderung übermittelt haben, warten Sie, bis die Bereitstellung beendet ist. Wenn bei der Bereitstellung ein Fehler auftritt, wenden Sie sich an [Microsoft Peering](mailto:peering@microsoft.com). Eine erfolgreiche Bereitstellung wird wie unten dargestellt angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Peering erfolgreich](../media/setup-direct-success.png)
