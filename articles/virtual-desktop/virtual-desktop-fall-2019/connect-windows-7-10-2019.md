---
title: 'Herstellen einer Verbindung mit Windows Virtual Desktop (klassisch) unter Windows 10 oder 7: Azure'
description: Informationen zum Herstellen einer Verbindung mit Windows Virtual Desktop (klassisch) mithilfe des Windows Desktop-Clients.
author: Heidilohr
ms.topic: how-to
ms.date: 07/16/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 420c507361e3e2437366e6ccf2d46a8b78684e59
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "88008356"
---
# <a name="connect-with-the-windows-desktop-classic-client"></a>Herstellen einer Verbindung mit dem Windows-Desktopclient (klassisch)

> Gilt für: Windows 7, Windows 10 und Windows 10 IoT Enterprise

>[!IMPORTANT]
>Dieser Inhalt gilt für den Windows Virtual Desktop-Dienst (klassisch), der keine Windows Virtual Desktop-Objekte in Azure Resource Manager unterstützt. Wenn Sie Windows Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, helfen Ihnen die Informationen in [diesem Artikel](../connect-windows-7-10.md) weiter.

Mit dem Windows Desktop-Client können Sie auf Geräten mit Windows 7, Windows 10 und Windows 10 IoT Enterprise auf Windows Virtual Desktop-Ressourcen zugreifen. Der Client unterstützt weder Windows 8 noch Windows 8.1.

>[!NOTE]
>Der Windows-Client verwendet standardmäßig automatisch Windows Virtual Desktop (klassisch). Wenn der Client jedoch erkennt, dass der Benutzer auch über Azure Resource Manager-Ressourcen verfügt, werden die Ressourcen automatisch hinzugefügt, oder der Benutzer wird benachrichtigt, dass sie verfügbar sind.

> [!IMPORTANT]
> Windows Virtual Desktop unterstützt nicht den RemoteApp-Client und den Client für Desktopverbindungen (RADC). Der Client für Remotedesktopverbindung (MSTSC) wird ebenfalls nicht unterstützt.

> [!IMPORTANT]
> Windows Virtual Desktop verfügt derzeit nicht über Unterstützung für den Remotedesktopclient aus dem Windows Store.

## <a name="install-the-windows-desktop-client"></a>Installieren des Windows Desktop-Clients

Wählen Sie den Client aus, der Ihrer Windows-Version entspricht:

- [Windows (64-Bit)](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows (32-Bit)](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

Sie können den Client für den aktuellen Benutzer installieren. Dafür sind keine Administratorrechte erforderlich. Der Administrator kann den Client aber auch so installieren und konfigurieren, dass alle Benutzer auf dem Gerät darauf zugreifen können.

Nach der Installation kann der Client über das Startmenü gestartet werden, indem Sie nach **Remote Desktop** suchen.

## <a name="subscribe-to-a-workspace"></a>Abonnieren eines Arbeitsbereichs

Es gibt zwei Möglichkeiten, einen Arbeitsbereich zu abonnieren. Der Client kann versuchen, die Ressourcen zu ermitteln, die Ihnen über Ihr Geschäfts-, Schul- oder Unikonto zur Verfügung stehen. Alternativ können Sie die URL, unter der sich die Ressourcen befinden, direkt angeben, falls der Client diese nicht finden kann. Sobald Sie einen Arbeitsbereich abonniert haben, können Sie auf eine der folgenden Arten Ressourcen starten:

- Navigieren Sie zum Connection Center, und doppelklicken Sie auf eine Ressource, um diese zu starten.
- Alternativ können Sie zum Startmenü navigieren und nach einem Ordner mit dem Namen des Arbeitsbereichs suchen oder den Ressourcennamen in die Suchleiste eingeben.

### <a name="subscribe-with-a-user-account"></a>Abonnieren mit einem Benutzerkonto

1. Klicken Sie auf der Hauptseite des Clients auf **Abonnieren**.
2. Melden Sie sich mit Ihrem Benutzerkonto an, wenn Sie dazu aufgefordert werden.
3. Die Ressourcen werden nach Arbeitsbereichen gruppiert im Connection Center angezeigt.

### <a name="subscribe-with-a-url"></a>Abonnieren mit einer URL

1. Klicken Sie auf der Hauptseite des Clients auf **Subscribe with URL** (Mit URL abonnieren).
2. Geben Sie die Arbeitsbereichs-URL oder Ihre E-Mail-Adresse ein:
   - Wenn Sie die **Arbeitsbereichs-URL** verwenden, verwenden Sie die URL, die Ihr Administrator Ihnen mitgeteilt hat. Wenn Sie über Windows Virtual Desktop auf Ressourcen zugreifen, können Sie eine der folgenden URLs verwenden:
     - Windows Virtual Desktop (klassisch): `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`
     - Windows Virtual Desktop: `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`
   - Wenn Sie stattdessen das Feld **E-Mail** verwenden, geben Sie Ihre E-Mail-Adresse ein. Dies weist den Client an, nach einer URL zu suchen, die Ihrer E-Mail-Adresse zugeordnet ist, wenn Ihr Administrator [E-Mail-Ermittlung](/windows-server/remote/remote-desktop-services/rds-email-discovery) eingerichtet hat.
3. Wählen Sie **Weiter** aus.
4. Melden Sie sich mit Ihrem Benutzerkonto an, wenn Sie dazu aufgefordert werden.
5. Die Ressourcen sollten im Connection Center nach Arbeitsbereich gruppiert angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung des Windows Desktop-Clients finden Sie unter [Erste Schritte mit dem Windows Desktop-Client](/windows-server/remote/remote-desktop-services/clients/windowsdesktop/).
