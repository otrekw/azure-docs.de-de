---
title: Azure-Startdiagnose
description: Übersicht über die Azure-Startdiagnose und die verwaltete Startdiagnose
services: virtual-machines
ms.service: virtual-machines
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.date: 08/04/2020
ms.openlocfilehash: e2ba5d909a3aa43921f52295d2f7216aac76bc32
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88067085"
---
# <a name="azure-boot-diagnostics"></a>Azure-Startdiagnose

Die Startdiagnose ist ein Debuggingfeature für Azure-VMs, die eine Diagnose von Fehlern beim Starten von VMs ermöglicht. Mit der Startdiagnose können Benutzer anhand von Informationen des seriellen Protokolls und Screenshots den Zustand ihrer VMs beim Systemstart beobachten.

## <a name="boot-diagnostics-view"></a>Ansicht der Startdiagnose
Die Option der Startdiagnose befindet sich im Azure-Portal auf dem Blatt Ihrer VM im Abschnitt *Support und Problembehandlung*. Durch Auswählen der Startdiagnose werden ein Screenshot und Informationen des seriellen Protokolls angezeigt. Das serielle Protokoll enthält Kernelmeldungen, der Screenshot ist eine Momentaufnahme des aktuellen Zustands Ihrer VM. Je nachdem, ob auf der VM Windows oder Linux ausgeführt wird, unterscheiden sich die zu erwartenden Screenshots. Windows-Benutzern wird ein Desktophintergrund angezeigt, Linux-Benutzern eine Anmeldeaufforderung.

:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-linux.png" alt-text="Screenshot der Linux-Startdiagnose":::
:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-windows.png" alt-text="Screenshot der Windows-Startdiagnose":::


## <a name="limitations"></a>Einschränkungen
- Die Startdiagnose ist nur für VMs verfügbar, die über Azure Resource Manager bereitgestellt wurden. 
- Die Startdiagnose unterstützt keine Storage Premium-Konten. Wenn ein solches Konto für die Startdiagnose verwendet wird, erhalten Benutzer beim Starten der VM einen `StorageAccountTypeNotSupported`-Fehler. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [serielle Azure-Konsole](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-overview) und die Verwendung der Startdiagnose zur [Behandlung von Problemen mit virtuellen Computern in Azure](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics).
