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
ms.openlocfilehash: cd51eca0ea4563e1b56f74677df0829669d9e177
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773725"
---
1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Konfiguration** die Felder wie unten gezeigt aus.

    > [!div class="mx-imgBorder"]
    > ![Peeringkonfiguration: Exchange](../media/setup-exchange-conf-tab.png)

    * Wählen Sie unter **Peeringtyp** die Option *Exchange* aus.
    * Wählen Sie unter **SKU** die Option *Basic Free* aus.
    * Wählen Sie unter **Metro** den Standort aus, an dem Sie Peering einrichten möchten.

        > [!NOTE]
        > Wenn Sie am unter **Metro** ausgewählten Standort bereits über Peeringverbindungen mit Microsoft verfügen und Sie das Portal zum ersten Mal zum Einrichten von Peering an diesem Standort verwenden, werden Ihre vorhandenen Peeringverbindungen im Abschnitt **Peeringverbindungen** aufgelistet, wie unten zu sehen. Microsoft konvertiert diese Peeringverbindungen automatisch in Azure-Ressourcen, sodass Sie sie zusammen mit den neuen Verbindungen an einem zentralen Ort verwalten können. Weitere Informationen finden Sie unter [Konvertieren eines älteren Austauschpeerings in eine Azure-Ressource über das Portal](../howto-legacy-exchange-portal.md).
        >

1. Klicken Sie unter **Peeringverbindungen** auf **Neu erstellen**, um für jede neue Verbindung, die Sie einrichten möchten, eine Zeile hinzuzufügen.

    * Klicken Sie für eine Zeile auf die Schaltfläche „Bearbeiten“, um Verbindungseinstellungen zu konfigurieren oder zu ändern.

        > [!div class="mx-imgBorder"]
        > ![Peeringkonfiguration: Austausch – Bearbeitung](../media/setup-exchange-conf-tab-edit.png)

    * Klicken Sie zum Löschen einer Zeile auf die Schaltfläche **...** und anschließend auf **Löschen**.

        > [!div class="mx-imgBorder"]
        > ![Peeringkonfiguration: Austausch – Bearbeitung](../media/setup-exchange-conf-tab-delete.png)

    * Pro Verbindung müssen alle Einstellungen wie unten gezeigt angegeben werden.

         > [!div class="mx-imgBorder"]
         > ![Peeringkonfiguration: Austausch – Verbindung](../media/setup-exchange-conf-tab-connection.png)

        1. Wählen Sie die **Peeringeinrichtung** aus, in der die Verbindung eingerichtet werden soll.
        1. Geben Sie in den Feldern **IPv4-Adresse** und **IPv6-Adresse** die IPv4- bzw. die IPv6-Adresse ein, die in Microsoft-Routern mithilfe des Nachbarbefehls konfiguriert wird.
        1. Geben Sie im Feld **Maximum advertised IPv4 addresses** (Maximal angekündigte IPv4-Adressen) bzw. **Maximum advertised IPv6 addresses** (Maximal angekündigte IPv6-Adressen) die Anzahl von IPv4- und IPv6-Präfixen ein, die Sie ankündigen möchten.
        1. Klicken Sie auf **OK**, um Ihre Verbindungseinstellungen zu speichern.

1. Wiederholen Sie den obigen Schritt, um weitere Verbindungen für Einrichtungen hinzuzufügen, bei denen sich Microsoft und Ihr Netzwerk am gleichen Standort befinden, den Sie zuvor unter **Metro** ausgewählt haben.

1. Nachdem Sie alle erforderlichen Verbindungen hinzugefügt haben, klicken Sie auf **Überprüfen + erstellen**.

    > [!div class="mx-imgBorder"]
    > ![Peeringkonfiguration: Letzte Registerkarte](../media/setup-exchange-conf-tab-final.png)

1. Daraufhin wird vom Portal eine grundlegende Überprüfung der eingegebenen Informationen durchgeführt. Dies wird auf einem Menüband im oberen Bereich als *Abschließende Überprüfung wird ausgeführt...* angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „Peeringüberprüfung“](../media/setup-direct-review-tab-validation.png)

1. Nachdem sich die Anzeige in *Überprüfung bestanden* geändert hat, überprüfen Sie Ihre Informationen, und übermitteln Sie die Anforderung, indem Sie auf **Erstellen** klicken. Wenn Sie Ihre Anforderung ändern müssen, klicken Sie auf **Zurück**, und wiederholen Sie die obigen Schritte.

    > [!div class="mx-imgBorder"]
    > ![Peeringübermittlung](../media/setup-exchange-review-tab-submit.png)

1. Nachdem Sie die Anforderung übermittelt haben, warten Sie, bis die Bereitstellung beendet ist. Sollte bei der Bereitstellung ein Fehler auftreten, wenden Sie sich an [Microsoft Peering](mailto:peering@microsoft.com). Eine erfolgreiche Bereitstellung sieht wie unten dargestellt aus.

    > [!div class="mx-imgBorder"]
    > ![Peering erfolgreich](../media/setup-direct-success.png)
