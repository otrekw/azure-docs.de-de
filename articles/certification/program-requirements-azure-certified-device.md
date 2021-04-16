---
title: Azure Certified Device Anforderungen
description: Azure Certified Device Programm-Anforderungen
author: cbroad
ms.author: cbroad
ms.topic: conceptual
ms.date: 03/15/2021
ms.custom: Azure Certified Device Certification Requirements
ms.service: certification
ms.openlocfilehash: 497ffa4b3026491d6aa95df87708b3b1f2f1619e
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107308284"
---
# <a name="azure-certified-device-requirements"></a>Azure Certified Device Anforderungen 
(früher als IOT Hub bezeichnet)

In diesem Dokument werden die gerätespezifischen Funktionen beschrieben, die im Azure Certified Device-Katalog dargestellt sind. Eine Funktion ist ein einzelnes Geräteattribut, das in Form einer Softwareimplementierung oder einer Kombination aus Software- und Hardwareimplementierungen vorliegen kann. 

## <a name="program-purpose"></a>Zweck des Programms

Microsoft vereinfacht IOT und die Azure Certified-Geräte Zertifizierung ist ein grundlegender Zertifizierungsprogramm, mit dem sichergestellt wird, dass alle Gerätetypen auf sichere Azure IOT Hub bereitgestellt werden.

Die Zusage der Azure Certified-Geräte Zertifizierung lautet:

1. Geräte Unterstützung für Telemetrie, die mit IOT Hub funktioniert
2.  Geräteunterstützung IoT Hub Device Provisioning Service (DPS) zur sicheren Bereitstellung an Azure IoT Hub
3.  Das Gerät unterstützt die einfache Eingabe der Ziel-DPS-ID-Bereichs Übertragung, ohne dass der Benutzer eingebetteten Code neu kompilieren muss.
4.  Validiert optional andere Elemente wie Cloud-to-Device-Nachrichten, direkte Methoden und Gerätezwilling 

## <a name="requirements"></a>Anforderungen

**[Erforderlich] Gerät an Cloud: Der Zweck des Tests ist es, sicherzustellen, dass Telemetrie sendende Geräte mit IoT Hub funktionieren**

| **Name**                | AzureCertified.D2C                                               |
| ----------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit** | Jetzt verfügbar                                                |
| **Gilt für**          | Blattgerät/Edgegerät                                      |
| **Betriebssystem**                  | Agnostisch                                                     |
| **Überprüfungstyp**     | Automatisiert                                                    |
| **Überprüfung**          | Das Gerät muss alle Telemetrieschemas an IoT Hub senden. Microsoft stellt den [Portalworkflow](https://certify.azure.com/) zum Ausführen der Tests bereit. Gerät an Cloud (erforderlich): **1.** Überprüft, ob das Gerät Nachrichten an den vom AICS verwalteten IoT Hub senden kann **2.** Der Benutzer muss die Anzahl und die Häufigkeit der Nachrichten angeben. **3.** AICS überprüft, ob die Telemetrie von der Hub-Instanz empfangen wird. |
| **Ressourcen**           | [Zertifizierungsschritte](./overview.md) (enthält alle zusätzlichen Ressourcen) |

**[Erforderlich] DPS: Der Zweck des Tests ist die Überprüfung mit einer der drei Nachweismethoden, ob das Gerät den IoT Hub Device Provisioning Service implementiert und unterstützt**

| **Name**                | AzureCertified.DPS                                               |
| ----------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit** | Neu                                                          |
| **Gilt für**          | Jedes Gerät                                                   |
| **Betriebssystem**                  | Agnostisch                                                     |
| **Überprüfungstyp**     | Automatisiert                                                    |
| **Überprüfung**          | Das Gerät unterstützt die einfache Eingabe des Besitzes des Ziel-DPS-ID-Bereichs, ohne dass der eingebettete Code neu kompiliert werden muss. Microsoft stellt den [Portalworkflow](https://certify.azure.com) für die Ausführung der Tests zur Verfügung, um zu prüfen, ob das Gerät DPS unterstützt **1.** Der Benutzer muss eine der Nachweismethoden (X.509, TPM und SAS-Schlüssel) auswählen **2.** Je nach gewählter Nachweismethode muss der Benutzer entsprechende Maßnahmen ergreifen, wie etwa **a)** Hochladen eines X.509-Zertifikats in den AICS-verwalteten DPS-Bereich **b)** Implementieren des SAS-Schlüssels oder Endorsement Keys auf dem Gerät |
| **Ressourcen**           | [Device Provisioning Service – Übersicht](../iot-dps/about-iot-dps.md) |

**[Falls implementiert] Cloud zu Gerät: Der Zweck des Tests besteht darin, sicherzustellen, dass Nachrichten von der Cloud an Geräte gesendet werden können**                                                              

| **Name**                | AzureCertified.C2D                                                  |
| ----------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit** | Jetzt verfügbar                                            |
| **Gilt für**          | Blattgerät/Edgegerät                                                   |
| **Betriebssystem**                  | Agnostisch                                                     |
| **Überprüfungstyp**     | Automatisiert                                                    |
| **Überprüfung**          | Das Gerät muss in der Lage sein, Nachrichten von IoT Hub aus der Cloud an das Gerät zu empfangen. Microsoft stellt den [Portal-Workflow](https://certify.azure.com) zum Ausführen dieser Tests bereit. Cloud zu Gerät (sofern implementiert): **1.** Überprüft, ob das Gerät Nachrichten von IoT Hub empfangen kann **2.** AICS sendet zufällige Nachrichten und prüft anhand der Nachrichten-ACK vom Gerät  |
| **Ressourcen**           | **a)** [Schritte zur Zertifizierung](./overview.md) (hat alle zusätzlichen Ressourcen) **b)** [Senden von Cloud-to-Device-Nachrichten von einem IoT-Hub](../iot-hub/iot-hub-devguide-messages-c2d.md) |

**[Falls implementiert] Direkte Methoden: Der Zweck des Tests besteht darin, sicherzustellen, dass das Gerät mit IoT Hub funktioniert und direkte Methoden unterstützt**

| **Name**                | AzureCertified.DirectMethods                                             |
| ----------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit** | Jetzt verfügbar                                            |
| **Gilt für**          | Blattgerät/Edgegerät                                                   |
| **Betriebssystem**                  | Agnostisch                                                     |
| **Überprüfungstyp**     | Automatisiert                                                    |
| **Überprüfung**          | Das Gerät muss in der Lage sein, Befehlsanforderungen von IoT Hub zu empfangen und zu beantworten. Microsoft stellt den [Portalworkflow](https://certify.azure.com) zum Ausführen der Tests bereit. Direkte Methoden (falls implementiert): **1.** Der Benutzer muss die Methodennutzlast der direkten Methode angeben. **2.** AICS überprüft, ob die angegebene Nutzlastanforderung vom Hub gesendet und die ACK-Nachricht vom Gerät empfangen wird. |
| **Ressourcen**           | **a)** [Zertifizierungsschritte](./overview.md) (verfügt über alle zusätzlichen Ressourcen) **b)** [Verstehen direkter Methoden von IOT Hub](../iot-hub/iot-hub-devguide-direct-methods.md) |

**[Falls implementiert] Gerätezwillingseigenschaft: Der Zweck des Tests ist, sicherzustellen, dass Geräte, die Telemetrie senden, mit IoT Hub funktionieren und einen Teil der IoT Hub-Funktionen unterstützen, wie etwa direkte Methoden und die Gerätezwillingseigenschaft**

| **Name**                                  | AzureCertified.DeviceTwin                                      |
| ----------------------------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit**                   | Jetzt verfügbar                                            |
| **Gilt für**                            | Blattgerät/Edgegerät                                                   |
| **Betriebssystem**                                    | Agnostisch                                                     |
| **Überprüfungstyp**                       | Automatisiert                                                       |
| **Überprüfung**                            | Das Gerät muss alle Telemetrieschemas an IoT Hub senden. Microsoft stellt den [Portalworkflow](https://certify.azure.com) zum Ausführen der Tests bereit. Gerätezwillingseigenschaft (falls implementiert): **1.** AICS überprüft die Lese-/Schreib-Eigenschaft im JSON des Gerätezwillings **2.** Der Benutzer muss die zu ändernde JSON-Nutzlast angeben **3.** AICS überprüft, ob die angegebenen gewünschten Eigenschaften von IoT Hub gesendet und die ACK-Nachrichten vom Gerät empfangen werden. |
| **Ressourcen**                             | **a)** [Zertifizierungsschritte](./overview.md) (verfügt über alle zusätzlichen Ressourcen) **b)** [verwenden Gerätezwillinge mit IOT Hub](../iot-hub/iot-hub-devguide-device-twins.md) |
