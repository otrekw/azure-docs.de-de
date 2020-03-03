---
title: 'Schnellstart: Erstellen und Verwenden einer Lernschleife mit dem SDK – Personalisierung'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Ihre Wissensdatenbank mit dem Client-SDK erstellen und verwalten.
ms.topic: quickstart
ms.date: 01/15/2020
zone_pivot_groups: programming-languages-set-six
ms.openlocfilehash: 7ebe22227b4323b2e6b1c3fc9ca31e171d1d97cd
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2020
ms.locfileid: "77524868"
---
# <a name="quickstart-personalizer-client-library"></a>Schnellstart: Personalisierungsclientbibliothek

Zeigen Sie personalisierten Inhalt in dieser Schnellstartanleitung mit dem Personalisierungsdienst an.

Führen Sie erste Schritte mit der Personalisierungsclientbibliothek aus. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben.

 * Rangfolge-API: Basierend auf den Echtzeitinformationen, die Sie für den Inhalt und zum Kontext angeben, wird das beste Element aus den Inhaltselementen ausgewählt.
 * Relevanz-API: Sie bestimmen die Relevanzbewertung basierend auf Ihren Geschäftsanforderungen und senden sie mit dieser API an die Personalisierung. Diese Bewertung kann ein einzelner Wert (z. B. „1“ für „Gut“ und „0“ für „Schlecht“) oder ein Algorithmus sein, den Sie anhand Ihrer Geschäftsanforderungen erstellt haben.

::: zone pivot="programming-language-csharp"
[!INCLUDE [Get intent with C# SDK](./includes/quickstart-sdk-csharp.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [Get intent with Node.js SDK](./includes/quickstart-sdk-nodejs.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Get intent with Python SDK](./includes/quickstart-sdk-python.md)]
::: zone-end

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
>[Funktionsweise der Personalisierung](how-personalizer-works.md)

* [Was ist die Personalisierung?](what-is-personalizer.md)
* [Wo können Sie Personalisierung verwenden?](where-can-you-use-personalizer.md)
* [Problembehandlung](troubleshooting.md)
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/python/sample.py).
