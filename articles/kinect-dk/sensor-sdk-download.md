---
title: Download des Sensor-SDK für Azure Kinect
description: Hier erfahren Sie, wie Sie das Sensor-SDK für Azure Kinect unter Windows und Linux herunterladen und installieren.
author: Brent-A
ms.author: brenta
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Azure, Kinect, SDK, Update herunterladen, aktuell, verfügbar, installieren
ms.openlocfilehash: 2c23977c7e01a9137b72b44d1bdc0e1373bafa0a
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "84334268"
---
# <a name="azure-kinect-sensor-sdk-download"></a>Download des Sensor-SDK für Azure Kinect

Diese Seite enthält die Downloadlinks für alle Versionen des Sensor-SDK für Azure Kinect. Das Installationsprogramm stellt alle erforderlichen Dateien für die Entwicklung für Azure Kinect bereit.

## <a name="azure-kinect-sensor-sdk-contents"></a>Inhalt des Sensor-SDK für Azure Kinect

- Header und Bibliotheken zum Erstellen einer Anwendung mit dem Azure Kinect DK
- Weitervertreibbare DLLs, die von Anwendungen benötigt werden, die das Azure Kinect DK nutzen
- [Azure Kinect Viewer](azure-kinect-viewer.md)
- [Azure Kinect-Rekorder](azure-kinect-recorder.md)
- [Azure Kinect-Firmwaretool](azure-kinect-firmware-tool.md)

## <a name="windows-download-link"></a>Windows-Downloadlink

[Microsoft Installer](https://download.microsoft.com/download/4/5/a/45aa3917-45bf-4f24-b934-5cff74df73e1/Azure%20Kinect%20SDK%201.4.0.exe) | [GitHub-Quellcode](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1093)

> [!NOTE]
> Merken Sie sich bei der Installation des SDK den Installationspfad. Beispiel: C:\Program Files\Azure Kinect SDK 1.2. Die Tools, auf die in diesem Artikel verwiesen wird, befinden sich in diesem Pfad.

Frühere Versionen des Azure Kinect Sensor SDK und der Firmware finden Sie auf [GitHub](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md).

## <a name="linux-installation-instructions"></a>Linux-Installationsanweisungen

Derzeit wird nur die Distribution Ubuntu 18.04 unterstützt. Weitere Informationen zum Anfordern der Unterstützung für andere Distributionen finden Sie auf [dieser Seite](https://aka.ms/azurekinectfeedback).

Zunächst müssen Sie das [Paketrepository von Microsoft](https://packages.microsoft.com/) gemäß [diesen Anweisungen](https://docs.microsoft.com/windows-server/administration/linux-package-repository-for-microsoft-software) konfigurieren.

Nun können Sie die erforderlichen Pakete installieren. Das Paket `k4a-tools` enthält [Azure Kinect Viewer](azure-kinect-viewer.md), den [Azure Kinect-Rekorder](record-sensor-streams-file.md) und das [Azure Kinect-Firmwaretool](azure-kinect-firmware-tool.md). Führen Sie zum Installieren Folgendes aus:

 `sudo apt install k4a-tools`

 Das Paket `libk4a<major>.<minor>-dev` enthält die Header und CMake-Dateien für die Erstellung auf der Grundlage von `libk4a`.
Das Paket `libk4a<major>.<minor>` enthält die freigegebenen Objekte, die erforderlich sind, um von `libk4a` abhängige ausführbare Dateien auszuführen.

 Für die allgemeinen Tutorials ist das Paket `libk4a<major>.<minor>-dev` erforderlich. Führen Sie zum Installieren Folgendes aus:

 `sudo apt install libk4a1.1-dev`

Wurde der Befehl erfolgreich ausgeführt, kann das SDK verwendet werden.

## <a name="change-log-and-older-versions"></a>Änderungsprotokoll und ältere Versionen

Das Änderungsprotokoll für das Sensor-SDK für Azure Kinect finden Sie [hier](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/CHANGELOG.md).

Ältere Versionen des Sensor-SDK für Azure Kinect finden Sie [hier](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md).

## <a name="next-steps"></a>Nächste Schritte

[Schnellstart: Einrichten Ihres Azure Kinect DK-Geräts](set-up-azure-kinect-dk.md)
