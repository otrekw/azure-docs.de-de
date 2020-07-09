---
title: Aktivieren der Azure Automation-Updateverwaltung über ein Runbook
description: In diesem Artikel erfahren Sie, wie Sie die Updateverwaltung über ein Runbook aktivieren.
services: automation
ms.topic: conceptual
ms.date: 05/10/2018
ms.custom: mvc
ms.openlocfilehash: d55a47e4eb921c71ff3539d34b6db514684da073
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86186128"
---
# <a name="enable-update-management-from-a-runbook"></a>Aktivieren der Updateverwaltung über ein Runbook

In diesem Artikel wird beschrieben, wie Sie mit einem Runbook das Feature [Updateverwaltung](automation-update-management.md) für VMs in Ihrer Umgebung aktivieren können. Sie müssen eine vorhandene VM über die Updateverwaltung aktivieren, um Azure VMs im großen Stil zu aktivieren. 

> [!NOTE]
> Wenn Sie die Updateverwaltung aktivieren, werden nur bestimmte Regionen zum Verknüpfen mit einem Log Analytics-Arbeitsbereich und einem Automation-Konto unterstützt. Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](how-to/region-mappings.md).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](./index.yml) zum Verwalten von Computern.
* Ein [virtueller Computer](../virtual-machines/windows/quick-create-portal.md).

## <a name="enable-update-management"></a>Aktivieren der Updateverwaltung

1. Wählen Sie in Ihrem Automation-Konto unter **Updateverwaltung** die Option **Updateverwaltung** aus.

2. Wählen Sie den Log Analytics-Arbeitsbereich aus, und klicken Sie dann auf **Aktivieren**. Während die Updateverwaltung aktiviert ist, wird ein blaues Banner angezeigt. 

    ![Aktivieren der Updateverwaltung](media/automation-onboard-solutions/update-onboard.png)

## <a name="select-azure-vm-to-manage"></a>Auswählen von Azure VM zur Verwaltung

Wenn die Updateverwaltung aktiviert ist, können Sie eine Azure-VM hinzufügen, um Updates zu erhalten.

1. Wählen Sie in Ihrem Automation-Konto unter **Updateverwaltung** die Option **Updateverwaltung** aus.

2. Wählen Sie **Azure-VMs hinzufügen**, um Ihren virtuellen Computer hinzuzufügen

3. Wählen Sie die VM aus der Liste aus, und klicken Sie auf  **Aktivieren**, um die VM für Updates einzurichten. 

   ![Aktivieren der Updateverwaltung für eine VM](media/automation-onboard-solutions/enable-update.png)

    > [!NOTE]
    > Wenn Sie versuchen, ein anderes Feature zu aktivieren, bevor die Einrichtung der Updateverwaltung abgeschlossen ist, erhalten Sie diese Meldung: `Installation of another solution is in progress on this or a different virtual machine. When that installation completes the Enable button is enabled, and you can request installation of the solution on this virtual machine.`

## <a name="install-and-update-modules"></a>Installieren und Aktualisieren von Modulen

Für ein erfolgreiches Aktivieren der Updateverwaltung für Ihre VMs müssen Sie das Update auf die aktuellen Azure-Module durchführen und das Modul [Az.OperationalInsights](/powershell/module/az.operationalinsights/?view=azps-3.7.0) importieren.

1. Wählen Sie in Ihrem Automation-Konto unter **Freigegebene Ressourcen** die Option **Module** aus. 
2. Klicken Sie auf **Azure-Module aktualisieren**, um die Azure-Module auf die neueste Version zu aktualisieren. 
3. Klicken Sie auf **Ja**, um alle vorhandenen Azure-Module auf die aktuelle Version zu aktualisieren.

    ![Aktualisieren von Modulen](media/automation-onboard-solutions/update-modules.png)

4. Kehren Sie unter **Freigegebene Ressourcen** zur Option **Module** zurück. 
5. Wählen Sie die Option **Katalog durchsuchen** aus, um den Modulkatalog zu öffnen. 
6. Suchen Sie nach `Az.OperationalInsights`, und importieren Sie dieses Modul in Ihr Automation-Konto.

    ![Importieren des OperationalInsights-Moduls](media/automation-onboard-solutions/import-operational-insights-module.png)

## <a name="import-a-runbook-to-enable-update-management"></a>Importieren eines Runbooks zum Aktivieren der Updateverwaltung

1. Wählen Sie in Ihrem Automation-Konto unter **Prozessautomatisierung** die Option **Runbooks** aus.
2. Klicken Sie auf **Katalog durchsuchen**.
3. Suchen Sie nach `update and change tracking`.
4. Wählen Sie das Runbook aus, und klicken Sie auf der Seite „Quelle anzeigen“ auf **Importieren**. 
5. Klicken Sie auf **OK**, um das Runbook in das Automation-Konto zu importieren.

   ![Importieren eines Runbooks für Setup](media/automation-onboard-solutions/import-from-gallery.png)

6. Klicken Sie auf der Seite „Runbook“ auf **Bearbeiten**, und wählen Sie dann **Veröffentlichen** aus. 
7. Klicken Sie im Bereich „Runbook veröffentlichen“ auf **Ja**, um das Runbook zu veröffentlichen.

## <a name="start-the-runbook"></a>Starten des Runbooks

Sie müssen die Updateverwaltung für eine Azure-VM aktiviert haben, um dieses Runbook zu starten. Es erfordert eine vorhandene VM und Ressourcengruppe mit dem für Parameter aktivierten Feature.

1. Öffnen Sie das Runbook **Enable-MultipleSolution**.

   ![Runbook für mehrere Lösungen](media/automation-onboard-solutions/runbook-overview.png)

2. Klicken Sie auf die Schaltfläche „Start“, und geben Sie die Parameterwerte in die folgenden Felder ein:

   * **VMNAME**: Der Name einer vorhandenen VM, die der Updateverwaltung hinzugefügt werden soll. Lassen Sie dieses Feld leer, um alle virtuellen Computer in der Ressourcengruppe hinzuzufügen.
   * **VMRESOURCEGROUP**: Der Name der Ressourcengruppe für die zu aktivierenden VMs.
   * **SUBSCRIPTIONID**: Die Abonnement-ID der zu aktivierenden neuen VM. Lassen Sie dieses Feld leer, um das Abonnement des Arbeitsbereichs zu verwenden. Wenn Sie eine andere Abonnement-ID verwenden, fügen Sie das ausführende Konto für das Automation-Konto als Mitwirkender für das Abonnement hinzu.
   * **ALREADYONBOARDEDVM**: Der Name der VM, die bereits manuell für Updates aktiviert ist.
   * **ALREADYONBOARDEDVMRESOURCEGROUP**: Der Name der Ressourcengruppe, der die VM angehört.
   * **SOLUTIONTYPE**: Geben Sie **Updates** ein.

   ![Parameter für das Enable-MultipleSolution-Runbook](media/automation-onboard-solutions/runbook-parameters.png)

3. Klicken Sie auf **OK**, um den Runbookauftrag zu starten.
4. Überwachen Sie den Status und etwaige Fehler auf der Seite für den Runbookauftrag.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Planen eines Runbooks finden Sie unter [Verwalten von Zeitplänen in Azure Automation](shared-resources/schedules.md).
* Wie Sie die Updateverwaltung für VMs verwenden, erfahren Sie unter [Verwalten von Updates und Patches für Ihre Azure-VMs](automation-tutorial-update-management.md).
* Informationen zum Behandeln von Fehlern bei der Updateverwaltung finden Sie unter [Behandeln von Problemen mit der Updateverwaltung](troubleshoot/update-management.md).
* Informationen zum Behandeln von Problemen mit dem Windows Update-Agent finden Sie unter [Beheben von Problemen mit dem Windows Update-Agent](troubleshoot/update-agent-issues.md).
* Informationen zum Behandeln von Problemen mit dem Linux-Update-Agent finden Sie unter [Beheben von Problemen mit dem Linux-Update-Agent](troubleshoot/update-agent-issues-linux.md).
