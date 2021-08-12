---
title: Was ist Azure IoT Hub | Microsoft-Dokumentation
description: In diesem Artikel werden die Verwendungsmöglichkeiten für eine Azure IoT Hub erläutert. Sie ermöglicht ihnen das skalierbare Lesen von Daten und die sichere Verwaltung von Geräten.
author: robinsh
ms.author: robinsh
ms.date: 05/03/2021
ms.topic: overview
ms.custom:
- mvc
- amqp
- mqtt
- 'role: Direction'
- 'role: System Architecture'
- contentperf:fy21q3
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: 8e491e6fc8d0eb93a3d9696f68f045191c2da20b
ms.sourcegitcommit: 8669087bcbda39e3377296c54014ce7b58909746
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2021
ms.locfileid: "114400901"
---
# <a name="what-is-azure-iot-hub"></a>Was ist Azure IoT Hub

IoT Hub ist ein verwalteter Dienst, der in der Cloud gehostet wird und als zentraler Nachrichten-Hub für die Kommunikation in beide Richtungen zwischen einer IoT-Anwendung und den angeschlossenen Geräten fungiert. Sie können Millionen von Geräten und deren Back-End-Lösungen zuverlässig und sicher verbinden. Fast jedes Gerät kann mit einem IoT Hub verbunden werden. 

Es werden mehrere Messagingmuster unterstützt, einschließlich D2C-Telemetrie, Hochladen von Dateien von Geräten und Anforderung-Antwort-Methoden zum Steuern Ihrer Geräte aus der Cloud. IoT Hub unterstützt auch die Überwachung, um das Erstellen von Geräten, das Verbinden von Geräten und Gerätefehler nachzuverfolgen.

Mit den IoT Hub-Funktionen können Sie skalierbare IoT-Lösungen mit vollem Funktionsumfang erstellen, z.B. für die Verwaltung von Werkzeugen und Maschinen für die industrielle Fertigung, für die Nachverfolgung wertvoller Ressourcen im Gesundheitswesen oder für die Überwachung der Nutzung von Bürogebäuden.

## <a name="scale-your-solution"></a>Skalieren Ihrer Lösung

IoT Hub lässt sich auf Millionen von gleichzeitig verbundenen Geräten und Millionen von Ereignissen pro Sekunde skalieren, um Ihre IoT-Workloads zu unterstützen. Weitere Informationen zum Skalieren Ihres IoT-Hubs finden Sie unter [Skalieren von Azure IoT Hub](iot-hub-scaling.md). Weitere Informationen zu den verschiedenen Dienstebenen von IoT Hub und der besten Option für Ihre Skalierbarkeitsanforderungen finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/iot-hub/).

## <a name="secure-your-communications"></a>Sichern der Kommunikation

Sie können Daten mithilfe von IoT Hub sicher senden.

* Dank der gerätebasierten Authentifizierung kann jedes Gerät eine sichere Verbindung mit IoT Hub herstellen und sicher verwaltet werden.

* Sie verfügen über vollständige Kontrolle über den Gerätezugriff und können Verbindungen pro Gerät steuern.

* Der [IoT Hub Device Provisioning Service](../iot-dps/index.yml) stellt Geräte automatisch für die richtige IoT Hub-Instanz bereit, wenn das Gerät zum ersten Mal gestartet wird.

* Mehrere Authentifizierungstypen unterstützen eine Vielzahl von Gerätefunktionen:

  * Tokenbasierte SAS-Authentifizierung, damit Sie schnell mit Ihrer IoT-Lösung loslegen können.

  * Individuelle X.509-Zertifikate stehen für eine sichere, standardbasierte Authentifizierung zur Verfügung.

  * Authentifizierung über X.509-Zertifizierungsstellen können für eine einfache, standardbasierte Registrierung verwendet werden.

## <a name="route-device-data"></a>Weiterleiten von Gerätedaten

Integrierte Funktionen für die Nachrichtenweiterleitung sorgen für Flexibilität bei der Einrichtung einer automatischen regelbasierten Nachrichtenauffächerung:

* Die [Nachrichtenweiterleitung](iot-hub-devguide-messages-d2c.md) wird verwendet, um zu steuern, wohin Ihr Hub Gerätetelemetriedaten sendet.

* Bei der Weiterleitung von Nachrichten an mehrere Endpunkte fallen keine zusätzlichen Kosten an.

* Routingregeln können so konfiguriert werden, dass Nachrichten basierend auf dem Inhalt dieser Nachrichten automatisch weitergeleitet werden, ohne einen Code schreiben zu müssen.

## <a name="integrate-with-other-services"></a>Integration in andere Dienste

Sie können IoT Hub in andere Azure-Dienste integrieren, um vollständige End-to-End-Lösungen zu erstellen. Verwenden Sie z.B. Folgendes:

* [Azure Event Grid](../event-grid/index.yml): Mit diesem Dienst können Sie schnell, zuverlässig, skalierbar und sicher auf kritische Ereignisse reagieren.

* [Azure Logic Apps](../logic-apps/index.yml): Mit diesem Dienst automatisieren Sie Ihre Geschäftsprozesse.

* [Azure Machine Learning](iot-hub-weather-forecast-machine-learning.md): Mit diesem Dienst fügen Sie Ihrer Lösung Modelle für maschinelles Lernen und KI hinzu.

* [Azure Stream Analytics](../stream-analytics/index.yml): Mit diesem Dienst führen Sie in Echtzeit Analyseberechnungen für die Datenströme aus Ihren Geräten aus.

## <a name="configure-and-control-your-devices"></a>Konfigurieren und Steuern von Geräten

Sie können Ihre mit IoT Hub verbundenen Geräte mit einer Vielzahl von integrierten Funktionen verwalten.

* Sie können Metadaten und Statusinformationen für all Ihre Geräte speichern, synchronisieren und abfragen.

* Legen Sie den Gerätestatus entweder pro Gerät oder basierend auf gemeinsamen Gerätemerkmalen fest.

* Dank der Integration von Nachrichtenweiterleitungsfunktionen können Sie die Reaktion auf eine von einem Gerät gemeldete Statusänderung automatisieren.

## <a name="make-your-solution-highly-available"></a>Hochverfügbarkeit für Ihre Lösung

IoT Hub hat eine [IoT Hub Vereinbarung zum Servicelevel](https://azure.microsoft.com/support/legal/sla/iot-hub/) von 99,9 %. Die vollständige [Azure-SLA](https://azure.microsoft.com/support/legal/sla/) erläutert die garantierte Verfügbarkeit von Azure insgesamt.

## <a name="connect-your-devices"></a>Verbinden von Geräten

Verwenden Sie die Bibliotheken des [Azure IoT-Geräte-SDK](./iot-hub-devguide-sdks.md), um Anwendungen zu erstellen, die auf Ihren Geräten ausgeführt werden und mit IoT Hub interagieren. Zu den unterstützten Plattformen gehören verschiedene Linux-Distributionen, Windows und Echtzeit-Betriebssysteme. Unterstützte Sprachen:

* C
* Embedded C
* C#
* Java
* Python
* Node.js

IoT Hub und die Geräte-SDKs unterstützen die folgenden Protokolle zum Verbinden von Geräten:

* HTTPS
* AMQP
* AMQP über WebSockets
* MQTT
* MQTT über WebSockets

IoT Hub und die Geräte-SDKs unterstützen die [Azure IoT Plug & Play](../iot-develop/overview-iot-plug-and-play.md)-Konventionen zum Verbinden von Geräten. IoT Plug & Play-Geräte nutzen ein Gerätemodell, um ihre Funktionen für IoT Plug & Play-fähige Anwendungen verfügbar zu machen. Das Gerätemodell ermöglicht es Lösungsentwicklern, intelligente Geräte ohne manuelle Konfiguration in ihre Lösungen zu integrieren.

Falls Ihre Lösung die Gerätebibliotheken nicht nutzen kann, können die Geräte die Protokolle MQTT v3.1.1, HTTPS 1.1 oder AMQP 1.0 verwenden, um nativ eine Verbindung mit Ihrem Hub herzustellen.

Wenn Ihre Lösung keines der unterstützten Protokolle nutzen kann, können Sie IoT Hub so erweitern, dass benutzerdefinierte Protokolle unterstützt werden:

* Verwenden Sie [Azure IoT Edge](../iot-edge/index.yml), um ein Bereichsgateway zu erstellen, das die Protokollübersetzung im Edge ausführt.

* Passen Sie das [Azure IoT-Protokollgateway](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) so an, dass die Protokollübersetzung in der Cloud erfolgt.

## <a name="quotas-and-limits"></a>Kontingente und Grenzwerte

Jedes Azure-Abonnement verfügt über standardmäßige Kontingentgrenzen, um einen Missbrauch des Dienstes zu verhindern. Diese Grenzen können sich auf den Umfang Ihrer IoT-Lösung auswirken. Der derzeitige Grenzwert liegt bei 50 IoT Hub-Instanzen pro Abonnement. Wenden Sie sich an den Support, um eine Erhöhung des Kontingents anzufordern. Weitere Informationen finden Sie unter [IoT Hub-Kontingente und -Drosselung](iot-hub-devguide-quotas-throttling.md). Weitere Details zu Kontingentgrenzen finden Sie in einem der folgenden Artikel:

* [Einschränkungen bei Azure-Abonnementdiensten](../azure-resource-manager/management/azure-subscription-service-limits.md)

* [IoT Hub throttling and you](https://azure.microsoft.com/blog/iot-hub-throttling-and-you/)

## <a name="iot-hub-on-azure-stack-hub-preview"></a>IoT Hub in Azure Stack Hub (Vorschau)

Mit IoT Hub in Azure Stack Hub (Vorschau) können Sie hybride IoT-Lösungen erstellen. IoT Hub ist ein verwalteter Dienst, der als zentraler Nachrichtenhub für die bidirektionale Kommunikation zwischen Ihrer IoT-Anwendung und den Geräten fungiert, die von der Anwendung verwaltet werden. Sie können IoT Hub in Azure Stack Hub verwenden, um IoT-Lösungen mit zuverlässiger und sicherer Kommunikation zwischen IoT-Geräten und Ihren lokalen Lösungen zu erstellen.

Während der Public Preview-Phase ist IoT Hub in Azure Stack Hub kostenlos. Weitere Informationen finden Sie in der [Übersicht über IoT Hub in Azure Stack Hub](/azure-stack/operator/iot-hub-rp-overview).

## <a name="next-steps"></a>Nächste Schritte

Um eine umfassende IoT-Lösung zu testen, lesen Sie die Schnellstartanleitungen zu IoT Hub:

* [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-nodejs)

Weitere Informationen zu den Möglichkeiten, wie Sie IoT-Lösungen mit Azure IoT erstellen und bereitstellen können, finden Sie unter:

* [Grundlagen: Azure IoT-Technologien und -Lösungen](../iot-fundamentals/iot-services-and-technologies.md).