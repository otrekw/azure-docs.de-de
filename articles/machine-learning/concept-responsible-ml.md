---
title: Verantwortungsvolles maschinelles Lernen (ML)
titleSuffix: Azure Machine Learning
description: Erfahren Sie, was verantwortungsvolles ML ist und wie es in Azure Machine Learning eingesetzt werden kann.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: luquinta
author: luisquintanilla
ms.date: 05/08/2020
ms.openlocfilehash: 3cef3c2179019f6d84de5596e61abaf8d7d3182c
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595009"
---
# <a name="responsible-machine-learning-ml"></a>Verantwortungsvolles maschinelles Lernen (ML)

In diesem Artikel erfahren Sie, was verantwortungsvolles ML ist, und wie Sie es mit Azure Machine Learning in die Praxis umsetzen können.

Während der gesamten Entwicklung und Nutzung von KI-Systemen muss Vertrauen im Mittelpunkt stehen. Vertrauen in die Plattform, den Prozess und die Modelle. Bei Microsoft umfasst verantwortungsvolles ML die folgenden Werte und Prinzipien:

- Verstehen der Machine Learning-Modelle
  - Interpretieren und Erläutern des Modellverhaltens
  - Bewerten und Minimieren der Modellungerechtigkeiten
- Schützen von Personen und ihren Daten
  - Verhindern der Offenlegung von Daten mit differenziellem Datenschutz  
- Steuern des End-to-End-Machine Learning-Prozesses
  - Dokumentieren des Machine Learning-Lebenszyklus mit Datenblättern

:::image type="content" source="media/concept-responsible-ml/responsible-ml-pillars.png" alt-text="Säulen des verantwortungsvollen maschinellen Lernens":::

Da sich künstliche Intelligenz und autonome Systeme immer mehr in das Gefüge der Gesellschaft integrieren, ist es wichtig, proaktiv Maßnahmen zu ergreifen, um den unbeabsichtigten Folgen dieser Technologien vorzugreifen und sie zu minimieren.

## <a name="interpret-and-explain-model-behavior"></a>Interpretieren und Erläutern des Modellverhaltens

Schwer zu erklärende oder Blackbox-Systeme können problematisch sein, da sie es Projektbeteiligten wie Systementwicklern, Regulierungsbehörden, Benutzern und Entscheidungsträgern schwer machen zu verstehen, warum Systeme bestimmte Entscheidungen treffen. Einige KI-Systeme sind leichter zu erklären als andere, und gelegentlich gibt es einen Kompromiss zwischen einem System mit höherer Genauigkeit und einem, das leichter zu erklären ist.

Für die Erstellung interpretierbarer KI-Systeme verwenden Sie [InterpretML](https://github.com/interpretml/interpret), ein von Microsoft erstelltes Open-Source-Paket. [InterpretML kann innerhalb von Azure Machine Learning](how-to-machine-learning-interpretability.md) verwendet werden, um [Ihre Machine Learning-Modelle zu interpretieren und zu erklären](how-to-machine-learning-interpretability-aml.md), einschließlich [automatisierter Machine Learning-Modelle](how-to-machine-learning-interpretability-automl.md).

## <a name="assess-and-mitigate-model-unfairness"></a>Bewerten und Minimieren der Modellungerechtigkeiten

Da KI-Systeme immer mehr in die alltäglichen Entscheidungen der Gesellschaft einbezogen werden, ist es von äußerster Wichtigkeit, dass diese Systeme bestmöglich funktionieren, um faire Ergebnisse für alle zu erzielen.

Ungerechtigkeiten in KI-Systemen können zu folgenden unbeabsichtigten Folgen führen:

- Vorenthalten von Gelegenheiten, Ressourcen oder Informationen gegenüber Einzelpersonen.
- Verstärkung von Voreingenommenheiten und Stereotypen.

Viele Aspekte der Gerechtigkeit oder Fairness lassen sich nicht durch Metriken erfassen oder darstellen. Es gibt Tools und Methoden, die die Gerechtigkeit beim Entwurf und bei der Entwicklung von KI-Systemen verbessern können.

Zwei wichtige Schritte zur Verringerung der Ungerechtigkeit in KI-Systemen sind die Bewertung und die Risikominderung. Wir empfehlen [FairLearn](https://github.com/fairlearn/fairlearn), ein Open-Source-Paket, das die potenzielle Ungerechtigkeit von KI-Systemen bewerten und mindern kann. Weitere Informationen zur Gerechtigkeit und zum FairLearn-Paket finden Sie im Artikel [Gerechtigkeit in ML](./concept-fairness-ml.md).

## <a name="prevent-data-exposure-with-differential-privacy"></a>Verhindern der Offenlegung von Daten mit differenziellem Datenschutz

Wenn Daten zur Analyse verwendet werden, ist es wichtig, dass die Daten während ihrer gesamten Verwendung geschützt und vertraulich bleiben. Der differenzielle Datenschutz umfasst eine Reihe von Systemen und Methoden, die dazu beitragen, die Daten von Einzelpersonen sicher und privat zu halten.

In herkömmlichen Szenarien werden die Rohdaten in Dateien und Datenbanken gespeichert. Wenn Benutzer Daten analysieren, verwenden sie in der Regel diese Rohdaten. Dies ist bedenklich, da dadurch die Privacy- und Datenschutzgesetze des Einzelnen verletzt werden könnten. Der differenzielle Datenschutz versucht, diesem Problem zu begegnen, indem er den Daten entsprechende „Stördaten“ oder Zufallselemente hinzufügt, sodass die Benutzer keine einzelnen Datenpunkte identifizieren können.

Das Implementieren differenzieller privater Systeme ist schwierig. [WhiteNoise](https://github.com/opendifferentialprivacy/whitenoise-core) ist ein Open-Source-Projekt, das verschiedene Komponenten zum Erstellen globaler, differenzieller privater Systeme enthält. Weitere Informationen zum differenziellen Datenschutz und dem WhiteNoise-Projekt finden Sie im Artikel [Bewahren des Datenschutzes mithilfe des differenziellen Datenschutzes und mit WhiteNoise](./concept-differential-privacy.md).

## <a name="document-the-machine-learning-lifecycle-with-datasheets"></a>Dokumentieren des Machine Learning-Lebenszyklus mit Datenblättern

Das Dokumentieren der richtigen Informationen im Machine Learning-Prozess ist der Schlüssel zu verantwortungsvollen Entscheidungen in jeder Phase. Datenblätter sind eine Möglichkeit zum Dokumentieren von Machine Learning-Ressourcen, die im Rahmen des Machine Learning-Lebenszyklus verwendet und erstellt werden.

Modelle werden oft als „Blackbox“ betrachtet, und häufig gibt es nur wenige Informationen über sie. Da Machine Learning-Systeme immer weiter verbreitet sind und zur Entscheidungsfindung eingesetzt werden, ist die Verwendung von Datenblättern ein Schritt in Richtung der Entwicklung verantwortungsvollerer Machine Learning-Systeme.

Einige Modellinformationen, die Sie vielleicht im Rahmen eines Datenblattes dokumentieren möchten:

- Beabsichtigte Verwendung
- Modellarchitektur
- Verwendete Trainingsdaten
- Verwendete Auswertungsdaten
- Leistungsmetriken für Trainingsmodelle
- Informationen zur Gerechtigkeit

Im folgenden Beispiel erfahren Sie, wie das Azure Machine Learning SDK zur Implementierung von [Datenblättern für Modelle](https://github.com/microsoft/MLOps/blob/master/pytorch_with_datasheet/model_with_datasheet.ipynb) verwendet wird.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- Erfahren Sie mehr über die [ABOUT ML](https://www.partnershiponai.org/about-ml/)-Richtlinien zum Dokumentieren von Machine Learning-Systemen.