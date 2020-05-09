---
title: Problembehandlung bei Windows Virtual Desktop-Dienstverbindungen – Azure
description: Informationen zum Beheben von Problemen beim Einrichten von Clientverbindungen in einer Windows Virtual Desktop-Mandantenumgebung
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 01aff34839cc7385834468a08f30696efe84561f
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82614067"
---
# <a name="windows-virtual-desktop-service-connections"></a>Windows Virtual Desktop-Dienstverbindungen

>[!IMPORTANT]
>Dieser Inhalt gilt für das Release vom Herbst 2019, das keine Windows Virtual Desktop-Objekte in Azure Resource Manager unterstützt. Wenn Sie Windows Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, die im Update vom Frühjahr 2020 eingeführt wurden, finden Sie weitere Informationen in [diesem Artikel](../troubleshoot-service-connection.md).

Verwenden Sie diesen Artikel zum Beheben von Problemen mit Windows Virtual Desktop-Clientverbindungen.

## <a name="provide-feedback"></a>Feedback geben

In der [Windows Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) können Sie uns Feedback geben und sich mit dem Produktteam und anderen aktiven Communitymitgliedern über den Windows Virtual Desktop-Dienst austauschen.

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>Der Benutzer stellt eine Verbindung her, aber es wird nichts angezeigt (kein Feed).

Ein Benutzer kann Remotedesktopclients starten und sich authentifizieren, ihm werden jedoch im Websuchfeed keine Symbole angezeigt.

Vergewissern Sie sich, dass der Benutzer, der die Probleme meldet, mithilfe dieser Befehlszeile Anwendungsgruppen zugewiesen wurde:

```PowerShell
Get-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname>
```

Vergewissern Sie sich, dass sich der Benutzer mit den richtigen Anmeldeinformationen anmeldet.

Wenn der Webclient verwendet wird, vergewissern Sie sich, dass keine Probleme mit zwischengespeicherten Anmeldeinformationen vorliegen.

## <a name="windows-10-enterprise-multi-session-virtual-machines-dont-respond"></a>Virtuelle Computer mit mehreren Sitzungen unter Windows 10 Enterprise antworten nicht

Wenn ein virtueller Computer nicht reagiert und Sie nicht über RDP darauf zugreifen können, müssen Sie mit der Diagnosefunktion eine Problembehandlung durchführen, indem Sie den Hoststatus überprüfen.

Führen Sie dieses Cmdlet aus, um den Hoststatus zu überprüfen:

```powershell
Get-RdsSessionHost -TenantName $TenantName -HostPoolName $HostPool | ft SessionHostName, LastHeartBeat, AllowNewSession, Status
```

Wenn der Hoststatus `NoHeartBeat` ist, bedeutet dies, dass der virtuelle Computer nicht antwortet und der Agent nicht mit dem Windows Virtual Desktop-Dienst kommunizieren kann.

```powershell
SessionHostName          LastHeartBeat     AllowNewSession    Status 
---------------          -------------     ---------------    ------ 
WVDHost1.contoso.com     21-Nov-19 5:21:35            True     Available 
WVDHost2.contoso.com     21-Nov-19 5:21:35            True     Available 
WVDHost3.contoso.com     21-Nov-19 5:21:35            True     NoHeartBeat 
WVDHost4.contoso.com     21-Nov-19 5:21:35            True     NoHeartBeat 
WVDHost5.contoso.com     21-Nov-19 5:21:35            True     NoHeartBeat 
```

Es gibt einige Möglichkeiten, um den NoHeartBeat-Status zu beseitigen.

### <a name="update-fslogix"></a>Aktualisieren von FSLogix

Wenn FSLogix nicht auf dem neuesten Stand ist, insbesondere bei der Version 2.9.7205.27375 von frxdrvvt.sys, kann dies zu einem Deadlock führen. Sie müssen dann [FSLogix auf die aktuelle Version aktualisieren](https://go.microsoft.com/fwlink/?linkid=2084562).

### <a name="disable-bgtaskregistrationmaintenancetask"></a>Deaktivieren von BgTaskRegistrationMaintenanceTask

Wenn die Aktualisierung von FSLogix nicht funktioniert, kann das Problem darin bestehen, dass eine BiSrv-Komponente bei einer wöchentlichen Wartungsaufgabe Systemressourcen komplett auslastet. Deaktivieren Sie die Wartungsaufgabe vorübergehend, indem Sie BgTaskRegistrationMaintenanceTask mit einer der beiden folgenden Methoden deaktivieren:

- Wechseln Sie zum Startmenü und suchen Sie nach **Taskplaner**. Navigieren Sie zu **Aufgabenplanungsbibliothek** > **Microsoft** > **Windows** > **BrokerInfrastructure**. Suchen Sie nach einer Aufgabe namens **BgTaskRegistrationMaintenanceTask**. Klicken Sie mit der rechten Maustaste darauf, wenn Sie sie gefunden haben, und wählen Sie im Dropdown-Menü **Deaktivieren** aus.
- Öffnen Sie ein Befehlszeilenmenü als Administrator, und führen Sie den folgenden Befehl aus:
    
    ```cmd
    schtasks /change /tn "\Microsoft\Windows\BrokerInfrastructure\BgTaskRegistrationMaintenanceTask" /disable 
    ```

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Problembehandlung von Windows Virtual Desktop und die Eskalationspfade finden Sie unter [Überblick über Problembehandlung, Feedback und Support](troubleshoot-set-up-overview-2019.md).
- Informationen zur Problembehandlung beim Erstellen eines Mandanten- und Hostpools in einer Windows Virtual Desktop-Umgebung finden Sie unter [Mandanten- und Hostpoolerstellung](troubleshoot-set-up-issues-2019.md).
- Informationen zur Problembehandlung bei der Konfiguration eines virtuellen Computers (VM) in Windows Virtual Desktop finden Sie unter [Konfiguration des virtuellen Sitzungshostcomputers](troubleshoot-vm-configuration-2019.md).
- Informationen zur Problembehandlung bei der Verwendung von PowerShell mit Windows Virtual Desktop finden Sie unter [Windows Virtual Desktop – PowerShell](troubleshoot-powershell-2019.md).
- Ein Tutorial zur Problembehandlung finden Sie unter [Tutorial: Problembehandlung von Bereitstellungen der Resource Manager-Vorlage](../../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
