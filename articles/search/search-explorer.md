---
title: Suchexplorer-Abfragetool im Azure-Portal
titleSuffix: Azure Cognitive Search
description: In diesem Schnellstart im Azure-Portal verwenden Sie den Suchexplorer, um die Abfragesyntax zu lernen, Abfrageausdrücke zu testen oder ein Suchdokument zu untersuchen. Der Suchexplorer fragt Indizes in Azure Cognitive Search ab.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 09/25/2020
ms.openlocfilehash: adcd07ad370ad9f1301caa41c494a33958743dd8
ms.sourcegitcommit: 4313e0d13714559d67d51770b2b9b92e4b0cc629
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2020
ms.locfileid: "91398407"
---
# <a name="quickstart-use-search-explorer-to-run-queries-in-the-portal"></a>Schnellstart: Verwenden des Suchexplorers zum Ausführen von Abfragen im Portal

Der **Suchexplorer** ist ein integriertes Abfragetool, das zum Ausführen von Abfragen über einen Suchindex in Azure Cognitive Search verwendet wird. Mit diesem Tool können Sie problemlos Abfragesyntax erlernen, einen Abfrage- oder Filterausdruck testen oder eine Datenaktualisierung bestätigen, indem Sie überprüfen, ob neue Inhalte im Index vorhanden sind.

In diesem Schnellstart wird der Suchexplorer anhand eines vorhandenen Index veranschaulicht. Anforderungen werden mit der [Search-REST-API](/rest/api/searchservice/) formuliert, und Antworten werden als JSON-Dokumente zurückgegeben.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Lernprogramm beginnen können, benötigen Sie Folgendes:

+ Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/).

+ Ein Azure Cognitive Search-Dienst. [Erstellen Sie einen Dienst](search-create-service-portal.md), oder wechseln Sie in Ihrem aktuellen Abonnement [zu einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). Für diesen Schnellstart können Sie einen kostenlosen Dienst verwenden. 

+ *realestate-us-sample-index* wird für diesen Schnellstart verwendet. Verwenden Sie den [**Datenimport**](search-import-data-portal.md)-Assistenten, um den Index zu erstellen. Wenn Sie im ersten Schritt nach der Datenquelle gefragt werden, wählen Sie **Beispiele** und dann die Datenquelle **realestate-us-sample** aus. Übernehmen Sie zum Erstellen des Index alle Standardeinstellungen des Assistenten.

## <a name="start-search-explorer"></a>Starten des Suchexplorers

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) im Dashboard die Seite mit dem Suchdienst, oder [suchen Sie Ihren Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).

1. Öffnen Sie den Suchexplorer über die Befehlsleiste:

   :::image type="content" source="media/search-explorer/search-explorer-cmd2.png" alt-text="Befehl für den Suchexplorer im Portal" border="false":::

    Oder verwenden Sie die eingebettete Registerkarte **Suchexplorer** in einem geöffneten Index:

   :::image type="content" source="media/search-explorer/search-explorer-tab.png" alt-text="Befehl für den Suchexplorer im Portal" border="false":::

## <a name="unspecified-query"></a>Abfrage ohne Angabe

Führen Sie für einen ersten Blick auf den Inhalt eine leere Suche aus, indem Sie ohne Angabe von Begriffen auf **Suchen** klicken. Eine leere Suche ist eine sinnvolle erste Abfrage, da sie vollständige Dokumente zurückgibt, sodass Sie sich mit dem Aufbau des Dokuments vertraut machen können. Bei einer leeren Suche gibt es keinen Suchrang, und die Dokumente werden in beliebiger Reihenfolge (`"@search.score": 1` für alle Dokumente) zurückgegeben. Standardmäßig werden in einer Suchanforderung 50 Dokumente zurückgegeben.

Die äquivalente Syntax für eine leere Suche ist `*` oder `search=*`.
   
   ```http
   search=*
   ```

   **Ergebnisse**
   
   :::image type="content" source="media/search-explorer/search-explorer-example-empty.png" alt-text="Befehl für den Suchexplorer im Portal" border="false":::

## <a name="free-text-search"></a>Freitextsuche

Freiformabfragen mit oder ohne Operatoren sind nützlich zum Simulieren von benutzerdefinierten Abfragen, die von einer benutzerdefinierten App an die kognitive Azure-Suche gesendet werden. Nur die Felder, die in der Indexdefinition als **Durchsuchbar** gekennzeichnet sind, werden auf Übereinstimmungen überprüft. 

Beachten Sie, dass bei der Angabe von Suchkriterien wie z. B. Abfragebegriffen oder -ausdrücken der Suchrang eine Rolle spielt. Im folgenden Beispiel wird eine Freitextsuche veranschaulicht.

   ```http
   Seattle apartment "Lake Washington" miele OR thermador appliance
   ```

   **Ergebnisse**

   Sie können die Ergebnisse mit STRG+F nach bestimmten Begriffen durchsuchen.

   :::image type="content" source="media/search-explorer/search-explorer-example-freetext.png" alt-text="Befehl für den Suchexplorer im Portal" border="false":::

## <a name="count-of-matching-documents"></a>Anzahl übereinstimmender Dokumente 

Fügen Sie **$count=true** hinzu, um die Anzahl der Übereinstimmungen in einem Index abzurufen. Bei einer leeren Suche ist die Anzahl die Gesamtanzahl der Dokumente im Index. Bei einer qualifizierten Suche ist es die Anzahl der Dokumente, die der eingegebenen Abfrage entsprechen.

   ```http
   $count=true
   ```

   **Ergebnisse**

   :::image type="content" source="media/search-explorer/search-explorer-example-count.png" alt-text="Befehl für den Suchexplorer im Portal" border="false":::

## <a name="limit-fields-in-search-results"></a>Beschränken der Felder in den Suchergebnissen

Fügen Sie [ **$select**](search-query-odata-select.md) hinzu, um die Ergebnisse auf die explizit benannten Felder zu beschränken und damit die Lesbarkeit der Ausgabe im **Suchexplorer** zu erhöhen. Um die Suchzeichenfolge und **$count=true** beizubehalten, stellen Sie den Argumenten das Präfix **&** voran. 

   ```http
   search=seattle condo&$select=listingId,beds,baths,description,street,city,price&$count=true
   ```

   **Ergebnisse**

   :::image type="content" source="media/search-explorer/search-explorer-example-selectfield.png" alt-text="Befehl für den Suchexplorer im Portal" border="false":::

## <a name="return-next-batch-of-results"></a>Zurückgeben des nächsten Batches von Ergebnissen

Die kognitive Azure-Suche gibt die ersten 50 Übereinstimmungen basierend auf dem Suchrang zurück. Um den nächsten Satz von übereinstimmenden Dokumenten abzurufen, fügen Sie **$top=100,&$skip=50** an, um das Resultset auf 100 Dokumente zu vergrößern (Standardwert 50, Höchstwert 1.000) und die ersten 50 Dokumente zu überspringen. Denken Sie daran, dass Sie Suchkriterien angeben müssen, z.B. einen Abfragebegriff oder -ausdruck, um priorisierte Ergebnisse zu erhalten. Beachten Sie, dass sich die Suchbewertungen verringern, je weiter Sie in die Suchergebnisse vordringen.

   ```http
   search=seattle condo&$select=listingId,beds,baths,description,street,city,price&$count=true&$top=100&$skip=50
   ```

   **Ergebnisse**

   :::image type="content" source="media/search-explorer/search-explorer-example-topskip.png" alt-text="Befehl für den Suchexplorer im Portal" border="false":::

## <a name="filter-expressions-greater-than-less-than-equal-to"></a>Filterausdrücke (größer als, kleiner als, gleich)

Verwenden Sie den Parameter [ **$filter**](search-query-odata-filter.md), wenn Sie anstelle einer Freitextsuche präzise Kriterien angeben möchten. Das Feld muss im Index als **Filterbar**  gekennzeichnet sein. In diesem Beispiel wird für die Anzahl von Schlafzimmern nach einem Wert größer als 3 gesucht:

   ```http
   search=seattle condo&$filter=beds gt 3&$count=true
   ```
   
   **Ergebnisse**

   :::image type="content" source="media/search-explorer/search-explorer-example-filter.png" alt-text="Befehl für den Suchexplorer im Portal" border="false":::

## <a name="order-by-expressions"></a>Sortierausdrücke

Fügen Sie [ **$orderby**](search-query-odata-orderby.md) hinzu, um die Ergebnisse nach einem anderen Feld als der Suchbewertung zu sortieren. Das Feld muss im Index als **Sortierbar**  gekennzeichnet sein. Als Beispielausdruck zum Testen können Sie den folgenden verwenden:

   ```http
   search=seattle condo&$select=listingId,beds,price&$filter=beds gt 3&$count=true&$orderby=price asc
   ```
   
   **Ergebnisse**

   :::image type="content" source="media/search-explorer/search-explorer-example-ordery.png" alt-text="Befehl für den Suchexplorer im Portal" border="false":::

Die Ausdrücke **$filter** und **$orderby** sind OData-Konstrukte. Weitere Informationen finden Sie unter [OData Expression Syntax for Azure Search](/rest/api/searchservice/odata-expression-syntax-for-azure-search) (OData-Ausdruckssyntax für Azure Search).

<a name="start-search-explorer"></a>

## <a name="takeaways"></a>Wesentliche Punkte

In diesem Schnellstart haben Sie den **Suchexplorer** verwendet, um einen Index mithilfe der REST-API abzufragen.

+ Ergebnisse werden als ausführliche JSON-Dokumente zurückgegeben, sodass Sie den Dokumentaufbau und den Inhalt vollständig anzeigen können. Sie können in den Beispielen gezeigte Abfrageausdrücke verwenden, um die zurückgegebenen Felder einzuschränken.

+ Dokumente bestehen aus allen Feldern, die im Index als **abrufbar** gekennzeichnet sind. Klicken Sie zum Anzeigen von Indexattributen im Portal auf der Übersichtsseite der Suche in der Liste **Indizes** auf *realestate-us-sample*.

+ Freiformabfragen ähneln der Eingabe in kommerziellen Webbrowsern und eignen sich zum Testen der Endbenutzererfahrung. Beim integrierten Beispielimmobilienindex könnten Sie z.B. „Seattle apartments lake washington“ eingeben und dann mit STRG+F Begriffe in den Suchergebnissen finden. 

+ Abfrage- und Filterausdrücke werden in einer Syntax formuliert, die von Azure Cognitive Search unterstützt wird. Der Standardwert ist eine [einfache Syntax](/rest/api/searchservice/simple-query-syntax-in-azure-search), Sie können aber optional für leistungsstarke Abfragen die [vollständige Lucene-Syntax](/rest/api/searchservice/lucene-query-syntax-in-azure-search) verwenden. [Filterausdrücke](/rest/api/searchservice/odata-expression-syntax-for-azure-search) folgen einer OData-Syntax.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie in Ihrem eigenen Abonnement arbeiten, sollten Sie sich am Ende eines Projekts überlegen, ob Sie die erstellten Ressourcen noch benötigen. Ressourcen, die weiterhin ausgeführt werden, können Sie Geld kosten. Sie können entweder einzelne Ressourcen oder aber die Ressourcengruppe löschen, um den gesamten Ressourcensatz zu entfernen.

Ressourcen können im Portal über den Link **Alle Ressourcen** oder **Ressourcengruppen** im linken Navigationsbereich gesucht und verwaltet werden.

Denken Sie bei Verwendung eines kostenlosen Diensts an die Beschränkung auf maximal drei Indizes, Indexer und Datenquellen. Sie können einzelne Elemente über das Portal löschen, um unter dem Limit zu bleiben. 

## <a name="next-steps"></a>Nächste Schritte

Um mehr über Abfragestrukturen und -syntax zu erfahren, verwenden Sie Postman oder ein gleichwertiges Tool, um Abfrageausdrücke zu erstellen, die mehr Teile der API nutzen. Die [Search-REST-API](/rest/api/searchservice/) ist besonders hilfreich für das Lernen und Erkunden.

> [!div class="nextstepaction"]
> [Erstellen einer einfachen Abfrage in Postman](search-query-simple-examples.md)