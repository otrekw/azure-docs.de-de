---
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/28/2020
ms.author: normesta
ms.openlocfilehash: b2e753a3f9741856dd8b81755912ad7bd78b5557
ms.sourcegitcommit: 19dce034650c654b656f44aab44de0c7a8bd7efe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2020
ms.locfileid: "91710987"
---
Azure Storage stellt in Azure Monitor folgende Transaktionsmetriken bereit:

| Metrik | BESCHREIBUNG |
| ------------------- | ----------------- |
| Transaktionen | Die Anzahl von Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Diese Anzahl umfasst erfolgreiche und nicht erfolgreiche Anforderungen sowie Anforderungen, die zu Fehlern geführt haben. <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Gesamt <br/> Verfügbare Dimensionen: ResponseType, GeoType, ApiName, Authentication ([Definition](#metrics-dimensions))<br/> Beispielwert: 1024 |
| Eingehende Daten | Die eingehende Datenmenge. Dieser Wert umfasst an Azure Storage gerichtete eingehende Daten von einem externen Client sowie eingehende Daten innerhalb von Azure. <br/><br/> Einheit: Byte <br/> Aggregationstyp: Gesamt <br/> Verfügbare Dimensionen: GeoType, ApiName, Authentication ([Definition](#metrics-dimensions)) <br/> Beispielwert: 1024 |
| Ausgehende Daten | Die ausgehende Datenmenge. Dieser Wert umfasst an Azure Storage gerichtete ausgehende Daten von einem externen Client sowie ausgehende Daten innerhalb von Azure. Der Wert stellt somit keine gebührenpflichtigen ausgehenden Daten dar. <br/><br/> Einheit: Byte <br/> Aggregationstyp: Gesamt <br/> Verfügbare Dimensionen: GeoType, ApiName, Authentication ([Definition](#metrics-dimensions)) <br/> Beispielwert: 1024 |
| SuccessServerLatency | Die durchschnittliche Verarbeitungszeit einer erfolgreichen Anforderung durch Azure Storage. Dieser Wert beinhaltet nicht die in „SuccessE2ELatency“ angegebene Netzwerklatenz. <br/><br/> Einheit: Millisekunden <br/> Aggregationstyp: Average <br/> Verfügbare Dimensionen: GeoType, ApiName, Authentication ([Definition](#metrics-dimensions)) <br/> Beispielwert: 1024 |
| SuccessE2ELatency | Die durchschnittliche End-to-End-Wartezeit (in Millisekunden) bei erfolgreichen Anforderungen, die an einen Speicherdienst oder an den angegebenen API-Vorgang gerichtet wurden. Dieser Wert umfasst die erforderliche Verarbeitungszeit in Azure Storage für das Lesen der Anforderung, das Senden der Antwort und das Empfangen der Antwortbestätigung. <br/><br/> Einheit: Millisekunden <br/> Aggregationstyp: Average <br/> Verfügbare Dimensionen: GeoType, ApiName, Authentication ([Definition](#metrics-dimensions)) <br/> Beispielwert: 1024 |
| Verfügbarkeit | Die Verfügbarkeit (in Prozent) für den Speicherdienst oder den angegebenen API-Vorgang. Zur Berechnung der Verfügbarkeit wird der Wert der gesamten gebührenpflichtigen Anforderungen durch die Anzahl entsprechender Anforderungen geteilt. Dabei werden auch Anforderungen einbezogen, die zu unerwarteten Fehlern geführt haben. Alle unerwarteten Fehler verringern die Verfügbarkeit für den Speicherdienst oder den angegebenen API-Vorgang. <br/><br/> Einheit: Percent <br/> Aggregationstyp: Average <br/> Verfügbare Dimensionen: GeoType, ApiName, Authentication ([Definition](#metrics-dimensions)) <br/> Beispielwert: 99,99 |