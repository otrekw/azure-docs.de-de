---
title: include file
titleSuffix: Azure
description: include file
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: include
ms.date: 5/22/2020
ms.author: derekol
ms.openlocfilehash: e2804c8f9b1af89ac0ea86ec14136df66d900060
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "83846135"
---
1. Wählen Sie **Ressource erstellen** > **Alle anzeigen** aus.

    > [!div class="mx-imgBorder"]
    > ![Nach Peering suchen](../media/setup-seeall.png)

1. Suchen Sie im Suchfeld nach **Peering**, und drücken Sie die **EINGABETASTE** auf der Tastatur. Wählen Sie in den Ergebnissen eine **Peering**-Ressource aus.

    > [!div class="mx-imgBorder"]
    > ![Starten von Peering](../media/setup-launch.png)

1. Sehen Sie sich die Seite nach dem Start des **Peerings** an, um die Details zu verstehen. Wählen Sie **Erstellen** aus, wenn Sie fertig sind.

    > [!div class="mx-imgBorder"]
    > ![Peering erstellen](../media/setup-create.png)

1. Füllen Sie auf der Seite **Peering erstellen** auf der Registerkarte **Grundlagen** die Felder wie hier gezeigt aus.

    > [!div class="mx-imgBorder"]
    > ![Registerkarte „Grundlagen“ für das Peering](../media/setup-basics-tab.png)

    * Wählen Sie Ihr Azure-**Abonnement** aus.
    * Unter **Ressourcengruppe** können Sie in der Dropdownliste eine vorhandene Ressourcengruppe auswählen oder eine neue Gruppe erstellen, indem Sie **Neu erstellen** auswählen. Im Rahmen dieses Beispiels erstellen Sie eine neue Ressourcengruppe.
    * **Name** entspricht dem Ressourcennamen und ist frei wählbar.
    * **Region** wird automatisch ausgewählt, wenn Sie eine vorhandene Ressourcengruppe ausgewählt haben. Wenn Sie eine neue Ressourcengruppe erstellen möchten, müssen Sie auch die Azure-Region auswählen, in der sich die Ressource befinden soll.

        > [!NOTE]
        > Die Region, in der sich die Ressourcengruppe befindet, ist unabhängig vom Standort, an dem Sie das Peering mit Microsoft erstellen möchten. Es empfiehlt sich jedoch, Ihre Peeringressourcen innerhalb von Ressourcengruppen zu organisieren, die sich in nahegelegenen Azure-Regionen befinden. Beispiel: Für Peerings in Ashburn können Sie eine Ressourcengruppe in „USA, Osten“ oder in „USA, Osten 2“ erstellen.

    * Wählen Sie Ihre ASN im Feld **Peer-ASN** aus.

        > [!IMPORTANT]
        > * Sie können nur eine ASN mit dem ValidationState „Genehmigt“ auswählen, bevor Sie eine Peeringanforderung übermitteln. Wenn Sie Ihre PeerAsn-Anforderung gerade erst übermittelt haben, warten Sie ca. 12 Stunden, damit die ASN-Zuordnung genehmigt werden kann. Wenn die Genehmigung der von Ihnen ausgewählten ASN noch aussteht, wird eine Fehlermeldung angezeigt. 
        > * Wenn die benötigte ASN nicht angezeigt wird, überprüfen Sie, ob Sie das richtige Abonnement ausgewählt haben. Ist dies der Fall, überprüfen Sie mithilfe von [Zuordnen der Peer-ASN zum Azure-Abonnement](../howto-subscription-association-portal.md), ob Sie bereits eine PeerAsn erstellt haben.

        > [!div class="mx-imgBorder"]
        > ![Peering-Grundlagen ausgefüllt](../media/setup-direct-basics-filled-tab.png)

    * Wählen Sie **Weiter: Konfiguration >** , um fortzufahren.
