---
title: Behandeln von Problemen mit dem Azure Linux-Agent
description: Erfahren Sie mehr zum Beheben von Problemen mit dem Azure Linux-Agent.
services: virtual-machines-linux
ms.service: virtual-machines-linux
author: axelg
manager: dcscontentpm
editor: ''
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/17/2020
ms.author: axelg
ms.openlocfilehash: 247324c30bbe0edaef78c0b0d5e6a6d593e8cac9
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97586396"
---
# <a name="troubleshoot-the-azure-linux-agent"></a>Behandeln von Problemen mit dem Azure Linux-Agent

Der [Azure Linux-Agent](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) ermöglicht einem virtuellen Computer (VM) die Kommunikation mit dem Fabric Controller (dem zugrunde liegenden physischen Server, auf dem die VM gehostet wird) über die IP-Adresse 168.63.129.16.

>[!NOTE]
>Diese IP-Adresse ist eine virtuelle öffentliche IP-Adresse, die die Kommunikation erleichtert und nicht gesperrt werden darf. Unter [Was ist die IP-Adresse 168.63.129.16?](../../virtual-network/what-is-ip-address-168-63-129-16.md) finden Sie weitere Informationen.

## <a name="before-you-begin"></a>Voraussetzungen

Überprüfen Sie Status und Version des Agents, um sicherzustellen, dass er nach wie vor unterstützt wird. Informationen zur Überprüfung der Unterstützung von Versionen finden Sie unter [Unterstützte Mindestversion für VM-Agents in Azure](https://docs.microsoft.com/troubleshoot/azure/virtual-machines/support-extensions-agent-version). Anweisungen zum Ermitteln von Status und Version finden Sie unter [Häufig gestellte Fragen zu WALinuxAgent](https://github.com/Azure/WALinuxAgent/wiki/FAQ#what-does-goal-state-agent-mean-in-waagent---version-output).

## <a name="troubleshoot-a-not-ready-status"></a>Problembehandlung beim Status „Nicht bereit“

1. Überprüfen Sie den Dienststatus des Azure Linux-Agents, um sicherzustellen, dass er ausgeführt wird. Der Dienstname kann **walinuxagent** oder **waagent** lauten.

   ```
   root@nam-u18:/home/nam# service walinuxagent status
   ● walinuxagent.service - Azure Linux Agent
      Loaded: loaded (/lib/systemd/system/walinuxagent.service; enabled; vendor preset: enabled)
      Active: active (running) since Thu 2020-10-08 17:10:29 UTC; 3min 9s ago
    Main PID: 1036 (python3)
       Tasks: 4 (limit: 4915)
      CGroup: /system.slice/walinuxagent.service
              ├─1036 /usr/bin/python3 -u /usr/sbin/waagent -daemon
              └─1156 python3 -u bin/WALinuxAgent-2.2.51-py2.7.egg -run-exthandlers
   Oct 08 17:10:33 nam-u18 python3[1036]: 2020-10-08T17:10:33.129375Z INFO ExtHandler ExtHandler Started tracking cgroup: Microsoft.OSTCExtensions.VMAccessForLinux-1.5.10, path: /sys/fs/cgroup/memory/sys
   Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.189020Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] Target handler state: enabled [incarnation 2]
   Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.197932Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] [Enable] current handler state is: enabled
   Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.212316Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] Update settings file: 0.settings
   Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.224062Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] Enable extension [bin/run-command-shim enable]
   Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.236993Z INFO ExtHandler ExtHandler Started extension in unit 'Microsoft.CPlat.Core.RunCommandLinux_1.0.1_db014406-294a-49ed-b112-c7912a86ae9e
   Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.263572Z INFO ExtHandler ExtHandler Started tracking cgroup: Microsoft.CPlat.Core.RunCommandLinux-1.0.1, path: /sys/fs/cgroup/cpu,cpuacct/syst
   Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.280691Z INFO ExtHandler ExtHandler Started tracking cgroup: Microsoft.CPlat.Core.RunCommandLinux-1.0.1, path: /sys/fs/cgroup/memory/system.sl
   Oct 08 17:10:37 nam-u18 python3[1036]: 2020-10-08T17:10:37.349090Z INFO ExtHandler ExtHandler ProcessGoalState completed [incarnation 2; 4496 ms]
   Oct 08 17:10:37 nam-u18 python3[1036]: 2020-10-08T17:10:37.365590Z INFO ExtHandler ExtHandler [HEARTBEAT] Agent WALinuxAgent-2.2.51 is running as the goal state agent [DEBUG HeartbeatCounter: 1;Heartb
   root@nam-u18:/home/nam#
   ```

   Wenn der Dienst ausgeführt wird, starten Sie ihn neu, um das Problem zu beheben. Wenn der Dienst beendet wurde, starten Sie ihn, warten einige Minuten und überprüfen dann den Status erneut.

1. Vergewissern Sie sich, dass die automatische Aktualisierung aktiviert ist. Dazu überprüfen Sie diese Einstellung in **/etc/waagent.conf**.

   ```
   AutoUpdate.Enabled=y
   ```

   Weitere Informationen zum Aktualisieren des Azure Linux-Agents finden Sie unter [Aktualisieren des Azure Linux-Agents auf einer VM](https://docs.microsoft.com/azure/virtual-machines/extensions/update-linux-agent).

1. Vergewissern Sie sich, dass die VM eine Verbindung mit dem Fabric Controller herstellen kann. Testen Sie mit einem Tool wie cURL, ob sich die VM mit 168.63.129.16 an den Ports 80, 443 und 32526 verbinden kann. Wenn die VM nicht wie erwartet eine Verbindung herstellt, prüfen Sie, ob ausgehende Kommunikation über die Ports 80, 443 und 32526 in Ihrer lokalen Firewall auf der VM möglich ist. Wenn diese IP-Adresse blockiert wurde, zeigt der VM-Agent möglicherweise unerwartetes Verhalten.

## <a name="advanced-troubleshooting"></a>Erweiterte Problembehandlung

Ereignisse für die Problembehandlung beim Azure Linux-Agent werden in der Datei **/var/log/waagent.log** aufgezeichnet.

### <a name="unable-to-connect-to-wireserver-ip-host-ip"></a>Es kann keine Verbindung mit der WireServer-IP (Host-IP) hergestellt werden

Die Datei **/var/log/waagent.log** enthält den folgenden Fehler, wenn die VM die IP-Adresse von WireServer auf dem Hostserver nicht erreichen kann.

```
2020-10-02T18:11:13.148998Z WARNING ExtHandler ExtHandler An error occurred while retrieving the goal state:
```

So lösen Sie das Problem:

* Stellen Sie per SSH eine Verbindung mit der VM her, und versuchen Sie dann, über cURL auf die folgende URL zuzugreifen: http://168.63.129.16/?comp=versions.
* Suchen Sie nach Problemen, die möglicherweise von einer Firewall, einem Proxy oder einer anderen Quelle verursacht werden, die ggf. den Zugriff auf die IP-Adresse 168.63.129.16 blockieren.
* Überprüfen Sie, ob IPTables von Linux oder eine Firewall eines Drittanbieters den Zugriff auf die Ports 80, 443 und 32526 blockiert.

## <a name="next-steps"></a>Nächste Schritte

Wenden Sie sich zum Beheben weiterer Probleme mit dem Azure Linux-Agent an den [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
