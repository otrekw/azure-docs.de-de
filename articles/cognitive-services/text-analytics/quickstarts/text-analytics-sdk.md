---
title: 'Schnellstart: Textanalyse-Clientbibliothek v3 | Microsoft-Dokumentation'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung wird gezeigt, wie Sie Ihre Anwendungen mit der Textanalyse-API von Azure Cognitive Services verbinden.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
zone_pivot_groups: programming-languages-text-analytics
ms.openlocfilehash: cc3f48ec1113f954336cfae0bda2cba2499d9a1d
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80240121"
---
# <a name="quickstart-use-the-text-analytics-client-library"></a>Schnellstart: Verwenden der Textanalyse-Clientbibliothek

Hier finden Sie Informationen zu den ersten Schritten mit der Textanalyse-Clientbibliothek. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben.

Verwenden Sie die Textanalyse-Clientbibliothek für folgende Zwecke:

* Stimmungsanalyse
* Spracherkennung
* Entitätserkennung
* Schlüsselwortextraktion

::: zone pivot="programming-language-csharp"

> [!IMPORTANT]
> * Die aktuelle Vorschauversion der Textanalyse-API ist `3.0-preview`. Diese beinhaltet eine Public Preview für verbesserte [Standpunktanalyse](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) und [Erkennung benannter Entitäten (Named Entity Recognition, NER)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features). Die aktuelle stabile Version ist `2.1`.
>    * Befolgen Sie nur die Anweisungen für die von Ihnen verwendete Version.
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Informationen finden Sie in der Referenzdokumentation weiter unten.

[!INCLUDE [C# quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

> [!IMPORTANT]
> * Diese Schnellstartanleitung gilt nur für Version `3.0-preview` der Textanalyse-Clientbibliothek. Diese beinhaltet eine Public Preview für verbesserte [Standpunktanalyse](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) und [Erkennung benannter Entitäten (Named Entity Recognition, NER)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features).
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Informationen finden Sie in der Referenzdokumentation weiter unten.

[!INCLUDE [Java quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

> [!IMPORTANT]
> * Die aktuelle Vorschauversion der Textanalyse-API ist `3.0-preview`. Diese beinhaltet eine Public Preview für verbesserte [Standpunktanalyse](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) und [Erkennung benannter Entitäten (Named Entity Recognition, NER)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features). Die aktuelle stabile Version ist `2.1`.
>    * Befolgen Sie nur die Anweisungen für die von Ihnen verwendete Version.
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Informationen finden Sie in der Referenzdokumentation weiter unten.
> * Diese Version der Textanalyse-Clientbibliothek können Sie auch [in Ihrem Browser](https://github.com/Azure/azure-sdk-for-js/blob/master/documentation/Bundling.md) ausführen.

[!INCLUDE [NodeJS quickstart](../includes/quickstarts/nodejs-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

> [!IMPORTANT]
> * Die aktuelle Vorschauversion der Textanalyse-API ist `3.0-preview`. Diese beinhaltet eine Public Preview für verbesserte [Standpunktanalyse](../how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) und [Erkennung benannter Entitäten (Named Entity Recognition, NER)](../how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features). Die aktuelle stabile Version ist `2.1`.
>    * Befolgen Sie nur die Anweisungen für die von Ihnen verwendete Version.
> * Im Code dieses Artikels werden der Einfachheit halber synchrone Methoden und ein ungeschützter Anmeldeinformationsspeicher verwendet. Für Produktionsszenarien wird aus Leistungs- und Skalierbarkeitsgründen die Verwendung der asynchronen Batchmethoden empfohlen. Informationen finden Sie in der Referenzdokumentation weiter unten. 

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
