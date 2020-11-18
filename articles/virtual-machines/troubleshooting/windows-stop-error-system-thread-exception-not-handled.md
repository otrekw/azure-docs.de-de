---
title: 'Windows: kritischer Fehler 0x0000007E – Systemthreadausnahme nicht behandelt'
description: In diesem Artikel finden Sie Schritte zum Beheben von Problemen, bei denen im Gastbetriebssystem ein Problem auftritt und daher ein Neustart Ihrer Azure-VM angefordert wird. Die Meldung enthält die Information, dass eine „Systemthreadausnahme nicht behandelt“ wurde.
services: virtual-machines-windows
documentationcenter: ''
author: mibufo
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 11/04/2020
ms.author: v-mibufo
ms.openlocfilehash: dd18d69009b9c150c4c12a755e9060cd5dfaccae
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423737"
---
# <a name="windows-stop-error---0x0000007e-system-thread-exception-not-handled"></a>Windows: kritischer Fehler 0x0000007E – Systemthreadausnahme nicht behandelt

In diesem Artikel finden Sie Schritte zum Beheben von Problemen, bei denen im Gastbetriebssystem ein Problem auftritt und daher ein Neustart Ihrer Azure-VM angefordert wird. Die Meldung enthält die Information, dass eine „Systemthreadausnahme nicht behandelt“ wurde.

## <a name="symptoms"></a>Symptome

Wenn Sie die [Startdiagnose](./boot-diagnostics.md) verwenden, um den Screenshot der VM anzuzeigen, sehen Sie, dass darin angezeigt wird, dass Windows neu gestartet werden muss: mit dem Stoppcode **Systemthreadausnahme nicht behandelt** oder mit dem Fehlercode **0x0000007E**.

![Screenshot der Situation, dass Windows während des Starts mit der folgenden Meldung hängen bleibt: „Auf dem PC ist ein Problem aufgetreten. Er muss neu gestartet werden. Wir führen den Neustart für Sie durch.“ Stop code: „SYSTEMTHREADAUSNAHME NICHT BEHANDELT“](media/windows-stop-error-system-thread-exception-not-handled/windows-stop-error-system-thread-exception-not-handled-1.png)

## <a name="cause"></a>Ursache

Die Ursache kann erst bestimmt werden, wenn eine Speicherabbilddatei analysiert wird. Fahren Sie mit dem Erfassen der Speicherabbilddatei fort.

## <a name="solution"></a>Lösung

### <a name="collect-the-memory-dump-file"></a>Erfassen der Speicherabbilddatei

Um dieses Problem zu behandeln, müssen Sie zunächst die Speicherabbilddatei für den Absturz erstellen und an den Support senden. Gehen Sie wie folgt vor, um die Speicherabbilddatei zu erfassen:

#### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Anfügen des Betriebssystemdatenträgers an eine neue Reparatur-VM

1. Führen Sie die [Schritte 1 bis 3 der VM-Reparaturbefehle](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) aus, um eine Reparatur-VM vorzubereiten.
2. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

#### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Suchen nach der Speicherabbilddatei und Senden eines Supporttickets

1. Navigieren Sie auf der Reparatur-VM zum Windows-Ordner auf dem angefügten Betriebssystemdatenträger. Wenn der dem angefügten Betriebssystemdatenträger zugewiesene Laufwerkbuchstabe *F* lautet, müssen Sie zu `F:\Windows` navigieren.
2. Suchen Sie nach der Datei **memory.dmp**, und [senden Sie dann ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) mit der Speicherabbilddatei.
3. Wenn Sie Probleme beim Auffinden der Datei **memory.dmp** haben, befolgen Sie die Anweisungen im Leitfaden zum [Generieren einer Absturzabbilddatei mithilfe von NMI-Aufrufen (nicht maskierbarer Interrupt)](/windows/client-management/generate-kernel-or-complete-crash-dump).

Weitere Informationen zu NMI-Aufrufen finden Sie im Benutzerhandbuch zu [NMI-Aufrufen (nicht maskierbarer Interrupt) in der seriellen Konsole](./serial-console-windows.md#use-the-serial-console-for-nmi-calls).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Problembehandlung bei Startfehlern von Azure-VMs](./boot-error-troubleshoot.md)