---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: d669874040b04af089b4d57333869fe34066720e
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841836"
---
Eine cloudbasierte Anwendung kann am einfachsten mit einer verwalteten Identität authentifiziert werden. Einzelheiten hierzu finden Sie unter [Verwenden einer verwalteten App Service-Identität für den Zugriff auf Azure Key Vault](/azure/key-vault/general/managed-identity). 

Der Einfachheit halber wird in dieser Schnellstartanleitung eine Desktopanwendung erstellt, die die Verwendung eines Dienstprinzipals und einer Zugriffssteuerungsrichtlinie erfordert. Ihr Dienstprinzipal muss einen eindeutigen Namen im Format „http://&lt;eindeutiger Dienstprinzipalname&gt;“ haben.

Erstellen Sie mithilfe des Azure CLI-Befehls [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) einen Dienstprinzipal:

```azurecli
az ad sp create-for-rbac -n "http://<my-unique-service-principal-name>" --sdk-auth
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
