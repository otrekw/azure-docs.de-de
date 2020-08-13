---
title: 'Schnellstart: Python-Clientbibliothek in Azure Key Vault – Verwalten von Zertifikaten'
description: Erfahren Sie, wie Sie mithilfe der Python-Clientbibliothek Zertifikate in einer Azure Key Vault-Instanz erstellen, daraus abrufen und löschen.
author: msmbaldwin
ms.author: mbaldwin
ms.date: 3/30/2020
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.custom: devx-track-python
ms.openlocfilehash: 86ce2b95cce82bd6d6877f83c3bc0b0496804c83
ms.sourcegitcommit: dea88d5e28bd4bbd55f5303d7d58785fad5a341d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87875335"
---
# <a name="quickstart-azure-key-vault-certificates-client-library-for-python"></a>Schnellstart: Azure Key Vault-Zertifikatclientbibliothek für Python

Hier finden Sie Informationen zu den ersten Schritten mit der Azure Key Vault-Clientbibliothek für Python. Führen Sie die weiter unten beschriebenen Schritte aus, um das Paket zu installieren und den Beispielcode für grundlegende Aufgaben auszuprobieren.

Azure Key Vault unterstützt Sie dabei, kryptografische Schlüssel und Geheimnisse zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Die Key Vault-Clientbibliothek für Python ermöglicht Folgendes:

- Verbessern der Sicherheit und der Kontrolle über Schlüssel und Kennwörter
- Erstellen und Importieren von Verschlüsselungsschlüsseln in wenigen Minuten
- Verringern der Wartezeit durch Cloudskalierung und globale Redundanz
- Vereinfachen und Automatisieren von Aufgaben für TLS-/SSL-Zertifikate
- Verwenden FIPS 140-2 Level 2-zertifizierter HSMs

[API-Referenzdokumentation](/python/api/overview/azure/keyvault-certificates-readme?view=azure-python) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault) | [Paket (Python-Paketindex)](https://pypi.org/project/azure-keyvault/)

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement ([kostenloses Abonnement erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))
- Python 2.7, 3.5.3 oder höher
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) oder [Azure PowerShell](/powershell/azure/)

In diesem Schnellstart wird davon ausgegangen, dass Sie die [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) in einem Linux-Terminalfenster ausführen.

## <a name="setting-up"></a>Einrichten

### <a name="install-the-package"></a>Installieren des Pakets

Installieren Sie über das Konsolenfenster die Azure Key Vault-Zertifikatbibliothek für Python.

```console
pip install azure-keyvault-certificates
```

Im Rahmen dieses Schnellstarts muss außerdem das azure.identity-Paket installiert werden:

```console
pip install azure.identity
```

### <a name="create-a-resource-group-and-key-vault"></a>Erstellen einer Ressourcengruppe und eines Schlüsseltresors

In dieser Schnellstartanleitung wird eine vorab erstellte Azure Key Vault-Instanz verwendet. Eine Anleitung zum Erstellen eines Schlüsseltresors finden Sie unter [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe der Azure CLI](quick-create-cli.md), [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe von PowerShell](quick-create-powershell.md) oder [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe des Azure-Portals](quick-create-portal.md). Alternativ können Sie die unten angegebenen Azure CLI-Befehle ausführen.

> [!Important]
> Jeder Schlüsseltresor muss einen eindeutigen Namen haben. Ersetzen Sie in den folgenden Beispielen „<your-unique-keyvault-name>“ durch den Namen Ihres Schlüsseltresors.

```azurecli
az group create --name "myResourceGroup" -l "EastUS"

az keyvault create --name <your-unique-keyvault-name> -g "myResourceGroup"
```

### <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Eine cloudbasierte .NET-Anwendung kann am einfachsten mit einer verwalteten Identität authentifiziert werden. Einzelheiten hierzu finden Sie unter [Verwenden einer verwalteten App Service-Identität für den Zugriff auf Azure Key Vault](../general/managed-identity.md).

Der Einfachheit halber wird in dieser Schnellstartanleitung eine Desktopanwendung erstellt, die die Verwendung eines Dienstprinzipals und einer Zugriffssteuerungsrichtlinie erfordert. Ihr Dienstprinzipal muss einen eindeutigen Namen im Format „http://&lt;eindeutiger Dienstprinzipalname&gt;“ haben.

Erstellen Sie mithilfe des Azure CLI-Befehls [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) einen Dienstprinzipal:

```azurecli
az ad sp create-for-rbac -n "http://&lt;my-unique-service-principal-name&gt;" --sdk-auth
```

Daraufhin wird eine Reihe von Schlüssel-Wert-Paaren zurückgegeben. 

```console
{
  "clientId": "7da18cae-779c-41fc-992e-0527854c6583",
  "clientSecret": "b421b443-1669-4cd7-b5b1-394d5c945002",
  "subscriptionId": "443e30da-feca-47c4-b68f-1636b75e16b3",
  "tenantId": "35ad10f1-7799-4766-9acf-f2d946161b77",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

Notieren Sie sich die Client-ID (clientId) und das Clientgeheimnis (clientSecret). Sie werden weiter unten im Schritt [Festlegen der Umgebungsvariable](#set-environmental-variables) benötigt.

#### <a name="give-the-service-principal-access-to-your-key-vault"></a>Gewähren des Zugriffs auf Ihren Schlüsseltresor für den Dienstprinzipal

Erstellen Sie eine Zugriffsrichtlinie für Ihren Schlüsseltresor, die dem Dienstprinzipal Berechtigungen erteilt, indem Sie die Client-ID an den Befehl [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) übergeben. Erteilen Sie dem Dienstprinzipal Berechtigungen zum Abrufen, Auflisten und Erstellen von Zertifikaten.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --certificate-permissions delete get list create 
```

#### <a name="set-environmental-variables"></a>Festlegen von Umgebungsvariablen

Die DefaultAzureCredential-Methode in unserer Anwendung basiert auf drei Umgebungsvariablen: `AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET` und `AZURE_TENANT_ID`. Legen Sie diese Variablen auf die clientId-, clientSecret- und tenantId-Werte fest, die Sie sich im Schritt [Erstellen eines Dienstprinzipals](#create-a-service-principal) notiert haben. Verwenden Sie hierbei das Format `export VARNAME=VALUE`. (Mit dieser Methode werden nur die Variablen für Ihre aktuelle Shell und daraus erstellte Prozesse festgelegt. Um diese Variablen dauerhaft zu Ihrer Umgebung hinzuzufügen, bearbeiten Sie Ihre Datei `/etc/environment `.) 

Sie müssen außerdem den Namen Ihres Schlüsseltresors als Umgebungsvariable `KEY_VAULT_NAME` speichern.

```console
export AZURE_CLIENT_ID=<your-clientID>

export AZURE_CLIENT_SECRET=<your-clientSecret>

export AZURE_TENANT_ID=<your-tenantId>

export KEY_VAULT_NAME=<your-key-vault-name>
````

## <a name="object-model"></a>Objektmodell

Mit der Azure Key Vault-Clientbibliothek für Python können Sie Schlüssel und zugehörige Objekte wie z. B. Zertifikate und Geheimnisse verwalten. In den folgenden Codebeispielen wird gezeigt, wie Sie einen Client und ein Zertifikat erstellen sowie ein Zertifikat abrufen und löschen.

## <a name="code-examples"></a>Codebeispiele

### <a name="add-directives"></a>Hinzufügen von Anweisungen

Fügen Sie am Anfang Ihres Codes die folgenden Anweisungen hinzu:

```python
import os
from azure.keyvault.certificates import CertificateClient, CertificatePolicy,CertificateContentType, WellKnownIssuerNames 
from azure.identity import DefaultAzureCredential
```

### <a name="authenticate-and-create-a-client"></a>Authentifizieren und Erstellen eines Clients

Die Authentifizierung bei Ihrem Schlüsseltresor und die Erstellung eines Schlüsseltresorclients hängen von den Umgebungsvariablen im Schritt [Festlegen von Umgebungsvariablen](#set-environmental-variables) oben ab. Der Name Ihres Schlüsseltresors wird in den Schlüsseltresor-URI im Format „https://<Name-Ihres-Schlüsseltresors>.vault.azure.net“ erweitert.

```python
credential = DefaultAzureCredential()

client = CertificateClient(vault_url=KVUri, credential=credential)
```

### <a name="save-a-certificate"></a>Speichern eines Zertifikats

Nachdem Ihre Anwendung authentifiziert wurde, können Sie Ihrem Schlüsseltresor ein selbstsigniertes Zertifikat hinzufügen. 

```python
certificate_operation_poller = client.begin_create_certificate(
    certificate_name=certificateName, policy=CertificatePolicy.get_default()
)
certificate = certificate_operation_poller.result()
```

Mithilfe des Befehls [az keyvault certificate show](/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-show) können Sie sich vergewissern, dass das Zertifikat festgelegt wurde:

```azurecli
az keyvault certificate show --vault-name <your-unique-keyvault-name> --name myCertificate
```

### <a name="retrieve-a-certificate"></a>Abrufen eines Zertifikats

Sie können das zuvor erstellte Zertifikat jetzt abrufen.

```python
retrieved_certificate = client.get_certificate(certificateName)
 ```

Ihr Zertifikat ist jetzt als `retrieved_certificate` gespeichert.

### <a name="delete-a-certificate"></a>Löschen eines Zertifikats

Löschen Sie abschließend das Zertifikat aus Ihrem Schlüsseltresor.

```python
client.delete_certificate(certificateName)
```

Mithilfe des Befehls [az keyvault certificate show](/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-show) können Sie sich vergewissern, dass das Zertifikat gelöscht wurde:

```azurecli
az keyvault certifcate show --vault-name <your-unique-keyvault-name> --name myCertificate
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen nicht mehr benötigen, können Sie Azure CLI oder Azure PowerShell verwenden, um Ihren Schlüsseltresor und die zugehörige Ressourcengruppe zu entfernen.

```azurecli
az group delete -g "myResourceGroup"
```

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="sample-code"></a>Beispielcode

```python
import os
from azure.keyvault.certificates import CertificateClient, CertificatePolicy,CertificateContentType, WellKnownIssuerNames 
from azure.identity import DefaultAzureCredential

keyVaultName = os.environ["KEY_VAULT_NAME"]
KVUri = "https://" + keyVaultName + ".vault.azure.net"

credential = DefaultAzureCredential()
client = CertificateClient(vault_url=KVUri, credential=credential)

certificateName = "myCertificate"

print("Creating a certificate in " + keyVaultName + " called '" + certificateName  + "` ...")

certificate_operation_poller = client.begin_create_certificate(
    certificate_name=certificateName, policy=CertificatePolicy.get_default()

certificate = certificate_operation_poller.result()

print(" done.")

print("Retrieving your certificate from " + keyVaultName + ".")

retrieved_certificate = client.get_certificate(certificateName)

print("Certificate with name '{0}' was found'.".format(retrieved_certificate.name))
print("Deleting your certificate from " + keyVaultName + " ...")

client.delete_certificate(certificateName)

print(" done.")
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen Schlüsseltresor erstellt, ein Zertifikat gespeichert und dieses Zertifikat abgerufen. Weitere Informationen zu Key Vault und zur Integration in Ihre Anwendungen finden Sie in den folgenden Artikeln:

- [Was ist der Azure-Schlüsseltresor?](../general/overview.md)
- [Entwicklerhandbuch zu Azure-Schlüsseltresor](../general/developers-guide.md)
- [Bewährte Methoden zum Verwenden von Key Vault](../general/best-practices.md)
