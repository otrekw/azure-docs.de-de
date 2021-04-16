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
ms.openlocfilehash: 632490498b8dd13414657edb9518cd543ac07af6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "81678820"
---
1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Konfiguration** die Felder wie hier gezeigt aus.

    > [!div class="mx-imgBorder"]
    > ![Seite „Peering erstellen“, Registerkarte „Konfiguration“](../media/setup-direct-conf-tab.png)

    * Wählen Sie für **Peeringtyp** **Direct** aus.
    * Wählen Sie für **Microsoft-Netzwerk** die Option **AS8075** aus. Wählen Sie nicht „ASN 8069“ aus. Diese ist für Sonderanwendungen reserviert und wird nur von [Microsoft-Peering](mailto:peering@microsoft.com) verwendet.
    * Wählen Sie unter **SKU** die Option **Basic Free** aus. Wählen Sie nicht Premium Free aus, da diese Option für Sonderanwendungen reserviert ist.
    * Wählen Sie den **Metro**-Standort aus, für den Sie das Peering in eine Azure-Ressource konvertieren möchten. Wenn Sie über Peeringverbindungen mit Microsoft am ausgewählten **Metro**-Standort verfügen, die nicht in eine Azure-Ressource konvertiert wurden, werden diese Verbindungen im Abschnitt **Peeringverbindungen** aufgeführt, wie unten dargestellt. Sie können diese Peeringverbindungen nun in eine Azure-Ressource konvertieren.

        > [!div class="mx-imgBorder"]
        > ![Liste der Peeringverbindungen](../media/setup-directlegacy-conf-tab.png)

1. Wenn Sie die Bandbreite aktualisieren müssen, wählen Sie die Schaltfläche „Bearbeiten“ für eine Zeile aus, um die Verbindungseinstellungen zu ändern.

    > [!div class="mx-imgBorder"]
    > ![Schaltfläche „Bearbeiten“](../media/setup-directlegacy-conf-tab-edit.png)

    > [!NOTE]
    > Wenn Sie am ausgewählten **Metro**-Standort weitere Peeringverbindungen mit Microsoft hinzufügen möchten, wählen Sie **Neu erstellen** aus. Weitere Informationen finden Sie unter [Erstellen oder Ändern einer Direct-Peeringverbindung über das Portal](../howto-direct-portal.md).
    >

1. Klicken Sie auf **Überprüfen + erstellen**. Beachten Sie, dass vom Portal eine grundlegende Überprüfung der eingegebenen Informationen durchgeführt wird. In einem Menüband am oberen Rand wird die Meldung *Abschließende Überprüfung wird ausgeführt* angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „Peeringüberprüfung“](../media/setup-direct-review-tab-validation.png)

1. Überprüfen Sie Ihre Informationen, nachdem die Meldung in *Überprüfung erfolgreich* geändert wurde. Übermitteln Sie die Anforderung, indem Sie **Erstellen** auswählen. Wenn Sie Ihre Anforderung ändern möchten, wählen Sie **Zurück** aus, und wiederholen Sie die Schritte.

    > [!div class="mx-imgBorder"]
    > ![Übermittlung des Peerings](../media/setup-direct-review-tab-submit.png)

1. Nachdem Sie die Anforderung übermittelt haben, warten Sie, bis die Bereitstellung abgeschlossen wurde. Sollte bei der Bereitstellung ein Fehler auftreten, wenden Sie sich an [Microsoft Peering](mailto:peering@microsoft.com). Eine erfolgreiche Bereitstellung wird wie folgt angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Erfolgreiches Peering](../media/setup-direct-success.png)
