---
title: 'Schnellstart: Python-Clientbibliothek in Azure Key Vault – Verwalten von Geheimnissen'
description: Erfahren Sie, wie Sie mithilfe der Python-Clientbibliothek Geheimnisse in einer Azure Key Vault-Instanz erstellen, daraus abrufen und löschen.
author: msmbaldwin
ms.author: mbaldwin
ms.date: 09/03/2020
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: devx-track-python
ms.openlocfilehash: cd8a5751c018b9b3b3b2ef96765545f2edab685b
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "89489203"
---
# <a name="quickstart-azure-key-vault-secrets-client-library-for-python"></a>Schnellstart: Azure Key Vault-Geheimnisclientbibliothek für Python

Hier finden Sie Informationen zu den ersten Schritten mit der Azure Key Vault-Clientbibliothek für Python. Führen Sie die weiter unten beschriebenen Schritte aus, um das Paket zu installieren und den Beispielcode für grundlegende Aufgaben auszuprobieren. Wenn Sie Key Vault zum Speichern von Geheimnissen verwenden, vermeiden Sie das Speichern von Geheimnissen im Code, was die Sicherheit Ihrer App erhöht.

[API-Referenzdokumentation](/python/api/overview/azure/keyvault-secrets-readme?view=azure-python) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-secrets) | [Paket (Python-Paketindex)](https://pypi.org/project/azure-keyvault-secrets/)

## <a name="set-up-your-local-environment"></a>Einrichten Ihrer lokalen Umgebung

[!INCLUDE [Set up your local environment](../../../includes/key-vault-python-qs-setup.md)]

7. Installieren Sie die Key Vault-Geheimnisbibliothek:

    ```terminal
    pip install azure-keyvault-secrets
    ```

## <a name="create-a-resource-group-and-key-vault"></a>Erstellen einer Ressourcengruppe und eines Schlüsseltresors

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-python-qs-rg-kv-creation.md)]

## <a name="give-the-service-principal-access-to-your-key-vault"></a>Gewähren des Zugriffs auf Ihren Schlüsseltresor für den Dienstprinzipal

Führen Sie den folgenden Befehl [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) aus, um den Dienstprinzipal für get-, list- und set-Vorgänge für Geheimnisse zu autorisieren. Dieser Befehl stützt sich auf die `KEY_VAULT_NAME`- und `AZURE_CLIENT_ID`-Umgebungsvariablen, die in den vorherigen Schritten erstellt wurden.

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az keyvault set-policy --name %KEY_VAULT_NAME% --spn %AZURE_CLIENT_ID% --resource-group KeyVault-PythonQS-rg --secret-permissions delete get list set 
```

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
az keyvault set-policy --name $KEY_VAULT_NAME --spn $AZURE_CLIENT_ID --resource-group KeyVault-PythonQS-rg --secret-permissions delete get list set 
```

---

Dieser Befehl stützt sich auf die `KEY_VAULT_NAME`- und `AZURE_CLIENT_ID`-Umgebungsvariablen, die in den vorherigen Schritten erstellt wurden.

Weitere Informationen finden Sie unter [Zuweisen einer Zugriffsrichtlinie: CLI](../general/assign-access-policy-cli.md).

## <a name="create-the-sample-code"></a>Erstellen des Beispielcodes

Die Azure Key Vault-Clientbibliothek für Python ermöglicht Ihnen die Verwaltung von Geheimnissen und zugehörigen Ressourcen wie Zertifikaten und kryptographischen Schlüsseln. Im folgenden Codebeispiel wird gezeigt, wie Sie einen Client erstellen und ein Geheimnis festlegen, abrufen und löschen.

Erstellen Sie eine Datei mit dem Namen *kv_secrets.py*, die diesen Code enthält.

```python
import os
import cmd
from azure.keyvault.secrets import SecretClient
from azure.identity import DefaultAzureCredential

keyVaultName = os.environ["KEY_VAULT_NAME"]
KVUri = f"https://{keyVaultName}.vault.azure.net"

credential = DefaultAzureCredential()
client = SecretClient(vault_url=KVUri, credential=credential)

secretName = input("Input a name for your secret > ")
secretValue = input("Input a value for your secret > ")

print(f"Creating a secret in {keyVaultName} called '{secretName}' with the value '{secretValue}' ...")

client.set_secret(secretName, secretValue)

print(" done.")

print(f"Retrieving your secret from {keyVaultName}.")

retrieved_secret = client.get_secret(secretName)

print(f"Your secret is '{retrieved_secret.value}'.")
print(f"Deleting your secret from {keyVaultName} ...")

poller = client.begin_delete_secret(secretName)
deleted_secret = poller.result()

print(" done.")
```

## <a name="run-the-code"></a>Ausführen des Codes

Stellen Sie sicher, dass sich der Code aus dem vorherigen Abschnitt in einer Datei namens *kv_secrets.py* befindet. Führen Sie den Code dann mithilfe des folgenden Befehls aus:

```terminal
python kv_secrets.py
```

- Wenn Berechtigungsfehler auftreten, stellen Sie sicher, dass Sie den [`az keyvault set-policy`-Befehl](#give-the-service-principal-access-to-your-key-vault) ausgeführt haben.
- Wenn Sie den Code mit dem gleichen Geheimnisnamen erneut ausführen, wird möglicherweise der Fehler „(Konflikt) Geheimnis <name> befindet sich derzeit in einem gelöschten, aber wiederherstellbaren Zustand“ angezeigt. Verwenden Sie einen anderen Geheimnisnamen.

## <a name="code-details"></a>Codedetails

### <a name="authenticate-and-create-a-client"></a>Authentifizieren und Erstellen eines Clients

Im Code oben verwendet das [`DefaultAzureCredential`](/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python)-Objekt die Umgebungsvariablen, die Sie für den Dienstprinzipal erstellt haben. Sie geben diese Anmeldeinformationen immer dann an, wenn Sie ein Clientobjekt aus einer Azure-Bibliothek erstellen (z. B. [`SecretClient`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python)). Die Angabe erfolgt zusammen mit dem URI der Ressource, mit der Sie über diesen Client arbeiten möchten:

```python
credential = DefaultAzureCredential()
client = SecretClient(vault_url=KVUri, credential=credential)
```

### <a name="save-a-secret"></a>Speichern eines Geheimnisses

Nachdem Sie das Clientobjekt für den Schlüsseltresor abgerufen haben, können Sie ein Geheimnis mithilfe der [set-secret](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python#set-secret-name--value----kwargs-)-Methode speichern: 

```python
client.set_secret(secretName, secretValue)
```

Der Aufruf von `set_secret` generiert einen Aufruf der Azure-REST-API für den Schlüsseltresor.

Bei der Verarbeitung der Anforderung authentifiziert Azure die Identität des Aufrufers (Dienstprinzipal) mithilfe des Anmeldeinformationenobjekts, das Sie für den Client bereitgestellt haben.

Außerdem wird überprüft, ob der Aufrufer autorisiert ist, die angeforderte Aktion auszuführen. Sie haben diese Autorisierung dem Dienstprinzipal zuvor mithilfe des [`az keyvault set-policy`-Befehls](#give-the-service-principal-access-to-your-key-vault) erteilt.

### <a name="retrieve-a-secret"></a>Abrufen eines Geheimnisses

Um ein Geheimnis aus Key Vault zu lesen, verwenden Sie die [get_secret](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python#get-secret-name--version-none----kwargs-)-Methode:

```python
retrieved_secret = client.get_secret(secretName)
 ```

Der Wert für das Geheimnis ist in `retrieved_secret.value` enthalten.

Sie können ein Geheimnis auch mit dem Azure CLI-Befehl [az keyvault secret show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) abrufen.

### <a name="delete-a-secret"></a>Löschen eines Geheimnisses

Um ein Geheimnis zu löschen, verwenden Sie die [begin_delete_secret](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python#begin-delete-secret-name----kwargs-)-Methode:

```python
poller = client.begin_delete_secret(secretName)
deleted_secret = poller.result()
```

Die `begin_delete_secret`-Methode ist asynchron und gibt ein Pollerobjekt zurück. Wenn die `result`-Methode des Pollers aufgerufen wird, wird auf ihren Abschluss gewartet.

Mithilfe des Azure CLI-Befehls [az keyvault secret show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) können Sie sich vergewissern, dass das Geheimnis entfernt wurde.

Nach dem Löschen verbleibt ein Geheimnis für einen bestimmten Zeitraum in einem gelöschten, aber wiederherstellbaren Zustand. Wenn Sie den Code erneut ausführen, verwenden Sie einen anderen Geheimnisnamen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie auch mit [Zertifikaten](../certificates/quick-create-python.md) und [Schlüsseln](../keys/quick-create-python.md) experimentieren möchten, können Sie den in diesem Artikel erstellten Key Vault wiederverwenden.

Verwenden Sie andernfalls den folgenden Befehl, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu löschen, wenn Sie mit die in diesem Artikel erstellten Ressourcen nicht mehr benötigen:

```azurecli
az group delete --resource-group KeyVault-PythonQS-rg
```

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über den Azure-Schlüsseltresor](../general/overview.md)
- [Entwicklerhandbuch zu Azure Key Vault](../general/developers-guide.md)
- [Bewährte Methoden zum Verwenden von Key Vault](../general/best-practices.md)
- [Authentifizieren mit Key Vault](../general/authentication.md)
