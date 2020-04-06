---
title: Verschlüsseln Ihrer Anwendungsquelle im Ruhezustand
description: Verschlüsseln Sie Ihre Anwendungsdaten in Azure Storage, und stellen Sie sie als Paketdatei bereit.
ms.topic: article
ms.date: 03/06/2020
ms.openlocfilehash: 62179e900ace0d6d7b8b1f07e8f0ab685508f991
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79408723"
---
# <a name="encryption-at-rest-using-customer-managed-keys"></a>Verschlüsselung im Ruhezustand mithilfe von kundenseitig verwalteten Schlüsseln

Zum Verschlüsseln der Anwendungsdaten Ihrer Funktions-App im Ruhezustand sind ein Azure Storage-Konto und eine Azure Key Vault-Instanz erforderlich. Diese Dienste werden verwendet, wenn Sie die App aus einem Bereitstellungspaket ausführen.

  - [Azure Storage ermöglicht die Verschlüsselung ruhender Daten](../storage/common/storage-service-encryption.md). Sie können vom System bereitgestellte Schlüssel oder eigene, kundenseitig verwaltete Schlüssel verwenden. Dort werden Ihre Anwendungsdaten gespeichert, wenn sie nicht in einer Funktions-App in Azure ausgeführt werden.
  - [Ausführung aus einem Bereitstellungspaket] ((run-functions-from-deployment-package.md) ist ein Bereitstellungsfeature von App Service. Es ermöglicht Ihnen, Ihre Websiteinhalte mithilfe einer SAS-URL (Shared Access Signature) über ein Azure Storage-Konto bereitzustellen.
  - [Key Vault-Verweise](../app-service/app-service-key-vault-references.md) sind ein Sicherheitsfeature von App Service. Es ermöglicht Ihnen, Geheimnisse zur Laufzeit als Anwendungseinstellungen zu importieren. Verwenden Sie dies zum Verschlüsseln der SAS-URL Ihres Azure Storage-Kontos.

## <a name="set-up-encryption-at-rest"></a>Einrichten der Verschlüsselung ruhender Daten

### <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

[Erstellen Sie zunächst ein Azure Storage-Konto](../storage/common/storage-account-create.md), und [verschlüsseln Sie es mit kundenseitig verwalteten Schlüsseln](../storage/common/encryption-customer-managed-keys.md). Nachdem das Konto erstellt wurde, laden Sie die Paketdateien mit dem [Azure Storage-Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) hoch.

Verwenden Sie als nächstes den Storage-Explorer, um [eine SAS zu generieren](../vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=windows#generate-a-sas-in-storage-explorer). 

> [!NOTE]
> Speichern Sie diese SAS-URL. Sie wird später verwendet, um zur Laufzeit sicheren Zugriff auf das Bereitstellungspaket zu aktivieren.

### <a name="configure-running-from-a-package-from-your-storage-account"></a>Konfigurieren der Ausführung aus einem Paket aus Ihrem Speicherkonto
  
Wenn Sie Ihre Datei in Blobspeicher hochgeladen haben und über eine SAS-URL für die Datei verfügen, können Sie die Anwendungseinstellung `WEBSITE_RUN_FROM_PACKAGE` auf die SAS-URL festlegen. Im folgenden Beispiel wird dazu die Azure-Befehlszeilenschnittstelle verwendet:

```
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_RUN_FROM_PACKAGE="<your-SAS-URL>"
```

Wenn Sie diese Anwendungseinstellung hinzufügen, wird Ihre Funktions-App neu gestartet. Nachdem die App neu gestartet wurde, navigieren Sie zu der App, und stellen Sie sicher, dass die App ordnungsgemäß mit dem Bereitstellungspaket gestartet wurde. Wenn die Anwendung nicht ordnungsgemäß gestartet wurde, nutzen Sie die Informationen zur [Problembehandlung](run-functions-from-deployment-package.md#troubleshooting).

### <a name="encrypt-the-application-setting-using-key-vault-references"></a>Verschlüsseln der Anwendungseinstellung mithilfe von Key Vault-Verweisen

Nun können Sie den Wert der Anwendungseinstellung `WEBSITE_RUN_FROM_PACKAGE` durch einen Key Vault-Verweis auf die SAS-codierte URL ersetzen. Dadurch wird die SAS-URL in Key Vault verschlüsselt, was eine zusätzliche Sicherheitsebene bietet.

1. Erstellen Sie mit dem Befehl [`az keyvault create`](/cli/azure/keyvault#az-keyvault-create) eine Key Vault-Instanz.       

    ```azurecli    
    az keyvault create --name "Contoso-Vault" --resource-group <group-name> --location eastus    
    ```    

1. Befolgen Sie [diese Anweisungen, um Ihrer App Zugriff](../app-service/app-service-key-vault-references.md#granting-your-app-access-to-key-vault) auf Ihren Schlüsseltresor zu gewähren:

1. Fügen Sie mit dem Befehl [`az keyvault secret set`](/cli/azure/keyvault/secret#az-keyvault-secret-set) in Ihrem Schlüsseltresor Ihrer externen URL ein Geheimnis hinzu:   

    ```azurecli    
    az keyvault secret set --vault-name "Contoso-Vault" --name "external-url" --value "<SAS-URL>"    
    ```    

1.  Erstellen Sie mit dem Befehl [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) die `WEBSITE_RUN_FROM_PACKAGE`-Anwendungseinstellung mit dem Wert als Key Vault-Verweis auf die externe URL:

    ```azurecli    
    az webapp config appsettings set --settings WEBSITE_RUN_FROM_PACKAGE="@Microsoft.KeyVault(SecretUri=https://Contoso-Vault.vault.azure.net/secrets/external-url/<secret-version>"    
    ```

    Die `<secret-version>` befindet sich in der Ausgabe des vorherigen `az keyvault secret set`-Befehls.

Wenn Sie diese Anwendungseinstellung aktualisieren, wird Ihre Funktions-App neu gestartet. Nachdem die App neu gestartet wurde, navigieren Sie zu der App, und stellen Sie sicher, dass die App ordnungsgemäß mit dem Key Vault-Verweis gestartet wurde.

## <a name="how-to-rotate-the-access-token"></a>Rotieren des Zugriffstokens

Sie sollten den SAS-Schlüssel Ihres Speicherkontos regelmäßig rotieren. Um sicherzustellen, dass die Funktions-App nicht versehentlich den Zugriff verliert, müssen Sie auch die SAS-URL in Key Vault aktualisieren.

1. Rotieren Sie den SAS-Schlüssel, indem Sie im Azure-Portal zu Ihrem Speicherkonto navigieren. Klicken Sie unter **Einstellungen** > **Zugriffsschlüssel** auf das Symbol zum Rotieren des SAS-Schlüssels.

1. Kopieren Sie die neue SAS-URL, und legen Sie mit dem folgenden Befehl die aktualisierte SAS-URL in Ihrem Schlüsseltresor fest:

    ```azurecli    
    az keyvault secret set --vault-name "Contoso-Vault" --name "external-url" --value "<SAS-URL>"    
    ``` 

1. Aktualisieren Sie den Key Vault-Verweis in der Anwendungseinstellung auf die neue Geheimnisversion:

    ```azurecli    
    az webapp config appsettings set --settings WEBSITE_RUN_FROM_PACKAGE="@Microsoft.KeyVault(SecretUri=https://Contoso-Vault.vault.azure.net/secrets/external-url/<secret-version>"    
    ```

    Die `<secret-version>` befindet sich in der Ausgabe des vorherigen `az keyvault secret set`-Befehls.

## <a name="how-to-revoke-the-function-apps-data-access"></a>Widerrufen des Datenzugriffs der Funktions-App

Es gibt zwei Methoden, den Zugriff der Funktions-App auf das Speicherkonto zu widerrufen. 

### <a name="rotate-the-sas-key-for-the-azure-storage-account"></a>Rotieren des SAS-Schlüssels für das Azure Storage-Konto

Wenn der SAS-Schlüssel für das Speicherkonto rotiert wird, kann die Funktions-App nicht mehr auf das Speicherkonto zugreifen, wird jedoch weiterhin mit der zuletzt heruntergeladenen Version der Paketdatei ausgeführt. Starten Sie die Funktions-App neu, um die zuletzt heruntergeladene Version zu löschen.

### <a name="remove-the-function-apps-access-to-key-vault"></a>Entfernen des Zugriffs der Funktions-App auf Key Vault

Sie können den Zugriff der Funktions-App auf die Websitedaten widerrufen, indem Sie den Zugriff der Funktions-App auf Key Vault deaktivieren. Entfernen Sie hierzu die Zugriffsrichtlinie für die Identität der Funktions-App. Dies ist dieselbe Identität, die Sie zuvor beim Konfigurieren von Key Vault-Verweisen erstellt haben.

## <a name="summary"></a>Zusammenfassung

Ihre Anwendungsdateien sind jetzt im Ruhezustand in Ihrem Speicherkonto verschlüsselt. Wenn Ihre Funktions-App gestartet wird, ruft sie die SAS-URL aus Ihrem Schlüsseltresor ab. Zum Schluss lädt die Funktions-App die Anwendungsdateien aus dem Speicherkonto. 

Wenn Sie den Zugriff auf Ihr Speicherkonto für die Funktions-App widerrufen müssen, können Sie entweder den Zugriff auf den Schlüsseltresor widerrufen oder die Speicherkontoschlüssel rotieren, wodurch die SAS-URL ungültig wird.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="is-there-any-additional-charge-for-running-my-function-app-from-the-deployment-package"></a>Fällt eine zusätzliche Gebühr für das Ausführen meiner Funktions-App aus dem Bereitstellungspaket an?

Nur die Kosten im Zusammenhang mit dem Azure Storage-Konto und ggf. Gebühren für ausgehenden Datenverkehr.

### <a name="how-does-running-from-the-deployment-package-affect-my-function-app"></a>Wie wirkt sich die Ausführung des Bereitstellungspakets auf meine Funktions-App aus?

- Wenn Sie Ihre App aus dem Bereitstellungspaket ausführen, wird `wwwroot/` schreibgeschützt. Ihre App empfängt eine Fehlermeldung bei dem Versuch, in dieses Verzeichnis zu schreiben.
- Das TAR- und das GZIP-Format werden nicht unterstützt.
- Dieses Feature ist nicht mit lokalem Cache kompatibel.

## <a name="next-steps"></a>Nächste Schritte

- [Key Vault-Verweise für App Service](../app-service/app-service-key-vault-references.md)
- [Azure Storage encryption for data at rest (Azure Storage-Verschlüsselung für ruhende Daten)](../storage/common/storage-service-encryption.md)
