---
title: 'Schnellstart: Aktivieren der automatischen Azure-Verwaltung für virtuelle Computer über das Azure-Portal'
description: Hier erfahren Sie, wie Sie über das Azure-Portal schnell die automatische Verwaltung für virtuelle Computer auf einem neuen oder vorhandenen virtuellen Computer aktivieren.
author: ju-shim
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: quickstart
ms.date: 02/17/2021
ms.author: jushiman
ms.openlocfilehash: 6e0e582ed37230ba3f379f193a229cfec06f066c
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101648032"
---
# <a name="quickstart-enable-azure-automanage-for-virtual-machines-in-the-azure-portal"></a>Schnellstart: Aktivieren der automatischen Azure-Verwaltung für virtuelle Computer im Azure-Portal

Beginnen Sie mit der automatischen Azure-Verwaltung für virtuelle Computer, indem Sie über das Azure-Portal die automatische Verwaltung für einen neuen oder vorhandenen virtuellen Computer aktivieren.


## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/), bevor Sie beginnen.

> [!NOTE]
> Mit kostenlosen Testkonten ist kein Zugriff auf die virtuellen Computer möglich, die in diesem Tutorial verwendet werden. Führen Sie ein Upgrade auf ein Abonnement mit nutzungsbasierter Zahlung durch.

> [!IMPORTANT]
> Sie müssen über die Rolle **Mitwirkender** für die Ressourcengruppe verfügen, die Ihre virtuellen Computer enthält, um Automanage mithilfe eines vorhandenen Automanage-Kontos zu aktivieren. Wenn Sie automatische Verwaltung mit einem neuen Konto für die automatische Verwaltung aktivieren, benötigen Sie die folgenden Berechtigungen: Rolle **Besitzer** oder **Mitwirkender** sowie Rollen vom Typ **Benutzerzugriffsadministrator** für Ihr Abonnement.


## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://aka.ms/AutomanagePortal-Ignite21) an.

## <a name="enable-automanage-for-a-single-vm"></a>Aktivieren von Automanage für eine einzelne VM

1. Navigieren Sie zu der VM, die Sie aktivieren möchten.

2. Klicken Sie im Inhaltsverzeichnis unter **Vorgänge** auf den Eintrag **Automanage (Vorschau)** .

3. Wählen Sie **Erste Schritte** aus.

    :::image type="content" source="media\quick-create-virtual-machine-portal\VMManage-GetStarted.png" alt-text="„Erste Schritte“ auf einer einzelnen VM.":::

4. Wählen Sie Ihre Automanage-Einstellungen aus (Umgebung, Voreinstellungen, Automanage-Konto), und klicken Sie auf **Aktivieren**.

    :::image type="content" source="media\quick-create-virtual-machine-portal\VMManage-Enable.png" alt-text="„Aktivieren“ auf einer einzelnen VM.":::

## <a name="enable-automanage-for-multiple-vms"></a>Aktivieren von Automanage für mehrere VMs

1. Suchen Sie über die Suchleiste nach **Automanage – Best Practices für Azure-VMs**, und wählen Sie die Option aus.

2. Wählen Sie **Für vorhandene VM aktivieren** aus.

    :::image type="content" source="media\quick-create-virtual-machine-portal\zero-vm-list-view.png" alt-text="Für vorhandene VM aktivieren":::

3. Gehen Sie auf dem Blatt **Computer auswählen** wie folgt vor:
    1. Filtern Sie die Liste der virtuellen Computer nach Ihrem **Abonnement** und nach der **Ressourcengruppe**.
    1. Aktivieren Sie das Kontrollkästchen für jeden virtuellen Computer, den Sie integrieren möchten.
    1. Klicken Sie auf die Schaltfläche **Auswählen**.

    :::image type="content" source="media\quick-create-virtual-machine-portal\existing-vm-select-machine.png" alt-text="Auswählen eines vorhandenen virtuellen Computers aus der Liste verfügbarer virtueller Computer":::

4. Klicken Sie unter **Configuration Profile** (Konfigurationsprofil) auf **Browse and change profiles and preferences** (Profile und Einstellungen durchsuchen und ändern).

    :::image type="content" source="media\quick-create-virtual-machine-portal\existing-vm-quick-create.png" alt-text="Browse and change profiles and preferences (Profile und Einstellungen durchsuchen und ändern)":::

5. Gehen Sie auf dem Blatt **Konfigurationsprofil und -einstellungen auswählen** wie folgt vor:
    1. Wählen Sie auf der linken Seite ein Profil aus: *Dev/Test* für Tests, *Prod* für die Produktion.
    1. Klicken Sie auf die Schaltfläche **Auswählen**.

    :::image type="content" source="media\quick-create-virtual-machine-portal\browse-production-profile.png" alt-text="Navigieren zum Produktionskonfigurationsprofil":::

6. Klicken Sie auf die Schaltfläche **Aktivieren**.


## <a name="enable-automanage-for-a-new-vm"></a>Aktivieren von Automanage für eine neue VM

Melden Sie sich [hier](https://aka.ms/AutomanagePortal-Ignite21) beim Azure-Portal an, um einen neuen virtuellen Computer zu erstellen und automatische Verwaltung zu aktivieren.

1. Klicken Sie links oben im Azure-Portal auf **Ressource erstellen**.

2. Suchen Sie im Suchfeld über der Liste mit den Azure Marketplace-Ressourcen nach dem gewünschten Image, wählen Sie es aus, und klicken Sie anschließend auf **Erstellen**.

> [!NOTE]
> Überprüfen Sie die von Automanage unterstützten [Linux-Distributionen](automanage-linux.md#supported-linux-distributions-and-versions) und [Windows Server-Versionen](automanage-windows-server.md#supported-windows-server-versions).

3. Füllen Sie die Registerkarte **Grundlagen** mit Ihren VM-Details aus.

> [!NOTE]
> Überprüfen Sie die [unterstützten Regionen](automanage-virtual-machines#supported-regions) für Automanage.

4. Navigieren Sie zur Registerkarte **Verwaltung**, und wählen Sie Ihre **Automanage-Umgebung** aus.

    :::image type="content" source="media\quick-create-virtual-machine-portal\VMCreate-Management-Tab.png" alt-text="„Automanage aktivieren“ auf der Registerkarte „Verwaltung“.":::

5. Belassen Sie die übrigen Standardeinstellungen, und wählen Sie dann die Schaltfläche **Überprüfen + erstellen** am unteren Rand der Seite aus.

6. Wenn eine Meldung über die erfolgreiche Validierung angezeigt wird, wählen Sie **Erstellen** aus.

## <a name="disable-automanage-for-vms"></a>Deaktivieren der automatischen Verwaltung für virtuelle Computer

Sie können die Verwendung der automatischen Azure-Verwaltung für virtuelle Computer schnell beenden, indem Sie die automatische Verwaltung deaktivieren.

:::image type="content" source="media\automanage-virtual-machines\disable-step-1.png" alt-text="Deaktivieren der automatischen Verwaltung auf einem virtuellen Computer":::

1. Navigieren Sie zur Seite **Automatische Verwaltung – Best Practices für Azure-VMs**, auf der alle automatisch verwalteten virtuellen Computer aufgeführt sind.
1. Aktivieren Sie das Kontrollkästchen neben dem virtuellen Computer, den Sie deaktivieren möchten.
1. Klicken Sie auf die Schaltfläche **Automatische Verwaltung deaktivieren**.
1. Lesen Sie die Meldung im angezeigten Popupelement sorgfältig durch, bevor Sie dem **Deaktivieren** zustimmen.


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie eine neue Ressourcengruppe erstellt haben, um die automatische Azure-Verwaltung für virtuelle Computer zu testen, und die Ressourcengruppe nicht mehr benötigen, können Sie sie löschen. Wenn Sie die Gruppe löschen, werden auch der virtuelle Computer und alle Ressourcen in der Ressourcengruppe gelöscht.

Die automatische Azure-Verwaltung erstellt Standardressourcengruppen zum Speichern von Ressourcen. Überprüfen Sie die Ressourcengruppen mit der Namenskonvention „DefaultResourceGroupRegionName“ und „AzureBackupRGRegionName“, um alle Ressourcen zu bereinigen.

1. Wählen Sie die **Ressourcengruppe** aus.
1. Wählen Sie auf der Seite für die Ressourcengruppe die Option **Löschen** aus.
1. Geben Sie bei entsprechender Aufforderung den Namen der Ressourcengruppe ein, und wählen Sie dann **Löschen** aus.


## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie die automatische Azure-Verwaltung für virtuelle Computer aktiviert.

Erfahren Sie, wie Sie angepasste Einstellungen erstellen und anwenden können, wenn Sie die automatische Verwaltung auf virtuellen Computern aktivieren.

> [!div class="nextstepaction"]
> [Azure Automanage für VMs – Benutzerdefiniertes Konfigurationsprofil](virtual-machines-custom-preferences.md)
