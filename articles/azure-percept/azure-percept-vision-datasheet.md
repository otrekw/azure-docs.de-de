---
title: Datenblatt für Azure Percept-Vision
description: Das Datenblatt für Azure Percept-Vision enthält ausführliche Informationen zu Gerätespezifikationen.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: reference
ms.date: 02/16/2021
ms.openlocfilehash: 7bbb3a88bbc3011ec5dd917cdb0c1e49f7556aab
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102177163"
---
# <a name="azure-percept-vision-datasheet"></a>Datenblatt für Azure Percept-Vision

Die unten aufgeführten Spezifikationen gelten für das Azure Percept-Vision-Gerät, das in [Azure Percept DK](./azure-percept-dk-datasheet.md) enthalten ist.

|Produktspezifikation           |Wert     |
|--------------------------------|---------------------|
|Zielbranchen               |Fertigung <br> Intelligente Gebäude <br> Automatisch <br> Einzelhandel |
|Hero-Szenarien                  |Käuferanalyse <br> On-Shelf Availability <br> Reduzierung von Warenschwund <br> Arbeitsplatzüberwachung|
|Dimensionen                      |42 mm × 42 mm × 40 mm (Azure Percept-Vision-SOM-Komponente mit Gehäuse) <br> 42 mm × 42 mm × 6 mm (Vision-SOM-Chip)|
|Verwaltungssteuerungsebene        |Azure Device Update (ADU)          |
|Unterstützte Software und Dienste |[Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) <br> [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) <br> [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) <br> [ONNX-Runtime](https://www.onnxruntime.ai/) <br> [OpenVINO](https://docs.openvinotoolkit.org/latest/index.html) <br> Azure Device Update |
|KI-Beschleunigung                 |Intel Movidius Myriad X (MA2085) VPU (Vision Processing Unit) mit Intel-Kamera-ISP (integriert), 0,7 TOPS |
|Sensoren und visuelle Indikatoren   |Sony IMX219-Kamerasensor mit 6P-Objektiv<br>Auflösung: 8 MP bei 30 FPS, Entfernung: 50 cm bis unendlich<br>Sichtfeld: 120 Grad (diagonal), Farbe: WDR (Wide Dynamic Range), Rolling Shutter mit festem Fokus|
|Kameraunterstützung                  |RGB <br> Zwei Kameras können gleichzeitig betrieben werden. |
|Sicherheit: Crypto-Controller      |ST-Micro STM32L462CE      |
|Versionierung/ID-Komponente       |EEPROM (64 KB) |
|Arbeitsspeicher                          |LPDDR4 (2 GB)     |
|Power                           |3,5 W     |
|Ports                           |1x USB 3.0 (Typ C) <br> 2x MIPI 4-Lane (bis zu 1,5 GBit/s pro Lane)     |
|Steuerungsschnittstellen              |2x I2C <br> 2x SPI <br> 6x PWM (GPIOs: 2x Takt, 2x Framesynchronisierung, 2x nicht verwendet) <br> 2x Reserve-GPIO |
|Zertifizierung                   |FCC <br> IC <br> RoHS-Richtlinien <br> REACH <br> UL   |
|Betriebstemperatur           |0 °C bis 27 °C (Azure Percept-Vision-SOM-Komponente mit Gehäuse) <br> -10 °C bis 70 °C (Vision-SOM-Chip) |
|Berührungstemperatur               |<= 48 °C |
|Relative Luftfeuchtigkeit               |8 bis 90 Prozent    |
