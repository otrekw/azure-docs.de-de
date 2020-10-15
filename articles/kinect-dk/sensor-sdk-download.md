---
title: Download des Sensor-SDK für Azure Kinect
description: Hier erfahren Sie, wie Sie das Sensor-SDK für Azure Kinect unter Windows und Linux herunterladen und installieren.
author: Brent-A
ms.author: brenta
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Azure, Kinect, SDK, Update herunterladen, aktuell, verfügbar, installieren
ms.openlocfilehash: 48cdd35a80d68a7ec0d900639c0fca3a2c66787e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86171787"
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

[Microsoft Installer](https://download.microsoft.com/download/3/d/6/3d6d9e99-a251-4cf3-8c6a-8e108e960b4b/Azure%20Kinect%20SDK%201.4.1.exe) | [GitHub-Quellcode](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/issues/1093)

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
