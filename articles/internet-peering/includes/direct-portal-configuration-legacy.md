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
ms.openlocfilehash: 16909e0a5d59b0ae019d91aad00e0168b0c5f433
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75773901"
---
1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Konfiguration** die Felder wie unten gezeigt aus.

    > [!div class="mx-imgBorder"]
    > ![Peeringkonfiguration – Direct](../media/setup-direct-conf-tab.png)

    * Wählen Sie für **Peeringtyp** *Direct* aus.
    * Wählen Sie für **Microsoft-Netzwerk** *AS8075* aus. Wählen Sie nicht ASN 8069 aus. Sie ist für Sonderanwendungen reserviert und wird nur von [Microsoft Peering](mailto:peering@microsoft.com) verwendet.
    * Wählen Sie unter **SKU** die Option *Basic Free* aus. Wählen Sie nicht *Premium Free* aus, da diese Option für Sonderanwendungen reserviert ist.
    * Wählen Sie unter **Metro** den Standort aus, für den Sie das Peering in eine Azure-Ressource konvertieren möchten. Wenn Sie über Peeringverbindungen mit Microsoft am ausgewählten **Metro**-Standort verfügen, die nicht in eine Azure-Ressource konvertiert wurden, werden diese Verbindungen im Abschnitt **Peeringverbindungen** aufgeführt, wie unten dargestellt. Sie können diese Peeringverbindungen nun in Azure-Ressourcen konvertieren.

        > [!div class="mx-imgBorder"]
        > ![Peeringkonfiguration – Direct – Legacy-Verbindungen](../media/setup-directlegacy-conf-tab.png)

1. Wenn Sie die Bandbreite aktualisieren müssen, klicken Sie auf die Bearbeiten-Schaltfläche für eine Zeile, wie unten hervorgehoben, um die Verbindungseinstellungen zu ändern.

    > [!div class="mx-imgBorder"]
    > ![Peeringkonfiguration – Direkte Bearbeitung](../media/setup-directlegacy-conf-tab-edit.png)

    > [!NOTE]
    > Wenn Sie am ausgewählten **Metro**-Standort weitere Peeringverbindungen mit Microsoft hinzufügen möchten, können Sie das tun, indem Sie auf die Schaltfläche **Neue erstellen** klicken. Weitere Informationen finden Sie unter [Erstellen oder Ändern einer Instanz für Direct Peering über das Portal](../howto-direct-portal.md).
    >

1. Klicken Sie auf **Überprüfen + erstellen**. Daraufhin wird vom Portal eine grundlegende Überprüfung der eingegebenen Informationen durchgeführt. Dies wird auf einem Menüband im oberen Bereich als *Abschließende Überprüfung wird ausgeführt...* angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „Peeringüberprüfung“](../media/setup-direct-review-tab-validation.png)

1. Nachdem sich die Anzeige in *Überprüfung bestanden* geändert hat, überprüfen Sie Ihre Informationen, und übermitteln Sie die Anforderung, indem Sie auf **Erstellen** klicken. Wenn Sie Ihre Anforderung ändern müssen, klicken Sie auf **Zurück**, und wiederholen Sie die obigen Schritte.

    > [!div class="mx-imgBorder"]
    > ![Peeringübermittlung](../media/setup-direct-review-tab-submit.png)

1. Nachdem Sie die Anforderung übermittelt haben, warten Sie, bis die Bereitstellung beendet ist. Sollte bei der Bereitstellung ein Fehler auftreten, wenden Sie sich an [Microsoft Peering](mailto:peering@microsoft.com). Eine erfolgreiche Bereitstellung sieht wie unten dargestellt aus.

    > [!div class="mx-imgBorder"]
    > ![Peering erfolgreich](../media/setup-direct-success.png)
