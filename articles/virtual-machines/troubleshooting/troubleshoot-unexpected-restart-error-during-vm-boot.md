---
title: 'Betriebssystemstart: Computer wurde unerwartet neu gestartet, oder es ist ein unerwarteter Fehler aufgetreten'
description: Dieser Artikel enthält Schritte zum Beheben von Problemen, wenn für den virtuellen Computer bei der Installation von Windows ein unerwarteter Neustart oder Fehler auftritt.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 1d249a4e-71ba-475d-8461-31ff13e57811
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 06/22/2020
ms.author: v-mibufo
ms.openlocfilehash: 186b1c46303be59e191a1754361e07a2003b997a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87036181"
---
# <a name="os-start-up--computer-restarted-unexpectedly-or-encountered-an-unexpected-error"></a>Betriebssystemstart: Computer wurde unerwartet neu gestartet, oder es ist ein unerwarteter Fehler aufgetreten

Dieser Artikel enthält Schritte zum Beheben von Problemen, wenn für den virtuellen Computer (virtuelle Maschine, VM) bei der Installation von Windows ein unerwarteter Neustart oder Fehler auftritt.

## <a name="symptom"></a>Symptom

Wenn Sie die [Startdiagnose](./boot-diagnostics.md) verwenden, um den Screenshot des virtuellen Computers anzuzeigen, sehen Sie, dass im Screenshot eine fehlgeschlagene Windows-Installation mit dem folgenden Fehler angezeigt wird:

**Der Computer wurde unerwartet neu gestartet, oder ein unerwarteter Fehler ist aufgetreten. Die Windows-Installation kann nicht fortgesetzt werden. Klicken Sie auf "OK", um den Computer neu zu starten, und führen Sie die Installation dann erneut aus.**

![Fehler beim Ausführen der Windows-Installation: Der Computer wurde unerwartet neu gestartet, oder ein unerwarteter Fehler ist aufgetreten. Die Windows-Installation kann nicht fortgesetzt werden. Klicken Sie auf "OK", um den Computer neu zu starten, und führen Sie die Installation dann erneut aus.](./media/unexpected-restart-error-during-vm-boot/1.png)
 
![Fehler, wenn das Setupprogramm bei der Windows-Installation Dienste startet: Der Computer wurde unerwartet neu gestartet, oder ein unerwarteter Fehler ist aufgetreten. Die Windows-Installation kann nicht fortgesetzt werden. Klicken Sie auf "OK", um den Computer neu zu starten, und führen Sie die Installation dann erneut aus.](./media/unexpected-restart-error-during-vm-boot/2.png)

## <a name="cause"></a>Ursache

Der Computer versucht, einen Erststart eines [generalisierten Image](/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) auszuführen, stößt aber auf Probleme, weil eine benutzerdefinierte Antwortdatei (unattend.xml) verarbeitet wird. Benutzerdefinierte Antwortdateien werden in Azure nicht unterstützt. 

Die Antwortdatei ist eine spezielle XML-Datei, die Einstellungsdefinitionen und Werte für die Konfigurationseinstellungen enthält, die Sie während der Installation eines Windows Server-Betriebssystems automatisieren möchten. Zu den Konfigurationsoptionen gehören Anweisungen, wie Datenträger partitioniert werden sollen und wo das zu installierende Windows-Image, die anzuwendenden Produktschlüssel und weitere Befehle zu finden sind, die Sie ausführen möchten.

Benutzerdefinierte Antwortdateien werden in Azure nicht unterstützt. Dieser Fehler kann auftreten, wenn Sie mit der Option `sysprep /unattend:<your file’s name>` eine benutzerdefinierte **Unattend.xml**-Datei angegeben haben.

Verwenden Sie in Azure die Option **Out-of-Box-Experience (OOBE) für System aktivieren** in **Sysprep.exe**, oder verwenden Sie `sysprep /oobe` anstelle der Datei „Unattend.xml“.

Dieses Problem tritt meist auf, während Sie **Sysprep.exe** mit einer lokalen VM verwenden, um eine generalisierte VM in Azure hochzuladen. In dieser Situation möchten Sie möglicherweise auch wissen, wie ein generalisierter virtueller Computer ordnungsgemäß hochgeladen wird.

## <a name="solution"></a>Lösung

### <a name="replace-unattended-answer-file-option"></a>Ersetzen der Option für unbeaufsichtigte Antwortdatei

Diese Situation tritt auf, wenn ein Image für die Verwendung in Azure vorbereitet wurde, dabei aber eine benutzerdefinierte Antwortdatei verwendet wurde, die in Azure nicht unterstützt wird, und Sie **SYSPREP** mit einem Flag verwendet haben, das der Angabe im folgenden Befehl ähnelt:

`sysprep /oobe /generalize /unattend:<NameOfYourAnswerFile.XML> /shutdown`

- Ersetzen Sie in diesem Befehl den Dateinamen `<NameOfYourAnswerFile.XML>` durch den Namen Ihrer Datei.

Um dieses Problem zu beheben, befolgen Sie [die Azure-Anleitung zum Vorbereiten/Erfassen eines Image](../windows/upload-generalized-managed.md), und bereiten Sie ein neues generalisiertes Image vor. Verwenden Sie beim Ausführen von „sysprep“ nicht das Flag `/unattend:<answerfile>`. Verwenden Sie stattdessen nur die folgenden Flags:

`sysprep /oobe /generalize /shutdown`

- **Out-of-Box-Experience** (OOBE) ist die unterstützte Einstellung für Azure-VMs.

Sie können dieselbe Aufgabe wie im obigen Befehl auch ausführen, indem Sie in der grafischen Benutzeroberfläche von **System Preparation Tool** die nachstehend gezeigten Optionen auswählen:

- Enter System Out-of-Box-Experience
- Generalize
- Shutdown
 
![Fenster von „System Preparation Tool“ mit ausgewählten Optionen „OOBE“, „Generalize“ und „Shutdown“](./media/unexpected-restart-error-during-vm-boot/3.png)
