---
title: Problembehandlung bei Windows Virtual Desktop-Dienstverbindungen – Azure
description: Informationen zum Beheben von Problemen beim Einrichten von Clientverbindungen in einer Windows Virtual Desktop-Mandantenumgebung
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 12/13/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: a6298b3a9c5769b1d82f89956736b451935b2c5d
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612638"
---
# <a name="windows-virtual-desktop-service-connections"></a>Windows Virtual Desktop-Dienstverbindungen

>[!IMPORTANT]
>Dieser Inhalt gilt für das Update vom Frühjahr 2020 mit Windows Virtual Desktop-Objekten in Azure Resource Manager. Wenn Sie das Windows Virtual Desktop-Release vom Herbst 2019 ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/troubleshoot-service-connection-2019.md).
>
> Das Windows Virtual Desktop-Update vom Frühjahr 2020 befindet sich derzeit in der öffentlichen Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. 
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Verwenden Sie diesen Artikel zum Beheben von Problemen mit Windows Virtual Desktop-Clientverbindungen.

## <a name="provide-feedback"></a>Feedback geben

In der [Windows Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) können Sie uns Feedback geben und sich mit dem Produktteam und anderen aktiven Communitymitgliedern über den Windows Virtual Desktop-Dienst austauschen.

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>Der Benutzer stellt eine Verbindung her, aber es wird nichts angezeigt (kein Feed).

Ein Benutzer kann Remotedesktopclients starten und sich authentifizieren, ihm werden jedoch im Websuchfeed keine Symbole angezeigt.

Vergewissern Sie sich, dass der Benutzer, der die Probleme meldet, mithilfe dieser Befehlszeile Anwendungsgruppen zugewiesen wurde:

```PowerShell
Get-AzRoleAssignment -SignInName <userupn>
```

Vergewissern Sie sich, dass sich der Benutzer mit den richtigen Anmeldeinformationen anmeldet.

Wenn der Webclient verwendet wird, vergewissern Sie sich, dass keine Probleme mit zwischengespeicherten Anmeldeinformationen vorliegen.

## <a name="windows-10-enterprise-multi-session-virtual-machines-dont-respond"></a>Virtuelle Computer mit mehreren Sitzungen unter Windows 10 Enterprise antworten nicht

Wenn ein virtueller Computer nicht reagiert und Sie nicht über RDP darauf zugreifen können, müssen Sie mit der Diagnosefunktion eine Problembehandlung durchführen, indem Sie den Hoststatus überprüfen.

Führen Sie dieses Cmdlet aus, um den Hoststatus zu überprüfen:

```powershell
Get-AzWvdSessionHost -HostPoolName <hostpoolname> -ResourceGroupName <resourcegroupname>| Format-List Name, LastHeartBeat, AllowNewSession, Status
```

Wenn der Hoststatus `NoHeartBeat` ist, bedeutet dies, dass der virtuelle Computer nicht antwortet und der Agent nicht mit dem Windows Virtual Desktop-Dienst kommunizieren kann.

```powershell
Name            : 0301HP/win10pd-0.contoso.com 
LastHeartBeat   : 4/8/2020 1:48:35 AM 
AllowNewSession : True 
Status          : Available 

Name            : 0301HP/win10pd-1.contoso.com 
LastHeartBeat   : 4/8/2020 1:45:44 AM 
AllowNewSession : True 
Status          : NoHeartBeat
```

Es gibt einige Möglichkeiten, um den NoHeartBeat-Status zu beseitigen.

### <a name="update-fslogix"></a>Aktualisieren von FSLogix

Wenn FSLogix nicht auf dem neuesten Stand ist, insbesondere bei der Version 2.9.7205.27375 von frxdrvvt.sys, kann dies zu einem Deadlock führen. Sie müssen dann [FSLogix auf die aktuelle Version aktualisieren](https://go.microsoft.com/fwlink/?linkid=2084562).

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Problembehandlung von Windows Virtual Desktop und die Eskalationspfade finden Sie unter [Überblick über Problembehandlung, Feedback und Support](troubleshoot-set-up-overview.md).
- Informationen zur Problembehandlung beim Erstellen einer Windows Virtual Desktop-Umgebung und eines Hostpools in einer Windows Virtual Desktop-Umgebung finden Sie unter [Umgebungs- und Hostpoolerstellung](troubleshoot-set-up-issues.md).
- Informationen zur Problembehandlung bei der Konfiguration eines virtuellen Computers (VM) in Windows Virtual Desktop finden Sie unter [Konfiguration des virtuellen Sitzungshostcomputers](troubleshoot-vm-configuration.md).
- Informationen zur Problembehandlung bei der Verwendung von PowerShell mit Windows Virtual Desktop finden Sie unter [Windows Virtual Desktop – PowerShell](troubleshoot-powershell.md).
- Ein Tutorial zur Problembehandlung finden Sie unter [Tutorial: Problembehandlung von Bereitstellungen der Resource Manager-Vorlage](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
