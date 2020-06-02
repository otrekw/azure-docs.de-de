---
title: include file
description: include file
services: cognitive-services
author: roy-har
manager: diberry
ms.service: cognitive-services
ms.date: 05/18/2020
ms.subservice: language-understanding
ms.topic: include
ms.custom: include file
ms.author: roy-har
ms.openlocfilehash: 7ab6aa5f830e335a30502207d3a49a528d03f7fd
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83654267"
---
1. Wählen Sie [pizza-app-for-luis-v6.json](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-app-for-luis-v6.json) aus, um die GitHub-Seite für die Datei `pizza-app-for-luis.json` anzuzeigen.
1. Klicken Sie mit der rechten Maustaste auf die Schaltfläche **Raw** (oder tippen Sie lang darauf), und wählen Sie **Link speichern unter** aus, um `pizza-app-for-luis.json` auf Ihrem Computer zu speichern.
1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an.
1. Wählen Sie [Meine Apps](https://www.luis.ai/applications) aus.
1. Wählen Sie auf der Seite **Meine Apps** **+ Neue App für Unterhaltung** aus.
1. Wählen Sie **Als JSON importieren** aus.
1. Wählen Sie im Dialogfeld **Neue App importieren** die Schaltfläche **Datei auswählen** aus.
1. Wählen Sie die heruntergeladene Datei `pizza-app-for-luis.json` und dann **Öffnen** aus.
1. Geben Sie im Feld **Name** im Dialogfeld **Neue App importieren** einen Namen für Ihre Pizza-App ein, und wählen Sie dann die Schaltfläche **Fertig** aus.

Die App wird importiert.

Wenn ein Dialogfeld **Erstellen einer effektiven LUIS-App** angezeigt wird, schließen Sie das Dialogfeld.

## <a name="train-and-publish-the-pizza-app"></a>Trainieren und Veröffentlichen der Pizza-App

Die Seite **Absichten** sollte mit einer Liste aller Absichten in der Pizza-App angezeigt werden.

[!INCLUDE [How to train](howto-train.md)]

[!INCLUDE [How to publish](howto-publish.md)]

Ihre Pizza-App ist jetzt einsatzbereit.

## <a name="record-the-app-id-prediction-key-and-prediction-endpoint-of-your-pizza-app"></a>Notieren Sie die App-ID, den Vorhersageschlüssel und den Vorhersage-Endpunkt Ihrer Pizza-App

Um Ihre neue Pizza-App zu verwenden, benötigen Sie die App-ID, den Vorhersageschlüssel und den Endpunkt ihrer Pizza-App.

So finden Sie diese Werte:

1. Wählen Sie auf der Seite **Absichten** **VERWALTEN** aus.
1. Notieren Sie auf der Seite **Anwendungseinstellungen** die **App-ID**.
1. Wählen Sie **Azure-Ressourcen** aus.
1. Notieren Sie auf der Seite **Azure-Ressourcen** den **Primärschlüssel**. Dieser Wert ist Ihr Vorhersageschlüssel.
1. Notieren Sie die **Endpunkt-URL**. Dieser Wert ist Ihr Vorhersage-Endpunkt.
