---
title: Übersicht über die Suche in Azure API for FHIR
description: In diesem Artikel wird eine Übersicht über die FHIR-Suche beschrieben, die in der Azure API for FHIR
author: stevewohl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 5/17/2021
ms.author: cavoeg
ms.openlocfilehash: 93468d2be1c9ab8fbdbce1e82a58f04f0676f260
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110071180"
---
# <a name="overview-of-fhir-search"></a>Übersicht über die FHIR-Suche

Die FHIR-Spezifikation definiert die Grundlagen der Suche nach FHIR-Ressourcen. Dieser Artikel führt Sie durch einige wichtige Aspekte bei der Suche nach Ressourcen in FHIR. Vollständige Informationen zum Durchsuchen von FHIR-Ressourcen finden Sie unter [Suchen](https://www.hl7.org/fhir/search.html) in der HL7 FHIR-Spezifikation. In diesem Artikel werden Beispiele für die Suchsyntax erläutert. Jede Suche wird für Ihren FHIR-Server ausgeführt, der in der Regel über die URL `https://<FHIRSERVERNAME>.azurewebsites.net` verfügt. In den Beispielen verwenden wir den Platzhalter {{FHIR_URL}} für diese URL. 

FHIR-Suchvorgänge können für einen bestimmten Ressourcentyp, ein [angegebenes](https://www.hl7.org/fhir/compartmentdefinition.html)Abteil oder alle Ressourcen durchgeführt werden. Die einfachste Möglichkeit zum Ausführen einer Suche in FHIR ist die Verwendung einer `GET` Anforderung. Wenn Sie beispielsweise alle Patienten in die Datenbank pullen möchten, können Sie die folgende Anforderung verwenden: 

```rest
GET {{FHIR_URL}}/Patient
```

Sie können auch mithilfe von `POST` suchen. Dies ist nützlich, wenn die Abfragezeichenfolge zu lang ist. Für die Suche mit `POST` können die Suchparameter als Formularkörper übermittelt werden. Dies ermöglicht eine längere, komplexere Reihe von Abfrageparametern, die in einer Abfragezeichenfolge schwer zu erkennen und zu verstehen sind.

Wenn die Suchanforderung erfolgreich ist, erhalten Sie eine FHIR-Bündelantwort vom Typ `searchset` . Wenn bei der Suche ein Fehler auftritt, finden Sie die Fehlerdetails im , damit Sie nachvollziehen können, warum `OperationOutcome` die Suche fehlgeschlagen ist.

In den folgenden Abschnitten werden die verschiedenen Aspekte der Suche erläutert. Nachdem Sie diese Details überprüft haben, finden Sie auf unserer Seite mit Beispielen Beispiele für Suchvorgänge, die Sie in der Azure API for FHIR. [](search-samples.md)

## <a name="search-parameters"></a>Suchparameter

Bei einer Suche suchen Sie basierend auf verschiedenen Attributen der Ressource. Diese Attribute werden als Suchparameter bezeichnet. Jede Ressource verfügt über einen Satz definierter Suchparameter. Der Suchparameter muss definiert und in der Datenbank indiziert werden, damit Sie erfolgreich danach suchen können.

Jeder Suchparameter verfügt über einen definierten [Datentyp.](https://www.hl7.org/fhir/search.html#ptypes) Die Unterstützung für die verschiedenen Datentypen wird unten beschrieben:


| **Suchparametertyp**  | **Unterstützt: PaaS** | **Unterstützt: OSS (SQL)** | **Unterstützt: OSS (Cosmos DB)** | **Kommentar**|
| -------------------------  | -------------------- | ------------------------- | ------------------------------- |------------|
|  number                    | Ja                  | Ja                       | Ja                             |
|  date                      | Ja                  | Ja                       | Ja                             |
|  Zeichenfolge                    | Ja                  | Ja                       | Ja                             |
|  token                     | Ja                  | Ja                       | Ja                             |
|  Referenz                 | Ja                  | Ja                       | Ja                             |
|  Zusammengesetzt                 | Partial              | Partial                   | Partial                         | Die Liste der unterstützten zusammengesetzten Typen wird weiter unten in diesem Artikel beschrieben. |
|  quantity                  | Ja                  | Ja                       | Ja                             |
|  uri                       | Ja                  | Ja                       | Ja                             |
|  speziell                   | Nein                   | Nein                        | Nein                              |

### <a name="common-search-parameters"></a>Allgemeine Suchparameter

Es gibt [allgemeine Suchparameter,](https://www.hl7.org/fhir/search.html#all) die für alle Ressourcen gelten. Diese werden unten zusammen mit ihrer Unterstützung innerhalb der Azure API for FHIR aufgeführt:

| **Allgemeiner Suchparameter** | **Unterstützt: PaaS** | **Unterstützt: OSS (SQL)** | **Unterstützt: OSS (Cosmos DB)** | **Kommentar** |
| --------------------------  | -------------------- | ------------------------- | ------------------------------- | ------------|
| _id                         | Ja                  | Ja                       | Ja                             |             |
| _lastUpdated                | Ja                  | Ja                       | Ja                             |             |
| _tag                        | Ja                  | Ja                       | Ja                             |             | 
| _type                       | Ja                  | Ja                       | Ja                             |             |
| _security                   | Ja                  | Ja                       | Ja                             |             |
| _profile                    | Ja                  | Ja                       | Ja                             | Wenn Sie Ihre R4-Datenbank vor dem 20. Februar 2021 erstellt haben, müssen Sie einen [Neuindizierungsauftrag](how-to-run-a-reindex.md) ausführen, um **_profile** zu aktivieren.|
| _has                        | Partial              | Ja                       | Teilweise                         | Die Unterstützung für _has befindet sich im MVP im Azure API for FHIR und der OSS-Version, die von Cosmos DB unterstützt wird. Weitere Informationen finden Sie unten im Verkettungsabschnitt. |
| _query                      | Nein                   | Nein                        | Nein                              |             |
| _filter                     | Nein                   | Nein                        | Nein                              |             |
| _list                       | Nein                   | Nein                        | Nein                              |             |
| _text                       | Nein                   | Nein                        | Nein                              |             |
| _content                    | Nein                   | Nein                        | Nein                              |             |

### <a name="resource-specific-parameters"></a>Ressourcenspezifische Parameter

Mit dem Azure API for FHIR unterstützen wir fast alle [ressourcenspezifischen Suchparameter,](https://www.hl7.org/fhir/searchparameter-registry.html) die durch die FHIR-Spezifikation definiert sind. Die einzigen Suchparameter, die wir nicht unterstützen, sind unter den folgenden Links verfügbar:

* [NICHT unterstützte Suchparameter fürHEIT3](https://github.com/microsoft/fhir-server/blob/main/src/Microsoft.Health.Fhir.Core/Data/Stu3/unsupported-search-parameters.json)

* [Nicht unterstützte R4-Suchparameter](https://github.com/microsoft/fhir-server/blob/main/src/Microsoft.Health.Fhir.Core/Data/R4/unsupported-search-parameters.json)

Sie können auch die aktuelle Unterstützung für Suchparameter in der [FHIR Capability Statement mit](https://www.hl7.org/fhir/capabilitystatement.html) der folgenden Anforderung sehen:

```rest
GET {{FHIR_URL}}/metadata
```

Um die Suchparameter in der Capability-Anweisung zu sehen, navigieren Sie zu , um die Suchparameter für jede Ressource und die Suchparameter für `CapabilityStatement.rest.resource.searchParam` `CapabilityStatement.rest.searchParam` alle Ressourcen zu suchen.

> [!NOTE]
> Die Azure API for FHIR erstellt oder indiziert nicht automatisch Suchparameter, die nicht durch die FHIR-Spezifikation definiert sind. Wir unterstützen Sie jedoch bei der Definition Ihrer eigenen [Suchparameter.](how-to-do-custom-search.md)

### <a name="composite-search-parameters"></a>Zusammengesetzte Suchparameter
Mit der zusammengesetzten Suche können Sie nach Wertpaaren suchen. Wenn Sie beispielsweise nach einer Höhenerkennung suchen, bei der die Person 60 Zoll beträgt, möchten  Sie sicherstellen, dass eine einzelne Komponente der Beobachtung den Höhencode und den Wert von 60 enthielt. Sie möchten keine Beobachtung erhalten, bei der eine Gewichtung von 60 und eine Höhe von 48 gespeichert wurde, obwohl die Beobachtung Einträge mit einem Wert von 60 und einem Höhencode nur in verschiedenen Komponentenabschnitten enthalten würde. 

Mit dem Azure API for FHIR unterstützen wir die folgenden Suchparametertyppaare:

* Verweis, Token
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
|  :above (URI) | Ja                  | Ja                       | Ja                             | 
|  :in (Token)  | Nein                   | Nein                        | Nein                              | 
|  :below (Token) | Nein                 | Nein                        | Nein                              | 
|  :above (Token) | Nein                 | Nein                        | Nein                              | 
|  :not-in (Token) | Nein                | Nein                        | Nein                              | 

Für Suchparameter mit einer bestimmten Reihenfolge (Zahlen, Datumsangaben und Mengen) können Sie ein [Präfix](https://www.hl7.org/fhir/search.html#prefix) für den Parameter verwenden, um Übereinstimmungen zu finden. Die Azure API for FHIR unterstützt alle Präfixe.

 ### <a name="search-result-parameters"></a>Suchergebnisparameter
Um die zurückgegebenen Ressourcen zu verwalten, gibt es Suchergebnisparameter, die Sie in Ihrer Suche verwenden können. Weitere Informationen zur Verwendung der einzelnen Suchergebnisparameter finden Sie auf der [HL7-Website.](https://www.hl7.org/fhir/search.html#return) 

| **Suchergebnisparameter**  | **Unterstützt: PaaS** | **Unterstützt: OSS (SQL)** | **Unterstützt: OSS (Cosmos DB)** | **Kommentare**                 |
| ----------------------------  | -------------------- | ------------------------- | ------------------------------- | -----------------------------|
| _elements                     | Ja                  | Ja                       | Ja                             |                                |
| _count                        | Ja                  | Ja                       | Ja                             | _count ist auf 1.000 Ressourcen beschränkt. Wenn er höher als 1000 festgelegt ist, werden nur 1.000 zurückgegeben, und im Paket wird eine Warnung zurückgegeben.                               |
| _include                      | Ja                  | Ja                       | Ja                             | Enthaltene Elemente sind auf 100 beschränkt. _include unter PaaS und OSS auf Cosmos DB enthalten keine :iterate-Unterstützung [(#1313).](https://github.com/microsoft/fhir-server/issues/1313)                               |
| _revinclude                   | Ja                  | Ja                       | Ja                             |  Enthaltene Elemente sind auf 100 beschränkt. _revinclude unter PaaS und OSS auf Cosmos DB enthalten keine :iterate-Unterstützung [(#1313).](https://github.com/microsoft/fhir-server/issues/1313)  Problem [Nr. 1319](https://github.com/microsoft/fhir-server/issues/1319)                            |
| _summary                      | Ja             | Ja                   | Ja                        |                               |
| _total                        | Partial              | Partial                   | Partial                         | _total=none und _total=accurate                               |
| _sort                         | Partial              | Partial                   | Partial                         | sort=_lastUpdated wird unterstützt. Für Azure API for FHIR- und OSS Cosmos DB-Datenbanken, die nach dem 20. April 2021 erstellt wurden, wird die Sortierung auch nach Vor- und Nachname sowie nach dem datumsgemäßen Zeitpunkt unterstützt.                               |
| _contained                    | Nein                   | Nein                        | Nein                              |                                |
| _containedType                | Nein                   | Nein                        | Nein                              |                                |
| _score                        | Nein                   | Nein                        | Nein                              |                                |

Standardmäßig ist die Azure API for FHIR auf lenient handling (lenient handling) festgelegt. Dies bedeutet, dass der Server alle unbekannten oder nicht unterstützten Parameter ignoriert. Wenn Sie eine strikte Behandlung verwenden möchten, können Sie den **Prefer-Header** verwenden und `handling=strict` festlegen.

 ## <a name="chained--reverse-chained-searching"></a>Verkettete & umgekehrte verkettete Suche

Mit einer [verketteten Suche](https://www.hl7.org/fhir/search.html#chaining) können Sie mithilfe eines Suchparameters nach einer Ressource suchen, auf die von einer anderen Ressource verwiesen wird. Wenn Sie z. B. Treffer finden möchten, bei denen der Name des Patienten Jane lautet, verwenden Sie Folgendes:

`GET {{FHIR_URL}}/Encounter?subject:Patient.name=Jane`

Ebenso können Sie eine umgekehrte verkettete Suche durchführen. Dadurch können Sie Ressourcen abrufen, in denen Sie Kriterien für andere Ressourcen angeben, die auf sie verweisen. Weitere Beispiele für verkettete und umgekehrt verkettete Suche finden Sie auf der Seite [mit FHIR-Suchbeispielen.](search-samples.md) 

> [!NOTE]
> In der Azure API for FHIR und der Open Source, die von Cosmos DB unterstützt wird, gibt es eine Einschränkung, dass jede für die verkettete und umgekehrt verkettete Suche erforderliche Unterabfrage nur 100 Elemente zurückgibt. Wenn mehr als 100 Elemente gefunden werden, erhalten Sie die folgende Fehlermeldung: "Unterabfragen in einem verketteten Ausdruck können nicht mehr als 100 Ergebnisse zurückgeben, verwenden Sie ein selektiveres Kriterium." Um eine erfolgreiche Abfrage zu erhalten, müssen Sie spezifischer sein, was Sie suchen.

## <a name="pagination"></a>Paginierung

Wie bereits erwähnt, sind die Ergebnisse einer Suche ein ausgelagerunges Bündel. Standardmäßig gibt die Suche 10 Ergebnisse pro Seite zurück, dies kann jedoch durch Angabe von erhöht (oder verringert) `_count` werden. Innerhalb des Pakets gibt es einen Selbstlink, der das aktuelle Ergebnis der Suche enthält. Wenn zusätzliche Übereinstimmungen vorhanden sind, enthält das Paket einen nächsten Link. Sie können weiterhin den nächsten Link verwenden, um die nachfolgenden Ergebnisseiten abzurufen. `_count` ist auf maximal 1.000 Elemente beschränkt. 

Derzeit unterstützt der Azure API for FHIR nur den nächsten Link in Paketen und keine ersten, letzten oder vorherigen Links.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit den Grundlagen der Suche beschäftigt haben, finden Sie auf der Seite mit den Suchbeispielen Details zur Suche mit verschiedenen Suchparametern, Modifizierern und anderen FHIR-Suchszenarien.

>[!div class="nextstepaction"]
>[FHIR-Suchbeispiele](search-samples.md)
