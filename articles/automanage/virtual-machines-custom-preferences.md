---
title: Erstellen einer benutzerdefinierten Voreinstellung in der automatischen Azure-Verwaltung für VMs
description: Erfahren Sie, wie Sie das Konfigurationsprofil in der automatischen Azure-Verwaltung für VMs anpassen und Ihre eigenen Voreinstellungen festlegen können.
author: ju-shim
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: how-to
ms.date: 09/04/2020
ms.author: jushiman
ms.openlocfilehash: 377677c9e5e81487059241db68baff639a3de033
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91715046"
---
# <a name="create-a-custom-preference-in-azure-automanage-for-vms"></a>Erstellen einer benutzerdefinierten Voreinstellung in der automatischen Azure-Verwaltung für VMs

Die bewährten Methoden für die automatische Azure-Verwaltung von virtuellen Computern verfügt über Standardkonfigurationsprofile, die bei Bedarf angepasst werden können. In diesem Artikel wird erläutert, wie Sie Ihre eigenen Konfigurationsprofileinstellungen festlegen können, wenn Sie die automatische Verwaltung auf einer neuen oder bestehenden VM aktivieren.

Wir unterstützen derzeit Anpassungen bei [Azure Backup](..\backup\backup-azure-arm-vms-prepare.md#create-a-custom-policy) und [Microsoft Antimalware](../security/fundamentals/antimalware.md#default-and-custom-antimalware-configuration).


> [!NOTE]
> Sie können das Konfigurationsprofil oder die Konfigurationseinstellung auf Ihrer VM nicht ändern, solange die automatische Verwaltung aktiviert ist. Sie müssen die automatische Verwaltung für diese VM deaktivieren und dann die automatische Verwaltung mit dem gewünschten Konfigurationsprofil und den gewünschten Voreinstellungen erneut aktivieren.


## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/), bevor Sie beginnen.

> [!NOTE]
> Mit kostenlosen Testkonten ist kein Zugriff auf die virtuellen Computer möglich, die in diesem Tutorial verwendet werden. Führen Sie ein Upgrade auf ein Abonnement mit nutzungsbasierter Zahlung durch.

> [!IMPORTANT]
> Zum Aktivieren der automatischen Verwaltung ist die folgende Azure RBAC-Berechtigung erforderlich: Rolle **Besitzer** oder **Mitwirkender** sowie Rollen vom Typ **Benutzerzugriffsadministrator**.


## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.


## <a name="enable-automanage-for-vms-on-an-existing-vm"></a>Aktivieren der automatischen Verwaltung für virtuelle Computer auf einem vorhandenen virtuellen Computer

1. Suchen Sie über die Suchleiste nach **Automatische Verwaltung – Best Practices für Azure-VMs**, und wählen Sie die Option aus.

2. Wählen Sie **Für vorhandene VM aktivieren** aus.

3. Gehen Sie auf dem Blatt **Computer auswählen** wie folgt vor:
    1. Filtern Sie die Liste der virtuellen Computer nach Ihrem **Abonnement** und nach der **Ressourcengruppe**.
    1. Aktivieren Sie das Kontrollkästchen für jeden virtuellen Computer, den Sie integrieren möchten.
    1. Klicken Sie auf die Schaltfläche **Auswählen**.

    :::image type="content" source="media\virtual-machine-custom-preferences\existing-vm-select-machine.png" alt-text="Auswählen eines vorhandenen virtuellen Computers aus der Liste verfügbarer virtueller Computer":::

4. Klicken Sie unter **Configuration Profile** (Konfigurationsprofil) auf **Browse and change profiles and preferences** (Profile und Einstellungen durchsuchen und ändern).

    :::image type="content" source="media\virtual-machine-custom-preferences\existing-vm-quick-create.png" alt-text="Auswählen eines vorhandenen virtuellen Computers aus der Liste verfügbarer virtueller Computer":::

5. Wählen Sie auf dem Blatt **Konfigurationsprofil und -einstellungen auswählen** ein Profil auf der linken Seite aus: *Dev/Test* für Tests, *Prod* für die Produktion.

    :::image type="content" source="media\virtual-machine-custom-preferences\browse-production-profile.png" alt-text="Auswählen eines vorhandenen virtuellen Computers aus der Liste verfügbarer virtueller Computer":::

6. Auf dem gewählten Profil gibt es unter **Konfigurationseinstellungen** eine Dropdownliste, in der Sie bestimmte Dienste anpassen können.
    1. Klicken Sie auf die Option zum **Erstellen neuer Voreinstellungen**.
    1. Füllen Sie im Blatt **Konfigurationseinstellung erstellen** die Registerkarte „Grundlagen“ aus:
        1. Subscription
        1. Resource group
        1. Name der Voreinstellung
        1. Region

    :::image type="content" source="media\virtual-machine-custom-preferences\create-preference.png" alt-text="Auswählen eines vorhandenen virtuellen Computers aus der Liste verfügbarer virtueller Computer":::

7. Wechseln Sie zur Registerkarte „Voreinstellungen“, und passen Sie die von Ihnen gewünschten Konfigurationseinstellungen an.
        
    > [!NOTE]
    > Bei der Änderung von Profilkonfigurationen werden nur Anpassungen zugelassen, die noch innerhalb der oberen und unteren Grenzen unserer bewährten Methoden liegen.

8. Überprüfen Sie Ihr Konfigurationsprofil.
9. Klicken Sie auf die Schaltfläche **Erstellen**.

10. Klicken Sie auf die Schaltfläche **Aktivieren**.


## <a name="disable-automanage-for-vms"></a>Deaktivieren der automatischen Verwaltung für virtuelle Computer

Sie können die Verwendung der automatischen Azure-Verwaltung für virtuelle Computer schnell beenden, indem Sie die automatische Verwaltung deaktivieren.

:::image type="content" source="media\virtual-machine-custom-preferences\disable-step-1.png" alt-text="Auswählen eines vorhandenen virtuellen Computers aus der Liste verfügbarer virtueller Computer":::

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

Die am häufigsten gestellten Fragen werden in unserem FAQ-Abschnitt (Häufig gestellte Fragen) beantwortet. 

> [!div class="nextstepaction"]
> [Häufig gestellte Fragen](faq.md)