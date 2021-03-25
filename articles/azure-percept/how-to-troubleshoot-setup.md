---
title: Behandeln von Problemen während des Onboardings von Azure Percept DK
description: Hier finden Sie Tipps zur Problembehandlung für einige der häufigsten Probleme während des Onboardings.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: d71cfa6ba52052e4b68175be84934c8b4294ed25
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101660366"
---
# <a name="azure-percept-dk-onboarding-experience-troubleshooting-guide"></a>Leitfaden zur Problembehandlung beim Onboarding von Azure Percept DK

Nachfolgend sind einige Probleme beschrieben, die beim Onboarding von Azure Percept DK auftreten können. Sollte nach dem Ausführen der Schritte in diesem Handbuch das Problem weiterhin bestehen, wenden Sie sich an den Azure-Kundensupport.

|Problem|`Reason`|Problemumgehung|
|:-----|:------|:----------|
|Beim Herstellen einer Verbindung mit den Seiten für die Azure-Kontoregistrierung oder mit dem Azure-Portal können Sie sich automatisch mit einem zwischengespeicherten Konto anmelden. Handelt es sich dabei nicht um das vorgesehene Konto, ergeben sich möglicherweise Inkonsistenzen mit der Dokumentation.|Dies liegt in der Regel an einer Browsereinstellung, mit der Sie ein zuvor verwendetes Konto speichern können.|Klicken Sie auf der Azure-Seite in der Ecke oben rechts, in der der Name Ihres Kontos angezeigt wird, und klicken Sie dann auf „Abmelden“. Anschließend können Sie sich mit dem richtigen Konto anmelden.|
|Das Netzwerk des Azure Percept DK-Zugriffspunkts (scz-xxxx) wird nicht in der Liste der verfügbaren WLANs angezeigt.|Dabei handelt es sich normalerweise um ein temporäres Problem, das sich nach einiger Zeit von selbst löst.|Warten Sie darauf, dass das Netzwerk angezeigt wird. Wird es nach mehr als 15 Minuten nicht angezeigt, starten Sie das Gerät neu.|
|Die Verbindung mit dem Azure Percept DK-Zugriffspunkt wird häufig unterbrochen.|Dies kann auf eine schlechte Verbindung zwischen dem Gerät und dem Hostcomputer zurückzuführen sein. Es kann auch durch Störungen durch andere WLAN-Verbindungen auf dem Hostcomputer verursacht werden.|Vergewissern Sie sich, dass die Antennen ordnungsgemäß an das DevKit angeschlossen wurden. Ist das DevKit weit vom Hostcomputer entfernt, sollten Sie es näher am Hostcomputer platzieren. Deaktivieren Sie alle anderen Internetverbindungen (etwa LTE/5G), wenn diese auf dem Hostcomputer hergestellt wurden.|
|Auf dem Hostcomputer wird eine Sicherheitswarnung bezüglich der Verbindung mit dem Azure Percept DK-Zugriffspunkt angezeigt.|Dies ist ein bekanntes Problem, das in einem späteren Update behoben wird.|Es ist sicher, das Onboarding über den WLAN-Zugriffspunkt des DevKits fortzusetzen.|
|Das Netzwerk des Azure Percept DK-Zugriffspunkts (scz-xxxx) wird in der Netzwerkliste angezeigt, es kann aber keine Verbindung hergestellt werden.|Die Ursache kann eine vorübergehende Störung des WLAN-Zugriffspunkts des DevKits sein.|Starten Sie das DevKit neu, und versuchen Sie es noch einmal.|
|Während des Setups kann keine Verbindung mit einem WLAN hergestellt werden.|Das WLAN muss aktuell über eine Internetverbindung verfügen, damit die Kommunikation mit Azure möglich ist. EAP[PEAP/MSCHAP], Captive Portals und Enterprise EAP-TLS-Konnektivität werden derzeit nicht unterstützt.|Stellen Sie sicher, dass der WLAN-Typ, mit dem Sie eine Verbindung herstellen, unterstützt wird und über Internetkonnektivität verfügt.|