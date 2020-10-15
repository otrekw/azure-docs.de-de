---
title: Tutorial zur Rotation von Ressourcen mit zwei Sätzen mit Anmeldeinformationen
description: In diesem Tutorial erfahren Sie, wie Sie die Rotation eines Geheimnisses für Ressourcen automatisieren, bei denen zwei Sätze mit Anmeldeinformationen für die Authentifizierung verwendet werden.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: rotation
ms.service: key-vault
ms.subservice: secrets
ms.topic: tutorial
ms.date: 06/22/2020
ms.author: jalichwa
ms.openlocfilehash: 5da31d45e068f414c8afa38bcb46cdf1f790a9e5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91843276"
---
# <a name="automate-the-rotation-of-a-secret-for-resources-with-two-sets-of-authentication-credentials"></a>Automatisieren der Rotation eines Geheimnisses für Ressourcen mit zwei Sätzen mit Anmeldeinformationen für die Authentifizierung

Die beste Möglichkeit zur Authentifizierung bei Azure-Diensten ist die Verwendung einer [verwalteten Identität](../general/authentication.md). Es gibt jedoch einige Szenarien, in denen dies nicht möglich ist. In diesen Fällen werden Zugriffsschlüssel oder Kennwörter verwendet. Zugriffsschlüssel und Kennwörter sollten häufig rotiert werden.

In diesem Tutorial wird gezeigt, wie Sie die regelmäßige Rotation von Geheimnissen für Datenbanken und Dienste automatisieren, bei denen zwei Sätze mit Anmeldeinformationen für die Authentifizierung verwendet werden. Genauer gesagt werden in diesem Tutorial Azure Storage-Kontoschlüssel, die in Azure Key Vault als Geheimnisse gespeichert sind, mit einer Funktion rotiert, die durch eine Azure Event Grid-Benachrichtigung ausgelöst wird. :

> [!NOTE]
> Storage-Kontoschlüssel können in Key Vault automatisch verwaltet werden, indem Shared Access Signature-Token für den delegierten Zugriff auf das Storage-Konto bereitgestellt werden. Es sind Dienste vorhanden, für die eine Speicherkonto-Verbindungszeichenfolge mit Zugriffsschlüssel erforderlich ist. Für dieses Szenario wird diese Lösung empfohlen.

![Diagramm der Rotationslösung](../media/secrets/rotation-dual/rotation-diagram.png)

In der obigen Lösung werden von Azure Key Vault individuelle Speicherkonto-Zugriffsschlüssel als Versionen desselben Geheimnisses gespeichert, und für nachfolgende Versionen wird zwischen dem Primär- und dem Sekundärschlüssel gewechselt. Wenn ein Zugriffsschlüssel in der aktuellen Version des Geheimnisses gespeichert wird, wird der Alternativschlüssel erneut generiert und Key Vault als neue und aktuelle Version des Geheimnisses hinzugefügt. Diese Lösung ermöglicht für Anwendungen einen gesamten Rotationszyklus, um die Aktualisierung auf den aktuellsten erneut generierten Schlüssel durchzuführen. 

1. 30 Tage vor dem Ablaufdatum eines Geheimnisses veröffentlicht Key Vault das Ereignis „Läuft demnächst ab“ in Event Grid.
1. Event Grid überprüft die Ereignisabonnements und ruft mit HTTP POST den Funktions-App-Endpunkt auf, der dieses Ereignis abonniert hat.
1. Die Funktions-App identifiziert den Alternativschlüssel (anderer Schlüssel als der aktuelle) und ruft das Speicherkonto auf, um die erneute Generierung durchzuführen.
1. Die Funktions-App fügt den erneut generierten Schlüssel Azure Key Vault als neue Version des Geheimnisses hinzu.

## <a name="prerequisites"></a>Voraussetzungen
* Azure-Abonnement ([kostenloses Abonnement erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))
* Azure-Schlüsseltresor
* Zwei Azure Storage-Konten

Sie können den unten angegebenen Bereitstellungslink verwenden, falls Sie nicht über vorhandene Key Vault- und Storage-Konten verfügen:

[![Bild einer Schaltfläche mit der Bezeichnung „Bereitstellung in Azure“](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2FKeyVault-Rotation-StorageAccountKey-PowerShell%2Fmaster%2Farm-templates%2FInitial-Setup%2Fazuredeploy.json)

1. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus. Geben Sie der Gruppe den Namen **akvrotation**, und klicken Sie auf **OK**.
1. Wählen Sie **Bewerten + erstellen** aus.
1. Klicken Sie auf **Erstellen**

    ![Erstellen einer Ressourcengruppe](../media/secrets/rotation-dual/dual-rotation-1.png)

Sie verfügen jetzt über einen Schlüsseltresor und zwei Speicherkonten. Sie können dieses Setup in der Azure-Befehlszeilenschnittstelle überprüfen, indem Sie den folgenden Befehl ausführen:

```azurecli
az resource list -o table -g akvrotation
```

Das Ergebnis sieht in etwa wie die folgende Ausgabe aus:

```console
Name                     ResourceGroup         Location    Type                               Status
-----------------------  --------------------  ----------  ---------------------------------  --------
akvrotation-kv         akvrotation      eastus      Microsoft.KeyVault/vaults
akvrotationstorage     akvrotation      eastus      Microsoft.Storage/storageAccounts
akvrotationstorage2    akvrotation      eastus      Microsoft.Storage/storageAccounts
```

## <a name="create-and-deploy-storage-account-key-rotation-function"></a>Erstellen und Bereitstellen der Funktion für die Rotation von Speicherkontoschlüsseln
> [!IMPORTANT]
> Für die folgende Vorlage müssen sich Key Vault, Azure Storage-Konto und Azure-Funktion in der gleichen Ressourcengruppe befinden.

Erstellen Sie als Nächstes zusätzlich zu den anderen erforderlichen Komponenten eine Funktions-App mit einer systemseitig verwalteten Identität, und stellen Sie Funktionen für die Rotation von Speicherkontoschlüsseln bereit.

Für die Rotationsfunktionen der Funktions-App werden die unten angegebenen Komponenten und die folgende Konfiguration benötigt:
- Einen Azure App Service-Plan
- Ein Speicherkonto, das für die Triggerverwaltung der Funktions-App benötigt wird
- Eine Zugriffsrichtlinie zum Zugreifen auf Geheimnisse in Key Vault
- Zuweisung der Dienstrolle „Speicherkonto-Schlüsseloperator“ zur Funktions-App zum Zugreifen auf Speicherkonto-Zugriffsschlüssel
- Rotationsfunktionen für Speicherkontoschlüssel mit Ereignisauslöser und HTTP-Trigger (bedarfsgesteuerte Rotation)
- Event Grid-Ereignisabonnement für das Ereignis **SecretNearExpiry**

1. Wählen Sie den Link zur Bereitstellung der Vorlage in Azure aus: 

   [![Bild einer Schaltfläche mit der Bezeichnung „Bereitstellung in Azure“](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2FKeyVault-Rotation-StorageAccountKey-PowerShell%2Fmaster%2Farm-templates%2FFunction%2Fazuredeploy.json)

1. Wählen Sie in der Liste **Ressourcengruppe** die Option **akvrotation** aus.
1. Geben Sie unter **Name des Speicherkontos** den Namen des Speicherkontos ein, für das Zugriffsschlüssel rotiert werden sollen.
1. Geben Sie unter **Schlüsseltresorname** den Namen des Schlüsseltresors ein.
1. Geben Sie unter **Name der Funktions-App** den Namen der Funktions-App ein.
1. Geben Sie unter **Name des Geheimnisses** den Namen des Geheimnisses an, unter dem die Zugriffsschlüssel gespeichert werden.
1. Geben Sie unter **Repository-URL** den GitHub-Speicherort des Funktionscodes an ( **https://github.com/jlichwa/KeyVault-Rotation-StorageAccountKey-PowerShell.git** ).
1. Wählen Sie **Bewerten + erstellen** aus.
1. Klicken Sie auf **Erstellen**

   ![Überprüfen und Erstellen des ersten Speicherkontos](../media/secrets/rotation-dual/dual-rotation-2.png)

Nachdem Sie die obigen Schritte ausgeführt haben, verfügen Sie über ein Speicherkonto, eine Serverfarm, eine Funktions-App und eine Application Insights-Instanz. Nach Abschluss der Bereitstellung sollte der folgende Bildschirm angezeigt werden: ![Bereitstellung abgeschlossen](../media/secrets/rotation-dual/dual-rotation-3.png)
> [!NOTE]
> Falls Fehler auftreten, können Sie auf **Erneut bereitstellen** klicken, um die Bereitstellung der restlichen Komponenten abzuschließen.


Bereitstellungsvorlagen und den Code für die Rotationsfunktionen finden Sie auf [GitHub](https://github.com/jlichwa/KeyVault-Rotation-StorageAccountKey-PowerShell).

## <a name="add-storage-account-access-key-to-key-vault"></a>Hinzufügen eines Speicherkonto-Zugriffsschlüssels zu Key Vault

Legen Sie zuerst Ihre Zugriffsrichtlinie so fest, dass für Benutzer Berechtigungen zum *Verwalten von Geheimnissen* gewährt werden:

```azurecli
az keyvault set-policy --upn <email-address-of-user> --name akvrotation-kv --secret-permissions set delete get list
```

Sie können jetzt ein neues Geheimnis mit einem Speicherkonto-Zugriffsschlüssel als Wert erstellen. Darüber hinaus benötigen Sie die Ressourcen-ID des Speicherkontos, die Gültigkeitsdauer des Geheimnisses und die Schlüssel-ID zum Hinzufügen zum Geheimnis, damit der Schlüssel von der Rotationsfunktion im Speicherkonto erneut generiert werden kann.

Rufen Sie die Ressourcen-ID für das Speicherkonto ab. Sie finden den Wert unter der Eigenschaft `id`.
```azurecli
az storage account show -n akvrotationstorage
```

Listen Sie die Speicherkonto-Zugriffsschlüssel zum Abrufen von Schlüsselwerten auf.

```azurecli
az storage account keys list -n akvrotationstorage 
```

Fügen Sie die abgerufenen Werte für **key1Value** und **storageAccountResourceId** ein.

```azurecli
$tomorrowDate = (get-date).AddDays(+1).ToString("yyy-MM-ddThh:mm:ssZ")
az keyvault secret set --name storageKey --vault-name akvrotation-kv --value <key1Value> --tags "CredentialId=key1" "ProviderAddress=<storageAccountResourceId>" "ValidityPeriodDays=60" --expires $tomorrowDate
```

Wenn Sie ein Geheimnis mit einem kurzen Ablaufdatum erstellen, wird innerhalb weniger Minuten ein Ereignis vom Typ `SecretNearExpiry` veröffentlicht, wodurch wiederum die Funktion zum Rotieren des Geheimnisses ausgelöst wird.

Sie können überprüfen, ob Zugriffsschlüssel erneut generiert werden, indem Sie die Speicherkontoschlüssel und das Key Vault-Geheimnis abrufen und miteinander vergleichen.

Mit dem folgenden Befehl können Sie die Informationen zum Geheimnis anzeigen:
```azurecli
az keyvault secret show --vault-name akvrotation-kv --name storageKey
```
Beachten Sie, dass `CredentialId` auf den alternativen `keyName` aktualisiert wird und `value` die erneut generierte ![Ausgabe von „az keyvault secret show“ für das erste Speicherkonto](../media/secrets/rotation-dual/dual-rotation-4.png) ist.

Rufen Sie die Zugriffsschlüssel ab, um den Wert zu überprüfen.
```azurecli
az storage account keys list -n akvrotationstorage 
```
![Ausgabe von „az storage account keys list“ für das erste Speicherkonto](../media/secrets/rotation-dual/dual-rotation-5.png)

## <a name="add-additional-storage-accounts-for-rotation"></a>Hinzufügen zusätzlicher Speicherkonten zur Rotation

Sie können dieselbe Funktions-App wiederverwenden, um die Rotation für mehrere Speicherkonten durchzuführen. 

Sie benötigen Folgendes, um für eine vorhandene Funktion zusätzliche Speicherkontoschlüssel für die Rotation hinzuzufügen:
- Zuweisung der Dienstrolle „Speicherkonto-Schlüsseloperator“ zur Funktions-App zum Zugreifen auf Speicherkonto-Zugriffsschlüssel
- Event Grid-Ereignisabonnement für das Ereignis **SecretNearExpiry**

1. Wählen Sie den Link zur Bereitstellung der Vorlage in Azure aus: 

   [![Bild einer Schaltfläche mit der Bezeichnung „Bereitstellung in Azure“](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjlichwa%2FKeyVault-Rotation-StorageAccountKey-PowerShell%2Fmaster%2Farm-templates%2FAdd-Event-Subscription%2Fazuredeploy.json)

1. Wählen Sie in der Liste **Ressourcengruppe** die Option **akvrotation** aus.
1. Geben Sie unter **Name des Speicherkontos** den Namen des Speicherkontos ein, für das Zugriffsschlüssel rotiert werden sollen.
1. Geben Sie unter **Schlüsseltresorname** den Namen des Schlüsseltresors ein.
1. Geben Sie unter **Name der Funktions-App** den Namen der Funktions-App ein.
1. Geben Sie unter **Name des Geheimnisses** den Namen des Geheimnisses an, unter dem die Zugriffsschlüssel gespeichert werden.
1. Wählen Sie **Bewerten + erstellen** aus.
1. Klicken Sie auf **Erstellen**

   ![Überprüfen und Erstellen des zweiten Speicherkontos](../media/secrets/rotation-dual/dual-rotation-7.png)

### <a name="add-another-storage-account-access-key-to-key-vault"></a>Hinzufügen eines weiteren Speicherkonto-Zugriffsschlüssels zu Key Vault

Rufen Sie die Ressourcen-ID für das Speicherkonto ab. Sie finden den Wert unter der Eigenschaft `id`.
```azurecli
az storage account show -n akvrotationstorage2
```

Listen Sie die Speicherkonto-Zugriffsschlüssel zum Abrufen des Werts von „key2“ auf.

```azurecli
az storage account keys list -n akvrotationstorage2 
```

Fügen Sie die abgerufenen Werte für **key2Value** und **storageAccountResourceId** ein.

```azurecli
tomorrowDate=`date -d tomorrow -Iseconds -u | awk -F'+' '{print $1"Z"}'`
az keyvault secret set --name storageKey2 --vault-name akvrotation-kv --value <key2Value> --tags "CredentialId=key2" "ProviderAddress=<storageAccountResourceId>" "ValidityPeriodDays=60" --expires $tomorrowDate
```

Verwenden Sie den folgenden Befehl, um die Informationen zum Geheimnis anzuzeigen:
```azurecli
az keyvault secret show --vault-name akvrotation-kv --name storageKey2
```
Beachten Sie, dass `CredentialId` auf den alternativen `keyName` aktualisiert wird und `value` die erneut generierte ![Ausgabe von „az keyvault secret show“ für das zweite Speicherkonto](../media/secrets/rotation-dual/dual-rotation-8.png) ist.

Rufen Sie die Zugriffsschlüssel ab, um den Wert zu überprüfen.
```azurecli
az storage account keys list -n akvrotationstorage 
```
![Ausgabe von „az storage account keys list“ für das zweite Speicherkonto](../media/secrets/rotation-dual/dual-rotation-9.png)

## <a name="available-key-vault-dual-credential-rotation-functions"></a>Verfügbare Key Vault-Funktionen für die Rotation von dualen Anmeldeinformationen

- [Speicherkonto](https://github.com/jlichwa/KeyVault-Rotation-StorageAccountKey-PowerShell)
- [Redis Cache](https://github.com/jlichwa/KeyVault-Rotation-RedisCacheKey-PowerShell)

## <a name="learn-more"></a>Weitere Informationen
- Übersicht: [Überwachen von Key Vault mit Azure Event Grid](../general/event-grid-overview.md)
- Gewusst wie: [Erstellen Ihrer ersten Funktion im Azure-Portal](../../azure-functions/functions-create-first-azure-function.md)
- Gewusst wie: [Verwenden von Logic Apps zum Empfangen einer E-Mail bei Statusänderungen von Key Vault-Geheimnissen](../general/event-grid-logicapps.md)
- [Azure Event Grid-Ereignisschema für Azure Key Vault](../../event-grid/event-schema-key-vault.md)
