---
title: Verbinden von Azure Percept über Mobilfunknetze
description: In diesem Artikel wird erläutert, wie Sie das Azure Percept DK über Mobilfunknetze verbinden.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: conceptual
ms.date: 05/20/2021
ms.custom: template-concept
ms.openlocfilehash: 93a2ed0c7f800461f7de6c895b9be1508603fd82
ms.sourcegitcommit: 070122ad3aba7c602bf004fbcf1c70419b48f29e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111441993"
---
# <a name="connect-the-azure-percept-dk-over-cellular-networks"></a>Verbinden des Azure Percept DK über Mobilfunknetze

Die Verbindung von Edge-KI-Geräten über Mobilfunknetze (LTE und 5G) bietet zahlreiche Vorteile. Edge-KI ist in Szenarien besonders effektiv, in denen WLAN- und LAN-Konnektivität nur eingeschränkt vorhanden ist, z. B. in intelligenten Städten, autonomen Fahrzeugen und der Landwirtschaft. Darüber hinaus bieten Mobilfunknetze mehr Sicherheit als ein WLAN. Und schließlich ermöglicht die Verwendung von IoT-Geräten, die KI am Edge ausführen, die Optimierung der Bandbreite von Mobilfunknetzen. Dabei werden nur notwendige Informationen an die Cloud gesendet, der größte Teil der Daten wird auf dem Gerät verarbeitet. Derzeit kann das Azure Percept DK keine direkte Verbindung mit Mobilfunknetzen herstellen. Das DK kann jedoch über die integrierten Ethernet- und WLAN-Funktionen eine Verbindung mit Mobilfunkgateways herstellen. In diesem Artikel wird erläutert, wie dies funktioniert.

## <a name="options-for-connecting-the-azure-percept-dk-over-cellular-networks"></a>Optionen zum Verbinden des Azure Percept DK über Mobilfunknetze
Mit zusätzlicher Hardware können Sie das Azure Percept DK über einen Mobilfunkstandard wie LTE oder 5G verbinden. Derzeit werden zwei primäre Optionen unterstützt:
- **Mobilfunkgerät für WLAN-Hotspot**: Hierbei wird das Development Kit mit dem WLAN verbunden, das vom WLAN-Hotspot bereitgestellt wird. In diesem Fall stellt das Development Kit die Verbindung genauso her wie mit jedem anderen WLAN auch. Weitere Informationen finden Sie im [Einrichtungsleitfaden für Azure Percept DK](./quickstart-percept-dk-set-up.md). Wählen Sie das WLAN aus, das vom Hotspot übertragen wird.
- **Gerät für Mobilfunk-Ethernet-Gateway**: Hierbei wird das Development Kit per Ethernet mit dem drahtlosen Gateway verbunden. Dies bietet eine höhere Sicherheit als WLAN-Verbindungen. Im Rest dieses Artikels wird detailliert erläutert, wie ein solches Netzwerk konfiguriert wird.

## <a name="cellular-gateway-topology"></a>Topologie eines Mobilfunkgateways
:::image type="Image" source="media/connect-over-cellular/topology.png" alt-text="Dieses Diagramm zeigt, wie das Azure Percept DK über Ethernet eine Verbindung mit einem Mobilfunkgateway herstellt.":::

Im Diagramm oben sehen Sie, wie ein Mobilfunkgateway ganz einfach mit dem Azure Percept DK gekoppelt werden kann.

## <a name="considerations-when-connecting-to-a-cellular-gateway"></a>Überlegungen für Verbindungen mit einem Mobilfunkgateway
Im Folgenden finden Sie einige wichtige Punkte, die Sie berücksichtigen sollten, wenn Sie das Azure Percept DK mit einem Mobilfunkgateway verbinden.
- Richten Sie zuerst das Gateway ein, und überprüfen Sie, ob es über die SIM-Karte eine Verbindung empfängt. Damit wird es einfacher, mögliche Probleme beim Herstellen der Verbindung mit dem Azure Percept DK zu beheben.
- Stellen Sie sicher, dass beide Enden des Ethernet-Kabels fest mit dem Gateway und dem Azure Percept DK verbunden sind.
- Befolgen Sie die [Standardanweisungen](./how-to-connect-over-ethernet.md), um das Azure Percept DK über Ethernet zu verbinden.
- Wenn Ihr Mobilfunktarif kontingentbasiert ist, empfiehlt es sich, die Menge an Daten zu optimieren, die von Ihren Azure Percept DK-Modellen an die Cloud gesendet werden.
- Stellen Sie sicher, dass Sie über eine [ordnungsgemäß konfigurierte Firewall](./concept-security-configuration.md) verfügen, die eingehenden Datenverkehr externen Ursprungs blockiert.

## <a name="ssh-over-a-cellular-network"></a>SSH über ein Mobilfunknetz
Um über ein Mobilfunk-Ethernet-Gateway eine SSH-Verbindung zwischen Development Kit und Ethernet-Netzwerk herzustellen, stehen Ihnen die folgenden Optionen zur Verfügung:
- **Verwenden des WLAN-Zugriffspunkts des Development Kits**. Wenn das WLAN deaktiviert ist, können Sie es erneut aktivieren, indem Sie das Development Kit neu starten. Danach können Sie eine Verbindung mit dem WLAN-Zugriffspunkt des Kits herstellen und [diese SSH-Verfahren](./how-to-ssh-into-percept-dk.md) befolgen.
- **Verwenden einer Ethernet-Verbindung mit einem lokalen Netzwerk (LAN)** . Bei dieser Option trennen Sie das Development Kit physisch vom Mobilfunkgateway und verbinden es mit dem LAN-Router. Weitere Informationen finden Sie unter [Herstellen einer Ethernet-Verbindung](./how-to-connect-over-ethernet.md). 
- **Verwenden der Remotezugriffsfunktionen des Gateways**. Viele Mobilfunkgateways enthalten Verwaltungsprogramme für den Remotezugriff, über die eine SSH-Verbindung mit Geräten im Netzwerk hergestellt werden kann. Erkundigen Sie sich beim Hersteller Ihres Mobilfunkgateways, ob dieses eine solche Funktion bietet. Hier finden Sie ein Beispiel für ein Verwaltungsprogramm für den Remotezugriff für [Mobilfunkgateways von Cradlepoint](https://customer.cradlepoint.com/s/article/NCM-Remote-Connect-LAN-Manager).
- **Verwenden des seriellen Anschlusses des Development Kits**. Das Azure Percept DK verfügt über einen seriellen Anschluss, über den eine direkte Verbindung mit dem Gerät hergestellt werden kann. Detaillierte Anweisungen finden Sie unter [Herstellen einer seriellen Verbindung mit Ihrem Azure Percept DK](./how-to-connect-to-percept-dk-over-serial.md).

## <a name="considerations-when-selecting-a-cellular-gateway-device"></a>Überlegungen bei der Auswahl eines Mobilfunkgatewaygeräts
Mobilfunkgateway unterstützen verschiedene Technologien, die sich auf die maximale Datenübertragungsrate für Downloads und Uploads auswirken. Die angekündigten Datenraten können zur Orientierung bei der Entscheidung dienen, werden aber in aller Regel nie erreicht. Im Folgenden finden Sie einige Hinweise für die Auswahl des richtigen Gateways für Ihre Anforderungen.
 
- **LTE CAT-1** bietet bis zu 10 Mbit/s für Downloads und 5 Mbit/s für Uploads. Diese Übertragungsraten genügen für die Standardfeatures des Azure Percept DK wie etwa die Objekterkennung und das Erstellen eines Sprach-Assistenten. Für Lösungen, bei denen Videodaten in die Cloud gestreamt werden müssen, ist dieser Standard möglichweise nicht ausreichend.
- **LTE CAT-3 und 4** bieten bis zu 100 Mbit/s für Downloads und 50 Mbit/s für Uploads. Das genügt zum Streamen von Videodaten in die Cloud. Zum Streamen von Videos in Full-HD-Qualität reichen diese Raten jedoch nicht aus.
- **LTE CAT-5 und höher** bieten Datenraten, die zum Streamen von HD-Videodaten für ein einzelnes Gerät ausreichen. Wenn Sie mehrere Geräte an ein einziges Gateway anschließen müssen, sollten Sie 5G in Erwägung ziehen.
- **5G**-Gateways sind eine sichere Basis, damit Ihre Szenarien auch in Zukunft reibungslos funktionieren. Dieser Standard bietet Datenübertragungsraten und Bandbreiten, mit denen sich ein hoher Datendurchsatz für mehrere Geräte gleichzeitig realisieren lässt. Darüber hinaus sorgen sie auch für niedrigere Latenzen bei der Datenübertragung.


## <a name="next-steps"></a>Nächste Schritte
Wenn Sie über ein Mobilfunkgateway verfügen und Ihr Azure Percept DK damit verbinden möchten, führen Sie die nächsten Schritte aus:
- [Verbinden des Azure Percept DK über Ethernet](./how-to-connect-over-ethernet.md)
