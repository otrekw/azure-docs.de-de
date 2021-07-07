---
title: Erste Schritte mit Teams-Interoperabilität unter Azure Communication Services
titleSuffix: An Azure Communication Services quickstart
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie mit dem Azure Communication Services Chat SDK einer Teams-Besprechung beitreten.
author: askaur
ms.author: askaur
ms.date: 03/10/2021
ms.topic: quickstart
ms.service: azure-communication-services
zone_pivot_groups: acs-web-ios
ms.openlocfilehash: 196c35d78b18241a808d4f941dd51d0f08a5b722
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110080271"
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