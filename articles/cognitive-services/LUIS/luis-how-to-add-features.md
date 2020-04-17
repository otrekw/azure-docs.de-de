---
title: Deskriptor – LUIS
titleSuffix: Azure Cognitive Services
description: Verwenden Sie Language Understanding (LUIS), um App-Features hinzuzufügen, die die Erkennung oder Vorhersage von Absichten und Entitäten verbessern können, die Kategorien und Muster
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/02/2020
ms.author: diberry
ms.openlocfilehash: 7560fdcbfc77ea2655e8af641794478ead4c11c7
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631446"
---
# <a name="use-descriptors-to-boost-signal-of-word-list"></a>Verwenden von Deskriptoren zum Verstärken des Signals von Wortlisten

Sie können Ihrer LUIS-App Features hinzufügen, um ihre Genauigkeit zu verbessern. Features unterstützen LUIS mit Hinweisen, dass bestimmte Wörter und Ausdrücke Teil eines App-Domänenvokabulars sind.

Ein [Deskriptor](luis-concept-feature.md) (Ausdrucksliste) enthält eine Gruppe von Werten (Wörter oder Ausdrücke), die derselben Klasse angehören und auf ähnliche Weise behandelt werden sollen (z. B. Städte- oder Produktnamen). Erkenntnisse, die LUIS zu einem davon sammelt, werden automatisch auch auf die anderen angewandt. Diese Liste ist nicht dasselbe wie eine [Listenentität](reference-entity-list.md) (genaue Textübereinstimmungen) der übereinstimmenden Wörter.

Ein Deskriptor ist Teil des Vokabulars der App-Domäne als zweiter Hinweis zu diesen Wörtern für LUIS.

Lesen Sie [Featurekonzepte](luis-concept-feature.md), um zu verstehen, wann und warum ein Deskriptor verwendet werden soll.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="add-descriptor"></a>Hinzufügen eines Deskriptors

1. Öffnen Sie die App, indem Sie auf der Seite **Meine Apps** auf ihren Namen, dann auf **Erstellen** und im linken Bereich Ihrer App auf **Deskriptor** klicken.

1. Klicken Sie auf der Seite **Deskriptor** auf **+ Deskriptor hinzufügen**.

1. Geben Sie im Dialogfeld **Neuen Ausdruckslistendeskriptor erstellen** einen Namen für den Deskriptor ein, z. B. `Cities`. Geben Sie im Feld **Wert** die Werte des Deskriptors ein, z. B. `Seattle`. Sie können jeweils einen Wert oder eine Gruppe durch Kommas getrennter Werte eingeben und dann die **EINGABETASTE** drücken.

    > [!div class="mx-imgBorder"]
    > ![Hinzufügen von Deskriptorstädten](./media/luis-add-features/add-phrase-list-cities.png)

    Sobald Sie genügend Werte für LUIS eingegeben haben, werden Vorschläge angezeigt. Sie können **+ Alle hinzufügen** verwenden, um alle vorgeschlagenen Werte hinzufügen, oder einzelne Begriffe auswählen.

1. Lassen Sie die Option **Diese Werte sind austauschbar** aktiviert, wenn die hinzugefügten Deskriptorwerte Alternativen sind, die austauschbar verwendet werden können.

1. Die Liste der Ausdrücke kann auf die gesamte App mit der Einstellung **Global** oder nur auf ein bestimmtes Modell (Absicht oder Entität) angewendet werden. Wenn Sie die Ausdrucksliste als _Deskriptor_ aus einer Absicht oder Entität erstellen, wird die Umschaltfläche auf „nicht global“ festgelegt. In diesem Fall bedeutet die Umschaltfläche, dass der Deskriptor ein Feature für nur dieses Modell ist und deshalb _nicht global_ für die Anwendung.

1. Wählen Sie **Fertig**aus. Der neue Deskriptor wird der Seite **Deskriptor** hinzugefügt.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Sie können einen Deskriptor aus der kontextbezogenen Symbolleiste auf der Seite **Deskriptor** löschen oder deaktivieren.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Hinzufügen, Bearbeiten, Löschen oder Deaktivieren eines Deskriptors [trainieren und testen Sie die App](luis-interactive-test.md) erneut, um festzustellen, ob die Leistung besser wurde.
