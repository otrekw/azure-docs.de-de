---
title: Attribute der Konferenzinstanzentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Konferenzinstanzentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: darrine
manager: kuansanw
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: c6761206a58724dae96d9dc6f6234658892d4d18
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74146559"
---
# <a name="conference-instance-entity"></a>Konferenzinstanzentität

> [!NOTE]
> Die folgenden Attribute sind spezifisch für die Konferenzinstanzentität. (Ty = '4')

NAME | BESCHREIBUNG | type | Vorgänge
--- | --- | --- | ---
CC      |Gesamtzitatanzahl der Konferenzinstanz           |Int32      |Keine  
CD.D    |Datum des Konferenzinstanzereignisses    |Date       |Equals, IsBetween
CD.T    |Titel des Konferenzinstanzereignisses   |Date       |Equals, IsBetween
CIARD   |Fälligkeitsdatum für die Abstraktregistrierung der Konferenzinstanz  |Date       |Equals, IsBetween
CIED    |Enddatum der Konferenzinstanz    |Date       |Equals, IsBetween
CIFVD   |Fälligkeitsdatum der endgültigen Version der Konferenzinstanz  |Date       |Equals, IsBetween
CIL     |Speicherort der Konferenzinstanz    |Zeichenfolge     |Equals, StartsWith
CIN     |Normalisierter Name der Konferenzinstanz |Zeichenfolge        |Equals
CINDD   |Benachrichtigungsdatum der Konferenzinstanz   |Date       |Equals, IsBetween
CISD    |Startdatum der Konferenzinstanz  |Date       |Equals, IsBetween
CISDD   |Fälligkeitsdatum für die Übermittlung der Konferenzinstanz     |Date       |Equals, IsBetween
DCN     |Anzeigename der Konferenzinstanz  |Zeichenfolge      |Keine
E | Erweiterte Metadaten</br></br>**WICHTIG:** Dieses Attribut ist veraltet und wird nur für ältere Anwendungen unterstützt. Die individuelle Abfrage dieses Attributs (z. B. attributes=Id,Ti,E) führt dazu, dass alle erweiterten Metadatenattribute in einer *serialisierten JSON-Zeichenfolge* zurückgegeben werden.</br></br>Alle in den erweiterten Metadaten enthaltenen Attribute stehen jetzt als Attribut der obersten Ebene zur Verfügung und können als solche angefordert werden (z. B. attributes=Id,Ti,DOI,IA). | [Erweitert](#extended) | Keine
ECC     |Geschätzte Gesamtzitatanzahl der Konferenzinstanz |Int32      |Keine
FN | Vollständiger Name der Konferenzinstanz | Zeichenfolge | Keine
Id      |Entitäts-ID                              |Int64      |Equals
PC | Gesamtveröffentlichungsanzahl der Konferenzinstanz | Int32 | Keine
PCS.CN  |Name der übergeordneten Konferenzreihe der Instanz |Zeichenfolge  |Equals
PCS.CId |ID der übergeordneten Konferenzreihe der Instanz |Int64     |Equals

## <a name="extended"></a>Erweitert

> [!IMPORTANT]
> Dieses Attribut ist veraltet und wird nur für ältere Anwendungen unterstützt. Die individuelle Abfrage dieses Attributs (z. B. attributes=Id,Ti,E) führt dazu, dass alle erweiterten Metadatenattribute in einer *serialisierten JSON-Zeichenfolge* zurückgegeben werden.</br></br>Alle in den erweiterten Metadaten enthaltenen Attribute stehen jetzt als Attribut der obersten Ebene zur Verfügung und können als solche angefordert werden (z. B. attributes=Id,Ti,DOI,IA).

> [!IMPORTANT]
> Die Unterstützung für die Anforderung einzelner erweiterter Attribute mithilfe des Bereichs „E.“ d. h. „E.DN“, ist veraltet. Obwohl dies technisch noch unterstützt wird, führt die Anforderung einzelner erweiterter Attribute mit dem Bereich „E.“ dazu, dass der Attributwert an zwei Stellen in der JSON-Antwort, als Teil des Objekts „E“ und als Attribut der obersten Ebene zurückgegeben wird.

NAME | BESCHREIBUNG | type | Vorgänge
--- | --- | --- | ---
FN | Vollständiger Name der Konferenzinstanz | Zeichenfolge | Keine
PC | Gesamtveröffentlichungsanzahl der Konferenzinstanz | Int32 | Keine
