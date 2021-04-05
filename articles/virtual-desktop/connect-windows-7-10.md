---
title: 'Herstellen einer Verbindung mit Windows Virtual Desktop (Windows 10 oder 7): Azure'
description: Informationen zum Herstellen einer Verbindung mit Windows Virtual Desktop mithilfe des Windows Desktop-Clients.
author: Heidilohr
ms.topic: how-to
ms.date: 09/22/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 59379dd9c3a41729466de269b52dfd3fb206eea9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97368714"
---
# <a name="connect-with-the-windows-desktop-client"></a>Herstellen einer Verbindung mit dem Windows-Desktopclient

> Gilt für: Windows 10, Windows 10 IoT Enterprise und Windows 7

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Windows Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/connect-windows-7-10-2019.md).

Mit dem Windows Desktop-Client können Sie auf Geräten mit Windows 10, Windows 10 IoT Enterprise und Windows 7 auf Windows Virtual Desktop-Ressourcen zugreifen. Der Client unterstützt weder Windows 8 noch Windows 8.1.

## <a name="install-the-windows-desktop-client"></a>Installieren des Windows Desktop-Clients

Wählen Sie den Client aus, der Ihrer Windows-Version entspricht:

- [Windows (64-Bit)](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows 32-Bit](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

Sie können den Client für den aktuellen Benutzer installieren. Dafür sind keine Administratorrechte erforderlich. Der Administrator kann den Client aber auch so installieren und konfigurieren, dass alle Benutzer auf dem Gerät darauf zugreifen können.

Nach der Installation kann der Client über das Startmenü gestartet werden, indem Sie nach **Remote Desktop** suchen.

> [!IMPORTANT]
> Windows Virtual Desktop unterstützt nicht den RemoteApp-Client und den Client für Desktopverbindungen (RADC). Der Client für Remotedesktopverbindung (MSTSC) wird ebenfalls nicht unterstützt.

## <a name="subscribe-to-a-workspace"></a>Abonnieren eines Arbeitsbereichs

Es gibt zwei Möglichkeiten zum Abonnieren eines Arbeitsbereichs. Der Client kann versuchen, die Ressourcen, die dir über dein Geschäfts- oder Schul-/Unikonto zur Verfügung stehen, zu ermitteln, oder du kannst die URL, unter der deine Ressourcen zu finden sind, direkt angeben, falls der Client sie nicht finden kann. Sobald du einen Arbeitsbereich abonniert hast, kannst du Ressourcen mit einer der folgenden Methoden starten:

- Wechsle zum Connection Center, und doppelklicke auf eine Ressource, um sie zu starten.
- Alternativ kannst du zum Startmenü navigieren und nach einem Ordner mit dem Namen des Arbeitsbereichs suchen oder den Namen der Ressource in die Suchleiste eingeben.

### <a name="subscribe-with-a-user-account"></a>Abonnieren mit einem Benutzerkonto

1. Klicken Sie auf der Hauptseite des Clients auf **Abonnieren**.
2. Melden Sie sich mit Ihrem Benutzerkonto an, wenn Sie dazu aufgefordert werden.
3. Die Ressourcen werden nach Arbeitsbereichen gruppiert im Connection Center angezeigt.

>[!NOTE]
>Der Windows-Client verwendet standardmäßig automatisch Windows Virtual Desktop (klassisch). Wenn der Client jedoch erkennt, dass der Benutzer auch über Azure Resource Manager-Ressourcen verfügt, werden die Ressourcen automatisch hinzugefügt, oder der Benutzer wird benachrichtigt, dass sie verfügbar sind.

### <a name="subscribe-with-a-url"></a>Abonnieren mit einer URL

1. Klicken Sie auf der Hauptseite des Clients auf **Subscribe with URL** (Mit URL abonnieren).
2. Geben Sie die Arbeitsbereichs-URL oder Ihre E-Mail-Adresse ein:
   - Verwende im Fall einer **Arbeitsbereichs-URL** diejenige, die du vom Administrator erhalten hast. Wenn du auf Ressourcen von Windows Virtual Desktop zugreifst, kannst du eine der folgenden URLs verwenden:
     - Windows Virtual Desktop (klassisch): `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`
     - Windows Virtual Desktop: `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`
     - Windows Virtual Desktop (US Gov): `https://rdweb.wvd.azure.us/api/arm/feeddiscovery`
   - Wenn Sie stattdessen das Feld **E-Mail** verwenden, geben Sie Ihre E-Mail-Adresse ein. Dies weist den Client an, nach einer URL zu suchen, die Ihrer E-Mail-Adresse zugeordnet ist, wenn Ihr Administrator [E-Mail-Ermittlung](/windows-server/remote/remote-desktop-services/rds-email-discovery) eingerichtet hat.
3. Wählen Sie **Weiter** aus.
4. Melden Sie sich mit Ihrem Benutzerkonto an, wenn Sie dazu aufgefordert werden.
5. Die Ressourcen sollten im Connection Center nach Arbeitsbereich gruppiert angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung des Windows Desktop-Clients finden Sie unter [Erste Schritte mit dem Windows Desktop-Client](/windows-server/remote/remote-desktop-services/clients/windowsdesktop/).

Wenn Sie ein Administrator sind, der ausführlichere Kenntnisse zur Verwendung von Windows Desktop erwerben möchte, sehen Sie sich [Windows Desktop-Client für Administratoren](/windows-server/remote/remote-desktop-services/clients/windowsdesktop-admin) an.
