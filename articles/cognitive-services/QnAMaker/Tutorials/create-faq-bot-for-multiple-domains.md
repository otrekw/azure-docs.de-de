---
title: 'Tutorial: Erstellen eines FAQ-Bots für mehrere Domänen mit Azure Bot Service'
description: In diesem Tutorial erstellen Sie mit QnA Maker und Azure Bot Service einen FAQ-Bot ohne Code für Anwendungsfälle in der Produktion.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.author: diagarw
ms.date: 03/31/2021
ms.openlocfilehash: d79eed22d441949810cfc1738f3af2c0f8703adc
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110116446"
---
# <a name="add-multiple-domains-to-your-faq-bot"></a>Hinzufügen mehrerer Domänen zu Ihrem FAQ-Bot

Wenn Sie einen FAQ-Bot erstellen, werden Sie möglicherweise auf Anwendungsfälle stoßen, bei denen Sie Abfragen aus verschiedenen Domänen beantworten müssen. Angenommen, das Marketingteam von Microsoft möchte einen Bot für den Kundensupport erstellen, der häufige Abfragen von Benutzern zu verschiedenen Surface-Produkten beantwortet. Der Einfachheit halber verwenden wir hier je eine FAQ-URL für [Surface Pen](https://support.microsoft.com/surface/how-to-use-your-surface-pen-8a403519-cd1f-15b2-c9df-faa5aa924e98) und [Surface Earbuds](https://support.microsoft.com/surface/use-surface-earbuds-aea108c3-9344-0f11-e5f5-6fc9f57b21f9), um die Wissensdatenbank anzulegen.

Sie können Ihren Bot so gestalten, dass er Abfragen aus mehreren Domänen mit dem QnA Maker verarbeiten kann, und zwar auf folgende Weisen:

* Erstellen Sie eine einzelne Wissensdatenbank, und markieren Sie QnA-Paare in unterschiedlichen Domänen mit Metadaten.
* Erstellen Sie eine separate Wissensdatenbank für jede Domäne.
* Erstellen Sie eine separate QnA Maker-Ressource für jede Domäne.

## <a name="create-a-single-knowledge-base-and-tag-qna-pairs-into-distinct-domains-with-metadata"></a>Erstellen Sie eine einzelne Wissensdatenbank, und markieren Sie QnA-Paare in unterschiedlichen Domänen mit Metadaten.

Die Autoren der Inhalte können Dokumente verwenden, um QnAs zu extrahieren oder benutzerdefinierte QnAs in die Wissensdatenbank aufzunehmen. Um diese QnAs in bestimmte Domänen oder Kategorien zu gruppieren, können Sie [Metadaten](../How-To/query-knowledge-base-with-metadata.md) zu den QnA-Paaren hinzufügen.

Für den Bot für Surface-Produkte können Sie die folgenden Schritte ausführen, um einen Bot zu erstellen, der Abfragen für beide Produkttypen beantwortet:

1. Fügen Sie die folgenden FAQ-URLs für Surface-Produkte im SCHRITT 3 der Seite „Wissensdatenbank erstellen“ hinzu, und klicken Sie auf „Ihre Wissensdatenbank erstellen“. Nach der Extraktion von QnA-Paaren aus diesen Quellen wird eine neue Wissensdatenbank erstellt. 
   
   [FAQ zu Surface Pen](https://support.microsoft.com/surface/how-to-use-your-surface-pen-8a403519-cd1f-15b2-c9df-faa5aa924e98)<br>[FAQ zu Surface Earbuds](https://support.microsoft.com/surface/use-surface-earbuds-aea108c3-9344-0f11-e5f5-6fc9f57b21f9)
 
2. Nachdem wir die Wissensdatenbank erstellt haben, können wir zu **Ansichtsoptionen** wechseln und auf **Metadaten anzeigen** klicken. Dadurch wird eine Metadatenspalte für die QnAs geöffnet.

   >[!div class="mx-imgBorder"]
   >[![Metadaten anzeigen]( ../media/qnamaker-tutorial-updates/show-metadata.png) ]( ../media/qnamaker-tutorial-updates/expand/show-metadata.png#lightbox)


3. In dieser Wissensdatenbank befinden sich QnAs zu zwei Produkten, die wir gerne unterscheiden möchten, damit wir unter den QnAs zu einem bestimmten Produkt nach Antworten suchen können. Dazu müssen wir das Metadatenfeld für die QnA-Paare entsprechend aktualisieren. 

   Wie Sie im Beispiel unten sehen können, haben wir ein Metadatenelement mit **Produkt** als Schlüssel und **surface_pen** oder **surface_earbuds** an den entsprechenden Stellen als Werte hinzugefügt. Sie können dieses Beispiel so erweitern, dass Daten für mehrere Produkte extrahiert werden und für jedes Produkt ein anderer Wert hinzugefügt wird.

   >[!div class="mx-imgBorder"]
   >[![Metadaten]( ../media/qnamaker-tutorial-updates/metadata-example-2.png) ]( ../media/qnamaker-tutorial-updates/expand/metadata-example-2.png#lightbox)

4. Um nun das System auf die Suche nach der Antwort für ein bestimmtes Produkt einzuschränken, müssten Sie dieses Produkt in der GenerateAnswer-API übergeben.

    Erfahren Sie, [wie Sie die GenerateAnswer-API verwenden](../How-To/metadata-generateanswer-usage.md). Die GenerateAnswer-URL weist das folgende Format auf:
    ```
    https://{QnA-Maker-endpoint}/knowledgebases/{knowledge-base-ID}/generateAnswer
    ```

    Im JSON-Rumpf des API-Aufrufs haben wir *surface_pen* als Wert für die Metadaten zu *product* übergeben. Daher sucht das System nur nach der Antwort unter den QnA-Paaren mit den gleichen Metadaten. 

    ```json
    {
        "question": "What is the price?",
        "top": 6,
        "isTest": true,
        "scoreThreshold": 30,
        "rankerType": ""  // values: QuestionOnly
        "strictFilters": [
        {
            "name": "product",
            "value": "surface_pen"
        }],
        "userId": "sd53lsY="
    }
    ```

    Sie können Metadatenwerte auf Grundlage von Benutzereingaben auf folgende Weisen abrufen: 

    * Verwenden Sie die Domäne explizit als Eingabe des Benutzers über den Botclient. Zum Beispiel können Sie, wie unten gezeigt, die Produktkategorie als Eingabe vom Benutzer übernehmen, wenn die Unterhaltung eingeleitet wird.

      ![Verwenden der Metadateneingaben](../media/qnamaker-tutorial-updates/expand/explicit-metadata-input.png)

    * Identifizieren Sie die Domäne implizit anhand des Kontexts des Bots. Wenn sich die vorherige Frage z. B. auf ein bestimmtes Surface-Produkt bezog, kann sie vom Client als Kontext gespeichert werden. Wenn der Benutzer das Produkt bei der nächsten Abfrage nicht angibt, könnten Sie den Kontext des Bots als Metadaten an die GenerateAnswer-API weitergeben.

      ![Übergeben des Kontexts]( ../media/qnamaker-tutorial-updates/expand/extract-metadata-from-context.png)

    * Extrahieren Sie die Entität aus der Abfrage des Benutzers, um die Domäne zu bestimmen, die als Metadatenfilter verwendet werden soll. Sie können andere Cognitive Services wie Textanalyse und LUIS für die Extraktion von Entitäten nutzen.

      ![Extrahieren von Metadaten aus einer Abfrage]( ../media/qnamaker-tutorial-updates/expand/extract-metadata-from-query.png)

### <a name="how-large-can-our-knowledge-bases-be"></a>Wie groß können unsere Wissensdatenbanken sein? 

Sie können einer einzelnen Wissensdatenbank bis zu 50.000 QnA-Paare hinzufügen. Wenn Ihre Daten 50.000 QnA-Paare überschreiten, sollten Sie erwägen, die Wissensdatenbank aufzuteilen.

## <a name="create-a-separate-knowledge-base-for-each-domain"></a>Erstellen einer separaten Wissensdatenbank für jede Domäne

Sie können auch eine separate Wissensdatenbank für jede Domäne erstellen und die Wissensdatenbanken separat verwalten. Alle APIs erfordern, dass der Benutzer die Wissensdatenbank-ID übergibt, um eine Aktualisierung der Wissensdatenbank vorzunehmen oder eine Antwort auf die Frage des Benutzers abzurufen.  

Wenn die Frage des Benutzers vom Dienst empfangen wird, müssen Sie die Wissensdatenbank-ID im oben gezeigten Endpunkt „Antwort generieren“ übergeben, um eine Antwort von der entsprechenden Wissensdatenbank abzurufen. Die Wissensdatenbank-ID finden Sie im Abschnitt **Veröffentlichen** der Seite wie unten gezeigt.

>[!div class="mx-imgBorder"]
>![Abrufen der Wissensdatenbank-ID](../media/qnamaker-tutorial-updates/fetch-kb-id.png)

## <a name="create-a-separate-qna-maker-resource-for-each-domain"></a>Erstellen Sie eine separate QnA Maker-Ressource für jede Domäne.

Angenommen, das Marketingteam von Microsoft möchte einen Bot für den Kundensupport erstellen, der häufige Abfragen von Benutzern zu Surface- und Xbox-Produkten beantwortet. Geplant ist, verschiedenen Teams den Zugriff auf die Wissensdatenbanken für Surface und Xbox zuzuweisen. In diesem Fall ist es ratsam, zwei QnA Maker-Ressourcen zu erstellen: eine für Surface und eine für Xbox. Sie können jedoch unterschiedliche Rollen für Benutzer definieren, die auf dieselbe Ressource zugreifen. Weitere Informationen finden Sie unter [Verwalten des Zugriffs mit rollenbasierter Zugriffssteuerung](../How-To/manage-qna-maker-app.md). 
