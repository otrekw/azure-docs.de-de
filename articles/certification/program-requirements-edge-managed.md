---
title: Anforderungen für die Edge Managed-Zertifizierung
description: Anforderungen für das Edge Managed-Zertifizierungsprogramm
author: cbroad
ms.author: cbroad
ms.topic: conceptual
ms.date: 03/15/2021
ms.custom: Edge Managed Certification Requirements
ms.service: certification
ms.openlocfilehash: 744f81d0544a765f60793ece1aa539c6c37e39cd
ms.sourcegitcommit: f5448fe5b24c67e24aea769e1ab438a465dfe037
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105975750"
---
# <a name="azure-certification-edge-managed"></a>Azure Edge Managed-Zertifizierung 

In diesem Dokument werden die gerätespezifischen Funktionen beschrieben, die im Azure Certified Device-Katalog dargestellt sind. Eine Funktion ist ein einzelnes Geräteattribut, das zur Beschreibung des Geräts verwendet werden kann. 

## <a name="program-purpose"></a>Zweck des Programms

Edge Managed-Zertifizierung, eine inkrementelle Zertifizierung über die Azure Certified Device-Basiszertifizierung hinaus. Edge Managed legt den Schwerpunkt auf Standards zur Geräteverwaltung für mit Azure verbundene Geräte und überprüft die IoT Edge-Laufzeitkompatibilität für Modulbereitstellung und -verwaltung. (Dieses Programm wurde zuvor als IoT Edge-Zertifizierungsprogramm bezeichnet.) 

Die Edge Managed-Zertifizierung überprüft die IoT Edge-Laufzeitkompatibilität für Modulbereitstellung und -verwaltung. Dieses Programm schafft Vertrauen in die Verwaltung von mit Azure verbundenen IoT-Geräten.

## <a name="requirements"></a>Anforderungen

Die Edge Managed-Zertifizierung setzt voraus, dass alle Anforderungen aus dem [Azure Certified Device Baseline-Programm](.\program-requirements-azure-certified-device.md) erfüllt werden.

**DPS: Der Zweck des Tests ist die Überprüfung mit einer der drei Nachweismethoden, ob das Gerät den IoT Hub Device Provisioning Service implementiert und unterstützt**

| **Name**                | AzureReady.DPS                                               |
| ----------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit** | Ignite (Vorschauversion)                                                |
| **Gilt für**          | Jedes Gerät                                      |
| **Betriebssystem**                  | Agnostisch                                                     |
| **Überprüfungstyp**     | Automatisiert                                                    |
| **Überprüfung**          | AICS überprüft, ob der Gerätecode DPS unterstützt. **1.** Der Benutzer muss eine der Nachweismethoden (X.509, TPM und SAS-Schlüssel) auswählen. **2.** Je nach gewählter Nachweismethode muss der Benutzer entsprechende Maßnahmen ergreifen, wie etwa **a)** Hochladen eines X.509-Zertifikats in den AICS-verwalteten DPS-Bereich **b)** Implementieren des SAS-Schlüssels oder Endorsement Keys auf dem Gerät. **3.** Anschließend betätigt der Benutzer die Schaltfläche „Verbinden“, um eine Verbindung mit dem AICS-verwalteten IoT Hub per DPS herzustellen                                                    |
| **Ressourcen**           |                                                      |
| **Für Azure empfohlen:**     | –                                                    |

## <a name="iot-edge"></a>IoT Edge

**Edge-Laufzeit vorhanden: Der Zweck dieses Tests ist es, sicherzustellen, dass das Gerät eine IoT Edge-Runtime ($edgehub und $edgeagent) mit ordnungsgemäßer Funktion enthält.**

| **Name**                | EdgeManaged.EdgeRT                                               |
| ----------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit** | Jetzt verfügbar                                                          |
| **Gilt für**          | IoT Edge-Gerät                                                   |
| **Betriebssystem**                  | [Betriebssystemebene 1 und 2](../iot-edge/support.md)                                                     |
| **Überprüfungstyp**     | Automatisiert                                                    |
| **Überprüfung**          | AICS überprüft die Bereitstellungsfähigkeit der installierten IoT Edge RT. **1.** Der Benutzer muss ein bestimmtes Betriebssystem angeben (Betriebssysteme, die nicht in der Liste von Ebene 1/2 aufgeführt sind, werden nicht akzeptiert) **2.** AICS generiert seine config.yaml-Datei und stellt ein kanonisches [simuliertes Temperatursensor-Edgemodul](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azure-iot.simulated-temperature-sensor?tab=Overview) bereit **3.** AICS überprüft, ob das Docker-kompatible Containersubsystem (Moby) auf dem Gerät installiert ist **4.** Das Testergebnis wird anhand der erfolgreichen Bereitstellung des simulierten Temperatursensor-Edgemoduls und der Funktion des Docker-kompatiblen Containersubsystems ermittelt                                                    |
| **Ressourcen**           | **a)** [AICS-Blog](https://azure.microsoft.com/en-in/blog/expanding-azure-iot-certification-service-to-support-azure-iot-edge-device-certification/), **b)** [Zertifizierungsschritte](./overview.md) (enthält alle zusätzlichen Ressourcen), **c)** [Anforderungen](./program-requirements-azure-certified-device.md) |
| **Für Azure empfohlen:**     | –                                                    |

### <a name="capability-template"></a>Funktionsvorlage:

**IoT Edge mühelose Einrichtung: Der Zweck des Tests ist es, sicherzustellen, dass das IoT Edge-Gerät mühelos einzurichten ist; außerdem wird während der physischen Geräteüberprüfung geprüft, ob die IoT Edge-Runtime vorinstalliert ist**

| **Name**                | EdgeManaged.PhysicalDevice                                             |
| ----------------------- | ------------------------------------------------------------ |
| **Zielverfügbarkeit** | Jetzt verfügbar (aktuell aufgrund von COVID-19 angehalten)                                            |
| **Gilt für**          | IoT Edge-Gerät                                                   |
| **Betriebssystem**                  | [Betriebssystemebene 1 und 2](../iot-edge/support.md)                                                     |
| **Überprüfungstyp**     | Manuell/Lab-überprüft                                                    |
| **Überprüfung**          | Der OEM muss das physische Gerät an die IoT-Verwaltung (HCL) einsenden. HCL führt die manuelle Überprüfung des zu prüfenden Geräts durch: **1.** EdgeRT verwendet das Moby-Subsystem (Version mit zulässiger Weiterverteilung). Nicht Docker **2.** Wählen Sie das neueste Edgemodul aus, um die Fähigkeit zur Edgebereitstellung zu überprüfen                                                     |
| **Ressourcen**           | **a)** [AICS-Blog](https://azure.microsoft.com/en-in/blog/expanding-azure-iot-certification-service-to-support-azure-iot-edge-device-certification/), **b)** [Zertifizierungsschritte](./overview.md), **c)** [Anforderungen](./program-requirements-azure-certified-device.md) |
| **Für Azure empfohlen:**     | Nicht zutreffend                                                    |