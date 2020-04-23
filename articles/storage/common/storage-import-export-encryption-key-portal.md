---
title: Verwenden des Azure-Portals zum Konfigurieren von kundenseitig verwalteten Schlüsseln für den Import/Export-Dienst
description: Erfahren Sie, wie Sie über das Azure-Portal kundenseitig verwaltete Schlüssel mit Azure Key Vault für den Azure Import/Export-Dienst konfigurieren. Mit von Kunden verwalteten Schlüsseln können Sie Zugriffssteuerungen erstellen, rotieren, deaktivieren und widerrufen.
services: storage
author: alkohli
ms.service: storage
ms.topic: how-to
ms.date: 03/12/2020
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: ddcb47bfe8ba2b77efd8ff0aed52f1412107f0c5
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81456497"
---
# <a name="use-customer-managed-keys-in-azure-key-vault-for-importexport-service"></a>Verwenden kundenseitig verwalteter Schlüssel in Azure Key Vault für den Import/Export-Dienst

Azure Import/Export schützt die BitLocker-Schlüssel, die zum Sperren der Laufwerke verwendet werden, mit einem Verschlüsselungsschlüssel. Standardmäßig werden BitLocker-Schlüssel mit von Microsoft verwalteten Schlüsseln verschlüsselt. Um zusätzliche Kontrolle über die Verschlüsselungsschlüssel zu erhalten, können Sie auch kundenseitig verwaltete Schlüssel bereitstellen.

Kundenseitig verwaltete Schlüssel müssen in Azure Key Vault erstellt und gespeichert werden. Weitere Informationen zum Azure-Schlüsseltresor finden Sie unter [Was ist der Azure-Schlüsseltresor?](../../key-vault/general/overview.md)

In diesem Artikel wird gezeigt, wie Sie kundenseitig verwaltete Schlüssel mit dem Import/Export-Dienst im [Azure-Portal](https://portal.azure.com/) verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

1. Sie haben einen Import- oder Exportauftrag gemäß den Anleitungen in folgenden Artikeln erstellt:

    - [Erstellen eines Importauftrags für Blobs](storage-import-export-data-to-blobs.md)
    - [Erstellen eines Importauftrags für Dateien](storage-import-export-data-to-files.md)
    - [Erstellen eines Exportauftrags für Blobs](storage-import-export-data-from-blobs.md)

2. Sie verfügen über eine vorhandene Azure Key Vault-Instanz mit einem Schlüssel, den Sie zum Schützen Ihres BitLocker-Schlüssels verwenden können. Informationen zum Erstellen eines Schlüsseltresors über das Azure-Portal finden Sie unter [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe des Azure-Portals](../../key-vault/secrets/quick-create-portal.md).

    - Für Ihre vorhandene Key Vault-Instanz ist **Vorläufiges Löschen** und **Nicht bereinigen** festgelegt. Diese Eigenschaften sind standardmäßig nicht aktiviert. Informationen zum Aktivieren dieser Eigenschaften finden Sie in den Abschnitten **Aktivieren des vorläufigen Löschens** und **Aktivieren des Bereinigungsschutzes** in einem der folgenden Artikel:

        - [Verwenden des vorläufigen Löschens mit PowerShell](../../key-vault/general/soft-delete-powershell.md)
        - [Verwenden des vorläufigen Löschens mit der CLI](../../key-vault/general/soft-delete-cli.md)
    - Der vorhandene Schlüsseltresor muss über einen RSA-Schlüssel mit einer Größe von mindestens 2048 verfügen. Weitere Informationen zu Schlüsseln finden Sie unter **Key Vault-Schlüssel** in [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten in Azure Key Vault](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys).
    - Der Schlüsseltresor muss sich in derselben Region wie das Speicherkonto für Ihre Daten befinden.  
    - Wenn Sie noch nicht über eine Azure Key Vault-Instanz verfügen, können Sie diese auch inline erstellen, wie im folgenden Abschnitt beschrieben.

## <a name="enable-keys"></a>Aktivieren von Schlüsseln

Das Konfigurieren eines kundenseitig verwalteten Schlüssels für den Import/Export-Dienst ist optional. Standardmäßig verwendet der Import/Export-Dienst einen von Microsoft verwalteten Schlüssel zum Schützen des BitLocker-Schlüssels. Um vom Kunden verwaltete Schlüssel im Azure-Portal zu aktivieren, gehen Sie folgendermaßen vor:

1. Navigieren Sie zum Blatt **Übersicht** für Ihren Importauftrag.
2. Wählen Sie im rechten Bereich die Option **Wählen Sie aus, wie Ihre BitLocker-Schlüssel verschlüsselt werden**.

    ![Auswählen der Verschlüsselungsoption](./media/storage-import-export-encryption-key-portal/encryption-key-1.png)

3. Auf dem Blatt **Verschlüsselung** können Sie den BitLocker-Schlüssel des Geräts anzeigen und kopieren. Unter **Verschlüsselungstyp** können Sie auswählen, wie Sie den BitLocker-Schlüssel schützen möchten. Standardmäßig wird ein von Microsoft verwalteter Schlüssel verwendet.

    ![Anzeigen des BitLocker-Schlüssels](./media/storage-import-export-encryption-key-portal/encryption-key-2.png)

4. Sie haben die Möglichkeit, einen kundenseitig verwalteten Schlüssel anzugeben. Nachdem Sie den kundenseitig verwalteten Schlüssel ausgewählt haben, wählen Sie die Option **Schlüsseltresor und Schlüssel auswählen** aus.

    ![Auswählen eines kundenseitig verwalteten Schlüssels](./media/storage-import-export-encryption-key-portal/encryption-key-3.png)

5. Auf dem Blatt **Schlüssel aus Azure Key Vault auswählen** wird das Abonnementfeld automatisch aufgefüllt. Für **Schlüsseltresor** können Sie einen vorhandenen Schlüsseltresor aus der Dropdownliste auswählen.

    ![Auswählen oder Erstellen einer Azure Key Vault-Instanz](./media/storage-import-export-encryption-key-portal/encryption-key-4.png)

6. Sie können auch **Neu erstellen** auswählen, um einen neuen Schlüsseltresor zu erstellen. Geben Sie auf dem Blatt **Schlüsseltresor erstellen** die Ressourcengruppe und den Namen des Schlüsseltresors ein. Behalten Sie alle anderen Standardeinstellungen bei. Klicken Sie auf **Überprüfen + erstellen**.

    ![Erstellen einer neuen Azure Key Vault-Instanz](./media/storage-import-export-encryption-key-portal/encryption-key-5.png)

7. Überprüfen Sie die Informationen im Zusammenhang mit Ihrem Schlüsseltresor, und wählen Sie **Erstellen** aus. Warten Sie einige Minuten, bis die Erstellung des Schlüsseltresors abgeschlossen ist.

    ![Azure Key Vault erstellen](./media/storage-import-export-encryption-key-portal/encryption-key-6.png)

8. Unter **Schlüssel aus Azure Key Vault auswählen** können Sie einen Schlüssel im vorhandenen Schlüsseltresor auswählen.

9. Wenn Sie einen neuen Schlüsseltresor erstellt haben, wählen Sie **Neu erstellen** aus, um einen Schlüssel zu erstellen. Der RSA-Schlüssel kann eine Größe von 2048 oder mehr haben.

    ![Erstellen eines neuen Schlüssels in Azure Key Vault](./media/storage-import-export-encryption-key-portal/encryption-key-7.png)

    Wenn das vorläufige Löschen und der Bereinigungsschutz beim Erstellen des Schlüsseltresors nicht aktiviert wurden, wird der Schlüsseltresor so aktualisiert, dass vorläufiges Löschen und Bereinigungsschutz aktiviert sind.

10. Geben Sie den Namen für Ihren Schlüssel an, behalten Sie ansonsten die Standardeinstellungen bei, und wählen Sie **Erstellen** aus.

    ![Erstellen eines neuen Schlüssels](./media/storage-import-export-encryption-key-portal/encryption-key-8.png)

11. Wählen Sie die **Version** und dann **Auswählen** aus. Sie werden benachrichtigt, dass ein Schlüssel in Ihrem Schlüsseltresor erstellt wurde.

    ![Neu erstellter Schlüssel im Schlüsseltresor](./media/storage-import-export-encryption-key-portal/encryption-key-9.png)

Auf dem Blatt **Verschlüsselung** werden der Schlüsseltresor und der Schlüssel angezeigt, der als kundenseitig verwalteter Schlüssel ausgewählt wurde.

## <a name="disable-keys"></a>Deaktivieren von Schlüsseln

Sie können in jeder Phase des Import-/Exportauftrags lediglich von Microsoft verwaltete Schlüssel deaktivieren und zu kundenseitig verwalteten Schlüsseln wechseln. Den kundenseitig verwalteten Schlüssel können Sie nach der Erstellung jedoch nicht deaktivieren.

## <a name="troubleshoot-customer-managed-key-errors"></a>Beheben von Fehlern beim kundenseitig verwalteten Schlüssel

Wenn Ihnen Fehler im Zusammenhang mit dem kundenseitig verwalteten Schlüssel angezeigt werden, verwenden Sie zur Problembehandlung die folgende Tabelle:

| Fehlercode     |Details     | Wiederherstellbar?    |
|----------------|------------|-----------------|
| CmkErrorAccessRevoked | Es wurde ein kundenseitig verwalteter Schlüssel angewendet, aber der Schlüsselzugriff wurde widerrufen. Weitere Informationen finden Sie unter [Aktivieren des Schlüsselzugriffs](https://docs.microsoft.com/rest/api/keyvault/vaults/updateaccesspolicy).                                                      | Ja. Überprüfen Sie Folgendes: <ol><li>Der Schlüsseltresor verfügt weiterhin über die MSI in der Zugriffsrichtlinie.</li><li>Die Zugriffsrichtlinie bietet Berechtigungen zum Abrufen, Packen und Entpacken.</li><li>Wenn sich der Schlüsseltresor in einem VNET hinter der Firewall befindet, überprüfen Sie, ob **Vertrauenswürdige Microsoft-Dienste zulassen** aktiviert ist.</li></ol>                                                                                            |
| CmkErrorDisabled      | Es wurde ein kundenseitig verwalteter Schlüssel angewendet, aber der Schlüssel ist deaktiviert. Weitere Informationen finden Sie unter [Aktivieren des Schlüssels](https://docs.microsoft.com/rest/api/keyvault/vaults/createorupdate).                                                                             | Ja, durch Aktivieren der Schlüsselversion     |
| CmkErrorNotFound      | Es wurde ein kundenseitig verwalteter Schlüssel angewendet, aber der Schlüssel wurde nicht gefunden. <br>Wenn der Schlüssel nach Ablauf der Aufbewahrungsdauer gelöscht und bereinigt wird, können Sie den Schlüssel nicht wiederherstellen. Wenn Sie den Schlüssel gesichert haben, können Sie den Schlüssel wiederherstellen, um dieses Problem zu lösen. | Nein. Der Schlüssel wurde gelöscht und nach Ablauf der Aufbewahrungsdauer außerdem bereinigt. <br>Ja, aber nur wenn der Kunde den Schlüssel gesichert hat und ihn wiederherstellt.  |
| CmkErrorVaultNotFound | Es wurde ein kundenseitig verwalteter Schlüssel angewendet, aber der dem Schlüssel zugeordnete Schlüsseltresor wurde nicht gefunden.<br>Wenn Sie den Schlüsseltresor gelöscht haben, können Sie den kundenseitig verwalteten Schlüssel nicht wiederherstellen.  Wenn Sie den Schlüsseltresor zu einem anderen Mandanten migriert haben, finden Sie weitere Informationen unter [Ändern der Mandanten-ID des Schlüsseltresors nach einer Abonnementverschiebung](https://docs.microsoft.com/azure/key-vault/key-vault-subscription-move-fix). |   Nein, wenn der Kunde den Schlüsseltresor gelöscht hat.<br> Ja, wenn der Schlüsseltresor eine Mandantenmigration durchlaufen hat. Führen Sie in diesem Fall einen der folgenden Schritte aus: <ol><li>Verschieben Sie den Schlüsseltresor zurück zum alten Mandanten.</li><li>Legen Sie „Identity = None“ und dann wieder „Identity = SystemAssigned“ fest. Dadurch wird die Identität gelöscht und neu erstellt.</li></ol>|

## <a name="next-steps"></a>Nächste Schritte

- [What is Azure Key Vault? (Was ist Azure Key Vault?)](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
