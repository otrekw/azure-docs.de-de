---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: 6dfc96df3623e44a4ba513c238f21482c2a2989d
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "87013103"
---
Die DefaultAzureCredential-Methode in unserer Anwendung basiert auf drei Umgebungsvariablen: `AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET` und `AZURE_TENANT_ID`. Legen Sie diese Variablen auf die clientId-, clientSecret- und tenantId-Werte fest, die im Schritt „Erstellen eines Dienstprinzipals“ mit dem Format `export VARNAME=VALUE` zurückgegeben wurden. (Mit dieser Methode werden nur die Variablen für Ihre aktuelle Shell und daraus erstellte Prozesse festgelegt. Um diese Variablen dauerhaft zu Ihrer Umgebung hinzuzufügen, bearbeiten Sie Ihre Datei `/etc/environment `.) 

Sie müssen außerdem den Namen Ihres Schlüsseltresors als Umgebungsvariable `KEY_VAULT_NAME` speichern.

```console
export AZURE_CLIENT_ID=<your-clientID>

export AZURE_CLIENT_SECRET=<your-clientSecret>

export AZURE_TENANT_ID=<your-tenantId>

export KEY_VAULT_NAME=<your-key-vault-name>
````
