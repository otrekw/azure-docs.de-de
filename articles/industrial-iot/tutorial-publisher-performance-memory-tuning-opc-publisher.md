---
title: Optimieren der Leistung und des Arbeitsspeichers von Microsoft OPC Publisher
description: In diesem Tutorial erfahren Sie, wie Sie die Leistung und den Arbeitsspeicher von OPC Publisher optimieren.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: tutorial
ms.date: 3/22/2021
ms.openlocfilehash: 89e288d1186efd405019d6474dcbd332e7925d67
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104787275"
---
# <a name="tutorial-tune-the-opc-publisher-performance-and-memory"></a>Tutorial: Optimieren der Leistung und des Arbeitsspeichers von OPC Publisher

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:

> [!div class="checklist"]
> * Anpassen der Leistung
> * Anpassen des Nachrichtenflusses an die Arbeitsspeicherressourcen

Bei der Ausführung von OPC Publisher in Produktionsumgebungen müssen die Anforderungen an die Netzwerkleistung (Durchsatz und Wartezeit) und Arbeitsspeicherressourcen berücksichtigt werden. OPC Publisher stellt die folgenden Befehlszeilenparameter zur Verfügung, um diese Anforderungen zu erfüllen:

* Kapazität der Nachrichtenwarteschlange (`mq` für Version 2.5 und niedriger, nicht verfügbar in Version 2.6, `om` für Version 2.7)
* IoT Hub-Sendeintervall (`si`)
* IoT Hub-Nachrichtengröße (`ms`)

## <a name="adjusting-iot-hub-send-interval-and-iot-hub-message-size"></a>Anpassen des IoT Hub-Sendeintervalls und der IoT Hub-Nachrichtengröße

Der `mq/om`-Parameter steuert die Obergrenze der Kapazität der internen Nachrichtenwarteschlange. Diese Warteschlange puffert alle Nachrichten, bevor Sie an IoT Hub gesendet werden. Die Standardgröße der Warteschlange beträgt bis zu 2 MB für OPC Publisher bis Version 2.5, und 4.000 IoT Hub-Nachrichten für Version 2.7 (d. h., wenn die Einstellung für die IoT Hub-Nachrichtengröße 256 KB beträgt, beträgt die Größe der Warteschlange bis zu 1 GB). Wenn OPC Publisher nicht in der Lage ist, Nachrichten an IoT Hub schnell genug zu senden, erhöht sich die Anzahl der Elemente in dieser Warteschlange. Wenn dies während der Testläufe der Fall ist, können Sie eine oder beide der folgenden Maßnahmen ergriffen werden:

* Reduzieren des IoT Hub-Sendeintervalls (`si`)

* Erhöhen der IoT Hub-Nachrichtengröße (`ms`, der höchstmögliche Wert ist 256 KB)

Wenn die Warteschlange weiterhin zunimmt, obwohl die Parameter `si` und `ms` angepasst wurden, wird die maximale Warteschlangenkapazität irgendwann erreicht, und die Nachrichten gehen verloren. Dies liegt daran, dass sowohl für den `si`- als auch den `ms`-Parameter physische Grenzwerte gelten und die Internetverbindung zwischen OPC Publisher und IoT Hub nicht schnell genug für die Anzahl der Nachrichten ist, die in einem bestimmten Szenario gesendet werden müssen. In diesem Fall hilft nur das Einrichten mehrerer, paralleler OPC Publisher-Instanzen. Der `mq/om`-Parameter hat zudem die größte Auswirkung auf den Arbeitsspeicherverbrauch durch OPC Publisher. 

Mit dem Parameter `si` wird für OPC Publisher erzwungen, dass Nachrichten im angegebenen Intervall an IoT Hub gesendet werden. Eine Nachricht wird entweder gesendet, wenn die maximale IoT Hub-Nachrichtengröße von 256 KB an Daten verfügbar ist (wodurch das Zurücksetzen des Sendeintervalls ausgelöst wird) oder wenn die angegebene Intervallzeit verstrichen ist.

Der `ms`-Parameter ermöglicht die Batchverarbeitung von an IoT Hub gesendeten Nachrichten. In den meisten Netzwerkkonfigurationen ist die Wartezeit beim Senden einer einzelnen Nachricht an den IoT Hub hoch, verglichen mit der Zeit, die für die Übertragung der Payload benötigt wird. Dies liegt hauptsächlich an den QoS-Anforderungen (Quality of Service, da Nachrichten erst bestätigt werden, nachdem sie von IoT Hub verarbeitet wurden). Wenn also eine Verzögerung beim Eingang der Daten an IoT Hub akzeptabel ist, sollte OPC Publisher so konfiguriert werden, dass die maximale Nachrichtengröße von 256 KB verwendet wird, indem der `ms`-Parameter auf 0 festgelegt wird. Dies ist auch die kostengünstigste Methode, OPC Publisher zu verwenden.

Mit der Standardkonfiguration werden alle 10 Sekunden (`si=10`) oder wenn 256 KB an IoT Hub-Nachrichtendaten verfügbar sind (`ms=0`), Daten an IoT Hub gesendet. Dies führt zu einer maximalen Verzögerung von 10 Sekunden, die Wahrscheinlichkeit von Datenverlust aufgrund der Nachrichtengröße ist jedoch geringer. Die Metrik `monitored item notifications enqueue failure` in OPC Publisher bis Version 2.5 und `messages lost` in OPC Publisher Version 2.7 gibt an, wie viele Nachrichten verloren gegangen sind.

Wenn sowohl der `si`- als auch der `ms`-Parameter auf 0 festgelegt sind, sendet OPC Publisher eine Nachricht an IoT Hub, sobald Daten verfügbar sind. Daraus ergibt sich eine durchschnittliche IoT Hub-Nachrichtengröße von knapp über 200 Byte. Der Vorteil dieser Konfiguration ist jedoch, dass OPC Publisher die Daten ohne Verzögerung aus der verbundenen Ressource sendet. Die Anzahl verlorener Nachrichten ist hoch für Anwendungsfälle, in denen eine große Datenmenge veröffentlicht werden muss. Daher wird diese Konfiguration für solche Szenarien nicht empfohlen.

Zum Messen der Leistung von OPC Publisher kann der `di`-Parameter verwendet werden, um Leistungsmetriken im angegebenen Intervall (in Sekunden) im Ablaufverfolgungsprotokoll auszugeben.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun erfahren haben, wie Leistung und Arbeitsspeicher von OPC Publisher optimiert werden, können Sie sich weitere Ressourcen im OPC Publisher-GitHub-Repository ansehen:

> [!div class="nextstepaction"]
> [OPC Publisher-GitHub-Repository](https://github.com/Azure/Industrial-IoT)