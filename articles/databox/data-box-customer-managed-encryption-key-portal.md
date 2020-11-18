---
title: Verwenden des Azure-Portals zum Konfigurieren von kundenseitig verwalteten Schlüsseln für Azure Data Box
description: Erfahren Sie, wie Sie über das Azure-Portal von Kunden verwaltete Schlüssel mit Azure Key Vault für die Azure Data Box konfigurieren. Mit von Kunden verwalteten Schlüsseln können Sie Zugriffssteuerungen erstellen, rotieren, deaktivieren und widerrufen.
services: databox
author: alkohli
ms.service: databox
ms.topic: how-to
ms.date: 05/07/2020
ms.author: alkohli
ms.subservice: pod
ms.openlocfilehash: 40b777342c2c565efc5b40d361a259c98eae693c
ms.sourcegitcommit: 2a8a53e5438596f99537f7279619258e9ecb357a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "94337707"
---
# <a name="use-customer-managed-keys-in-azure-key-vault-for-azure-data-box"></a>Verwenden kundenseitig verwalteter Schlüssel in Azure Key Vault für Azure Data Box

Azure Data Box schützt den Schlüssel für die Geräteentsperrung (auch als Gerätekennwort bezeichnet), der zum Sperren des Geräts über einen Verschlüsselungsschlüssel verwendet wird. Standardmäßig wird der Schlüssel für die Geräteentsperrung für einen Data Box-Auftrag mit einem von Microsoft verwalteten Schlüssel verschlüsselt. Um zusätzliche Kontrolle über den Schlüssel für die Geräteentsperrung zu erhalten, können Sie auch kundenseitig verwaltete Schlüssel bereitstellen. 

Kundenseitig verwaltete Schlüssel müssen in Azure Key Vault erstellt und gespeichert werden. Weitere Informationen zum Azure Key Vault finden Sie unter [What is Azure Key Vault? (Was ist der Azure Key Vault?)](../key-vault/general/overview.md).

In diesem Artikel wird gezeigt, wie Sie kundenseitig verwaltete Schlüssel mit Azure Data Box im [Azure-Portal](https://portal.azure.com/) verwenden. Dieser Artikel gilt sowohl für Azure Data Box- als auch für Azure Data Box Heavy-Geräte.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

1. Sie haben einen Azure Data Box-Auftrag gemäß den Anweisungen in [Tutorial: Bestellen von Azure Data Box](data-box-deploy-ordered.md) ausgeführt.

2. Sie verfügen über eine vorhandene Azure Key Vault-Instanz mit einem Schlüssel, den Sie zum Schützen Ihres Schlüssels für die Geräteentsperrung verwenden können. Informationen zum Erstellen eines Schlüsseltresors über das Azure-Portal finden Sie unter [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe des Azure-Portals](../key-vault/secrets/quick-create-portal.md).

    - Für Ihren vorhandenen Schlüsseltresor ist **Vorläufiges Löschen** und **Nicht bereinigen** festgelegt. Diese Eigenschaften sind standardmäßig nicht aktiviert. Informationen zum Aktivieren dieser Eigenschaften finden Sie in den Abschnitten **Aktivieren des vorläufigen Löschens** und **Aktivieren des Bereinigungsschutzes** in einem der folgenden Artikel:

        - [Verwenden des vorläufigen Löschens mit PowerShell](../key-vault/general/soft-delete-powershell.md)
        - [Verwenden des vorläufigen Löschens mit der CLI](../key-vault/general/soft-delete-cli.md)
    - Der vorhandene Schlüsseltresor muss über einen RSA-Schlüssel mit einer Größe von mindestens 2048 verfügen. Weitere Informationen zu Schlüsseln finden Sie unter [Informationen zu Azure Key Vault-Schlüsseln](../key-vault/keys/about-keys.md).
    - Der Schlüsseltresor muss sich in derselben Region wie die für Ihre Daten verwendeten Speicherkonten befinden. Mehrere Speicherkonten können mit Ihrer Azure Data Box-Ressource verknüpft werden.
    - Wenn Sie noch nicht über einen Schlüsseltresor verfügen, können Sie diesen auch inline erstellen, wie im folgenden Abschnitt beschrieben.

## <a name="enable-keys"></a>Aktivieren von Schlüsseln

Das Konfigurieren eines kundenseitig verwalteten Schlüssels für Azure Data Box ist optional. Standardmäßig verwendet Data Box einen von Microsoft verwalteten Schlüssel zum Schützen des BitLocker-Schlüssels. Um einen vom Kunden verwalteten Schlüssel im Azure-Portal zu aktivieren, gehen Sie folgendermaßen vor:

1. Navigieren Sie zum Blatt **Übersicht** für Ihren Data Box-Auftrag.

    ![Blatt „Übersicht“ des Data Box-Auftrags](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-1.png)

2. Navigieren Sie zu **Einstellungen > Verschlüsselung**. Unter **Verschlüsselungstyp** können Sie auswählen, wie Sie den Schlüssel für die Geräteentsperrung schützen möchten. Standardmäßig wird ein von Microsoft verwalteter Schlüssel verwendet, um Kennwort zum Entsperren des Geräts zu schützen. 

    ![Auswählen der Verschlüsselungsoption](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-2.png)

3. Wählen Sie den Verschlüsselungstyp als **Kundenseitig verwalteter Schlüssel** aus. Nachdem Sie den kundenseitig verwalteten Schlüssel ausgewählt haben, **wählen Sie einen Schlüsseltresor und einen Schlüssel aus**.

    ![Auswählen des kundenseitig verwalteten Schlüssels](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-3.png)

4. Auf dem Blatt **Schlüssel aus Azure Key Vault auswählen** wird das Abonnementfeld automatisch aufgefüllt. Für **Schlüsseltresor** können Sie einen vorhandenen Schlüsseltresor aus der Dropdownliste auswählen.

    ![Auswählen einer vorhandenen Azure Key Vault-Instanz](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-3-a.png)

    Sie können auch **Neu erstellen** auswählen, um einen neuen Schlüsseltresor zu erstellen. Geben Sie auf dem Blatt **Schlüsseltresor erstellen** die Ressourcengruppe und den Namen des Schlüsseltresors ein. Stellen Sie sicher, dass **Vorläufiges Löschen** und **Löschschutz** aktiviert sind. Behalten Sie alle anderen Standardeinstellungen bei. Klicken Sie auf **Überprüfen + erstellen**.

    ![Überprüfen und Erstellen einer Azure Key Vault-Instanz](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-4.png)

5. Überprüfen Sie die Informationen im Zusammenhang mit Ihrem Schlüsseltresor, und wählen Sie **Erstellen** aus. Warten Sie einige Minuten, bis die Erstellung des Schlüsseltresors abgeschlossen ist.

    ![Erstellen einer Azure Key Vault-Instanz mit Ihren Einstellungen](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-5.png)

6. Unter **Schlüssel aus Azure Key Vault auswählen** können Sie einen Schlüssel im vorhandenen Schlüsseltresor auswählen.

    ![Auswählen des Schlüssels aus Azure Key Vault](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-6.png)

7. Wenn Sie einen neuen Schlüssel erstellen möchten, wählen Sie **Neu erstellen** aus, um einen Schlüssel zu erstellen. Der RSA-Schlüssel kann eine Größe von 2048 oder mehr haben.

    ![Erstellen eines neuen Schlüssels in Azure Key Vault](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-6-a.png)

8. Geben Sie den Namen für Ihren Schlüssel an, behalten Sie ansonsten die Standardeinstellungen bei, und wählen Sie **Erstellen** aus.

    ![Benennen eines neuen Schlüssels](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-7.png)


9. Sie werden benachrichtigt, dass ein Schlüssel in Ihrem Schlüsseltresor erstellt wurde. Wählen Sie die **Version** und dann **Auswählen** aus.

    ![Auswählen der Version für einen neuen Schlüssel](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-8.png)

10. Im Bereich **Verschlüsselungstyp** werden der Schlüsseltresor und der Schlüssel angezeigt, der als kundenseitig verwalteter Schlüssel ausgewählt wurde.

    ![Schlüssel und Schlüsseltresor für kundenseitig verwalteten Schlüssel](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-9.png)

11. Speichern Sie den Schlüssel. 

    ![Speichern des kundenseitig verwalteten Schlüssels](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-10.png)

    Die Schlüssel-URL wird unter **Verschlüsselungstyp** angezeigt.

    ![URL des Kundenseitig verwalteten Schlüssels](./media/data-box-customer-managed-encryption-key-portal/customer-managed-key-11.png)

> [!IMPORTANT]
> Sie können in jeder Phase des Data Box-Auftrags den von Microsoft verwaltete Schlüssel deaktivieren und zu kundenseitig verwalteten Schlüsseln wechseln. Nachdem Sie den kundenseitig verwalteten Schlüssel erstellt haben, können Sie jedoch nicht wieder zurück zum von Microsoft verwalteten Schlüssel wechseln.

## <a name="troubleshoot-errors"></a>Beheben von Fehlern

Wenn Ihnen Fehler im Zusammenhang mit dem kundenseitig verwalteten Schlüssel angezeigt werden, verwenden Sie zur Problembehandlung die folgende Tabelle.

| Fehlercode| Fehlerdetails| Wiederherstellbar?|
|-------------|--------------|---------|
| SsemUserErrorEncryptionKeyDisabled| Der Hauptschlüssel konnte nicht abgerufen werden, weil der kundenseitig verwaltete Schlüssel deaktiviert ist.| Ja, durch Aktivieren der Schlüsselversion.|
| SsemUserErrorEncryptionKeyExpired| Der Hauptschlüssel konnte nicht abgerufen werden, weil der kundenseitig verwaltete Schlüssel abgelaufen ist.| Ja, durch Aktivieren der Schlüsselversion.|
| SsemUserErrorKeyDetailsNotFound| Der Hauptschlüssel konnte nicht abgerufen werden, weil der kundenseitig verwaltete Schlüssel nicht gefunden wurde.| Wenn Sie den Schlüsseltresor gelöscht haben, können Sie den kundenseitig verwalteten Schlüssel nicht wiederherstellen.  Wenn Sie den Schlüsseltresor zu einem anderen Mandanten migriert haben, finden Sie weitere Informationen unter [Ändern der Mandanten-ID des Schlüsseltresors nach einer Abonnementverschiebung](../key-vault/general/move-subscription.md). Wenn Sie den Schlüsseltresor gelöscht haben:<ol><li>Ja, wenn Sie sich noch innerhalb des Zeitraums für den Schutz vor dem endgültigen Löschen befinden, können Sie die Schritte unter [Wiederherstellen eines Schlüsseltresors](../key-vault/general/soft-delete-powershell.md#recovering-a-key-vault) ausführen.</li><li>Nein, wenn der Zeitraum für den Schutz vor dem endgültigen Löschen abgelaufen ist.</li></ol><br>Andernfalls ja: Wenn der Schlüsseltresor eine Mandantenmigration durchlaufen hat, kann er mit einem der folgenden Schritte wiederhergestellt werden: <ol><li>Stellen Sie den Schlüsseltresor im alten Mandanten wieder her.</li><li>Legen Sie `Identity = None` fest, und setzen Sie dann den Wert auf `Identity = SystemAssigned` zurück. Dadurch wird die Identität gelöscht und neu erstellt, sobald die neue Identität erstellt wurde. Gewähren Sie der neuen Identität in der Zugriffsrichtlinie des Schlüsseltresors die Berechtigungen `Get`, `Wrap` und `Unwrap`.</li></ol> |
| SsemUserErrorKeyVaultBadRequestException| Der Hauptschlüssel konnte nicht abgerufen werden, weil der Zugriff auf den kundenseitig verwalteten Schlüssel widerrufen wurde.| Ja. Überprüfen Sie Folgendes: <ol><li>Der Schlüsseltresor verfügt weiterhin über die MSI in der Zugriffsrichtlinie.</li><li>Die Zugriffsrichtlinie bietet Berechtigungen zum Abrufen, Packen und Entpacken.</li><li>Wenn sich der Schlüsseltresor in einem VNET hinter der Firewall befindet, überprüfen Sie, ob **Vertrauenswürdige Microsoft-Dienste zulassen** aktiviert ist.</li></ol>|
| SsemUserErrorKeyVaultDetailsNotFound| Der Hauptschlüssel konnte nicht abgerufen werden, weil der dem kundenseitig verwalteten Schlüssel zugeordnete Schlüsseltresor nicht gefunden wurde. | Wenn Sie den Schlüsseltresor gelöscht haben, können Sie den kundenseitig verwalteten Schlüssel nicht wiederherstellen.  Wenn Sie den Schlüsseltresor zu einem anderen Mandanten migriert haben, finden Sie weitere Informationen unter [Ändern der Mandanten-ID des Schlüsseltresors nach einer Abonnementverschiebung](../key-vault/general/move-subscription.md). Wenn Sie den Schlüsseltresor gelöscht haben:<ol><li>Ja, wenn Sie sich noch innerhalb des Zeitraums für den Schutz vor dem endgültigen Löschen befinden, können Sie die Schritte unter [Wiederherstellen eines Schlüsseltresors](../key-vault/general/soft-delete-powershell.md#recovering-a-key-vault) ausführen.</li><li>Nein, wenn der Zeitraum für den Schutz vor dem endgültigen Löschen abgelaufen ist.</li></ol><br>Andernfalls ja: Wenn der Schlüsseltresor eine Mandantenmigration durchlaufen hat, kann er mit einem der folgenden Schritte wiederhergestellt werden: <ol><li>Stellen Sie den Schlüsseltresor im alten Mandanten wieder her.</li><li>Legen Sie `Identity = None` fest, und setzen Sie dann den Wert auf `Identity = SystemAssigned` zurück. Dadurch wird die Identität gelöscht und neu erstellt, sobald die neue Identität erstellt wurde. Gewähren Sie der neuen Identität in der Zugriffsrichtlinie des Schlüsseltresors die Berechtigungen `Get`, `Wrap` und `Unwrap`.</li></ol> |
| SsemUserErrorSystemAssignedIdentityAbsent  | Der Hauptschlüssel konnte nicht abgerufen werden, weil der kundenseitig verwaltete Schlüssel nicht gefunden wurde.| Ja. Überprüfen Sie Folgendes: <ol><li>Der Schlüsseltresor verfügt weiterhin über die MSI in der Zugriffsrichtlinie.</li><li>Die Identität ist vom Typ „Vom System zugewiesen“.</li><li>Gewähren Sie der neuen Identität in der Zugriffsrichtlinie des Schlüsseltresors die Berechtigungen „Get“, „Wrap“ und „Unwrap“.</li></ol>|
| Allgemeiner Fehler  | Der Hauptschlüssel konnte nicht abgerufen werden.| Hierbei handelt es sich um einen generischen Fehler. Wenden Sie sich an den Microsoft-Support, um den Fehler zu beheben, und ermitteln Sie die nächsten Schritte.|


## <a name="next-steps"></a>Nächste Schritte

- [What is Azure Key Vault? (Was ist Azure Key Vault?)](../key-vault/general/overview.md)
