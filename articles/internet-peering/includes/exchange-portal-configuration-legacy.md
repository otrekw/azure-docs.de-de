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
ms.openlocfilehash: b8869e93a7156b24d61ac555c95b9ca7f850ae34
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "81678548"
---
1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Konfiguration** die Felder wie hier gezeigt aus.

    > [!div class="mx-imgBorder"]
    > ![Seite „Peering erstellen“ mit Peeringtyp „Exchange“](../media/setup-exchange-conf-tab.png)

    * Wählen Sie unter **Peeringtyp** die Option **Exchange** aus.
    * Wählen Sie unter **SKU** die Option **Basic Free** aus.
    * Wählen Sie den **Metro**-Standort aus, für den Sie das Peering in eine Azure-Ressource konvertieren möchten. Wenn Sie über Peeringverbindungen mit Microsoft am ausgewählten **Metro**-Standort verfügen, die nicht in eine Azure-Ressource konvertiert wurden, werden diese Verbindungen im Abschnitt **Peeringverbindungen** aufgeführt, wie unten dargestellt. Sie können diese Peeringverbindungen nun in eine Azure-Ressource konvertieren.

        > [!div class="mx-imgBorder"]
        > ![Liste der Peeringverbindungen](../media/setup-exchange-legacy-conf-tab.png)

        > [!NOTE]
        > Die Einstellungen für Legacy-Peeringverbindungen können nicht geändert werden. Wenn Sie am ausgewählten **Metro**-Standort weitere Peeringverbindungen mit Microsoft hinzufügen möchten, wählen Sie **Neu erstellen** aus. Weitere Informationen finden Sie unter [Erstellen oder Ändern einer Exchange-Peeringverbindung über das Portal](../howto-exchange-portal.md).
        >

1. Klicken Sie auf **Überprüfen + erstellen**. Beachten Sie, dass vom Portal eine grundlegende Überprüfung der eingegebenen Informationen durchgeführt wird. In einem Menüband am oberen Rand wird die Meldung *Abschließende Überprüfung wird ausgeführt* angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „Peeringüberprüfung“](../media/setup-direct-review-tab-validation.png)

1. Überprüfen Sie Ihre Informationen, nachdem die Meldung in *Überprüfung erfolgreich* geändert wurde. Übermitteln Sie die Anforderung, indem Sie **Erstellen** auswählen. Wenn Sie Ihre Anforderung ändern möchten, wählen Sie **Zurück** aus, und wiederholen Sie die Schritte.

    > [!div class="mx-imgBorder"]
    > ![Übermittlung des Peerings](../media/setup-exchange-review-tab-submit.png)

1. Nachdem Sie die Anforderung übermittelt haben, warten Sie, bis die Bereitstellung abgeschlossen wurde. Sollte bei der Bereitstellung ein Fehler auftreten, wenden Sie sich an [Microsoft Peering](mailto:peering@microsoft.com). Eine erfolgreiche Bereitstellung wird wie folgt angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Erfolgreiches Peering](../media/setup-direct-success.png)
