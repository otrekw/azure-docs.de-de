---
title: 'Erstellen einer neuen App: LUIS'
titleSuffix: Azure Cognitive Services
description: Erstellen und verwalten Sie Ihre Anwendungen auf der LUIS-Webseite (Language Understanding Intelligent Service).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/08/2019
ms.author: diberry
ms.openlocfilehash: d3f8696388a33a8ea112aae438c6bbe9af520c61
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2019
ms.locfileid: "73904272"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>Erstellen einer LUIS-App im LUIS-Portal
Es gibt zwei Verfahren zum Erstellen einer LUIS-App. Sie können eine LUIS-App im LUIS-Portal oder mithilfe der LUIS-Erstellungs-[APIs](developer-reference-resource.md) erstellen.

[!INCLUDE [Uses preview portal](includes/uses-portal-preview.md)]

## <a name="using-the-luis-portal"></a>Mithilfe des LUIS-Portals

Sie können eine neue App auf verschiedene Arten im Vorschauportal erstellen:

* Beginnen Sie mit einer leeren App, und erstellen Sie Absichten, Äußerungen und Entitäten.
* Beginnen Sie mit einer leeren App, und fügen Sie eine [vordefinierte Domäne](luis-how-to-use-prebuilt-domains.md) hinzu.
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

1. Wählen Sie auf der Seite **Meine Apps** Ihr Abonnement und die Erstellungsressource sowie anschließend **+ Erstellen** aus. Wenn Sie einen Schlüssel für die kostenlose Testversion verwenden, erfahren Sie, wie Sie [eine Erstellungsressource erstellen können](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal).

    ![Liste der LUIS-Apps](./media/create-app-in-portal.png)


1. Geben Sie im Dialogfeld den Namen Ihrer Anwendung ein, z. B. `Pizza Tutorial`.

    ![Dialogfeld „Neue App erstellen“](./media/create-pizza-tutorial-app-in-portal.png)

1. Wählen Sie Ihre Anwendungskultur und dann **Fertig** aus. Die Beschreibungs- und Vorhersageressource sind an dieser Stelle optional. Die Festlegung kann dann jederzeit im Bereich **Verwalten** des Portals erfolgen.

    > [!NOTE]
    > Die Kultur kann nach dem Erstellen der Anwendung nicht mehr geändert werden. 

    Nachdem die App erstellt wurde, zeigt das LUIS-Portal die Liste **Absichten** mit der bereits für Sie erstellten Absicht `None` an. Sie verfügen jetzt über eine leere App. 
    
    > [!div class="mx-imgBorder"]
    > ![Liste „Absichten“ mit erstellter Absicht „Keine“ ohne Beispieläußerungen.](media/pizza-tutorial-new-app-empty-intent-list.png)

## <a name="other-actions-available"></a>Weitere verfügbare Aktionen

Die Kontextsymbolleiste bietet weitere Aktionen:

* Benennen der App
* Importieren aus Datei mit `.lu` oder `.json`
* Exportieren der App als `.lu` (für [LUDown](https://github.com/microsoft/botbuilder-tools/tree/master/packages/Ludown)), `.json` oder `.zip` (für [LUIS-Container](luis-container-howto.md))
* Importieren von Containerendpunktprotokollen zum Überprüfen von Endpunktäußerungen
* Exportieren von Endpunktprotokollen als `.csv` für die Offlineanalyse
* Löschen einer App

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr App-Entwurf die Absichtserkennung umfasst, [erstellen Sie neue Absichten](luis-how-to-add-intents.md), und fügen Sie Beispieläußerungen hinzu. Wenn Ihr App-Entwurf nur die Datenextraktion umfasst, fügen Sie der Absicht „Keine“ Beispieläußerungen hinzu. Dann [erstellen Sie Entitäten](luis-how-to-add-example-utterances.md) und bezeichnen die Beispieläußerungen mit diesen Entitäten. 
