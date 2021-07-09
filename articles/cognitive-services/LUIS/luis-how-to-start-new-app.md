---
title: 'Erstellen einer neuen App: LUIS'
titleSuffix: Azure Cognitive Services
description: Erstellen und verwalten Sie Ihre Anwendungen auf der LUIS-Webseite (Language Understanding Intelligent Service).
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 05/13/2021
ms.openlocfilehash: 31316a5d0688c7cee0bc15f6e7cebdea23d0f29d
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110097088"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>Erstellen einer LUIS-App im LUIS-Portal
Es gibt zwei Verfahren zum Erstellen einer LUIS-App. Sie können eine LUIS-App im LUIS-Portal oder mithilfe der LUIS-Erstellungs-[APIs](developer-reference-resource.md) erstellen.

## <a name="using-the-luis-portal"></a>Mithilfe des LUIS-Portals

Eine neue App kann im Portal auf verschiedene Arten erstellt werden:

* Beginnen Sie mit einer leeren App, und erstellen Sie Absichten, Äußerungen und Entitäten.
* Beginnen Sie mit einer leeren App, und fügen Sie eine [vordefinierte Domäne](./howto-add-prebuilt-models.md) hinzu.
* Importieren Sie eine LUIS-App aus einer `.lu`- oder `.json`-Datei, die bereits Absichten, Äußerungen und Entitäten enthält.

## <a name="using-the-authoring-apis"></a>Mithilfe der Erstellungs-APIs
Sie können eine neue App mit den Erstellungs-APIs auf zwei Arten erstellen:

* [Anwendung hinzufügen](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c2f): Beginnen Sie mit einer leeren App, und erstellen Sie Absichten, Äußerungen und Entitäten.
* [Vorgefertigte Anwendung hinzufügen](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/59104e515aca2f0b48c76be5): Beginnen Sie mit einer vorgefertigten Domäne, einschließlich Absichten, Äußerungen und Entitäten.


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>


[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

## <a name="create-new-app-in-luis"></a>Erstellen einer neuen App in LUIS

1. Wählen Sie auf der Seite **Meine Apps** Ihr **Abonnement** und die **Erstellungsressource** aus, und klicken Sie dann auf **+ Erstellen**. 
    
    :::image type="content" source="media/create-app-in-portal.png" alt-text="Liste der LUIS-Apps" lightbox="media/create-app-in-portal.png":::

1. Geben Sie im Dialogfeld den Namen Ihrer Anwendung ein, z. B. `Pizza Tutorial`.

    :::image type="content" source="media/create-pizza-tutorial-app-in-portal.png" alt-text="Dialogfeld „Neue App erstellen“" lightbox="media/create-pizza-tutorial-app-in-portal.png":::

1. Wählen Sie Ihre Anwendungskultur und dann **Fertig** aus. Die Beschreibungs- und Vorhersageressource sind an dieser Stelle optional. Die Festlegung kann dann jederzeit im Bereich **Verwalten** des Portals erfolgen.

    > [!NOTE]
    > Die Kultur kann nach dem Erstellen der Anwendung nicht mehr geändert werden.

    Nachdem die App erstellt wurde, zeigt das LUIS-Portal die Liste **Absichten** mit der bereits für Sie erstellten Absicht `None` an. Sie verfügen jetzt über eine leere App.

    :::image type="content" source="media/pizza-tutorial-new-app-empty-intent-list.png" alt-text="Liste der Absichten mit einer „Keine“-Absicht und ohne Beispieläußerungen" lightbox="media/pizza-tutorial-new-app-empty-intent-list.png":::

## <a name="other-actions-available-on-my-apps-page"></a>Weitere verfügbare Aktionen auf der Seite „Meine Apps“

Die Kontextsymbolleiste bietet weitere Aktionen:

* Benennen der App
* Importieren aus Datei mit `.lu` oder `.json`
* Exportieren der App als `.lu` (für [LUDown](https://github.com/microsoft/botbuilder-tools/tree/master/packages/Ludown)), `.json` oder `.zip` (für [LUIS-Container](luis-container-howto.md))
* Importieren von Containerendpunktprotokollen zum Überprüfen von Endpunktäußerungen
* Exportieren von Endpunktprotokollen als `.csv` für die Offlineanalyse
* Löschen einer App

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr App-Entwurf die Absichtserkennung umfasst, [erstellen Sie neue Absichten](luis-how-to-add-intents.md), und fügen Sie Beispieläußerungen hinzu. Wenn Ihr App-Entwurf nur die Datenextraktion umfasst, fügen Sie der Absicht „Keine“ Beispieläußerungen hinzu. Dann [erstellen Sie Entitäten](./luis-how-to-add-entities.md) und bezeichnen die Beispieläußerungen mit diesen Entitäten.