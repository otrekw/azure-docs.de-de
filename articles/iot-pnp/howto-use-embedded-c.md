---
title: Verwenden von IoT Plug & Play auf eingeschränkten Geräten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie IoT Plug & Play auf eingeschränkten Geräten implementieren können, wahlweise mithilfe des SDKs für Embedded C oder für Azure RTOS.
author: EliotSeattle
ms.author: eliotgra
ms.date: 09/23/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 6c792fbbb44b7dc769e7cdc56850684bd69ea40b
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107864135"
---
# <a name="implement-iot-plug-and-play-on-constrained-devices"></a>Implementieren von IoT Plug & Play auf eingeschränkten Geräten

Wenn Sie für *eingeschränkte Geräte* entwickeln, können Sie IoT Plug & Play zusammen mit den [Azure SDK for Embedded C IoT-Clientbibliotheken](https://aka.ms/embeddedcsdk) oder mit [Azure RTOS](/azure/rtos/overview-rtos) verwenden. Dieser Artikel enthält Links und Ressourcen für diese eingeschränkten Szenarien.

## <a name="use-the-sdk-for-embedded-c"></a>Verwenden des SDK for Embedded C

Das SDK for Embedded C bietet eine schlanke Lösung zum Verbinden von eingeschränkten Geräten mit Azure IoT-Diensten, einschließlich der Verwendung der [IoT Plug & Play-Konventionen](concepts-convention.md). Die folgenden Links führen zu Beispielen für reale Geräte und für Schulungs- und Debugzwecke.

### <a name="use-a-real-device"></a>Verwenden eines echten Geräts

Ein vollständiges End-to-End-Tutorial zum Verwenden des SDK for Embedded C, des Gerätebereitstellungsdiensts und IoT Plug & Play auf einem realen Gerät finden Sie unter [Repurpose PIC-IoT Wx Development Board to Connect to Azure through IoT Hub Device Provisioning Service](https://github.com/Azure-Samples/Microchip-PIC-IoT-Wx).

### <a name="introductory-samples"></a>Beispiele zur Einführung

Das SDK for Embedded C-Repository enthält [mehrere Beispiele](https://github.com/Azure/azure-sdk-for-c/tree/master/sdk/samples/iot#iot-hub-plug-and-play-sample) zur Verdeutlichung der Verwendung von IoT Plug & Play:

> [!NOTE]
> Diese Beispiele werden zu Schulungs- und Debugzwecken bei der Ausführung unter Windows und Linux gezeigt. In einem Produktionsszenario sind die Beispiele nur für eingeschränkte Geräte vorgesehen.

- [Beispiel für einen Thermostaten mit dem SDK for Embedded C](https://github.com/Azure/azure-sdk-for-c/blob/master/sdk/samples/iot/paho_iot_hub_pnp_sample.c)

- [Beispiel für eine Temperatursteuerung mit dem SDK for Embedded C](https://github.com/Azure/azure-sdk-for-c/blob/master/sdk/samples/iot/paho_iot_hub_pnp_component_sample.c)

## <a name="using-azure-rtos"></a>Verwenden von Azure RTOS

Azure-RTOS umfasst eine vereinfachte Schicht, die native Konnektivität zu Azure IoT-Clouddiensten hinzufügt. Diese Schicht bietet einen einfachen Mechanismus zum Verbinden eingeschränkter Geräte mit Azure IoT und verwendet zugleich die erweiterten Funktionen von Azure RTOS. Weitere Informationen finden Sie unter [Was ist Microsoft Azure RTOS](/azure/rtos/overview-rtos).

### <a name="toolchains"></a>Toolketten

Die Azure RTOS-Beispiele stehen mit verschiedenen Kombinationen aus IDE und Toolchain zur Verfügung, wie etwa:

- IAR: [Embedded Workbench](https://www.iar.com/iar-embedded-workbench/)-IDE von IAR
- GCC/CMake: Erstellen von Builds auf der Grundlage des Open Source-Buildsystems [CMake](https://cmake.org/) und der [GNU Arm Embedded Toolchain](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm).
- MCUExpresso: [MCUXpresso-IDE](https://www.nxp.com/design/software/development-software/mcuxpresso-software-and-tools-/mcuxpresso-integrated-development-environment-ide:MCUXpresso-IDE) von NXP
- STM32Cube: [STM32Cube-IDE](https://www.st.com/en/development-tools/stm32cubeide.html) von STMicroelectronic
- MPLAB: [MPLAB X-IDE](https://www.microchip.com/mplab/mplab-x-ide) von Microchip

### <a name="samples"></a>Beispiele

Beispiele, die den Einstieg für verschiedene Geräte mit Azure RTOS und IoT Plug & Play zeigen, finden Sie in der folgenden Tabelle:

Hersteller | Sicherungsmedium | Beispiele |
| --- | --- | --- |
| Microchip | [ATSAME54-XPRO](https://www.microchip.com/developmenttools/productdetails/atsame54-xpro) | [GCC/CMake](https://github.com/azure-rtos/getting-started/tree/master/Microchip/ATSAME54-XPRO) • [IAR](https://aka.ms/azrtos-sample/e54-iar) • [MPLAB](https://aka.ms/azrtos-sample/e54-mplab)
| MXCHIP | [AZ3166](https://aka.ms/iot-devkit) | [GCC/CMake](https://github.com/azure-rtos/getting-started/tree/master/MXChip/AZ3166)
| NXP | [MIMXRT1060-EVK](https://www.nxp.com/design/development-boards/i-mx-evaluation-and-development-boards/mimxrt1060-evk-i-mx-rt1060-evaluation-kit:MIMXRT1060-EVK) | [GCC/CMake](https://github.com/azure-rtos/getting-started/tree/master/NXP/MIMXRT1060-EVK) • [IAR](https://aka.ms/azrtos-sample/rt1060-iar) • [MCUXpresso](https://aka.ms/azrtos-sample/rt1060-mcuxpresso)
| STMicroelectronics | [32F746GDISCOVERY](https://www.st.com/en/evaluation-tools/32f746gdiscovery.html) | [IAR](https://aka.ms/azrtos-sample/f746g-iar) • [STM32Cube](https://aka.ms/azrtos-sample/f746g-cubeide)
| STMicroelectronics | [B-L475E-IOT01](https://www.st.com/en/evaluation-tools/b-l475e-iot01a.html) | [GCC/CMake](https://github.com/azure-rtos/getting-started/tree/master/STMicroelectronics/STM32L4_L4%2B) • [IAR](https://aka.ms/azrtos-sample/l4s5-iar) • [STM32Cube](https://aka.ms/azrtos-sample/l4s5-cubeide)
| STMicroelectronics | [B-L4S5I-IOT01](https://www.st.com/en/evaluation-tools/b-l4s5i-iot01a.html) | [GCC/CMake](https://github.com/azure-rtos/getting-started/tree/master/STMicroelectronics/STM32L4_L4%2B) • [IAR](https://aka.ms/azrtos-sample/l4s5-iar) • [STM32Cube](https://aka.ms/azrtos-sample/l4s5-cubeide)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit den Optionen für die Implementierung von IoT Plug & Play auf eingeschränkten Geräten vertraut gemacht haben, empfiehlt es sich, mehr über die [IoT Plug & Play Konventionen](concepts-convention.md) zu erfahren.