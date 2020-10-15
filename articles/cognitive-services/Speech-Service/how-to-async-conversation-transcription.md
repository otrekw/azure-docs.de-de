---
title: Asynchrone Unterhaltungstranskription – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie die asynchrone Unterhaltungstranskription mit dem Speech-Dienst verwenden. Nur für Java und C# verfügbar.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: amishu
ms.custom: devx-track-csharp
zone_pivot_groups: programming-languages-set-twenty-one
ms.openlocfilehash: 4b2dfa8d474f10d6b4ca1c46ac2b575e8d8407ff
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88934639"
---
# <a name="asynchronous-conversation-transcription"></a>Asynchrone Unterhaltungstranskription

In diesem Artikel wird die asynchrone Unterhaltungstranskription mithilfe der **RemoteConversationTranscriptionClient**-API veranschaulicht. Wenn Sie die Unterhaltungstranskription für die asynchrone Transkription konfiguriert haben und über eine `conversationId` verfügen, können Sie die mit dieser `conversationId` verbundene Transkription mit der **RemoteConversationTranscriptionClient**-API abrufen.

## <a name="asynchronous-vs-real-time--asynchronous"></a>Asynchron im Vergleich zu Echtzeit + Asynchron

Bei der asynchronen Transkription streamen Sie die Audiodaten der Unterhaltung, benötigen aber keine Transkription, die in Echtzeit zurückgegeben wird. Verwenden Sie stattdessen nach dem Senden der Audiodaten die `conversationId` von `Conversation`, um den Status der asynchronen Transkription abzufragen. Wenn die asynchrone Transkription bereit ist, erhalten Sie ein `RemoteConversationTranscriptionResult`.

Bei „Echtzeit plus asynchron“ erhalten Sie die Transkription in Echtzeit, aber auch die Transkription durch Abfragen mit dem `conversationId` (ähnlich dem asynchronen Szenario).

Es sind zwei Schritte erforderlich, um eine asynchrone Transkription durchzuführen. Der erste Schritt ist das Hochladen der Audiodaten, wobei Sie entweder nur „Asynchron“ oder „Echtzeit plus asynchron“ auswählen. Der zweite Schritt besteht darin, die Ergebnisse der Transkription abzurufen.

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/remote-conversation/csharp/examples.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/remote-conversation/java/examples.md)]
::: zone-end


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erkunden unserer Beispiele auf GitHub](https://aka.ms/csspeech/samples)
