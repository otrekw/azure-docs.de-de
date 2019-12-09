---
title: Attribute der Autorentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Autorentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: darrine
manager: kuansanw
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: d5fc770c380397f605f8810fa41d3a8907f2358e
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74146497"
---
# <a name="author-entity"></a>Autorentität

> [!NOTE]
> Bei den folgenden Attributen handelt es sich um spezifische Attribute für die Autorentität. (Ty = '1')

NAME | BESCHREIBUNG | type | Vorgänge
--- | --- | --- | ---
Id      | Entitäts-ID                             |Int64      |Equals
AuN     | Normalisierter Autorname                    |Zeichenfolge     |Equals
CC      | Gesamtzitatanzahl des Autors           |Int32      |Keine  
DAuN    | Autoranzeigename                   |Zeichenfolge     |Keine
E | Erweiterte Metadaten</br></br>**WICHTIG:** Dieses Attribut ist veraltet und wird nur für ältere Anwendungen unterstützt. Die individuelle Abfrage dieses Attributs (attributes=Id,Ti,E) führt dazu, dass alle erweiterten Metadatenattribute in einer *serialisierten JSON-Zeichenfolge* zurückgegeben werden.</br></br>Alle in den erweiterten Metadaten enthaltenen Attribute stehen jetzt als Attribut der obersten Ebene zur Verfügung und können als solche angefordert werden (attributes=Id,Ti,DOI,IA). | [Erweitert](#extended) | Keine
ECC     | Geschätzte Gesamtzitatanzahl des Autors |Int32      |Keine
LKA.AfId | Entitäts-ID der letzten bekannten Zugehörigkeit, die für den Autor gefunden wurde. | Int64 | Keine
LKA.AfN | Normalisierter Name der letzten bekannten Zugehörigkeit, die für den Autor gefunden wurde. | Zeichenfolge | Keine
PC | Gesamtanzahl der Veröffentlichungen des Autors | Int32 | Keine

## <a name="extended"></a>Erweitert

> [!IMPORTANT]
> Dieses Attribut ist veraltet und wird nur für ältere Anwendungen unterstützt. Die individuelle Abfrage dieses Attributs (attributes=Id,Ti,E) führt dazu, dass alle erweiterten Metadatenattribute in einer *serialisierten JSON-Zeichenfolge* zurückgegeben werden.</br></br>Alle in den erweiterten Metadaten enthaltenen Attribute stehen jetzt als Attribut der obersten Ebene zur Verfügung und können als solche angefordert werden (attributes=Id,Ti,DOI,IA).

> [!IMPORTANT]
> Die Unterstützung für die Anforderung einzelner erweiterter Attribute mithilfe des Bereichs „E.“ (also „E.DN“) ist veraltet. Dies wird zwar technisch noch unterstützt, die Anforderung einzelner erweiterter Attribute mit dem Bereich „E.“ führt jedoch dazu, dass der Attributwert an zwei Stellen in der JSON-Antwort zurückgegeben wird: als Teil des Objekts „E“ und als Attribut der obersten Ebene.

NAME | BESCHREIBUNG | type | Vorgänge
--- | --- | --- | ---
LKA.AfId | Entitäts-ID der letzten bekannten Zugehörigkeit, die für den Autor gefunden wurde. | Int64 | Keine
LKA.AfN | Normalisierter Name der letzten bekannten Zugehörigkeit, die für den Autor gefunden wurde. | Zeichenfolge | Keine
PC | Gesamtanzahl der Veröffentlichungen des Autors | Int32 | Keine
