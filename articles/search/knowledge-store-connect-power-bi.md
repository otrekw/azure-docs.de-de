---
title: Herstellen einer Verbindung mit einem Wissensspeicher mit Power BI
titleSuffix: Azure Cognitive Search
description: Stellen Sie mit Power BI eine Verbindung mit einem Azure Cognitive Search-Wissensspeicher her, um diesen zu analysieren und zu untersuchen.
author: HeidiSteen
ms.author: heidist
manager: nitinme
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: 5be97414a2e4270870d976a9922415b227c66e27
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88924349"
---
# <a name="connect-a-knowledge-store-with-power-bi"></a>Herstellen einer Verbindung mit einem Wissensspeicher mit Power BI

In diesem Artikel erfahren Sie, wie Sie mithilfe von Power Query in der Power BI Desktop-App eine Verbindung mit einem Wissensspeicher herstellen und ihn anschließend erkunden. Sie können mit Vorlagen schneller beginnen oder ein benutzerdefiniertes Dashboard von Grund auf neu erstellen. In diesem kurzen Video unten wird veranschaulicht, wie Sie Ihre Umgebung mit Ihren Daten unter Verwendung von Azure Cognitive Search in Kombination mit Power BI anreichern können.


> [!VIDEO https://www.youtube.com/embed/XWzLBP8iWqg?version=3&start=593&end=663]



+ Führen Sie die Schritte aus, die unter [Erstellen eines Wissensspeichers im Azure-Portal](knowledge-store-create-portal.md) bzw. [Erstellen eines Azure Cognitive Search-Wissensspeichers mithilfe von REST](knowledge-store-create-rest.md) beschrieben sind, um den Beispielwissensspeicher zu erstellen, der in dieser exemplarischen Vorgehensweise verwendet wird. Sie benötigen außerdem den Namen des Azure Storage-Kontos, das Sie zum Erstellen des Wissensspeichers verwendet haben, sowie den Zugriffsschlüssel aus dem Azure-Portal.

+ [Installieren Sie Power BI Desktop](https://powerbi.microsoft.com/downloads/).

## <a name="sample-power-bi-template---azure-portal-only"></a>Power BI-Beispielvorlage (nur Azure-Portal)

Beim [Erstellen eines Wissensspeichers über das Azure-Portal](knowledge-store-create-portal.md) können Sie auf der zweiten Seite des Assistenten **Daten importieren** eine [Power BI-Vorlage](https://github.com/Azure-Samples/cognitive-search-templates) herunterladen. Die Vorlage bietet mehrere Visualisierungen für textbasierte Inhalte (beispielweise WordCloud und Network Navigator). 

Klicken Sie auf der Seite **Kognitive Qualifikationen hinzufügen (Optional)** auf **Power BI-Vorlage abrufen**, um die Vorlage aus dem öffentlichen GitHub-Speicherort abzurufen und herunterzuladen. Der Assistent passt die Vorlage an die Form Ihrer Daten an (gemäß der Erfassung in den im Assistenten angegebenen Wissensspeicherprojektionen). Aus diesem Grund variiert die heruntergeladene Vorlage bei jeder Ausführung des Assistenten, wenn sich die Dateneingaben und die ausgewählten Qualifikationen unterscheiden.

![Beispiel: Power BI-Vorlage für Azure Cognitive Search](media/knowledge-store-connect-power-bi/powerbi-sample-template-portal-only.png "Beispiel für Power BI-Vorlage")

> [!NOTE]
> Die Vorlage wird zwar während der Ausführung des Assistenten heruntergeladen, Sie müssen jedoch warten, bis der Wissensspeicher in Azure Table Storage erstellt wurde, um ihn verwenden zu können.

## <a name="connect-with-power-bi"></a>Herstellen einer Verbindung mit Power BI

1. Starten Sie Power BI Desktop, und klicken Sie auf **Daten abrufen**.

1. Wählen Sie im Fenster **Daten abrufen** die Option **Azure** und anschließend **Azure-Tabellenspeicher** aus.

1. Klicken Sie auf **Verbinden**.

1. Geben Sie unter **Kontoname oder URL** den Namen Ihres Azure Storage-Kontos ein (die vollständige URL wird für Sie erstellt).

1. Wenn Sie dazu aufgefordert werden, geben Sie den Schlüssel des Speicherkontos ein.

1. Wählen Sie die Tabellen mit den Hotelrezensionsdaten aus, die im Rahmen der vorherigen exemplarischen Vorgehensweisen erstellt wurden. 

   + Bei der exemplarischen Vorgehensweise für das Portal lauten die Tabellennamen *hotelReviewsSsDocument*, *hotelReviewsSsEntities*, *hotelReviewsSsKeyPhrases* und *hotelReviewsSsPages*. 
   
   + Bei der exemplarischen Vorgehensweise für REST lauten die Tabellennamen *hotelReviewsDocument*, *hotelReviewsPages*, *hotelReviewsKeyPhrases* und *hotelReviewsSentiment*.

1. Klicken Sie auf **Laden**.

1. Klicken Sie im oberen Menüband auf **Abfragen bearbeiten**, um den **Power Query-Editor** zu öffnen.

   ![Power Query öffnen](media/knowledge-store-connect-power-bi/powerbi-edit-queries.png "Power Query öffnen")

1. Wählen Sie *hotelReviewsSsDocument* aus, und entfernen Sie anschließend die Spalten *PartitionKey*, *RowKey* und *Timestamp*. 
   ![Tabellen bearbeiten](media/knowledge-store-connect-power-bi/powerbi-edit-table.png "Tabellen bearbeiten")

1. Klicken Sie rechts oben in der Tabelle auf das Symbol mit den entgegengesetzten Pfeilen, um den *Inhalt* zu erweitern. Wenn die Liste mit Spalten angezeigt wird, wählen Sie alle Spalten aus, und heben Sie anschließend die Markierung der Spalten auf, die mit „metadata“ beginnen. Klicken Sie auf **OK**, um die ausgewählten Spalten anzuzeigen.

   ![Tabellen bearbeiten](media/knowledge-store-connect-power-bi/powerbi-expand-content-table.png "Inhalt erweitern")

1. Ändern Sie den Datentyp für die folgenden Spalten, indem Sie oben links in der Spalte auf das Symbol „ABC-123“ klicken.

   + Wählen Sie für *content.latitude* und *Content.longitude* die Option **Dezimalzahl** aus.
   + Wählen Sie für *Content.reviews_date* und *Content.reviews_dateAdded* die Option **Datum/Uhrzeit** aus.

   ![Datentypen ändern](media/knowledge-store-connect-power-bi/powerbi-change-type.png "Datentypen ändern")

1. Wählen Sie *hotelReviewsSsPages* aus, und wiederholen Sie dann die Schritte 9 und 10, um die Spalten zu löschen und den *Inhalt* zu erweitern.
1. Ändern Sie den Datentyp für *Content.SentimentScore* in **Dezimalzahl**.
1. Wählen Sie *hotelReviewsSsKeyPhrases*, und wiederholen Sie die Schritte 9 und 10, um die Spalten zu löschen und den *Inhalt* zu erweitern. Für diese Tabelle sind keine Datentypänderungen erforderlich.

1. Klicken Sie in der Befehlsleiste auf **Close and Apply** (Schließen und Anwenden).

1. Klicken Sie im linken Navigationsbereich auf die Modellkachel, und überprüfen Sie, ob Power BI Beziehungen zwischen allen drei Tabellen anzeigt.

   ![Beziehungen überprüfen](media/knowledge-store-connect-power-bi/powerbi-relationships.png "Beziehungen überprüfen")

1. Doppelklicken Sie auf die einzelnen Beziehungen, und stellen Sie sicher, dass die **Kreuzfilterrichtung** auf **Beide** festgelegt ist.  Dadurch können visuelle Elemente aktualisiert werden, wenn ein Filter angewendet wird.

1. Klicken Sie im linken Navigationsbereich auf die Kachel „Bericht“, um Daten anhand von Visualisierungen zu untersuchen. Für Textfelder sind Tabellen und Karten hilfreiche Visualisierungen. Sie können Felder aus jeder der drei Tabellen auswählen, um die Tabelle oder Karte zu füllen. 

<!-- ## Try with larger data sets

We purposely kept the data set small to avoid charges for a demo walkthrough. For a more realistic experience, you can create and then attach a billable Cognitive Services resource to enable a larger number of transactions against the sentiment analyzer, keyphrase extraction, and language detector skills.

Create new containers in Azure Blob storage and upload each CSV file to its own container. Specify one of these containers in the data source creation step in Import data wizard.

| Description | Link |
|-------------|------|
| Free tier   | [HotelReviews_Free.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Small (500 Records) | [HotelReviews_Small.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Small.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Medium (6000 Records)| [HotelReviews_Medium.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Medium.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D)
| Large (Full dataset 35000 Records) | [HotelReviews_Large.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Large.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Be aware that very large data sets are expensive to process. This one costs roughly $1000 U.S dollars.|

In the enrichment step of the wizard, attach a billable [Cognitive Services](../cognitive-services/cognitive-services-apis-create-account.md) resource, created at the *S0* tier, in the same region as Azure Cognitive Search to use larger data sets. 

  ![Create a Cognitive Services resource](media/knowledge-store-connect-power-bi/create-cognitive-service.png "Create a Cognitive Services resource") -->

## <a name="clean-up"></a>Bereinigung

Wenn Sie in Ihrem eigenen Abonnement arbeiten, sollten Sie sich am Ende eines Projekts überlegen, ob Sie die erstellten Ressourcen noch benötigen. Ressourcen, die weiterhin ausgeführt werden, können Sie Geld kosten. Sie können entweder einzelne Ressourcen oder aber die Ressourcengruppe löschen, um den gesamten Ressourcensatz zu entfernen.

Ressourcen können im Portal über den Link **Alle Ressourcen** oder **Ressourcengruppen** im linken Navigationsbereich gesucht und verwaltet werden.

Denken Sie bei Verwendung eines kostenlosen Diensts an die Beschränkung auf maximal drei Indizes, Indexer und Datenquellen. Sie können einzelne Elemente über das Portal löschen, um unter dem Limit zu bleiben.

## <a name="next-steps"></a>Nächste Schritte

Im folgenden Artikel finden Sie Informationen dazu, wie Sie diesen Wissensspeicher mithilfe von Storage-Explorer untersuchen können.

> [!div class="nextstepaction"]
> [Anzeigen mit Storage-Explorer](knowledge-store-view-storage-explorer.md)