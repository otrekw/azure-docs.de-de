---
title: Übernehmen von Branchenstandardontologien
titleSuffix: Azure Digital Twins
description: Hier erfahren Sie mehr über bereits vorhandene branchenspezifische Ontologien, die für Azure Digital Twins übernommen werden können.
author: baanders
ms.author: baanders
ms.date: 2/12/2021
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 71795e9dc439d5f634fc4d777aa6b5cdd66e8f5c
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100563369"
---
# <a name="adopting-an-industry-ontology"></a>Übernehmen einer branchenspezifischen Ontologie

Da es einfacher sein kann, mit einer Open-Source-DTDL-Ontologie zu beginnen, als von Grund auf neu zu entwickeln, arbeitet Microsoft mit Branchenexperten zusammen, um Ontologien zu veröffentlichen, die weitgehend akzeptierte Branchenkonventionen darstellen und eine Vielzahl verschiedener Anwendungsfälle von Kunden unterstützen. 

Das Ergebnis dieser Zusammenarbeit ist eine Reihe von quelloffenen DTDL-basierten Ontologien, die aus Branchenstandards lernen, auf diesen aufbauen oder sie direkt verwenden. Diese Ontologien sind so konzipiert, dass sie die Anforderungen von Entwicklern, die damit weiterarbeiten, erfüllen und das Potenzial haben, in der entsprechenden Branche von vielen übernommen und/oder erweitert zu werden.

Bisher hat Microsoft mit Partnern aus der Immobilienbranche zusammengearbeitet, um eine Ontologie für intelligente Gebäude zu entwickeln, die eine gemeinsame Grundlage für die Modellierung von intelligenten Gebäuden basierend auf Branchenstandards bietet, um ein Neuerfinden zu vermeiden. 

## <a name="realestatecore-smart-building-ontology"></a>RealEstateCore-Ontologie für intelligente Gebäude

Microsoft hat in Zusammenarbeit mit [RealEstateCore](https://www.realestatecore.io/), einem schwedischen Zusammenschluss von Immobilieneigentümern, Softwareherstellern und Forschungseinrichtungen, eine quelloffene DTDL-Ontologie für die Immobilienbranche bereitgestellt: die [**DTDL-basierte RealEstateCore-Ontologie für intelligente Gebäude**](https://github.com/Azure/opendigitaltwins-building).

Diese Ontologie für intelligente Gebäude bietet eine gemeinsame Grundlage für die Modellierung von intelligenten Gebäuden basierend auf Branchenstandards (z. B. dem  [Brick-Schema](https://brickschema.org/ontology/) oder der  [W3C Building Topology Ontology](https://w3c-lbd-cg.github.io/bot/index.html)), um ein Neuerfinden zu vermeiden. Außerdem werden bewährte Methoden für die Nutzung und richtige Erweiterung der Ontologie bereitgestellt. 

Weitere Informationen zu den Struktur- und Modellierungskonventionen für die Ontologie, ihrer Verwendung, ihrer Erweiterung sowie zur Mitwirkung daran finden Sie im [Repository für die Ontologie auf GitHub](https://github.com/Azure/opendigitaltwins-building). 

Weitere Informationen zur Partnerschaft mit RealEstateCore und den Zielen für diese Initiative finden Sie in diesem Blogbeitrag und dem zugehörigen Video: [*RealEstateCore, eine Ontologie für intelligente Gebäude für Digital Twins, ist jetzt verfügbar*](https://techcommunity.microsoft.com/t5/internet-of-things/realestatecore-a-smart-building-ontology-for-digital-twins-is/ba-p/1914794).

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das Erweitern von Branchenstandardontologien zur Erfüllung Ihrer Spezifikationen: [*Konzepte: Erweitern von Branchenontologien*](concepts-ontologies-extend.md).

* Oder fahren Sie mit dem Pfad für das Entwickeln von Modellen basierend auf Ontologien fort: [*Verwenden von Ontologiestrategien in einem Modellentwicklungspfad*](concepts-ontologies.md#using-ontology-strategies-in-a-model-development-path).