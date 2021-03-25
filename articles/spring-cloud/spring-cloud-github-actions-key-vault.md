---
title: Authentifizieren von Azure Spring Cloud mit Schlüsseltresor in GitHub Actions
description: Hier erfahren Sie, wie Sie einen Schlüsseltresor mit CI/CD-Workflow für Azure Spring Cloud mit GitHub Actions verwenden.
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 09/08/2020
ms.custom: devx-track-java
ms.openlocfilehash: 0ea0db1faf8c452958b8d95c193d45506057777c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98673331"
---
# <a name="authenticate-azure-spring-cloud-with-key-vault-in-github-actions"></a>Authentifizieren von Azure Spring Cloud mit Schlüsseltresor in GitHub Actions

**Dieser Artikel gilt für:** ✔️ Java ✔️ C#

Ein Schlüsseltresor ist ein sicherer Ort zum Speichern von Schlüsseln. Unternehmensbenutzer müssen Anmeldeinformationen für CI/CD-Umgebungen im von ihnen kontrollierten Bereich speichern. Der Schlüssel zum Abrufen von Anmeldeinformationen im Schlüsseltresor muss auf den Ressourcenbereich beschränkt sein.  Er ermöglicht nur den Zugriff auf den Schlüsseltresorbereich und nicht auf den gesamten Azure-Bereich. Somit ist er vergleichbar mit einem Schlüssel, der nur ein bestimmtes Schließfach öffnet (im Gegensatz zu einem Hauptschlüssel, mit dem sämtliche Türen in einem Gebäude geöffnet werden können). Er ermöglicht das Abrufen eines Schlüssels mithilfe eines anderen Schlüssels, was in einem CI/CD-Workflow hilfreich ist. 

## <a name="generate-credential"></a>Generieren von Anmeldeinformationen
Führen Sie auf Ihrem lokalen Computer den folgenden Befehl aus, um den Schlüssel für den Zugriff auf den Schlüsseltresor zu generieren:

```azurecli
az ad sp create-for-rbac --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP>/providers/Microsoft.KeyVault/vaults/<KEY_VAULT> --sdk-auth
```
Der durch den Parameter `--scopes` angegebene Bereich beschränkt den Schlüsselzugriff auf die Ressource.  Er kann nur auf das Schließfach zugreifen.

Ergebnisse:
```output
{
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
}
```
Speichern Sie die Ergebnisse in GitHub-**Geheimnissen**, wie unter [Einrichten des GitHub-Repositorys und Durchführen der Authentifizierung](./spring-cloud-howto-github-actions.md#set-up-github-repository-and-authenticate) beschrieben.

## <a name="add-access-policies-for-the-credential"></a>Hinzufügen von Zugriffsrichtlinien für die Anmeldeinformationen
Mit den oben erstellten Anmeldeinformationen können nur allgemeine Informationen zur Key Vault-Instanz abgerufen werden, aber nicht der darin gespeicherte Inhalt.  Zum Abrufen von in der Key Vault-Instanz gespeicherten Geheimnissen müssen Zugriffsrichtlinien für die Anmeldeinformationen festgelegt werden.

Navigieren Sie im Azure-Portal zum Dashboard **Key Vault**, klicken Sie auf das Menü **Zugriffssteuerung**, und öffnen Sie die Registerkarte **Rollenzuweisungen**. Wählen Sie unter **Typ** die Option **Apps** und unter **Bereich** die Option `This resource` aus.  Daraufhin sollten die im vorherigen Schritt erstellten Anmeldeinformationen angezeigt werden:

 ![Festlegen der Zugriffsrichtlinie](./media/github-actions/key-vault1.png)

Kopieren Sie den Anmeldeinformationsnamen (beispielsweise `azure-cli-2020-01-19-04-39-02`). Öffnen Sie das Menü **Zugriffsrichtlinien**, und klicken Sie auf den Link **+ Zugriffsrichtlinie hinzufügen**.  Wählen Sie unter **Vorlage** die Option `Secret Management` und anschließend **Prinzipal** aus. Fügen Sie den Anmeldeinformationsnamen in das Eingabefeld unter **Prinzipal**/**Auswählen** ein:

 ![Auswählen](./media/github-actions/key-vault2.png)

 Klicken Sie im Dialogfeld **Zugriffsrichtlinie hinzufügen** auf die Schaltfläche **Hinzufügen**, und klicken Sie anschließend auf **Speichern**.

## <a name="generate-full-scope-azure-credential"></a>Generieren von Azure-Anmeldeinformationen für den gesamten Bereich
Dies ist der Hauptschlüssel, mit dem alle Türen im Gebäude geöffnet werden können. Die Vorgehensweise ähnelt dem vorherigen Schritt. In diesem Fall ändern wir aber den Bereich, um den Hauptschlüssel zu generieren:

```azurecli
az ad sp create-for-rbac --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID> --sdk-auth
```

Ergebnisse:
```output
{
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
}
```
Kopieren Sie die gesamte JSON-Zeichenfolge.  Kehren Sie zum Dashboard **Key Vault** zurück. Öffnen Sie das Menü **Geheimnisse**, und klicken Sie auf die Schaltfläche **Generieren/importieren**. Geben Sie den Namen des Geheimnisses ein (beispielsweise `AZURE-CREDENTIALS-FOR-SPRING`). Fügen Sie die JSON-Anmeldeinformationszeichenfolge in das Eingabefeld **Wert** ein. Wie Sie sehen, ist das Eingabefeld für den Wert kein Textbereich mit mehreren Zeilen, sondern ein einzeiliges Textfeld.  Dort können Sie die gesamte JSON-Zeichenfolge einfügen.

 ![Anmeldeinformationen für den gesamten Bereich](./media/github-actions/key-vault3.png)

## <a name="combine-credentials-in-github-actions"></a>Kombinieren von Anmeldeinformationen in GitHub Actions
Legen Sie die Anmeldeinformationen fest, die beim Ausführen der CI/CD-Pipeline verwendet werden:

```console
on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}           # Strong box key you generated in the first step
    - uses: Azure/get-keyvault-secrets@v1.0
      with:
        keyvault: "<Your Key Vault Name>"
        secrets: "AZURE-CREDENTIALS-FOR-SPRING"           # Master key to open all doors in the building
      id: keyvaultaction
    - uses: azure/login@v1
      with:
        creds: ${{ steps.keyvaultaction.outputs.AZURE-CREDENTIALS-FOR-SPRING }}
    - name: Azure CLI script
      uses: azure/CLI@v1
      with:
        azcliversion: 2.0.75
        inlineScript: |
          az extension add --name spring-cloud             # Spring CLI commands from here
          az spring-cloud list

```

## <a name="next-steps"></a>Nächste Schritte
* [Azure Spring Cloud: CI/CD mit GitHub Actions](./spring-cloud-howto-github-actions.md)
