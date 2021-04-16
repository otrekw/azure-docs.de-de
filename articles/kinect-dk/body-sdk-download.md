---
title: Download des Azure Kinect Body Tracking SDK
description: Erfahren Sie, wie Sie die einzelnen Versionen des Azure Kinect Sensor SDK unter Windows oder Linux herunterladen.
author: qm13
ms.author: quentinm
ms.prod: kinect-dk
ms.date: 03/18/2021
ms.topic: conceptual
keywords: Azure, Kinect, SDK, Update herunterladen, aktuell, verfügbar, installieren, Körper, Body Tracking
ms.openlocfilehash: 60018df56433785f3cb723dd54cc96a8e5289b67
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104654491"
---
# <a name="download-azure-kinect-body-tracking-sdk"></a>Download des Azure Kinect Body Tracking SDK

In diesem Dokument finden Sie Links zur Installation der einzelnen Versionen des Azure Kinect Body Tracking SDK.

## <a name="azure-kinect-body-tracking-sdk-contents"></a>Inhalt des Azure Kinect Body Tracking SDK

- Header und Bibliotheken zum Erstellen einer Body Tracking-Anwendung mit Azure Kinect DK
- Weitervertreibbare DLLs, die von Body Tracking-Anwendungen zur Verwendung von Azure Kinect DK benötigt werden
- Beispielanwendungen für das Body Tracking

## <a name="windows-download-links"></a>Downloadlinks für Windows

Version       | Download
--------------|----------
1.1.0 | [msi](https://www.microsoft.com/en-us/download/details.aspx?id=102901)
1.0.1 | [msi](https://www.microsoft.com/en-us/download/details.aspx?id=100942) [nuget](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/1.0.1)
1.0.0 | [msi](https://www.microsoft.com/en-us/download/details.aspx?id=100848) [nuget](https://www.nuget.org/packages/Microsoft.Azure.Kinect.BodyTracking/1.0.0)

## <a name="linux-installation-instructions"></a>Linux-Installationsanweisungen

Derzeit werden nur die Distributionen Ubuntu 18.04 und 20.04 unterstützt. Weitere Informationen zum Anfordern der Unterstützung für andere Distributionen finden Sie auf [dieser Seite](https://aka.ms/azurekinectfeedback).

Zunächst müssen Sie das [Paketrepository von Microsoft](https://packages.microsoft.com/) gemäß [diesen Anweisungen](/windows-server/administration/linux-package-repository-for-microsoft-software) konfigurieren.

Das Paket `libk4abt<major>.<minor>-dev` enthält die Header und CMake-Dateien für die Erstellung auf der Grundlage von `libk4abt`.
Das Paket `libk4abt<major>.<minor>` enthält die freigegebenen Objekte, die erforderlich sind, um von `libk4abt` abhängige ausführbare Dateien auszuführen. Es enthält auch den Beispielviewer.

Für die allgemeinen Tutorials ist das Paket `libk4abt<major>.<minor>-dev` erforderlich. Führen Sie zum Installieren Folgendes aus:

`sudo apt install libk4abt<major>.<minor>-dev`

Wurde der Befehl erfolgreich ausgeführt, kann das SDK verwendet werden.

> [!NOTE]
> Merken Sie sich bei der Installation des SDK den Installationspfad. Beispiel: C:\Programme\Azure Kinect Body Tracking SDK 1.0.0. In diesem Pfad finden Sie die Beispiele, auf die in den entsprechenden Artikeln verwiesen wird.
> Body Tracking-Beispiele befinden sich im Repository „Azure-Kinect-Samples“ im Ordner [body-tracking-samples](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples). Dort finden Sie die Beispiele, auf die in den entsprechenden Artikeln verwiesen wird.

## <a name="change-log"></a>Änderungsprotokoll

### <a name="v110"></a>1\.1.0
* [Feature] Die DirectML- (nur Windows) und TensorRT-Ausführung des Posenschätzungsmodells werden jetzt unterstützt. Weitere Informationen finden Sie in den FAQ zu neuen Ausführungsumgebungen.
* [Feature] `model_path` wurde zur Struktur `k4abt_tracker_configuration_t` hinzugefügt. Ermöglicht es Benutzern, den Pfadnamen für das Posenschätzungsmodell anzugeben. Standardmäßig wird das Standard-Posenschätzungsmodell `dnn_model_2_0_op11.onnx` im aktuellen Verzeichnis verwendet.
* [Feature] Das Lite-Posenschätzungsmodell `dnn_model_2_0_lite_op11.onnx` wurde eingeschlossen. Dieses Modell bietet eine ungefähr zweimal höhere Leistung auf Kosten einer um ungefähr 5 % geringeren Genauigkeit.
* [Feature] Überprüfte Beispiele werden mit Visual Studio 2019 kompiliert, und Beispiele wurden für dieses Release aktualisiert.
* [Breaking Change] Aktualisierung von ONNX Runtime 1.6 mit Unterstützung für die CPU-, CUDA 11.1-, DirectML- (nur Windows) und TensorRT 7.2.1-Ausführungsumgebung. Erfordert ein Update des NVIDIA-Treibers auf R455 oder höher.
* [Breaking Change] Keine NuGet-Installation.
* [Programmfehlerbehebung] GPUs der NVIDIA RTX 30xx-Serie werden jetzt unterstützt: [Link](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1481)
* [Programmfehlerbehebung] AMD- und integrierte Intel-GPUs (nur Windows) werden jetzt unterstützt: [Link](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1481)
* [Programmfehlerbehebung] Update auf CUDA 11.1: [Link](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1125)
* [Programmfehlerbehebung] Update auf Sensor SDK 1.4.1: [Link](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1248)

### <a name="v101"></a>v1.0.1
* [Fehlerbehebung] Es wurde der Fehler behoben, dass das SDK abstürzt, wenn „onnxruntime.dll“ aus dem Pfad im Windows-Build 19025 oder höher geladen wird: [Link](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/932)

### <a name="v100"></a>v1.0.0
* [Feature] Dem MSI-Installationsprogramm wurde ein C#-Wrapper hinzugefügt.
* [Fehlerbehebung] Es wurde der Fehler behoben, dass Kopfdrehungen nicht richtig erkannt wurden: [Link](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/997)
* [Fehlerbehebung] Es wurde der Fehler behoben, dass die CPU-Auslastung auf Linux-Computern bis zu 100 % beträgt: [Link](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1007)
* [Beispiele] Dem Beispielrepository wurden zwei Beispiele hinzugefügt. Beispiel 1 zeigt, wie Body Tracking-Ergebnisse aus dem Tiefenraum in den Farbraum transformiert werden: [Link](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples/camera_space_transform_sample). Beispiel 2 demonstriert die Ermittlung der Bodenebene: [Link](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples/floor_detector_sample)

## <a name="next-steps"></a>Nächste Schritte

- [Azure Kinect DK: Übersicht](about-azure-kinect-dk.md)

- [Schnellstart: Einrichten Ihres Azure Kinect DK-Geräts](set-up-azure-kinect-dk.md)

- [Einrichten von Azure Kinect Body Tracking](body-sdk-setup.md)