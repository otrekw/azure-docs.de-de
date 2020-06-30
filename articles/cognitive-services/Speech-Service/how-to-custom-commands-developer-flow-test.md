---
title: Testen Ihrer App für benutzerdefinierte Befehle
titleSuffix: Azure Cognitive Services
description: In diesem Artikel lernen Sie verschiedene Ansätze zum Testen einer Anwendung für benutzerdefinierte Befehle kennen.
services: cognitive-services
author: xiaojul
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: xiaojul
ms.openlocfilehash: a7992a2a27822f87cc8667ff86f642d1d47d09aa
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307235"
---
# <a name="test-your-custom-commands-application"></a>Testen Ihrer Anwendung für benutzerdefinierte Befehle

In diesem Artikel lernen Sie verschiedene Ansätze zum Testen einer Anwendung für benutzerdefinierte Befehle kennen.

## <a name="test-in-the-portal"></a>Testen im Portal

Das Testen im Portal ist der einfachste und schnellste Weg, um zu überprüfen, ob Ihre Anwendung für benutzerdefinierte Befehle wie erwartet funktioniert. Nachdem die App erfolgreich trainiert wurde, klicken Sie auf die Schaltfläche `Test`, um mit dem Testen zu beginnen.

> [!div class="mx-imgBorder"]
> ![Testen im Portal](media/custom-commands/create-basic-test-chat.png)

## <a name="test-with-windows-voice-assistant-client"></a>Testen mit dem Windows-Sprachassistent-Client

Der Windows-Sprachassistent-Client ist eine WPF-Anwendung (Windows Presentation Foundation) in C#, mit der Sie die Interaktionen mit Ihrem Bot leicht testen können, bevor Sie eine benutzerdefinierte Clientanwendung erstellen.

Das Tool kann eine benutzerdefinierte ID der Anwendung für benutzerdefinierte Befehle akzeptieren. Es ermöglicht es Ihnen, Ihr Aufgabenerledigungs- oder Befehl-und-Steuerung-Szenario zu testen, das in dem Dienst für benutzerdefinierte Befehle gehostet wird.

Checken Sie zum Einrichten des Clients den [Windows-Sprachassistent-Client](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/tree/master/clients/csharp-wpf) aus.

> [!div class="mx-imgBorder"]
> ![WVAC: Profil erstellen](media/custom-commands/conversation.png)

## <a name="test-with-speech-sdk-enabled-client-applications"></a>Testen mit Speech SDK-fähigen Clientanwendungen 
Das Speech SDK (Software Development Kit) macht viele der Funktionen des Speech-Dienstes verfügbar und ermöglicht Ihnen das Entwickeln sprachaktivierter Anwendungen. Es ist auch in vielen Programmiersprachen und für alle Plattformen verfügbar.

So richten Sie eine UWP-Clientanwendung (Universelle Windows-Plattform) mit dem Speech SDK ein und integrieren sie in Ihre Anwendung für benutzerdefinierte Befehle  
- [Vorgehensweise: Integrieren in eine Clientanwendung mithilfe des Speech SDK](./how-to-custom-commands-setup-speech-sdk.md)
- [Vorgehensweise: Senden einer Aktivität an eine Clientanwendung](./how-to-custom-commands-send-activity-to-client.md)
- [Vorgehensweise: Einrichten von Webendpunkten](./how-to-custom-commands-setup-web-endpoints.md)

Für andere Programmiersprachen und Plattformen:
- [Programmiersprachen, Plattformen, Szenariokapazitäten des Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk)
- [Beispielcodes für Sprachassistenten](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen finden Sie in den Beispielen auf GitHub](https://aka.ms/speech/cc-samples)

