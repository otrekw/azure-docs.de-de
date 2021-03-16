---
title: Was ist Azure IoT Edge für Linux unter Windows? | Microsoft-Dokumentation
description: Übersicht über das Ausführen von Linux-IoT Edge-Modulen auf Windows 10-Geräten
author: kgremban
manager: philmea
ms.reviewer: twarwick
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 01/20/2021
ms.author: kgremban
monikerRange: =iotedge-2018-06
ms.openlocfilehash: 9d6bb9da67f66f46e5c72d5d64b8b6c111c98e08
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102215356"
---
# <a name="what-is-azure-iot-edge-for-linux-on-windows-preview"></a>Was ist Azure IoT Edge für Linux unter Windows (Vorschau)?

Azure IoT Edge für Linux unter Windows ermöglicht Ihnen das Ausführen von Linux-Workloads in Containern zusammen mit Windows-Anwendungen in Windows-IoT-Bereitstellungen. Unternehmen, die für die Unterstützung ihrer Edgegeräte Windows-IoT nutzen, können jetzt von den cloudnativen Analyselösungen profitieren, die in Linux erstellt werden.

Für IoT Edge für Linux unter Windows wird eine Linux-VM auf einem Windows-Gerät ausgeführt. Die Linux-VM wird mit der IoT Edge-Runtime vorinstalliert. Alle auf dem Gerät bereitgestellten IoT Edge-Module werden auf der VM ausgeführt. Gleichzeitig können Windows-Anwendungen, die auf dem Windows-Hostgerät ausgeführt werden, mit den auf der Linux-VM ausgeführten Modulen kommunizieren.

[Beginnen Sie](how-to-install-iot-edge-on-windows.md) heute mit der Vorschauversion.

>[!NOTE]
>Sie würden uns sehr bei der Verbesserung von Azure IoT Edge für Linux unter Windows helfen, wenn Sie an unserer [Produktumfrage](https://aka.ms/AzEFLOW-Registration) teilnehmen und uns von Ihrem IoT Edge-Hintergrund und Ihren Zielen berichten. Sie können sich bei dieser Umfrage auch für zukünftige Ankündigungen zu Azure IoT Edge für Linux unter Windows registrieren.

## <a name="components"></a>Komponenten

IoT Edge für Linux unter Windows nutzt die folgenden Komponenten für die parallele Ausführung von Linux- und Windows-Workloads und deren nahtlose Kommunikation:

* **Eine Linux-VM, auf der Azure IoT Edge ausgeführt wird:** Eine Linux-VM auf der Grundlage des Microsoft-eigenen [Mariner](https://github.com/microsoft/CBL-Mariner)-Betriebssystems wird mit der IoT Edge-Runtime erstellt und als Umgebung für IoT Edge-Workloads der 1. Ebene validiert.

* **Windows Admin Center:** Eine IoT Edge-Erweiterung für Windows Admin Center ermöglicht die Installation, Konfiguration und Diagnose von IoT Edge auf der Linux-VM. Mit Windows Admin Center kann IoT Edge für Linux unter Windows auf dem lokalen Gerät bereitgestellt werden. Außerdem können Sie damit eine Verbindung mit Zielgeräten herstellen und diese remote verwalten.

* **Microsoft Update:** Durch die Integration in Microsoft Update werden die Komponenten der Windows-Runtime, die Mariner-Linux-VM und IoT Edge auf dem neuesten Stand gehalten.

![Windows und die Linux-VM werden parallel ausgeführt, während Windows Admin Center beide Komponenten steuert.](./media/iot-edge-for-linux-on-windows/architecture-and-communication.png)

Aufgrund der bidirektionalen Kommunikation zwischen Windows-Prozess und Linux-VM können Windows-Prozesse Benutzeroberflächen oder Hardwareproxys für Workloads bereitstellen, die in Linux-Containern ausgeführt werden.

## <a name="samples"></a>Beispiele

Der Fokus von IoT Edge für Linux unter Windows liegt ganz klar auf der Interoperabilität zwischen den Linux- und den Windows-Komponenten.

Beispiele für die Kommunikation zwischen Windows-Anwendungen und IoT Edge-Modulen finden Sie in den [Windows 10 IoT-Beispielen](https://github.com/microsoft/Windows-IoT-Samples).

## <a name="public-preview"></a>Public Preview

IoT Edge für Linux unter Windows befindet sich derzeit in der [öffentlichen Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Die Installations- und Verwaltungsprozesse können sich von denen bei allgemeiner Verfügbarkeit unterscheiden.

Derzeit verwendet IoT Edge für Linux unter Windows die Windows-Insider-Vorschauversion von Windows Admin Center. Weitere Informationen zum Windows-Insider-Programm und zur Registrierung finden Sie unter [Was ist das Windows-Insider-Programm?](https://insider.windows.com/about-windows-insider-program).

## <a name="support"></a>Support

Nutzen Sie die Kanäle des IoT Edge-Supports und für Feedback, um Unterstützung bei IoT Edge für Linux unter Windows zu erhalten.

**Melden von Fehlern:** Fehler können auf der Seite [Probleme](https://github.com/azure/iotedge/issues) des Open-Source-Projekts für IoT Edge gemeldet werden. 

**Microsoft-Kundendienstteam:** Benutzer mit einem [Supportplan](https://azure.microsoft.com/support/plans/) können direkt über das [Azure-Portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac) ein Supportticket erstellen, um sich an das Microsoft-Kundendienstteam zu wenden.

**Anfragen von Features:** Angefragte Features werden von Azure IoT Edge über die [User Voice-Seite](https://feedback.azure.com/forums/907045-azure-iot-edge) des Produkts verfolgt.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich für weitere Informationen und ein Beispiel aus der Praxis das Video [IoT Edge for Linux on Windows 10 IoT Enterprise](https://aka.ms/EFLOWPPC9) (IoT Edge für Linux unter Windows 10 IoT Enterprise).

Führen Sie die Schritte unter [Installieren und Bereitstellen von Azure IoT Edge für Linux auf einem Windows-Gerät](how-to-install-iot-edge-on-windows.md) aus, um ein Gerät mit IoT Edge für Linux unter Windows einzurichten.
