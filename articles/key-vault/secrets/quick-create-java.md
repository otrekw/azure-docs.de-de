---
title: 'Schnellstart: Azure Key Vault-Clientbibliothek für Java'
description: Hier finden Sie Format- und Inhaltskriterien für das Schreiben von Schnellstartanleitungen für Azure SDK-Clientbibliotheken.
author: msmbaldwin
ms.author: mbaldwin
ms.date: 10/20/2019
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.openlocfilehash: 6351a3d27806d791e478999d04922b961949c46a
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82982835"
---
# <a name="quickstart-azure-key-vault-client-library-for-java"></a>Schnellstart: Azure Key Vault-Clientbibliothek für Java

Hier finden Sie Informationen zu den ersten Schritten mit der Azure Key Vault-Clientbibliothek für Java. Führen Sie die weiter unten beschriebenen Schritte aus, um das Paket zu installieren und den Beispielcode für grundlegende Aufgaben auszuprobieren.

Azure Key Vault unterstützt Sie dabei, kryptografische Schlüssel und Geheimnisse zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Die Key Vault-Clientbibliothek für Java ermöglicht Folgendes:

- Verbessern der Sicherheit und der Kontrolle über Schlüssel und Kennwörter
- Erstellen und Importieren von Verschlüsselungsschlüsseln in wenigen Minuten
- Verringern der Wartezeit durch Cloudskalierung und globale Redundanz
- Vereinfachen und Automatisieren von Aufgaben für TLS-/SSL-Zertifikate
- Verwenden FIPS 140-2 Level 2-zertifizierter HSMs

[Quellcode](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault) | [API-Referenzdokumentation](https://azure.github.io/azure-sdk-for-java) | [Produktdokumentation](index.yml) | [Beispiele](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-secrets/src/samples/java/com/azure/security/keyvault/secrets)

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement ([kostenloses Abonnement erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))
- [Java Development Kit (JDK)](/java/azure/jdk/?view=azure-java-stable), Version 8 oder höher
- [Apache Maven](https://maven.apache.org)
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) oder [Azure PowerShell](/powershell/azure/overview)

In diesem Schnellstart wird davon ausgegangen, dass Sie die [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) und [Apache Maven](https://maven.apache.org) in einem Linux-Terminalfenster ausführen.

## <a name="setting-up"></a>Einrichten

### <a name="create-new-java-console-app"></a>Erstellen einer neuen Java-Konsolen-App

Erstellen Sie in einem Konsolenfenster mithilfe des Befehls `mvn` eine neue Java-Konsolen-App mit dem Namen `akv-java`.

```console
mvn archetype:generate -DgroupId=com.keyvault.quickstart
                       -DartifactId=akv-java
                       -DarchetypeArtifactId=maven-archetype-quickstart
                       -DarchetypeVersion=1.4
                       -DinteractiveMode=false
```

Die Ausgabe der Erstellung des Projekts sieht in etwa wie folgt aus:

```console
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: maven-archetype-quickstart:1.4
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: groupId, Value: com.keyvault.quickstart
[INFO] Parameter: artifactId, Value: akv-java
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: package, Value: com.keyvault.quickstart
[INFO] Parameter: packageInPathFormat, Value: com/keyvault/quickstart
[INFO] Parameter: package, Value: com.keyvault.quickstart
[INFO] Parameter: groupId, Value: com.keyvault.quickstart
[INFO] Parameter: artifactId, Value: akv-java
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Project created from Archetype in dir: /home/user/quickstarts/akv-java
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  38.124 s
[INFO] Finished at: 2019-11-15T13:19:06-08:00
[INFO] ------------------------------------------------------------------------
```

Wechseln Sie zum neu erstellten Ordner „akv-java/“.

```console
cd akv-java
```

### <a name="install-the-package"></a>Installieren des Pakets

Öffnen Sie die Datei *pom.xml* in Ihrem Text-Editor. Fügen Sie der Gruppe der Abhängigkeiten das folgende Abhängigkeitselement hinzu:

```xml
    <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-security-keyvault-secrets</artifactId>
      <version>4.0.0</version>
    </dependency>

    <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-identity</artifactId>
      <version>1.0.0</version>
    </dependency>
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

Eine cloudbasierte Anwendung kann am einfachsten mit einer verwalteten Identität authentifiziert werden. Einzelheiten hierzu finden Sie unter [Verwenden einer verwalteten App Service-Identität für den Zugriff auf Azure Key Vault](../general/managed-identity.md).

Der Einfachheit halber wird in dieser Schnellstartanleitung eine Desktopanwendung erstellt, die die Verwendung eines Dienstprinzipals und einer Zugriffssteuerungsrichtlinie erfordert. Ihr Dienstprinzipal muss einen eindeutigen Namen im Format „http://&lt;eindeutiger Dienstprinzipalname&gt;“ haben.

Erstellen Sie mithilfe des Azure CLI-Befehls [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) einen Dienstprinzipal:

```azurecli
az ad sp create-for-rbac -n "http://&lt;my-unique-service-principle-name&gt;" --sdk-auth
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

Notieren Sie sich die Werte für „clientId“, „clientSecret“ und „tenantId“, da wir diese in den nächsten beiden Schritten verwenden.

#### <a name="give-the-service-principal-access-to-your-key-vault"></a>Gewähren des Zugriffs auf Ihren Schlüsseltresor für den Dienstprinzipal

Erstellen Sie eine Zugriffsrichtlinie für Ihren Schlüsseltresor, die dem Dienstprinzipal Berechtigungen erteilt, indem Sie die Client-ID an den Befehl [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) übergeben. Erteilen Sie dem Dienstprinzipal Berechtigungen zum Abrufen, Auflisten und Festlegen von Schlüsseln und Geheimnissen.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey
```

#### <a name="set-environmental-variables"></a>Festlegen von Umgebungsvariablen

Die DefaultAzureCredential-Methode in unserer Anwendung basiert auf drei Umgebungsvariablen: `AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET` und `AZURE_TENANT_ID`. Legen Sie diese Variablen auf die Werte von clientId, clientSecret und tenantId fest, die Sie sich im Schritt [Erstellen eines Dienstprinzipals](#create-a-service-principal) notiert haben. Verwenden Sie das Format `export VARNAME=VALUE` zum Festlegen Ihrer Umgebungsvariablen. (Mit dieser Methode werden nur die Variablen für Ihre aktuelle Shell und daraus erstellte Prozesse festgelegt. Um diese Variablen dauerhaft zu Ihrer Umgebung hinzuzufügen, bearbeiten Sie Ihre Datei `/etc/environment `.) 

Sie müssen außerdem den Namen Ihres Schlüsseltresors als Umgebungsvariable `KEY_VAULT_NAME` speichern.

```console
export AZURE_CLIENT_ID=<your-clientID>

export AZURE_CLIENT_SECRET=<your-clientSecret>

export AZURE_TENANT_ID=<your-tenantId>

export KEY_VAULT_NAME=<your-key-vault-name>
````

## <a name="object-model"></a>Objektmodell

Mit der Azure Key Vault-Clientbibliothek für Java können Sie Schlüssel und zugehörige Objekte wie etwa Zertifikate und Geheimnisse verwalten. In den folgenden Codebeispielen wird gezeigt, wie Sie einen Client erstellen und ein Geheimnis festlegen, abrufen und löschen.

Die vollständige Konsolen-App finden Sie [weiter unten](#sample-code).

## <a name="code-examples"></a>Codebeispiele

### <a name="add-directives"></a>Hinzufügen von Anweisungen

Fügen Sie am Anfang Ihres Codes die folgenden Anweisungen hinzu:

```java
import com.azure.identity.DefaultAzureCredentialBuilder;

import com.azure.security.keyvault.secrets.SecretClient;
import com.azure.security.keyvault.secrets.SecretClientBuilder;
import com.azure.security.keyvault.secrets.models.KeyVaultSecret;
```

### <a name="authenticate-and-create-a-client"></a>Authentifizieren und Erstellen eines Clients

Die Authentifizierung bei Ihrem Schlüsseltresor und die Erstellung eines Schlüsseltresorclients hängen von den Umgebungsvariablen im Schritt [Festlegen von Umgebungsvariablen](#set-environmental-variables) oben ab. Der Name Ihres Schlüsseltresors wird in den Schlüsseltresor-URI im Format `https://<your-key-vault-name>.vault.azure.net` erweitert.

```java
String keyVaultName = System.getenv("KEY_VAULT_NAME");
String kvUri = "https://" + keyVaultName + ".vault.azure.net";

SecretClient secretClient = new SecretClientBuilder()
    .vaultUrl(kvUri)
    .credential(new DefaultAzureCredentialBuilder().build())
    .buildClient();
```

### <a name="save-a-secret"></a>Speichern eines Geheimnisses

Nachdem Ihre Anwendung authentifiziert wurde, können Sie Ihrem Schlüsseltresor mithilfe der Methode `secretClient.setSecret` ein Geheimnis hinzufügen Hierzu benötigen Sie einen Namen für das Geheimnis. In diesem Beispiel haben wir der Variable `secretName` den Wert „mySecret“ zugewiesen.  

```java
secretClient.setSecret(new KeyVaultSecret(secretName, secretValue));
```

Mithilfe des Befehls [az keyvault secret show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) können Sie sich vergewissern, dass das Geheimnis festgelegt wurde:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

### <a name="retrieve-a-secret"></a>Abrufen eines Geheimnisses

Nun können Sie den zuvor festgelegten Wert mithilfe der Methode `secretClient.getSecret` abrufen.

```java
KeyVaultSecret retrievedSecret = secretClient.getSecret(secretName);
 ```

Sie können jetzt mit `retrievedSecret.getValue()` auf den Wert des abgerufenen Geheimnisses zugreifen.

### <a name="delete-a-secret"></a>Löschen eines Geheimnisses

Abschließend löschen wir das Geheimnis mithilfe der `secretClient.beginDeleteSecret`-Methode aus dem Schlüsseltresor.

```java
secretClient.beginDeleteSecret(secretName);
```

Sie können sich mithilfe des Befehls [az keyvault secret show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) vergewissern, dass das Geheimnis entfernt wurde:

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

```java
package com.keyvault.quickstart;

import java.io.Console;   

import com.azure.identity.DefaultAzureCredentialBuilder;

import com.azure.security.keyvault.secrets.SecretClient;
import com.azure.security.keyvault.secrets.SecretClientBuilder;
import com.azure.security.keyvault.secrets.models.KeyVaultSecret;

public class App {

    public static void main(String[] args) throws InterruptedException, IllegalArgumentException {

        String keyVaultName = System.getenv("KEY_VAULT_NAME");
        String kvUri = "https://" + keyVaultName + ".vault.azure.net";

        System.out.printf("key vault name = %s and kv uri = %s \n", keyVaultName, kvUri);

        SecretClient secretClient = new SecretClientBuilder()
            .vaultUrl(kvUri)
            .credential(new DefaultAzureCredentialBuilder().build())
            .buildClient();


        Console con = System.console();  

        String secretName = "mySecret";

        System.out.println("Input the value of your secret > ");
        String secretValue = con.readLine();

        System.out.print("Creating a secret in " + keyVaultName + " called '" + secretName + "' with the value '" + secretValue + "` ... ");

        secretClient.setSecret(new KeyVaultSecret(secretName, secretValue));

        System.out.println("done.");

        System.out.println("Forgetting your secret.");
        secretValue = "";
        System.out.println("Your secret is '" + secretValue + "'.");

        System.out.println("Retrieving your secret from " + keyVaultName + ".");

        KeyVaultSecret retrievedSecret = secretClient.getSecret(secretName);

        System.out.println("Your secret is '" + retrievedSecret.getValue() + "'.");
        System.out.print("Deleting your secret from " + keyVaultName + " ... ");

        secretClient.beginDeleteSecret(secretName);

        System.out.println("done.");


    }
}
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen Schlüsseltresor erstellt, ein Geheimnis gespeichert und dieses Geheimnis abgerufen. Weitere Informationen zu Key Vault und zur Integration in Ihre Anwendungen finden Sie in den folgenden Artikeln:

- [Was ist der Azure-Schlüsseltresor?](../general/overview.md)
- [Entwicklerhandbuch zu Azure-Schlüsseltresor](../general/developers-guide.md)
- [Bewährte Methoden zum Verwenden von Key Vault](../general/best-practices.md)
