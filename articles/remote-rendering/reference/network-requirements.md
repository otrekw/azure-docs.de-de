---
title: Netzwerkanforderungen
description: Anforderungen an und bewährte Methoden für Netzwerke, um optimale Leistungen zu erzielen
author: florianborn71
ms.author: flborn
ms.date: 02/13/2020
ms.topic: reference
ms.openlocfilehash: fd8686cc396d5fcee20590fbac8bccaf187b024d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "101735946"
---
# <a name="network-requirements"></a>Netzwerkanforderungen

Eine stabile Netzwerkverbindung mit geringer Latenz zu einem Azure-Rechenzentrum ist wichtig für die Benutzerfreundlichkeit von Azure Remote Rendering. Schlechte Netzwerkbedingungen können dazu führen, dass bei der Aktualisierung des serverseitigen Szenengraphs Verbindungen unterbrochen oder instabile, unregelmäßige oder springende Hologramme und spürbare Verzögerungen auftreten.

## <a name="guidelines-for-network-connectivity"></a>Richtlinien für die Netzwerkkonnektivität

Die genauen Netzwerkanforderungen variieren je nach Anwendungsfall und sind z. B. von der Anzahl und der Häufigkeit der an dem Remoteszenegraph vorgenommenen Änderungen und der Komplexität der gerenderten Ansicht abhängig. Es gibt allerdings einige Richtlinien, die Ihnen weiterhelfen sollten:

* Ihre Internetverbindung muss beständig mindestens **40 MBit/s Downstream** und **5 MBit/s Upstream** für einzelne Benutzersitzungen von Azure Remote Rendering unterstützen, vorausgesetzt es gibt keinen konkurrierenden Datenverkehr auf dem Netzwerk. Es werden aber höhere Geschwindigkeiten empfohlen, um die bestmögliche Leistung zu erzielen. 
* Der empfohlene Netzwerktyp ist **WLAN**, da dieses niedrige Latenz, hohe Bandbreite und eine stabile Verbindung unterstützt. Einige mobile Netzwerke weisen Jitter auf, was zu einer schlechten Leistung führen kann. 
* Bei der Verwendung einer **WLAN-Bandbreite von 5 GHz** werden in der Regel bessere Ergebnisse erzielt als mit einer WLAN-Bandbreite von 2,4 GHz. Beide Bandbreien sollten allerdings ausreichen.
* Wenn sich in der Nähe andere WLAN-Netzwerke befinden, sollten Sie die Verwendung von WLAN-Kanälen vermeiden, die von diesen anderen Netzwerken verwendet werden. Sie können Netzwerküberprüfungstools wie [WifiInfoView](https://www.nirsoft.net/utils/wifi_information_view.html) verwenden, um zu überprüfen, ob die Kanäle, die Ihr WLAN-Netzwerk verwendet, frei von konkurrierendem Datenverkehr sind.
* Vermeiden Sie generell die **Verwendung von WLAN-Repeatern** und die Weiterleitung über LAN-Anschlüsse.
* **Vermeiden Sie konkurrierenden Datenverkehr mit hoher Bandbreite** im selben Netzwerk, der z. B. durch das Streaming von Videos oder Computerspielen entsteht.
* Wenn Sie über mehrere Geräte auf demselben Zugriffspunkt verfügen, erhöhen sich die Anforderungen dementsprechend. Wenn Sie über mehrere Zugriffspunkte in einer Umgebung verfügen, führen Sie den Lastenausgleich für Geräte über die Zugriffspunkte aus, damit diese gleichmäßig verteilt werden.
* Ein gutes **WLAN-Signal** ist essenziell. Wenn möglich, sollten Sie sich in der Nähe Ihres WLAN-Zugangspunkts befinden und Hindernisse zwischen Ihrem Clientgerät und den Zugangspunkten vermeiden.
* Stellen Sie sicher, dass Sie immer eine Verbindung mit dem **nächstgelegenen Azure-Rechenzentrum** für Ihre [Region](regions.md) herstellen. Je näher das Rechenzentrum gelegen ist, desto geringer ist die Netzwerklatenz, die eine enorme Auswirkung auf die Hologrammstabilität hat.

> [!NOTE]
> Die Downstreambandbreite wird zum größten Teil durch den Videostream beansprucht, der wiederum in Farb- und Tiefeninformationen (beide 60 Hz, Stereo) aufgeteilt ist.

## <a name="network-performance-tests"></a>Netzwerkleistungstests

Wenn Sie wissen möchten, ob die Qualität Ihrer Netzwerkkonnektivität zum Ausführen von Azure Remote Rendering ausreicht, können Sie dies mithilfe verschiedener Onlinetools herausfinden. Es wird dringend empfohlen, diese Onlinetools auf einem leistungsfähigen Laptop auszuführen, der mit demselben WLAN-Netzwerk verbunden ist wie das Gerät, auf dem Sie Ihre Clientanwendung für Azure Remote Rendering ausführen möchten. Ergebnisse von Tests, die auf einem Mobiltelefon oder einer HoloLens2 ausgeführt werden, sind in der Regel weniger nützlich, da es erwiesenermaßen bei Endpunktgeräten mit schwacher Energieversorgung zu deutlichen Schwankungen kommt. Der Ort, an dem Sie den Laptop platzieren, sollte nicht weit von dem Ort entfernt sein, an dem Sie das Gerät verwenden möchten, auf dem Ihre Clientanwendung für Azure Remote Rendering ausgeführt wird.

Im Folgenden sind Sie einige einfache Möglichkeiten für einen schnellen Test der Netzwerkkonnektivität aufgeführt:

1. **Führen Sie ein Netzwerktesttool wie www.speedtest.net aus, um Daten zur Gesamtlatenz und Upstream-/Downstreambandbreite Ihrer Netzwerkverbindung abzurufen.**
Wählen Sie den Server aus, der Ihnen am nächsten ist, und führen Sie den Test durch. Obwohl es sich bei dem Server nicht um das Azure-Rechenzentrum handelt, mit dem Azure Remote Rendering eine Verbindung herstellt, sind die Ergebnisdaten immer noch nützlich, um sich ein Bild von der Leistung Ihrer Internetverbindung und Ihres WLAN-Netzwerks zu machen.
   * **Mindestanforderung** für Azure Remote Rendering: ca. 40 MBit/s Downstream und 5 MBit/s Upstream.
   * **Empfohlen** für Azure Remote Rendering: ca. 100 MBit/s Downstream und 10 MBit/s Upstream.
Es wird empfohlen, den Test mehrmals durchführen und die schlechtesten Ergebnisse zu verwenden.
1. **Verwenden Sie ein Tool wie www.azurespeed.com, um die Latenz für Azure-Rechenzentren zu messen.** Wählen Sie das Azure-Rechenzentrum aus, das von Azure Remote Rendering unterstützt wird und Ihnen am nächsten gelegen ist (weitere Informationen finden Sie unter [unterstützte Regionen](regions.md)), und führen Sie einen **Latenztest** durch. Wenn Ihnen unterschiedliche Zahlen angezeigt werden, sollten Sie kurz warten, damit sich die Ergebnisse stabilisieren können.
   * **Mindestanforderung** für Azure Remote Rendering: Die Latenz sollte beständig weniger als 80 ms betragen.
   * **Empfohlen** für Azure Remote Rendering: Die Latenz sollte beständig weniger als 40 ms betragen.

Obwohl geringe Latenz keine Garantie dafür ist, dass Azure Remote Rendering in Ihrem Netzwerk ordnungsgemäß funktioniert, haben wir festgestellt, dass der Dienst in der Regel problemlos verwendet werden kann, wenn diese Texts erfolgreich waren.
Wenn beim Ausführen von Azure Remote Rendering instabile, unregelmäßige oder springende Hologramme auftreten, finden Sie weitere Informationen in der [Anleitung zum Troubleshooting](../resources/troubleshoot.md).

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Rendern eines Modells mit Unity](../quickstarts/render-model.md)
