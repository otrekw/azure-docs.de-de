---
title: Aktivieren der automatischen Verwaltung für virtuelle Computer über Azure Policy
description: Erfahren Sie, wie Sie die automatische Azure-Verwaltung für VMs durch eine integrierte Azure Policy im Azure-Portal aktivieren können.
author: ju-shim
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: how-to
ms.date: 09/04/2020
ms.author: jushiman
ms.openlocfilehash: 8846efa3619cec383809cdbd6efe70e3622fa007
ms.sourcegitcommit: dddd1596fa368f68861856849fbbbb9ea55cb4c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107365194"
---
# <a name="enable-automanage-for-virtual-machines-through-azure-policy"></a>Aktivieren der automatischen Verwaltung für virtuelle Computer über Azure Policy

Wenn Sie die automatische Verwaltung für viele VMs aktivieren möchten, können Sie dies mithilfe einer integrierten [Azure Policy](..\governance\azure-management.md) erstellen. Dieser Artikel führt Sie durch die Suche nach der richtigen Richtlinie und deren Zuweisung, um die automatische Verwaltung im Azure-Portal zu ermöglichen.


## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/), bevor Sie beginnen.

> [!NOTE]
> Mit kostenlosen Testkonten ist kein Zugriff auf die virtuellen Computer möglich, die in diesem Tutorial verwendet werden. Führen Sie ein Upgrade auf ein Abonnement mit nutzungsbasierter Zahlung durch.

> [!IMPORTANT]
> Zum Aktivieren der automatischen Verwaltung ist die folgende Azure RBAC-Berechtigung erforderlich: Rolle **Besitzer** oder **Mitwirkender** sowie Rollen vom Typ **Benutzerzugriffsadministrator**

## <a name="direct-link-to-policy"></a>Direkter Link zur Richtlinie
Die Definition der Automanage-Richtlinie finden Sie im Azure-Portal unter dem Namen [VMs für das Onboarding in Azure Automanage konfigurieren](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F270610db-8c04-438a-a739-e8e6745b22d3). Wenn Sie auf diesen Link klicken, fahren Sie direkt mit Schritt 8 unter [Suchen und Zuweisen der Richtlinie](#locate-and-assign-the-policy) fort.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.


## <a name="locate-and-assign-the-policy"></a>Suchen und Zuweisen der Richtlinie

1. Navigieren Sie zu **Richtlinie** im Azure-Portal.
1. Wechseln Sie in den Bereich **Definitionen**.
1. Klicken Sie auf die Dropdownliste **Kategorien**, um die verfügbaren Optionen anzuzeigen.
1. Wählen Sie die Option **Automatische Verwaltung aktivieren – Bewährte Methoden für virtuelle Azure-Computer** aus.
1. Jetzt wird die Liste aktualisiert und zeigt eine integrierte Richtlinie mit einem Namen an, der mit *Automatische Verwaltung aktivieren...* beginnt.
1. Klicken Sie auf den integrierten Richtliniennamen *Aktivieren der automatischen Verwaltung – Bewährte Methoden für virtuelle Azure-Computer*.
1. Nachdem Sie auf die Richtlinie geklickt haben, können Sie jetzt die Registerkarte **Definition** sehen.

    > [!NOTE]
    > Die Azure Policy-Definition wird mithilfe der automatischen Verwaltung von Parametern wie dem Konfigurationsprofil oder dem Konto festgelegt. Sie legt auch Filter fest, die sicherstellen, dass die Richtlinie nur auf die richtigen VMs angewendet wird.

1. Klicken Sie auf die Schaltfläche **Zuweisen**, um eine Zuweisung zu erstellen.
1. Unter der Registerkarte **Grundlagen** füllen Sie **Bereich** aus, indem Sie das *Abonnement* und die *Ressourcengruppe* festlegen.

    > [!NOTE]
    > Über den Bereich können Sie festlegen, für welche VMs diese Richtlinie gilt. Sie können Anwendungen auf Abonnementebene oder Ressourcengruppenebene festlegen. Wenn Sie eine Ressourcengruppe festlegen, wird für alle VMs, die sich derzeit in dieser Ressourcengruppe befinden, oder für alle zukünftigen VMs, die wir dieser Gruppe hinzufügen, automatisch die automatische Verwaltung aktiviert.

1. Klicken Sie auf die Registerkarte **Parameter**, und legen Sie das **Konto für die automatische Verwaltung** und das gewünschte **Konfigurationsprofil** fest.
1. Überprüfen Sie unter der Registerkarte **Überprüfen + erstellen** die Einstellungen.
1. Wenden Sie die Zuweisung an, indem Sie auf **Erstellen** klicken.
1. Zeigen Sie Ihre Zuweisungen auf der Registerkarte **Zuweisungen** neben **Definition** an.

> [!NOTE]
> Es wird einige Zeit dauern, bis diese Richtlinie auf den VMs, die sich derzeit in der Ressourcengruppe oder im Abonnement befinden, in Kraft tritt.


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie eine weitere Möglichkeit, die automatische Azure-Verwaltung für virtuelle Computer über das Azure-Portal zu aktivieren.

> [!div class="nextstepaction"]
> [Aktivieren der automatischen Verwaltung für virtuelle Computer im Azure-Portal](quick-create-virtual-machines-portal.md)