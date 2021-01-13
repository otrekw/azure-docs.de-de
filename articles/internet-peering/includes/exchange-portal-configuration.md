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
ms.openlocfilehash: 268703081a2a40e8bcc665889eaeaf8edd673bfd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "81680946"
---
1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Konfiguration** die Felder wie hier gezeigt aus.

    > [!div class="mx-imgBorder"]
    > ![Seite „Peering erstellen“ mit Peeringtyp „Exchange“](../media/setup-exchange-conf-tab.png)

    * Wählen Sie unter **Peeringtyp** die Option **Exchange** aus.
    * Wählen Sie unter **SKU** die Option **Basic Free** aus.
    * Wählen Sie den **Metro**-Standort aus, an dem Sie Peering einrichten möchten.

        > [!NOTE]
        > Wenn Sie bereits über Peeringverbindungen mit Microsoft am ausgewählten **Metro**-Standort verfügen und Sie das Portal zum ersten Mal zum Einrichten von Peering an diesem Speicherort verwenden, werden Ihre vorhandenen Peeringverbindungen im Abschnitt **Peeringverbindungen** aufgelistet, wie unten dargestellt. Microsoft konvertiert diese Peeringverbindungen automatisch in Azure-Ressourcen, sodass Sie sie zusammen mit den neuen Verbindungen an einem zentralen Ort verwalten können. Weitere Informationen finden Sie unter [Konvertieren einer Legacy-Instanz für Exchange Peering in eine Azure-Ressource über das Azure-Portal](../howto-legacy-exchange-portal.md).
        >

1. Wählen Sie unter **Peeringverbindungen** die Option **Neu erstellen** aus, um für jede neue Verbindung, die Sie einrichten möchten, eine Zeile hinzuzufügen.

    * Wählen Sie für eine Zeile die Schaltfläche „Bearbeiten“ aus, um die Verbindungseinstellungen zu konfigurieren oder zu ändern.

        > [!div class="mx-imgBorder"]
        > ![Schaltfläche „Bearbeiten“](../media/setup-exchange-conf-tab-edit.png)

    * Um eine Zeile zu löschen, wählen Sie **...**  > **Löschen** aus.

        > [!div class="mx-imgBorder"]
        > ![Schaltfläche „Löschen“](../media/setup-exchange-conf-tab-delete.png)

    * Pro Verbindung müssen alle Einstellungen wie im Folgenden gezeigt angegeben werden.

         > [!div class="mx-imgBorder"]
         > ![Seite für Exchange-Peeringverbindungen](../media/setup-exchange-conf-tab-connection.png)

        1. Wählen Sie die **Peeringeinrichtung** aus, in der die Verbindung eingerichtet werden soll.
        1. Geben Sie in den Feldern **IPv4-Adresse** und **IPv6-Adresse** die IPv4- bzw. IPv6-Adresse ein, die in Microsoft-Routern mithilfe des Nachbarbefehls konfiguriert wird.
        1. Geben Sie im Feld **Maximum advertised IPv4 addresses** (Maximal angekündigte IPv4-Adressen) bzw. **Maximum advertised IPv6 addresses** (Maximal angekündigte IPv6-Adressen) die Anzahl von IPv4- und IPv6-Präfixen ein, die Sie ankündigen möchten.
        1. Wählen Sie **OK** aus, um Ihre Verbindungseinstellungen zu speichern.

1. Wiederholen Sie den Schritt, um weitere Verbindungen für Einrichtungen hinzuzufügen, bei denen sich Microsoft und Ihr Netzwerk am gleichen Standort befinden, den Sie zuvor unter **Metro** ausgewählt haben.

1. Nachdem Sie alle erforderlichen Verbindungen hinzugefügt haben, wählen Sie **Überprüfen + erstellen** aus.

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „Konfiguration“ für das Peering](../media/setup-exchange-conf-tab-final.png)

1. Beachten Sie, dass vom Portal eine grundlegende Überprüfung der eingegebenen Informationen durchgeführt wird. In einem Menüband am oberen Rand wird die Meldung *Abschließende Überprüfung wird ausgeführt* angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „Peeringüberprüfung“](../media/setup-direct-review-tab-validation.png)

1. Überprüfen Sie Ihre Informationen, nachdem die Meldung in *Überprüfung erfolgreich* geändert wurde. Übermitteln Sie die Anforderung, indem Sie **Erstellen** auswählen. Wenn Sie Ihre Anforderung ändern möchten, wählen Sie **Zurück** aus, und wiederholen Sie die Schritte.

    > [!div class="mx-imgBorder"]
    > ![Übermittlung des Peerings](../media/setup-exchange-review-tab-submit.png)

1. Nachdem Sie die Anforderung übermittelt haben, warten Sie, bis die Bereitstellung abgeschlossen wurde. Sollte bei der Bereitstellung ein Fehler auftreten, wenden Sie sich an [Microsoft Peering](mailto:peering@microsoft.com). Eine erfolgreiche Bereitstellung wird wie folgt angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Erfolgreiches Peering](../media/setup-direct-success.png)
