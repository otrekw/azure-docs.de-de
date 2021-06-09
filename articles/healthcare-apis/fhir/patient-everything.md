---
title: Verwenden von patient-everything in Azure API for FHIR
description: In diesem Artikel wird erläutert, wie Sie den Patient-Everything-Vorgang in der Azure API for FHIR
services: healthcare-apis
author: caitlinv39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 06/04/2021
ms.author: cavoeg
ms.openlocfilehash: fee544c318df318e09c75ebaf18fc0837cb4af0d
ms.sourcegitcommit: 832e92d3b81435c0aeb3d4edbe8f2c1f0aa8a46d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2021
ms.locfileid: "111562739"
---
# <a name="patient-everything-in-fhir"></a>Patient-everything in FHIR

Der [$patient-everything-Vorgang](https://www.hl7.org/fhir/patient-operation-everything.html) wurde erstellt, um einem Patienten Zugriff auf den gesamten Datensatz zu ermöglichen oder einem Anbieter oder einem anderen Benutzer einen Massendownload von Daten zu ermöglichen. Dieser Vorgang wird verwendet, um alle Informationen im Zusammenhang mit einem oder mehr Patienten zurück, die in der Ressource oder im Kontext beschrieben werden, in der dieser Vorgang aufgerufen wird.  

## <a name="use-patient-everything"></a>Verwenden von patient-everything
Verwenden Sie zum Aufrufen von patient-everything den folgenden Befehl:

```json
GET {FHIRURL}/Patient/{ID}/$everything
```
Der Azure API for FHIR überprüft, ob der Patient mit der angegebenen Patienten-ID übereinstimmen kann. Wenn ein Ergebnis gefunden wird, ist die Antwort ein Paket vom Typ "searchset" mit den folgenden Informationen: 
* [Patient-Ressource](https://www.hl7.org/fhir/patient.html) 
*  Ressourcen, auf die direkt von der Patient-Ressource verwiesen wird (außer Link) 
*  Ressourcen im [Patientendepartment](https://www.hl7.org/fhir/compartmentdefinition-patient.html)
*  [Geräteressourcen,](https://www.hl7.org/fhir/device.html) die auf die Patient-Ressource verweisen  

 
> [!Note]
> $patient alles ist jetzt auf dem open source-FHIR-Server verfügbar, der von Cosmos DB unterstützt wird, und Azure API for FHIR 1. Juli verfügbar. Die Capability-Anweisung für den FHIR-Server fehlt die Unterstützung für $patient-everything, die hier nachverfolgt wird: Issue [1989](https://github.com/microsoft/fhir-server/issues/1989). 


## <a name="patient-everything-parameters"></a>Patient-everything-Parameter
Die Azure API for FHIR unterstützt die folgenden Abfrageparameter, die alle optional sind:

|Query parameter (Abfrageparameter)        |  BESCHREIBUNG|
|-----------------------|------------|
| \_type | Hier können Sie angeben, welche Ressourcentypen in die Antwort eingeschlossen werden. \_Type=Encounter würde beispielsweise nur Ressourcen zurückgeben, die dem Patienten zugeordnet `Encounter` sind. |
| \_since | Gibt nur Ressourcen zurück, die seit der angegebenen Zeit geändert wurden. |
| start | Wenn Sie das Startdatum angeben, werden Ressourcen mit dem Angegebenen Startdatum eingezogen. Wenn kein Startdatum angegeben wird, befinden sich alle Datensätze vor dem Enddatum im Gültigkeitsbereich. |
| end | Wenn Sie das Enddatum angeben, werden Ressourcen eingezogen, bei denen das datum der Studie vor dem angegebenen Enddatum liegt. Wenn kein Enddatum angegeben wird, befinden sich alle Datensätze nach dem Startdatum im Gültigkeitsbereich. |

> [!Note]
> Sie müssen eine ID für einen bestimmten Patienten angeben. Wenn Sie alle Daten für alle Patienten benötigen, lesen Sie [$export](export-data.md). 


## <a name="examples-of-patient-everything"></a>Beispiele für $patient alles 

Im Folgenden finden Sie einige zusätzliche Beispiele für die Verwendung $patient -alles-Vorgangs. 

Verwenden Sie $patient Aufruf, um "alles" eines Patienten zwischen 2010 und 2020 mithilfe von "alles" abfragen zu können: 

```json
GET {FHIRURL}/Patient/{ID}/$everything?start=2010&end=2020
``` 

Verwenden Sie $patient Aufruf, um die Beobachtung und die Beobachtung eines Patienten mithilfe von "alles" abfragen zu können: 
```json
GET {FHIRURL}/Patient/{ID}/$everything_type=Observation,Encounter 
```

Um "$patient-everything" zum Abfragen des "Alles" eines Patienten seit 2021-05-27T05:00:00Z zu verwenden, verwenden Sie den folgenden Aufruf: 

```json
GET {FHIRURL}/Patient/{ID}/$everything?_since=2021-05-27T05:00:00Z 
```

Wenn für jeden dieser Aufrufe ein Patient gefunden wird, erhalten Sie eine 200-Antwort mit einem Bündel der entsprechenden Ressourcen zurück.

## <a name="next-step"></a>Nächster Schritt
Nachdem Sie nun wissen, wie Der Patient-Everything-Vorgang verwendet wird, können Sie sich über weitere Suchoptionen in der Übersicht über den Suchleitfaden informieren.

>[!div class="nextstepaction"]
>[Übersicht über die FHIR-Suche](overview-of-search.md)