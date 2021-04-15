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
ms.openlocfilehash: fda22346a44388248e37473bc7891b8a130569c4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "81681065"
---
1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Konfiguration** die Felder wie hier gezeigt aus.

    * Wählen Sie für **Peeringtyp** **Direct** aus.
    * Wählen Sie für **Microsoft-Netzwerk** die Option **AS8075** aus. Erstellen Sie kein Peering mit ASN 8069. Diese ist für Sonderanwendungen reserviert und wird nur von [Microsoft-Peering](mailto:peering@microsoft.com) verwendet.
    * Wählen Sie unter **SKU** die Option **Basic Free** aus. Wählen Sie nicht Premium Free aus, da diese Option für Sonderanwendungen reserviert ist.
    * Wählen Sie den **Metro**-Standort aus, an dem Sie Peering einrichten möchten.

        > [!NOTE]
        > Wenn Sie bereits über Peeringverbindungen mit Microsoft am ausgewählten **Metro**-Standort verfügen und Sie das Azure-Portal zum ersten Mal zum Einrichten von Peering an diesem Speicherort verwenden, werden Ihre vorhandenen Peeringverbindungen im Abschnitt **Peeringverbindungen** aufgelistet, wie unten dargestellt. Microsoft konvertiert diese Peeringverbindungen automatisch in Azure-Ressourcen, sodass Sie sie zusammen mit den neuen Verbindungen an einem zentralen Ort verwalten können. Weitere Informationen finden Sie unter [Konvertieren einer älteren Instanz für Direct-Peering in eine Azure-Ressource über das Portal](../howto-legacy-direct-portal.md).
        >

1. Wählen Sie unter **Peeringverbindungen** die Option **Neu erstellen** aus, um für jede neue Verbindung, die Sie einrichten möchten, eine Zeile hinzuzufügen.

    * Wählen Sie für eine Zeile die Schaltfläche „Bearbeiten“ aus, um die Verbindungseinstellungen zu konfigurieren oder zu ändern.

        > [!div class="mx-imgBorder"]
        > ![Schaltfläche „Bearbeiten“](../media/setup-direct-conf-tab-edit.png)
    
    * Um eine Zeile zu löschen, wählen Sie **...**  > **Löschen** aus.

        > [!div class="mx-imgBorder"]
        > ![Schaltfläche „Löschen“](../media/setup-direct-conf-tab-delete.png)

    * Pro Verbindung müssen alle Einstellungen wie im Folgenden gezeigt angegeben werden.

         > [!div class="mx-imgBorder"]
         > ![Seite für Direct-Peeringverbindungen](../media/setup-direct-conf-tab-connection.png)

        1. Wählen Sie die **Peeringeinrichtung** aus, in der die Verbindung eingerichtet werden soll.
        1. **Sitzungsadressanbieter** wird verwendet, um zu bestimmen, wer das erforderliche Subnetz bereitstellt, das zum Einrichten der BGP-Sitzung zwischen Ihrem Netzwerk und Microsoft benötigt wird. Wenn Sie das Subnetz angeben können, wählen Sie **Peer** aus. Wählen Sie andernfalls **Microsoft** aus, dann wird sich das [Microsoft-Peering-Team](mailto:peering@microsoft.com) mit Ihnen in Verbindung setzen. Bei Auswahl dieser Option benötigt Microsoft mehr Zeit für die Verarbeitung der Peeringanforderung. In einigen Fällen ist Microsoft möglicherweise nicht in der Lage, Subnetze bereitzustellen, sodass die Anforderung abgelehnt wird.
        1. Wenn Sie für **Sitzungsadressanbieter** die Option **Peer** ausgewählt haben, geben Sie die IPv4- und die IPv6-Adresse zusammen mit der Präfixmaske im Feld **IPv4-Sitzungspräfix** bzw. **IPv6-Sitzungspräfix** ein.
        1. Geben Sie im Feld **Maximum advertised IPv4 addresses** (Maximal angekündigte IPv4-Adressen) bzw. **Maximum advertised IPv6 addresses** (Maximal angekündigte IPv6-Adressen) die Anzahl von IPv4- und IPv6-Präfixen ein, die Sie ankündigen möchten.
        1. Passen Sie den Schieberegler **Bandbreite gesamt** an, sodass er die Bandbreite der Verbindung wiedergibt.
        1. Wählen Sie **Speichern** aus, um Ihre Verbindungseinstellungen zu speichern.

1. Wiederholen Sie den obigen Schritt, um weitere Verbindungen für Einrichtungen hinzuzufügen, bei denen sich Microsoft und Ihr Netzwerk am gleichen Standort befinden, den Sie zuvor unter **Metro** ausgewählt haben.

1. Nachdem Sie alle erforderlichen Verbindungen hinzugefügt haben, wählen Sie **Überprüfen + erstellen** aus.

    > [!div class="mx-imgBorder"]
    > ![Peeringkonfiguration: letzte Registerkarte](../media/setup-direct-conf-tab-final.png)

1. Beachten Sie, dass vom Portal eine grundlegende Überprüfung der eingegebenen Informationen durchgeführt wird. In einem Menüband am oberen Rand wird die Meldung *Abschließende Überprüfung wird ausgeführt* angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „Peeringüberprüfung“](../media/setup-direct-review-tab-validation.png)

1. Überprüfen Sie Ihre Informationen, nachdem die Meldung in *Überprüfung erfolgreich* geändert wurde. Übermitteln Sie die Anforderung, indem Sie **Erstellen** auswählen. Wenn Sie Ihre Anforderung ändern möchten, wählen Sie **Zurück** aus, und wiederholen Sie die Schritte.

    > [!div class="mx-imgBorder"]
    > ![Übermittlung des Peerings](../media/setup-direct-review-tab-submit.png)

1. Nachdem Sie die Anforderung übermittelt haben, warten Sie, bis die Bereitstellung abgeschlossen wurde. Sollte bei der Bereitstellung ein Fehler auftreten, wenden Sie sich an [Microsoft Peering](mailto:peering@microsoft.com). Eine erfolgreiche Bereitstellung wird wie folgt angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Erfolgreiches Peering](../media/setup-direct-success.png)
