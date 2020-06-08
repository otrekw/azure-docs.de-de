---
title: Aktivieren des Features „Änderungsnachverfolgung und Bestand“ von Azure Automation über einen virtuellen Azure-Computer
description: In diesem Artikel erfahren Sie, wie Sie „Änderungsnachverfolgung und Bestand“ über einen virtuellen Azure-Computer aktivieren.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 5379f2c46bbeaba4ee8509603b7b739b75d08f04
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836786"
---
# <a name="enable-change-tracking-and-inventory-from-an-azure-vm"></a>Aktivieren von Änderungsnachverfolgung und Bestand über einen virtuellen Azure-Computer

In diesem Artikel wird beschrieben, wie Sie mit einem virtuellen Azure-Computer die Funktion [Änderungsnachverfolgung und Bestand](change-tracking.md) auf anderen Computern aktivieren können. Wenn Sie virtuelle Azure-Computer im großen Stil aktivieren möchten, müssen Sie einen vorhandenen virtuellen Computer mithilfe von „Änderungsnachverfolgung und Bestand“ aktivieren. 

> [!NOTE]
> Wenn Sie „Änderungsnachverfolgung und Bestand“ aktivieren, werden nur bestimmte Regionen für die Verknüpfung eines Log Analytics-Arbeitsbereichs und eines Automation-Kontos unterstützt. Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](how-to/region-mappings.md).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](automation-offering-get-started.md) zum Verwalten von Computern.
* Ein [virtueller Computer](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="enable-change-tracking-and-inventory"></a>Aktivieren der Lösung für Änderungsnachverfolgung und Bestand

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Virtuelle Computer** aus, oder suchen Sie auf der Startseite nach **Virtuelle Computer**, und wählen Sie die entsprechende Option aus.

2. Wählen Sie den virtuellen Computer aus, für den Sie „Änderungsnachverfolgung und Bestand“ aktivieren möchten. Virtuelle Computer können sich unabhängig vom Standort Ihres Automation-Kontos in einer beliebigen Region befinden.

3. Wählen Sie auf der Seite des virtuellen Computers unter **Konfigurationsverwaltung** eine der Optionen **Bestand** oder **Änderungsnachverfolgung** aus.

4. Sie müssen über die Berechtigung `Microsoft.OperationalInsights/workspaces/read` verfügen, um zu bestimmen, ob der virtuelle Computer für einen Arbeitsbereich aktiviert ist. Informationen zu weiteren erforderlichen Berechtigungen finden Sie unter [Berechtigungen für die Featureeinrichtung](automation-role-based-access-control.md#feature-setup-permissions). Informationen zum gleichzeitigen Aktivieren mehrerer Computer finden Sie unter [Aktivieren von Änderungsnachverfolgung und Bestand über ein Automation-Konto](automation-enable-changes-from-auto-acct.md).

5. Wählen Sie den Log Analytics-Arbeitsbereich und das Automation-Konto aus, und klicken Sie auf **Aktivieren**, um „Änderungsnachverfolgung und Bestand“ für den virtuellen Computer zu aktivieren. Die Einrichtung dauert bis zu 15 Minuten. 

## <a name="check-the-scope-configuration"></a><a name="scope-configuration">Überprüfen der Bereichskonfiguration</a>

Von „Änderungsnachverfolgung und Bestand“ wird eine Bereichskonfiguration innerhalb des Arbeitsbereichs verwendet, um die Computer anzugeben, für die das Feature aktiviert werden soll. Die Bereichskonfiguration besteht aus einer Gruppe von mindestens einem gespeicherten Suchvorgang, der zum Begrenzen des Bereichs der Funktion auf bestimmte Computer verwendet wird. Weitere Informationen finden Sie unter [Einschränken des Bereitstellungsumfangs für „Änderungsnachverfolgung und Bestand“](automation-scope-configurations-change-tracking.md).

## <a name="next-steps"></a>Nächste Schritte

* Ausführliche Informationen zur Verwendung des Features finden Sie unter [Verwalten der Änderungsnachverfolgung und des Bestands](change-tracking-file-contents.md).
* Informationen zu Bereichskonfigurationen finden Sie unter [Einschränken des Bereitstellungsumfangs für „Änderungsnachverfolgung und Bestand“](automation-scope-configurations-change-tracking.md).
* Unter [Ermitteln der auf Ihren VMs installierten Software](automation-tutorial-installed-software.md) erfahren Sie, wie Sie mithilfe des Features in Ihrer Umgebung installierte Software identifizieren können.
* Falls Sie Ihr Automation-Konto beim Aktivieren des Features nicht in einen Log Analytics-Arbeitsbereich integrieren möchten, lesen Sie [Aufheben der Verknüpfung eines Arbeitsbereichs über ein Automation-Konto](automation-unlink-workspace-change-tracking.md).
* Nach Abschluss der Änderungsbereitstellung für virtuelle Computer können Sie sie wie unter [Entfernen von VMs aus Änderungsnachverfolgung und Bestand](automation-remove-vms-from-change-tracking.md) beschrieben entfernen.
* Informationen zur Behandlung allgemeiner Probleme mit dem Feature finden Sie unter [Behandeln von Problemen mit Änderungsnachverfolgung und Bestand](troubleshoot/change-tracking.md).
