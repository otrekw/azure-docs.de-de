---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 09/03/2020
ms.author: msmbaldwin
ms.openlocfilehash: 5e912b76c2ef68aa01dae57d1b42abc386a8e67b
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "89482126"
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

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```cmd
    set KEY_VAULT_NAME=<your-unique-keyvault-name>
    ```

    # <a name="bash"></a>[Bash](#tab/bash)

    ```bash
    export KEY_VAULT_NAME=<your-unique-keyvault-name>
    ```

    ---
