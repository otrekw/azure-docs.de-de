---
title: Exportieren eines Zertifikats aus Azure Key Vault
description: Exportieren eines Zertifikats aus Azure Key Vault
services: key-vault
author: sebansal
tags: azure-key-vault
ms.service: key-vault
ms.subservice: certificates
ms.topic: how-to
ms.custom: mvc, devx-track-azurecli
ms.date: 08/11/2020
ms.author: sebansal
ms.openlocfilehash: afab65b22d9487f30da458346bf143a557bec0d8
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88588891"
---
# <a name="export-certificate-from-azure-key-vault"></a>Exportieren eines Zertifikats aus Azure Key Vault

Azure Key Vault ermöglicht das einfache Bereitstellen und Verwalten von digitalen Zertifikaten für Ihr Netzwerk und das Aktivieren der sicheren Kommunikation für Anwendungen. Weitere allgemeine Informationen zu Zertifikaten finden Sie unter [Azure Key Vault-Zertifikate](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates).

## <a name="about-azure-key-vault-certificate"></a>Informationen zum Azure Key Vault-Zertifikat

### <a name="composition-of-certificate"></a>Zusammensetzung des Zertifikats
Wenn ein Key Vault-Zertifikat erstellt wird, werden auch ein adressierbarer Schlüssel und ein Geheimnis gleichen Namens erstellt. Der Key Vault-Schlüssel ermöglicht Schlüsselvorgänge, und das Key Vault-Geheimnis ermöglicht den Abruf des Zertifikatwerts als Geheimnis. Ein Key Vault-Zertifikat enthält auch öffentliche X509-Zertifikatmetadaten. [hier](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates#composition-of-a-certificate)

### <a name="exportable-or-non-exportable-keys"></a>Exportierbare oder nicht exportierbare Schlüssel
Wenn ein Key Vault-Zertifikat erstellt wird, kann es aus dem adressierbaren Geheimnis mit dem privaten Schlüssel entweder im PFX- oder PEM-Format abgerufen werden. Die zum Erstellen des Zertifikats verwendete Richtlinie muss angeben, dass der Schlüssel exportierbar ist. Wenn die Richtlinie angibt, dass der Schlüssel nicht exportierbar ist, ist der private Schlüssel beim Abruf als Geheimnis kein Teil des Werts.

Zwei Typen des Schlüssels werden unterstützt – RSA oder RSA-HSM mit Zertifikaten. Die Exportierbarkeit ist nur mit RSA zulässig, sie wird nicht von RSA-HSM unterstützt. [hier](https://docs.microsoft.com/azure/key-vault/certificates/about-certificates#exportable-or-non-exportable-key)

Das in Azure Key Vault gespeicherte Zertifikat kann per Azure CLI, PowerShell oder über das Portal exportiert werden.

> [!NOTE]
> Es ist wichtig zu beachten, dass Sie nur dann ein Kennwort für ein Zertifikat benötigen, wenn Sie es in den Schlüsseltresor importieren. Das zugehörige Kennwort wird von Key Vault nicht gespeichert. Daher ist das Kennwort beim Exportieren des Zertifikats leer.

## <a name="exporting-certificate-using-cli"></a>Exportieren eines Zertifikats mit der CLI
Mit dem folgenden Befehl können Sie den **öffentlichen Teil** eines Key Vault-Zertifikats herunterladen.

```azurecli
az keyvault certificate download --file
                                 [--encoding {DER, PEM}]
                                 [--id]
                                 [--name]
                                 [--subscription]
                                 [--vault-name]
                                 [--version]
```
Beispiele und Parameterdefinitionen finden Sie [hier](https://docs.microsoft.com/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-download).



Falls Sie das gesamte Zertifikat herunterladen möchten, d. h. **sowohl den öffentlichen als auch den privaten Teil der Zusammensetzung**, können Sie den Download des Zertifikats als Geheimnis durchführen.

```azurecli
az keyvault secret download –file {nameofcert.pfx}
                            [--encoding {ascii, base64, hex, utf-16be, utf-16le, utf-8}]
                            [--id]
                            [--name]
                            [--subscription]
                            [--vault-name]
                            [--version]
```
Parameterdefinitionen finden Sie [hier](https://docs.microsoft.com/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-download).


## <a name="exporting-certificate-using-powershell"></a>Exportieren des Zertifikats mit PowerShell

Mit diesem Befehl wird das Zertifikat mit dem Namen „TestCert01“ aus dem Schlüsseltresor mit dem Namen „ContosoKV01“ abgerufen. Führen Sie den folgenden Befehl aus, um das Zertifikat als PFX-Datei herunterzuladen. Mit diesen Befehlen wird auf die „SecretId“ zugegriffen, und anschließend wird der Inhalt als PFX-Datei gespeichert.

```azurepowershell
$cert = Get-AzKeyVaultCertificate -VaultName "ContosoKV01" -Name "TestCert01"
$kvSecret = Get-AzKeyVaultSecret -VaultName "ContosoKV01" -Name $Cert.Name
$kvSecretBytes = [System.Convert]::FromBase64String($kvSecret.SecretValueText)
$certCollection = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2Collection
$certCollection.Import($kvSecretBytes,$null,[System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable)
$password = '******'
$protectedCertificateBytes = $certCollection.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $password)
$pfxPath = [Environment]::GetFolderPath("Desktop") + "\MyCert.pfx"
[System.IO.File]::WriteAllBytes($pfxPath, $protectedCertificateBytes)
```
Die gesamte Zertifikatkette mit dem privaten Schlüssel wird exportiert, und das Zertifikat ist kennwortgeschützt.
Weitere Informationen zu ```Get-AzKeyVaultCertificate```-Befehlen und -Parametern finden Sie unter [Beispiel 2](https://docs.microsoft.com/powershell/module/az.keyvault/Get-AzKeyVaultCertificate?view=azps-4.4.0).

## <a name="exporting-certificate-using-portal"></a>Exportieren eines Zertifikats mit dem Portal

Wenn Sie im Portal auf dem Blatt „Zertifikat“ ein Zertifikat erstellen oder importieren, erhalten Sie die Benachrichtigung, dass die Erstellung des Zertifikats erfolgreich war. Beim Auswählen des Zertifikats können Sie auf die aktuelle Version klicken, um die Downloadoption anzuzeigen.


Sie können das Zertifikat herunterladen, indem Sie auf eine der Schaltflächen „Im CER-Format herunterladen“ oder „Im PFX-/PEM-Format herunterladen“ klicken.


![Zertifikatdownload](../media/certificates/quick-create-portal/current-version-shown.png)


## <a name="exporting-app-service-certificate-from-key-vault"></a>Exportieren eines Zertifikats aus dem Schlüsseltresor mit App Service Certificate

Über Azure App Service Certificate können Sie bequem SSL-Zertifikate erwerben und diese direkt im Portal den Azure-Apps zuweisen. Es ist auch möglich, diese Zertifikate über das Portal als PFX-Dateien zu exportieren, damit sie an anderen Orten genutzt werden können.
Nach dem Importieren können die App Service-Zertifikate **unter den Geheimnissen** angeordnet werden.

Die Schritte zum Exportieren von App Service-Zertifikaten finden Sie [hier](https://social.technet.microsoft.com/wiki/contents/articles/37431.exporting-azure-app-service-certificates.aspx).

## <a name="read-more"></a>Weitere Informationen
* [Verschiedene Zertifikatsdateitypen und -definitionen](https://docs.microsoft.com/archive/blogs/kaushal/various-ssltls-certificate-file-typesextensions)