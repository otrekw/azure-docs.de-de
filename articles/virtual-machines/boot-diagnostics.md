---
title: Azure-Startdiagnose
description: Übersicht über die Azure-Startdiagnose und die verwaltete Startdiagnose
services: virtual-machines
ms.service: virtual-machines
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.date: 08/04/2020
ms.openlocfilehash: b51b44f3a3d0889836bb41e0bf2fa37234338cf4
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91287082"
---
# <a name="azure-boot-diagnostics"></a>Azure-Startdiagnose

Die Startdiagnose ist ein Debuggingfeature für Azure-VMs, die eine Diagnose von Fehlern beim Starten von VMs ermöglicht. Mit der Startdiagnose können Benutzer anhand von Informationen des seriellen Protokolls und Screenshots den Zustand ihrer VMs beim Systemstart beobachten.

## <a name="boot-diagnostics-storage-account"></a>Startdiagnose-Speicherkonto
Wenn Sie eine VM im Azure-Portal erstellen, ist Startdiagnose standardmäßig aktiviert. Die empfohlene Vorgehensweise bei der Startdiagnose ist die Verwendung eines verwalteten Speicherkontos, da dies zu erheblichen Leistungsverbesserungen der Zeit führt, die für die Erstellung einer Azure-VM benötigt wird. Dies liegt daran, dass ein verwaltetes Azure-Speicherkonto verwendet wird, wodurch der Zeitraum, der zum Erstellen eines neuen Benutzerspeicherkontos zum Speichern der Startdiagnosedaten benötigt wird, entfällt.

Eine alternative Vorgehensweise bei der Startdiagnose ist die Verwendung eines vom Benutzer verwalteten Speicherkontos. Ein Benutzer kann entweder ein neues Speicherkonto erstellen oder ein vorhandenes Konto verwenden.

> [!IMPORTANT]
> Azure-Kunden werden bis Ende Oktober 2020 nicht mit den Speicherkosten belastet, die bei der Startdiagnose über ein verwaltetes Speicherkonto anfallen.

## <a name="boot-diagnostics-view"></a>Ansicht der Startdiagnose
Die Option der Startdiagnose befindet sich im Azure-Portal auf dem Blatt Ihrer VM im Abschnitt *Support und Problembehandlung*. Durch Auswählen der Startdiagnose werden ein Screenshot und Informationen des seriellen Protokolls angezeigt. Das serielle Protokoll enthält Kernelmeldungen, der Screenshot ist eine Momentaufnahme des aktuellen Zustands Ihrer VM. Je nachdem, ob auf der VM Windows oder Linux ausgeführt wird, unterscheiden sich die zu erwartenden Screenshots. Windows-Benutzern wird ein Desktophintergrund angezeigt, Linux-Benutzern eine Anmeldeaufforderung.

:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-linux.png" alt-text="Screenshot der Linux-Startdiagnose":::
:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-windows.png" alt-text="Screenshot der Linux-Startdiagnose":::


## <a name="limitations"></a>Einschränkungen
- Die Startdiagnose ist nur für VMs verfügbar, die über Azure Resource Manager bereitgestellt wurden. 
- Die Startdiagnose unterstützt keine Storage Premium-Konten. Wenn ein solches Konto für die Startdiagnose verwendet wird, erhalten Benutzer beim Starten der VM einen `StorageAccountTypeNotSupported`-Fehler. 
- Verwaltete Speicherkonten werden ab Resource Manager-API-Version 2020-06-01 unterstützt.
- Die serielle Konsole von Azure ist zurzeit nicht mit einem verwalteten Speicherkonto für die Startdiagnose kompatibel. Weitere Informationen zur [seriellen Azure-Konsole](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-overview).
- Startdiagnose mit einem verwalteten Speicherkonto kann derzeit nur über das Azure-Portal angewendet werden. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [serielle Azure-Konsole](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-overview) und die Verwendung der Startdiagnose zur [Behandlung von Problemen mit virtuellen Computern in Azure](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics).
