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
ms.openlocfilehash: 7d7b9f847cdcc4ab4b1ff065425eebe07fb4d888
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773945"
---
1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Konfiguration** die Felder wie unten gezeigt aus.

    > [!div class="mx-imgBorder"]
    > ![Peeringkonfiguration: Exchange](../media/setup-exchange-conf-tab.png)

    * Wählen Sie unter **Peeringtyp** die Option *Exchange* aus.
    * Wählen Sie unter **SKU** die Option *Basic Free* aus.
    * Wählen Sie unter **Metro** den Standort aus, für den Sie das Peering in eine Azure-Ressource konvertieren möchten. Wenn Sie über Peeringverbindungen mit Microsoft am ausgewählten **Metro**-Standort verfügen, die nicht in eine Azure-Ressource konvertiert wurden, werden diese Verbindungen im Abschnitt **Peeringverbindungen** aufgeführt, wie unten dargestellt. Sie können diese Peeringverbindungen nun in Azure-Ressourcen konvertieren.

        > [!div class="mx-imgBorder"]
        > ![Peeringkonfiguration: Exchange –  Legacy-Verbindungen](../media/setup-exchange-legacy-conf-tab.png)

        > [!NOTE]
        > Die Einstellungen für Legacy-Peeringverbindungen können nicht geändert werden. Wenn Sie am ausgewählten **Metro**-Standort weitere Peeringverbindungen mit Microsoft hinzufügen möchten, können Sie das tun, indem Sie auf die Schaltfläche **Neue erstellen** klicken. Weitere Informationen finden Sie unter [Erstellen oder Ändern einer Instanz für Exchange Peering über das Portal](../howto-exchange-portal.md).
        >

1. Klicken Sie auf **Überprüfen + erstellen**. Daraufhin wird vom Portal eine grundlegende Überprüfung der eingegebenen Informationen durchgeführt. Dies wird auf einem Menüband im oberen Bereich als *Abschließende Überprüfung wird ausgeführt...* angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „Peeringüberprüfung“](../media/setup-direct-review-tab-validation.png)

1. Nachdem sich die Anzeige in *Überprüfung bestanden* geändert hat, überprüfen Sie Ihre Informationen, und übermitteln Sie die Anforderung, indem Sie auf **Erstellen** klicken. Wenn Sie Ihre Anforderung ändern müssen, klicken Sie auf **Zurück**, und wiederholen Sie die obigen Schritte.

    > [!div class="mx-imgBorder"]
    > ![Peeringübermittlung](../media/setup-exchange-review-tab-submit.png)

1. Nachdem Sie die Anforderung übermittelt haben, warten Sie, bis die Bereitstellung beendet ist. Sollte bei der Bereitstellung ein Fehler auftreten, wenden Sie sich an [Microsoft Peering](mailto:peering@microsoft.com). Eine erfolgreiche Bereitstellung sieht wie unten dargestellt aus.

    > [!div class="mx-imgBorder"]
    > ![Peering erfolgreich](../media/setup-direct-success.png)
