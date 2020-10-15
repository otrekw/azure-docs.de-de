---
title: Aktivieren des Features „Änderungsnachverfolgung und Bestand“ von Azure Automation über einen virtuellen Azure-Computer
description: In diesem Artikel erfahren Sie, wie Sie „Änderungsnachverfolgung und Bestand“ über einen virtuellen Azure-Computer aktivieren.
services: automation
ms.date: 03/04/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 5ff6e0ffd4040393a040dc67a511aab47887f6e1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86186264"
---
# <a name="enable-change-tracking-and-inventory-from-an-azure-vm"></a>Aktivieren von Änderungsnachverfolgung und Bestand über einen virtuellen Azure-Computer

In diesem Artikel wird beschrieben, wie Sie mit einem virtuellen Azure-Computer die Funktion [Änderungsnachverfolgung und Bestand](change-tracking.md) auf anderen Computern aktivieren können. Wenn Sie virtuelle Azure-Computer im großen Stil aktivieren möchten, müssen Sie einen vorhandenen virtuellen Computer mithilfe von „Änderungsnachverfolgung und Bestand“ aktivieren. 

> [!NOTE]
> Wenn Sie „Änderungsnachverfolgung und Bestand“ aktivieren, werden nur bestimmte Regionen für die Verknüpfung eines Log Analytics-Arbeitsbereichs und eines Automation-Kontos unterstützt. Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](how-to/region-mappings.md).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](./index.yml) zum Verwalten von Computern.
* Ein [virtueller Computer](../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="enable-change-tracking-and-inventory"></a>Aktivieren der Lösung für Änderungsnachverfolgung und Bestand

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Virtuelle Computer** aus, oder suchen Sie auf der Startseite nach **Virtuelle Computer**, und wählen Sie die entsprechende Option aus.

2. Wählen Sie den virtuellen Computer aus, für den Sie „Änderungsnachverfolgung und Bestand“ aktivieren möchten. Virtuelle Computer können sich unabhängig vom Standort Ihres Automation-Kontos in einer beliebigen Region befinden.

3. Wählen Sie auf der Seite des virtuellen Computers unter **Konfigurationsverwaltung** eine der Optionen **Bestand** oder **Änderungsnachverfolgung** aus.

4. Sie müssen über die Berechtigung `Microsoft.OperationalInsights/workspaces/read` verfügen, um zu bestimmen, ob der virtuelle Computer für einen Arbeitsbereich aktiviert ist. Informationen zu weiteren erforderlichen Berechtigungen finden Sie unter [Berechtigungen für die Featureeinrichtung](automation-role-based-access-control.md#feature-setup-permissions). Informationen zum gleichzeitigen Aktivieren mehrerer Computer finden Sie unter [Aktivieren von Änderungsnachverfolgung und Bestand über ein Automation-Konto](automation-enable-changes-from-auto-acct.md).

5. Wählen Sie den Log Analytics-Arbeitsbereich und das Automation-Konto aus, und klicken Sie auf **Aktivieren**, um „Änderungsnachverfolgung und Bestand“ für den virtuellen Computer zu aktivieren. Die Einrichtung dauert bis zu 15 Minuten. 

## <a name="next-steps"></a>Nächste Schritte

* Ausführliche Informationen zur Verwendung des Features finden Sie unter [Verwalten der Änderungsnachverfolgung und des Bestands](change-tracking-file-contents.md).
* Informationen zur Behandlung allgemeiner Probleme mit dem Feature finden Sie unter [Behandeln von Problemen mit Änderungsnachverfolgung und Bestand](troubleshoot/change-tracking.md).
