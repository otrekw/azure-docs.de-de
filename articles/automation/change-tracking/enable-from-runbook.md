---
title: Aktivieren des Features „Änderungsnachverfolgung und Bestand“ von Azure Automation über ein Runbook
description: In diesem Artikel erfahren Sie, wie Sie „Änderungsnachverfolgung und Bestand“ über ein Runbook aktivieren.
services: automation
ms.subservice: change-inventory-management
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: 842b0a92ba4a2cb6b3ceb54675ef95f9c8275311
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92209121"
---
# <a name="enable-change-tracking-and-inventory-from-a-runbook"></a>Aktivieren von Änderungsnachverfolgung und Bestand über ein Runbook

In diesem Artikel erfahren Sie, wie Sie [Änderungsnachverfolgung und Bestand](overview.md) mithilfe eines Runbooks für virtuelle Computer in Ihrer Umgebung aktivieren. Wenn Sie virtuelle Azure-Computer im großen Stil aktivieren möchten, müssen Sie einen vorhandenen virtuellen Computer mithilfe von „Änderungsnachverfolgung und Bestand“ aktivieren.

> [!NOTE]
> Wenn Sie „Änderungsnachverfolgung und Bestand“ aktivieren, werden nur bestimmte Regionen für die Verknüpfung eines Log Analytics-Arbeitsbereichs und eines Automation-Kontos unterstützt. Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](../how-to/region-mappings.md).

Diese Methode verwendet zwei Runbooks:

* **Enable-MultipleSolution** : Das primäre Runbook, das zur Eingabe von Konfigurationsinformationen auffordert, den angegebenen virtuellen Computer abfragt und andere Überprüfungen durchführt. Anschließend wird das Runbook **Enable-AutomationSolution** aufgerufen, um „Änderungsnachverfolgung und Bestand“ für jeden virtuellen Computer in der angegebenen Ressourcengruppe zu konfigurieren.
* **Enable-AutomationSolution** : Aktiviert „Änderungsnachverfolgung und Bestand“ für mindestens einen in der Zielressourcengruppe angegebenen virtuellen Computer. Das Runbook überprüft, ob die Voraussetzungen erfüllt sind und die Log Analytics-VM-Erweiterung installiert ist, und installiert diese, sofern sie nicht gefunden wird. Darüber hinaus fügt es die virtuellen Computer zur Bereichskonfiguration im angegebenen Log Analytics-Arbeitsbereich hinzu, der mit dem Automation-Konto verknüpft ist.

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](../automation-security-overview.md) zum Verwalten von Computern.
* [Log Analytics-Arbeitsbereich](../../azure-monitor/platform/design-logs-deployment.md)
* Ein [virtueller Computer](../../virtual-machines/windows/quick-create-portal.md).
* Zwei Automation-Ressourcen, die vom Runbook **Enable-AutomationSolution** verwendet werden. Wenn dieses Runbook nicht bereits in Ihrem Automation-Konto vorhanden ist, wird es während der ersten Ausführung automatisch vom Runbook **Enable-MultipleSolution** importiert.
    * *LASolutionSubscriptionId:* ID des Abonnements, in dem sich Ihr Log Analytics-Arbeitsbereich befindet.
    * *LASolutionWorkspaceId:* Arbeitsbereichs-ID des Log Analytics-Arbeitsbereichs, der mit Ihrem Automation-Konto verknüpft ist.

    Diese Variablen werden verwendet, um den Arbeitsbereich des integrierten virtuellen Computers zu konfigurieren. Sind diese nicht angegeben, sucht das Skript zunächst nach allen virtuellen Computern, die in ihrem Abonnement in „Änderungsnachverfolgung und Bestand“ integriert sind. Anschließend sucht es nach dem Abonnement, in dem sich das Automation-Konto befindet und dann nach allen anderen Abonnements, auf die das Benutzerkonto Zugriff hat. Bei nicht ordnungsgemäßer Konfiguration führt dies unter Umständen dazu, dass Ihre Computer in einen zufällig ausgewählten Log Analytics-Arbeitsbereich integriert werden.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="enable-change-tracking-and-inventory"></a>Aktivieren der Lösung für Änderungsnachverfolgung und Bestand

1. Navigieren Sie im Azure-Portal zu **Automation-Konten** . Wählen Sie auf der Seite **Automation-Konten** Ihr Konto in der Liste aus.

1. Wählen Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** die Option **Bestand** oder **Änderungsnachverfolgung** aus.

1. Wählen Sie den Log Analytics-Arbeitsbereich aus, und klicken Sie dann auf **Aktivieren** . Während Bestand oder Änderungsnachverfolgung aktiviert wird, wird ein Banner angezeigt.

    ![Aktivieren der Lösung für Änderungsnachverfolgung und Bestand](media/enable-from-automation-account/enable-feature.png)

## <a name="install-and-update-modules"></a>Installieren und Aktualisieren von Modulen

Für ein erfolgreiches Aktivieren der Updateverwaltung für Ihre virtuellen Computer mit dem Runbook müssen Sie ein Update auf die aktuellen Azure-Module durchführen und das Modul [Az.OperationalInsights](/powershell/module/az.operationalinsights) importieren.

1. Wählen Sie in Ihrem Automation-Konto unter **Freigegebene Ressourcen** die Option **Module** aus.

2. Klicken Sie auf **Azure-Module aktualisieren** , um die Azure-Module auf die neueste Version zu aktualisieren.

3. Klicken Sie auf **Ja** , um alle vorhandenen Azure-Module auf die aktuelle Version zu aktualisieren.

    ![Aktualisieren von Modulen](media/enable-from-runbook/update-modules.png)

4. Kehren Sie unter **Freigegebene Ressourcen** zur Option **Module** zurück.

5. Wählen Sie die Option **Katalog durchsuchen** aus, um den Modulkatalog zu öffnen.

6. Suchen Sie nach `Az.OperationalInsights`, und importieren Sie dieses Modul in Ihr Automation-Konto.

    ![Importieren des OperationalInsights-Moduls](media/enable-from-runbook/import-operational-insights-module.png)

## <a name="select-azure-vm-to-manage"></a>Auswählen eines zu verwaltenden virtuellen Azure-Computers

Wenn „Änderungsnachverfolgung und Bestand“ aktiviert ist, können Sie einen virtuellen Azure-Computer hinzufügen, der durch das Feature verwaltet werden soll.

1. Wählen Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** die Option **Änderungsnachverfolgung** oder **Bestand** aus.

2. Klicken Sie auf **Azure-VMs hinzufügen** , um Ihren virtuellen Computer hinzuzufügen.

3. Wählen Sie in der Liste Ihren virtuellen Computer aus, und klicken Sie auf **Aktivieren** . Dadurch wird „Änderungsnachverfolgung und Bestand“ für den virtuellen Computer aktiviert.

   ![Aktivieren von „Änderungsnachverfolgung und Bestand“ für einen virtuellen Computer](media/enable-from-runbook/enable-azure-vm.png)

    > [!NOTE]
    > Wenn Sie versuchen, ein anderes Feature zu aktivieren, bevor die Einrichtung von „Änderungsnachverfolgung und Bestand“ abgeschlossen ist, erhalten Sie die folgende Meldung: `Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="import-a-runbook-to-enable-change-tracking-and-inventory"></a>Importieren eines Runbooks, um „Änderungsnachverfolgung und Bestand“ zu aktivieren

1. Wählen Sie in Ihrem Automation-Konto unter **Prozessautomatisierung** die Option **Runbooks** aus.

2. Klicken Sie auf **Katalog durchsuchen** .

3. Suchen Sie nach **Update- und Änderungsnachverfolgung** .

4. Wählen Sie das Runbook aus, und klicken Sie auf der Seite **Quelle anzeigen** auf **Importieren** .

5. Klicken Sie auf **OK** , um das Runbook in das Automation-Konto zu importieren.

   ![Importieren eines Runbooks für Setup](media/enable-from-runbook/import-from-gallery.png)

6. Wählen Sie auf der Seite **Runbook** das Runbook **Enable-MultipleSolution** aus, und klicken Sie dann auf **Bearbeiten** . Wählen Sie im Text-Editor die Option **Veröffentlichen** aus.

7. Wenn Sie zum Bestätigen aufgefordert werden, klicken Sie auf **Ja** , um das Runbook zu veröffentlichen.

## <a name="start-the-runbook"></a>Starten des Runbooks

„Änderungsnachverfolgung und Bestand“ muss für einen virtuellen Azure-Computer aktiviert sein, um dieses Runbook starten zu können. Hierfür sind ein vorhandener virtueller Computer und eine vorhandene Ressourcengruppe mit dem aktivierten Feature erforderlich, um mindestens einen virtuellen Computer in der Zielressourcengruppe zu konfigurieren.

1. Öffnen Sie das Runbook **Enable-MultipleSolution** .

   ![Runbook für mehrere Lösungen](media/enable-from-runbook/runbook-overview.png)

2. Klicken Sie auf die Schaltfläche „Start“, und geben Sie die Parameterwerte in die folgenden Felder ein:

   * **VMNAME** : Der Name eines vorhandenen virtuellen Computers, der „Änderungsnachverfolgung und Bestand“ hinzugefügt werden soll. Lassen Sie dieses Feld leer, um alle virtuellen Computer in der Ressourcengruppe hinzuzufügen.
   * **VMRESOURCEGROUP** : Der Name der Ressourcengruppe für die zu aktivierenden VMs.
   * **SUBSCRIPTIONID** : Die Abonnement-ID der zu aktivierenden neuen VM. Lassen Sie dieses Feld leer, um das Abonnement des Arbeitsbereichs zu verwenden. Wenn Sie eine andere Abonnement-ID verwenden, fügen Sie das ausführende Konto für das Automation-Konto als Mitwirkender für das Abonnement hinzu.
   * **ALREADYONBOARDEDVM** : Der Name der VM, die bereits manuell für Updates aktiviert ist.
   * **ALREADYONBOARDEDVMRESOURCEGROUP** : Der Name der Ressourcengruppe, der die VM angehört.
   * **SOLUTIONTYPE** : Geben Sie **ChangeTracking** ein.

   ![Parameter für das Enable-MultipleSolution-Runbook](media/enable-from-runbook/runbook-parameters.png)

3. Klicken Sie auf **OK** , um den Runbookauftrag zu starten.

4. Überwachen Sie den Status des Runbookauftrags und etwaige Fehler auf der Seite **Aufträge** .

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Planen eines Runbooks finden Sie unter [Verwalten von Zeitplänen in Azure Automation](../shared-resources/schedules.md).

* Ausführliche Informationen zur Verwendung des Features finden Sie unter [Verwalten der Änderungsnachverfolgung und des Bestands](manage-change-tracking.md) und [Verwalten der Bestandserfassung von virtuellen Computern](manage-inventory-vms.md).

* Informationen zur Behandlung allgemeiner Probleme mit dem Feature finden Sie unter [Behandeln von Problemen mit Änderungsnachverfolgung und Bestand](../troubleshoot/change-tracking.md).


