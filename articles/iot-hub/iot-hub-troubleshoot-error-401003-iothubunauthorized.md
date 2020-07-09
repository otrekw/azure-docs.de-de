---
title: Problembehandlung – Azure IoT Hub-Fehler „401003 IoTHubUnauthorized“
description: Grundlegendes zum Beheben des Fehlers „401003 IoTHubUnauthorized“
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: f46d41c8287d03cbe9582ed560244cbd85cdeeaa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "81759596"
---
# <a name="401003-iothubunauthorized"></a>401003 IoTHubUnauthorized

In diesem Artikel werden die Ursachen des Fehlers **401003 IoTHubUnauthorized** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

### <a name="symptom-1"></a>Symptom 1

In Diagnoseprotokollen wird ein Muster von Geräten angezeigt, bei denen gerade mit der Meldung **401003 IoTHubUnauthorized** die Verbindung getrennt wird, gefolgt von **404104 DeviceConnectionClosedRemotely** und kurz danach der erfolgreichen Herstellung einer Verbindung.

### <a name="symptom-2"></a>Symptom 2

Anforderungen an IoT Hub schlagen mit einer der folgenden Meldungen fehl:

* Autorisierungsheader fehlt
* IotHub „\*“ enthält das angegebene Gerät „\*“ nicht.
* Die Autorisierungsregel „\*“ lässt keinen Zugriff für „\*“ zu.
* Fehler bei der Authentifizierung für dieses Gerät, beim Erneuern von Token oder Zertifikat und beim erneuten Herstellen einer Verbindung
* Der Fingerabdruck entspricht nicht der Konfiguration: Fingerabdruck: SHA1Hash=\*, SHA2Hash=\*; Konfiguration: PrimaryThumbprint=\*, SecondaryThumbprint=\*

## <a name="cause"></a>Ursache

### <a name="cause-1"></a>Ursache 1

Bei MQTT verlassen sich einige SDKs darauf, dass IoT Hub beim Ablauf des Tokens eine Trennung der Verbindung auslöst, um zu wissen, wann es aktualisiert werden muss. Also: 

1. Das SAS-Token läuft ab.
1. IoT Hub bemerkt den Ablauf und trennt die Geräteverbindung mit **401003 IoTHubUnauthorized**.
1. Das Gerät schließt die Trennung der Verbindung mit **404104 DeviceConnectionClosedRemotely** ab.
1. Das IoT SDK generiert ein neues SAS-Token.
1. Das Gerät stellt die Verbindung mit IoT Hub erfolgreich wieder her.

### <a name="cause-2"></a>Ursache 2

IoT Hub konnte den Autorisierungsheader, die Regel oder den Schlüssel nicht authentifizieren.

## <a name="solution"></a>Lösung

### <a name="solution-1"></a>Lösung 1

Wenn das IoT SDK – unter Angabe der Geräteverbindungszeichenfolge – für die Verbindung verwendet wird, ist keine Aktion erforderlich. Das IoT SDK generiert das neue Token erneut, um bei SAS-Tokenablauf die Verbindung wiederherzustellen. 

Wenn die Fehlermenge ein Problem darstellt, wechseln Sie zum C SDK, von dem das SAS-Token vor dem Ablauf erneuert wird. Darüber hinaus kann das SAS-Token bei AMQP ohne eine Trennung der Verbindung aktualisiert werden.

### <a name="solution-2"></a>Lösung 2

Im Allgemeinen sollte in der angezeigten Fehlermeldung erläutert werden, wie der Fehler behoben werden kann. Wenn Sie aus irgendeinem Grund auf die Details der Fehlermeldung nicht zugreifen können, stellen Sie Folgendes sicher:

- Die SAS (Shared Access Signature) oder ein anderes verwendetes Sicherheitstoken ist nicht abgelaufen. 
- Die Autorisierungsanmeldeinformationen sind für das verwendete Protokoll richtig formatiert. Weitere Informationen finden Sie unter [IoT Hub-Zugriffssteuerung](iot-hub-devguide-security.md).
- Die verwendete Autorisierungsregel verfügt über die Berechtigung für den angeforderten Vorgang.

## <a name="next-steps"></a>Nächste Schritte

Zur Vereinfachung der Authentifizierung bei IoT Hub empfehlen wir, [Azure IoT SDKs](iot-hub-devguide-sdks.md) zu verwenden.