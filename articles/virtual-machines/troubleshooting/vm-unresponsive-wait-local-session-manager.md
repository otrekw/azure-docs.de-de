---
title: VM reagiert nicht, während auf den Dienst „Lokaler Sitzungs-Manager“ gewartet wird
description: Dieser Artikel enthält die Schritte zum Lösen von Problemen, bei denen das Gastbetriebssystem nicht mehr reagiert, während darauf gewartet wird, dass „Lokaler Sitzungs-Manager“ beim Starten einer Azure-VM die Verarbeitung beendet.
services: virtual-machines-windows
documentationcenter: ''
author: mibufo
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/22/2020
ms.author: v-mibufo
ms.openlocfilehash: fc3bd5d2590e969db07e9dffa61b4902ea4604c3
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632545"
---
# <a name="vm-is-unresponsive-while-waiting-for-the-local-session-manager-service"></a>VM reagiert nicht, während auf den Dienst „Lokaler Sitzungs-Manager“ gewartet wird

Dieser Artikel enthält die Schritte zum Lösen von Problemen, bei denen das Gastbetriebssystem nicht mehr reagiert, während darauf gewartet wird, dass „Lokaler Sitzungs-Manager“ beim Starten einer Azure-VM die Verarbeitung beendet.

## <a name="symptoms"></a>Symptome

Wenn Sie die [Startdiagnose](./boot-diagnostics.md) zum Anzeigen eines Screenshots der VM-Ausgabe verwenden, sehen Sie, dass für den Screenshot eine Eingabeaufforderung mit der Meldung „Warten auf Lokaler Sitzungs-Manager“ angezeigt wird.

![Screenshot: Nicht mehr reagierendes Gastbetriebssystem unter Windows Server 2012 R2 mit Meldung „Warten auf Lokaler Sitzungs-Manager“](media/vm-unresponsive-wait-local-session-manager/vm-unresponsive-wait-local-session-manager-1.png)

## <a name="cause"></a>Ursache

Es kann mehrere Gründe geben, warum ein virtueller Computer beim Warten auf „Lokaler Sitzungs-Manager“ nicht mehr reagiert. Falls dieses Problem weiterhin besteht, sollten Sie zu Analysezwecken ein Speicherabbild erfassen.

## <a name="solution"></a>Lösung

> [!TIP]
> Wenn Sie über eine aktuelle Sicherung der VM verfügen, können Sie versuchen, die [VM aus der Sicherung wiederherzustellen](../../backup/backup-azure-arm-restore-vms.md), um das Startproblem zu beheben.

In einigen Fällen kann das Problem behoben werden, indem einfach auf den Abschluss des Prozesses gewartet wird. Wenn Ihre VM nicht mehr reagiert und der Wartebildschirm länger als eine Stunde angezeigt wird, sollten Sie ein Speicherabbild des Arbeitsspeichers erfassen und sich dann an den Microsoft-Support wenden.

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Anfügen des Betriebssystemdatenträgers an eine neue Reparatur-VM

1. Führen Sie die [Schritte 1 bis 3 der VM-Reparaturbefehle](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) aus, um eine Reparatur-VM vorzubereiten.
1. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Suchen nach der Speicherabbilddatei und Senden eines Supporttickets

1. Navigieren Sie auf der Reparatur-VM zum Windows-Ordner auf dem angefügten Betriebssystemdatenträger. Wenn dem angefügten Betriebssystemdatenträger beispielsweise der Laufwerkbuchstabe *F* zugeordnet ist, rufen Sie `F:\Windows` auf.
1. Suchen Sie nach der Datei *memory.dmp*, und [senden Sie dann ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) mit der Speicherabbilddatei als Anhang.
1. Wenn Sie Probleme beim Auffinden der Datei *memory.dmp* haben, sollten Sie die Anweisungen im Leitfaden zum [Generieren einer Absturzabbilddatei mithilfe von NMI-Aufrufen (nicht maskierbarer Interrupt)](/windows/client-management/generate-kernel-or-complete-crash-dump) befolgen.

Weitere Informationen zu NMI-Aufrufen finden Sie im Benutzerhandbuch zu [NMI-Aufrufen in der seriellen Azure-Konsole](./serial-console-windows.md#use-the-serial-console-for-nmi-calls).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Problembehandlung bei Startfehlern von Azure-VMs](boot-error-troubleshoot.md)