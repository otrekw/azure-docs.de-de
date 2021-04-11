---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 09/03/2020
ms.author: msmbaldwin
ms.openlocfilehash: 59359d37fe347c8568c7944f75accdbc04cddb93
ms.sourcegitcommit: f5448fe5b24c67e24aea769e1ab438a465dfe037
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105967120"
---
1. Verwenden Sie den Befehl `az group create`, um eine Ressourcengruppe zu erstellen:

    ```azurecli
    az group create --name KeyVault-PythonQS-rg --location eastus
    ```

    Falls gewünscht, können Sie „eastus“ in eine Region ändern, die näher an Ihrem Standort liegt.

1. Verwenden Sie `az keyvault create`, um den Schlüsseltresor zu erstellen:

    ```azurecli
    az keyvault create --name <your-unique-keyvault-name> --resource-group KeyVault-PythonQS-rg
    ```

    Ersetzen Sie `<your-unique-keyvault-name>` durch einen Namen, der innerhalb von Azure eindeutig ist. In der Regel verwenden Sie Ihren persönlichen oder Firmennamen zusammen mit Ziffern und anderen Bezeichnern. 

1. Erstellen Sie eine Umgebungsvariable, die den Namen des Schlüsseltresors für den Code bereitstellt:

    ```bash
    export KEY_VAULT_NAME=<your-unique-keyvault-name>
    ```