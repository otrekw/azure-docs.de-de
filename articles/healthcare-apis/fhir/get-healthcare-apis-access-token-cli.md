---
title: 'Abrufen von Zugriffstoken mithilfe der Azure CLI: Azure API for FHIR'
description: In diesem Artikel wird erläutert, wie Sie ein Zugriffstoken für die Azure API for FHIR über die Azure CLI erhalten.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: matjazl
ms.openlocfilehash: b7bdb7f4a22d080f382fea984e710980428067ff
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103019147"
---
# <a name="get-access-token-for-azure-api-for-fhir-using-azure-cli"></a>Abrufen von Zugriffstoken für die Azure API for FHIR mit Azure CLI

In diesem Artikel erfahren Sie, wie Sie mithilfe der Azure CLI ein Zugriffstoken für die Azure API for FHIR erhalten. Wenn Sie [die Azure API for FHIR bereitstellen](fhir-paas-portal-quickstart.md), konfigurieren Sie eine Reihe von Benutzern oder Dienstprinzipalen mit Zugriff auf den Dienst. Wenn sich Ihre Benutzerobjekt-ID in der Liste der zulässigen Objekt-IDs befindet, können Sie auf den Dienst über ein Token zugreifen, das Sie über die Azure CLI erhalten haben.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

## <a name="obtain-a-token"></a>Erhalten eines Tokens

Die Azure API for FHIR verwendet `resource` oder `Audience` mit einer URI, die der URI des FHIR-Servers entspricht: `https://<FHIR ACCOUNT NAME>.azurehealthcareapis.com`. Sie können ein Token erhalten und mit dem folgenden Befehl in einer Variablen (namens `$token`) speichern:

```azurecli-interactive
token=$(az account get-access-token --resource=https://<FHIR ACCOUNT NAME>.azurehealthcareapis.com --query accessToken --output tsv)
```

## <a name="use-with-azure-api-for-fhir"></a>Verwenden mit Azure API for FHIR

```azurecli-interactive
curl -X GET --header "Authorization: Bearer $token" https://<FHIR ACCOUNT NAME>.azurehealthcareapis.com/Patient
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie mithilfe der Azure CLI ein Zugriffstoken für die Azure API for FHIR erhalten. Informationen zum Zugriff auf die FHIR-API mit Postman finden Sie im Tutorial zu Postman.

>[!div class="nextstepaction"]
>[Zugriff auf die FHIR-API mit Postman](access-fhir-postman-tutorial.md)
