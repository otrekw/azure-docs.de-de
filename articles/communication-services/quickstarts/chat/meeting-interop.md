---
title: Erste Schritte mit Teams-Interoperabilität unter Azure Communication Services
titleSuffix: An Azure Communication Services quickstart
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie mit dem Azure Communication Services Chat SDK einer Teams-Besprechung beitreten.
author: askaur
ms.author: askaur
ms.date: 03/10/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: ba3a589c5d0f09f24950bd3fee8edc7f4dcd4601
ms.sourcegitcommit: d23602c57d797fb89a470288fcf94c63546b1314
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2021
ms.locfileid: "106169061"
---
# <a name="quickstart-join-your-chat-app-to-a-teams-meeting"></a>Schnellstart: Beitreten mit einer Chat-App zu einer Teams-Besprechung

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include-chat.md)]

> [!IMPORTANT]
> Um die [Interoperabilität von Teams-Mandanten](../../concepts/teams-interop.md) zu aktivieren/deaktivieren, füllen Sie [dieses Formular](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR21ouQM6BHtHiripswZoZsdURDQ5SUNQTElKR0VZU0VUU1hMOTBBMVhESS4u) aus.

Steigen Sie in die Nutzung von Azure Communication Services ein, indem Sie Ihre Chatlösung über das JavaScript SDK mit Microsoft Teams verbinden. 

## <a name="prerequisites"></a>Voraussetzungen 

1. Eine  [Teams-Bereitstellung](/deployoffice/teams-install). 
2. Eine funktionierende [Chat-App](./get-started.md). 

## <a name="enable-teams-interoperability"></a>Aktivieren der Teams-Interoperabilität 

Ein Communication Services-Benutzer, der einer Teams-Besprechung als Gastbenutzer hinzugefügt wird, kann nur auf den Chat der Besprechung zugreifen, wenn er der Teams-Telefonkonferenz beitritt. Weitere Informationen zum Hinzufügen eines Communication Services-Benutzers zu einer Teams-Telefonkonferenz finden Sie in der Dokumentation zur [Teams-Interoperabilität](../voice-video-calling/get-started-teams-interop.md).

Sie müssen Mitglied der besitzenden Organisation beider Entitäten sein, um dieses Feature verwenden zu können.

[!INCLUDE [Join Teams meetings](./includes/meeting-interop-javascript.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Communication Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind. Weitere Informationen zum Bereinigen von Ressourcen finden Sie [hier](../create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln:

- Sehen Sie sich unser [Hero-Beispiel für Chat](../../samples/chat-hero-sample.md) an.
- Erfahren Sie mehr über die [Funktionsweise des Chats](../../concepts/chat/concepts.md).