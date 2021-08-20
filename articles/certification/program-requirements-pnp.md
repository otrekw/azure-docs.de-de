---
title: Anforderungen für IoT Plug & Play-Zertifizierungen
description: Anforderungen für IoT Plug & Play-Zertifizierungen
author: cbroad
ms.author: cbroad
ms.topic: conceptual
ms.date: 03/15/2021
ms.custom: IoT Plug and Play Certification Requirements
ms.service: certification
ms.openlocfilehash: 54ef14a3851d656d81d9b0ec194f6b8127eba5b3
ms.sourcegitcommit: 8669087bcbda39e3377296c54014ce7b58909746
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2021
ms.locfileid: "114402756"
---
# <a name="iot-plug-and-play-certification-requirements"></a>Anforderungen für IoT Plug & Play-Zertifizierungen

In diesem Dokument werden die gerätespezifischen Funktionen beschrieben, die im Azure IoT-Gerätekatalog dargestellt werden. Eine Funktion ist ein einzelnes Geräteattribut, das in Form einer Softwareimplementierung oder einer Kombination aus Software- und Hardwareimplementierungen vorliegen kann.

## <a name="program-purpose"></a>Zweck des Programms

IoT Plug & Play (Vorschauversion) ermöglicht es Lösungsentwicklern, intelligente Geräte ohne manuelle Konfiguration in ihre Lösungen zu integrieren. Das Herzstück von IoT Plug & Play ist ein Gerätemodell, mit dessen Hilfe ein Gerät einer IoT Plug & Play-fähigen Anwendung seine Funktionen verfügbar macht. Dieses Modell weist die Struktur einer Gruppe von Elementen auf: Telemetrie, Eigenschaften und Befehle.

Dies sind die Versprechen der IoT Plug & Play-Zertifizierung:

1.  Definierte Gerätemodelle und Schnittstellen sind mit der [Digital Twin Definition Language](https://github.com/Azure/opendigitaltwins-dtdl) kompatibel  
1.  Einfache Integration mit Azure IoT-basierten Lösungen mithilfe der [Digital Twin-APIs](../iot-develop/concepts-digital-twin.md): Azure IoT Hub und Azure IoT Central
1.  Validierte Produktwahrheit auf zertifizierten Geräten
1.  Erfüllt alle Anforderungen von [Azure Certified Device](./program-requirements-azure-certified-device.md)

## <a name="requirements"></a>Anforderungen

**[Erforderlich] Gerät an Cloud: Der Zweck des Tests ist es, sicherzustellen, dass Telemetrie sendende Geräte mit IoT Hub funktionieren**

| **Name**                | IoTPnP.D2C                                               |
| ----------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit** | Jetzt verfügbar                                                |
| **Gilt für**          | Blattgerät/Edgegerät                                      |
| **Betriebssystem**                  | Agnostisch                                                     |
| **Überprüfungstyp**     | Automatisiert                                                    |
| **Überprüfung**          | Das Gerät muss alle Telemetrieschemas an IoT Hub senden. Microsoft stellt den [Portalworkflow](https://certify.azure.com) zum Ausführen der Tests bereit. Gerät an Cloud (erforderlich): **1.** Überprüft, ob das Gerät Nachrichten an den vom AICS verwalteten IoT Hub senden kann **2.** Der Benutzer muss die Anzahl und die Häufigkeit der Nachrichten angeben. **3.** AICS überprüft, ob die Telemetrie von der Hub-Instanz empfangen wird. |
| **Ressourcen**           | [Zertifizierungsschritte](./overview.md) (enthält alle zusätzlichen Ressourcen) |


**[Erforderlich] DTDL v2: Der Zweck des Test ist, sicherzustellen, dass die definierten Gerätemodelle und Schnittstellen mit der Digital Twins Definition Language v2 kompatibel sind.**                                                              

| **Name**                | IoTPnP.DTDL                                                  |
| ----------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit** | Jetzt verfügbar                                                |
| **Gilt für**          | Jedes Gerät                                                   |
| **Betriebssystem**                  | Agnostisch                                                     |
| **Überprüfungstyp**     | Automatisiert                                                    |
| **Überprüfung**          | Der [Portalworkflow](https://certify.azure.com) überprüft: **1.** Ankündigung der Modell-ID und Sicherstellung, dass das Gerät entweder mit dem MQTT oder dem MQTT-über-WebSockets-Protokoll verbunden ist **2.** Die Modelle sind mit der DTDL v2 kompatibel **3.** Telemetrie, Eigenschaften und Befehle sind ordnungsgemäß implementiert und interagieren zwischen dem digitalen IoT Hub-Zwilling und dem Gerätezwilling auf dem Gerät |
| **Ressourcen**           | [Updates zur Aktualisierung der öffentlichen Vorschau](../iot-develop/overview-iot-plug-and-play.md) |

**[Erforderlich] Die Gerätemodelle sind im öffentlichen Modellrepository veröffentlicht**

| **Name**                | IoTPnP.ModelRepo                                             |
| ----------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit** | Jetzt verfügbar                                                |
| **Gilt für**          | Jedes Gerät                                                   |
| **Betriebssystem**                  | Agnostisch                                                     |
| **Überprüfungstyp**     | Automatisiert                                                    |
| **Überprüfung**          | Alle Gerätemodelle müssen im öffentlichen Repository veröffentlicht sein. Die Gerätemodelle werden anhand der im öffentlichen Repository verfügbaren Modelle aufgelöst **1.** Der Benutzer muss die Modelle vor dem Einreichen zur Zertifizierung manuell im öffentlichen Repository veröffentlichen. **2.** Beachten Sie, dass nach der Veröffentlichung der Modelle keine Veränderungen mehr möglich sind. Wir empfehlen dringend, erst dann zu veröffentlichen, wenn die Modelle und der eingebettete Gerätecode endgültig sind.*1  *1 Der Benutzer muss sich an den Microsoft-Support wenden, um einmal im Modellrepository veröffentlichte Modelle zu widerrufen **3.** Der [Portalworkflow](https://certify.azure.com) überprüft, ob die Modelle im öffentlichen Repository vorhanden sind, wenn das Gerät mit dem Zertifizierungsdienst verbunden wird. |
| **Ressourcen**           | [Modellrepository](../iot-develop/overview-iot-plug-and-play.md) |

**[Erforderlich] Physische Geräteüberprüfung mithilfe von GSG**

| **Name**                                  | IoTPnP.Physicaldevice                                      |
| ----------------------------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit**                   | Jetzt verfügbar                                                |
| **Gilt für**                            | Jedes Gerät                                                   |
| **Betriebssystem**                                    | Agnostisch                                                     |
| **Überprüfungstyp**                       | Manuell                                                       |
| **Überprüfung**                            | Die Partner müssen sich mit ihrem Microsoft-Ansprechpartner ([iotcert@microsoft.com](mailto:iotcert@microsoft.com)) in Verbindung setzen, um Maßnahmen zum Durchführen zusätzlicher Überprüfungen auf physischen Geräten zu vereinbaren. Aufgrund der COVID-19-Situation untersuchen wir verschiedene Wege, eine Überprüfung physischer Geräte durchzuführen, ohne die Geräte an Microsoft zu senden. |
| **Ressourcen**                             | Details werden später bekannt gegeben.                                 |
| **Für Azure empfohlen**       | Nicht zutreffend    |

**[Falls implementiert] Geräteinformationsschnittstelle: Der Zweck des Tests ist die Überprüfung, ob die Geräteinformationsschnittstelle ordnungsgemäß im Gerätecode implementiert ist**

| **Name**                | IoTPnP.DeviceInfoInterface                                   |
| ----------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit** | Jetzt verfügbar                                                |
| **Gilt für**          | Jedes Gerät                                                   |
| **Betriebssystem**                  | Agnostisch                                                     |
| **Überprüfungstyp**     | Automatisiert                                                    |
| **Überprüfung**          | Der [Portalworkflow](https://certify.azure.com) überprüft, ob die Geräteinformatinsschnittstelle vom Gerätecode implementiert wird **1.** Überprüft, ob die Werte vom Gerätecode an IoT Hub ausgegeben werden **2.** Überprüft, ob die Schnittstelle im DCM implementiert ist (diese Implementierung wird in DTDL v2 geändert) **3.** Überprüft, ob die Eigenschaften schreibgeschützt sind (nur Lesezugriff zulassen) **4.** Überprüft, ob der Schematyp Zeichenfolge und/oder long und nicht NULL ist |
| **Ressourcen**           | [Von Microsoft definierte Schnittstelle](../iot-develop/overview-iot-plug-and-play.md) |
| **Für Azure empfohlen**  | Nicht zutreffend                                                          |

**[Falls implementiert] Cloud zu Gerät: Der Zweck des Tests besteht darin, sicherzustellen, dass Nachrichten von der Cloud an Geräte gesendet werden können**

| **Name**                | IoTPnP.C2D                                               |
| ----------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit** | Jetzt verfügbar                                                |
| **Gilt für**          | Blattgerät/Edgegerät                                      |
| **Betriebssystem**                  | Agnostisch                                                     |
| **Überprüfungstyp**     | Automatisiert                                                    |
| **Überprüfung**          | Das Gerät muss in der Lage sein, Nachrichten von IoT Hub aus der Cloud an das Gerät zu empfangen. Microsoft stellt den [Portalworkflow](https://certify.azure.com) zum Durchführen dieser Tests zur Verfügung. Cloud zu Gerät (falls implementiert): **1.** Überprüft, ob das Gerät Nachrichten von IoT Hub empfangen kann **2.** AICS sendet zufällige Nachrichten und prüft anhand der Nachrichten-ACK vom Gerät |
| **Ressourcen**           | **1.** [Zertifizierungsschritte](./overview.md) (enthält alle zusätzlichen Ressourcen), **2.** [Senden von C2D-Nachrichten von einem IoT Hub](../iot-hub/iot-hub-devguide-messages-c2d.md) |

**[Falls implementiert] Direkte Methoden: Der Zweck des Tests besteht darin, sicherzustellen, dass das Gerät mit IoT Hub funktioniert und direkte Methoden unterstützt**

| **Name**                | IoTPnP.DirectMethods                                     |
| ----------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit** | Jetzt verfügbar                                                |
| **Gilt für**          | Blattgerät/Edgegerät                                      |
| **Betriebssystem**                  | Agnostisch                                                     |
| **Überprüfungstyp**     | Automatisiert                                                    |
| **Überprüfung**          | Das Gerät muss in der Lage sein, Befehlsanforderungen von IoT Hub zu empfangen und zu beantworten. Microsoft stellt den [Portalworkflow](https://certify.azure.com) zum Ausführen der Tests bereit. Direkte Methoden (falls implementiert): **1.** Der Benutzer muss die Methodennutzlast der direkten Methode angeben. **2.** AICS überprüft, ob die angegebene Nutzlastanforderung vom Hub gesendet und die ACK-Nachricht vom Gerät empfangen wird. |
| **Ressourcen**           | **1.** [Zertifizierungsschritte](./overview.md) (enthält alle zusätzlichen Ressourcen), **2.** [Verstehen direkter Methoden von IoT Hub](../iot-hub/iot-hub-devguide-direct-methods.md) |

**[Falls implementiert] Gerätezwillingseigenschaft: Der Zweck des Tests ist, sicherzustellen, dass Geräte, die Telemetrie senden, mit IoT Hub funktionieren und einen Teil der IoT Hub-Funktionen unterstützen, wie etwa direkte Methoden und die Gerätezwillingseigenschaft**

| **Name**                | IoTPnP.DeviceTwin                                        |
| ----------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit** | Jetzt verfügbar                                                |
| **Gilt für**          | Blattgerät/Edgegerät                                      |
| **Betriebssystem**                  | Agnostisch                                                     |
| **Überprüfungstyp**     | Automatisiert                                                    |
| **Überprüfung**          | Das Gerät muss alle Telemetrieschemas an IoT Hub senden. Microsoft stellt den [Portalworkflow](https://certify.azure.com) zum Ausführen der Tests bereit. Gerätezwillingseigenschaft (falls implementiert): **1.** AICS überprüft die Lese-/Schreib-Eigenschaft im JSON des Gerätezwillings **2.** Der Benutzer muss die zu ändernde JSON-Nutzlast angeben **3.** AICS überprüft, ob die angegebenen gewünschten Eigenschaften von IoT Hub gesendet und die ACK-Nachrichten vom Gerät empfangen werden. |
| **Ressourcen**           | **1.** [Zertifizierungsschritte](./overview.md) (enthält alle zusätzlichen Ressourcen), **2.** [Verwenden von Gerätezwillingen mit IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md) |