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
ms.openlocfilehash: af057750e81086bf691b87057da97af3de19cd3b
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2020
ms.locfileid: "92909640"
---
# <a name="401003-iothubunauthorized"></a>401003 IoTHubUnauthorized

In diesem Artikel werden die Ursachen des Fehlers **401003 IoTHubUnauthorized** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

### <a name="symptom-1"></a>Symptom 1

In Protokollen wird ein Muster von Geräten angezeigt, bei denen gerade mit der Meldung **401003 IoTHubUnauthorized** die Verbindung getrennt wird, gefolgt von **404104 DeviceConnectionClosedRemotely** und kurz danach der erfolgreichen Herstellung einer Verbindung.

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

IoT Hub konnte den Autorisierungsheader, die Regel oder den Schlüssel nicht authentifizieren. Dies kann auf einen der in den Symptomen genannten Gründe zurückzuführen sein.

## <a name="solution"></a>Lösung

### <a name="solution-1"></a>Lösung 1

Wenn das IoT SDK – unter Angabe der Geräteverbindungszeichenfolge – für die Verbindung verwendet wird, ist keine Aktion erforderlich. Das IoT SDK generiert das neue Token erneut, um bei SAS-Tokenablauf die Verbindung wiederherzustellen. 

Wenn die Fehlermenge ein Problem darstellt, wechseln Sie zum C SDK, von dem das SAS-Token vor dem Ablauf erneuert wird. Darüber hinaus kann das SAS-Token bei AMQP ohne eine Trennung der Verbindung aktualisiert werden.

### <a name="solution-2"></a>Lösung 2

Im Allgemeinen sollte in der angezeigten Fehlermeldung erläutert werden, wie der Fehler behoben werden kann. Wenn Sie aus irgendeinem Grund auf die Details der Fehlermeldung nicht zugreifen können, stellen Sie Folgendes sicher:

- Die SAS (Shared Access Signature) oder ein anderes verwendetes Sicherheitstoken ist nicht abgelaufen.
- Bei der X.509-Zertifikatsauthentifizierung ist das mit dem Gerät verbundene Geräte- oder Zertifizierungsstellenzertifikat nicht abgelaufen. Wie Sie X.509-Zertifizierungsstellenzertifikate bei IoT Hub registrieren können, erfahren Sie unter [Einrichten der X.509-Sicherheit in Ihrem Azure IoT Hub](iot-hub-security-x509-get-started.md).
- Bei der X.509-Authentifizierung per Fingerabdruck wird der Fingerabdruck des Gerätezertifikats bei IoT Hub registriert.
- Die Autorisierungsanmeldeinformationen sind für das verwendete Protokoll richtig formatiert. Weitere Informationen finden Sie unter [Steuern des Zugriffs auf IoT Hub](iot-hub-devguide-security.md).
- Die verwendete Autorisierungsregel verfügt über die Berechtigung für den angeforderten Vorgang.

## <a name="next-steps"></a>Nächste Schritte

- Zur Vereinfachung der Authentifizierung bei IoT Hub empfehlen wir, [Azure IoT SDKs](iot-hub-devguide-sdks.md) zu verwenden.
- Details zur Authentifizierung bei IoT Hub finden Sie unter [Verwalten des Zugriffs auf IoT Hub](iot-hub-devguide-security.md).
