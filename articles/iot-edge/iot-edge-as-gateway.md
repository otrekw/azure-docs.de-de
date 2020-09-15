---
title: Gateways für nachgeschaltete Geräte – Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden Sie Azure IoT Edge zum Erstellen eines transparenten oder nicht transparenten Gateway-Geräts oder eines Proxygatewaygeräts, das Daten von mehreren nachgeschalteten Geräten in die Cloud sendet oder lokal verarbeitet.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/21/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: 0589779de2ddb0bc75dde3b57d6444634b879f86
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "89017021"
---
# <a name="how-an-iot-edge-device-can-be-used-as-a-gateway"></a>Verwendung eines IoT Edge-Geräts als Gateway

Gateways in IoT Edge-Lösungen bieten Gerätekonnektivität und Edgeanalysen für IoT-Geräte, die sonst nicht über diese Funktionen verfügen würden. Mithilfe von Azure IoT Edge kann jede beliebige Anforderung an ein IoT-Gateway erfüllt werden – unabhängig davon, ob es im Zusammenhang mit Konnektivität, Identität oder Edge Analytics steht. Gatewaymuster in diesem Artikel beziehen sich nur auf Merkmale der Konnektivität und Identität nachgeschalteter Geräte, nicht auf die Verarbeitung von Gerätedaten auf dem Gateway.

## <a name="patterns"></a>Muster

Es gibt drei Muster für die Verwendung eines IoT Edge-Geräts als Gateway: transparent, Protokollübersetzung und Identitätsübersetzung.

Ein wichtiger Unterschied zwischen den Mustern besteht darin, dass ein transparentes Gateway Nachrichten zwischen Downstreamgeräten und IoT Hub weiterleitet, ohne dass zusätzliche Verarbeitungsschritte erforderlich sind. Protokollübersetzung und Identitätsübersetzung erfordern hingegen eine Verarbeitung auf dem Gateway, um die Kommunikation zu ermöglichen.

Alle Gateways können IoT Edge-Module verwenden, um Analysen oder Vorverarbeitungsschritte am Edge auszuführen, bevor die Nachrichten von Downstreamgeräten an IoT Hub übergeben werden.

![Diagramm – Transparente, Protokoll- und Identitätsgatewaymuster](./media/iot-edge-as-gateway/edge-as-gateway.png)

### <a name="transparent-pattern"></a>Transparentes Muster

In einem *transparenten* Muster können Geräte, die theoretisch eine Verbindung mit IoT Hub herstellen könnten, stattdessen eine Verbindung mit einem Gatewaygerät herstellen. Die nachgeschalteten Geräte verfügen über eigene IoT Hub-Identitäten und arbeiten mit dem MQTT-, AMQP- oder HTTP-Protokoll. Das Gateway übergibt einfach die Kommunikation zwischen den Geräten und IoT Hub. Weder die Geräte noch die Benutzer, die damit über IoT Hub interagieren, wissen, dass ein Gateway ihre Kommunikation vermittelt. Dieses fehlende Wissen bedeutet, dass das Gateway als *transparent* betrachtet wird.

Die IoT Edge-Runtime schließt Funktionen für transparente Gateways ein. Weitere Informationen finden Sie unter [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md).

### <a name="protocol-translation-pattern"></a>Protokollübersetzungsmuster

Ein Gateway mit *Protokollübersetzung* wird im Vergleich zum transparenten Gatewaymuster auch als *nicht transparentes* (opakes) Gateway bezeichnet. In diesem Muster können Geräte, die MQTT, AMQP und HTTP nicht unterstützen, Daten über ein Gatewaygerät an IoT Hub senden. Das Gateway versteht das Protokoll, das von den nachgeschalteten Geräten verwendet wird, und ist das einzige Gerät, das eine Identität in IoT Hub hat. Alle Informationen scheinen von einem Gerät – dem Gateway – zu stammen. Nachgeschaltete Geräte müssen zusätzliche identifizierende Informationen in ihre Nachrichten einbetten, wenn Cloudanwendungen die Daten pro Gerät analysieren möchten. Darüber hinaus sind IoT Hub-Grundtypen wie Zwillinge und Methoden nur für Gatewaygeräte, nicht jedoch für nachgeschaltete Geräte verfügbar.

Die IoT Edge-Runtime bietet keine Protokollübersetzungsfunktionen. Dieses Muster erfordert benutzerdefinierte Module oder Module von Drittanbietern, die häufig für die verwendete Hardware und das verwendete Protokoll spezifisch sind. Im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules) stehen mehrere Protokollübersetzungsmodule zur Auswahl.

### <a name="identity-translation-pattern"></a>Identitätsübersetzungsmuster

Bei einem Gatewaymuster mit *Identitätsübersetzung* können Geräte, die keine Verbindung mit IoT Hub herstellen können, stattdessen eine Verbindung mit einem Gatewaygerät herstellen. Das Gateway stellt für die nachgeschalteten Geräte eine IoT Hub-Identität bereit und übernimmt die Protokollübersetzung. Das Gateway ist intelligent genug, um das von den nachgeschalteten Geräten verwendete Protokoll zu verstehen, ihnen Identität bereitzustellen und IoT Hub-Grundtypen zu übersetzen. Nachgeschaltete Geräte werden in IoT Hub als erstrangige Geräte mit Zwillingen und Methoden angezeigt. Ein Benutzer kann mit den Geräten in IoT Hub interagieren, ohne zu merken, dass ein Gatewaygerät zwischengeschaltet ist.

Die IoT Edge-Runtime bietet keine Identitätsübersetzungsfunktionen. Dieses Muster erfordert benutzerdefinierte Module oder Module von Drittanbietern, die häufig für die verwendete Hardware und das verwendete Protokoll spezifisch sind. Ein Beispiel, in dem das Identitätsübersetzungsmuster verwendet wird, finden Sie unter [Azure IoT Edge LoRaWAN Starter Kit](https://github.com/Azure/iotedge-lorawan-starterkit).

## <a name="use-cases"></a>Anwendungsfälle

Alle Gatewaymuster bieten folgende Vorteile:

* **Edgeanalysen**: Verwenden Sie lokal KI-Dienste zum Verarbeiten von Daten, die von nachgeschalteten Geräten stammen, ohne vollständige Telemetriedaten in die Cloud zu senden. Suchen und reagieren Sie lokal auf Informationen, senden Sie nur eine Teilmenge von Daten an IoT Hub.
* **Isolierung von nachgeschalteten Geräten**: Das Gatewaygerät kann alle nachgeschalteten Geräte vor dem Zugriff über das Internet schützen. Es kann sich zwischen einem OT-Netzwerk ohne Konnektivität und einem IT-Netzwerk mit Internetzugriff befinden.
* **Multiplexing der Verbindung**: Alle Geräte, die über ein IoT Edge-Gateway mit IoT Hub verbunden sind, verwenden dieselbe zugrunde liegende Verbindung.
* **Glättung des Datenverkehrs**: Das IoT Edge-Gerät implementiert automatisch exponentielles Backoff, wenn IoT Hub den Datenverkehr drosselt, wobei die Nachrichten lokal beibehalten werden. Durch diesen Vorteil wird Ihre Lösung unempfindlich gegenüber Spitzen im Datenverkehr.
* **Offlineunterstützung**: Das Gatewaygerät speichert Nachrichten und Zwillingsupdates, die nicht an IoT Hub übermittelt werden können.

Ein Gateway, das eine Protokollübersetzung durchführt, kann vorhandene Geräte und neue Geräte unterstützen, die über eingeschränkte Ressourcen verfügen. Zahlreiche vorhandene Geräte erzeugen Daten, aus denen geschäftliche Erkenntnissen gewonnen werden können; sie wurden jedoch nicht speziell für Cloudkonnektivität konzipiert. Nicht transparente Gateways ermöglichen das Entsperren und Verwenden dieser Daten in einer IoT-Lösung.

Ein Gateway, das Identitätsübersetzungen vornimmt, bietet die Vorteile der Protokollübersetzung und ermöglicht außerdem die vollständige Verwaltbarkeit von nachgeschalteten Geräten über die Cloud. Alle Geräte in Ihrer IoT-Lösung werden unabhängig vom jeweiligen Protokoll in IoT Hub angezeigt.

## <a name="cheat-sheet"></a>Cheat Sheet

Hier finden Sie ein schnelles Cheat Sheet, das IoT Hub-Grundtypen bei Verwendung von transparenten, nicht transparenten (Protokoll) und Proxy-Gateways vergleicht.

| Primitiv | Transparentes Gateway | Protokollübersetzung | Identitätsübersetzung |
|--------|-------------|--------|--------|
| In der IoT Hub-Identitätsregistrierung gespeicherte Identitäten | Identitäten aller verbundenen Geräte | Nur die Identität des Gatewaygeräts | Identitäten aller verbundenen Geräte |
| Gerätezwilling | Jedes verbundene Gerät hat einen eigenen Gerätezwilling | Nur das Gateway hat einen Geräte- und Modulzwilling | Jedes verbundene Gerät hat einen eigenen Gerätezwilling |
| Direkte Methoden und C2D-Nachrichten | Die Cloud kann jedes verbundene Gerät einzeln adressieren | Die Cloud kann nur das Gatewaygerät adressieren | Die Cloud kann jedes verbundene Gerät einzeln adressieren |
| [IoT Hub-Drosselungen und -Kontingente](../iot-hub/iot-hub-devguide-quotas-throttling.md) | Gelten für jedes Gerät | Gelten für das Gatewaygerät | Gelten für jedes Gerät |

Bei Verwendung eines nicht transparenten Gatewaymusters (Protokollübersetzung) verwenden alle Geräte, die eine Verbindung über dieses Gateway herstellen, dieselbe C2D-Warteschlange, die höchstens 50 Nachrichten enthalten kann. Daraus folgt, dass das nicht transparente Gatewaymuster nur verwendet werden sollte, wenn wenige Geräte eine Verbindung über die einzelnen Bereichsgateways herstellen und der Cloud-zu-Gerät-Datenverkehr gering ist.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über die drei Schritte zum Einrichten eines transparenten Gateways:

* [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md)
* [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md)
* [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md)
