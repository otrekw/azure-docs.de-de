---
title: 'Schnellstart: Python-Clientbibliothek in Azure Key Vault – Verwalten von Schlüsseln'
description: Hier erfahren Sie, wie Sie mithilfe der Python-Clientbibliothek Schlüssel in einer Azure Key Vault-Instanz erstellen, daraus abrufen und löschen.
author: msmbaldwin
ms.author: mbaldwin
ms.date: 09/03/2020
ms.service: key-vault
ms.subservice: keys
ms.topic: quickstart
ms.custom: devx-track-python
ms.openlocfilehash: 44942067756f82c224decc218de17bf7dbc69734
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "89482124"
---
# <a name="quickstart-azure-key-vault-keys-client-library-for-python"></a>Schnellstart: Azure Key Vault-Schlüsselclientbibliothek für Python

Hier finden Sie Informationen zu den ersten Schritten mit der Azure Key Vault-Clientbibliothek für Python. Führen Sie die weiter unten beschriebenen Schritte aus, um das Paket zu installieren und den Beispielcode für grundlegende Aufgaben auszuprobieren. Wenn Sie Key Vault zum Speichern von Kryptografieschlüsseln verwenden, vermeiden Sie das Speichern solcher Schlüssel im Code, was die Sicherheit Ihrer App erhöht.

[API-Referenzdokumentation](/python/api/overview/azure/keyvault-keys-readme?view=azure-python) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-keys) | [Paket (Python-Paketindex)](https://pypi.org/project/azure-keyvault-keys/)

## <a name="set-up-your-local-environment"></a>Einrichten Ihrer lokalen Umgebung

[!INCLUDE [Set up your local environment](../../../includes/key-vault-python-qs-setup.md)]

7. Installieren Sie die Key Vault-Schlüsselbibliothek:

    ```terminal
    pip install azure-keyvault-keys
    ```

## <a name="create-a-resource-group-and-key-vault"></a>Erstellen einer Ressourcengruppe und eines Schlüsseltresors

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-python-qs-rg-kv-creation.md)]

## <a name="give-the-service-principal-access-to-your-key-vault"></a>Gewähren des Zugriffs auf Ihren Schlüsseltresor für den Dienstprinzipal

Führen Sie den folgenden Befehl [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) aus, um den Dienstprinzipal für delete-, get-, list- und create-Vorgänge für Schlüssel zu autorisieren. 

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az keyvault set-policy --name %KEY_VAULT_NAME% --spn %AZURE_CLIENT_ID% --resource-group KeyVault-PythonQS-rg --key-permissions delete get list create
```

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
az keyvault set-policy --name $KEY_VAULT_NAME --spn $AZURE_CLIENT_ID --resource-group KeyVault-PythonQS-rg --key-permissions delete get list create
```

---

Dieser Befehl stützt sich auf die `KEY_VAULT_NAME`- und `AZURE_CLIENT_ID`-Umgebungsvariablen, die in den vorherigen Schritten erstellt wurden.

Weitere Informationen finden Sie unter [Zuweisen einer Zugriffsrichtlinie: CLI](../general/assign-access-policy-cli.md).

## <a name="create-the-sample-code"></a>Erstellen des Beispielcodes

Mit der Azure Key Vault-Clientbibliothek für Python können Sie Kryptografieschlüssel und zugehörige Objekte wie z. B. Zertifikate und Geheimnisse verwalten. Im folgenden Codebeispiel wird gezeigt, wie Sie einen Client erstellen und ein Geheimnis festlegen, abrufen und löschen.

Erstellen Sie eine Datei mit dem Namen *kv_keys.py*, die diesen Code enthält.

```python
import os
from azure.keyvault.keys import KeyClient
from azure.identity import DefaultAzureCredential

keyVaultName = os.environ["KEY_VAULT_NAME"]
KVUri = "https://" + keyVaultName + ".vault.azure.net"

credential = DefaultAzureCredential()
client = KeyClient(vault_url=KVUri, credential=credential)

keyName = input("Input a name for your key > ")

print(f"Creating a key in {keyVaultName} called '{keyName}' ...")

rsa_key = client.create_rsa_key(keyName, size=2048)

print(" done.")

print(f"Retrieving your key from {keyVaultName}.")

retrieved_key = client.get_key(keyName)

print(f"Key with name '{retrieved_key.name}' was found.")
print(f"Deleting your key from {keyVaultName} ...")

poller = client.begin_delete_key(keyName)
deleted_key = poller.result()

print(" done.")
```

## <a name="run-the-code"></a>Ausführen des Codes

Stellen Sie sicher, dass sich der Code aus dem vorherigen Abschnitt in einer Datei namens *kv_keys.py* befindet. Führen Sie den Code dann mithilfe des folgenden Befehls aus:

```terminal
python kv_keys.py
```

- Wenn Berechtigungsfehler auftreten, stellen Sie sicher, dass Sie den [`az keyvault set-policy`-Befehl](#give-the-service-principal-access-to-your-key-vault) ausgeführt haben.
- Wenn Sie den Code mit dem gleichen Schlüsselnamen erneut ausführen, wird möglicherweise der Fehler „(Konflikt) Schlüssel <name> befindet sich derzeit in einem gelöschten, aber wiederherstellbaren Zustand“ angezeigt. Verwenden Sie einen anderen Schlüsselnamen.

## <a name="code-details"></a>Codedetails

### <a name="authenticate-and-create-a-client"></a>Authentifizieren und Erstellen eines Clients

Im Code oben verwendet das [`DefaultAzureCredential`](/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python)-Objekt die Umgebungsvariablen, die Sie für den Dienstprinzipal erstellt haben. Sie geben diese Anmeldeinformationen immer dann an, wenn Sie ein Clientobjekt aus einer Azure-Bibliothek erstellen (z. B. [`KeyClient`](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python)). Die Angabe erfolgt zusammen mit dem URI der Ressource, mit der Sie über diesen Client arbeiten möchten:

```python
credential = DefaultAzureCredential()
client = KeyClient(vault_url=KVUri, credential=credential)
```

## <a name="save-a-key"></a>Speichern eines Schlüssels

Nachdem Sie das Clientobjekt für den Schlüsseltresor abgerufen haben, können Sie einen Schlüssel mithilfe der [create_rsa_key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#create-rsa-key-name----kwargs-)-Methode speichern: 

```python
rsa_key = client.create_rsa_key(keyName, size=2048)
```

Sie können auch [create_key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#create-key-name--key-type----kwargs-) oder [create_ec_key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#create-ec-key-name----kwargs-) verwenden.

Beim Aufrufen einer `create`-Methode wird ein Aufruf der Azure-REST-API für den Schlüsseltresor generiert.

Bei der Verarbeitung der Anforderung authentifiziert Azure die Identität des Aufrufers (Dienstprinzipal) mithilfe des Anmeldeinformationenobjekts, das Sie für den Client bereitgestellt haben.

Außerdem wird überprüft, ob der Aufrufer autorisiert ist, die angeforderte Aktion auszuführen. Sie haben diese Autorisierung dem Dienstprinzipal zuvor mithilfe des [`az keyvault set-policy`-Befehls](#give-the-service-principal-access-to-your-key-vault) erteilt.

## <a name="retrieve-a-key"></a>Abrufen eines Schlüssels

Um einen Schlüssel aus Key Vault zu lesen, verwenden Sie die [get_Key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#get-key-name--version-none----kwargs-)-Methode:

```python
retrieved_key = client.get_key(keyName)
 ```

Mithilfe des Azure CLI-Befehls [az keyvault key show](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-show) können Sie auch sich vergewissern, dass der Schlüssel festgelegt wurde.

### <a name="delete-a-key"></a>Löschen eines Schlüssels

Verwenden Sie die [begin_delete_key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#begin-delete-key-name----kwargs-)-Methode, um einen Schlüssel zu löschen:

```python
poller = client.begin_delete_key(keyName)
deleted_key = poller.result()
```

Die `begin_delete_key`-Methode ist asynchron und gibt ein Pollerobjekt zurück. Wenn die `result`-Methode des Pollers aufgerufen wird, wird auf ihren Abschluss gewartet.

Mithilfe des Azure CLI-Befehls [az keyvault key show](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-show) können Sie sich vergewissern, dass der Schlüssel gelöscht wurde.

Nach dem Löschen verbleibt ein Schlüssel für einen bestimmten Zeitraum in einem gelöschten, aber wiederherstellbaren Zustand. Wenn Sie den Code erneut ausführen, verwenden Sie einen anderen Schlüsselnamen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie auch mit [Zertifikaten](../certificates/quick-create-python.md) und [Geheimnissen](../secrets/quick-create-python.md) experimentieren möchten, können Sie den in diesem Artikel erstellten Key Vault wiederverwenden.

Verwenden Sie andernfalls den folgenden Befehl, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu löschen, wenn Sie mit die in diesem Artikel erstellten Ressourcen nicht mehr benötigen:

```azurecli
az group delete --resource-group KeyVault-PythonQS-rg
```

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über den Azure-Schlüsseltresor](../general/overview.md)
- [Entwicklerhandbuch zu Azure Key Vault](../general/developers-guide.md)
- [Bewährte Methoden zum Verwenden von Key Vault](../general/best-practices.md)
- [Authentifizieren mit Key Vault](../general/authentication.md)
