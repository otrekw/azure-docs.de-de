---
title: Windows-kritischer Fehler „Status System Process Terminated“
description: Dieser Artikel enthält Schritte zum Beheben von Problemen, bei denen der kritische Fehler 0xC000021A des Betriebssystems das Starten eines virtuellen Azure-Computers verhindert.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 037d0858-4611-4707-bd46-cc8085d011ed
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/21/2020
ms.author: v-mibufo
ms.openlocfilehash: b13b61aff819271ed1722572f251f9a6d14b17ab
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91976996"
---
# <a name="windows-stop-error---0xc000021a-status-system-process-terminated"></a>Windows-kritischer Fehler: 0xC000021A – Status System Process Terminated

Dieser Artikel enthält Schritte zum Beheben von Problemen, bei denen der kritische Fehler 0xC000021A des Betriebssystems (OS) das Starten eines virtuellen Azure-Computers verhindert.

## <a name="symptom"></a>Symptom

Wenn Sie [Startdiagnose](./boot-diagnostics.md) verwenden, um den Screenshot der VM anzuzeigen, zeigt der Screenshot die folgende Meldung an, dass für das Betriebssystem beim Starten ein Fehler aufgetreten ist:

**Auf dem PC ist ein Problem aufgetreten. Er muss neu gestartet werden. Wir erfassen nur einige Fehlerinformationen, anschließend können Sie neu starten (x % abgeschlossen). Wenn Sie mehr erfahren möchten, können Sie später online nach diesem Fehler suchen: 0xC000021a**.

  ![In Abbildung 1 wird der Fehlercode #0xC000021A mit der Meldung angezeigt, dass auf dem PC ein Problem aufgetreten ist und ein Neustart erfolgen muss. Es werden einige Fehlerinformationen gesammelt, dann kann ein Neustart ausgeführt werden.](./media/windows-stop-error-system-process-terminated/1-pc-problem-restart.png)

## <a name="cause"></a>Ursache

Der Windows-kritischer Fehler 0xC000021A bedeutet **STATUS_SYSTEM_PROCESS_TERMINATED**.

Dieser Fehler tritt auf, wenn ein kritischer Prozess, z. B. WinLogon („winlogon.exe“) oder das Clientserver-Laufzeitsubsystem („csrss.exe“) fehlschlägt. Sobald der Kernel feststellt, dass einer dieser Dienste beendet wurde, löst er den Fehler **STOP 0xC000021A** aus. Dieser Fehler kann verschiedene Ursachen haben, beispielsweise:

- Es wurden nicht übereinstimmende Systemdateien installiert.
- Eine Service Pack- oder KB-Updateinstallation ist fehlgeschlagen.
- Ein Sicherungsprogramm, das zum Wiederherstellen einer Festplatte verwendet wird, hat Dateien, die möglicherweise bereits verwendet wurden, nicht ordnungsgemäß wiederhergestellt.
- Es wurde ein inkompatibles Drittanbieterprogramm installiert.

## <a name="solution"></a>Lösung

### <a name="collect-the-memory-dump-file"></a>Erfassen der Speicherabbilddatei

Um dieses Problem zu beheben, muss das Speicherabbild analysiert werden. Erfassen Sie die Speicherabbilddatei für den Absturz, und wenden Sie sich an den Support. Gehen Sie wie folgt vor, um die Speicherabbilddatei zu erfassen:

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Anfügen des Betriebssystemdatenträgers an eine neue Reparatur-VM

1.  Führen Sie die Schritte 1-3 der [Reparaturbefehle für virtuelle Computer](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) aus, um eine Reparatur-VM vorzubereiten.
2.  Stellen Sie über eine **Remotedesktopverbindung** eine Verbindung mit der Reparatur-VM her.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Suchen nach der Speicherabbilddatei und Senden eines Supporttickets

1.  Navigieren Sie auf der Reparatur-VM zum Windows-Ordner auf dem angefügten Betriebssystemdatenträger. Wenn der dem angefügten Betriebssystemdatenträger zugewiesene Laufwerkbuchstabe „F“ ist, navigieren Sie zu „F:\Windows“.
2.  Suchen Sie nach der Datei „memory.dmp“, und [senden Sie dann ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) mit der Speicherabbilddatei.
3.  Wenn Sie die Datei „memory.dmp“ nicht finden, sollten Sie möglicherweise stattdessen [NMI-Aufrufe (Nicht maskierbarer Interrupt) in der seriellen Konsole](./serial-console-windows.md#use-the-serial-console-for-nmi-calls) verwenden. Sie können den Leitfaden zum [Generieren eines Absturzabbilds mithilfe von NMI-Aufrufen](/windows/client-management/generate-kernel-or-complete-crash-dump) befolgen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Problembehandlung finden Sie unter [Problembehandlung von häufigen Startfehlern](./boot-error-troubleshoot.md) oder [Problembehandlung einer Windows-VM durch Anfügen des Betriebssystemdatenträgers an eine Wiederherstellungs-VM](./troubleshoot-recovery-disks-windows.md). Sie sollten sich auch mit [Verwenden von Startdiagnose für die Problembehandlung virtueller Computer](./boot-diagnostics.md) vertraut machen.
- Weitere Informationen zu Resource Manager finden Sie unter [Übersicht über den Azure Resource Manager](../../azure-resource-manager/management/overview.md).
- Wenn Sie keine Verbindung mit Ihrer VM herstellen können, helfen Ihnen die Informationen unter [Problembehandlung von Remotedesktopverbindungen mit einer Azure-VM](./troubleshoot-rdp-connection.md) weiter.