---
title: 'Schnellstart: Erstellen und Verwalten von Zugriffstoken'
titleSuffix: An Azure Communication Services quickstart
description: Hier erfahren Sie, wie Sie Identitäten und Zugriffstoken mithilfe der Azure Communication Services-Clientbibliothek „Administration“ verwalten.
author: tomaschladek
manager: nmurav
services: azure-communication-services
ms.author: tchladek
ms.date: 08/20/2020
ms.topic: quickstart
ms.service: azure-communication-services
zone_pivot_groups: acs-js-csharp-java-python
ms.openlocfilehash: b67d0808643797d88628b626403c1b9d97cf1cad
ms.sourcegitcommit: 4bee52a3601b226cfc4e6eac71c1cb3b4b0eafe2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2020
ms.locfileid: "94506217"
---
# <a name="quickstart-create-and-manage-access-tokens"></a>Schnellstart: Erstellen und Verwalten von Zugriffstoken

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Steigen Sie in Azure Communication Services ein, indem Sie die Communication Services-Verwaltungsclientbibliothek verwenden. Sie ermöglicht es Ihnen, Identitäten zu erstellen und Ihre Zugriffstoken zu verwalten. Die Identität stellt die Entität Ihrer Anwendung in Azure Communication Service (z. B. Benutzer oder Gerät) dar. Zugriffstoken ermöglichen die direkte Authentifizierung Ihrer Clientbibliotheken für Chats und Telefonie bei Azure Communication Services. Es wird empfohlen, Zugriffstoken mit einem serverseitigen Dienst zu erstellen. Zugriffstoken werden dann verwendet, um die Communication Services-Clientbibliotheken auf Clientgeräten zu initialisieren.

Sämtliche Preise, die auf Abbildungen in diesem Tutorial zu sehen sind, dienen lediglich Demonstrationszwecken.

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

Created an identity: 8:acs:4ccc92c8-9815-4422-bddc-ceea181dc774_00000006-19e0-2727-80f5-8b3a0d003502

Issued a access token with 'voip' scope for identity with ID: 8:acs:4ccc92c8-9815-4422-bddc-ceea181dc774_00000006-19e0-2727-80f5-8b3a0d003502:
<token signature here>

Successfully deleted the identity with ID: 8:acs:4ccc92c8-9815-4422-bddc-ceea181dc774_00000006-19e0-2727-80f5-8b3a0d003502

Deleted the identity with ID: 8:acs:4ccc92c8-9815-4422-bddc-ceea181dc774_00000006-19e0-2727-80f5-8b3a0d003502
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
 