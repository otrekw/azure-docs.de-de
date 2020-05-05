---
title: Einschränkungen von Azure Blockchain Service
description: Übersicht über die Einschränkungen des Diensts und der Funktionalität des Azure Blockchain-Diensts
ms.date: 04/02/2020
ms.topic: conceptual
ms.reviewer: ravastra
ms.openlocfilehash: 71e1bebf10fa0142870d03977182472da1ad031f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "80676520"
---
# <a name="limits-in-azure-blockchain-service"></a>Einschränkungen des Azure Blockchain-Diensts

Der Azure Blockchain-Dienst weist dienstliche und funktionale Einschränkungen wie die Anzahl von Knoten, über die ein Mitglied verfügen kann, Konsortiumseinschränkungen und Speichermengen auf.

## <a name="pricing-tier"></a>Tarif

Die maximalen Grenzwerte für Transaktionen und Validierungsknoten hängen davon ab, ob Sie über den Tarif „Basic“ oder „Standard“ für den Azure Blockchain-Dienst verfügen.

| Tarif | Max. Transaktionsknoten | Max. Validierungsknoten |
|:---|:---:|:---:|
| Basic | 10 | 1 |
| Standard | 10 | 2 |

Ihr Konsortiumsnetzwerk sollte über mindestens zwei Azure Blockchain Service-Knoten im Tarif „Standard“ verfügen. Knoten im Tarif „Standard“ enthalten zwei Validierungsknoten. Vier Validierungsknoten sind erforderlich, um den [Istanbul Byzantine Fault Tolerance Consensus](https://github.com/jpmorganchase/quorum/wiki/Quorum-Consensus) zu erfüllen.

Verwenden Sie den Tarif „Basic“ für die Entwicklung, das Testen und Proof of Concept-Vorgänge. Verwenden Sie den Tarif „Standard“ für Bereitstellungen für die Produktion. Sie sollten den Tarif *Standard* auch verwenden, wenn Sie Blockchain Data Manager verwenden oder eine große Menge privater Transaktionen senden.

Das Wechseln zwischen den Tarifen „Basic“ und „Standard“ nach der Erstellung eines Mitglieds wird nicht unterstützt.

## <a name="storage-capacity"></a>Speicherkapazität

Die maximale Speichermenge, die pro Knoten für Ledgerdaten und Protokolle verwendet werden kann, beträgt 1,8 Terabyte.

Die Speichergröße für Ledger und Protokolle kann nicht verringert werden.
## <a name="consortium-limits"></a>Konsortiumseinschränkungen

* **Namen von Konsortien und Mitgliedern müssen eindeutig sein**, d. h. sie müssen sich von den Namen anderer Konsortien und Mitglieder im Azure Blockchain-Dienst unterscheiden.

* **Die Namen von Mitgliedern und Konsortien können nicht geändert werden.**

* **Alle Mitglieder eines Konsortiums müssen im gleichen Tarif sein.**

* **Alle Mitglieder eines Konsortiums müssen sich in derselben Region befinden.**

    Das erste in einem Konsortium erstellte Mitglied bestimmt die Region. In das Konsortium eingeladene Mitglieder müssen sich in derselben Region wie das erste Mitglied befinden. Durch die Einschränkung der Mitglieder auf eine Region wird sichergestellt, dass der Netzwerkkonsens nicht negativ beeinträchtigt wird.

* **Ein Konsortium muss über mindestens einen Administrator verfügen.**

    Wenn nur ein Administrator in einem Konsortium enthalten ist, kann dieser sich nicht selbst aus dem Konsortium entfernen oder sein Mitglied löschen, es sei denn, ein weiterer Administrator wird im Konsortium hinzugefügt bzw. höher gestuft.

* **Mitglieder, die aus dem Konsortium entfernt wurden, können nicht wieder hinzugefügt werden.**

    Stattdessen müssen diese erneut eingeladen werden, damit sie ein neues Mitglied erstellen können, um dem Konsortium beizutreten. Vorhandene Mitgliederressourcen werden nicht gelöscht, um Transaktionsverläufe beizubehalten.

* **Alle Mitglieder eines Konsortiums müssen dieselbe Ledgerversion verwenden.**

    Weitere Informationen zu Patches, Updates und Ledgerversionen, die für den Azure Blockchain-Dienst verfügbar sind, finden Sie unter [Patches, Updates und Versionen](ledger-versions.md).

## <a name="performance"></a>Leistung

Verwenden Sie nicht die gas-Funktion *eth.estimate* für jede Transaktionsübermittlung. Die Funktion *eth.estimate* ist sehr arbeitsspeicherintensiv. Das mehrmalige Aufrufen der Funktion verringert die Transaktionen pro Sekunde drastisch.

Verwenden Sie nach Möglichkeit einen konservativen gas-Wert für die Übermittlung von Transaktionen, und minimieren Sie die Verwendung von *eth.estimate*.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Richtlinien für das Patchen und Aktualisieren von Systemen: [Patchen, Updates und Versionen](ledger-versions.md).
