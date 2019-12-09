---
title: Attribute der Konferenzreihenentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Konferenzreihenentität verwenden können.
services: cognitive-services
author: darrine
manager: kuansanw
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: ecd8042212987849d9d302642f03e59493235599
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74143905"
---
# <a name="conference-series-entity"></a>Konferenzreihenentität

> [!NOTE]
> Die folgenden Attribute sind spezifisch für die Konferenzreihenentität. (Ty = '3')

NAME | BESCHREIBUNG | type | Vorgänge
--- | --- | --- | ---
CC      |Gesamtzitatanzahl der Konferenzreihe         |Int32      |Keine  
CN      |Normalisierter Name der Konferenzreihe      |Zeichenfolge     |Equals
DCN     |Anzeigename der Konferenzreihe         |Zeichenfolge     |Keine
ECC     |Geschätzte Gesamtzitatanzahl der Konferenzreihe   |Int32      |Keine
F.FId   |Die mit der Konferenzreihe verknüpfte Entitäts-ID des Forschungsbereichs |Int64  | Equals
F.FN    |Der mit der Konferenzreihe verknüpfte Name des Forschungsbereichs  | Equals,<br/>StartsWith
Id      |Entitäts-ID                              |Int64      |Equals
PC    |Gesamtzahl der Veröffentlichungen der Konferenzreihe |Int32 | Keine
