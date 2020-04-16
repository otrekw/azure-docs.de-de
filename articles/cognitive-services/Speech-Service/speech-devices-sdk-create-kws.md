---
title: Erstellen von benutzerdefinierten Schlüsselwörtern – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Ihr Gerät lauscht immer auf ein Schlüsselwort (oder einen Ausdruck). Wenn der Benutzer das Schlüsselwort nennt, sendet das Gerät sämtliche nachfolgende Audioaufnahmen an die Cloud, bis der Benutzer aufhört zu sprechen. Durch das Anpassen Ihres Schlüsselworts können Sie Ihr Gerät auf effektive Weise differenzieren und Ihr Branding stärken.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/11/2019
ms.author: trbye
ms.openlocfilehash: 8e67d624c77eb838f7646731bbdedd8f97f81b96
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400055"
---
# <a name="create-a-custom-keyword-using-speech-studio"></a>Erstellen eines benutzerdefinierten Schlüsselworts mithilfe von Speech Studio

Ihr Gerät lauscht immer auf ein Schlüsselwort (oder einen Ausdruck). „Hey Cortana“ ist beispielsweise ein Schlüsselwort für den Cortana-Assistenten. Wenn der Benutzer das Schlüsselwort nennt, sendet das Gerät sämtliche nachfolgende Audioaufnahmen an die Cloud, bis der Benutzer aufhört zu sprechen. Durch das Anpassen Ihres Schlüsselworts können Sie Ihr Gerät auf effektive Weise differenzieren und Ihr Branding stärken.

In diesem Artikel erfahren Sie, wie Sie für Ihr Gerät ein benutzerdefiniertes Schlüsselwort erstellen.

## <a name="create-your-keyword"></a>Erstellen Ihres Schlüsselworts

Bevor Sie ein benutzerdefiniertes Schlüsselwort verwenden können, müssen Sie auf der Seite [Benutzerdefiniertes Schlüsselwort](https://aka.ms/sdsdk-wakewordportal) in [Speech Studio](https://aka.ms/sdsdk-speechportal) ein Schlüsselwort erstellen. Nachdem Sie ein Schlüsselwort angegeben haben, wird eine Datei erstellt, die Sie auf Ihrem Gerät bereitstellen.

1. Wechseln Sie zu [Speech Studio](https://aka.ms/sdsdk-speechportal), und **melden Sie sich an**. Wenn Sie kein Speech-Abonnement haben, wählen Sie [**Abonnement erstellen**](https://go.microsoft.com/fwlink/?linkid=2086754) aus.

1. Erstellen Sie auf der Seite [Benutzerdefiniertes Schlüsselwort](https://aka.ms/sdsdk-wakewordportal) ein **neues Projekt**. 

1. Geben Sie unter **Name** einen Namen und unter **Beschreibung** eine optionale Beschreibung ein, und wählen Sie die Sprache aus. Sie benötigen jeweils ein Projekt pro Sprache, und der Support ist derzeit auf die Sprache „en-US“ beschränkt.

    ![Beschreiben Ihres Schlüsselwortprojekts](media/custom-keyword/custom-kws-portal-new-project.png)

1. Wählen Sie Ihr Projekt in der Liste aus. 

    ![Auswählen Ihres Schlüsselwortprojekts](media/custom-keyword/custom-kws-portal-project-list.png)

1. Klicken Sie zum Starten eines neuen Schlüsselwortmodells auf **Modell trainieren**.

1. Geben Sie unter **Name** einen Namen für das Schlüsselwortmodell, unter **Beschreibung** eine optionale Beschreibung und unter **Schlüsselwort** das gewünschte Schlüsselwort ein, und klicken Sie anschließend auf **Weiter**. Wir haben einige [Richtlinien](speech-devices-sdk-kws-guidelines.md#choose-an-effective-keyword), die bei der Auswahl eines effektiven Stichworts helfen.

    ![Eingeben Ihres Schlüsselworts](media/custom-keyword/custom-kws-portal-new-model.png)

1. Im Portal werden nun Kandidaten für die Aussprache Ihres Schlüsselworts erstellt. Hören Sie sich die einzelnen Kandidaten an, indem Sie auf die Wiedergabeschaltflächen klicken. Deaktivieren Sie dann alle nicht ordnungsgemäßen Aussprachevarianten. Wenn nur noch geeignete Aussprachen aktiviert sind, klicken Sie auf **Trainieren**, um mit dem Generieren des Schlüsselworts zu beginnen. 

    ![Überprüfen Ihres Schlüsselworts](media/custom-keyword/custom-kws-portal-choose-prons.png)

1. Die Generierung des Modells kann bis zu dreißig Minuten dauern. Wenn das Modell fertig ist, ändert sich die Liste mit den Schlüsselwörtern von **Wird verarbeitet...** in **Erfolgreich**. Nun können Sie die Datei herunterladen.

    ![Überprüfen Ihres Schlüsselworts](media/custom-keyword/custom-kws-portal-download-model.png)

1. Speichern Sie die ZIP-Datei auf Ihrem Computer. Sie benötigen diese Datei, um Ihr benutzerdefiniertes Schlüsselwort auf Ihrem Gerät bereitzustellen.

## <a name="next-steps"></a>Nächste Schritte

Testen Ihres benutzerdefinierten Schlüsselworts mit dem [Schnellstart zum Speech-Geräte-SDK](https://aka.ms/sdsdk-quickstart).
