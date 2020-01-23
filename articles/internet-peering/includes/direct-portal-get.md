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
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75773905"
---
1. Wechseln Sie zu **Ressourcengruppen**, und klicken Sie auf die Ressourcengruppe, die Sie beim Erstellen der **Peering**-Ressource ausgewählt haben. Sie können den *Filter* verwenden, wenn zu viele Ressourcengruppen aufgeführt werden.

    > [!div class="mx-imgBorder"]
    > ![Peeringressourcengruppe](../media/setup-direct-get-resourcegroup.png)

1. Klicken Sie auf die von Ihnen erstellte **Peering**-Ressource.

    > [!div class="mx-imgBorder"]
    > ![Peeringressourcenansicht](../media/setup-direct-get-open.png)

1. Auf der Seite **Übersicht** werden allgemeine Informationen angezeigt. Sehen Sie sich die unten hervorgehobenen Informationen an.

    > [!div class="mx-imgBorder"]
    > ![Peeringressourcenansicht](../media/setup-direct-get-overview.png)

1. Klicken Sie auf der linken Seite auf **ASN-Informationen**, um die beim Erstellen von PeerAsn übermittelten Informationen anzuzeigen.

    > [!div class="mx-imgBorder"]
    > ![Peeringressourcenansicht](../media/setup-direct-get-asninfo.png)

1. Klicken Sie auf der linken Seite auf **Verbindungen**. Oben können Sie eine Zusammenfassung der Peeringverbindungen zwischen Ihrer ASN und Microsoft über verschiedene Einrichtungen innerhalb der Metropolregion hinweg betrachten. Außerdem können Sie von der Seite **Übersicht** aus zur Verbindungszusammenfassung gelangen, indem Sie im mittleren Bereich auf **Verbindungen** klicken, wie oben dargestellt.

    > [!div class="mx-imgBorder"]
    > ![Peeringressourcenansicht](../media/setup-direct-get-connectionssummary.png)

    * **Verbindungsstatus** entspricht dem Zustand der eingerichteten Peeringverbindung. Die in diesem Feld angezeigten Zustände entsprechen dem Statusdiagramm, das in der [exemplarischen Vorgehensweise zum Direct Peering](../walkthrough-direct-all.md) abgebildet ist.
    * **IPv4-Sitzungsstatus** und **IPv6-Sitzungsstatus** entsprechen den Zuständen der IPv4- bzw. IPv6-BGP-Sitzungen.  
    * Wenn Sie oben eine Zeile auswählen, werden im Abschnitt ***Verbindung*** im unteren Bereich Details zu den einzelnen Verbindungen angezeigt. Sie können auf die Pfeilmarkierungen klicken, um die Unterabschnitte ***Konfiguration***, ***IPv4-Adresse*** und ***IPv6-Adresse*** zu erweitern.
