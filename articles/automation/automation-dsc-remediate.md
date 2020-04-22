---
title: Korrigieren nicht konformer Azure Automation State Configuration-Server
description: Es wird beschrieben, wie Sie Konfigurationen bedarfsabhängig erneut auf Server anwenden, für die sich der Konfigurationszustand geändert hat.
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.topic: conceptual
ms.date: 07/17/2019
manager: nirb
ms.openlocfilehash: cc5884e1f70bdccee4e7a113e6e3ee2d6604b50a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81406087"
---
# <a name="remediate-noncompliant-dsc-servers"></a>Korrigieren nicht konformer DSC-Server

Wenn Server mit Azure Automation State Configuration registriert wurden, wird der Konfigurationsmodus auf `ApplyOnly`, `ApplyandMonitor` oder `ApplyAndAutoCorrect` festgelegt. Wenn der Modus nicht auf `ApplyAndAutoCorrect` festgelegt ist, verbleiben Server, für die aus irgendeinem Grund der konforme Zustand nicht mehr besteht, im nicht konformen Zustand, bis der Fehler manuell behoben wird.

Azure Compute verfügt über das Feature „Befehl ausführen“, mit dem Kunden Skripts auf virtuellen Computern ausführen können.
Dieses Dokument enthält Beispielskripts für dieses Feature, die genutzt werden können, wenn Abweichungen bei der Konfiguration manuell korrigiert werden sollen.

## <a name="correct-drift-of-windows-virtual-machines-using-powershell"></a>Korrigieren von Abweichungen virtueller Windows-Computer mit PowerShell

Eine Schrittanleitung zur Verwendung des Features „Befehl ausführen“ auf virtuellen Windows-Computern finden Sie in der Dokumentation auf der Seite [Ausführen von PowerShell-Skripts in Ihrer Windows-VM mit „Befehl ausführen“](/azure/virtual-machines/windows/run-command).

Verwenden Sie das Cmdlet [Update-DscConfiguration](/powershell/module/psdesiredstateconfiguration/update-dscconfiguration), um zu erzwingen, dass ein Azure Automation State Configuration-Knoten die aktuelle Konfiguration herunterlädt und anwendet.

```powershell
Update-DscConfiguration -Wait -Verbose
```

## <a name="correct-drift-of-linux-virtual-machines"></a>Korrigieren von Abweichungen virtueller Linux-Computer

Für Linux-Server ist derzeit keine vergleichbare Funktion verfügbar.
Die einzige Möglichkeit besteht darin, den Registrierungsvorgang zu wiederholen.

Für Azure-Knoten können Sie die Abweichung im Azure-Portal oder mithilfe von Cmdlets des Az-Moduls korrigieren. Ausführliche Informationen zu diesem Prozess sind unter [Onboarding von Computern zur Verwaltung durch Azure Automation State Configuration](automation-dsc-onboarding.md#onboard-vms-by-using-the-azure-portal) dokumentiert.
Für Hybridknoten kann die Korrektur von Abweichungen mit den enthaltenen Python-Skripts durchgeführt werden.
Weitere Informationen finden Sie im Repository [PowerShell DSC für Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux#performing-dsc-operations-from-the-linux-computer).

## <a name="next-steps"></a>Nächste Schritte

- Eine Referenz zu den PowerShell-Cmdlets finden Sie unter [Az.Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
).
- Ein Verwendungsbeispiel für Azure Automation State Configuration in einer Continuous Deployment-Pipeline finden Sie unter [Continuous Deployment mit Azure Automation State Configuration und Chocolatey](automation-dsc-cd-chocolatey.md).