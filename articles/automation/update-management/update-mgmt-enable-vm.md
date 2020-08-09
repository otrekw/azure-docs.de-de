---
title: Aktivieren der Azure Automation-Updateverwaltung von einer Azure-VM aus
description: In diesem Artikel erfahren Sie, wie Sie die Updateverwaltung von einer Azure-VM aus aktivieren.
services: automation
ms.date: 07/28/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 27832190125840e367edbfb2db8e4134f98b192d
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2020
ms.locfileid: "87449599"
---
# <a name="enable-update-management-from-an-azure-vm"></a>Aktivieren der Updateverwaltung über einen virtuellen Azure-Computer

In diesem Artikel wird beschrieben, wie Sie mit einem virtuellen Azure-Computer die Funktion [Updateverwaltung](update-mgmt-overview.md) auf anderen Computern aktivieren können. Sie müssen eine vorhandene VM über die Updateverwaltung aktivieren, um Azure-VMs im großen Stil zu aktivieren.

> [!NOTE]
> Wenn Sie die Updateverwaltung aktivieren, werden nur bestimmte Regionen zum Verknüpfen mit einem Log Analytics-Arbeitsbereich und einem Automation-Konto unterstützt. Eine Liste der unterstützten Zuordnungspaare finden Sie unter [Regionszuordnung für Automation-Konto und Log Analytics-Arbeitsbereich](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](../index.yml) zum Verwalten von Computern.
* Ein [virtueller Computer](../../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="enable-the-feature-for-deployment"></a>Aktivieren der Funktion für die Bereitstellung

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Virtuelle Computer** aus, oder suchen Sie auf der Startseite nach **Virtuelle Computer**, und wählen Sie die entsprechende Option aus.

2. Wählen Sie den virtuellen Computer aus, für den Sie die Updateverwaltung aktivieren möchten. VMs können in jeder Region vorhanden sein, unabhängig vom Standort Ihres Automation-Kontos. Sie

3. Wählen Sie auf der Seite des virtuellen Computers unter **Vorgänge** die Option **Updateverwaltung** aus.

4. Sie müssen über die Berechtigung `Microsoft.OperationalInsights/workspaces/read` verfügen, um zu bestimmen, ob der virtuelle Computer für einen Arbeitsbereich aktiviert ist. Informationen zu weiteren erforderlichen Berechtigungen finden Sie unter [Erforderliche Berechtigungen für das Aktivieren von Computern](../automation-role-based-access-control.md#feature-setup-permissions). Informationen zum gleichzeitigen Aktivieren mehrerer Computer finden Sie unter [Aktivieren der Updateverwaltung aus einem Automation-Konto](update-mgmt-enable-automation-account.md).

5. Wählen Sie den Log Analytics-Arbeitsbereich und das Automation-Konto aus, und klicken Sie auf **Aktivieren**, um die Updateverwaltung zu aktivieren. Nach dem Aktivieren der Updateverwaltung kann es ca. 15 Minuten dauern, bis Sie die Updatebewertung der VM anzeigen können.

    ![Aktivieren der Updateverwaltung](media/update-mgmt-enable-vm/manageupdates-update-enable.png)

## <a name="next-steps"></a>Nächste Schritte

* Wie Sie die Updateverwaltung für VMs verwenden, erfahren Sie unter [Verwalten von Updates und Patches für Ihre Azure-VMs](update-mgmt-manage-updates-for-vm.md).

* Informationen zum Behandeln von Fehlern bei der Updateverwaltung finden Sie unter [Behandeln von Problemen mit der Updateverwaltung](../troubleshoot/update-management.md).
