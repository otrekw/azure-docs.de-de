---
title: 'Schnellstart: Textmining mithilfe der Textanalyse-Clientbibliothek'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung, um Stimmungsanalyse und mehr mithilfe der Textanalyse-API aus Azure Cognitive Services durchzuführen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 10/07/2020
ms.author: aahi
keywords: Textmining, Stimmungsanalyse, Textanalyse
ms.custom: devx-track-python, devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
zone_pivot_groups: programming-languages-text-analytics
ms.openlocfilehash: 5a0856df71f87e49c1a7d627ba92419352c796d5
ms.sourcegitcommit: f311f112c9ca711d88a096bed43040fcdad24433
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2020
ms.locfileid: "94980958"
---
# <a name="quickstart-use-the-text-analytics-client-library"></a>Schnellstart: Verwenden der Textanalyse-Clientbibliothek

Verwenden Sie diesen Artikel, um in die Textanalyse-Clientbibliothek einzusteigen. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für Textmining.

Verwenden Sie die Textanalyse-Clientbibliothek für folgende Zwecke:

* Stimmungsanalyse
* Spracherkennung
* Entitätserkennung
* Schlüsselwortextraktion
* Opinion Mining

::: zone pivot="programming-language-csharp"

> [!IMPORTANT]
> * Die neueste stabile Version der Textanalyse-API ist `3.0`.
>    * Befolgen Sie nur die Anweisungen für die von Ihnen verwendete Version.
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Informationen finden Sie in der Referenzdokumentation weiter unten.
> * Wenn Sie die Textanalyse für Gesundheit oder asynchrone Vorgänge verwenden möchten, sehen Sie sich auf GitHub die Beispiele für [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics), [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/) oder [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics) an.

[!INCLUDE [v3 region availability](../includes/v3-region-availability.md)]

[!INCLUDE [C# quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

> [!IMPORTANT]
> * Die neueste stabile Version der Textanalyse-API ist `3.0`.
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Informationen finden Sie in der Referenzdokumentation weiter unten.
Wenn Sie die Textanalyse für Gesundheit oder asynchrone Vorgänge verwenden möchten, sehen Sie sich auf GitHub die Beispiele für [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics), [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/) oder [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics) an.

[!INCLUDE [Java quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

> [!IMPORTANT]
> * Die neueste stabile Version der Textanalyse-API ist `3.0`.
>    * Befolgen Sie nur die Anweisungen für die von Ihnen verwendete Version.
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Informationen finden Sie in der Referenzdokumentation weiter unten.
> * Diese Version der Textanalyse-Clientbibliothek können Sie auch [in Ihrem Browser](https://github.com/Azure/azure-sdk-for-js/blob/master/documentation/Bundling.md) ausführen.

[!INCLUDE [NodeJS quickstart](../includes/quickstarts/nodejs-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

> [!IMPORTANT]
> * Die neueste stabile Version der Textanalyse-API ist `3.0`.
>    * Befolgen Sie nur die Anweisungen für die von Ihnen verwendete Version.
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Informationen finden Sie in der Referenzdokumentation weiter unten. Wenn Sie die Textanalyse für Gesundheit oder asynchrone Vorgänge verwenden möchten, sehen Sie sich auf GitHub die Beispiele für [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics), [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/) oder [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics) an.

[!INCLUDE [Python quickstart](../includes/quickstarts/python-sdk.md)]

::: zone-end

::: zone pivot="programming-language-other"

## <a name="additional-language-support"></a>Zusätzliche Sprachunterstützung

Wenn Sie auf diese Registerkarte geklickt haben, wurde wahrscheinlich keine Schnellstartanleitung für Ihre bevorzugte Programmiersprache angezeigt. Keine Sorge, es stehen weitere Schnellstartanleitungen zur Verfügung. Verwenden Sie die Tabelle, um das richtige Beispiel für Ihre Programmiersprache zu finden.

| Sprache | Verfügbare Version | 
|----------|------------------------|
| Ruby     | [Version 2.1](ruby-sdk.md) | 
| Go       | [Version 2.1](go-sdk.md) | 

::: zone-end

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Lösung erkunden](../text-analytics-user-scenarios.md#analyze-recorded-inbound-customer-calls)

* [Übersicht über die Textanalyse](../overview.md)
* [Standpunktanalyse](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Entitätserkennung](../how-tos/text-analytics-how-to-entity-linking.md)
* [Sprache erkennen](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [Spracherkennung](../how-tos/text-analytics-how-to-language-detection.md)
