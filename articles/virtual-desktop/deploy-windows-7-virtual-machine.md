---
title: Bereitstellen eines virtuellen Windows 7-Computers in Windows Virtual Desktop – Azure
description: Konfigurieren und Bereitstellen eines virtuellen Windows 7-Computers in Windows Virtual Desktop.
author: Heidilohr
ms.topic: how-to
ms.date: 07/11/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 65ef7af1247fdb38a7c85c1a8134257942f5ef5b
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106447028"
---
# <a name="deploy-a-windows-7-virtual-machine-on-windows-virtual-desktop"></a>Bereitstellen eines virtuellen Windows 7-Computers in Windows Virtual Desktop

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Windows Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/deploy-windows-7-virtual-machine.md).

Der Vorgang zum Bereitstellen eines virtuellen Windows 7-Computers (VM) in Windows Virtual Desktop unterscheidet sich geringfügig von der Bereitstellung auf virtuellen Computern, auf denen höhere Versionen von Windows ausgeführt werden. In dieser Anleitung erfahren Sie, wie Sie Windows 7 bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, befolgen Sie die Anleitungen unter [Erstellen eines Hostpools mit PowerShell](create-host-pools-powershell.md), um einen Hostpool zu erstellen. Wenn Sie das Portal verwenden, befolgen Sie die Anweisungen in den unter [Erstellen eines Hostpools mit dem Azure-Portal](create-host-pools-azure-marketplace.md) beschriebenen Schritten 1 bis 9. Klicken Sie anschließend auf **Überprüfen + Erstellen**, um einen leeren Hostpool zu erstellen.

## <a name="configure-a-windows-7-virtual-machine"></a>Konfigurieren eines virtuellen Windows 7-Computers

Nachdem Sie die Voraussetzungen erfüllt haben, können Sie Ihre Windows 7-VM für die Bereitstellung in Windows Virtual Desktop konfigurieren.

So richten Sie einen virtuellen Windows 7-Computer in Windows Virtual Desktop ein:

1. Melden Sie sich am Azure-Portal an, und suchen Sie entweder nach dem Windows 7 Enterprise-Image, oder laden Sie ein eigenes benutzerdefiniertes Windows 7 Enterprise-Image (x64) hoch.
2. Stellen Sie mindestens einen virtuellen Computer mit Windows 7 Enterprise als Hostbetriebssystem bereit. Stellen Sie sicher, dass die virtuellen Computer das Remotedesktopprotokoll (RDP) zulassen (TCP-Port/Port 3389).
3. Stellen Sie mithilfe von RDP eine Verbindung mit dem Windows 7 Enterprise-Host her, und authentifizieren Sie sich mit den Anmeldeinformationen, die Sie beim Konfigurieren der Bereitstellung definiert haben.
4. Fügen Sie das Konto, das Sie beim Herstellen einer Verbindung mit dem Host über RDP verwendet haben, der Gruppe „Remotedesktopbenutzer“ hinzu. Wenn Sie das Konto nicht hinzufügen, können Sie möglicherweise keine Verbindung mit der VM herstellen, nachdem Sie sie Ihrer Active Directory-Domäne hinzugefügt haben.
5. Navigieren Sie auf Ihrem virtuellen Computer zu Windows Update.
6. Installieren Sie alle Windows-Updates in der Kategorie „Wichtig“.
7. Installieren Sie alle Windows-Updates in der Kategorie „Optional“ (mit Ausnahme von Language Packs). Dadurch wird das Update für das Remotedesktopprotokoll 8.0 ([KB2592687](https://www.microsoft.com/download/details.aspx?id=35387)) installiert, das Sie benötigen, um diese Anweisungen abzuschließen.
8. Öffnen Sie den lokalen Gruppenrichtlinien-Editor, und navigieren Sie zu **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Remote Desktop Services** > **Remote Desktop Session Host** > **Remote Session Environment** (Computerkonfiguration > Administrative Vorlagen > Windows-Komponenten > Remotedesktopdienste > Remotedesktop-Sitzungshost > Remotesitzungsumgebung).
9. Aktivieren Sie die Richtlinie für das Remotedesktopprotokoll 8.0.
10. Fügen Sie diesen virtuellen Computer Ihrer Active Directory-Domäne hinzu.
11. Starten Sie den virtuellen Computer neu, indem Sie den folgenden Befehl ausführen:

     ```cmd
     shutdown /r /t 0
     ```

12. Führen Sie die [hier](/powershell/module/az.desktopvirtualization/new-azwvdregistrationinfo) aufgeführten Schritte aus, um ein Registrierungstoken abzurufen.

      - Wenn Sie stattdessen das Azure-Portal verwenden möchten, können Sie auch zur Übersichtsseite des Hostpools wechseln, dem Sie den virtuellen Computer hinzufügen möchten, und dort ein Token erstellen.

13. [Herunterladen des Windows Virtual Desktop-Agents für Windows 7](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3JZCm).
14. [Herunterladen des Windows Virtual Desktop-Agent-Managers für Windows 7](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3K2e3).
15. Öffnen Sie das Installationsprogramm für den Windows Virtual Desktop-Agent, und befolgen Sie die Anweisungen. Wenn Sie dazu aufgefordert werden, geben Sie den in Schritt 12 erstellten Registrierungsschlüssel ein.
16. Öffnen Sie den Windows Virtual Desktop-Agent-Manager, und befolgen Sie die Anweisungen.
17. Blockieren Sie optional den Port TCP/3389, um den direkten Zugriff auf die VM über das Remotedesktopprotokoll zu entfernen.
18. Vergewissern Sie sich ggf., dass .NET Framework mindestens in der Version 4.7.2 vorliegt. Insbesondere wenn Sie ein benutzerdefiniertes Image erstellen ist es wichtig, dass Sie Ihr Framework aktualisieren.

## <a name="next-steps"></a>Nächste Schritte

Ihre Windows Virtual Desktop-Bereitstellung kann jetzt verwendet werden. [Laden Sie die neueste Version des Windows Virtual Desktop-Clients herunter](https://aka.ms/wvd/clients/windows), um loszulegen.

Eine Liste bekannter Probleme und Anleitungen zur Problembehandlung für Windows 7 in Windows Virtual Desktop finden Sie im Artikel zur Problembehandlung unter [Problembehandlung für virtuelle Windows 7-Computer in Windows Virtual Desktop](./virtual-desktop-fall-2019/troubleshoot-windows-7-vm.md).
