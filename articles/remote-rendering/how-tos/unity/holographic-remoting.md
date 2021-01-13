---
title: Verwenden von Holographic Remoting und Remote Rendering in Unity
description: Verwenden der Holographic Remoting-Vorschau in Kombination mit Azure Remote Rendering
author: christophermanthei
ms.author: chmant
ms.date: 03/23/2020
ms.topic: how-to
ms.openlocfilehash: bd69710b6e4404a76d3cca385b6c07ea7c1f3f5c
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92201816"
---
# <a name="use-holographic-remoting-and-remote-rendering-in-unity"></a>Verwenden von Holographic Remoting und Remote Rendering in Unity

[Holographic Remoting](/windows/mixed-reality/holographic-remoting-player) und Azure Remote Rendering können in einer Anwendung nicht gemeinsam verwendet werden. Wenn Holographic Remoting nicht verfügbar ist, gilt dies ebenfalls für den [Unity-Wiedergabemodus](/windows/mixed-reality/unity-play-mode).

Bei jedem Ausführen des Unity-Editors kann nur einer der beiden Modi verwendet werden. Um den anderen zu verwenden, müssen Sie Unity neu starten.

## <a name="use-unity-play-mode-to-preview-on-hololens-2"></a>Verwenden des Unity-Wiedergabemodus für die Vorschau in HoloLens 2

 Der Unity-Wiedergabemodus kann dennoch verwendet werden, z. B. um die Benutzeroberfläche der Anwendung zu testen. ARR darf jedoch keinesfalls initialisiert werden. Andernfalls stürzt es ab.

## <a name="use-a-wmr-vr-headset-to-preview-on-desktop"></a>Verwenden eines WMR-VR-Headsets zum Anzeigen der Vorschau auf dem Desktop

Wenn ein Windows Mixed Reality-VR-Headset vorhanden ist, kann es für die Vorschau in Unity verwendet werden. In diesem Fall kann ARR initialisiert werden, es ist jedoch nicht möglich, eine Verbindung mit einer Sitzung herzustellen, während das WMR-Headset verwendet wird.