---
title: 'Anleitung: Verwenden von Custom Commands mit Custom Voice – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel geben Sie die Ausgabestimme für eine Custom Commands-Anwendung an.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: sausin
ms.openlocfilehash: b1e488653f210089ee5ed6757fb2f3d1e4fb0fe4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87293789"
---
# <a name="use-custom-commands-with-custom-voice"></a>Verwenden benutzerdefinierter Befehle mit Custom Voice

In diesem Artikel erfahren Sie, wie Sie eine benutzerdefinierte Ausgabesprache für eine Custom Commands-Anwendung auswählen.

## <a name="select-a-custom-voice"></a>Auswählen einer Custom Voice

1. Wählen Sie in Ihrer Anwendung für benutzerdefinierte Befehle die Option **Einstellungen** im linken Bereich aus.
1. Wählen Sie im mittleren Bereich **Custom Voice** aus.
1. Wählen Sie die gewünschte benutzerdefinierte oder öffentliche Stimme aus der Tabelle aus.
1. Wählen Sie **Speichern** aus.

> [!div class="mx-imgBorder"]
> ![Beispielsätze mit Parametern](media/custom-commands/select-custom-voice.png)

> [!NOTE]
> - Bei den **öffentlichen Stimmen** sind **neuronale Typen** nur für bestimmte Regionen verfügbar. Informationen zur Verfügbarkeit finden Sie unter [Standardstimmen und neuronale Stimmen nach Region/Endpunkt](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices).
> - Für **benutzerdefinierte Stimmen** können sie auf der Custom Voice-Projektseite erstellt werden. Mehr dazu erfahren Sie unter [Erste Schritte mit Custom Voice](./how-to-custom-voice.md)

Die Anwendung antwortet nun in der ausgewählten Stimme statt in der Standardstimme.
