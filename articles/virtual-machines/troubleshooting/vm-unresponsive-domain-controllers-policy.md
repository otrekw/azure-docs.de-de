---
title: Der virtuelle Computer reagiert nicht, während die Standarddomänencontroller-Richtlinie angewendet wird.
titlesuffix: Azure Virtual Machines
description: Dieser Artikel enthält Schritte zum Beheben von Problemen, bei denen die Standarddomänencontroller-Richtlinie das Starten einer Azure-VM verhindert.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 34e6b765-3496-46a1-b7d7-6def00884394
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/08/2020
ms.author: v-miegge
ms.openlocfilehash: 4f181a6dee7ed182150bd0cad2b51690b6c77d7a
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91963124"
---
# <a name="vm-is-unresponsive-while-applying-default-domain-controllers-policy"></a>Der virtuelle Computer reagiert nicht, während die Standarddomänencontroller-Richtlinie angewendet wird.

Dieser Artikel enthält Schritte zum Beheben von Problemen, bei denen die Standarddomänencontroller-Richtlinie das Starten eines virtuellen Azure-Computers verhindert.

## <a name="symptom"></a>Symptom

Wenn Sie die [Startdiagnose](./boot-diagnostics.md) verwenden, um den Screenshot der VM anzuzeigen, zeigt der Screenshot während des Startens das nicht mehr reagierende Betriebssystem mit der folgenden Meldung an: **Default Domain Controller Policy** (Standarddomänencontroller-Richtlinie).

  ![In Abbildung 1 wird das nicht mehr reagierende Betriebssystem mit der Meldung „Default Domain Controller Policy“ (Standarddomänencontroller-Richtlinie) angezeigt.](./media/vm-unresponsive-domain-controllers-policy/1-default-domain-controllers-policy.png)

## <a name="cause"></a>Ursache

Dieses Problem ist möglicherweise auf kürzlich erfolgte Änderungen an der Standarddomänencontroller-Richtlinie zurückzuführen. Wenn dies nicht zutrifft, muss eine Analyse der Speicherabbilddatei durchgeführt werden, um die Grundursache zu ermitteln.

## <a name="solution"></a>Lösung

Wenn Sie vor Kurzem Änderungen an der Standarddomänencontroller-Richtlinie vorgenommen haben, können Sie diese Änderungen rückgängig machen, um das Problem zu beheben. Wenn Sie sich über die Ursache des Problems nicht sicher sind, erfassen Sie ein Speicherabbild, und senden Sie ein Supportticket.

### <a name="collect-the-memory-dump-file"></a>Erfassen der Speicherabbilddatei

Um dieses Problem zu lösen, müssen Sie zunächst die Speicherabbilddatei für den Absturz erfassen und an den Support senden. Gehen Sie wie folgt vor, um die Speicherabbilddatei zu erfassen:

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Anfügen des Betriebssystemdatenträgers an eine neue Reparatur-VM

1. Führen Sie die Schritte 1-3 der [Reparaturbefehle für virtuelle Computer](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) aus, um eine Reparatur-VM vorzubereiten.

1. Stellen Sie über eine Remotedesktopverbindung eine Verbindung mit der Reparatur-VM her.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Suchen nach der Speicherabbilddatei und Senden eines Supporttickets

1. Navigieren Sie auf der Reparatur-VM zum Windows-Ordner auf dem angefügten Betriebssystemdatenträger. Wenn der dem angefügten Betriebssystemdatenträger zugewiesene Laufwerkbuchstabe beispielsweise `F` lautet, müssen Sie zu `F:\Windows` navigieren.

1. Suchen Sie nach der Datei „memory.dmp“, und [senden Sie dann ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) mit der Speicherabbilddatei.

1. Wenn Sie die Datei „memory.dmp“ nicht finden, sollten Sie möglicherweise stattdessen [NMI-Aufrufe (Nicht maskierbarer Interrupt) in der seriellen Konsole](./serial-console-windows.md#use-the-serial-console-for-nmi-calls) verwenden. Befolgen Sie den Leitfaden zum [Generieren eines Absturzabbilds mithilfe von NMI-Aufrufen](/windows/client-management/generate-kernel-or-complete-crash-dump).