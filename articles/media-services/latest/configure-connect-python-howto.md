---
title: Verbinden mit der Azure Media Services v3-API – Python
description: In diesem Artikel wird demonstriert, wie Sie mit Python eine Verbindung mit der Media Services v3-API herstellen.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 11/18/2020
ms.author: inhenkel
ms.custom: devx-track-python
ms.openlocfilehash: 24e2ba4027dc818256dc9572f697fe7ec5a5a56b
ms.sourcegitcommit: edc7dc50c4f5550d9776a4c42167a872032a4151
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105960689"
---
# <a name="connect-to-media-services-v3-api---python"></a>Verbinden mit der Media Services v3-API – Python

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Dieser Artikel zeigt Ihnen, wie Sie mit der Methode der Dienstprinzipalanmeldung eine Verbindung zum Azure Media Services v3 Python SDK herstellen.

## <a name="prerequisites"></a>Voraussetzungen

- Laden Sie Python von [python.org](https://www.python.org/downloads/) herunter.
- Achten Sie darauf, die Umgebungsvariable `PATH` festzulegen.
- [Erstellen Sie ein Media Services-Konto.](./account-create-how-to.md) Merken Sie sich unbedingt den Namen der Ressourcengruppe und den Namen des Media Services-Kontos.
- Führen Sie die Schritte im Thema [Zugreifen auf APIs](./access-api-howto.md) aus, und wählen Sie Dienstprinzipal als Authentifizierungsmethode aus. Halten Sie die Abonnement-ID (`SubscriptionId`), die Anwendungsclient-ID (`AadClientId`), den Authentifizierungsschlüssel (`AadSecret`) und die Mandanten-ID (`AadTenantId`) fest. Sie benötigen diese Angaben in einem späteren Schritt.

> [!IMPORTANT]
> Informieren Sie sich über die [Namenskonventionen](media-services-apis-overview.md#naming-conventions).

## <a name="install-the-modules"></a>Installieren der Module

Zum Arbeiten in Azure Media Services mit Python müssen Sie diese Module installieren.

* Das Modul `azure-mgmt-resource`, in dem Azure-Module für Active Directory enthalten sind.
* Das `azure-mgmt-media` Modul, das die Media Services-Entitäten enthält.

    Verwenden Sie unbedingt [die aktuelle Version des Media Services SDK für Python](https://pypi.org/project/azure-mgmt-media/).

Öffnen Sie ein Befehlszeilentool, und führen Sie die folgenden Befehle aus, um die Module zu installieren.

```
pip3 install azure-mgmt-resource
pip3 install azure-mgmt-media==3.0.0
```

## <a name="connect-to-the-python-client"></a>Herstellen der Verbindung mit dem Python-Client

1. Erstellen Sie eine Datei mit der Erweiterung `.py`.
1. Öffnen Sie die Datei in einem Text-Editor Ihrer Wahl.
1. Fügen Sie der Datei folgenden Code hinzu. Der Code importiert die erforderlichen Module und erstellt das Active Directory-Anmeldeinformationsobjekt, das zum Herstellen der Verbindung mit Media Services benötigt wird.

      Legen Sie die Werte der Variablen auf die Werte fest, die Sie in [Zugreifen auf APIs](./access-api-howto.md) erhalten haben. Aktualisieren Sie die Variablen `ACCOUNT_NAME` und `RESOUCE_GROUP_NAME` auf den Media Services-Kontonamen und den Namen der Ressourcengruppe, die beim Erstellen der Ressourcen verwendet wurden.

      ```
      import adal
      from azure.mgmt.media import AzureMediaServices
      from msrestazure.azure_active_directory import AdalAuthentication
      from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD

      RESOURCE = "https://management.core.windows.net/"
      # Tenant ID for your Azure Subscription (AadTenantId)
      TENANT_ID = "00000000-0000-0000-000000000000"
      # Your Service Principal App ID (AadClientId)
      CLIENT = "00000000-0000-0000-000000000000"
      # Your Service Principal Password (AadSecret)
      KEY = "00000000-0000-0000-000000000000"
      # Your Azure Subscription ID (SubscriptionId)
      SUBSCRIPTION_ID = "00000000-0000-0000-000000000000"
      # Your Azure Media Service (AMS) Account Name
      ACCOUNT_NAME = "amsaccount"
      # Your Resource Group Name
      RESOUCE_GROUP_NAME = "amsResourceGroup"

      LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
      RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

      context = adal.AuthenticationContext(LOGIN_ENDPOINT + "/" + TENANT_ID)
      credentials = AdalAuthentication(
          context.acquire_token_with_client_credentials, RESOURCE, CLIENT, KEY
      )

      # The AMS Client
      # You can now use this object to perform different operations to your AMS account.
      client = AzureMediaServices(credentials, SUBSCRIPTION_ID)

      print("signed in")

      # Now that you are authenticated, you can manipulate the entities.
      # For example, list assets in your AMS account
      print(client.assets.list(RESOUCE_GROUP_NAME, ACCOUNT_NAME).get(0))
      ```

1. Führen Sie die Datei aus.

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie das [Python SDK](https://aka.ms/ams-v3-python-sdk).
- Machen Sie sich mit der [Python-Referenzdokumentation](/python/api/overview/azure/mediaservices/management) vertraut.
