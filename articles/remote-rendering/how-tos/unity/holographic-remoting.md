---
title: Verwenden von Holographic Remoting und Remote Rendering in Unity
description: Verwenden der Holographic Remoting-Vorschau in Kombination mit Azure Remote Rendering
author: christophermanthei
ms.author: chmant
ms.date: 03/23/2020
ms.topic: how-to
ms.openlocfilehash: ac47a2922e92233f0acabf75817a712671306bc1
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80679204"
---
# <a name="use-holographic-remoting-and-remote-rendering-in-unity"></a>Verwenden von Holographic Remoting und Remote Rendering in Unity

[Holographic Remoting](https://docs.microsoft.com/windows/mixed-reality/holographic-remoting-player) und Azure Remote Rendering können in einer Anwendung nicht gemeinsam verwendet werden. Wenn Holographic Remoting nicht verfügbar ist, gilt dies ebenfalls für den [Unity-Wiedergabemodus](https://docs.microsoft.com/windows/mixed-reality/unity-play-mode).

Bei jedem Ausführen des Unity-Editors kann nur einer der beiden Modi verwendet werden. Um den anderen zu verwenden, müssen Sie Unity neu starten.

## <a name="use-unity-play-mode-to-preview-on-hololens-2"></a>Verwenden des Unity-Wiedergabemodus für die Vorschau in HoloLens 2

 Der Unity-Wiedergabemodus kann dennoch verwendet werden, z. B. um die Benutzeroberfläche der Anwendung zu testen. ARR darf jedoch keinesfalls initialisiert werden. Andernfalls stürzt es ab.

## <a name="use-a-wmr-vr-headset-to-preview-on-desktop"></a>Verwenden eines WMR-VR-Headsets zum Anzeigen der Vorschau auf dem Desktop

Wenn ein Windows Mixed Reality-VR-Headset vorhanden ist, kann es für die Vorschau in Unity verwendet werden. In diesem Fall kann ARR initialisiert werden, es ist jedoch nicht möglich, eine Verbindung mit einer Sitzung herzustellen, während das WMR-Headset verwendet wird.
