---
title: Problembehandlung bei Startfehlern von virtuellen Linux-Computern in Azure | Microsoft-Dokumentation
description: Dieser Artikel enthält Links zu Artikeln, die sich mit der Problembehandlung bei Startfehlern von virtuellen Linux-Computern in Azure befassen.
services: virtual-machines-linux
documentationCenter: ''
author: vilibert
manager: spogge
editor: ''
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/20/2019
ms.author: vilibert
ms.openlocfilehash: 37cb201751f72918838efe5837aa0e357d483f24
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74408560"
---
# <a name="troubleshoot-azure-linux-virtual-machines-boot-errors"></a>Problembehandlung bei Startfehlern von virtuellen Linux-Computern in Azure

In diesem Artikel werden häufige Startfehler aufgeführt, die beim Starten eines virtuellen Linux-Computers (Virtual Machine, VM) in Microsoft Azure auftreten können. Weitere Informationen zu den Fehlern finden Sie in den im Abschnitt **Startfehler und Lösungen** aufgeführten Artikeln.

## <a name="boot-errors-and-solutions"></a>Startfehler und Lösungen

* [GRUB-Wiederherstellung](troubleshoot-vm-boot-error.md)

## <a name="next-steps"></a>Nächste Schritte

- [Serielle Konsole für virtuelle Computer](serial-console-linux.md)

Beheben von Problemen einer Linux-VM durch Anfügen des Betriebssystemdatenträgers an eine Wiederherstellungs-VM mithilfe von Azure:

- [Reparieren einer Azure-VM](repair-linux-vm-using-azure-virtual-machine-repair-commands.md)

 Austausch des Datenträgers – Dies kann durch Folgendes automatisiert werden:
- [PowerShell-Wiederherstellungsskripts](https://github.com/Azure/azure-support-scripts/tree/master/VMRecovery/ResourceManager)
- [Bash-Wiederherstellungsskripts](https://github.com/sribs/azure-support-scripts)

- [BEFEHLSZEILENSCHNITTSTELLE (CLI)](troubleshoot-recovery-disks-linux.md)
- [Azure-Portal](troubleshoot-recovery-disks-portal-linux.md)


## <a name="disk-swap-video"></a>Video zum Austausch eines Datenträgers:

Wenn Sie keinen Zugriff auf GRUB haben, sehen Sie sich [dieses](https://youtu.be/m5t0GZ5oGAc) Video an, und sehen Sie, wie Sie den Austausch des Datenträgers einfach automatisieren können, um Ihre VM wiederherzustellen.

## <a name="unofficial-solution"></a>Inoffizielle Lösung

Die Wiederherstellung einer VM kann auch mit dem nicht unterstützten BETA-Skript [ALAR](https://github.com/malachma/azure-auto-recover) versucht werden.