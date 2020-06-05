---
title: Aktivieren der Azure Automation-Updateverwaltung über das Azure-Portal
description: In diesem Artikel erfahren Sie, wie Sie die Updateverwaltung über das Azure-Portal aktivieren.
services: automation
ms.date: 04/11/2019
ms.topic: article
ms.custom: mvc
ms.openlocfilehash: cb3bbf1a7c2e55d152d26c475369f9ccb6fb7d1e
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83743984"
---
# <a name="enable-update-management-from-azure-portal"></a>Aktivieren der Updateverwaltung über das Azure-Portal

In diesem Artikel erfahren Sie, wie Sie die Funktion [Updateverwaltung](automation-update-management.md) für virtuelle Computer über das Azure-Portal aktivieren können. Sie müssen eine vorhandene VM über die Updateverwaltung aktivieren, um Azure-VMs im großen Stil zu aktivieren. 

Die Anzahl von Ressourcengruppen, die für die Verwaltung Ihrer virtuellen Computer verwendet werden können, ist durch [Resource Manager-Bereitstellungsgrenzwerte](../azure-resource-manager/templates/cross-resource-group-deployment.md) eingeschränkt. Resource Manager-Bereitstellungen (nicht zu verwechseln mit Updatebereitstellungen) sind auf fünf Ressourcengruppen pro Bereitstellung beschränkt. Zwei dieser Ressourcengruppen sind für die Konfiguration des Log Analytics-Arbeitsbereichs, des Automation-Kontos und der zugehörigen Ressourcen reserviert. Somit können noch drei Ressourcengruppen für die Verwaltung durch die Updateverwaltung ausgewählt werden. Dieser Grenzwert gilt nur für die gleichzeitige Einrichtung, nicht für die Anzahl von Ressourcengruppen, die durch ein Automation-Feature verwaltet werden können.

> [!NOTE]
> Wenn Sie die Updateverwaltung aktivieren, werden nur bestimmte Regionen zum Verknüpfen mit einem Log Analytics-Arbeitsbereich und einem Automation-Konto unterstützt. Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](how-to/region-mappings.md).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](automation-offering-get-started.md) zum Verwalten von Computern.
* Ein [virtueller Computer](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com bei Azure an.

## <a name="enable-update-management"></a>Aktivieren der Updateverwaltung

1. Navigieren Sie im Azure-Portal zu **Virtuelle Computer**.

2. Wählen Sie mithilfe der Kontrollkästchen die virtuellen Computer aus, die zur Updateverwaltung hinzugefügt werden sollen. Es können gleichzeitig Computer für bis zu drei verschiedene Ressourcengruppen hinzugefügt werden. Virtuelle Azure-Computer können sich unabhängig vom Standort Ihres Automation-Kontos in einer beliebigen Region befinden.

    ![Liste der VMs](media/automation-onboard-solutions-from-browse/vmlist.png)

    > [!TIP]
    > Verwenden Sie die Filtersteuerelemente, um virtuelle Computer verschiedener Abonnements, Standorte und Ressourcengruppen auszuwählen. Durch Klicken auf das obere Kontrollkästchen können Sie alle virtuellen Computer in einer Liste auswählen.

    ![Aktivieren der Updateverwaltung](media/automation-onboard-solutions-from-browse/onboardsolutions.png)

3. Klicken Sie auf **Dienste**, und wählen Sie für die Funktion „Updateverwaltung“ **Updateverwaltung** aus. 

4. Die Liste der virtuellen Computer wird gefiltert, um nur die virtuellen Computer anzuzeigen, die sich in demselben Abonnement und Speicherort befinden. Wenn sich Ihr virtueller Computer in mehr als drei Ressourcengruppen befindet, werden die ersten drei Ressourcengruppen ausgewählt.

5. Standardmäßig sind ein vorhandener Log Analytics-Arbeitsbereich und ein neues Automation-Konto ausgewählt. Falls Sie einen anderen Log Analytics-Arbeitsbereich und ein anderes Automation-Konto verwenden möchten, klicken Sie auf **BENUTZERDEFINIERT**, um sie auf der Seite für die benutzerdefinierte Konfiguration auszuwählen. Wenn Sie einen Log Analytics-Arbeitsbereich auswählen, wird überprüft, ob eine Verknüpfung mit einem Automation-Konto besteht. Sollte ein verknüpftes Automation-Konto gefunden werden, wird der folgende Bildschirm angezeigt. Klicken Sie auf **OK**, wenn Sie fertig sind.

    ![Auswählen von Arbeitsbereich und Konto](media/automation-onboard-solutions-from-browse/selectworkspaceandaccount.png)

6. Wenn der ausgewählte Arbeitsbereich nicht mit einem Automation-Konto verknüpft ist, wird der folgende Bildschirm angezeigt. Wählen Sie ein Automation-Konto aus, und klicken Sie anschließend auf **OK**.

    ![Kein Arbeitsbereich](media/automation-onboard-solutions-from-browse/no-workspace.png)

7. Deaktivieren Sie das Kontrollkästchen neben jedem virtuellen Computern, den Sie nicht aktivieren möchten. Die Auswahl für virtuelle Computer, die nicht aktiviert werden können, ist bereits aufgehoben.

8. Klicken Sie auf **Aktivieren**, um das ausgewählte Feature zu aktivieren. Die Einrichtung dauert bis zu 15 Minuten.

## <a name="next-steps"></a>Nächste Schritte

* Wie Sie die Updateverwaltung für VMs verwenden, erfahren Sie unter [Verwalten von Updates und Patches für Ihre Azure-VMs](automation-tutorial-update-management.md).
* Informationen zu Bereichskonfigurationen finden Sie unter [Arbeiten mit Bereichskonfigurationen für die Updateverwaltung](automation-scope-configurations-update-management.md).
* Wenn Sie den Log Analytics-Arbeitsbereich nicht mehr benötigen, lesen Sie die Informationen unter [Aufheben der Verknüpfung eines Arbeitsbereichs über ein Automation-Konto für die Updateverwaltung](automation-unlink-workspace-update-management.md).
* Informationen zum Löschen von VMs aus der Updateverwaltung finden Sie unter [Entfernen virtueller Computer aus der Updateverwaltung](automation-remove-vms-from-update-management.md).
* Informationen zum Behandeln von Fehlern bei der Updateverwaltung finden Sie unter [Behandeln von Problemen mit der Updateverwaltung](troubleshoot/update-management.md).
* Informationen zum Behandeln von Problemen mit dem Windows Update-Agent finden Sie unter [Beheben von Problemen mit dem Windows Update-Agent](troubleshoot/update-agent-issues.md).
* Informationen zum Behandeln von Problemen mit dem Linux Update-Agent finden Sie unter [Beheben von Problemen mit dem Linux-Update-Agent](troubleshoot/update-agent-issues-linux.md).
