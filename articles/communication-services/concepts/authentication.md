---
title: Authentifizieren bei Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Erfahren Sie mehr über die Möglichkeiten, wie sich eine App oder ein Dienst bei Communication Services authentifizieren kann.
author: GrantMeStrength
manager: jken
services: azure-communication-services
ms.author: jken
ms.date: 07/24/2020
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: 1267fc53bd6dcbae504b01610267059545353dc5
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101655903"
---
# <a name="authenticate-to-azure-communication-services"></a>Authentifizieren bei Azure Communication Services

Jede Clientinteraktion mit Azure Communication Services muss authentifiziert werden. In einer typischen Architektur (siehe [Client-und Serverarchitektur](./client-and-server-architecture.md)) werden im vertrauenswürdigen Benutzerzugriffsdienst *Zugriffsschlüssel* oder *verwaltete Identitäten* zum Erstellen von Benutzern und Ausstellen von Token verwendet. Ein vom vertrauenswürdigen Benutzerzugriffsdienst ausgestelltes *Benutzerzugriffstoken* wird für Clientanwendungen für den Zugriff auf andere Kommunikationsdienste (etwa Chat- oder Telefoniedienste) verwendet.

Der SMS-Dienst von Azure Communication Services akzeptiert auch *Zugriffsschlüssel* oder eine *verwaltete Identität* für die Authentifizierung. Dies geschieht in der Regel in einer Dienstanwendung, die in einer vertrauenswürdigen Dienstumgebung ausgeführt wird.

Im Anschluss werden die einzelnen Autorisierungsoptionen kurz erläutert:

- Die Authentifizierung per **Zugriffsschlüssel** für Vorgänge in „SMS“ oder „Identität“. Die Authentifizierung per Zugriffsschlüssel eignet sich für Dienstanwendungen, die in einer vertrauenswürdigen Dienstumgebung ausgeführt werden. Den Zugriffsschlüssel finden Sie im Azure Communication Services-Portal. Zum Authentifizieren mit einem Zugriffsschlüssel verwendet eine Dienstanwendung den Zugriffsschlüssel als Anmeldeinformationen, um entsprechende SMS- oder Identitätsclientbibliotheken zu initialisieren. Weitere Informationen finden Sie unter [Erstellen und Verwalten von Zugriffstoken](../quickstarts/access-tokens.md). Da der Zugriffsschlüssel Teil der Verbindungszeichenfolge Ihrer Ressource ist (siehe [Erstellen und Verwalten einer Communication Services-Ressource](../quickstarts/create-communication-resource.md)), entspricht die Authentifizierung mit Verbindungszeichenfolge der Authentifizierung mit Zugriffsschlüssel.
- Die Authentifizierung per **verwaltete Identität** für Vorgänge in „SMS“ oder „Identität“. Die verwaltete Identität (siehe [Verwaltete Identität](../quickstarts/managed-identity.md)) eignet sich für Dienstanwendungen, die in einer vertrauenswürdigen Dienstumgebung ausgeführt werden. Zum Authentifizieren mit einer verwalteten Identität erstellt eine Dienstanwendung Anmeldeinformationen mit der ID und einem Geheimnis der verwalteten Identität und initialisiert dann entsprechende SMS- oder Identitätsclientbibliotheken. Weitere Informationen finden Sie unter [Erstellen und Verwalten von Zugriffstoken](../quickstarts/access-tokens.md).
- Die Authentifizierung per **Benutzerzugriffstoken** ist für „Chat“ und „Calling“ vorgesehen. Benutzerzugriffstoken ermöglichen die direkte Authentifizierung Ihrer Clientanwendungen bei Chat- und Telefoniediensten von Azure Communication Services. Diese Token werden in einem von Ihnen erstellten „vertrauenswürdigen Benutzerzugriffsdienst“ generiert. Anschließend werden sie Clientgeräten zur Verfügung gestellt, die das Token zur Initialisierung der Clientbibliotheken „Chat“ und „Calling“ verwenden. Weitere Informationen finden Sie beispielsweise unter [Hinzufügen von Chatfunktionen zu Ihrer App](../quickstarts/chat/get-started.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen und Verwalten von Communication Services-Ressourcen](../quickstarts/create-communication-resource.md)
> [Erstellen einer Azure Active Directory-Anwendung für verwaltete Identitäten über die Azure CLI](../quickstarts/managed-identity-from-cli.md)
> [Erstellen von Benutzerzugriffstoken](../quickstarts/access-tokens.md)

Weitere Informationen finden Sie in den folgenden Artikeln:
- [Erfahren Sie mehr über die Client- und Serverarchitektur](../concepts/client-and-server-architecture.md)
