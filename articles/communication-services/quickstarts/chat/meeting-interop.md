---
title: Erste Schritte mit Teams-Interoperabilität unter Azure Communication Services
titleSuffix: An Azure Communication Services quickstart
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie mit dem Azure Communication Services Chat SDK einer Teams-Besprechung beitreten.
author: askaur
ms.author: askaur
ms.date: 06/30/2021
ms.topic: quickstart
ms.service: azure-communication-services
zone_pivot_groups: acs-web-ios
ms.openlocfilehash: c49f2a51dda269c8addcd32c2c82564455bc9743
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/30/2021
ms.locfileid: "113113095"
---
# <a name="quickstart-join-your-chat-app-to-a-teams-meeting"></a>Schnellstart: Beitreten mit einer Chat-App zu einer Teams-Besprechung

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include-chat.md)]

> [!IMPORTANT]
> Um die [Interoperabilität von Teams-Mandanten](../../concepts/teams-interop.md) zu aktivieren/deaktivieren, füllen Sie [dieses Formular](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR21ouQM6BHtHiripswZoZsdURDQ5SUNQTElKR0VZU0VUU1hMOTBBMVhESS4u) aus.

Steigen Sie in die Nutzung von Azure Communication Services ein, indem Sie Ihre Chatlösung mit Microsoft Teams verbinden. 

::: zone pivot="platform-web"
[!INCLUDE [Teams interop with JavaScript SDK](./includes/meeting-interop-javascript.md)]
::: zone-end

::: zone pivot="platform-ios"
[!INCLUDE [Teams interop with iOS SDK](./includes/meeting-interop-swift.md)]
::: zone-end

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Communication Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind. Weitere Informationen zum Bereinigen von Ressourcen finden Sie [hier](../create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln:

- Sehen Sie sich unser [Hero-Beispiel für Chat](../../samples/chat-hero-sample.md) an.
- Erfahren Sie mehr über die [Funktionsweise des Chats](../../concepts/chat/concepts.md).