---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/13/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0912316d1c41f46e5dba74b58017f4fd5e8ed529
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76909119"
---
### <a name="portal"></a>Portal

Das erstmalige Einrichten von kundenseitig verwalteten Schlüssel für Ihre Datenträger erfordert die Erstellung von Ressourcen in einer bestimmten Reihenfolge. Zuerst müssen Sie eine Azure Key Vault-Instanz erstellen und einrichten.

#### <a name="setting-up-your-azure-key-vault"></a>Einrichten v on Azure Key Vault

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und suchen Sie nach „Key Vault“.
1. Suchen Sie nach **Schlüsseltresore**, und wählen Sie diese Option aus.

    [ ![sse-key-vault-portal-search.png](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search.png)](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search-expanded.png#lightbox)

    > [!IMPORTANT]
    > Ihre Azure Key Vault-Instanz, der Datenträgerverschlüsselungssatz, die VM, die Datenträger und die Momentaufnahmen müssen sich alle in derselben Region und demselben Abonnement befinden, damit die Bereitstellung erfolgreich ausgeführt werden kann.

1. Wählen Sie **+ Hinzufügen** aus, um einen neuen Schlüsseltresor zu erstellen.
1. Erstellen einer neuen Ressourcengruppe
1. Geben Sie einen Namen für den Schlüsseltresor ein, wählen Sie eine Region aus, und wählen Sie dann einen Tarif aus.
1. Wählen Sie **Bewerten + erstellen** aus, überprüfen Sie Ihre Auswahl, und wählen Sie dann **Erstellen** aus.

    ![sse-create-a-key-vault.png](media/virtual-machines-disk-encryption-portal/sse-create-a-key-vault.png)

1. Nach Abschluss der Bereitstellung Ihres Schlüsseltresors wählen Sie diesen aus.
1. Wählen Sie unter **Einstellungen** die Option **Schlüssel** aus.
1. Wählen Sie die Option **Generieren/importieren** aus.

    ![sse-key-vault-generate-settings.png](media/virtual-machines-disk-encryption-portal/sse-key-vault-generate-settings.png)

1. Lassen Sie **Schlüsseltyp** auf **RSA** und **RSA-Schlüsselgröße** auf **2080** festgelegt.
1. Treffen Sie Ihre Auswahl für die verbleibenden Optionen nach Wunsch, und wählen Sie dann **Erstellen** aus.

    ![sse-create-a-key-generate.png](media/virtual-machines-disk-encryption-portal/sse-create-a-key-generate.png)

#### <a name="setting-up-your-disk-encryption-set"></a>Einrichten des Datenträgerverschlüsselungssatzes

Sie müssen den folgenden Link verwenden, um Datenträgerverschlüsselungssätze zu erstellen und zu konfigurieren: https://aka.ms/diskencryptionsets. Die Erstellung des Datenträgerverschlüsselungssatzes ist im globalen Azure-Portal noch nicht verfügbar.

1. Öffnen Sie den [Link für Datenträgerverschlüsselungssätze](https://aka.ms/diskencryptionsets).
1. Wählen Sie **+ Hinzufügen** aus.

    ![sse-create-disk-encryption-set.png](media/virtual-machines-disk-encryption-portal/sse-create-disk-encryption-set.png)

1. Wählen Sie Ihre Ressourcengruppe aus, geben Sie Ihrem Verschlüsselungssatz einen Namen, und wählen Sie dieselbe Region aus wie Ihr Schlüsseltresor.
1. Wählen Sie **Schlüsseltresor und Schlüssel** aus.
1. Wählen Sie den Schlüsseltresor und den Schlüssel, die Sie zuvor erstellt haben, sowie die Version aus.
1. Drücken Sie **Auswählen**.
1. Wählen Sie **Überprüfen + erstellen** und danach **Erstellen** aus.

    ![sse-disk-enc-set-blade-key.png](media/virtual-machines-disk-encryption-portal/sse-disk-enc-set-blade-key.png)

1. Öffnen Sie den Datenträgerverschlüsselungssatz, nachdem die Erstellung abgeschlossen ist, und wählen Sie die angezeigte Warnung aus.

    ![sse-disk-enc-alert-fix.png](media/virtual-machines-disk-encryption-portal/sse-disk-enc-alert-fix.png)

Es sollten zwei Benachrichtigungen angezeigt werden. Auf diese Weise können Sie den Datenträgerverschlüsselungssatz mit Ihrem Schlüsseltresor verwenden.

![disk-enc-notification-success.png](media/virtual-machines-disk-encryption-portal/disk-enc-notification-success.png)

#### <a name="deploy-a-vm"></a>Bereitstellen einer VM

Nachdem Sie nun Ihren Schlüsseltresor und den Datenträgerverschlüsselungssatz erstellt und eingerichtet haben, können Sie eine VM mit der Verschlüsselung bereitstellen.
Das Verfahren zur VM-Bereitstellung ähnelt dem Standardbereitstellungsverfahren. Die einzigen Unterschiede bestehen darin, dass Sie den virtuellen Computer in derselben Region wie Ihre anderen Ressourcen bereitstellen müssen und einen vom Kunden verwalteten Schlüssel verwenden.

1. Öffnen Sie den [Link für Datenträgerverschlüsselungssätze](https://aka.ms/diskencryptionsets).
1. Suchen Sie nach **Virtuelle Computer**, und wählen Sie **+ Hinzufügen** aus, um eine VM zu erstellen.
1. Wählen Sie auf der Registerkarte **Standard** dieselbe Region aus, in der sich Ihr Datenträgerverschlüsselungssatz und Ihre Azure Key Vault-Instanz befinden.
1. Geben Sie die anderen Werte auf der Registerkarte **Standard** nach Wunsch ein.

    ![sse-create-a-vm-region.png](media/virtual-machines-disk-encryption-portal/sse-create-a-vm-region.png)

1. Wählen Sie auf der Registerkarte **Datenträger** die Option **Verschlüsselung ruhender Daten mit einem kundenseitig verwalteten Schlüssel** aus.
1. Wählen Sie den Datenträgerverschlüsselungssatz in der Dropdownliste **Datenträgerverschlüsselungssatz** aus.
1. Treffen Sie die verbleibende Auswahl nach Wunsch.

    ![sse-create-vm-select-cmk-encryption-set.png](media/virtual-machines-disk-encryption-portal/sse-create-vm-select-cmk-encryption-set.png)

#### <a name="enable-on-an-existing-disk"></a>Aktivieren auf einem vorhandenen Datenträger

Sie müssen den folgenden Link verwenden, um die Datenträgerverschlüsselung für Ihre vorhandenen Datenträger zu verwalten und zu konfigurieren: https://aka.ms/diskencryptionsets. Das Aktivieren von kundenseitig verwalteten Schlüsseln auf vorhandenen Datenträgern ist noch nicht im globalen Azure-Portal verfügbar.

> [!CAUTION]
> Zum Aktivieren der Datenträgerverschlüsselung auf Datenträgern, die an eine VM angefügt sind, müssen Sie die VM beenden.

1. Öffnen Sie den [Link für Datenträgerverschlüsselungssätze](https://aka.ms/diskencryptionsets).
1. Navigieren Sie zu einer VM, die sich in derselben Region befindet wie einer Ihrer Datenträgerverschlüsselungssätze.
1. Öffnen Sie die VM, und wählen Sie **Beenden** aus.

    ![sse-stop-VM-to-encrypt-disk.png](media/virtual-machines-disk-encryption-portal/sse-stop-VM-to-encrypt-disk.png)

1. Nachdem die VM beendet wurde, wählen Sie **Datenträger** und dann den Datenträger aus, den Sie verschlüsseln möchten.

    ![sse-existing-disk-select.png](media/virtual-machines-disk-encryption-portal/sse-existing-disk-select.png)

1. Wählen Sie **Verschlüsselung**, anschließend **Verschlüsselung ruhender Daten mit einem kundenseitig verwalteten Schlüssel** und dann in der Dropdownliste den Datenträgerverschlüsselungssatz aus.
1. Wählen Sie **Speichern** aus.

    ![sse-encrypt-existing-disk-customer-managed-key.png](media/virtual-machines-disk-encryption-portal/sse-encrypt-existing-disk-customer-managed-key.png)

1. Wiederholen Sie diesen Vorgang für alle anderen an die VM angefügten Datenträger, die Sie verschlüsseln möchten.
1. Wenn die Umstellung der Datenträger auf kundenseitig verwaltete Schlüssel abgeschlossen ist und Sie keine weiteren angefügten Datenträger mehr verschlüsseln möchten, können Sie Ihren virtuellen Computer starten.
