---
title: Problembehandlung bei VMware vCenter-Ermittlungsfehlern in Azure Site Recovery
description: In diesem Artikel wird beschrieben, wie Sie die Problembehandlung bei VMware vCenter-Ermittlungsfehlern in Azure Site Recovery durchführen können.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: mayg
ms.openlocfilehash: 1a8471305af93194ccae7b0928685e10d4d64726
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2020
ms.locfileid: "92366654"
---
# <a name="troubleshoot-vcenter-server-discovery-failures"></a>Problembehandlung von vCenter Server-Ermittlungsfehlern

Dieser Artikel hilft Ihnen bei der Behandlung von Problemen, die aufgrund von VMware vCenter-Ermittlungsfehler auftreten.

## <a name="non-numeric-values-in-the-maxsnapshots-property"></a>Nicht-numerische Werte in der maxSnapShots-Eigenschaft

In Versionen vor 9.20 trennt vCenter die Verbindung, wenn ein nicht-numerischer Wert für die Eigenschaft `snapshot.maxSnapShots` auf einer VM abgerufen wird.

Dieses Problem wird durch die Fehler-ID 95126 identifiziert.

```output
ERROR :: Hit an exception while fetching the required informationfrom vCenter/vSphere.Exception details:
System.FormatException: Input string was not in a correct format.
    at System.Number.StringToNumber(String str, NumberStyles options, NumberBuffer& number, NumberFormatInfo info, Boolean parseDecimal)
    at System.Number.ParseInt32(String s, NumberStyles style, NumberFormatInfo info)
    at VMware.VSphere.Management.InfraContracts.VirtualMachineInfo.get_MaxSnapshots()
```

So lösen Sie das Problem:

- Identifizieren Sie den virtuellen Computer, und legen Sie den Wert auf einen numerischen Wert fest (VM Edit-Einstellungen in vCenter).

oder

- Aktualisieren Sie Ihren Konfigurationsserver auf Version 9.20 oder höher.

## <a name="proxy-configuration-issues-for-vcenter-connectivity"></a>Proxy-Konfigurationsprobleme für vCenter-Konnektivität

Die vCenter-Ermittlung berücksichtigt die vom Systembenutzer konfigurierten Standard-Proxyeinstellungen des Systems. Der DRA-Dienst übernimmt die Proxyeinstellungen, die der Benutzer bei der Installation des Konfigurationsservers mit dem Installationsprogramm für das einheitliche Setup oder der OVA-Vorlage angegeben hat. 

Im Allgemeinen wird der Proxy zur Kommunikation mit öffentlichen Netzwerken verwendet, z.B. zur Kommunikation mit Azure. Wenn der Proxy konfiguriert ist und vCenter sich in einer lokalen Umgebung befindet, kann er nicht mit DRA kommunizieren.

Die folgenden Situationen treten auf, wenn dieses Problem vorliegt:

- Der vCenter-Server \<vCenter> ist aufgrund des Fehlers nicht erreichbar: The remote server returned an error: (503) Server nicht verfügbar
- Der vCenter-Server \<vCenter> ist aufgrund des Fehlers nicht erreichbar: The remote server returned an error: Es konnte keine Verbindung mit dem Remoteserver hergestellt werden.
- Es konnte keine Verbindung mit dem vCenter/ESXi-Server hergestellt werden.

So lösen Sie das Problem:

Laden Sie das [PsExec-Tool](/sysinternals/downloads/psexec) herunter. 

Verwenden Sie das PsExec-Tool, um auf den Systembenutzerkontext zuzugreifen und zu bestimmen, ob die Proxyadresse konfiguriert ist. Anschließend können Sie vCenter mit den folgenden Verfahren zur Umgehungsliste hinzufügen.

Für die Proxykonfiguration der Ermittlung:

1. Öffnen Sie IE im Systembenutzerkontext mit dem PsExec-Tool.
    
    psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe"

2. Ändern Sie die Proxyeinstellungen im Internet Explorer, um die vCenter-IP-Adresse zu umgehen.
3. Starten Sie den tmanssvc-Dienst neu.

Für die DRA-Proxykonfiguration:

1. Öffnen Sie eine Eingabeaufforderung und dann den Ordner „Microsoft Azure Site Recovery Provider“.
 
    **cd C:\Programme\Microsoft Azure Site Recovery Provider**

3. Führen Sie den folgenden Befehl an der Eingabeaufforderung aus:
   
   **DRCONFIGURATOR.EXE /configure /AddBypassUrls [IP-Adresse/beim Hinzufügen von vCenter angegebener vCenter Server-FQDN]**

4. Starten Sie den DRA-Dienst neu.

## <a name="next-steps"></a>Nächste Schritte

[Verwalten des Konfigurationsservers für die Notfallwiederherstellung von virtuellen VMware-Computern](./vmware-azure-manage-configuration-server.md#refresh-configuration-server)