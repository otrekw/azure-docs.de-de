---
title: Troubleshooting für den Remotedesktopclient in Windows Virtual Desktop – Azure
description: Informationen zum Beheben von Problemen beim Einrichten von Clientverbindungen in einer Windows Virtual Desktop-Mandantenumgebung
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 12/13/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: e3a240901ffca2c126e2b61eaee0cf287cc31d6e
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2020
ms.locfileid: "79127504"
---
# <a name="troubleshoot-the-remote-desktop-client"></a>Troubleshooting für den Remotedesktopclient

In diesem Artikel werden häufige Probleme mit dem Remotedesktopclient sowie deren Behebung beschrieben.

## <a name="remote-desktop-client-for-windows-7-or-windows-10-stops-responding-or-cannot-be-opened"></a>Der Remotedesktopclient für Windows 7 oder Windows 10 reagiert nicht mehr oder kann nicht geöffnet werden.

Verwenden Sie die folgenden PowerShell-Cmdlets, um Out-of-Band-Clientregistrierungen (OOB) zu bereinigen.

```PowerShell
Remove-ItemProperty 'HKCU:\Software\Microsoft\Terminal Server Client\Default' - Name FeedURLs

#Remove RdClientRadc registry key
Remove-Item 'HKCU:\Software\Microsoft\RdClientRadc' -Recurse

#Remove all files under %appdata%\RdClientRadc
Remove-Item C:\Users\pavithir\AppData\Roaming\RdClientRadc\* -Recurse
```

Navigieren Sie zu **%AppData%\RdClientRadc**, und löschen Sie den gesamten Inhalt.

Deinstallieren Sie den Remotedesktopclient für Windows 7 und Windows 10, und installieren Sie ihn erneut.

## <a name="web-client-wont-open"></a>Webclient wird nicht geöffnet.

Testen Sie zunächst Ihre Internetverbindung, indem Sie eine andere Website in Ihrem Browser öffnen, z. B. [www.bing.com](https://www.bing.com).

Bestätigen Sie mit **nslookup**, dass das DNS den FQDN auflösen kann:

```cmd
nslookup rdweb.wvd.microsoft.com
```

Versuchen Sie, eine Verbindung mit einem anderen Client herzustellen, z. B. dem Remotedesktopclient für Windows 7 oder Windows 10, und überprüfen Sie, ob Sie den Webclient öffnen können.

### <a name="opening-another-site-fails"></a>Beim Öffnen einer anderen Seite tritt ein Fehler auf.

Dies wird normalerweise durch Netzwerkverbindungsprobleme oder einen Netzwerkausfall verursacht. Es wird empfohlen, sich an den Netzwerksupport zu wenden.

### <a name="nslookup-cannot-resolve-the-name"></a>Nslookup kann den Namen nicht auflösen.

Dies wird normalerweise durch Netzwerkverbindungsprobleme oder einen Netzwerkausfall verursacht. Es wird empfohlen, sich an den Netzwerksupport zu wenden.

### <a name="your-client-cant-connect-but-other-clients-on-your-network-can-connect"></a>Ihr Client kann keine Verbindung herstellen, andere Clients in Ihrem Netzwerk hingegen schon.

Wenn bei der Verwendung des Webclients Probleme mit Ihrem Browser auftreten oder dieser nicht mehr funktioniert, befolgen Sie die folgenden Anweisungen zum Troubleshooting:

1. Starten Sie den Browser neu.
2. Löschen Sie die Browsercookies. Weitere Informationen finden Sie unter [Löschen von Cookiedateien in Internet Explorer](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
3. Löschen Sie den Browsercache. Weitere Informationen finden Sie unter [Leeren des Browsercache für Ihren Browser](https://binged.it/2RKyfdU).
4. Öffnen Sie den Browser im privaten Modus.

## <a name="web-client-stops-responding-or-disconnects"></a>Der Webclient reagiert nicht mehr oder trennt die Verbindung.

Versuchen Sie, eine Verbindung mit einem anderen Browser oder Client herzustellen.

### <a name="other-browsers-and-clients-also-malfunction-or-fail-to-open"></a>Andere Browser und Clients weisen ebenfalls Fehlfunktionen auf oder können nicht geöffnet werden.

Wenn selbst nach einem Browserwechsel weiterhin Probleme auftreten, liegt es möglicherweise nicht an Ihrem Browser, sondern am Netzwerk. Es wird empfohlen, sich an den Netzwerksupport zu wenden.

## <a name="web-client-keeps-prompting-for-credentials"></a>Der Webclient fordert immer wieder zur Eingabe von Anmeldeinformationen auf.

Wenn der Webclient immer wieder zur Eingabe von Anmeldeinformationen auffordert, gehen Sie wie folgt vor:

1. Vergewissern Sie sich, dass die URL des Webclients richtig ist.
2. Vergewissern Sie sich, dass die verwendeten Anmeldeinformationen für die an die URL gebundene Windows Virtual Desktop-Umgebung gültig sind.
3. Löschen Sie die Browsercookies. Weitere Informationen finden Sie unter [Löschen von Cookiedateien in Internet Explorer](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
4. Löschen Sie den Browsercache. Weitere Informationen finden Sie unter [Leeren des Browsercaches für Ihren Browser](https://binged.it/2RKyfdU).
5. Öffnen Sie den Browser im privaten Modus.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Problembehandlung von Windows Virtual Desktop und die Eskalationspfade finden Sie unter [Überblick über Problembehandlung, Feedback und Support](troubleshoot-set-up-overview.md).
- Informationen zur Problembehandlung beim Erstellen eines Mandanten- und Hostpools in einer Windows Virtual Desktop-Umgebung finden Sie unter [Mandanten- und Hostpoolerstellung](troubleshoot-set-up-issues.md).
- Informationen zur Problembehandlung bei der Konfiguration eines virtuellen Computers (VM) in Windows Virtual Desktop finden Sie unter [Konfiguration des virtuellen Sitzungshostcomputers](troubleshoot-vm-configuration.md).
- Informationen zur Problembehandlung bei der Verwendung von PowerShell mit Windows Virtual Desktop finden Sie unter [Windows Virtual Desktop – PowerShell](troubleshoot-powershell.md).
- Ein Tutorial zur Problembehandlung finden Sie unter [Tutorial: Problembehandlung von Bereitstellungen der Resource Manager-Vorlage](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).