---
title: 'Schnellstart: Python-Clientbibliothek in Azure Key Vault – Verwalten von Zertifikaten'
description: Erfahren Sie, wie Sie mithilfe der Python-Clientbibliothek Zertifikate in einer Azure Key Vault-Instanz erstellen, daraus abrufen und löschen.
author: msmbaldwin
ms.author: mbaldwin
ms.date: 09/03/2020
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.custom: devx-track-python
ms.openlocfilehash: b9ff7397ad29ac681e21c32608ade9c6ce557c37
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "89488625"
---
# <a name="quickstart-azure-key-vault-certificates-client-library-for-python"></a>Schnellstart: Azure Key Vault-Zertifikatclientbibliothek für Python

Hier finden Sie Informationen zu den ersten Schritten mit der Azure Key Vault-Clientbibliothek für Python. Führen Sie die weiter unten beschriebenen Schritte aus, um das Paket zu installieren und den Beispielcode für grundlegende Aufgaben auszuprobieren. Wenn Sie Key Vault zum Speichern von Zertifikaten verwenden, vermeiden Sie das Speichern von Zertifikaten im Code, was die Sicherheit Ihrer App erhöht.

[API-Referenzdokumentation](/python/api/overview/azure/keyvault-certificates-readme?view=azure-python) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-certificates) | [Paket (Python-Paketindex)](https://pypi.org/project/azure-keyvault-certificates)

## <a name="set-up-your-local-environment"></a>Einrichten Ihrer lokalen Umgebung

[!INCLUDE [Set up your local environment](../../../includes/key-vault-python-qs-setup.md)]

7. Installieren Sie die Key Vault-Zertifikatbibliothek:

    ```terminal
    pip install azure-keyvault-certificates
    ```

## <a name="create-a-resource-group-and-key-vault"></a>Erstellen einer Ressourcengruppe und eines Schlüsseltresors

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-python-qs-rg-kv-creation.md)]

## <a name="give-the-service-principal-access-to-your-key-vault"></a>Gewähren des Zugriffs auf Ihren Schlüsseltresor für den Dienstprinzipal

Führen Sie den folgenden Befehl [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) aus, um den Dienstprinzipal für get-, list- und create-Vorgänge für Zertifikate zu autorisieren.

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az keyvault set-policy --name %KEY_VAULT_NAME% --spn %AZURE_CLIENT_ID% --resource-group KeyVault-PythonQS-rg --certificate-permissions delete get list create
```

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
az keyvault set-policy --name $KEY_VAULT_NAME --spn $AZURE_CLIENT_ID --resource-group KeyVault-PythonQS-rg --certificate-permissions delete get list create 
```

---

Dieser Befehl stützt sich auf die `KEY_VAULT_NAME`- und `AZURE_CLIENT_ID`-Umgebungsvariablen, die in den vorherigen Schritten erstellt wurden.

Weitere Informationen finden Sie unter [Zuweisen einer Zugriffsrichtlinie: CLI](../general/assign-access-policy-cli.md).

## <a name="create-the-sample-code"></a>Erstellen des Beispielcodes

Die Azure Key Vault-Clientbibliothek für Python ermöglicht Ihnen die Verwaltung von Zertifikaten und zugehörigen Ressourcen wie Geheimnissen und kryptographischen Schlüsseln. Im folgenden Codebeispiel wird gezeigt, wie Sie einen Client erstellen und ein Geheimnis festlegen, abrufen und löschen.

Erstellen Sie eine Datei mit dem Namen *kv_certificates.py*, die diesen Code enthält.

```python
import os
from azure.keyvault.certificates import CertificateClient, CertificatePolicy,CertificateContentType, WellKnownIssuerNames 
from azure.identity import DefaultAzureCredential

keyVaultName = os.environ["KEY_VAULT_NAME"]
KVUri = "https://" + keyVaultName + ".vault.azure.net"

credential = DefaultAzureCredential()
client = CertificateClient(vault_url=KVUri, credential=credential)

certificateName = input("Input a name for your certificate > ")

print(f"Creating a certificate in {keyVaultName} called '{certificateName}' ...")

policy = CertificatePolicy.get_default()
poller = client.begin_create_certificate(certificate_name=certificateName, policy=policy)
certificate = poller.result()

print(" done.")

print(f"Retrieving your certificate from {keyVaultName}.")

retrieved_certificate = client.get_certificate(certificateName)

print(f"Certificate with name '{retrieved_certificate.name}' was found'.")
print(f"Deleting your certificate from {keyVaultName} ...")

poller = client.begin_delete_certificate(certificateName)
deleted_certificate = poller.result()

print(" done.")
```

## <a name="run-the-code"></a>Ausführen des Codes

Stellen Sie sicher, dass sich der Code aus dem vorherigen Abschnitt in einer Datei namens *kv_certificates.py* befindet. Führen Sie den Code dann mithilfe des folgenden Befehls aus:

```terminal
python kv_certificates.py
```

- Wenn Berechtigungsfehler auftreten, stellen Sie sicher, dass Sie den [`az keyvault set-policy`-Befehl](#give-the-service-principal-access-to-your-key-vault) ausgeführt haben.
- Wenn Sie den Code mit dem gleichen Schlüsselnamen erneut ausführen, wird möglicherweise der Fehler „(Konflikt) Zertifikat <name> befindet sich derzeit in einem gelöschten, aber wiederherstellbaren Zustand“ angezeigt. Verwenden Sie einen anderen Schlüsselnamen.

## <a name="code-details"></a>Codedetails

### <a name="authenticate-and-create-a-client"></a>Authentifizieren und Erstellen eines Clients

Im Code oben verwendet das [`DefaultAzureCredential`](/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python)-Objekt die Umgebungsvariablen, die Sie für den Dienstprinzipal erstellt haben. Sie geben diese Anmeldeinformationen immer dann an, wenn Sie ein Clientobjekt aus einer Azure-Bibliothek erstellen (z. B. [`CertificateClient`](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificateclient?view=azure-python)). Die Angabe erfolgt zusammen mit dem URI der Ressource, mit der Sie über diesen Client arbeiten möchten:

```python
credential = DefaultAzureCredential()
client = CertificateClient(vault_url=KVUri, credential=credential)
```

### <a name="save-a-certificate"></a>Speichern eines Zertifikats

Nachdem Sie das Clientobjekt für den Schlüsseltresor abgerufen haben, können Sie mithilfe der [begin_create_certificate](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificateclient?view=azure-python#begin-create-certificate-certificate-name--policy----kwargs-)-Methode ein Zertifikat erstellen: 

```python
policy = CertificatePolicy.get_default()
poller = client.begin_create_certificate(certificate_name=certificateName, policy=policy)
certificate = poller.result()
```

Hier ist für das Zertifikat eine Richtlinie erforderlich, die mit der [CertificatePolicy.get_default](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificatepolicy?view=azure-python#get-default--)-Methode abgerufen wird.

Beim Aufrufen einer `begin_create_certificate`-Methode wird ein asynchroner Aufruf der Azure-REST-API für den Schlüsseltresor generiert. Der asynchrone Aufruf gibt ein Pollerobjekt zurück. Um auf das Ergebnis des Vorgangs zu warten, rufen Sie die `result`-Methode des Pollers auf.

Bei der Verarbeitung der Anforderung authentifiziert Azure die Identität des Aufrufers (Dienstprinzipal) mithilfe des Anmeldeinformationenobjekts, das Sie für den Client bereitgestellt haben.

Außerdem wird überprüft, ob der Aufrufer autorisiert ist, die angeforderte Aktion auszuführen. Sie haben diese Autorisierung dem Dienstprinzipal zuvor mithilfe des [`az keyvault set-policy`-Befehls](#give-the-service-principal-access-to-your-key-vault) erteilt.

### <a name="retrieve-a-certificate"></a>Abrufen eines Zertifikats

Um ein Zertifikat aus Key Vault zu lesen, verwenden Sie die [get_certificate](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificateclient?view=azure-python#get-certificate-certificate-name----kwargs-)-Methode:

```python
retrieved_certificate = client.get_certificate(certificateName)
 ```

Mithilfe des Azure CLI-Befehls [az keyvault certificate show](/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-show) können Sie sich vergewissern, dass das Zertifikat festgelegt wurde.

### <a name="delete-a-certificate"></a>Löschen eines Zertifikats

Um ein Zertifikat zu löschen, verwenden Sie die [begin_delete_certificate](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificateclient?view=azure-python#begin-delete-certificate-certificate-name----kwargs-)-Methode:

```python
poller = client.begin_delete_certificate(certificateName)
deleted_certificate = poller.result()
```

Die `begin_delete_certificate`-Methode ist asynchron und gibt ein Pollerobjekt zurück. Wenn die `result`-Methode des Pollers aufgerufen wird, wird auf ihren Abschluss gewartet.

Mithilfe des Azure CLI-Befehls [az keyvault certificate show](/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-show) können Sie sich vergewissern, dass das Zertifikat gelöscht wurde.

Nach dem Löschen verbleibt ein Zertifikat für einen bestimmten Zeitraum in einem gelöschten, aber wiederherstellbaren Zustand. Wenn Sie den Code erneut ausführen, verwenden Sie einen anderen Zertifikatnamen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie auch mit [Geheimnissen](../secrets/quick-create-python.md) und [Schlüssel](../keys/quick-create-python.md) experimentieren möchten, können Sie den in diesem Artikel erstellten Key Vault wiederverwenden.

Verwenden Sie andernfalls den folgenden Befehl, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu löschen, wenn Sie mit die in diesem Artikel erstellten Ressourcen nicht mehr benötigen:

```azurecli
az group delete --resource-group KeyVault-PythonQS-rg
```

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über den Azure-Schlüsseltresor](../general/overview.md)
- [Entwicklerhandbuch zu Azure Key Vault](../general/developers-guide.md)
- [Bewährte Methoden zum Verwenden von Key Vault](../general/best-practices.md)
- [Authentifizieren mit Key Vault](../general/authentication.md)
