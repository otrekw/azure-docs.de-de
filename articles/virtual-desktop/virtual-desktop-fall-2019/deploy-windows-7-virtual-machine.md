---
title: Bereitstellen eines virtuellen Windows 7-Computers in Azure Virtual Desktop (klassisch) – Azure
description: Hier erfahren Sie, wie Sie einen virtuellen Windows 7-Computer in Azure Virtual Desktop (klassisch) konfigurieren und bereitstellen.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 1b7c78ab9a518649d205bb9dc92c86d7bb71bcea
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111744627"
---
# <a name="deploy-a-windows-7-virtual-machine-on-azure-virtual-desktop-classic"></a>Bereitstellen eines virtuellen Windows 7-Computers in Azure Virtual Desktop (klassisch)

>[!IMPORTANT]
>Dieser Inhalt gilt für Azure Virtual Desktop (klassisch). Der Dienst unterstützt keine Azure Virtual Desktop-Objekte in Azure Resource Manager. Wenn Sie Azure Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, die in der aktuellen Version von Azure Virtual Desktop eingeführt wurden, finden Sie weitere Informationen in [diesem Artikel](../deploy-windows-7-virtual-machine.md).

Der Vorgang zum Bereitstellen eines virtuellen Windows 7-Computers (VM) in Azure Virtual Desktop unterscheidet sich geringfügig von der Bereitstellung auf virtuellen Computern, auf denen höhere Versionen von Windows ausgeführt werden. In dieser Anleitung erfahren Sie, wie Sie Windows 7 bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, befolgen Sie die Anleitungen unter [Erstellen eines Hostpools mit PowerShell](create-host-pools-powershell-2019.md), um einen Hostpool zu erstellen. Befolgen Sie anschließend die Anleitungen in [Erstellen von Hostpools in Azure Marketplace](create-host-pools-azure-marketplace-2019.md#optional-assign-additional-users-to-the-desktop-application-group), um der Desktopanwendungsgruppe mindestens einen Benutzer zuzuweisen.

## <a name="configure-a-windows-7-virtual-machine"></a>Konfigurieren eines virtuellen Windows 7-Computers

Wenn alle Voraussetzungen erfüllt sind, können Sie Ihren virtuellen Windows 7-Computer für die Bereitstellung in Azure Virtual Desktop konfigurieren.

So richten Sie einen virtuellen Windows 7-Computer in Azure Virtual Desktop ein:

1. Melden Sie sich am Azure-Portal an, und suchen Sie entweder nach dem Windows 7 Enterprise-Image, oder laden Sie ein eigenes benutzerdefiniertes Windows 7 Enterprise-Image (x64) hoch.
2. Stellen Sie mindestens einen virtuellen Computer mit Windows 7 Enterprise als Hostbetriebssystem bereit. Stellen Sie sicher, dass die virtuellen Computer das Remotedesktopprotokoll (RDP) zulassen (TCP-Port/Port 3389).
3. Stellen Sie mithilfe von RDP eine Verbindung mit dem Windows 7 Enterprise-Host her, und authentifizieren Sie sich mit den Anmeldeinformationen, die Sie beim Konfigurieren der Bereitstellung definiert haben.
4. Fügen Sie das Konto, das Sie beim Herstellen einer Verbindung mit dem Host über RDP verwendet haben, der Gruppe „Remotedesktopbenutzer“ hinzu. Wenn Sie nicht so vorgehen, können Sie möglicherweise keine Verbindung mit dem virtuellen Computer herstellen, nachdem Sie ihn Ihrer Active Directory-Domäne hinzugefügt haben.
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

12. Führen Sie die [hier](/powershell/module/windowsvirtualdesktop/export-rdsregistrationinfo/) aufgeführten Schritte aus, um ein Registrierungstoken abzurufen.
13. [Laden Sie den Azure Virtual Desktop-Agent für Windows 7 herunter](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3JZCm).
14. [Laden Sie den Azure Virtual Desktop-Agent-Manager für Windows 7 herunter](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3K2e3).
15. Öffnen Sie das Installationsprogramm für den Azure Virtual Desktop-Agent, und befolgen Sie die Anweisungen. Wenn Sie dazu aufgefordert werden, geben Sie den in Schritt 12 erstellten Registrierungsschlüssel ein.
16. Öffnen Sie das Installationsprogramm für den Azure Virtual Desktop-Agent-Manager, und befolgen Sie die Anweisungen.
17. Blockieren Sie optional den Port TCP/3389, um den direkten Zugriff auf die VM über das Remotedesktopprotokoll zu entfernen.
18. Vergewissern Sie sich ggf., dass .NET Framework mindestens in der Version 4.7.2 vorliegt. Dies ist insbesondere wichtig, wenn Sie ein benutzerdefiniertes Image erstellen.

## <a name="next-steps"></a>Nächste Schritte

Ihre Azure Virtual Desktop-Bereitstellung kann jetzt verwendet werden. [Laden Sie die neueste Version des Azure Virtual Desktop-Clients herunter](https://aka.ms/wvd/clients/windows), um zu beginnen.

Eine Liste bekannter Probleme und Anleitungen zur Problembehandlung für Windows 7 in Azure Virtual Desktop finden Sie im Artikel zur Problembehandlung unter [Problembehandlung für virtuelle Windows 7-Computer in Azure Virtual Desktop](troubleshoot-windows-7-vm.md).
