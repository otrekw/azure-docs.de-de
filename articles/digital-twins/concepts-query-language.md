---
title: Abfragesprache
titleSuffix: Azure Digital Twins
description: Grundlagen der Azure Digital Twins-Abfragesprache
author: baanders
ms.author: baanders
ms.date: 4/22/2021
ms.topic: conceptual
ms.service: digital-twins
ms.custom: contperf-fy21q2
ms.openlocfilehash: 8a5a092da37b453b719669109bd4924b032c0604
ms.sourcegitcommit: 6323442dbe8effb3cbfc76ffdd6db417eab0cef7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110616221"
---
# <a name="about-the-query-language-for-azure-digital-twins"></a>Abfragesprache für Azure Digital Twins

Beachten Sie, dass im Mittelpunkt von Azure Digital Twins der [Zwillingsgraph](concepts-twins-graph.md) steht, der sich aus digitalen Zwillingen und Beziehungen zusammensetzt. 

Dieser Graph kann abgefragt werden, um Informationen zu den darin enthaltenen digitalen Zwillingen und Beziehungen abzurufen. Diese Abfragen werden in einer benutzerdefinierten SQL-ähnlichen Abfragesprache mit der Bezeichnung **Azure Digital Twins-Abfragesprache** geschrieben. Diese ähnelt der [IoT Hub-Abfragesprache](../iot-hub/iot-hub-devguide-query-language.md) mit vielen vergleichbaren Features.

In diesem Artikel werden die Grundlagen der Abfragesprache und ihrer Funktionen beschrieben. Ausführlichere Beispiele für die Abfragesyntax und das Ausführen von Abfrageanforderungen finden Sie unter [Vorgehensweise: Abfragen des Zwillingsdiagramms von Azure Digital Twins](how-to-query-graph.md).

## <a name="about-the-queries"></a>Informationen zu Abfragen

Sie können die Abfragesprache von Azure Digital Twins verwenden, um digitale Zwillinge gemäß folgender Aspekte abzurufen:
* ihren Eigenschaften (einschließlich [Tageigenschaften](how-to-use-tags.md))
* Modelle
* relationships
  - Eigenschaften der Beziehungen

Wenn Sie eine Abfrage aus einer Client-App an den Dienst senden möchten, verwenden Sie die Azure Digital Twins-[Abfrage-API](/rest/api/digital-twins/dataplane/query). Eine Möglichkeit, die API zu verwenden, ist durch eins der [SDKs für Azure Digital Twins](concepts-apis-sdks.md#overview-data-plane-apis).

[!INCLUDE [digital-twins-query-reference.md](../../includes/digital-twins-query-reference.md)]

## <a name="considerations-for-querying"></a>Hinweise zur Abfrage

Beachten Sie beim Schreiben von Abfragen für Azure Digital Twins Folgendes:
* **Denken Sie an die Beachtung der Groß-/Kleinschreibung:** Bei allen Azure Digital Twins-Abfragevorgängen wird die Groß-/Kleinschreibung beachtet. Verwenden Sie daher genau die in den Modellen definierten Namen. Wenn Eigenschaftennamen falsch oder in falscher Groß-/Kleinschreibung geschrieben werden, ist das Resultset leer, und es werden keine Fehler zurückgegeben.
* **Versehen Sie einfache Anführungszeichen mit Escapezeichen:** Wenn die Daten Ihres Abfragetexts ein einfaches Anführungszeichen enthalten, muss dieses mit dem Escapezeichen `\` versehen werden. Im folgenden Beispiel wird der Umgang mit dem Eigenschaftswert *D'Souza* gezeigt:

  :::code language="sql" source="~/digital-twins-docs-samples/queries/examples.sql" id="EscapedSingleQuote":::

* **Berücksichtigen Sie mögliche Latenz**: Nachdem Sie eine Änderung an den Daten in Ihrem Graphen vorgenommen haben, kann eine Latenz von bis zu 10 Sekunden auftreten, bevor die Änderungen in Abfragen berücksichtigt werden. Bei der [GetDigitalTwin-API](how-to-manage-twin.md#get-data-for-a-digital-twin) tritt diese Verzögerung nicht auf. Wenn Sie eine sofortige Antwort benötigen, verwenden Sie daher den API-Befehl anstelle von Abfragen, damit die Änderung sofort berücksichtigt wird.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das Schreiben von Abfragen, und untersuchen Sie Clientcodebeispiele in [Gewusst wie: Abfragen des Zwillingsgraphen](how-to-query-graph.md).