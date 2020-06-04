---
title: Aktivieren der Azure Automation-Updateverwaltung von einer Azure-VM aus
description: In diesem Artikel erfahren Sie, wie Sie die Updateverwaltung von einer Azure-VM aus aktivieren.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: cab114e50852f293a3d1caf5bdc9a341f75f2557
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83743964"
---
# <a name="enable-update-management-from-an-azure-vm"></a>Aktivieren der Updateverwaltung über einen virtuellen Azure-Computer

In diesem Artikel wird beschrieben, wie Sie mit einem virtuellen Azure-Computer die Funktion [Updateverwaltung](automation-update-management.md) auf anderen Computern aktivieren können. Sie müssen eine vorhandene VM über die Updateverwaltung aktivieren, um Azure-VMs im großen Stil zu aktivieren. 

> [!NOTE]
> Wenn Sie die Updateverwaltung aktivieren, werden nur bestimmte Regionen zum Verknüpfen mit einem Log Analytics-Arbeitsbereich und einem Automation-Konto unterstützt. Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](how-to/region-mappings.md).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](automation-offering-get-started.md) zum Verwalten von Computern.
* Ein [virtueller Computer](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="enable-update-management"></a>Aktivieren der Updateverwaltung

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Virtuelle Computer** aus, oder suchen Sie auf der Startseite nach **Virtuelle Computer**, und wählen Sie die entsprechende Option aus.

2. Wählen Sie den virtuellen Computer aus, für den Sie die Updateverwaltung aktivieren möchten. VMs können in jeder Region vorhanden sein, unabhängig vom Standort Ihres Automation-Kontos. Sie

3. Wählen Sie auf der Seite des virtuellen Computers unter **Vorgänge** die Option **Updateverwaltung** aus.

4. Sie müssen über die Berechtigung `Microsoft.OperationalInsights/workspaces/read` verfügen, um zu bestimmen, ob der virtuelle Computer für einen Arbeitsbereich aktiviert ist. Informationen zu weiteren erforderlichen Berechtigungen finden Sie unter [Erforderliche Berechtigungen für das Aktivieren von Computern](automation-role-based-access-control.md#feature-setup-permissions). Informationen zum gleichzeitigen Aktivieren mehrerer Computer finden Sie unter [Aktivieren der Updateverwaltung aus einem Automation-Konto](automation-onboard-solutions-from-automation-account.md).

5. Wählen Sie den Log Analytics-Arbeitsbereich und das Automation-Konto aus, und klicken Sie auf **Aktivieren**, um die Updateverwaltung zu aktivieren. Die Einrichtung dauert bis zu 15 Minuten. 

    ![Aktivieren der Updateverwaltung](media/automation-tutorial-update-management/manageupdates-update-enable.png)

## <a name="check-the-scope-configuration"></a><a name="scope-configuration"></a>Überprüfen der Bereichskonfiguration

Die Updateverwaltung verwendet eine Bereichskonfiguration innerhalb des Arbeitsbereichs, um die Computer zu erreichen, die für die Funktion aktiviert werden sollen. Die Bereichskonfiguration besteht aus einer Gruppe von mindestens einem gespeicherten Suchvorgang, der zum Begrenzen des Bereichs der Funktion auf bestimmte Computer verwendet wird. Weitere Informationen finden Sie unter [Arbeiten mit Bereichskonfigurationen für die Updateverwaltung](automation-scope-configurations-update-management.md).

## <a name="next-steps"></a>Nächste Schritte

* Wie Sie die Updateverwaltung für VMs verwenden, erfahren Sie unter [Verwalten von Updates und Patches für Ihre Azure-VMs](automation-tutorial-update-management.md).
* Informationen zu Bereichskonfigurationen finden Sie unter [Arbeiten mit Bereichskonfigurationen für die Updateverwaltung](automation-scope-configurations-update-management.md).
* Wenn Sie den Log Analytics-Arbeitsbereich nicht mehr benötigen, lesen Sie die Informationen unter [Aufheben der Verknüpfung eines Arbeitsbereichs über ein Automation-Konto für die Updateverwaltung](automation-unlink-workspace-update-management.md).
* Informationen zum Löschen von VMs aus der Updateverwaltung finden Sie unter [Entfernen virtueller Computer aus der Updateverwaltung](automation-remove-vms-from-update-management.md).
* Informationen zum Behandeln von Fehlern bei der Updateverwaltung finden Sie unter [Behandeln von Problemen mit der Updateverwaltung](troubleshoot/update-management.md).
* Informationen zum Behandeln von Problemen mit dem Windows Update-Agent finden Sie unter [Beheben von Problemen mit dem Windows Update-Agent](troubleshoot/update-agent-issues.md).
* Informationen zum Behandeln von Problemen mit dem Linux Update-Agent finden Sie unter [Beheben von Problemen mit dem Linux-Update-Agent](troubleshoot/update-agent-issues-linux.md).
