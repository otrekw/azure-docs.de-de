---
title: Attribute der Forschungsbereichsentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Forschungsbereichsentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: darrine
manager: kuansanw
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: b6bb5e0fd822541a0a4c10ef5f275d46664f8e10
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74146474"
---
# <a name="field-of-study-entity"></a>Forschungsbereichsentität

> [!NOTE]
> Die folgenden Attribute sind spezifisch für die Forschungsbereichsentität. (Ty = '6')

NAME | BESCHREIBUNG | type | Vorgänge
--- | --- | --- | ---
CC      |Gesamtzitatanzahl des Forschungsbereichs    |Int32      |Keine  
DFN     |Anzeigename des Forschungsbereichs            |Zeichenfolge     |Keine
ECC     |Geschätzte Gesamtzitatanzahl des Forschungsbereichs|Int32      |Keine
FL      |Ebene in der Hierarchie des Forschungsbereichs     |Int32      |Equals, IsBetween
FN      |Normalisierter Name des Forschungsbereichs         |Zeichenfolge     |Equals
FC.FId  |ID für untergeordneten Forschungsbereich                |Int64      |Equals
FC.FN   |Name für untergeordneten Forschungsbereich              |Zeichenfolge     |Equals
FP.FId  |ID für übergeordneten Forschungsbereich               |Int64      |Equals
FP.FN   |Name für übergeordneten Forschungsbereich             |Zeichenfolge     |Equals
Id      |Entitäts-ID                              |Int64      |Equals
PC    | Gesamtzahl der Veröffentlichungen des Forschungsbereichs | Int32 | Keine
