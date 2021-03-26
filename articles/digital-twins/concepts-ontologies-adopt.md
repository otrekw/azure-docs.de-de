---
title: Übernehmen von Branchenstandardontologien
titleSuffix: Azure Digital Twins
description: Hier erfahren Sie mehr über bereits vorhandene branchenspezifische Ontologien, die für Azure Digital Twins übernommen werden können.
author: baanders
ms.author: baanders
ms.date: 2/26/2021
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: c4f003fc9e418501af76281c10277fce3606e24c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102124226"
---
# <a name="adopting-an-industry-ontology"></a>Übernehmen einer branchenspezifischen Ontologie

Da es einfacher sein kann, mit einer Open-Source-DTDL-Ontologie zu beginnen, als von Grund auf neu zu entwickeln, arbeitet Microsoft mit Branchenexperten zusammen, um Ontologien zu veröffentlichen, die weitgehend akzeptierte Branchenkonventionen darstellen und eine Vielzahl verschiedener Anwendungsfälle von Kunden unterstützen. 

Das Ergebnis dieser Zusammenarbeit ist eine Reihe von quelloffenen DTDL-basierten Ontologien, die aus Branchenstandards lernen, auf diesen aufbauen oder sie direkt verwenden. Diese Ontologien sind so konzipiert, dass sie die Anforderungen von Entwicklern, die damit weiterarbeiten, erfüllen und das Potenzial haben, in der entsprechenden Branche von vielen übernommen und/oder erweitert zu werden.

Microsoft hat hierbei mit Partnern zusammengearbeitet, um eine Ontologie für [intelligente Gebäude](#realestatecore-smart-building-ontology) und eine Ontologie für [intelligente Städte](#smart-cities-ontology) zu entwickeln. Sie dienen als gemeinsame Grundlage für die Modellierung anhand der Standards in diesen Branchen, um zu vermeiden, dass Aufwand für Neuerfindungen anfällt. 

## <a name="realestatecore-smart-building-ontology"></a>RealEstateCore-Ontologie für intelligente Gebäude

*Die Ontologie finden Sie hier: [**Auf der Digital Twins Definition Language (DTDL) basierende RealEstateCore-Ontologie für intelligente Gebäude**](https://github.com/Azure/opendigitaltwins-building)* .

Microsoft hat in Zusammenarbeit mit [RealEstateCore](https://www.realestatecore.io/), einem schwedischen Zusammenschluss von Immobilieneigentümern, Softwareherstellern und Forschungseinrichtungen, diese quelloffene DTDL-Ontologie für die Immobilienbranche bereitgestellt.

Diese Ontologie für intelligente Gebäude bietet eine gemeinsame Grundlage für die Modellierung von intelligenten Gebäuden basierend auf Branchenstandards (z. B. dem  [Brick-Schema](https://brickschema.org/ontology/) oder der  [W3C Building Topology Ontology](https://w3c-lbd-cg.github.io/bot/index.html)), um ein Neuerfinden zu vermeiden. Außerdem werden bewährte Methoden für die Nutzung und richtige Erweiterung der Ontologie bereitgestellt. 

Weitere Informationen zu den Struktur- und Modellierungskonventionen für die Ontologie, ihrer Verwendung, ihrer Erweiterung sowie zur Mitwirkung daran finden Sie im Repository für die Ontologie auf GitHub: [Azure/opendigitaltwins-building](https://github.com/Azure/opendigitaltwins-building). 

Weitere Informationen zur Partnerschaft mit RealEstateCore und den Zielen für diese Initiative finden Sie in diesem Blogbeitrag und dem zugehörigen Video: [RealEstateCore, eine Ontologie für intelligente Gebäude für Digital Twins, ist jetzt verfügbar](https://techcommunity.microsoft.com/t5/internet-of-things/realestatecore-a-smart-building-ontology-for-digital-twins-is/ba-p/1914794).

## <a name="smart-cities-ontology"></a>Ontologie für intelligente Städte

*Die Ontologie finden Sie hier: [**DTDL-Ontologie (Digital Twins Definition Language) für intelligente Städte**](https://github.com/Azure/opendigitaltwins-smartcities)* .

Microsoft hat mit [Open Agile Smart Cities (OASC)](https://oascities.org/) und [Sirus](https://sirus.be/) zusammengearbeitet, um eine DTDL-basierte Ontologie für intelligente Städte bereitzustellen. Hierbei wurde mit [ETSI CIM NGSI-LD](https://www.etsi.org/committee/cim) begonnen. Zusätzlich zu ETSI NGSI-LD haben wir auch Saref4City, CityGML, ISO und andere evaluiert.

Beim aktuellen Release der Ontologie liegt der Schwerpunkt auf einer ersten Gruppe von Modellen. Die Ersteller der Ontologie laden Sie ein, zur Erweiterung der anfänglichen Gruppe mit Anwendungsfällen beizutragen und die vorhandenen Modelle zu verbessern. 

Weitere Informationen zur Ontologie, zu ihrer Verwendung und zur Möglichkeit zum Leisten eines Beitrags finden Sie im Repository der Ontologie auf GitHub: [Azure/opendigitaltwins-smartcities](https://github.com/Azure/opendigitaltwins-smartcities). 

Weitere Informationen zu den Partnerschaften und zum Ansatz für intelligente Städte erhalten Sie auch in diesem Blogbeitrag und dem zugehörigen Video: [Ontologie für intelligente Städte: Digital Twins](https://techcommunity.microsoft.com/t5/internet-of-things/smart-cities-ontology-for-digital-twins/ba-p/2166585).

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das Erweitern von Branchenstandardontologien zur Erfüllung Ihrer Spezifikationen: [*Konzepte: Erweitern von Branchenontologien*](concepts-ontologies-extend.md).

* Oder fahren Sie mit dem Pfad für das Entwickeln von Modellen basierend auf Ontologien fort: [*Verwenden von Ontologiestrategien in einem Modellentwicklungspfad*](concepts-ontologies.md#using-ontology-strategies-in-a-model-development-path).