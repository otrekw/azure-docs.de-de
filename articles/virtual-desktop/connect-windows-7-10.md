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
ms.openlocfilehash: 73811aa58b09e394468596f42e0ff221b2bb240b
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86261500"
---
# <a name="connect-with-the-windows-desktop-client"></a>Herstellen einer Verbindung mit dem Windows-Desktopclient

> Gilt für: Windows 7, Windows 10 und Windows 10 IoT Enterprise

>[!IMPORTANT]
>Dieser Inhalt gilt für das Update vom Frühjahr 2020 mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie das Windows Virtual Desktop-Release vom Herbst 2019 ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/connect-windows-7-10-2019.md).
>
> Das Windows Virtual Desktop-Update vom Frühjahr 2020 befindet sich derzeit in der öffentlichen Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mit dem Windows Desktop-Client können Sie auf Geräten mit Windows 7, Windows 10 und Windows 10 IoT Enterprise auf Windows Virtual Desktop-Ressourcen zugreifen.

>[!NOTE]
>Der Windows-Client verwendet standardmäßig automatisch das Windows Virtual Desktop-Release aus dem Herbst 2019. Wenn der Client jedoch erkennt, dass der Benutzer auch über Azure Resource Manager-Ressourcen verfügt, werden die Ressourcen automatisch hinzugefügt, oder der Benutzer wird benachrichtigt, dass sie verfügbar sind.

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
     - Windows Virtual Desktop-Release aus dem Herbst 2019: `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`
     - Windows Virtual Desktop-Release aus dem Frühjahr 2020: `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`
   - Wenn Sie stattdessen das Feld **E-Mail** verwenden, geben Sie Ihre E-Mail-Adresse ein. Dies weist den Client an, nach einer URL zu suchen, die Ihrer E-Mail-Adresse zugeordnet ist, wenn Ihr Administrator [E-Mail-Ermittlung](/windows-server/remote/remote-desktop-services/rds-email-discovery) eingerichtet hat.
3. Wählen Sie **Weiter** aus.
4. Melden Sie sich mit Ihrem Benutzerkonto an, wenn Sie dazu aufgefordert werden.
5. Die Ressourcen sollten im Connection Center nach Arbeitsbereich gruppiert angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung des Windows Desktop-Clients finden Sie unter [Erste Schritte mit dem Windows Desktop-Client](/windows-server/remote/remote-desktop-services/clients/windowsdesktop/).
