---
title: Aktivieren des Features „Änderungsnachverfolgung und Bestand“ von Azure Automation über ein Runbook
description: In diesem Artikel erfahren Sie, wie Sie „Änderungsnachverfolgung und Bestand“ über ein Runbook aktivieren.
services: automation
ms.topic: conceptual
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: 014442dee1be23a189e22a505abf86050601b2aa
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83826741"
---
# <a name="enable-change-tracking-and-inventory-from-a-runbook"></a>Aktivieren von Änderungsnachverfolgung und Bestand über ein Runbook

In diesem Artikel erfahren Sie, wie Sie das Feature [Änderungsnachverfolgung und Bestand](change-tracking.md) mithilfe eines Runbooks für virtuelle Computer in Ihrer Umgebung aktivieren. Wenn Sie virtuelle Azure-Computer im großen Stil aktivieren möchten, müssen Sie einen vorhandenen virtuellen Computer mithilfe von „Änderungsnachverfolgung und Bestand“ aktivieren. 

> [!NOTE]
> Wenn Sie „Änderungsnachverfolgung und Bestand“ aktivieren, werden nur bestimmte Regionen für die Verknüpfung eines Log Analytics-Arbeitsbereichs und eines Automation-Kontos unterstützt. Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](how-to/region-mappings.md).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](automation-offering-get-started.md) zum Verwalten von Computern.
* Ein [virtueller Computer](../virtual-machines/windows/quick-create-portal.md).

## <a name="enable-change-tracking-and-inventory"></a>Aktivieren der Lösung für Änderungsnachverfolgung und Bestand 

1. Wählen Sie im Azure-Portal die Option **Automation-Konten** und anschließend in der Liste Ihr Automation-Konto aus.
1. Wählen Sie unter **Konfigurationsverwaltung** die Option **Bestand** aus.
1. Wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich, oder erstellen Sie einen neuen. 
1. Klicken Sie auf **Aktivieren**.

    ![Aktivieren der Lösung für Änderungsnachverfolgung und Bestand](media/automation-enable-changes-from-runbook/inventory-onboard.png)

## <a name="select-azure-vm-to-manage"></a>Auswählen eines zu verwaltenden virtuellen Azure-Computers

Wenn „Änderungsnachverfolgung und Bestand“ aktiviert ist, können Sie einen virtuellen Azure-Computer hinzufügen, der durch das Feature verwaltet werden soll.

1. Wählen Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** die Option **Änderungsnachverfolgung** oder **Bestand** aus.

2. Klicken Sie auf **Azure-VMs hinzufügen**, um Ihren virtuellen Computer hinzuzufügen.

3. Wählen Sie in der Liste Ihren virtuellen Computer aus, und klicken Sie auf **Aktivieren**. Dadurch wird „Änderungsnachverfolgung und Bestand“ für den virtuellen Computer aktiviert.

   ![Aktivieren von „Änderungsnachverfolgung und Bestand“ für einen virtuellen Computer](media/automation-enable-changes-from-runbook/enable-change-tracking.png)

    > [!NOTE]
    > Wenn Sie versuchen, ein anderes Feature zu aktivieren, bevor die Einrichtung von „Änderungsnachverfolgung und Bestand“ abgeschlossen ist, erhalten Sie die folgende Meldung: `Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="install-and-update-modules"></a>Installieren und Aktualisieren von Modulen

Führen Sie ein Update auf die neuesten Azure-Module durch, und importieren Sie das Modul [Az.OperationalInsights](https://docs.microsoft.com/powershell/module/az.operationalinsights/?view=azps-3.7.0), damit „Änderungsnachverfolgung und Bestand“ erfolgreich für Ihren virtuellen Computer aktiviert werden kann.

1. Wählen Sie in Ihrem Automation-Konto unter **Freigegebene Ressourcen** die Option **Module** aus. 
2. Klicken Sie auf **Azure-Module aktualisieren**, um die Azure-Module auf die neueste Version zu aktualisieren. 
3. Klicken Sie auf **Ja**, um alle vorhandenen Azure-Module auf die aktuelle Version zu aktualisieren.

    ![Aktualisieren von Modulen](media/automation-enable-changes-from-runbook/update-modules.png)

4. Kehren Sie unter **Freigegebene Ressourcen** zur Option **Module** zurück. 
5. Wählen Sie die Option **Katalog durchsuchen** aus, um den Modulkatalog zu öffnen. 
6. Suchen Sie nach „Az.OperationalInsights“, und importieren Sie dieses Modul in das Automation-Konto.

    ![Importieren des OperationalInsights-Moduls](media/automation-enable-changes-from-runbook/import-operational-insights-module.png)

## <a name="import-a-runbook-to-enable-change-tracking-and-inventory"></a>Importieren eines Runbooks, um „Änderungsnachverfolgung und Bestand“ zu aktivieren

1. Wählen Sie in Ihrem Automation-Konto unter **Prozessautomatisierung** die Option **Runbooks** aus.
2. Klicken Sie auf **Katalog durchsuchen**.
3. Suchen Sie nach `update and change tracking`.
4. Wählen Sie das Runbook aus, und klicken Sie auf der Seite „Quelle anzeigen“ auf **Importieren**. 
5. Klicken Sie auf **OK**, um das Runbook in das Automation-Konto zu importieren.

   ![Importieren eines Runbooks für die Einrichtung](media/automation-enable-changes-from-runbook/import-from-gallery.png)

6. Klicken Sie auf der Seite „Runbook“ auf **Bearbeiten**, und wählen Sie dann **Veröffentlichen** aus. 
7. Klicken Sie im Bereich „Runbook veröffentlichen“ auf **Ja**, um das Runbook zu veröffentlichen.

## <a name="start-the-runbook"></a>Starten des Runbooks

„Änderungsnachverfolgung und Bestand“ muss für einen virtuellen Azure-Computer aktiviert sein, um dieses Runbook starten zu können. Für die Parameter werden ein vorhandener virtueller Computer und eine Ressourcengruppe mit dem aktivierten Feature benötigt.

1. Öffnen Sie das Runbook **Enable-MultipleSolution**.

   ![Runbooks für mehrere Lösungen](media/automation-enable-changes-from-runbook/runbook-overview.png)

1. Klicken Sie auf die Schaltfläche „Start“, und geben Sie Parameterwerte in die folgenden Felder ein:

   * **VMNAME**: Der Name eines vorhandenen virtuellen Computers, der „Änderungsnachverfolgung und Bestand“ hinzugefügt werden soll. Lassen Sie dieses Feld leer, um alle virtuellen Computer in der Ressourcengruppe hinzuzufügen.
   * **VMRESOURCEGROUP**: Der Name der Ressourcengruppe für die zu aktivierenden virtuellen Computer.
   * **SUBSCRIPTIONID**: Die Abonnement-ID des neuen zu aktivierenden virtuellen Computers. Lassen Sie dieses Feld leer, um das Abonnement des Arbeitsbereichs zu verwenden. Wenn Sie eine andere Abonnement-ID verwenden, fügen Sie das ausführende Konto für das Automation-Konto als Mitwirkender für das Abonnement hinzu.
   * **ALREADYONBOARDEDVM**: Der Name des virtuellen Computers, die bereits manuell für Änderungen aktiviert ist.
   * **ALREADYONBOARDEDVMRESOURCEGROUP**: Der Name der Ressourcengruppe, der die VM angehört.
   * **SOLUTIONTYPE**: Geben Sie **ChangeTracking** ein.

   ![Parameter für das Enable-MultipleSolution-Runbook](media/automation-enable-changes-from-runbook/runbook-parameters.png)

1. Klicken Sie auf **OK**, um den Runbookauftrag zu starten.
1. Überwachen Sie den Status und etwaige Fehler auf der Seite für den Runbookauftrag.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Planen eines Runbooks finden Sie unter [Verwalten von Zeitplänen in Azure Automation](shared-resources/schedules.md).
* Ausführliche Informationen zur Verwendung des Features finden Sie unter [Verwalten der Änderungsnachverfolgung und des Bestands](change-tracking-file-contents.md).
* Informationen zu Bereichskonfigurationen finden Sie unter [Einschränken des Bereitstellungsumfangs für „Änderungsnachverfolgung und Bestand“](automation-scope-configurations-change-tracking.md).
* Unter [Ermitteln der auf Ihren VMs installierten Software](automation-tutorial-installed-software.md) erfahren Sie, wie Sie mithilfe des Features in Ihrer Umgebung installierte Software identifizieren können.
* Falls Sie Ihr Automation-Konto beim Aktivieren des Features nicht in einen Log Analytics-Arbeitsbereich integrieren möchten, lesen Sie [Aufheben der Verknüpfung eines Arbeitsbereichs über ein Automation-Konto](automation-unlink-workspace-change-tracking.md).
* Nach Abschluss der Änderungsbereitstellung für virtuelle Computer können Sie sie wie unter [Entfernen von VMs aus Änderungsnachverfolgung und Bestand](automation-remove-vms-from-change-tracking.md) beschrieben entfernen.
* Informationen zur Behandlung allgemeiner Probleme mit dem Feature finden Sie unter [Behandeln von Problemen mit Änderungsnachverfolgung und Bestand](troubleshoot/change-tracking.md).