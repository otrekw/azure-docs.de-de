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
ms.date: 11/14/2019
ms.author: diberry
ms.openlocfilehash: 2b5046bb61dcafbba0b0540935e08777fbd747a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "74123126"
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

1. Wählen Sie **Fertig**aus. Der neue Deskriptor wird der Seite **Deskriptor** hinzugefügt.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Sie können einen Deskriptor aus der kontextbezogenen Symbolleiste auf der Seite **Deskriptor** löschen oder deaktivieren.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Hinzufügen, Bearbeiten, Löschen oder Deaktivieren eines Deskriptors [trainieren und testen Sie die App](luis-interactive-test.md) erneut, um festzustellen, ob die Leistung besser wurde.
