---
title: Durchführen der benutzerdefinierten Suche in Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie Sie ihre eigenen benutzerdefinierten Suchparameter definieren können, die in der Datenbank verwendet werden sollen.
author: stevewohl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 4/23/2021
ms.author: cavoeg
ms.openlocfilehash: 1482bd7f054c75c4f26b77907d50a6471c389d68
ms.sourcegitcommit: 52491b361b1cd51c4785c91e6f4acb2f3c76f0d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108322534"
---
# <a name="defining-custom-search-parameters"></a>Definieren benutzerdefinierter Suchparameter

Die FHIR-Spezifikation definiert einen Satz von Suchparametern für alle Ressourcen und Suchparameter, die spezifisch für eine(n) Ressource(n) sind. Es gibt jedoch Szenarien, in denen Sie möglicherweise nach einem Feld in einer Ressource suchen möchten, die nicht durch die Spezifikation als Standardsuchparameter definiert ist. In diesem Artikel wird beschrieben, wie Sie Ihre eigenen [Suchparameter](https://www.hl7.org/fhir/searchparameter.html) definieren können, die auf dem FHIR-Server verwendet werden sollen.

> [!NOTE]
> Jedes Mal, wenn Sie einen Suchparameter erstellen, aktualisieren oder löschen, müssen Sie einen [Neuindizierungsauftrag](how-to-run-a-reindex.md) ausführen, um die Änderung in Ihrer Datenbank zu aktivieren.

> [!Warning]
> Wenn Sie einen Suchparameter aktualisieren oder löschen, stellen Sie sicher, dass Sie sofort einen [Neuindizierungsauftrag](how-to-run-a-reindex.md)ausführen. Es besteht die Möglichkeit, dass sich Ihre Datenbank in einem ungewöhnlichen Zustand mit aktualisierten oder gelöschten Suchparametern befindet, die weiterhin aktiv erscheinen, da sie für die Änderungen indiziert werden müssen. 

## <a name="create-new-search-parameter"></a>Erstellen eines neuen Suchparameters

Um einen neuen Suchparameter zu erstellen, verwenden Sie `POST` einen neuen Suchparameter für die Datenbank. Das folgende Codebeispiel zeigt, wie der [Suchparameter "US Core Race"](http://hl7.org/fhir/us/core/STU3.1.1/SearchParameter-us-core-race.html) zur Ressource Patient hinzugefügt wird.

```json
POST {fhirurl}/SearchParameter
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
> Der neue Suchparameter wird in Ihrer Capability-Anweisung angezeigt, nachdem Sie ihn an den FHIR-Server gesendet und Ihre Datenbank neu indiziert haben. Sie kann erst verwendet werden, wenn Sie einen [Neuindizierungsauftrag](how-to-run-a-reindex.md)ausführen. Dies ist derzeit die einzige Möglichkeit, um zu ermitteln, ob ein Suchparameter in Ihrer Datenbank unterstützt wird oder nicht. Wenn Sie den Suchparameter finden können, indem Sie nach dem Suchparameter suchen, ihn aber nicht in der capability-Anweisung sehen, führen Sie einen Neuindizierungsauftrag aus. Sie können mehrere Suchparameter posten, bevor Sie einen Neuindizierungsvorgang auslösen.

Wichtige Feldbeschreibungen:

* **url:** Dies ist ein eindeutiger Schlüssel zum Beschreiben des Suchparameters. Viele Organisationen, z. B. HL7, verwenden ein Standardformat für die von ihnen definierten URLs, wie oben im Parameter für die US Core-Racesuche gezeigt.

* **code:** Der hier enthaltene Wert wird bei der Suche verwendet. Im obigen Beispiel würden Sie mit suchen, `GET {FHIR URL}/Patient?race=2028-9` um alle asiatischen Patienten zu erhalten. Dieser Wert muss für die Ressource(n) eindeutig sein, für die er gilt.

* **base:** Hier wird beschrieben, für welche Ressource(n) der Suchparameter gilt. Wenn dies für alle Ressourcen gilt, können Sie einfach Resource verwenden. Andernfalls können Sie alle relevanten Ressourcen auflisten.
 
* **type:** Beschreibt den Datentyp für den Suchparameter.

* **expression:** Wenn Sie einen Suchparameter beschreiben, müssen Sie den Ausdruck angeben, obwohl er technisch gesehen nicht für die Spezifikation erforderlich ist. Dies liegt daran, dass Sie entweder den Ausdruck oder die xpath-Syntax benötigen und Azure API for FHIR xpath-Syntax derzeit ignoriert. Hier wird beschrieben, wie sie den Wert für die Suche finden. 

> [!NOTE]
> "Typ" wird durch die Unterstützung für die Azure API for FHIR. Dies bedeutet, dass Sie keinen Suchparameter [](overview-of-search.md) vom Typ Special oder einen zusammengesetzten Suchparameter definieren können, es sei denn, er ist von einem Typ, den wir unterstützen.

Nachdem Sie Ihre Suchparameter hinzugefügt haben, führen Sie den Auftrag für die Neuindizierung aus, oder planen Sie ihn, damit die Suchparameter auf dem FHIR-Server verwendet werden können.

## <a name="update-a-search-parameter"></a>Aktualisieren eines Suchparameters

Um einen Suchparameter zu aktualisieren, verwenden Sie , `PUT` um eine neue Version des Suchparameters zu erstellen.

`PUT {fhirurl}/SearchParameter/{SearchParameter ID}`

Sie müssen den `SearchParameter ID` im ID-Feld des Anforderungskörpers `PUT` und im Aufruf `PUT` enthalten.

> [!NOTE]
> Wenn Sie die ID für Ihren Suchparameter nicht kennen, können Sie nach ihr suchen. Wenn Sie verwenden, werden alle benutzerdefinierten Suchparameter zurückgeben, und Sie können durch den Suchparameter scrollen, `GET {fhirurl}/SearchParameter` um den benötigten Suchparameter zu finden. Sie können die Suche auch nach Namen einschränken. Im folgenden Beispiel können Sie mit nach dem Namen `USCoreRace: GET {fhirurl}/SearchParameter?name=USCoreRace` suchen.

```json
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
> Seien Sie vorsichtig, wenn Sie SearchParameters aktualisieren, die bereits in Ihrer Datenbank indiziert wurden. Das Ändern des Verhaltens eines vorhandenen SearchParameter kann Auswirkungen auf das erwartete Verhalten haben. 

## <a name="delete-a-search-parameter"></a>Löschen eines Suchparameters

Wenn Sie einen Suchparameter löschen müssen, verwenden Sie Folgendes:

`Delete {fhirurl}/SearchParameter/{SearchParameter ID}`

> [!Warning]
> Seien Sie vorsichtig, wenn Sie SearchParameters löschen, die bereits in Ihrer Datenbank indiziert wurden. Das Ändern des Verhaltens eines vorhandenen SearchParameter kann Auswirkungen auf das erwartete Verhalten haben.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie einen Suchparameter erstellen. Informationen zum Erneuten Indizieren eines Auftrags finden Sie unter

>[!div class="nextstepaction"]
>[Ausführen eines Neuindizierungsauftrags](how-to-run-a-reindex.md)