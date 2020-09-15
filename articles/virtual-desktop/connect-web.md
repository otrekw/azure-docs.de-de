---
title: Herstellen einer Verbindung mit Windows Virtual Desktop über den Webclient – Azure
description: Informationen zum Herstellen einer Verbindung mit Windows Virtual Desktop mithilfe des Webclients.
author: Heidilohr
ms.topic: how-to
ms.date: 09/24/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: c8a4b22966c3d4db268e212bb3f2d1bbb78fee74
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89400635"
---
# <a name="connect-to-windows-virtual-desktop-with-the-web-client"></a>Herstellen einer Verbindung mit Windows Virtual Desktop über den Webclient

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Windows Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/connect-web-2019.md).

Mit dem Webclient können Sie über einen Webbrowser auf Ihre Ressourcen von Windows Virtual Desktop zugreifen, ohne den langwierigen Installationsvorgang durchführen zu müssen.

>[!NOTE]
>Der Webclient verfügt derzeit nicht über Unterstützung für Mobilgerät-Betriebssysteme.

## <a name="supported-operating-systems-and-browsers"></a>Unterstützte Betriebssysteme und Browser

Es sollten alle HTML5-fähigen Browser funktionieren, aber hier sind die Betriebssysteme und Browser angegeben, die offiziell unterstützt werden.

| Browser           | Unterstütztes Betriebssystem                     | Notizen               |
|-------------------|----------------------------------|---------------------|
| Microsoft Edge    | Windows                          |                     |
| Internet Explorer | Windows                          | Version 11 oder höher |
| Apple Safari      | macOS                            |                     |
| Mozilla Firefox   | Windows, macOS, Linux            | Version 55 oder höher |
| Google Chrome     | Windows, macOS, Linux, Chrome OS |                     |

## <a name="access-remote-resources-feed"></a>Zugreifen auf den Feed für Remoteressourcen

Navigieren Sie in einem Browser unter <https://rdweb.wvd.microsoft.com/arm/webclient> zu der mit dem Azure Resource Manager integrierten Version des Windows Virtual Desktop-Webclients, und melden Sie sich mit Ihrem Benutzerkonto an.

>[!NOTE]
>Wenn Sie Windows Virtual Desktop (klassisch) ohne Azure Resource Manager-Integration verwenden, stellen Sie stattdessen eine Verbindung mit Ihren Ressourcen unter <https://rdweb.wvd.microsoft.com/webclient> her.
>
> Verwenden Sie bei Nutzung des US Gov-Portals <https://rdweb.wvd.azure.us/arm/webclient/index.html>.

>[!NOTE]
>Wenn Sie sich bereits mit einem anderen Azure Active Directory-Konto angemeldet haben als dem, das Sie für Windows Virtual Desktop verwenden möchten, sollten Sie sich entweder abmelden oder ein privates Browserfenster verwenden.

Nach dem Anmelden sollte eine Liste mit Ressourcen angezeigt werden. Sie können Ressourcen starten, indem Sie sie auf der Registerkarte **Alle Ressourcen** wie eine normale App auswählen.

## <a name="using-an-input-method-editor"></a>Verwenden eines Eingabemethoden-Editors

Der Webclient unterstützt die Verwendung eines Eingabemethoden-Editors (Input Method Editor, IME) in der Remotesitzung in Version **1.0.21.16 oder höher**. Das Language Pack für die Tastatur, das Sie in der Remotesitzung verwenden möchten, muss auf dem virtuellen Hostcomputer installiert sein. Weitere Informationen zum Einrichten von Language Packs in der Remotesitzung finden Sie unter [Hinzufügen von Language Packs zu einem Image für Windows 10 (mehrere Sitzungen)](language-packs.md).

So aktivieren Sie die IME-Eingabe mithilfe des Webclients:

1. Wechseln Sie vor dem Herstellen einer Verbindung mit der Remotesitzung zum Bereich **Einstellungen** des Webclients.

2. Schalten Sie die Einstellung **Enable Input Method Editor** (Eingabemethoden-Editor aktivieren) auf **Ein** um.

3. Wählen Sie im Dropdownmenü die Tastatur aus, die Sie in der Remotesitzung verwenden möchten.

4. Stellen Sie eine Verbindung mit der Remotesitzung her.

Der Webclient unterdrückt das lokale IME-Fenster, wenn der Fokus auf der Remotesitzung liegt. Wenn Sie die IME-Einstellungen ändern, nachdem Sie eine Verbindung mit der Remotesitzung hergestellt haben, hat dies keine Auswirkungen.

>[!NOTE]
>Ist das Language Pack nicht auf dem virtuellen Hostcomputer installiert, wird für die Remotesitzung standardmäßig die englische Tastatur (USA) verwendet.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung des Webclients finden Sie unter [Erste Schritte mit dem Webclient](/windows-server/remote/remote-desktop-services/clients/remote-desktop-web-client).
