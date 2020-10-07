---
title: Bereitstellen des Unity-Beispiels für den Desktop
description: Schnellstartanleitung mit Informationen zum Bereitstellen des Unity-Beispiels auf einem Desktop-PC
author: christophermanthei
ms.author: chmant
ms.date: 03/20/2020
ms.topic: quickstart
ms.openlocfilehash: 26531e80292a53400671ac3507b47371c1fbbcad
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "88796815"
---
# <a name="quickstart-deploy-unity-sample-to-desktop"></a>Schnellstart: Bereitstellen des Unity-Beispiels für den Desktop

In dieser Schnellstartanleitung erfahren Sie, wie Sie die Schnellstartbeispiel-App für Unity auf einem Desktop-PC bereitstellen und ausführen.

In dieser Schnellstartanleitung wird Folgendes vermittelt:

> [!div class="checklist"]
>
>* Erstellen der Schnellstartbeispiel-App für den Desktop
>* Bereitstellen des Beispiels auf einem PC
>* Ausführen des Beispiels auf einem PC

## <a name="prerequisites"></a>Voraussetzungen

In dieser Schnellstartanleitung stellen Sie das Beispielprojekt aus [Schnellstart: Rendern eines Modells mit Unity](render-model.md) bereit.

Stellen Sie sicher, dass Ihre Anmeldeinformationen ordnungsgemäß mit der Szene gespeichert werden und Sie über den Unity-Editor eine Verbindung mit einer Sitzung herstellen können.

## <a name="disable-virtual-reality-support"></a>Deaktivieren der Virtual Reality-Unterstützung

Derzeit werden nur flache Desktop-Apps auf dem Desktop unterstützt. Die VR-Unterstützung muss daher deaktiviert werden.

1. Öffnen Sie *Edit > Project Settings...* (Bearbeiten > Projekteinstellungen).
1. Wählen Sie auf der linken Seite **Player** aus.
1. Wählen Sie die Registerkarte **Universal Windows Platform settings** (UWP-Einstellungen) aus.
1. Erweitern Sie den Eintrag **XR Settings** (XR-Einstellungen).
1. Deaktivieren Sie **Virtual Reality Supported** (Unterstützung für virtuelle Realität).\
    ![Playereinstellungen](./media/unity-disable-xr.png)
1. Erweitern Sie oberhalb von *XR Settings* (XR-Einstellungen) die Option **Publishing Settings** (Veröffentlichungseinstellungen).
1. Vergewissern Sie sich unter **Supported Device Families** (Unterstützte Gerätefamilien), ob **Desktop** aktiviert ist.

## <a name="build-the-sample-project"></a>Erstellen des Beispielprojekts

1. Öffnen Sie *File > Build Settings* (Datei > Buildeinstellungen).
1. Ändern Sie *Plattform* in **Universelle Windows-Plattform**. (**Eigenständiger PC** wird ebenfalls unterstützt, aber hier nicht verwendet. Siehe [Plattformeinschränkungen](../reference/limits.md#platform-limitations))
1. Legen Sie *Target Device* (Zielgerät) auf **PC** fest.
1. Legen Sie *Architecture* (Architektur) auf **x86** fest.
1. Legen Sie *Build Type* (Buildtyp) auf **D3D Project** (D3D-Projekt) fest.\
  ![Buildeinstellungen](./media/unity-build-settings-pc.png)
1. Wählen Sie **Switch to Platform** (Zu Plattform wechseln) aus.
1. Wenn Sie auf **Build** (Erstellen) oder „Build And Rund“ (Erstellen und ausführen) klicken, werden Sie zur Auswahl eines Ordners aufgefordert, in dem die Projektmappe gespeichert werden soll.
1. Öffnen Sie die generierte Datei **Quickstart.sln** mit Visual Studio.
1. Ändern Sie die Konfiguration in **Release** und **x86**.
1. Ändern Sie den Debuggermodus in **Local Machine** (Lokaler Computer).\
  ![Projektmappenkonfiguration](./media/unity-deploy-config-pc.png)
1. Erstellen Sie die Projektmappe.

## <a name="launch-the-sample-project"></a>Starten des Beispielprojekts

Starten Sie den Debugger in Visual Studio (F5). Die App wird automatisch auf dem PC bereitgestellt.

Daraufhin sollte die Beispiel-App gestartet und anschließend eine neue Sitzung initiiert werden. Nach einer Weile ist die Sitzung bereit, und das remote gerenderte Modell wird vor Ihnen angezeigt.
Wenn Sie das Beispiel später ein zweites Mal starten möchten, ist das nun auch über das Startmenü möglich.

## <a name="next-steps"></a>Nächste Schritte

In der nächsten Schnellstartanleitung wird das Konvertieren eines benutzerdefinierten Modells erläutert.

> [!div class="nextstepaction"]
> [Schnellstart: Konvertieren eines Modells für das Rendering](convert-model.md)
