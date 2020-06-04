---
title: 'Schnellstart: Verwenden von Custom Commands mit Custom Voice (Vorschau) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel geben Sie die Ausgabestimme für eine Custom Commands-Anwendung an.
services: cognitive-services
author: anhoang
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: anhoang
ms.openlocfilehash: c2b9b4d51e89975d988ed94bf85695bd8a1cc770
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872468"
---
# <a name="quickstart-use-custom-commands-with-custom-voice-preview"></a>Schnellstart: Verwenden benutzerdefinierter Befehle mit Custom Voice (Vorschau)

Im [vorherigen Artikel](./quickstart-custom-speech-commands-create-parameters.md) wurde ein neues Projekt für Custom Commands erstellt, das auf Befehle mit Parametern antwortet.

In diesem Artikel wählen wir eine benutzerdefinierte Ausgabestimme für die Anwendung aus, die wir erstellt haben.

## <a name="select-a-custom-voice"></a>Auswählen einer Custom Voice

1. Öffnen Sie das Projekt, [das wir zuvor erstellt haben](./quickstart-custom-speech-commands-create-parameters.md).
1. Wählen Sie im linken Bereich **Einstellungen** aus.
1. Wählen Sie im mittleren Bereich **Custom Voice** aus.
1. Wählen Sie die gewünschte benutzerdefinierte oder öffentliche Stimme aus der Tabelle aus.
1. Wählen Sie **Speichern** aus.

> [!div class="mx-imgBorder"]
> ![Beispielsätze mit Parametern](media/custom-speech-commands/select-custom-voice.png)

> [!NOTE]
> - Bei den **öffentlichen Stimmen** sind **neuronale Typen** nur für bestimmte Regionen verfügbar. Informationen zur Verfügbarkeit finden Sie unter [Standardstimmen und neuronale Stimmen nach Region/Endpunkt](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices).
> - Für **benutzerdefinierte Stimmen** können sie auf der Custom Voice-Projektseite erstellt werden. Mehr dazu erfahren Sie unter [Erste Schritte mit Custom Voice](./how-to-custom-voice.md)

Die Anwendung antwortet nun in der ausgewählten Stimme statt in der Standardstimme.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Schnellstart: Herstellen einer Verbindung mit einer Anwendung für benutzerdefinierte Befehle mit dem Speech SDK (Vorschau)](./quickstart-custom-speech-commands-speech-sdk.md)

