---
title: Entwicklerressourcen – Language Understanding
description: SDKs, REST-APIs und CLI unterstützen Sie beim Entwickeln von LUIS-Anwendungen (Language Understanding) in Ihrer Programmiersprache. Verwalten Sie Ihre Azure-Ressourcen und LUIS-Vorhersagen.
ms.topic: reference
ms.date: 03/16/2020
ms.openlocfilehash: 5e375157cef4789bc2980f6154ea8d59e765ff3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79457983"
---
# <a name="sdk-rest-and-cli-developer-resources-for-language-understanding-luis"></a>SDK-, REST- und CLI-Entwicklerressourcen für LUIS (Language Understanding)

SDKs, REST-APIs und CLI unterstützen Sie beim Entwickeln von LUIS-Anwendungen (Language Understanding) in Ihrer Programmiersprache. Verwalten Sie Ihre Azure-Ressourcen und LUIS-Vorhersagen.

## <a name="azure-resource-management"></a>Azure-Ressourcenverwaltung

Verwenden Sie die Azure Cognitive Services Management-Ebene, um die Ressource Language Understanding oder Cognitive Service zu erstellen, zu bearbeiten, aufzulisten und zu löschen.

Suchen Sie nach der Referenzdokumentation auf Grundlage des Tools:

* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/cognitiveservices#az-cognitiveservices-list)

* [Azure RM PowerShell](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/?view=azurermps-4.4.1#cognitive_services)


## <a name="language-understanding-authoring-and-prediction-requests"></a>Erstellungs- und Vorhersageanforderungen für Language Understanding

Der Zugriff auf den Language Understanding-Dienst erfolgt über eine Azure-Ressource, die Sie erstellen müssen. Dafür gibt es zwei Ressourcen:

* Verwenden Sie die Ressource **Erstellung** für das Training, um zu erstellen, zu bearbeiten, zu trainieren und zu veröffentlichen.
* Verwenden Sie **Vorhersage** als Runtime, um den Text eines Benutzers zu senden und eine Vorhersage zu erhalten.

Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).

Verwenden Sie den [Cognitive Services-Beispielcode](https://github.com/Azure-Samples/cognitive-services-quickstart-code), um die häufigsten Aufgaben zu erlernen und zu verwenden.

### <a name="rest-specifications"></a>REST-Spezifikationen

Die [LUIS REST-Spezifikationen](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/cognitiveservices/data-plane/LUIS) sind zusammen mit allen [Azure REST-Spezifikationen](https://github.com/Azure/azure-rest-api-specs) öffentlich auf GitHub erhältlich.

### <a name="rest-apis"></a>REST-APIs

Sowohl Erstellungs- als auch Vorhersage-Endpunkt-APIs sind über Rest-APIs verfügbar:

|type|Version|
|--|--|
|Erstellen|[Version 2](https://go.microsoft.com/fwlink/?linkid=2092087)<br>[Vorschau der Version 3](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview)|
|Vorhersage|[Version 2](https://go.microsoft.com/fwlink/?linkid=2092356)<br>[Version 3](https://westcentralus.dev.cognitive.microsoft.com/docs/services/luis-endpoint-api-v3-0/)|

### <a name="rest-endpoints"></a>REST-Endpunkte

LUIS weist zurzeit 2 Arten von Endpunkten auf:

* Erstellen auf dem Trainingsendpunkt
* Abfragevorhersage auf dem Laufzeitendpunkt.

|Zweck|URL|
|--|--|
|Erstellen auf dem Trainingsendpunkt|`https://{your-resource-name}.api.cognitive.microsoft.com/luis/api/v2.0/apps/{appID}/`|
|V2-Runtime: alle Vorhersagen am Laufzeitendpunkt|`https://{your-resource-name}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q={q}[&timezoneOffset][&verbose][&spellCheck][&staging][&bing-spell-check-subscription-key][&log]`|
|V3-Runtime: Versionsvorhersage am Laufzeitendpunkt|`https://{your-resource-name}.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/{appId}/versions/{versionId}/predict?query={query}[&verbose][&log][&show-all-intents]`|
|V3-Runtime: Slotvorhersagen am Laufzeitendpunkt|`https://{your-resource-name}.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/{appId}/slots/{slotName}/predict?query={query}[&verbose][&log][&show-all-intents]`|

In der folgende Tabelle werden die Parameter erläutert, die in der vorherigen Tabelle durch geschweifte Klammern `{}` gekennzeichnet sind.

|Parameter|Zweck|
|--|--|
|`your-resource-name`|Name der Azure-Ressource|
|`q` oder `query`|Äußerungstext, der von der Clientanwendung gesendet wird, wie z.B. ein Chatbot|
|`version`|Versionsname aus 10 Zeichen|
|`slot`| `production` oder `staging`|

### <a name="language-based-sdks"></a>Sprachbasierte SDKs

|Sprache |Referenzdokumentation|Paket|Beispiele|Schnellstarts|
|--|--|--|--|--|
|C#|[Erstellung](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring?view=azure-dotnet)</br>[Vorhersage](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime?view=azure-dotnet)|[NuGet-Erstellung](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Authoring/)<br>[NuGet-Vorhersage](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/)|[.NET SDK-Beispiele](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/LUIS)|[Erstellen und Verwalten einer App](sdk-authoring.md?pivots=programming-language-csharp)<br>[Abfragen eines Vorhersageendpunkts](sdk-query-prediction-endpoint.md)|
|Go|[Erstellung und Vorhersage](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.0/luis)|[SDK](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.0/luis)|[Erstellung](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/change-model/go)<br>[Vorhersage](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/analyze-text/go)|[Erstellung und Vorhersage mithilfe von REST](luis-get-started-get-intent-from-rest.md)|
|Java|[Erstellung und Vorhersage](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-java-stable)|[Maven-Erstellung](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-luis-authoring)<br>[Maven-Vorhersage](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-luis-runtime)|[Erstellung](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/change-model/java)<br>[Vorhersage](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/analyze-text/java)|[Erstellung und Vorhersage](luis-get-started-get-intent-from-rest.md)
|Node.js|[Erstellung](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/?view=azure-node-latest)<br>[Vorhersage](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/?view=azure-node-latest)|[NPM-Erstellung](https://www.npmjs.com/package/@azure/cognitiveservices-luis-authoring)<br>[NPM-Vorhersage](https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime)|[Erstellung](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/change-model/node)<br>[Vorhersage](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/analyze-text/node)|[Erstellung und Vorhersage mithilfe von REST](luis-get-started-get-intent-from-rest.md)|
|Python|[Erstellung und Vorhersage](sdk-authoring.md?pivots=programming-language-python)|[Pip](https://pypi.org/project/azure-cognitiveservices-language-luis/)|[Erstellung](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/LUIS/application_quickstart.py)|[Erstellung](sdk-authoring.md?pivots=programming-language-python)<br>[Vorhersage mithilfe von REST](luis-get-started-get-intent-from-rest.md)


### <a name="containers"></a>Container

Language Understanding (LUIS) stellt einen [Container](luis-container-howto.md) bereit, um lokale und in Containern enthaltene Versionen Ihrer App bereitzustellen.

### <a name="export-and-import-formats"></a>Export- und Importformate

Language Understanding bietet die Möglichkeit, Ihre App und ihre Modelle in einem JSON-Format, dem `.LU`-Format ([LUDown](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Ludown)), und einem komprimierten Paket für den Language Understanding-Container zu verwalten.

Das Importieren und Exportieren in diesen Formaten ist über die APIs und das LUIS-Portal möglich. Das Portal bietet Import und Export als Teil der App-Liste und der Versionsliste.

## <a name="other-tools-and-sdks"></a>Weitere Tools und SDKs

Das Bot Framework ist als [SDK](https://github.com/Microsoft/botframework) in einer Vielzahl von Sprachen und als Dienst mit [Azure Bot Service](https://dev.botframework.com/) verfügbar.

Bot Framework bietet [verschiedene Tools](https://github.com/microsoft/botbuilder-tools), um bei Language Understanding zu helfen, einschließlich:

* [LUDown](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Ludown): Erstellen von LUIS-Language Understanding-Modellen mithilfe von Markdowndateien
* [LUIS CLI](https://github.com/microsoft/botbuilder-tools/blob/master/packages/LUIS): Erstellen und Verwalten Ihrer LUIS.ai-Anwendungen
* [Dispatch](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Dispatch): Verwalten von übergeordneten und untergeordneten Apps
* [LUISGen](https://github.com/microsoft/botbuilder-tools/blob/master/packages/LUISGen): Automatisches Generieren von C#/Typescript-Klassen für Ihre LUIS-Absichten und -Entitäten
* [Bot Framework-Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases): Eine Desktopanwendung, mit der Entwickler von Bots mit dem Bot Framework SDK erstellte Bots testen und debuggen können
* [Bot Framework Composer](https://github.com/microsoft/BotFramework-Composer/blob/stable/README.md): Ein integriertes Entwicklungstool für Entwickler und bereichsübergreifende Teams zum Erstellen von Bots und Konversationsumgebungen mit Microsoft Bot Framework
* [microsoft/NLU.DevOps](https://github.com/microsoft/NLU.DevOps): Tools, die Continuous Integration und Continuous Deployment für NLU-Dienste unterstützen

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu allgemeinen [HTTP-Fehlercodes](luis-reference-response-codes.md)
* [Referenzdokumentation](https://docs.microsoft.com/azure/index) für alle APIs und SDKs
* [Bot Framework](https://github.com/Microsoft/botbuilder-dotnet) und [Azure Bot Service](https://dev.botframework.com/)
* [LUDown](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Ludown)
* [Cognitive-Container](../cognitive-services-container-support.md)
