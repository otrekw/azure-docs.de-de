---
title: 'Schnellstart: Erstellen und Verwalten von Zugriffstoken'
titleSuffix: An Azure Communication Services quickstart
description: Hier erfahren Sie, wie Sie Identitäten und Zugriffstoken mithilfe der Azure Communication Services-Clientbibliothek „Administration“ verwalten.
author: tomaschladek
manager: jken
services: azure-communication-services
ms.author: tchladek
ms.date: 08/20/2020
ms.topic: quickstart
ms.service: azure-communication-services
zone_pivot_groups: acs-js-csharp-java-python
ms.openlocfilehash: e323f1f50fe6c67a841c300fcbec1eed3afc4497
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92074123"
---
# <a name="quickstart-create-and-manage-access-tokens"></a>Schnellstart: Erstellen und Verwalten von Zugriffstoken

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Steigen Sie in Azure Communication Services ein, indem Sie Ihre Zugriffstoken mithilfe der Communication Services-Clientbibliothek „Administration“ bereitstellen und verwalten. Zugriffstoken ermöglichen die direkte Authentifizierung Ihrer Clientbibliotheken für Chats und Telefonie bei Azure Communication Services. Diese Token werden über einen von Ihnen implementierten serverseitigen Tokenbereitstellungsdienst generiert. Danach werden sie verwendet, um die Communication Services-Clientbibliotheken auf Clientgeräten zu initialisieren.

Beachten Sie, dass es sich bei sämtlichen Preisen, die auf Bildern in diesem Tutorial zu sehen sind, lediglich um Beispiele handelt.

::: zone pivot="programming-language-csharp"
[!INCLUDE [.NET](./includes/user-access-token-net.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript](./includes/user-access-token-js.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python](./includes/user-access-token-python.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java](./includes/user-access-token-java.md)]
::: zone-end

In der Ausgabe der App wird die jeweils abgeschlossene Aktion beschrieben:
<!---cSpell:disable --->
```console
Azure Communication Services - Access Tokens Quickstart

Issued a access token with 'voip' scope for identity with ID: 8:acs:fecfaddf-bf2c-4a0e-b52f-7d918c9536e6_65012b-1400da9050:
<token signature here>

Issued a access token with 'chat' scope for identity with ID: 8:acs:fecfaddf-bf2c-4a0e-b52f-7d918c9536e6_65012b-1400da9050:
<token signature here>

Successfully deleted the identity with ID: 8:acs:fecfaddf-bf2c-4a0e-b52f-7d918c9536e6_65012b-1400da9050

Deleted the identity with ID: 8:acs:fecfaddf-bf2c-4a0e-b52f-7d918c9536e6_65012b-1400da9050
```
<!---cSpell:enable --->

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Communication Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind. Weitere Informationen zum Bereinigen von Ressourcen finden Sie [hier](./create-communication-resource.md#clean-up-resources).


## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Identitäten verwalten
> * Ausstellen von Zugriffstoken
> * Verwenden der Communication Services-Clientbibliothek „Administration“


> [!div class="nextstepaction"]
> [Hinzufügen von Sprachanrufen zu Ihrer App](./voice-video-calling/getting-started-with-calling.md)

Das könnte Sie auch interessieren:

 - [Informationen zur Authentifizierung](../concepts/authentication.md)
 - [Hinzufügen von Chatfunktionen zu Ihrer App](./chat/get-started.md)
 - [Informationen zur Client- und Serverarchitektur](../concepts/client-and-server-architecture.md)
