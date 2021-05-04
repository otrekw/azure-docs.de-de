---
title: Grundlegendes zur Unterstützung für getrennte Geräteupdates mithilfe von Microsoft Connected Cache | Microsoft-Dokumentation
titleSuffix: Device Update for Azure IoT Hub
description: Grundlegendes zur Unterstützung für getrennte Geräteupdates mithilfe von Microsoft Connected Cache
author: andyriv
ms.author: andyriv
ms.date: 2/16/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: e216d42ff1f279d87e657126514fcfb50960f806
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2021
ms.locfileid: "107811902"
---
# <a name="understand-support-for-disconnected-device-updates"></a>Grundlegendes zur Unterstützung für getrennte Geräteupdates

In einem transparenten Gatewayszenario kann mindestens ein Gerät Nachrichten über ein einziges Gatewaygerät weiterleiten, das die Verbindung zu Azure IoT Hub aufrechterhält. In diesen Fällen haben die untergeordneten Geräte möglicherweise keine Internetverbindung oder es ist ihnen nicht gestattet, Inhalte aus dem Internet herunterzuladen. Das Modul „Microsoft Connected Cache Preview IoT Edge“ bietet Kunden von Device Update for Azure IoT Hub die Möglichkeit eines intelligenten netzwerkinternen Caches, der image- und paketbasierte Updates von Geräten, die auf dem Linux-Betriebssystem basieren, hinter dem IoT Edge-Gateway (IoT-Downstreamgeräte) ermöglicht und zudem dazu beiträgt, Bandbreite für Kunden von Device Update for Azure IoT Hub zu sparen.

## <a name="how-does-microsoft-connected-cache-preview-for-device-update-for-azure-iot-hub-work"></a>Wie funktioniert die Microsoft Connected Cache-Vorschauversion für Device Update for Azure IoT Hub?

Microsoft Connected Cache Preview ist ein intelligenter, transparenter Cache für Inhalte, die für Device Update for Azure IoT Hub-Inhalte veröffentlicht werden, und kann so angepasst werden, dass auch Inhalte aus anderen Quellen wie Paketrepositorys zwischengespeichert werden. Microsoft Connected Cache ist ein „kalter“ Cache, der durch Clientanforderungen für genau die vom Übermittlungsoptimierungs-Client angeforderten Dateibereiche aufgewärmt wird und das Seeding für den Inhalt nicht vorab durchführt. Das Diagramm und die schrittweise Beschreibung unten erklären, wie Microsoft Connected Cache innerhalb der Infrastruktur von Device Update for Azure IoT Hub funktioniert.

>[!Note]
>Bei der Definition dieses Flows wurde davon ausgegangen, dass das IoT-Edge-Gateway über Internetkonnektivität verfügt. Für das Szenario des IoT Edge-Downstreamgateways (Nested Edge) kann das Content Delivery Network (CDN) als das auf dem übergeordneten IoT Edge-Gateway gehostete MCC betrachtet werden.

  :::image type="content" source="media/connected-cache-overview/disconnected-device-update.png" alt-text="Getrenntes Geräteupdate" lightbox="media/connected-cache-overview/disconnected-device-update.png":::

1. Microsoft Connected Cache wird als IoT Edge-Modul auf dem lokalen Server bereitgestellt.
2. Device Update for Azure IoT Hub-Clients sind so konfiguriert, dass sie Inhalte von Microsoft Connected Cache aufgrund des Attributs „GatewayHostName“ der Geräteverbindungszeichenfolge für IoT-Blattgeräte **ODER** des in „config.toml“ festgelegten „parent_hostname“ für untergeordnete IoT Edge-Geräte herunterladen.
3. Die Device Update for Azure IoT Hub-Clients erhalten in beiden Fällen Befehle zum Herunterladen von Updateinhalten vom Device Update for Azure IoT Hub-Dienst und fordern Updateinhalte in Microsoft Connected Cache anstatt im CDN an. Microsoft Connected Cache ist standardmäßig so konfiguriert, dass es am HTTP-Port 80 lauscht, und der Übermittlungsoptimierungs-Client stellt die Inhaltsanforderungen an Port 80, sodass das übergeordnete Objekt so konfiguriert sein muss, dass es an diesem Port lauscht.  Zurzeit wird nur das HTTP-Protokoll unterstützt.
4. Der Microsoft Connected Cache-Server lädt Inhalte aus dem CDN herunter, führt das Seeding für seinen auf Datenträger gespeicherten lokalen Cache durch und liefert die Inhalte an den Device Update for Azure IoT Hub-Client.
   
>[!Note]
>Bei Verwendung von paketbasierten Updates wird der Microsoft Connected Cache-Server vom Administrator mit dem erforderlichen Pakethostnamen konfiguriert.

5. Nachfolgende Anforderungen von anderen Device Update for Azure IoT Hub-Clients für denselben Updateinhalt stammen jetzt aus dem Cache und Microsoft Connected Cache stellt keine Anforderungen an das CDN für denselben Inhalt.

### <a name="supporting-industrial-iot-iiot-with-parentchild-hosting-scenarios"></a>Unterstützung von industriellem IoT (Industrial IoT, IIoT) mit Szenarien für das Hosting von übergeordneten/untergeordneten Objekten

Wenn ein untergeordnetes oder IoT Edge-Downstreamgateway den Microsoft Connected Cache-Server hostet, wird es so konfiguriert, dass es Updateinhalte von dem übergeordneten IoT Edge-Gateway anfordert, das den Microsoft Connected Cache-Server hostet. Dies ist für so viele Ebenen wie nötig erforderlich, bevor das übergeordnete IoT Edge-Gateway erreicht wird, das einen Microsoft Connected Cache-Server mit Internetzugriff hostet. Vom mit dem Internet verbundenen Server wird der Inhalt vom CDN angefordert, woraufhin der Inhalt zurück an das untergeordnete IoT Edge-Gateway gesendet wird, das den Inhalt ursprünglich angefordert hat. Der Inhalt wird auf jeder Ebene auf dem Datenträger gespeichert.

## <a name="access-to-the-microsoft-connected-cache-preview-for-device-update-for-azure-iot-hub"></a>Zugriff auf die Microsoft Connected Cache-Vorschau für Device Update for Azure IoT Hub

Das Microsoft Connected Cache IoT Edge-Modul wird als Vorschau für Kunden freigegeben, die Lösungen mithilfe von Device Update für Azure IoT Hub bereitstellen. Der Zugriff auf die Vorschau erfolgt auf Einladung. [Fordern Sie Zugriff](https://aka.ms/MCCForDeviceUpdateForIoT) auf die Microsoft Connected Cache-Vorschau für Device Update for IoT Hub an. Geben Sie dann die geforderten Informationen an, wenn Sie Zugriff auf das Modul wünschen.
