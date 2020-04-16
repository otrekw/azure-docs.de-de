---
title: Unterstützung für Az-Module in Azure Automation
description: Dieser Artikel enthält Informationen zum Verwenden von Az-Modulen in Azure Automation.
services: automation
ms.subservice: shared-capabilities
ms.date: 02/08/2019
ms.topic: conceptual
ms.openlocfilehash: a8d6d25a2ba7f0040b13982f14f3d6081ac32f15
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637993"
---
# <a name="az-module-support-in-azure-automation"></a>Unterstützung für Az-Module in Azure Automation

Azure Automation unterstützt die Verwendung des [Az-Moduls von Azure PowerShell](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) in Ihren Runbooks. Das Az-Rollupmodul wird nicht automatisch in neue oder vorhandenen Automation-Konten importiert. 

## <a name="considerations"></a>Überlegungen

Es gilt zahlreiche Dinge zu beachten, wenn Sie das Az-Modul in Azure Automation verwenden:

* Allgemeinere Lösungen in Ihrem Automation-Konto können Runbooks und Module nutzen. Daher kann das Bearbeiten von Runbooks oder Aktualisieren von Modulen zu Problemen mit Ihren Runbooks führen. Sie sollten alle Runbooks und Lösungen in einem separaten Automation-Konto sorgfältig testen, bevor Sie neue Az-Module importieren. 

* Änderungen an Modulen können die Lösung zum [Starten/Beenden](automation-solution-vm-management.md) negativ beeinflussen. 

* Beim Importieren eines Az-Moduls in Ihr Automation-Konto wird das Modul in der PowerShell-Sitzung, die die Runbooks verwenden, nicht automatisch importiert. Module werden in den folgenden Situationen in die PowerShell-Sitzung importiert:

    * Wenn ein Runbook ein Cmdlet in einem Modul aufruft
    * Wenn ein Runbook das Modul mit einem `Import-Module`-Cmdlet explizit importiert
    * Wenn ein Runbook ein anderes Modul importiert, das vom Modul abhängig ist

> [!NOTE]
> Es wird nicht empfohlen, Module und Runbooks in Automation-Accounts zu ändern, die Lösungen enthalten. Diese Vorkehrung ist nicht spezifisch für die Az-Module. Sie sollte stets bei der Einführung von Änderungen in Ihrem Automation-Konto berücksichtigt werden.

> [!IMPORTANT]
> Stellen Sie sicher, dass Runbooks in einem Automation-Konto entweder nur Az-Module oder nur [AzureRM](https://www.powershellgallery.com/packages/AzureRM/6.13.1)-Module in eine PowerShell-Sitzung importieren. Wenn ein Runbook Az-Module vor AzureRM-Modulen importiert, wird das Runbook abgeschlossen. Wenn jedoch in den Auftragsdatenströmen ein Fehler beim Verweisen auf das Cmdlet [Get_SerializationSettings](troubleshoot/runbooks.md#get-serializationsettings) auftritt, werden die Cmdlets möglicherweise nicht ordnungsgemäß ausgeführt. Wenn das Runbook AzureRM-Module vor Az-Modulen importiert, wird das Runbook ebenfalls abgeschlossen. In diesem Fall erhalten Sie jedoch einen Fehler in den Auftragsdatenströmen, der besagt, dass sowohl Az- als auch AzureRM-Module nicht in derselben Sitzung importiert oder im selben Runbook verwendet werden dürfen.

## <a name="migrating-to-az-modules"></a>Migrieren zu Az-Modulen

Es wird empfohlen, eine Migration zu Az-Modulen in einem Automation-Testkonto zu testen. Sobald Sie das Konto erstellt haben, können Sie die Anweisungen in diesem Abschnitt befolgen, um mit den Modulen zu arbeiten.

### <a name="stop-and-unschedule-all-runbooks-that-use-azurerm-modules"></a>Anhalten und Aufheben des Zeitplans aller Runbooks, die AzureRM-Module verwenden

Um sicherzustellen, dass Sie keine bestehenden Runbooks ausführen, die AzureRM-Module verwenden, beenden Sie alle betroffenen Runbooks, und entfernen Sie sie aus dem Zeitplan. Sie können sehen, welche Zeitpläne vorhanden sind und welche Zeitpläne zu entfernen sind, indem Sie Code ähnlich wie in diesem Beispiel ausführen:

  ```powershell-interactive
  Get-AzureRmAutomationSchedule -AutomationAccountName "<AutomationAccountName>" -ResourceGroupName "<ResourceGroupName>" | Remove-AzureRmAutomationSchedule -WhatIf
  ```

Es ist wichtig, jeden Zeitplan separat zu überprüfen, um sicherzustellen, dass Sie ihn in Zukunft bei Bedarf für Ihre Runbooks neu planen können.

### <a name="import-the-az-modules"></a>Importieren der Az-Module

>[!NOTE]
>Lassen Sie Ihre Runbooks nur Az-Module importieren. Importieren Sie nicht das Az-Rollupmodul, da es alle Az-Module enthält. Diese Anleitung gilt für alle Module.

Das Modul [Az.Accounts](https://www.powershellgallery.com/packages/Az.Accounts/1.1.0) stellt für die anderen Az-Module eine Abhängigkeit dar. Aus diesem Grund müssen Ihre Runbooks dieses Modul in Ihr Automation-Konto importieren, bevor Sie andere Module importieren können.

So importieren Sie die Module im Azure-Portal

1. Wählen Sie in Ihrem Automation-Konto unter **Freigegebene Ressourcen** die Option **Module** aus. 
2. Klicken Sie auf **Katalog durchsuchen**, um die Seite „Katalog durchsuchen“ zu öffnen.  
3. Geben Sie in die Suchleiste den Namen des Moduls ein, z. B. `Az.Accounts`. 
4. Klicken Sie auf der Seite „PowerShell-Module“ auf **Importieren**, um das Modul in Ihr Automation-Konto zu importieren.

![Importieren von Modulen aus einem Automation-Konto](media/az-modules/import-module.png)

Dieser Importvorgang kann auch über den [PowerShell-Katalog](https://www.powershellgallery.com) erfolgen, indem nach dem zu importierenden Modul gesucht wird. Sobald Sie das Modul gefunden haben, wählen Sie es aus. Wählen Sie die Registerkarte **Azure Automation** aus, und klicken Sie auf **In Azure Automation bereitstellen**.

![Importieren von Modulen direkt aus dem Katalog](media/az-modules/import-gallery.png)

## <a name="testing-your-runbooks"></a>Testen Ihrer Runbooks

Sobald Sie die Az-Module in das Automation-Konto importiert haben, können Sie beginnen, Ihre Runbooks so zu bearbeiten, dass Sie die Module verwenden. Die Mehrzahl der Cmdlets hat denselben Namen wie beim AzureRM-Modul, mit der Ausnahme, dass das Präfix AzureRM (oder AzureRm) in Az geändert wurde. Eine Liste der Module, die diese Namenskonvention nicht befolgen, finden Sie in der [Liste der Ausnahmen](/powershell/azure/migrate-from-azurerm-to-az#update-cmdlets-modules-and-parameters).

Eine Möglichkeit zum Testen der Änderung eines Runbooks zur Verwendung der neuen Cmdlets besteht darin, `Enable-AzureRmAlias -Scope Process` am Anfang des Runbooks anzugeben. Wenn Sie diesen Befehl Ihrem Runbook hinzufügen, kann das Skript ohne Änderungen ausgeführt werden.

## <a name="after-migration-details"></a>Details nach der Migration

Versuchen Sie nach Abschluss der Migration nicht, Runbooks mit AzureRM-Modulen im Automation-Konto zu starten. Es wird auch empfohlen, keine AzureRM-Module in das Konto zu importieren oder zu aktualisieren. Betrachten Sie das Konto als zu Az migriert, und arbeiten Sie nur mit Az-Modulen. 

Wenn Sie ein neues Automation-Konto erstellen, sind die vorhandenen AzureRM-Module weiterhin installiert. Sie können die Runbooks von Tutorials weiterhin mit AzureRM-Cmdlets aktualisieren. Sie dürfen diese Runbooks jedoch nicht ausführen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Az-Modulen finden Sie unter [Erste Schritte mit Az-Modulen](/powershell/azure/get-started-azureps?view=azps-1.1.0).
