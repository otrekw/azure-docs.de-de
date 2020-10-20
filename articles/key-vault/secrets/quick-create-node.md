---
title: 'Schnellstart: Azure Key Vault-Clientbibliothek für JavaScript (v4)'
description: Es wird beschrieben, wie Sie mit der JavaScript-Clientbibliothek Geheimnisse in einer Azure Key Vault-Instanz erstellen, daraus abrufen und löschen.
author: msmbaldwin
ms.author: mbaldwin
ms.date: 10/20/2019
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: devx-track-js
ms.openlocfilehash: 045589d3b1f0e376eaf854562d271a4483702997
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92047895"
---
# <a name="quickstart-azure-key-vault-client-library-for-javascript-v4"></a>Schnellstart: Azure Key Vault-Clientbibliothek für JavaScript (v4)

Hier finden Sie Informationen zu den ersten Schritten mit der Azure Key Vault-Geheimnisclientbibliothek für JavaScript. Führen Sie die weiter unten beschriebenen Schritte aus, um das Paket zu installieren und den Beispielcode für grundlegende Aufgaben auszuprobieren.

[API-Referenzdokumentation](https://docs.microsoft.com/javascript/api/overview/azure/key-vault-index) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/keyvault) | [Paket (npm)](https://www.npmjs.com/package/@azure/keyvault-secrets)

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement ([kostenloses Abonnement erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))
- Aktuelle [Node.js](https://nodejs.org) für Ihr Betriebssystem.
- [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli)

In diesem Schnellstart wird davon ausgegangen, dass Sie die [Azure CLI](/cli/azure/install-azure-cli) in einem Linux-Terminalfenster ausführen.

## <a name="setting-up"></a>Einrichten
In dieser Schnellstartanleitung wird die Azure Identity-Bibliothek mit der Azure CLI verwendet, um den Benutzer bei Azure-Diensten zu authentifizieren. Entwickler können auch Visual Studio oder Visual Studio Code verwenden, um ihre Aufrufe zu authentifizieren. Weitere Informationen finden Sie unter [Authentifizieren des Clients mit der Azure Identity-Clientbibliothek](https://docs.microsoft.com/javascript/api/overview/azure/identity-readme).

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

1. Führen Sie den Befehl `login` aus.

    ```azurecli-interactive
    az login
    ```

    Die CLI öffnet Ihren Standardbrowser, sofern sie dazu in der Lage ist, und lädt eine Azure-Anmeldeseite.

    Öffnen Sie andernfalls die Browserseite [https://aka.ms/devicelogin](https://aka.ms/devicelogin), und geben Sie den in Ihrem Terminal angezeigten Autorisierungscode ein.

2. Melden Sie sich im Browser mit Ihren Anmeldeinformationen an.

### <a name="install-the-package"></a>Installieren des Pakets

Installieren Sie über das Konsolenfenster die Azure Key Vault-Geheimnisbibliothek für Node.js.

```console
npm install @azure/keyvault-secrets
```

Im Rahmen dieses Schnellstarts muss außerdem das azure.identity-Paket installiert werden:

```console
npm install @azure/identity
```

### <a name="create-a-resource-group-and-key-vault"></a>Erstellen einer Ressourcengruppe und eines Schlüsseltresors

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-rg-kv-creation.md)]

#### <a name="grant-access-to-your-key-vault"></a>Gewähren des Zugriffs auf Ihren Schlüsseltresor

Erstellen Sie für Ihren Schlüsseltresor eine Zugriffsrichtlinie, mit der Ihrem Benutzerkonto die Geheimnisberechtigung erteilt wird.

```console
az keyvault set-policy --name <YourKeyVaultName> --upn user@domain.com --secret-permissions delete get list set
```

#### <a name="set-environment-variables"></a>Festlegen von Umgebungsvariablen

Diese Anwendung verwendet den Namen des Schlüsseltresors als Umgebungsvariable namens `KEY_VAULT_NAME`.

Windows
```cmd
set KEY_VAULT_NAME=<your-key-vault-name>
````
Windows PowerShell
```powershell
$Env:KEY_VAULT_NAME=<your-key-vault-name>
```

macOS oder Linux
```cmd
export KEY_VAULT_NAME=<your-key-vault-name>
```

## <a name="object-model"></a>Objektmodell

Mit der Azure Key Vault-Geheimnisclientbibliothek für JavaScript können Sie Geheimnisse verwalten. In den folgenden Codebeispielen wird gezeigt, wie Sie einen Client erstellen und ein Geheimnis festlegen, abrufen und löschen.

## <a name="code-examples"></a>Codebeispiele

### <a name="add-directives"></a>Hinzufügen von Anweisungen

Fügen Sie am Anfang Ihres Codes die folgenden Anweisungen hinzu:

```javascript
const { DefaultAzureCredential } = require("@azure/identity");
const { SecretClient } = require("@azure/keyvault-secrets");
```

### <a name="authenticate-and-create-a-client"></a>Authentifizieren und Erstellen eines Clients

In dieser Schnellstartanleitung wird der angemeldete Benutzer zum Authentifizieren beim Schlüsseltresor verwendet. Dies ist die bevorzugte Methode für die lokale Entwicklung. Bei Anwendungen, die in Azure bereitgestellt werden, sollte die verwaltete Identität App Service oder einem virtuellen Computer zugewiesen werden. Weitere Informationen finden Sie in der [Übersicht zu verwalteten Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

Im folgenden Beispiel wird der Name Ihres Schlüsseltresors in den Schlüsseltresor-URI mit dem Format „https://\<your-key-vault-name\>.vault.azure.net“ erweitert. In diesem Beispiel wird die Klasse [DefaultAzureCredential()](https://docs.microsoft.com/javascript/api/@azure/identity/defaultazurecredential) verwendet, mit der derselbe Code in verschiedenen Umgebungen mit verschiedenen Optionen zum Bereitstellen von Identitäten verwendet werden kann. Weitere Informationen finden Sie unter der [DefaultAzureCredential-Authentifizierung](https://docs.microsoft.com/javascript/api/overview/azure/identity-readme). 

```javascript
const keyVaultName = process.env["KEY_VAULT_NAME"];
const KVUri = "https://" + keyVaultName + ".vault.azure.net";

const credential = new DefaultAzureCredential();
const client = new SecretClient(KVUri, credential);
```

### <a name="save-a-secret"></a>Speichern eines Geheimnisses

Nachdem Ihre Anwendung authentifiziert wurde, können Sie Ihrem Schlüsseltresor mit der [client.setSecret](/javascript/api/@azure/keyvault-secrets/secretclient?#setsecret-string--string--setsecretoptions-)-Methode ein Geheimnis hinzufügen. Hierzu benötigen Sie einen Namen für das Geheimnis. In diesem Beispiel verwenden wir „mySecret“.  

```javascript
await client.setSecret(secretName, secretValue);
```

Mithilfe des Befehls [az keyvault secret show](/cli/azure/keyvault/secret?#az-keyvault-secret-show) können Sie sich vergewissern, dass das Geheimnis festgelegt wurde:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

### <a name="retrieve-a-secret"></a>Abrufen eines Geheimnisses

Nun können Sie den zuvor festgelegten Wert mithilfe der [client.getSecret](/javascript/api/@azure/keyvault-secrets/secretclient?#getsecret-string--getsecretoptions-)-Methode abrufen.

```javascript
const retrievedSecret = await client.getSecret(secretName);
 ```

Ihr Geheimnis ist jetzt als `retrievedSecret.value` gespeichert.

### <a name="delete-a-secret"></a>Löschen eines Geheimnisses

Abschließend löschen wir das Geheimnis mithilfe der [client.beginDeleteSecret](/javascript/api/@azure/keyvault-secrets/secretclient?#begindeletesecret-string--begindeletesecretoptions-)-Methode aus dem Schlüsseltresor.

```javascript
await client.beginDeleteSecret(secretName)
```

Sie können sich mithilfe des Befehls [az keyvault secret show](/cli/azure/keyvault/secret?#az-keyvault-secret-show) vergewissern, dass das Geheimnis entfernt wurde:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
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

```javascript
const { DefaultAzureCredential } = require("@azure/identity");
const { SecretClient } = require("@azure/keyvault-secrets");

const readline = require('readline');

function askQuestion(query) {
    const rl = readline.createInterface({
        input: process.stdin,
        output: process.stdout,
    });

    return new Promise(resolve => rl.question(query, ans => {
        rl.close();
        resolve(ans);
    }))
}

async function main() {

  const keyVaultName = process.env["KEY_VAULT_NAME"];
  const KVUri = "https://" + keyVaultName + ".vault.azure.net";

  const credential = new DefaultAzureCredential();
  const client = new SecretClient(KVUri, credential);

  const secretName = "mySecret";
  var secretValue = await askQuestion("Input the value of your secret > ");

  console.log("Creating a secret in " + keyVaultName + " called '" + secretName + "' with the value '" + secretValue + "` ...");
  await client.setSecret(secretName, secretValue);

  console.log("Done.");

  console.log("Forgetting your secret.");
  secretValue = "";
  console.log("Your secret is '" + secretValue + "'.");

  console.log("Retrieving your secret from " + keyVaultName + ".");

  const retrievedSecret = await client.getSecret(secretName);

  console.log("Your secret is '" + retrievedSecret.value + "'.");
  console.log("Deleting your secret from " + keyVaultName + " ...");

  await client.beginDeleteSecret(secretName);

  console.log("Done.");

}

main()

```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen Schlüsseltresor erstellt, ein Geheimnis gespeichert und dieses Geheimnis abgerufen. Weitere Informationen zu Key Vault und zur Integration in Ihre Anwendungen finden Sie in den folgenden Artikeln:

- [Was ist der Azure-Schlüsseltresor?](../general/overview.md)
- [Sicherer Zugriff auf einen Schlüsseltresor](../general/secure-your-key-vault.md)
- [Entwicklerhandbuch zu Azure-Schlüsseltresor](../general/developers-guide.md)
- [Bewährte Methoden zum Verwenden von Key Vault](../general/best-practices.md)