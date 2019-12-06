---
title: Attribute der Zuordnungsentität in der Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Zuordnungsentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: darrine
manager: kuansanw
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: 52dcccebe1a1cbab7a6afadeb6b543b34b8b1eb7
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74143924"
---
# <a name="affiliation-entity"></a>Zuordnungsentität

> [!NOTE]
> Die folgenden Attribute sind spezifisch für die Zuordnungsentität. (Ty = '5')

NAME | BESCHREIBUNG | type | Vorgänge
--- | --- | --- | ---
AfN | Normalisierter Name der Zuordnung |Zeichenfolge |Equals
CC | Gesamtzitatanzahl der Konferenzinstanz der Zuordnung |Int32        |none  
DAfN | Anzeigename der Zuordnung |Zeichenfolge |none
E | Erweiterte Metadaten</br></br>**WICHTIG:** Dieses Attribut ist veraltet und wird nur für ältere Anwendungen unterstützt. Die individuelle Abfrage dieses Attributs (z. B. attributes=Id,Ti,E) führt dazu, dass alle erweiterten Metadatenattribute in einer *serialisierten JSON-Zeichenfolge* zurückgegeben werden.</br></br>Alle in den erweiterten Metadaten enthaltenen Attribute stehen jetzt als Attribut der obersten Ebene zur Verfügung und können als solche angefordert werden (z. B. attributes=Id,Ti,DOI,IA). | [Erweitert](#extended) | Keine
ECC | Geschätzte Gesamtzitatanzahl der Zuordnung |Int32 |none
Id | Entitäts-ID |Int64 |Equals
PC | Gesamtzahl der im Zusammenhang mit dieser Entität geschriebenen Veröffentlichungen | Int32 | Keine

## <a name="extended"></a>Erweitert

> [!IMPORTANT]
> Dieses Attribut ist veraltet und wird nur für ältere Anwendungen unterstützt. Die individuelle Abfrage dieses Attributs (z. B. attributes=Id,Ti,E) führt dazu, dass alle erweiterten Metadatenattribute in einer *serialisierten JSON-Zeichenfolge* zurückgegeben werden.</br></br>Alle in den erweiterten Metadaten enthaltenen Attribute stehen jetzt als Attribut der obersten Ebene zur Verfügung und können als solche angefordert werden (z. B. attributes=Id,Ti,DOI,IA).

> [!IMPORTANT]
> Die Unterstützung für die Anforderung einzelner erweiterter Attribute mithilfe des Bereichs „E.“ d. h. „E.DN“, ist veraltet. Obwohl dies technisch noch unterstützt wird, führt die Anforderung einzelner erweiterter Attribute mit dem Bereich „E.“ dazu, dass der Attributwert an zwei Stellen in der JSON-Antwort, als Teil des Objekts „E“ und als Attribut der obersten Ebene zurückgegeben wird.

NAME | BESCHREIBUNG | type | Vorgänge
--- | --- | --- | ---
PC | Gesamtzahl der im Zusammenhang mit dieser Entität geschriebenen Veröffentlichungen | Int32 | Keine
