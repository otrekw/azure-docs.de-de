---
title: Diensteinschränkungen
titleSuffix: Azure Machine Learning
description: Grenzwerte für den Dienst, die bei der Kapazitätsplanung verwendet werden, sowie Höchstwerte für Anforderungen und Antworten für Azure Machine Learning.
services: machine-learning
author: andscho
ms.author: andscho
ms.reviewer: mldocs
ms.topic: reference
ms.service: machine-learning
ms.subservice: core
ms.date: 12/21/2020
ms.openlocfilehash: 132c4b223b99f7110cd4553bb0dd93f1e58f4cdd
ms.sourcegitcommit: a89a517622a3886b3a44ed42839d41a301c786e0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2020
ms.locfileid: "97733216"
---
# <a name="service-limits-in-azure-machine-learning"></a>Diensteinschränkungen in Azure Machine Learning

In diesem Abschnitt werden die grundlegenden Kontingente und Drosselungsschwellenwerte in Azure Machine Learning aufgelistet.

## <a name="workspaces"></a>Arbeitsbereiche
| Begrenzung | Wert |
| --- | --- |
| Arbeitsbereichname | 2-32 Zeichen |

## <a name="runs"></a>Ausführungen
| Begrenzung | Wert |
| --- | --- |
| Ausführungen pro Arbeitsbereich | 10 Millionen |
| RunId/ParentRunId | 256 Zeichen |
| DataContainerId | 261 Zeichen |
| DisplayName |256 Zeichen|
| BESCHREIBUNG |5\.000 Zeichen|
| Anzahl von Eigenschaften |50 |
| Länge des Eigenschaftsschlüssels |100 Zeichen |
| Länge des Eigenschaftswerts |1\.000 Zeichen |
| Anzahl von Tags |50 |
| Länge des Tagschlüssels |100 |
| Länge des Tagwerts |1\.000 Zeichen |
| CancelUri/CompleteUri/DiagnosticsUri |1\.000 Zeichen |
| Länge der Fehlermeldung |3\.000 Zeichen |
| Länge der Warnmeldung |300 Zeichen |
| Anzahl von Eingabedatasets |200 |
| Anzahl von Ausgangsdatasets |20 |


## <a name="metrics"></a>Metriken
| Begrenzung | Wert |
| --- | --- |
| Metriknamen pro Ausführung |50|
| Metrikzeilen pro Metrikname |10 Millionen|
| Spalten pro Metrikzeile |15|
| Länge des Namens der Metrikspalte |255 Zeichen |
| Länge des Werts der Metrikspalte |255 Zeichen |
| Metrikzeilen pro hochgeladenem Batch | 250 |

> [!NOTE]
> Wenn Sie an die Grenze der Metriknamen pro Ausführung stoßen, da Sie Variablen im Metriknamen formatieren, sollten Sie stattdessen eine Zeilenmetrik verwenden, bei der eine Spalte der Variablenwert und die zweite Spalte der Metrikwert ist.

## <a name="artifacts"></a>Artifacts

| Begrenzung | Wert |
| --- | --- |
| Anzahl von Artefakten pro Ausführung |10 Millionen|
| Maximale Länge des Artefaktpfads |5\.000 Zeichen |

## <a name="limit-increases"></a>Grenzwerterhöhungen
Einige Grenzwerte können für einzelne Arbeitsbereiche erhöht werden, indem Sie sich an den [Support wenden](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/). 

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren Ihrer Azure Machine Learning-Umgebung](how-to-configure-environment.md)
