---
title: Erstellen einer benutzerdefinierten Voreinstellung in der automatischen Azure-Verwaltung für VMs
description: Erfahren Sie, wie Sie die Umgebungskonfiguration in Azure Automanage anpassen und Ihre eigenen Voreinstellungen festlegen können.
author: ju-shim
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: how-to
ms.date: 02/22/2021
ms.author: jushiman
ms.openlocfilehash: 584a3503bf736fcf727a169611e6c79e0c374c90
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101647930"
---
# <a name="create-a-custom-preference-in-azure-automanage-for-vms"></a>Erstellen einer benutzerdefinierten Voreinstellung in der automatischen Azure-Verwaltung für VMs

Die bewährten Methoden für Azure Automanage für virtuelle Computer verfügt über Standardumgebungen, die bei Bedarf angepasst werden können. In diesem Artikel wird erläutert, wie Sie Ihre eigenen Voreinstellungen festlegen können, wenn Sie die automatische Verwaltung auf einer neuen oder bestehenden VM aktivieren.

Wir unterstützen derzeit Anpassungen bei [Azure Backup](..\backup\backup-azure-arm-vms-prepare.md#create-a-custom-policy) und [Microsoft Antimalware](../security/fundamentals/antimalware.md#default-and-custom-antimalware-configuration).


> [!NOTE]
> Sie können die Umgebung oder die Voreinstellung auf Ihrer VM nicht ändern, solange Automanage aktiviert ist. Sie müssen die automatische Verwaltung für diese VM deaktivieren und dann Automanage mit der gewünschten Konfigurationsumgebung und den gewünschten Voreinstellungen erneut aktivieren.


## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/), bevor Sie beginnen.

> [!NOTE]
> Mit kostenlosen Testkonten ist kein Zugriff auf die virtuellen Computer möglich, die in diesem Tutorial verwendet werden. Führen Sie ein Upgrade auf ein Abonnement mit nutzungsbasierter Zahlung durch.

> [!IMPORTANT]
> Zum Aktivieren der automatischen Verwaltung ist die folgende Azure RBAC-Berechtigung erforderlich: Rolle **Besitzer** oder **Mitwirkender** sowie Rollen vom Typ **Benutzerzugriffsadministrator**.


## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.


## <a name="enable-automanage-for-vms-on-an-existing-vm"></a>Aktivieren der automatischen Verwaltung für virtuelle Computer auf einem vorhandenen virtuellen Computer

1. Suchen Sie über die Suchleiste nach **Automanage – Best Practices für Azure-VMs**, und wählen Sie die Option aus.

2. Wählen Sie **Für vorhandene VM aktivieren** aus.

3. Gehen Sie auf dem Blatt **Computer auswählen** wie folgt vor:
    1. Filtern Sie die Liste der virtuellen Computer nach Ihrem **Abonnement** und nach der **Ressourcengruppe**.
    1. Aktivieren Sie das Kontrollkästchen für jeden virtuellen Computer, den Sie integrieren möchten.
    1. Klicken Sie auf die Schaltfläche **Auswählen**.

    :::image type="content" source="media\virtual-machine-custom-preferences\existing-vm-select-machine.png" alt-text="Auswählen eines vorhandenen virtuellen Computers aus der Liste verfügbarer virtueller Computer":::

    > [!NOTE]
    > Klicken Sie auf die Option zum **Anzeigen unzulässiger Computer**, um die Liste der nicht unterstützten Computer und die Gründe dafür anzuzeigen. 

4. Klicken Sie unter **Konfiguration** auf **Umgebungen vergleichen**.

    :::image type="content" source="media\virtual-machine-custom-preferences\existing-vm-quick-create.png" alt-text="Vergleichen von Umgebungen":::

5. Wählen Sie auf dem Blatt **Umgebungsdetails** eine Umgebung aus dem Dropdownmenü aus: *Dev/Test* für Tests, *Prod* für Produktion, und klicken Sie dann auf **OK**.

    :::image type="content" source="media\virtual-machine-custom-preferences\browse-production-profile.png" alt-text="Durchsuchen der Produktionsumgebung":::

6. Nachdem Sie Ihre Umgebung ausgewählt haben, können Sie **Konfigurationseinstellungen** auswählen. Standardmäßig werden die Best Practices in Azure verwendet. Diese Voreinstellung enthält die empfohlenen Einstellungen für die einzelnen Dienste. Ändern Sie diese Einstellungen, indem Sie eine benutzerdefinierte Voreinstellung erstellen: 
    1. Klicken Sie auf die Option zum **Erstellen neuer Voreinstellungen**.
    1. Füllen Sie im Blatt **Konfigurationseinstellung erstellen** die Registerkarte „Grundlagen“ aus:
        1. Subscription
        1. Resource group
        1. Name der Voreinstellung
        1. Region

    :::image type="content" source="media\virtual-machine-custom-preferences\create-preference.png" alt-text="Füllen Sie die Konfigurationseinstellungen aus.":::

7. Passen Sie die gewünschten Konfigurationseinstellungen an.
        
    > [!NOTE]
    > Bei der Änderung von Umgebungskonfigurationen werden nur Anpassungen zugelassen, die noch innerhalb der oberen und unteren Grenzen unserer bewährten Methoden liegen.

8. Überprüfen Sie Ihre Konfigurationsdetails.
9. Klicken Sie auf die Schaltfläche **Erstellen**.

10. Klicken Sie auf die Schaltfläche **Aktivieren**.


## <a name="disable-automanage-for-vms"></a>Deaktivieren der automatischen Verwaltung für virtuelle Computer

Sie können die Verwendung der automatischen Azure-Verwaltung für virtuelle Computer schnell beenden, indem Sie die automatische Verwaltung deaktivieren.

:::image type="content" source="media\virtual-machine-custom-preferences\disable-step-1.png" alt-text="Deaktivieren der automatischen Verwaltung auf einem virtuellen Computer":::

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