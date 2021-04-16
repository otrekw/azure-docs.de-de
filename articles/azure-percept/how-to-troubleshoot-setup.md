---
title: Beheben von Problemen bei der Einrichtung von Azure Percept DK
description: Hier finden Sie Tipps zur Problembehandlung für einige der häufigsten Probleme während der Einrichtung.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 03/25/2021
ms.custom: template-how-to
ms.openlocfilehash: 7ce13cedff9afc25900c0bf75359ae49cc29fe19
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105608492"
---
# <a name="azure-percept-dk-setup-experience-troubleshooting-guide"></a>Leitfaden zur Problembehandlung beim Einrichten von Azure Percept DK

In der folgenden Tabelle finden Sie Problemumgehungen für häufige Probleme, die während der [Einrichtung von Azure Percept DK](./quickstart-percept-dk-set-up.md) auftreten. Wenn das Problem weiterhin besteht, wenden Sie sich an den Azure-Kundensupport.

|Problem|`Reason`|Problemumgehung|
|:-----|:------|:----------|
|Beim Herstellen einer Verbindung mit den Seiten für die Azure-Kontoregistrierung oder mit dem Azure-Portal können Sie sich automatisch mit einem zwischengespeicherten Konto anmelden. Handelt es sich dabei nicht um das vorgesehene Konto, ergeben sich möglicherweise Inkonsistenzen mit der Dokumentation.|Dies liegt in der Regel an einer Browsereinstellung, mit der Sie ein zuvor verwendetes Konto speichern können.|Klicken Sie auf der Azure-Seite auf Ihren Kontonamen in der oberen rechten Ecke, und wählen Sie **Abmelden** aus. Sie können sich dann mit dem richtigen Konto anmelden.|
|Das Netzwerk des Azure Percept DK-WLAN-Zugriffspunkts (scz-xxxx oder apd-xxxx) wird nicht in der Liste der verfügbaren WLAN-Netzwerke angezeigt.|Dies ist normalerweise ein temporäres Problem, das innerhalb von 15 Minuten behoben wird.|Warten Sie darauf, dass das Netzwerk angezeigt wird. Wird es nach mehr als 15 Minuten nicht angezeigt, starten Sie das Gerät neu.|
|Die Verbindung mit dem Azure Percept DK-WLAN-Zugriffspunkt wird häufig unterbrochen.|Dies kann auf eine schlechte Verbindung zwischen dem Gerät und dem Hostcomputer zurückzuführen sein. Es kann auch durch Störungen durch andere WLAN-Verbindungen auf dem Hostcomputer verursacht werden.|Vergewissern Sie sich, dass die Antennen ordnungsgemäß an das DevKit angeschlossen wurden. Ist das DevKit weit vom Hostcomputer entfernt, sollten Sie es näher am Hostcomputer platzieren. Deaktivieren Sie alle anderen Internetverbindungen (etwa LTE/5G), wenn diese auf dem Hostcomputer hergestellt wurden.|
|Auf dem Hostcomputer wird eine Sicherheitswarnung bezüglich der Verbindung mit dem Azure Percept DK-Zugriffspunkt angezeigt.|Dies ist ein bekanntes Problem, das in einem späteren Update behoben wird.|Es ist sicher, mit der Einrichtung fortzufahren.|
|Das Netzwerk des Azure Percept DK-WLAN-Zugriffspunkts (scz-xxxx oder apd-xxxx) wird in der Netzwerkliste angezeigt, es kann aber keine Verbindung hergestellt werden.|Die Ursache kann eine vorübergehende Störung des WLAN-Zugriffspunkts des DevKits sein.|Starten Sie das DevKit neu, und versuchen Sie es noch einmal.|
|Während des Setups kann keine Verbindung mit einem WLAN hergestellt werden.|Das WLAN muss aktuell über eine Internetverbindung verfügen, damit die Kommunikation mit Azure möglich ist. EAP[PEAP/MSCHAP], Captive Portals und Enterprise EAP-TLS-Konnektivität werden derzeit nicht unterstützt.|Stellen Sie sicher, dass Ihr WLAN-Netzwerktyp unterstützt wird und eine Internetverbindung besteht.|