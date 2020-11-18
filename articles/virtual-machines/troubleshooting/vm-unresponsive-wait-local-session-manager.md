---
title: VM reagiert nicht, während auf den lokalen Sitzungs-Manager gewartet wird
description: Dieser Artikel enthält die Schritte zum Lösen von Problemen, bei denen das Gastbetriebssystem nicht mehr reagiert, während darauf gewartet wird, dass der lokale Sitzungs-Manager beim Starten einer Azure-VM die Verarbeitung beendet.
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
ms.openlocfilehash: ae3d35bea1c75c797447100316ceab9a27fcbe48
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423739"
---
# <a name="vm-is-unresponsive-while-waiting-for-the-local-session-manager"></a>VM reagiert nicht, während auf den lokalen Sitzungs-Manager gewartet wird

Dieser Artikel enthält die Schritte zum Lösen von Problemen, bei denen das Gastbetriebssystem nicht mehr reagiert, während darauf gewartet wird, dass der lokale Sitzungs-Manager beim Starten einer Azure-VM die Verarbeitung beendet.

## <a name="symptoms"></a>Symptome

Wenn Sie die [Startdiagnose](./boot-diagnostics.md) verwenden, um einen Screenshot der VM anzuzeigen, zeigt dieser Screenshot eine Eingabeaufforderung mit dieser Meldung an: *Warten auf „Lokaler Sitzungs-Manager“*

![Der Screenshot zeigt das Gastbetriebssystem, das bei der Meldung „Warten auf ‚Lokaler Sitzungs-Manger‘“ unter Windows Server 2012 R2 nicht mehr reagiert.](media/vm-unresponsive-wait-local-session-manager/vm-unresponsive-wait-local-session-manager-1.png)

## <a name="cause"></a>Ursache

Es kann verschiedene Gründe dafür geben, dass ein virtueller Computer beim Warten auf den lokalen Sitzungs-Manager nicht mehr reagiert. Wenn dies ein dauerhaftes Problem ist, müssen Sie zur Analyse ein Speicherabbild des Arbeitsspeichers erfassen.

## <a name="solution"></a>Lösung

In einigen Fällen reicht es aus, einfach lang genug zu warten, bis der Prozess abgeschlossen ist. Wenn die VM über eine Stunde lang nicht mehr reagiert, sollten Sie ein Speicherabbild des Arbeitsspeichers erfassen und sich dann an den Microsoft-Support wenden.

### <a name="collect-the-memory-dump-file"></a>Erfassen der Speicherabbilddatei

Um dieses Problem zu lösen, müssen Sie zunächst die Speicherabbilddatei für den Absturz erstellen und sich dann mit dieser an den Support wenden. Gehen Sie wie folgt vor, um die Speicherabbilddatei zu erfassen:

#### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Anfügen des Betriebssystemdatenträgers an eine neue Reparatur-VM

1. Führen Sie die [Schritte 1 bis 3 der VM-Reparaturbefehle](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) aus, um eine Reparatur-VM vorzubereiten.
2. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

#### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Suchen nach der Speicherabbilddatei und Senden eines Supporttickets

1. Navigieren Sie auf der Reparatur-VM zum Windows-Ordner auf dem angefügten Betriebssystemdatenträger. Wenn der dem angefügten Betriebssystemdatenträger zugewiesene Laufwerkbuchstabe *F* lautet, müssen Sie zu `F:\Windows` navigieren.
2. Suchen Sie nach der Datei **memory.dmp**, und [senden Sie dann ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) mit der Speicherabbilddatei.
3. Wenn Sie Probleme beim Auffinden der Datei **memory.dmp** haben, befolgen Sie die Anweisungen im Leitfaden zum [Generieren einer Absturzabbilddatei mithilfe von NMI-Aufrufen (nicht maskierbarer Interrupt)](/windows/client-management/generate-kernel-or-complete-crash-dump).

Weitere Informationen zu NMI-Aufrufen finden Sie im Benutzerhandbuch [Verwenden der seriellen Konsole für NMI-Aufrufe](./serial-console-windows.md#use-the-serial-console-for-nmi-calls).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Problembehandlung bei Startfehlern von virtuellen Azure-Computern](boot-error-troubleshoot.md)