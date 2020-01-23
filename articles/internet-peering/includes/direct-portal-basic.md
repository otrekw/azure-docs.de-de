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
ms.openlocfilehash: 79cf4b2edd1a25df427cf15f9ee7f2f331ebd2df
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773769"
---
1. Klicken Sie auf **Ressource erstellen** > **Alle anzeigen**.

    > [!div class="mx-imgBorder"]
    > ![Nach Peering suchen](../media/setup-seeall.png)

1. Suchen Sie im Suchfeld nach *Peering*, und drücken Sie die *EINGABETASTE* auf der Tastatur. Klicken Sie in den Ergebnissen auf die **Peering**-Ressource.

    > [!div class="mx-imgBorder"]
    > ![Starten von Peering](../media/setup-launch.png)

1. Nachdem **Peering** gestartet wurde, überprüfen Sie die Seite, um die Details zu verstehen. Wenn Sie fertig sind, klicken Sie auf **Erstellen**.

    > [!div class="mx-imgBorder"]
    > ![Peering erstellen](../media/setup-create.png)

1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Grundlagen** die Felder wie unten gezeigt aus.

    > [!div class="mx-imgBorder"]
    > ![Peering-Grundlagen](../media/setup-basics-tab.png)

    * Wählen Sie Ihr Azure-**Abonnement**.
    * Unter **Ressourcengruppe** können Sie entweder im Dropdownmenü eine vorhandene Ressourcengruppe auswählen oder eine neue Gruppe erstellen, indem Sie auf **Neu erstellen** klicken. Wir erstellen im Rahmen dieses Beispiels eine neue Ressourcengruppe.
    * **Name** entspricht dem Ressourcennamen und ist frei wählbar.
    * **Region** wird automatisch ausgewählt, wenn Sie im Schritt oben eine vorhandene Ressourcengruppe ausgewählt haben. Wenn Sie eine neue Ressourcengruppe erstellen möchten, müssen Sie auch die Azure-Region auswählen, in der sich die Ressource befinden soll. East US

        > [!NOTE]
        > Die Region, in der sich die Ressourcengruppe befindet, ist unabhängig von dem Standort, an dem Sie Peering mit Microsoft erstellen möchten. Es empfiehlt sich jedoch, ihre Peeringressourcen innerhalb von Ressourcengruppen zu organisieren, die sich in den nächstgelegenen Azure-Regionen befinden. Beispiel: Für Peerings in Ashburn können Sie eine Ressourcengruppe in *USA, Osten* oder in *USA, Osten 2* erstellen.

    * Wählen Sie Ihre ASN im Feld **Peer-ASN** aus.

        > [!IMPORTANT]
        > * Sie können nur eine ASN mit ValidationState „Genehmigt“ auswählen, bevor Sie eine Peeringanforderung senden. Wenn Sie Ihre PeerAsn-Anforderung gerade erst übermittelt haben, warten Sie ca. 12 Stunden, damit die ASN-Zuordnung „Genehmigt“ werden kann. Wenn die Genehmigung der von Ihnen ausgewählten ASN noch aussteht, wird eine Fehlermeldung angezeigt. 
        > * Wenn die benötigte ASN nicht angezeigt wird, überprüfen Sie, ob Sie das richtige Abonnement ausgewählt haben. Ist dies der Fall, überprüfen Sie mithilfe von [Zuordnen der Peer-ASN zum Azure-Abonnement](../howto-subscription-association-portal.md), ob Sie bereits eine PeerAsn erstellt haben.

        > [!div class="mx-imgBorder"]
        > ![Peering-Grundlagen ausgefüllt](../media/setup-direct-basics-filled-tab.png)

    * Klicken Sie auf **Weiter: Konfiguration >** , um fortzufahren.
