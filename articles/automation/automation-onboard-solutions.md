---
title: Durchführen des Onboardings von Update-, Änderungsnachverfolgungs- und Bestandslösungen in Azure Automation
description: Es wird beschrieben, wie Sie Update- und Änderungsnachverfolgungslösungen in Azure Automation integrieren.
services: automation
ms.topic: tutorial
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 721157c333e381799ef08930c667c51a51a4fd6a
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457619"
---
# <a name="onboard-update-change-tracking-and-inventory-solutions-to-azure-automation"></a>Durchführen des Onboardings von Update-, Änderungsnachverfolgungs- und Bestandslösungen in Azure Automation

In diesem Tutorial wird beschrieben, wie Sie Update-, Änderungsnachverfolgungs- und Inventarlösungen für VMs in Azure Automation integrieren:

> [!div class="checklist"]
> * Integrieren einer Azure-VM
> * Aktivieren von Lösungen
> * Installieren und Aktualisieren von Modulen
> * Importieren des Runbooks für die Integration
> * Starten des Runbooks

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorials müssen folgende Voraussetzungen erfüllt sein:

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](automation-offering-get-started.md) zum Verwalten von Computern.
* Einen [virtuellen Computer](../virtual-machines/windows/quick-create-portal.md), der integriert werden soll.

## <a name="onboard-an-azure-vm"></a>Integrieren einer Azure-VM

Es gibt mehrere Möglichkeiten, Computer zu integrieren. Sie können die Lösung [über einen virtuellen Computer](automation-onboard-solutions-from-vm.md), [durch Durchsuchen mehrerer Computer](automation-onboard-solutions-from-browse.md), [über Ihr Automation-Konto](automation-onboard-solutions-from-automation-account.md) oder per Runbook integrieren. In diesem Tutorial wird das Aktivieren der Updateverwaltung über ein Runbook Schritt für Schritt beschrieben. Um virtuelle Azure-Computer bedarfsgesteuert zu integrieren, muss eine vorhandene VM mit der Lösung für die Änderungsnachverfolgung oder Updateverwaltung integriert werden. In diesem Schritt integrieren Sie eine VM mithilfe der Updateverwaltung und Änderungsnachverfolgung.

### <a name="enable-change-tracking-and-inventory"></a>Aktivieren der Lösung für Änderungsnachverfolgung und Bestand

Die Lösungen für Änderungsnachverfolgung und Bestand bieten die Möglichkeit zum Nachverfolgen von [Änderungen](automation-vm-change-tracking.md) und [Bestand](automation-vm-inventory.md) auf Ihren virtuellen Computern. In diesem Schritt aktivieren Sie die Lösungen auf einem virtuellen Computer.

1. Wählen Sie im Azure-Portal die Option **Automation-Konten**, und wählen Sie dann in der Liste Ihr Automation-Konto aus.
1. Wählen Sie unter **Konfigurationsverwaltung** die Option **Bestand** aus.
1. Wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich, oder erstellen Sie einen neuen. 
1. Klicken Sie auf **Aktivieren**.

    ![Integrieren der Updatelösung](media/automation-onboard-solutions/inventory-onboard.png)

### <a name="enable-update-management"></a>Aktivieren der Updateverwaltung

Mithilfe der Lösung zur Updateverwaltung können Sie Updates und Patches für Ihre Azure-Windows-VMs verwalten. Sie können den Status der verfügbaren Updates bewerten, die Installation der erforderlichen Updates planen und Bereitstellungsergebnisse überprüfen, um sicherzustellen, dass Updates erfolgreich auf die VM angewendet wurden. In diesem Schritt aktivieren Sie die Lösung für Ihre VM.

1. Wählen Sie in Ihrem Automation-Konto im Abschnitt **Updateverwaltung** die Option **Updateverwaltung** aus.
1. Der ausgewählte Log Analytics-Arbeitsbereich ist derselbe, der im vorherigen Schritt verwendet wurde. Klicken Sie auf **Aktivieren**, um die Lösung für die Updateverwaltung zu integrieren. Während die Lösung zur Updateverwaltung installiert wird, wird ein blaues Banner angezeigt. 

    ![Integrieren der Updatelösung](media/automation-onboard-solutions/update-onboard.png)

### <a name="select-azure-vm-to-be-managed"></a>Auswählen der zu verwaltenden VM

Nachdem die Lösungen aktiviert wurden, können Sie eine Azure-VM hinzufügen, die in diese Lösungen integriert werden soll.

1. Wählen Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** die Option **Änderungsnachverfolgung** aus. 
2. Klicken Sie auf der Seite „Änderungsnachverfolgung“ auf **Azure-VMs hinzufügen**, um Ihren virtuellen Computer hinzuzufügen.

3. Wählen Sie Ihren virtuellen Computer aus der Liste aus, und klicken Sie auf **Aktivieren**. Dadurch werden die Lösungen für Änderungsnachverfolgung und Bestand für den virtuellen Computer aktiviert.

   ![Aktivieren der Updatelösung für die VM](media/automation-onboard-solutions/enable-change-tracking.png)

4. Wenn Sie die Benachrichtigung erhalten haben, dass die VM integriert wurde, wählen Sie in Ihrem Automation-Konto unter **Updateverwaltung** die Option **Updateverwaltung** aus.

5. Wählen Sie **Azure-VMs hinzufügen**, um Ihren virtuellen Computer hinzuzufügen

6. Wählen Sie Ihre VM aus der Liste aus, und klicken Sie auf **Aktivieren**. Dadurch wird die Lösung zur Updateverwaltung für den virtuellen Computer aktiviert.

   ![Aktivieren der Updatelösung für die VM](media/automation-onboard-solutions/enable-update.png)

> [!NOTE]
> Wenn Sie nicht auf den Abschluss der anderen Lösung warten, erhalten Sie beim Aktivieren der nächsten Lösung die folgende Meldung: `Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="install-and-update-modules"></a>Installieren und Aktualisieren von Modulen

Für ein erfolgreiches Automatisieren der Lösungsintegration müssen Sie das Update auf die aktuellen Azure-Module durchführen und das Modul [Az.OperationalInsights](https://docs.microsoft.com/powershell/module/az.operationalinsights/?view=azps-3.7.0) importieren.

## <a name="update-azure-modules"></a>Aktualisieren von Azure-Modulen

1. Wählen Sie in Ihrem Automation-Konto unter **Freigegebene Ressourcen** die Option **Module** aus. 
2. Klicken Sie auf **Azure-Module aktualisieren**, um die Azure-Module auf die neueste Version zu aktualisieren. 
3. Klicken Sie auf **Ja**, um alle vorhandenen Azure-Module auf die aktuelle Version zu aktualisieren.

![Aktualisieren von Modulen](media/automation-onboard-solutions/update-modules.png) A

### <a name="install-azoperationalinsights-module"></a>Installieren des Moduls „Az.OperationalInsights“

1. Wählen Sie im Automation-Konto unter **Freigegebene Ressourcen** die Option **Module** aus. 
2. Wählen Sie die Option **Katalog durchsuchen** aus, um den Modulkatalog zu öffnen. 
3. Suchen Sie nach „Az.OperationalInsights“, und importieren Sie dieses Modul in das Automation-Konto.

![Importieren des OperationalInsights-Moduls](media/automation-onboard-solutions/import-operational-insights-module.png)

## <a name="import-the-onboarding-runbook"></a>Importieren des Runbooks für die Integration

1. Wählen Sie in Ihrem Automation-Konto unter **Prozessautomatisierung** die Option **Runbooks** aus.
1. Klicken Sie auf **Katalog durchsuchen**.
1. Suchen Sie nach `update and change tracking`.
3. Wählen Sie das Runbook aus, und klicken Sie auf der Seite „Quelle anzeigen“ auf **Importieren**. 
4. Klicken Sie auf **OK**, um das Runbook in das Automation-Konto zu importieren.

   ![Importieren des Runbooks für die Integration](media/automation-onboard-solutions/import-from-gallery.png)

6. Klicken Sie auf der Seite „Runbook“ auf **Bearbeiten**, und wählen Sie dann **Veröffentlichen** aus. 
7. Klicken Sie im Bereich „Runbook veröffentlichen“ auf **Ja**, um das Runbook zu veröffentlichen.

## <a name="start-the-runbook"></a>Starten des Runbooks

Sie müssen entweder eine Änderungsnachverfolgungs- oder Updatelösung in eine Azure-VM integriert haben, um dieses Runbook starten zu können. Hierfür sind ein vorhandener virtueller Computer und eine Ressourcengruppe sowie die integrierte Lösung für Parameter erforderlich.

1. Öffnen Sie das Runbook **Enable-MultipleSolution**.

   ![Runbooks für mehrere Lösungen](media/automation-onboard-solutions/runbook-overview.png)

1. Klicken Sie auf die Schaltfläche „Start“, und geben Sie die folgenden Werte für die Parameter ein:

   * **VMNAME**: Lassen Sie diesen Wert leer. Der Name einer vorhandenen VM für die Integration der Update- oder Änderungsnachverfolgungslösung. Wenn Sie diesen Wert leer lassen, werden alle VMs der Ressourcengruppe integriert.
   * **VMRESOURCEGROUP**: Der Name der Ressourcengruppe für die VMs, die integriert werden sollen.
   * **SUBSCRIPTIONID**: Lassen Sie diesen Wert leer. Die Abonnement-ID der neuen VM, die integriert werden soll. Wenn Sie diesen Wert leer lassen, wird das Abonnement des Arbeitsbereichs verwendet. Wenn eine andere Abonnement-ID angegeben wird, sollte das RunAs-Konto für das Automation-Konto auch als Mitwirkender für das Abonnement hinzugefügt werden.
   * **ALREADYONBOARDEDVM**: Der Name der VM, die für die Update- oder Änderungsnachverfolgungslösung manuell integriert wurde.
   * **ALREADYONBOARDEDVMRESOURCEGROUP**: Der Name der Ressourcengruppe, der die VM angehört.
   * **SOLUTIONTYPE**: Geben Sie **Updates** oder **ChangeTracking** ein.

   ![Parameter für das Enable-MultipleSolution-Runbook](media/automation-onboard-solutions/runbook-parameters.png)

1. Klicken Sie auf **OK**, um den Runbookauftrag zu starten.
1. Überwachen Sie den Status und etwaige Fehler auf der Seite für den Runbookauftrag.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

So entfernen Sie einen virtuellen Computer aus der Updateverwaltung:

1. Entfernen Sie in Ihrem Log Analytics-Arbeitsbereich den virtuellen Computer aus der gespeicherten Suche für die Bereichskonfiguration `MicrosoftDefaultScopeConfig-Updates`. Gespeicherte Suchen finden Sie unter **Allgemein** in Ihrem Arbeitsbereich.
2. Entfernen Sie den [Log Analytics-Agent für Windows](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) oder den [Log Analytics-Agent für Linux](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Manuelles Integrieren eines virtuellen Azure-Computers
> * Installieren und Aktualisieren der erforderlichen Azure-Module
> * Importieren eines Runbooks für die Integration von Azure-VMs
> * Starten des Runbooks für die automatische Integration von Azure-VMs

Klicken Sie auf diesen Link, um weitere Informationen zum Planen von Runbooks zu erhalten:

> [!div class="nextstepaction"]
> [Planen von RunBooks](automation-schedules.md)
