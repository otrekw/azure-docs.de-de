---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 09/03/2020
ms.author: msmbaldwin
ms.openlocfilehash: 9440a33858cb70389e86621ae76be9c08ec56d61
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "89482125"
---
1. Achten Sie darauf, dass Sie über ein [Azure-Konto mit einem aktiven Abonnement](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) verfügen.

1. Installieren Sie [Python 2.7+ oder 3.5.3+](https://www.python.org/downloads).

1. Installieren Sie die [Azure CLI](/cli/azure/install-azure-cli).

1. Befolgen Sie die Anweisungen unter dem Thema zum [Konfigurieren der Authentifizierung für eine lokale Bereitstellung](/azure/developer/python/configure-local-development-environment?tabs=cmd#configure-authentication). So erstellen Sie einen lokalen Dienstprinzipal und stellen diesen über Umgebungsvariablen für den Azure Key Vault-Client für Python bereit. 

    Wenn Sie Code direkt in Azure ausführen, wird kein separater Dienstprinzipal benötigt, wenn die App eine verwaltete Identität verwendet.

1. Erstellen Sie in einem Terminal oder an einer Eingabeaufforderung einen geeigneten Projektordner, und erstellen und aktivieren Sie dann eine virtuelle Python-Umgebung, wie unter [Verwenden von virtuellen Python-Umgebungen](/azure/developer/python/configure-local-development-environment?tabs=cmd#use-python-virtual-environments) beschrieben.

1. Installieren der Azure Active Directory-Identitätsbibliothek:

    ```terminal
    pip install azure.identity
    ```
