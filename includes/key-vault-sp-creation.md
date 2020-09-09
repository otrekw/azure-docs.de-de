---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: deb2eb877743d533c5daeee8b6636edd62418fe0
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2020
ms.locfileid: "89494262"
---
Die Authentifizierung einer cloudbasierten Anwendung lässt sich am einfachsten mit einer verwalteten Identität bewerkstelligen. Einzelheiten hierzu finden Sie unter [Authentifizieren bei Azure Key Vault](/azure/key-vault/general/authentication).

Der Einfachheit halber wird in dieser Schnellstartanleitung eine Desktopanwendung erstellt, die die Verwendung eines Dienstprinzipals und einer Zugriffssteuerungsrichtlinie erfordert. Ihr Dienstprinzipal muss einen eindeutigen Namen im Format „http://&lt;eindeutiger Dienstprinzipalname&gt;“ haben.

Erstellen Sie mithilfe des Azure CLI-Befehls [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) einen Dienstprinzipal:

```azurecli
az ad sp create-for-rbac --skip-assignment -n "http://<my-unique-service-principal-name>" --sdk-auth
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
