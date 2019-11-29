---
title: Aufrufen der Textanalyse-API
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird erläutert, wie Sie die Textanalyse-REST-API von Azure Cognitive Services und Postman aufrufen können.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: aahi
ms.openlocfilehash: afb576c265ccdd4a014ed678331f030a0442a197
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2019
ms.locfileid: "74286603"
---
# <a name="how-to-call-the-text-analytics-rest-api"></a>Aufrufen der Textanalyse-REST-API

Aufrufe der **Textanalyse-API** sind HTTP POST/GET-Aufrufe, die Sie in jeder Sprache formulieren können. In diesem Artikel verwenden wir REST und [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop), um wesentliche Konzepte zu demonstrieren.

Jede Anforderung muss Ihren Zugriffsschlüssel und einen HTTP-Endpunkt enthalten. Der Endpunkt gibt die Region an, die Sie bei der Registrierung ausgewählt haben, die Dienst-URL und eine in der Anforderung verwendete Ressource: `sentiment`, `keyphrases`, `languages` und `entities`. 

Denken Sie daran, dass Textanalyse zustandslos ist, weshalb es keine zu verwaltenden Datenobjekte gibt. Ihr Text wird hochgeladen, beim Empfang analysiert, und Ergebnisse werden sofort an die aufrufende Anwendung zurückgegeben.

[!INCLUDE [text-analytics-api-references](../includes/text-analytics-api-references.md)]

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

<a name="json-schema"></a>

## <a name="json-schema-definition"></a>JSON-Schemadefinition

Die Eingabe muss JSON in unformatiertem, unstrukturiertem Text sein. XML wird nicht unterstützt. Das Schema ist einfach, es besteht aus den in der folgenden Liste beschriebenen Elementen. 

Sie können derzeit dieselben Dokumenten für alle Textanalysevorgänge senden: Standpunktanalyse, Schlüsselbegriff, Sprachenerkennung und Entitätsidentifikation. (Das Schema wird in der Zukunft wahrscheinlich für jede Analyse variieren.)

| Element | Gültige Werte | Erforderlich? | Verwendung |
|---------|--------------|-----------|-------|
|`id` |Der Datentyp ist „Zeichenfolge“ (string), aber in der Praxis sind Dokument-IDs eher ganze Zahlen (integer). | Erforderlich | Das System verwendet die IDs, die Sie angeben, um die Ausgabe zu strukturieren. Sprachcodes, Schlüsselbegriffe und Stimmungspunktzahlen werden für jede ID in der Anforderung generiert.|
|`text` | Unstrukturierter, unformatierter Text mit bis zu 5.120 Zeichen. | Erforderlich | Für die Erkennung der Sprache kann der Text in einer beliebigen Sprache ausgedrückt werden. Für die Standpunktanalyse, Schlüsselbegriffserkennung und Entitätsidentifikation muss der Text in einer [unterstützten Sprache](../text-analytics-supported-languages.md) sein. |
|`language` | Aus 2 Zeichen bestehender [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)-Code für eine [unterstützte Sprache](../text-analytics-supported-languages.md) | Varies | Erforderlich für die Standpunktanalyse, Schlüsselbegriffserkennung und Entitätsverknüpfung, optional für die Sprachenerkennung. Es gibt keinen Fehler, wenn Sie sie ausschließen, aber die Analyse wird ohne geschwächt. Der Sprachcode sollte dem von Ihnen bereitgestellten `text` entsprechen. |

Weitere Informationen zu Limits finden Sie unter [Übersicht der Textanalyse > Datenlimits](../overview.md#data-limits). 

## <a name="set-up-a-request-in-postman"></a>Einrichten einer Anforderung in Postman

Der Dienst akzeptiert Anforderungen mit einer Größe von bis zu 1 MB. Wenn Sie Postman (oder ein anderes Web-API-Testtool) verwenden, richten Sie den Endpunkt so ein, dass er die Ressource einschließt, die Sie verwenden möchten, und geben Sie den Zugriffsschlüssel in einem Anforderungsheader an. Jeder Vorgang erfordert, dass Sie die entsprechende Ressource an den Endpunkt anfügen. 

1. In Postman:

   + Wählen Sie **Post** als Typ der Anforderung aus.
   + Fügen Sie den Endpunkt ein, den Sie von der Portalseite kopiert haben.
   + Fügen Sie eine Ressource an.

   Ressourcenendpunkte sind wie folgt (Ihre Region kann davon abweichen):

   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/keyPhrases`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/languages`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/entities`

2. Legen Sie die drei Anforderungsheader fest:

   + `Ocp-Apim-Subscription-Key`: Ihr aus dem Azure-Portal erhaltenes Zugriffstoken.
   + `Content-Type`: application/json.
   + `Accept`: application/json.

   Ihre Anforderung sollte in etwa wie im folgenden Screenshot aussehen, wobei eine **/keyPhrases**-Ressource angenommen wird.

   ![Screenshot der Anforderung mit Endpunkt und Headern](../media/postman-request-keyphrase-1.png)

4. Klicken Sie auf **Body**, und wählen Sie **raw** als Format aus.

   ![Screenshot der Anforderung mit Textkörpereinstellungen (body)](../media/postman-request-body-raw.png)

5. Fügen Sie JSON-Dokumente in einem Format ein, das für die beabsichtigte Analyse gültig ist. Weitere Informationen zu einer bestimmten Analyse finden Sie in den folgenden Themen:

  + [Sprachenerkennung](text-analytics-how-to-language-detection.md)  
  + [Schlüsselbegriffserkennung](text-analytics-how-to-keyword-extraction.md)  
  + [Standpunktanalyse](text-analytics-how-to-sentiment-analysis.md)  
  + [Entitätserkennung](text-analytics-how-to-entity-linking.md)  


6. Klicken Sie auf **Send**, um die Anforderung zu senden. Weitere Informationen zur Anzahl an Anforderungen, die Sie pro Minute und Sekunde senden können, finden Sie in der Übersicht im Abschnitt [Datengrenzwerte](../overview.md#data-limits).

   In Postman wird die Antwort im nächsten Fenster nach unten als einzelnes JSON-Dokument angezeigt, das für jede in der Anforderung angegebene Dokument-ID ein Element enthält.

## <a name="see-also"></a>Weitere Informationen 

 [Übersicht über die Textanalyse](../overview.md)  
 [Häufig gestellte Fragen (FAQ)](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Sprache erkennen](text-analytics-how-to-language-detection.md)
