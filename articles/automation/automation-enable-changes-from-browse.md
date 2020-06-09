---
title: Aktivieren des Features „Änderungsnachverfolgung und Bestand“ von Azure Automation über das Azure-Portal
description: In diesem Artikel erfahren Sie, wie Sie das Feature „Änderungsnachverfolgung und Bestand“ über das Azure-Portal aktivieren.
services: automation
ms.date: 04/11/2019
ms.topic: article
ms.custom: mvc
ms.openlocfilehash: 0491f10f22e35c81e5bc35d537546001f136be50
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83826824"
---
# <a name="enable-change-tracking-and-inventory-from-azure-portal"></a>Aktivieren von „Änderungsnachverfolgung und Bestand“ über das Azure-Portal

In diesem Artikel erfahren Sie, wie Sie das Feature [Änderungsnachverfolgung und Bestand](change-tracking.md) für virtuelle Computer über das Azure-Portal aktivieren. Wenn Sie virtuelle Azure-Computer im großen Stil aktivieren möchten, müssen Sie einen vorhandenen virtuellen Computer mithilfe von „Änderungsnachverfolgung und Bestand“ aktivieren. 

Die Anzahl von Ressourcengruppen, die für die Verwaltung Ihrer virtuellen Computer verwendet werden können, ist durch [Resource Manager-Bereitstellungsgrenzwerte](../azure-resource-manager/templates/cross-resource-group-deployment.md) eingeschränkt. Resource Manager-Bereitstellungen (nicht zu verwechseln mit Updatebereitstellungen) sind auf fünf Ressourcengruppen pro Bereitstellung beschränkt. Zwei dieser Ressourcengruppen sind für die Konfiguration des Log Analytics-Arbeitsbereichs, des Automation-Kontos und der zugehörigen Ressourcen reserviert. Somit können noch drei Ressourcengruppen für die Verwaltung durch „Änderungsnachverfolgung und Bestand“ ausgewählt werden. Dieser Grenzwert gilt nur für die gleichzeitige Einrichtung, nicht für die Anzahl von Ressourcengruppen, die durch ein Automation-Feature verwaltet werden können.

> [!NOTE]
> Wenn Sie „Änderungsnachverfolgung und Bestand“ aktivieren, werden nur bestimmte Regionen für die Verknüpfung eines Log Analytics-Arbeitsbereichs und eines Automation-Kontos unterstützt. Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](how-to/region-mappings.md).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](automation-offering-get-started.md) zum Verwalten von Computern.
* Ein [virtueller Computer](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com bei Azure an.

## <a name="enable-change-tracking-and-inventory"></a>Aktivieren der Lösung für Änderungsnachverfolgung und Bestand

1. Navigieren Sie im Azure-Portal zu **Virtuelle Computer**.

2. Wählen Sie mithilfe der Kontrollkästchen die virtuellen Computer aus, die „Änderungsnachverfolgung und Bestand“ hinzugefügt werden sollen. Es können gleichzeitig Computer für bis zu drei verschiedene Ressourcengruppen hinzugefügt werden. Virtuelle Azure-Computer können sich unabhängig vom Standort Ihres Automation-Kontos in einer beliebigen Region befinden.

    ![Liste der VMs](media/automation-enable-changes-from-browse/vmlist.png)

    > [!TIP]
    > Verwenden Sie die Filtersteuerelemente, um virtuelle Computer verschiedener Abonnements, Standorte und Ressourcengruppen auszuwählen. Durch Klicken auf das obere Kontrollkästchen können Sie alle virtuellen Computer in einer Liste auswählen.

3. Wählen Sie unter **Konfigurationsverwaltung** die Option **Änderungsnachverfolgung** oder **Bestand** aus.

4. Die Liste der virtuellen Computer wird gefiltert, um nur die virtuellen Computer anzuzeigen, die sich in demselben Abonnement und Speicherort befinden. Wenn sich Ihr virtueller Computer in mehr als drei Ressourcengruppen befindet, werden die ersten drei Ressourcengruppen ausgewählt.

5. Standardmäßig sind ein vorhandener Log Analytics-Arbeitsbereich und ein neues Automation-Konto ausgewählt. Falls Sie einen anderen Log Analytics-Arbeitsbereich und ein anderes Automation-Konto verwenden möchten, klicken Sie auf **BENUTZERDEFINIERT**, um sie auf der Seite für die benutzerdefinierte Konfiguration auszuwählen. Wenn Sie einen Log Analytics-Arbeitsbereich auswählen, wird überprüft, ob eine Verknüpfung mit einem Automation-Konto besteht. Sollte ein verknüpftes Automation-Konto gefunden werden, wird der folgende Bildschirm angezeigt. Klicken Sie auf **OK**, wenn Sie fertig sind.

    ![Auswählen von Arbeitsbereich und Konto](media/automation-enable-changes-from-browse/selectworkspaceandaccount.png)

6. Ist der ausgewählte Arbeitsbereich nicht mit einem Automation-Konto verknüpft, wird der folgende Bildschirm angezeigt. Wählen Sie ein Automation-Konto aus, und klicken Sie anschließend auf **OK**.

    ![Kein Arbeitsbereich](media/automation-enable-changes-from-browse/no-workspace.png)

7. Deaktivieren Sie das Kontrollkästchen neben jedem virtuellen Computern, den Sie nicht aktivieren möchten. Die Auswahl für virtuelle Computer, die nicht aktiviert werden können, ist bereits aufgehoben.

8. Klicken Sie auf **Aktivieren**, um das ausgewählte Feature zu aktivieren. Die Einrichtung dauert bis zu 15 Minuten.

## <a name="next-steps"></a>Nächste Schritte

* Ausführliche Informationen zur Verwendung des Features finden Sie unter [Verwalten der Änderungsnachverfolgung und des Bestands](change-tracking-file-contents.md).
* Informationen zu Bereichskonfigurationen finden Sie unter [Einschränken des Bereitstellungsumfangs für „Änderungsnachverfolgung und Bestand“](automation-scope-configurations-change-tracking.md).
* Unter [Ermitteln der auf Ihren VMs installierten Software](automation-tutorial-installed-software.md) erfahren Sie, wie Sie mithilfe des Features in Ihrer Umgebung installierte Software identifizieren können.
* Falls Sie Ihr Automation-Konto beim Aktivieren des Features nicht in einen Log Analytics-Arbeitsbereich integrieren möchten, lesen Sie [Aufheben der Verknüpfung eines Arbeitsbereichs über ein Automation-Konto](automation-unlink-workspace-change-tracking.md).
* Nach Abschluss der Änderungsbereitstellung für virtuelle Computer können Sie sie wie unter [Entfernen von VMs aus Änderungsnachverfolgung und Bestand](automation-remove-vms-from-change-tracking.md) beschrieben entfernen.
* Informationen zur Behandlung allgemeiner Probleme mit dem Feature finden Sie unter [Behandeln von Problemen mit Änderungsnachverfolgung und Bestand](troubleshoot/change-tracking.md).