---
title: Bewerten der Fairness und Vermeiden von Unfairness bei Machine Learning-Modellen
titleSuffix: Azure Machine Learning
description: Hier finden Sie Informationen zur Fairness bei Machine Learning-Modellen, und Sie erfahren, wie das Python-Paket „Fairlearn“ zur Erstellung gerechterer Modelle beitragen kann.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: luquinta
author: luisquintanilla
ms.date: 05/02/2020
ms.openlocfilehash: c21ec0329a7b5716a00262b7422296df3afe208b
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83596359"
---
# <a name="fairness-in-machine-learning-models"></a>Fairness bei Machine Learning-Modellen

Hier finden Sie Informationen zur Fairness bei Machine Learning, und Sie erfahren, wie das Open-Source-Python-Paket „Fairlearn“ zur Erstellung gerechterer Modelle beitragen kann.

## <a name="what-is-fairness-in-machine-learning-systems"></a>Was ist Fairness bei Machine Learning-Systemen?

Systeme mit künstlicher Intelligenz und Machine Learning können unfaires Verhalten an den Tag legen. Unfaires Verhalten kann beispielsweise auf der Grundlage entstandener Nachteile oder Auswirkungen auf Menschen definiert werden. KI-Systeme können viele Arten von Nachteilen verursachen. Zwei weit verbreitete Arten KI-bedingter Nachteile sind:

- Nachteile bei der Zuteilung: Ein KI-System stellt Gelegenheiten, Ressourcen oder Informationen bereit oder enthält sie vor. Beispiele wären etwa Personalentscheidungen, eine Aufnahme bei einer Bildungseinrichtung und eine Kreditvergabe, bei denen ein Modell geeignete Kandidaten aus einer bestimmten Personengruppe möglicherweise besser auswählen kann als aus anderen Gruppen.

- Nachteile bei der Servicequalität: Ein KI-System funktioniert für eine Personengruppe besser als für eine andere. Beispielsweise kann es vorkommen, dass ein Spracherkennungssystem bei Männern besser funktioniert als bei Frauen.

Diese Nachteile müssen bewertet und beseitigt werden, um unfaires Verhalten von KI-Systemen zu minimieren.

>[!NOTE]
> Fairness ist eine soziotechnische Herausforderung. Viele Fairnessaspekte wie Gerechtigkeit und ein faires Verfahren werden nicht in quantitativen Fairnessmetriken erfasst. Außerdem können viele quantitative Fairnessmetriken nicht gleichzeitig befriedigt werden. Das Ziel besteht darin, es Menschen zu ermöglichen, verschiedene Entschärfungsstrategien zu bewerten und dann geeignete Kompromisse für das jeweilige Szenario einzugehen.

## <a name="fairness-assessment-and-mitigation-with-fairlearn"></a>Bewertung der Fairness und Vermeidung von Unfairness mit Fairlearn

Fairlearn ist ein Open-Source-Python-Paket, mit dem Entwickler von Machine Learning-Systemen die Fairness ihrer Systeme bewerten und die festgestellten Fairnessprobleme behandeln können.

Fairlearn besteht aus zwei Komponenten:

- Bewertungsdashboard: Ein Jupyter Notebook-Widget, um die Auswirkungen der Vorhersagen eines Modells auf verschiedene Gruppen zu bewerten. Darüber hinaus können hier verschiedene Modelle anhand von Fairness- und Leistungsmetriken miteinander verglichen werden.
- Entschärfungsalgorithmen: Eine Reihe von Algorithmen zur Behandlung von Unfairness bei der binären Klassifizierung und Regression.

Diese Komponenten ermöglichen es Data Scientists und Führungskräften, Kompromisse zwischen Fairness und Leistung abzuwägen und die optimale Entschärfungsstrategie für ihre Anforderungen zu verfolgen.

## <a name="fairness-assessment"></a>Fairnessbewertung

In Fairlearn basiert das Fairnesskonzept auf einem als **Gruppenfairness** bezeichneten Ansatz. Dabei wird folgende Frage gestellt: Bei welchen Personengruppen besteht die Gefahr einer Benachteiligung?

Die relevanten Gruppen werden auch als Teilgesamtheiten bezeichnet und durch **sensible Features** oder sensible Attribute definiert. Sensible Features werden als Vektor oder Matrix namens `sensitive_features` an einen Fairlearn-Schätzer weitergegeben. Der Begriff legt nahe, dass Systemdesigner diese Features bei der Bewertung der Gruppenfairness besonders berücksichtigen sollten. Dabei ist darauf zu achten, ob diese Features personenbezogene Informationen enthalten und somit Auswirkungen auf den Datenschutz haben. Das Wort „sensibel“ bedeutet jedoch nicht, dass diese Features nicht für Vorhersagen herangezogen werden sollen.

Im Rahmen der Bewertungsphase wird Fairness durch Ungleichheitsmetriken quantifiziert. **Ungleichheitsmetriken** ermöglichen das Auswerten und Vergleichen des Verhaltens eines Modells für verschiedene Gruppen – entweder in Form von Verhältnissen oder in Form von Unterschieden. Von Fairlearn werden zwei Klassen von Ungleichheitsmetriken unterstützt:


- Ungleichheit bei der Modellleistung: Mit diesen Metriken wird die Ungleichheit (Differenz) der Werte der ausgewählten Leistungsmetrik für verschiedene Untergruppen berechnet. Beispiele hierfür sind:

  - Ungleichheit bei der Genauigkeitsrate
  - Ungleichheit bei der Fehlerrate
  - Ungleichheit bei der Präzision
  - Ungleichheit beim Abruf
  - Ungleichheit beim mittleren absoluten Fehler (Mean Absolute Error, MAE)
  - usw.

- Ungleichheit bei der Auswahlrate: Diese Metrik enthält die Differenz bei der Auswahlrate für verschiedene Untergruppen. Ein Beispiel hierfür ist die Ungleichheit bei der Darlehensbewilligungsrate. Mit Auswahlrate ist der Anteil von Datenpunkten in jeder Klasse, die als „1“ klassifiziert wurden (binäre Klassifizierung), oder die Verteilung von Vorhersagewerten (Regression) gemeint.

## <a name="unfairness-mitigation"></a>Vermeidung von Unfairness

### <a name="parity-constraints"></a>Gleichheitseinschränkungen

Fairlearn enthält verschiedene Algorithmen zur Vermeidung von Unfairness. Diese Algorithmen unterstützen eine Reihe von Einschränkungen für das Vorhersageverhalten, die als **Gleichheitseinschränkungen** oder -kriterien bezeichnet werden. Für Gleichheitseinschränkungen müssen einige Aspekte des Vorhersageverhaltens über die Gruppen hinweg vergleichbar sein, durch die sensible Features definiert werden (beispielsweise unterschiedliche Ethnien). Gleichheitseinschränkungen dieser Art werden von den Entschärfungsalgorithmen von Fairlearn genutzt, um die festgestellten Fairnessprobleme zu behandeln.

Von Fairlearn werden folgende Arten von Gleichheitseinschränkungen unterstützt:

|Gleichheitseinschränkung  | Zweck  |ML-Aufgabe  |
|---------|---------|---------|
|Demografische Gleichheit     |  Vermeiden von Nachteilen bei der Zuteilung | Binäre Klassifizierung, Regression |
|Ausgeglichene Chancen  | Diagnostizieren von Nachteilen bei der Zuteilung und bei der Servicequalität | Binäre Klassifizierung        |
|Begrenzter Gruppenverlust     |  Vermeidung von Nachteilen bei der Servicequalität | Regression |

### <a name="mitigation-algorithms"></a>Entschärfungsalgorithmen

Fairlearn bietet folgende Entschärfungsalgorithmen zur Vermeidung von Unfairness:

- Reduzierung: Bei diesen Algorithmen wird ein standardmäßiger Blackbox-ML-Schätzer (etwa ein LightGBM-Modell) verwendet und unter Verwendung einer Sequenz neu gewichteter Trainingsdatasets eine Reihe neu trainierter Modelle generiert. So kann beispielsweise die Gewichtung von Bewerbern eines bestimmten Geschlechts erhöht oder verringert werden, um Modelle neu zu trainieren und Ungleichheiten zwischen verschiedenen Geschlechtergruppen zu verringern. Benutzer können dann ein Modell auswählen, das den besten Kompromiss zwischen Genauigkeit (oder einer anderen Leistungsmetrik) und der Ungleichheit darstellt, wobei im Allgemeinen Geschäftsregeln und Kostenkalkulationen berücksichtigt werden müssen.  
- Nachbearbeitung: Bei diesen Algorithmen werden als Eingabe ein vorhandener Klassifizierer und das sensible Feature herangezogen. Anschließend wird von der Vorhersage des Klassifizierers eine Transformation abgeleitet, um die angegebenen Fairnesseinschränkungen zu erzwingen. Der größte Vorteil der Schwellenwertoptimierung ist ihre Einfachheit und Flexibilität, da das Modell nicht neu trainiert werden muss. 

| Algorithmus | Beschreibung | ML-Aufgabe | Sensible Features | Unterstützte Gleichheitseinschränkungen | Algorithmustyp |
| --- | --- | --- | --- | --- | --- |
| `ExponentiatedGradient` | Unter [Ein Reduzierungsansatz zu fairer Klassifizierung](https://arxiv.org/abs/1803.02453) beschriebener Blackbox-Ansatz für eine faire Klassifizierung | Binäre Klassifizierung | Kategorisch | [Demografische Gleichheit](#parity-constraints), [Ausgeglichene Chancen](#parity-constraints) | Reduzierung |
| `GridSearch` | Unter [Ein Reduzierungsansatz zu fairer Klassifizierung](https://arxiv.org/abs/1803.02453) beschriebener Blackbox-Ansatz| Binäre Klassifizierung | Binary | [Demografische Gleichheit](#parity-constraints), [Ausgeglichene Chancen](#parity-constraints) | Reduzierung |
| `GridSearch` | Blackbox-Ansatz, bei dem eine Rastersuchvariante der fairen Regression mit dem Algorithmus für den begrenzten Gruppenverlust implementiert wird, wie unter [Faire Regression: Quantitative Definitionen und reduzierungsbasierte Algorithmen](https://arxiv.org/abs/1905.12843) beschrieben | Regression | Binary | [Begrenzter Gruppenverlust](#parity-constraints) | Reduzierung |
| `ThresholdOptimizer` | Nachbearbeitungsalgorithmus auf der Grundlage von [Chancengleichheit bei beaufsichtigtem Lernen](https://arxiv.org/abs/1610.02413). Bei dieser Technik werden als Eingabe ein vorhandener Klassifizierer und ein sensibles Feature verwendet. Anschließend wird eine monotone Transformation von der Vorhersage des Klassifizierers abgeleitet, um die angegebenen Gleichheitseinschränkungen zu erzwingen. | Binäre Klassifizierung | Kategorisch | [Demografische Gleichheit](#parity-constraints), [Ausgeglichene Chancen](#parity-constraints) | Nachbearbeitung |

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Verwendung der verschiedenen Komponenten finden Sie im [GitHub-Repository für Fairlearn](https://github.com/fairlearn/fairlearn/) sowie in den [Beispiel-Notebooks](https://github.com/fairlearn/fairlearn/tree/master/notebooks).
- Informationen zur Wahrung des Datenschutzes durch Verwendung des differenziellen Datenschutzes und des WhisperNoise-Pakets finden Sie [hier](concept-differential-privacy.md).