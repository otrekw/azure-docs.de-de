---
title: How to do custom search in Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie Sie eigene benutzerdefinierte Suchparameter definieren können, die in der Datenbank verwendet werden sollen.
author: stevewohl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 05/03/2021
ms.author: cavoeg
ms.openlocfilehash: d61e886771fb3bc667ff9e219de736c03e591ba7
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108765195"
---
# <a name="defining-custom-search-parameters"></a>Definieren von benutzerdefinierten Suchparametern

Die FHIR-Spezifikation definiert einen Satz von Suchparametern für alle Ressourcen und Suchparameter, die für eine Ressource(n) spezifisch sind. Es gibt jedoch Szenarien, in denen Sie nach einem Element in einer Ressource suchen möchten, das nicht von der FHIR-Spezifikation als Standardsuchparameter definiert ist. In diesem Artikel wird beschrieben, wie Sie ihre eigenen [Suchparameter](https://www.hl7.org/fhir/searchparameter.html) definieren können, die in der -Azure API for FHIR.

> [!NOTE]
> Jedes Mal, wenn Sie einen Suchparameter erstellen, aktualisieren [](how-to-run-a-reindex.md) oder löschen, müssen Sie einen Auftrag zum erneuten Indizieren ausführen, damit der Suchparameter in der Produktion verwendet werden kann. Im Folgenden wird beschrieben, wie Sie Suchparameter testen können, bevor Sie den gesamten FHIR-Server neu indizieren.

## <a name="create-new-search-parameter"></a>Erstellen eines neuen Suchparameters

Um einen neuen Suchparameter zu erstellen, verwenden `POST` Sie die Ressource für die `SearchParameter` Datenbank. Im folgenden Codebeispiel wird gezeigt, wie sie der Ressource [den Suchparameter "US Core Race"](http://hl7.org/fhir/us/core/STU3.1.1/SearchParameter-us-core-race.html) `Patient` hinzufügen.

```rest
POST {{FHIR_URL}}/SearchParameter

{
  "resourceType" : "SearchParameter",
  "id" : "us-core-race",
  "url" : "http://hl7.org/fhir/us/core/SearchParameter/us-core-race",
  "version" : "3.1.1",
  "name" : "USCoreRace",
  "status" : "active",
  "date" : "2019-05-21",
  "publisher" : "US Realm Steering Committee",
  "contact" : [
    {
      "telecom" : [
        {
          "system" : "other",
          "value" : "http://www.healthit.gov/"
        }
      ]
    }
  ],
  "description" : "Returns patients with a race extension matching the specified code.",
  "jurisdiction" : [
    {
      "coding" : [
        {
          "system" : "urn:iso:std:iso:3166",
          "code" : "US",
          "display" : "United States of America"
        }
      ]
    }
  ],
  "code" : "race",
  "base" : [
    "Patient"
  ],
  "type" : "token",
  "expression" : "Patient.extension.where(url = 'http://hl7.org/fhir/us/core/StructureDefinition/us-core-race').extension.value.code"
}

``` 

> [!NOTE]
> Der neue Suchparameter wird in der Capability-Anweisung des FHIR-Servers angezeigt, nachdem Sie den Suchparameter an die Datenbank gesendet und **Ihre** Datenbank neu indiziert haben. Das Anzeigen von in der Capability-Anweisung ist die einzige Möglichkeit, um zu erkennen, ob ein `SearchParameter` Suchparameter auf Ihrem FHIR-Server unterstützt wird. Wenn Sie den Suchparameter finden können, indem Sie nach dem Suchparameter suchen, ihn aber nicht in der Capability-Anweisung sehen, müssen Sie den Suchparameter trotzdem indizieren. Sie können mehrere Suchparameter PER POST veröffentlichen, bevor Sie einen Neuindizierungsvorgang auslösen.

Wichtige Elemente eines `SearchParameter` :

* **url:** Ein eindeutiger Schlüssel zum Beschreiben des Suchparameters. Viele Organisationen, z. B. HL7, verwenden ein STANDARD-URL-Format für die suchparameter, die sie definieren, wie oben im Suchparameter FÜR DIE KERN-Racesuche gezeigt.

* **code:** Der im Code **gespeicherte** Wert wird bei der Suche verwendet. Im obigen Beispiel würden Sie mit suchen, `GET {FHIR_URL}/Patient?race=<code>` um alle Patienten einer bestimmten Race zu erhalten. Der Code muss für die Ressourcen eindeutig sein, für die der Suchparameter gilt.

* **base:** Beschreibt, für welche Ressourcen der Suchparameter gilt. Wenn der Suchparameter für alle Ressourcen gilt, können Sie `Resource` verwenden. Andernfalls können Sie alle relevanten Ressourcen auflisten.
 
* **type:** Beschreibt den Datentyp für den Suchparameter. Der Typ ist durch die Unterstützung für die Azure API for FHIR beschränkt. Dies bedeutet, dass Sie keinen Suchparameter vom Typ Special oder einen [zusammengesetzten Suchparameter](overview-of-search.md) definieren können, es sei denn, es handelt sich um eine unterstützte Kombination.

* **expression:** Beschreibt, wie der Wert für die Suche berechnet wird. Wenn Sie einen Suchparameter beschreiben, müssen Sie den Ausdruck einschließen, auch wenn er für die Spezifikation nicht erforderlich ist. Dies liegt daran, dass Sie entweder den Ausdruck oder die xpath-Syntax benötigen und die Azure API for FHIR die xpath-Syntax ignoriert.

## <a name="test-search-parameters"></a>Testsuchparameter

Obwohl Sie die Suchparameter erst in der Produktion verwenden können, wenn Sie einen Neuindizierungsauftrag ausführen, gibt es einige Möglichkeiten, Ihre Suchparameter zu testen, bevor Sie die gesamte Datenbank neu indizieren. 

Zunächst können Sie ihren neuen Suchparameter testen, um zu sehen, welche Werte zurückgegeben werden. Wenn Sie den folgenden Befehl für eine bestimmte Ressourceninstanz ausführen (durch Eingabe ihrer ID), erhalten Sie eine Liste von Wertpaaren mit dem Namen des Suchparameters und dem für den spezifischen Patienten gespeicherten Wert. Dies schließt alle Suchparameter für die Ressource ein, und Sie können durchblättern, um den von Ihnen erstellten Suchparameter zu finden. Wenn Sie diesen Befehl ausführen, ändert sich kein Verhalten auf Dem FHIR-Server. 

```rest
GET https://{{FHIR_URL}}/{{RESOURCE}}/{{RESOUCE_ID}}/$reindex

```
So suchen Sie beispielsweise nach allen Suchparametern für einen Patienten:

```rest
GET https://{{FHIR_URL}}/Patient/{{PATIENT_ID}}/$reindex

```

Das Ergebnis sieht so aus:

```json
{
  "resourceType": "Parameters",
  "id": "8be24e78-b333-49da-a861-523491c3437a",
  "meta": {
    "versionId": "1"
  },
  "parameter": [
    {
      "name": "deceased",
      "valueString": "http://hl7.org/fhir/special-values|false"
    },
    {
      "name": "language",
      "valueString": "urn:ietf:bcp:47|en-US"
    },
    {
      "name": "race",
      "valueString": "2028-9"
    },
...
```
Sobald Sie sehen, dass der Suchparameter wie erwartet angezeigt wird, können Sie eine einzelne Ressource neu indizieren, um die Suche mit dem -Element zu testen. Zunächst werden Sie eine einzelne Ressource neu indizieren:

```rest
POST https://{{FHIR_URL}/{{RESOURCE}}/{{RESOURCE_ID}}/$reindex
```

Durch Ausführen dieses Parameters werden die Indizes für alle Suchparameter für die jeweilige Ressource festgelegt, die Sie für diesen Ressourcentyp definiert haben. Dadurch wird ein Update für den FHIR-Server ausgeführt. Nun können Sie den Header "Use partial indices" suchen und auf TRUE festlegen. Dies bedeutet, dass Er Ergebnisse zurückgibt, bei denen eine der Ressourcen den Suchparameter indiziert hat, auch wenn er nicht für alle Ressourcen dieses Typs indiziert ist. 

Wenn Sie mit dem obigen Beispiel fortfahren, könnten Sie einen Patienten indizieren, um das US Core Race zu `SearchParameter` aktivieren:

```rest
POST https://{{FHIR_URL}/Patient/{{PATIENT_ID}}/$reindex
```

Suchen Sie dann nach Patienten, die eine bestimmte Race haben:

```rest
GET https://{{FHIR_URL}}/Patient?race=2028-9
x-ms-use-partial-indices: true
```

Nachdem Sie getestet haben und sich davon überzeugt haben, dass Ihr Suchparameter wie erwartet funktioniert, führen Sie den Auftrag für die Neuindizierung aus, oder planen Sie ihn, damit die Suchparameter auf dem FHIR-Server für Anwendungsfälle in der Produktion verwendet werden können.

## <a name="update-a-search-parameter"></a>Aktualisieren eines Suchparameters

Um einen Suchparameter zu aktualisieren, verwenden Sie , `PUT` um eine neue Version des Suchparameters zu erstellen. Sie müssen das `SearchParameter ID` in das `id` -Element des Anforderungskörpers und `PUT` in den -Aufruf ein `PUT` schließen.

> [!NOTE]
> Wenn Sie die ID für Ihren Suchparameter nicht kennen, können Sie nach ihr suchen. Wenn Sie verwenden, werden alle benutzerdefinierten Suchparameter zurückgeben, und Sie können durch den Suchparameter scrollen, `GET {{FHIR_URL}}/SearchParameter` um den benötigten Suchparameter zu finden. Sie können die Suche auch nach Namen einschränken. Im folgenden Beispiel können Sie mit nach dem Namen `USCoreRace: GET {{FHIR_URL}}/SearchParameter?name=USCoreRace` suchen.

```rest
PUT {{FHIR_ULR}}/SearchParameter/{SearchParameter ID}

{
  "resourceType" : "SearchParameter",
  "id" : "SearchParameter ID",
  "url" : "http://hl7.org/fhir/us/core/SearchParameter/us-core-race",
  "version" : "3.1.1",
  "name" : "USCoreRace",
  "status" : "active",
  "date" : "2019-05-21",
  "publisher" : "US Realm Steering Committee",
  "contact" : [
    {
      "telecom" : [
        {
          "system" : "other",
          "value" : "http://www.healthit.gov/"
        }
      ]
    }
  ],
  "description" : "New Description!",
  "jurisdiction" : [
    {
      "coding" : [
        {
          "system" : "urn:iso:std:iso:3166",
          "code" : "US",
          "display" : "United States of America"
        }
      ]
    }
  ],
  "code" : "race",
  "base" : [
    "Patient"
  ],
  "type" : "token",
  "expression" : "Patient.extension.where(url = 'http://hl7.org/fhir/us/core/StructureDefinition/us-core-race').extension.value.code"
}

```

Das Ergebnis wird aktualisiert, `SearchParameter` und die Version wird erhöht.

> [!Warning]
> Seien Sie vorsichtig, wenn Sie SearchParameters aktualisieren, die bereits in Ihrer Datenbank indiziert wurden. Das Ändern des Verhaltens eines vorhandenen SearchParameters kann Auswirkungen auf das erwartete Verhalten haben. Es wird empfohlen, einen Auftrag sofort neu zu indizieren.

## <a name="delete-a-search-parameter"></a>Löschen eines Suchparameters

Wenn Sie einen Suchparameter löschen müssen, verwenden Sie Folgendes:

```rest
Delete {{FHIR_URL}}/SearchParameter/{SearchParameter ID}
```

> [!Warning]
> Seien Sie vorsichtig, wenn Sie SearchParameters löschen, die bereits in Ihrer Datenbank indiziert wurden. Das Ändern des Verhaltens eines vorhandenen SearchParameters kann Auswirkungen auf das erwartete Verhalten haben. Es wird empfohlen, einen Auftrag sofort neu zu indizieren.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie einen Suchparameter erstellen. Als Nächstes erfahren Sie, wie Sie Ihren FHIR-Server neu indizieren.

>[!div class="nextstepaction"]
>[Ausführen eines Auftrags zum Neuindizieren](how-to-run-a-reindex.md)