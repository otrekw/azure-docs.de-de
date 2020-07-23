---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/23/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 8956d4f5b2243cab433fcb3abaf2e71da8f8c772
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86229413"
---
Das erstmalige Einrichten von kundenseitig verwalteten Schlüssel für Ihre Datenträger erfordert die Erstellung von Ressourcen in einer bestimmten Reihenfolge. Zuerst müssen Sie eine Azure Key Vault-Instanz erstellen und einrichten.

## <a name="set-up-your-azure-key-vault"></a>Einrichten des Azure-Schlüsseltresors

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Suchen Sie nach **Schlüsseltresore**, und wählen Sie diese Option aus.

    [ ![server-side-encryption-key-vault-portal-search.png](media/virtual-machines-disk-encryption-portal/server-side-encryption-key-vault-portal-search.png)](media/virtual-machines-disk-encryption-portal/sever-side-encryption-key-vault-portal-search-expanded.png#lightbox)

    > [!IMPORTANT]
    > Ihre Azure Key Vault-Instanz, der Datenträgerverschlüsselungssatz, die VM, die Datenträger und die Momentaufnahmen müssen sich alle in derselben Region und demselben Abonnement befinden, damit die Bereitstellung erfolgreich ausgeführt werden kann.

1. Wählen Sie **+ Hinzufügen** aus, um einen neuen Schlüsseltresor zu erstellen.
1. Erstellen Sie eine neue Ressourcengruppe.
1. Geben Sie einen Namen für den Schlüsseltresor ein, wählen Sie eine Region aus, und wählen Sie dann einen Tarif aus.
1. Wählen Sie **Bewerten + erstellen** aus, überprüfen Sie Ihre Auswahl, und wählen Sie dann **Erstellen** aus.

    ![Screenshot des Azure Key Vault-Erstellungsvorgangs. Die von Ihnen erstellten Werte werden dargestellt.](media/virtual-machines-disk-encryption-portal/server-side-encryption-create-a-key-vault.png)

1. Nach Abschluss der Bereitstellung Ihres Schlüsseltresors wählen Sie diesen aus.
1. Wählen Sie unter **Einstellungen** die Option **Schlüssel** aus.
1. Wählen Sie die Option **Generieren/Importieren** aus.

    ![Screenshot des Key Vault-Bereichs für Ressourceneinstellungen. Zeigt die Schaltfläche Generieren/Importieren in „Einstellungen“ an.](media/virtual-machines-disk-encryption-portal/sever-side-encryption-key-vault-generate-settings.png)

1. Lassen Sie **Schlüsseltyp** auf **RSA** und **RSA-Schlüsselgröße** auf **2048** festgelegt.
1. Treffen Sie Ihre Auswahl für die verbleibenden Optionen nach Wunsch, und wählen Sie dann **Erstellen** aus.

    ![Screenshot des Blatts „Schlüssel erstellen“, das nach dem Auswählen der Schaltfläche „Generieren/Importieren“ angezeigt wird.](media/virtual-machines-disk-encryption-portal/server-side-encryption-create-a-key-generate.png)

## <a name="set-up-your-disk-encryption-set"></a>Einrichten des Datenträgerverschlüsselungssatzes

1. Suchen Sie nach **Datenträgerverschlüsselungssätze**, und wählen Sie sie aus.
1. Wählen Sie auf dem Blatt **Datenträgerverschlüsselungssätze** die Option **+Hinzufügen** aus.

    ![Screenshot des Hauptbildschirms des Datenträgerverschlüsselungs-Portals. Die Schaltfläche „Hinzufügen“ ist hervorgehoben.](media/virtual-machines-disk-encryption-portal/sever-side-encryption-create-disk-encryption-set.png)

1. Wählen Sie Ihre Ressourcengruppe aus, geben Sie Ihrem Verschlüsselungssatz einen Namen, und wählen Sie dieselbe Region aus wie Ihr Schlüsseltresor.
1. Wählen Sie **Schlüsseltresor und Schlüssel** aus.
1. Wählen Sie den Schlüsseltresor und den Schlüssel, die Sie zuvor erstellt haben, sowie die Version aus.
1. Drücken Sie **Auswählen**.
1. Wählen Sie **Überprüfen + erstellen** und danach **Erstellen** aus.

    ![Screenshot des Blatts zum Erstellen der Datenträgerverschlüsselung. Dargestellt sind das Abonnement, die Ressourcengruppe, der Name des Datenträgerverschlüsselungssatzes, die Region und das Auswahlelement für den Schlüsseltresor und den Schlüssel.](media/virtual-machines-disk-encryption-portal/server-side-encryption-disk-encryption-set-blade-key.png)

1. Öffnen Sie den Datenträgerverschlüsselungssatz, nachdem die Erstellung abgeschlossen ist, und wählen Sie die angezeigte Warnung aus.

    ![Screenshot des Benachrichtigungs-Popups: „Um einem Datenträgerverschlüsselungssatz einen Datenträger, ein Image oder eine Momentaufnahme zuzuordnen, müssen Sie dem Schlüsseltresor Berechtigungen erteilen“. Wählen Sie diese Warnung aus, um fortzufahren.](media/virtual-machines-disk-encryption-portal/server-side-encryption-disk-encryption-set-alert-fix.png)

Es sollten zwei Benachrichtigungen angezeigt werden. Auf diese Weise können Sie den Datenträgerverschlüsselungssatz mit Ihrem Schlüsseltresor verwenden.

![Screenshot der erfolgreichen Berechtigungs- und Rollenzuweisung für Ihren Schlüsseltresor.](media/virtual-machines-disk-encryption-portal/disk-encryption-notification-success.png)

## <a name="deploy-a-vm"></a>Bereitstellen einer VM

Nachdem Sie nun Ihren Schlüsseltresor und den Datenträgerverschlüsselungssatz erstellt und eingerichtet haben, können Sie eine VM mit der Verschlüsselung bereitstellen.
Das Verfahren zur VM-Bereitstellung ähnelt dem Standardbereitstellungsverfahren. Die einzigen Unterschiede bestehen darin, dass Sie den virtuellen Computer in derselben Region wie Ihre anderen Ressourcen bereitstellen müssen und einen vom Kunden verwalteten Schlüssel verwenden.

1. Suchen Sie nach **Virtuelle Computer**, und wählen Sie **+ Hinzufügen** aus, um eine VM zu erstellen.
1. Wählen Sie auf der Registerkarte **Standard** dieselbe Region aus, in der sich Ihr Datenträgerverschlüsselungssatz und Ihre Azure Key Vault-Instanz befinden.
1. Geben Sie die anderen Werte auf der Registerkarte **Standard** nach Wunsch ein.

    ![Screenshot des VM-Erstellungsvorgangs mit hervorgehobenem Wert für die Region.](media/virtual-machines-disk-encryption-portal/server-side-encryption-create-a-vm-region.png)

1. Wählen Sie auf der Registerkarte **Datenträger** die Option **Verschlüsselung ruhender Daten mit einem kundenseitig verwalteten Schlüssel** aus.
1. Wählen Sie den Datenträgerverschlüsselungssatz in der Dropdownliste **Datenträgerverschlüsselungssatz** aus.
1. Treffen Sie die verbleibende Auswahl nach Wunsch.

    ![Screenshot des VM-Erstellungsvorgangs, Datenträgerblatt. Das Dropdownfeld für den Datenträgerverschlüsselungssatz ist hervorgehoben.](media/virtual-machines-disk-encryption-portal/server-side-encryption-create-vm-select-customer-managed-key-disk-encryption-set.png)

## <a name="enable-on-an-existing-disk"></a>Aktivieren auf einem vorhandenen Datenträger

> [!CAUTION]
> Zum Aktivieren der Datenträgerverschlüsselung auf Datenträgern, die an eine VM angefügt sind, müssen Sie die VM beenden.
    
1. Navigieren Sie zu einer VM, die sich in derselben Region befindet wie einer Ihrer Datenträgerverschlüsselungssätze.
1. Öffnen Sie die VM, und wählen Sie **Beenden** aus.

    ![Screenshot der Hauptüberlagerung für Ihre Beispiel-VM. Die Schaltfläche „Stopp“ ist hervorgehoben.](media/virtual-machines-disk-encryption-portal/server-side-encryption-stop-vm-to-encrypt-disk-fix.png)

1. Nachdem die VM beendet wurde, wählen Sie **Datenträger** und dann den Datenträger aus, den Sie verschlüsseln möchten.

    ![Screenshot Ihrer Beispiel-VM mit geöffnetem Datenträgerblatt. Der Betriebssystemdatenträger ist als Beispieldatenträger hervorgehoben, den Sie auswählen können.](media/virtual-machines-disk-encryption-portal/server-side-encryption-existing-disk-select.png)

1. Wählen Sie **Verschlüsselung**, anschließend **Verschlüsselung ruhender Daten mit einem kundenseitig verwalteten Schlüssel** und dann in der Dropdownliste den Datenträgerverschlüsselungssatz aus.
1. Wählen Sie **Speichern** aus.

    ![Screenshot Ihres Beispiel-Betriebssystemdatenträgers. Das Verschlüsselungsblatt ist geöffnet, die Verschlüsselung ruhender Daten mit einem kundenseitig verwalteten Schlüssel und Ihr Beispiel-Azure Key Vault sind ausgewählt. Nach dem Treffen dieser Auswahl wird die Speichern-Schaltfläche ausgewählt.](media/virtual-machines-disk-encryption-portal/server-side-encryption-encrypt-existing-disk-customer-managed-key.png)

1. Wiederholen Sie diesen Vorgang für alle anderen an die VM angefügten Datenträger, die Sie verschlüsseln möchten.
1. Wenn die Umstellung der Datenträger auf kundenseitig verwaltete Schlüssel abgeschlossen ist und Sie keine weiteren angefügten Datenträger mehr verschlüsseln möchten, können Sie Ihren virtuellen Computer starten.
