---
title: 'Herstellen einer Verbindung mit Windows Virtual Desktop (Windows 10 oder 7): Azure'
description: Informationen zum Herstellen einer Verbindung mit Windows Virtual Desktop mithilfe des Windows Desktop-Clients.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/19/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 5d333b2f8bbb660e9fd71339159d942c0520f4f4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85201547"
---
# <a name="connect-with-the-windows-desktop-client"></a>Herstellen einer Verbindung mit dem Windows-Desktopclient

> Gilt für: Windows 7, Windows 10 und Windows 10 IoT Enterprise

Mit dem Windows Desktop-Client können Sie auf Geräten mit Windows 7, Windows 10 und Windows 10 IoT Enterprise auf Windows Virtual Desktop-Ressourcen zugreifen.

>[!NOTE]
>Der Windows-Client verwendet standardmäßig automatisch das Windows Virtual Desktop-Release aus dem Herbst 2019. Wenn der Client jedoch erkennt, dass der Benutzer auch über Azure Resource Manager-Ressourcen verfügt, werden die Ressourcen automatisch hinzugefügt, oder der Benutzer wird benachrichtigt, dass sie verfügbar sind.

> [!IMPORTANT]
> Windows Virtual Desktop unterstützt nicht den RemoteApp-Client und den Client für Desktopverbindungen (RADC). Der Client für Remotedesktopverbindung (MSTSC) wird ebenfalls nicht unterstützt.

> [!IMPORTANT]
> Windows Virtual Desktop verfügt derzeit nicht über Unterstützung für den Remotedesktopclient aus dem Windows Store. Die Unterstützung für diesen Client wird in einer zukünftigen Version enthalten sein.

## <a name="install-the-windows-desktop-client"></a>Installieren des Windows Desktop-Clients

Wählen Sie den Client aus, der Ihrer Windows-Version entspricht:

- [Windows (64-Bit)](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows (32-Bit)](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

Sie können den Client für den aktuellen Benutzer installieren. Dafür sind keine Administratorrechte erforderlich. Der Administrator kann den Client aber auch so installieren und konfigurieren, dass alle Benutzer auf dem Gerät darauf zugreifen können.

Nach der Installation kann der Client über das Startmenü gestartet werden, indem Sie nach **Remote Desktop** suchen.

## <a name="subscribe-to-a-feed"></a>Abonnieren eines Feeds

Rufen Sie die Liste mit den verfügbaren verwalteten Ressourcen ab, indem Sie den von Ihrem Administrator bereitgestellten Feed abonnieren. Durch das Abonnieren werden die Ressourcen auf Ihrem lokalen PC verfügbar gemacht.

Abonnieren Sie einen Feed wie folgt:

1. Öffnen Sie den Windows Desktop-Client.
2. Wählen Sie auf der Hauptseite **Abonnieren** aus, um eine Verbindung mit dem Dienst herzustellen und Ihre Ressourcen abzurufen.
3. Melden Sie sich mit Ihrem Benutzerkonto an, wenn Sie dazu aufgefordert werden.

Nachdem Sie sich erfolgreich angemeldet haben, wird eine Liste der Ressourcen angezeigt, auf die Sie zugreifen können.

Sie können zwei Methoden nutzen, um Ressourcen zu starten.

- Doppelklicken Sie auf der Hauptseite des Clients auf eine Ressource, um sie zu starten.
- Starten Sie eine Ressource über das Startmenü, wie Sie dies sonst auch für andere Apps tun.
  - Sie können auch in der Suchleiste nach Apps suchen.

Nachdem Sie einen Feed abonniert haben, wird der Inhalt des Feeds in regelmäßigen Abständen automatisch aktualisiert. Ressourcen können basierend auf Änderungen durch Ihren Administrator hinzugefügt, geändert oder entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung des Windows Desktop-Clients finden Sie unter [Erste Schritte mit dem Windows Desktop-Client](/windows-server/remote/remote-desktop-services/clients/windowsdesktop/).
