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
ms.openlocfilehash: 2e8938f270ed175c687d975b0b248275ad92f8e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "75773905"
---
1. Navigieren Sie zu **Ressourcengruppen**, und klicken Sie auf die Ressourcengruppe, die Sie beim Erstellen der **Peeringressource** ausgewählt haben. Sollten zu viele Ressourcengruppen aufgeführt werden, können Sie das Feld *Filter* verwenden.

    > [!div class="mx-imgBorder"]
    > ![Peeringressourcengruppe](../media/setup-direct-get-resourcegroup.png)

1. Klicken Sie auf die von Ihnen erstellte **Peeringressource**.

    > [!div class="mx-imgBorder"]
    > ![Peeringressourcenansicht](../media/setup-direct-get-open.png)

1. Auf der Seite **Übersicht** werden allgemeine Informationen angezeigt. Sehen Sie sich die unten hervorgehobenen Informationen an.

    > [!div class="mx-imgBorder"]
    > ![Peeringressourcenansicht](../media/setup-direct-get-overview.png)

1. Klicken Sie auf der linken Seite auf **ASN-Informationen**, um die beim Erstellen von „PeerAsn“ übermittelten Informationen anzuzeigen.

    > [!div class="mx-imgBorder"]
    > ![Peeringressourcenansicht](../media/setup-direct-get-asninfo.png)

1. Klicken Sie auf der linken Seite auf **Verbindungen**. Im oberen Bereich finden Sie eine Zusammenfassung der Peeringverbindungen zwischen Ihrer ASN und Microsoft über verschiedene Einrichtungen innerhalb der Metropolregion hinweg. Sie können auch von der Seite **Übersicht** aus zur Verbindungszusammenfassung navigieren, indem Sie im mittleren Bereich auf **Verbindungen** klicken, wie oben dargestellt.

    > [!div class="mx-imgBorder"]
    > ![Peeringressourcenansicht](../media/setup-direct-get-connectionssummary.png)

    * **Verbindungsstatus** gibt den Status der eingerichteten Peeringverbindung an. Die in diesem Feld angezeigten Zustände entsprechen dem Statusdiagramm, das in der [exemplarischen Vorgehensweise zum Direct Peering](../walkthrough-direct-all.md) abgebildet ist.
    * **IPv4-Sitzungsstatus** und **IPv6-Sitzungsstatus** entsprechen den Statuswerten der IPv4- bzw. IPv6-BGP-Sitzung.  
    * Wenn Sie oben eine Zeile auswählen, werden im Abschnitt ***Verbindung*** im unteren Bereich Details zu den einzelnen Verbindungen angezeigt. Sie können auf die Pfeilmarkierungen klicken, um die Unterabschnitte ***Konfiguration***, ***IPv4-Adresse*** und ***IPv6-Adresse*** zu erweitern.
