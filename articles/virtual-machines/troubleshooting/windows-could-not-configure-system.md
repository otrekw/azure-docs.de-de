---
title: 'Problembehandlung: Windows konnte die Systemkonfiguration nicht abschließen.'
titlesuffix: Azure Virtual Machines
description: Dieser Artikel enthält Schritte zum Beheben von Problemen, bei denen der Sysprep-Prozess das Starten einer Azure-VM verhindert.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 5fc57a8f-5a0c-4b5f-beef-75eecb4d310d
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/09/2020
ms.author: v-miegge
ms.openlocfilehash: 9c170607d6300c4921285e85ac78db5a8a18ad9b
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078785"
---
# <a name="troubleshoot-windows-could-not-finish-configuring-the-system"></a>Problembehandlung: Windows konnte die Systemkonfiguration nicht abschließen.

Dieser Artikel enthält Schritte zum Beheben von Problemen, bei denen der Sysprep-Prozess das Starten eines virtuellen Azure Computers (Azure-VM) verhindert.

## <a name="symptom"></a>Symptom

Wenn Sie die [Startdiagnose](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) verwenden, um den Screenshot der VM anzuzeigen, wird im Screenshot ein „Windows installieren“-Fehler angezeigt, während Windows Setup Dienste startet. Für den Fehler wird folgende Meldung angezeigt:

`Windows could not finish configuring the system. To attempt to resume configuration, restart the computer. Setup is starting services`

  ![In Abbildung 1 wird ein „Windows installieren“-Fehler mit der folgenden Meldung angezeigt: „Die Konfiguration des Systems konnte nicht abgeschlossen werden. Starten Sie den Computer neu, um die Konfiguration fortzusetzen. Dienste werden gestartet.“](./media/windows-could-not-configure-system/1-windows-error-configure.png)

## <a name="cause"></a>Ursache

Dieser Fehler wird ausgelöst, wenn das Betriebssystem den [Sysprep-Prozess](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep-process-overview) nicht abschließen kann. Dieser Fehler tritt auf, wenn Sie versuchen, einen Erststart einer generalisierten VM auszuführen. Liegt dieses Problem vor, erstellen Sie das generalisierte Image erneut, da sich das Image in einem nicht bereitstellbaren Zustand befindet und nicht wiederhergestellt werden kann.

## <a name="solution"></a>Lösung

Um dieses Problem zu beheben, befolgen Sie [die Azure-Anleitung zum Vorbereiten/Erfassen eines Image](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed), und bereiten Sie ein neues generalisiertes Image vor.
