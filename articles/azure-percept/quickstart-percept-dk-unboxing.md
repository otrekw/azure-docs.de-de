---
title: Auspacken und Zusammensetzen der Komponenten Ihres Azure Percept DK
description: Erfahren Sie, wie Sie Ihr Azure Percept DK auspacken, verbinden und einschalten.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: quickstart
ms.date: 02/16/2021
ms.custom: template-quickstart
ms.openlocfilehash: efa255ba38f7e00785335bf458ecc0ed91da646b
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104608179"
---
# <a name="quickstart-unbox-and-assemble-your-azure-percept-dk-components"></a>Schnellstart: Auspacken und Zusammensetzen der Komponenten Ihres Azure Percept DK

Nachdem Sie Ihr Azure Percept DK erhalten haben, finden Sie in diesem Handbuch Informationen zum Verbinden der Komponenten und zum Einschalten des Geräts.

## <a name="prerequisites"></a>Voraussetzungen

- Azure Percept DK (DevKit)
- P7-Schraubendreher (optional, zur Befestigung des Netzkabelsteckers auf der Trägerplatine)

## <a name="unbox-and-assemble-your-device"></a>Auspacken und Zusammensetzen Ihres Geräts

1. Packen Sie die Komponenten des Azure Percept DK aus.

    Das DevKit enthält eine Trägerplatine, Azure Percept Vision SoM, eine Zubehörbox mit Antennen und Kabeln sowie eine Willkommenskarte mit einem Inbusschlüssel.

1. Verbinden Sie die DevKit-Komponenten.

    > [!NOTE]
    > Der Netzwerkadapterport befindet sich auf der rechten Seite der Trägerplatine. Die übrigen Ports (2 x USB-A, 1 x USB-C und 1 x Ethernet) und der Netzschalter befinden sich auf der linken Seite der Trägerplatine.

    1. Schrauben Sie beide WLAN-Antennen von Hand in die Trägerplatine.

    1. Verbinden Sie das Vision SoM über das USB-C-Kabel mit dem USB-C-Anschluss der Trägerplatine.

    1. Schließen Sie das Netzkabel an den Netzadapter an.

    1. Entfernen Sie alle Kunststoffverpackungen von den Geräten.

    1. Verbinden Sie den Netzadapter/das Netzkabel mit der Trägerplatine und einer Steckdose. Um den Stecker des Netzkabels sicher auf der Trägerplatine zu befestigen, verwenden Sie einen P7-Schraubendreher (nicht im DevKit enthalten), um die Schrauben des Steckers festzuziehen.

    1. Nach Einstecken des Netzkabels in eine Steckdose schaltet sich das Gerät automatisch ein. Der Netzschalter auf der linken Seite der Trägerplatine leuchtet dann. Warten Sie einige Zeit, bis das Gerät gestartet wird.

        > [!NOTE]
        > Der Netzschalter dient zum Ausschalten oder Neustarten des Geräts, während es an eine Steckdose angeschlossen ist. Bei einem Stromausfall wird das Gerät automatisch neu gestartet.

Im folgenden Video (0:00 bis 0:50) wird die DevKit-Assembly veranschaulicht:

</br>

> [!VIDEO https://www.youtube.com/embed/-dmcE2aQkDE]

## <a name="next-steps"></a>Nächste Schritte

Jetzt, wo Ihr DevKit angeschlossen und eingeschaltet ist, lesen Sie die [Anleitung zur Einrichtung des Azure Percept DK](./quickstart-percept-dk-set-up.md), um die Einrichtung des Geräts abzuschließen. Bei der Einrichtung können Sie Ihr DevKit mit einem WLAN-Netzwerk verbinden, eine SSH-Anmeldung einrichten, einen IoT-Hub erstellen und Ihr DevKit mit Ihrem Azure-Konto verbinden. Nachdem Sie die Einrichtung des Geräts abgeschlossen haben, können Sie mit dem Erstellen von Prototypen beginnen.