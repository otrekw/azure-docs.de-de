---
title: Übersicht über die Suche in Azure API for FHIR
description: In diesem Artikel wird eine Übersicht über die FHIR-Suche beschrieben, die in Azure API for FHIR
author: stevewohl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 4/21/2021
ms.author: cavoeg
ms.openlocfilehash: dce155da36579f94012a8dc9f6275a43d99519d3
ms.sourcegitcommit: 52491b361b1cd51c4785c91e6f4acb2f3c76f0d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108322555"
---
# <a name="overview-of-fhir-search"></a>Übersicht über die FHIR-Suche

Die FHIR-Spezifikation definiert die Grundlagen der Suche nach FHIR-Ressourcen. Dieser Artikel führt Sie durch einige wichtige Aspekte beim Durchsuchen von Ressourcen in FHIR. Ausführliche Informationen zum Durchsuchen von FHIR-Ressourcen finden Sie unter [Suchen](https://www.hl7.org/fhir/search.html) in der HL7 FHIR-Spezifikation.

FHIR-Suchvorgänge können für einen bestimmten Ressourcentyp, ein angegebenes [Depot](https://www.hl7.org/fhir/compartmentdefinition.html)oder alle Ressourcen durchgeführt werden. Die einfachste Möglichkeit zum Ausführen einer Suche in FHIR ist die Verwendung einer `GET` Anforderung. Wenn Sie beispielsweise alle Patienten in der Datenbank abrufen möchten, können Sie die folgende Anforderung verwenden: 

`GET {{FHIR URL}}/Patient`

Sie können auch mit `POST` suchen. Dies ist hilfreich, wenn die Abfragezeichenfolge zu lang ist. Für die Suche mit `POST` können die Suchparameter als Formulartext übermittelt werden. Dies ermöglicht eine längere, komplexere Reihe von Abfrageparametern, die in einer Abfragezeichenfolge schwer zu erkennen und zu verstehen sind.

Wenn die Suchanforderung erfolgreich ist, erhalten Sie eine FHIR-Paketantwort vom Typ `searchset` . Wenn die Suche fehlschlägt, finden Sie diese Details in der `OperationOutcome` , damit Sie besser verstehen können, warum die Suche fehlgeschlagen ist.

In den folgenden Abschnitten werden die verschiedenen Aspekte der Suche behandelt. Nachdem Sie diese Details überprüft haben, finden Sie auf unserer **Seite Beispiele** Beispiele für Suchvorgänge, die Sie im Azure API for FHIR vornehmen können.

## <a name="search-parameters"></a>Suchparameter

Wenn Sie eine Suche durchführen, sollten Sie die Suche basierend auf verschiedenen Attributen der Ressource in Betracht ziehen.  Diese Attribute werden als Suchparameter bezeichnet. Jede Ressource verfügt über einen Satz definierter Suchparameter. Der Suchparameter muss in der Datenbank definiert und indiziert werden, damit Sie erfolgreich nach ihm suchen können.

Jeder Suchparameter hat einen definierten Datentyp. Der Azure API for FHIR unterstützt alle [Datentypen mit](https://www.hl7.org/fhir/search.html#ptypes) Ausnahme des speziellen **Typs**:


| **Suchparametertyp**  | **Unterstützt: PaaS** | **Unterstützt: OSS (SQL)** | **Unterstützt: OSS (Cosmos DB)** |
| -------------------------  | -------------------- | ------------------------- | ------------------------------- |
|  number                    | Ja                  | Ja                       | Ja                             |
|  date                      | Ja                  | Ja                       | Ja                             |
|  Zeichenfolge                    | Ja                  | Ja                       | Ja                             |
|  token                     | Ja                  | Ja                       | Ja                             |
|  Referenz                 | Ja                  | Ja                       | Ja                             |
|  Zusammengesetzt                 | Ja                  | Ja                       | Ja                             |
|  quantity                  | Ja                  | Ja                       | Ja                             |
|  uri                       | Ja                  | Ja                       | Ja                             |
|  speziell                   | Nein                   | Nein                        | Nein                              |

### <a name="common-search-parameters"></a>Allgemeine Suchparameter

Es gibt [allgemeine Suchparameter,](https://www.hl7.org/fhir/search.html#all) die für alle Ressourcen gelten. Diese sind unten aufgeführt, zusammen mit ihrer Unterstützung innerhalb der Azure API for FHIR:

| **Allgemeiner Suchparameter** | **Unterstützt: PaaS** | **Unterstützt: OSS (SQL)** | **Unterstützt: OSS (Cosmos DB)** | **Kommentar**                    |
| --------------------------  | -------------------- | ------------------------- | ------------------------------- | ------------------------------ |
| _id                         | Ja                  | Ja                       | Ja                             |                                |
| _lastUpdated                | Ja                  | Ja                       | Ja                             |                                |
| _tag                        | Ja                  | Ja                       | Ja                             |                                | 
| _type                       | Ja                  | Ja                       | Ja                             |                                |
| _security                   | Ja                  | Ja                       | Ja                             |                                |
| _profile                    | Ja                  | Ja                       | Ja                             | **Hinweis:** Wenn Sie Ihre R4-Datenbank vor dem 20. Februar 2021 erstellt haben, müssen Sie einen Neuindizierungsauftrag ausführen, um **_profile.**                                                      |
| _text                       | Nein                   | Nein                        | Nein                              |                                |
| _content                    | Nein                   | Nein                        | Nein                              |                                |
| _has                        | Teilweise                   | Teilweise                        | Ja                              |                                |
| _query                      | Nein                   | Nein                        | Nein                              |                                |
| _filter                     | Nein                   | Nein                        | Nein                              |                                |
| _list                       | Nein                   | Nein                        | Nein                              |                                |

### <a name="resource-specific-parameters"></a>Ressourcenspezifische Parameter

Mit dem Azure API for FHIR unterstützen wir fast alle ressourcenspezifischen Suchparameter, die durch die FHIR-Spezifikation definiert sind. Die einzigen Suchparameter, die wir nicht unterstützen, sind unter den folgenden Links verfügbar:

* [STU3– Nicht unterstützte Suchparameter](https://github.com/microsoft/fhir-server/blob/main/src/Microsoft.Health.Fhir.Core/Data/Stu3/unsupported-search-parameters.json)

* [Nicht unterstützte R4-Suchparameter](https://github.com/microsoft/fhir-server/blob/main/src/Microsoft.Health.Fhir.Core/Data/R4/unsupported-search-parameters.json)

Sie können auch die aktuelle Unterstützung für Suchparameter in der [FHIR Capability Statement](https://www.hl7.org/fhir/capabilitystatement.html) mit der folgenden Anforderung anzeigen:

`GET {{FHIR URL}}/metadata`

Um die Suchparameter in der Capability-Anweisung anzuzeigen, navigieren Sie zu , `CapabilityStatement.rest.resource.searchParam` um die Suchparameter für jede Ressource anzuzeigen und `CapabilityStatement.rest.searchParam` die Suchparameter für alle Ressourcen zu finden.

> [!NOTE]
> Der Azure API for FHIR erstellt oder indiziert nicht automatisch Suchparameter, die nicht durch die FHIR-Spezifikation definiert sind. Wir unterstützen Sie jedoch beim Definieren Ihrer eigenen Suchparameter.

### <a name="composite-search-parameters"></a>Zusammengesetzte Suchparameter

Mit dem Azure API for FHIR unterstützen wir die folgenden Suchparametertyppaare:

* Referenz, Token
* Token, Datum
* Token, Zahl, Zahl
* Token, Menge
* Token, Zeichenfolge
* Token, Token

Weitere Informationen finden Sie unter HL7 Composite Search Parameters ( Parameter für die [zusammengesetzte HL7-Suche).](https://www.hl7.org/fhir/search.html#composite) 

> [!NOTE]
> Zusammengesetzte Suchparameter unterstützen keine Modifizierer gemäß FHIR-Spezifikation.

 ### <a name="modifiers--prefixes"></a>Modifizierer & Präfixe

[Modifizierer](https://www.hl7.org/fhir/search.html#modifiers) ermöglichen es Ihnen, den Suchparameter zu ändern. Im Folgenden finden Sie eine Übersicht über alle FHIR-Modifizierer und die Unterstützung im Azure API for FHIR. 

| **Modifizierer** | **Unterstützt: PaaS** | **Unterstützt: OSS (SQL)** | **Unterstützt: OSS (Cosmos DB)** |
| ------------- | -------------------- | ------------------------- | ------------------------------- |
|  :missing     | Ja                  | Ja                       | Ja                             |
|  :exact       | Ja                  | Ja                       | Ja                             | 
|  :contains    | Ja                  | Ja                       | Ja                             | 
|  :text        | Ja                  | Ja                       | Ja                             | 
|  :type (Referenz) | Ja             | Ja                       | Ja                             | 
|  :not         | Ja                  | Ja                       | Ja                             | 
|  :below (URI) | Ja                  | Ja                       | Ja                             |  
|  :above (URI) | Nein                   | Nein                        | Nein                              | 
|  :in (Token)  | Nein                   | Nein                        | Nein                              | 
|  :below (Token) | Nein                 | Nein                        | Nein                              | 
|  :above (Token) | Nein                 | Nein                        | Nein                              | 
|  :not-in (Token) | Nein                | Nein                        | Nein                              | 

Für Suchparameter mit einer bestimmten Reihenfolge (Zahlen, Datumsangaben und Mengen) können Sie ein Präfix für den Parameter verwenden, um Übereinstimmungen zu finden. [](https://www.hl7.org/fhir/search.html#prefix) Die Azure API for FHIR unterstützt alle Präfixe.

 ### <a name="search-result-parameters"></a>Suchergebnisparameter



Um die zurückgegebenen Ressourcen zu verwalten, gibt es andere Suchergebnisparameter, die Sie in Ihrer Suche verwenden können. Weitere Informationen zur Verwendung der einzelnen Suchergebnisparameter finden Sie auf der [HL7-Website.](https://www.hl7.org/fhir/search.html#return) 

| **Suchergebnisparameter**  | **Unterstützt: PaaS** | **Unterstützt: OSS (SQL)** | **Unterstützt: OSS (Cosmos DB)** | **Kommentare**                 |
| ----------------------------  | -------------------- | ------------------------- | ------------------------------- | -----------------------------|
| _elements                     | Ja                  | Ja                       | Ja                             |  Issue [1256](https://github.com/microsoft/fhir-server/issues/1256)                              |
| _count                        | Ja                  | Ja                       | Ja                             | _count ist auf 1.000 Ressourcen beschränkt. Wenn er höher als 1000 festgelegt ist, werden nur 1.000 zurückgegeben, und im Paket wird eine Warnung zurückgegeben.                               |
| _include                      | Ja                  | Ja                       | Ja                             | Enthaltene Elemente sind auf 100 beschränkt. _include unter PaaS und OSS unter Cosmos DB enthält keine Unterstützung für :iterate [(#1313).](https://github.com/microsoft/fhir-server/issues/1313)                               |
| _revinclude                   | Ja                  | Ja                       | Ja                             |  Enthaltene Elemente sind auf 100 beschränkt. _revinclude unter PaaS und OSS unter Cosmos DB enthält keine :iterate-Unterstützung [(#1313).](https://github.com/microsoft/fhir-server/issues/1313)  Problem [Nr. 1319](https://github.com/microsoft/fhir-server/issues/1319)                            |
| _summary                      | Ja             | Ja                   | Ja                        |                               |
| _total                        | Teilweise              | Teilweise                   | Teilweise                         | _total=none und _total=accurate                               |
| _sort                         | Teilweise              | Teilweise                   | Teilweise                         | sort=_lastUpdated wird unterstützt.                               |
| _contained                    | Nein                   | Nein                        | Nein                              |                                |
| _containedType                | Nein                   | Nein                        | Nein                              |                                |
| _score                        | Nein                   | Nein                        | Nein                              |                                |

Standardmäßig ist die Azure API for FHIR auf "lenient handling" festgelegt. Dies bedeutet, dass der Server alle unbekannten oder nicht unterstützten Parameter ignoriert. Wenn Sie eine strikte Behandlung verwenden möchten, können Sie den **Prefer-Header** verwenden und `handling=strict` festlegen.


 ## <a name="chained--reverse-chained-searching"></a>Verkettete & umgekehrte verkettete Suche

Mit einer [verketteten Suche](https://www.hl7.org/fhir/search.html#chaining) können Sie mithilfe eines Suchparameters nach einer Ressource suchen, auf die von einer anderen Ressource verwiesen wird. Wenn Sie z. B. Nach Treffern suchen möchten, bei denen der Name des Patienten Jane lautet, verwenden Sie Folgendes:

`GET {{FHIR URL}}/Encounter?subject:Patient.name=Jane`

Ebenso können Sie eine umgekehrte verkettete Suche durchführen. Dadurch können Sie Ressourcen abrufen, in denen Sie Kriterien für andere Ressourcen angeben, die auf sie verweisen. Weitere Beispiele für verkettete und umgekehrte Verkettungen finden Sie auf der Seite [mit FHIR-Suchbeispielen.](search-samples.md) 

**Hinweis:** In der Azure API for FHIR und der von Cosmos DB unterstützten Open Source-Abfrage gibt es eine Einschränkung, bei der jede Unterabfrage, die für die verketteten und umgekehrten verketteten Suchvorgänge erforderlich ist, nur 100 Elemente zurück gibt. Wenn mehr als 100 Elemente gefunden werden, erhalten Sie die folgende Fehlermeldung:

"Unterabfragen in einem verketteten Ausdruck können nicht mehr als 100 Ergebnisse zurückgeben. Verwenden Sie ein selektiveres Kriterium." 

Um eine erfolgreiche Abfrage zu erhalten, müssen Sie genauer darauf sein, was Sie suchen.

## <a name="pagination"></a>Paginierung

Wie bereits erwähnt, sind die Ergebnisse einer Suche ein seitenseitiges Bündel. Standardmäßig gibt die Suche 10 Ergebnisse pro Seite zurück. Dies kann jedoch durch Angabe von erhöht (oder verringert) `_count` werden. Innerhalb des Bündels gibt es einen Selbstlink, der das aktuelle Ergebnis der Suche enthält. Wenn es zusätzliche Übereinstimmungen gibt, enthält das Paket einen nächsten Link. Sie können den nächsten Link weiterhin verwenden, um die nachfolgenden Seiten der Ergebnisse zu erhalten. 

Derzeit unterstützt der Azure API for FHIR nur den nächsten Link in Paketen und keine ersten, letzten oder vorherigen Links.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mehr über die Grundlagen der Suche erfahren haben, finden Sie auf der Seite mit den Suchbeispielen Details zur Suche mit verschiedenen Suchparametern, Modifizierern und anderen FHIR-Suchtools.

>[!div class="nextstepaction"]
>[FHIR-Suchbeispiele](search-samples.md)
