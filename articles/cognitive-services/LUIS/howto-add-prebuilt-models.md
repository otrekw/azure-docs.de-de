---
title: Vordefinierte Modelle für Language Understanding
titleSuffix: Azure Cognitive Services
description: LUIS umfasst einen Satz von vordefinierten Modellen zum schnellen Hinzufügen von allgemeinen Szenarien für Benutzerkonversationen.
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/17/2020
ms.openlocfilehash: 2080139732d71aa5df9a9d4ad13fcc4545128dff
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91541916"
---
# <a name="add-prebuilt-models-for-common-usage-scenarios"></a>Hinzufügen vordefinierter Modelle für gängige Verwendungsszenarien

LUIS umfasst einen Satz von vordefinierten Modellen zum schnellen Hinzufügen von allgemeinen Szenarien für Benutzerkonversationen. Dies ist eine schnelle und einfache Möglichkeit, um Ihrer Clientanwendung für Konversationen grundlegende Fähigkeiten hinzuzufügen, ohne die Modelle für diese Fähigkeiten entwerfen zu müssen.

## <a name="add-a-prebuilt-domain"></a>Hinzufügen einer vordefinierten Domäne

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.

1. Wählen Sie auf der linken Symbolleiste **Vordefinierte Domänen** aus.

1. Suchen Sie die Domäne, die Sie der App hinzufügen möchten, und wählen Sie dann die Schaltfläche **Domäne hinzufügen** aus.

    > [!div class="mx-imgBorder"]
    > ![Hinzufügen der vordefinierten Domäne „Kalender“](./media/luis-prebuilt-domains/add-prebuilt-domain.png)

## <a name="add-a-prebuilt-intent"></a>Hinzufügen einer vordefinierten Absicht

1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.

1. Wählen Sie auf der Seite **Absichten** auf der Symbolleiste über der Absichtenliste **Vordefinierte Absicht hinzufügen** aus.

1. Wählen Sie im Popupdialogfeld eine Absicht aus.

    > [!div class="mx-imgBorder"]
    > ![Hinzufügen der vordefinierten Absicht](./media/luis-prebuilt-domains/add-prebuilt-domain-intents.png)

1. Wählen Sie die Schaltfläche **Fertig** aus.

## <a name="add-a-prebuilt-entity"></a>Hinzufügen einer vordefinierten Entität
1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.
1. Klicken Sie auf der linken Seite auf **Entitäten**.

1. Wählen Sie auf der Seite **Entitäten** die Option **Add prebuilt entity** (Vordefinierte Entität hinzufügen) aus.

1. Wählen Sie im Dialogfeld **Vordefinierte Entität hinzufügen** die vordefinierte Entität aus.

    > [!div class="mx-imgBorder"]
    > ![Dialogfeld „Vordefinierte Entität hinzufügen“](./media/luis-prebuilt-domains/add-prebuilt-entity.png)

1. Wählen Sie **Fertig**aus. Nachdem die Entität hinzugefügt wurde, müssen Sie die App nicht trainieren.

## <a name="add-a-prebuilt-domain-entity"></a>Hinzufügen einer vordefinierten Domänenentität
1. Melden Sie sich beim [LUIS-Portal](https://www.luis.ai) an, und wählen Sie Ihr **Abonnement** und Ihre **Erstellungsressource** aus, um die Apps anzuzeigen, die dieser Erstellungsressource zugewiesen sind.
1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** den Namen der App auswählen.
1. Klicken Sie auf der linken Seite auf **Entitäten**.

1. Wählen Sie auf der Seite **Entitäten** die Option **Vorgefertigte Domänenentität hinzufügen** aus.

1. Wählen Sie im Dialogfeld **Vorgefertigte Domänenmodelle hinzufügen** die vordefinierte Domänenentität aus.

1. Wählen Sie **Fertig**aus. Nachdem die Entität hinzugefügt wurde, müssen Sie die App nicht trainieren.

## <a name="publish-to-view-prebuilt-model-from-prediction-endpoint"></a>Veröffentlichen Sie sie, um ein vordefiniertes Modell vom Vorhersageendpunkt anzuzeigen

Die einfachste Möglichkeit, den Wert eines vordefinierten Modells anzuzeigen, ist eine Abfrage an den veröffentlichten Endpunkt.

## <a name="entities-containing-a-prebuilt-entity-token"></a>Entitäten mit einem vordefinierten Entitätstoken

Wenn Sie über eine durch maschinelles Lernen erworbene Entität verfügen, die ein erforderliches Feature einer vordefinierten Entität benötigt, fügen Sie der durch maschinelles Lernen erworbenen Entität eine untergeordnete Entität und anschließend ein _erforderliches_ Feature einer vorgefertigten Entität hinzu.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Erstellen eines Modells aus CSV mit REST-APIs](./luis-tutorial-node-import-utterances-csv.md)
