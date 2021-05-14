---
title: Erkennen von Bedrohungen mit integrierten Analyseregeln in Azure Sentinel | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie vorgefertigte Erkennungsregeln verwenden, die auf integrierten Vorlagen basieren, mit denen Sie bei verdächtigen Vorkommnissen eine Benachrichtigung erhalten.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2021
ms.author: yelevin
ms.openlocfilehash: ba757474ab24006b7b8b514bda085522bd353ea8
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107312670"
---
# <a name="tutorial-detect-threats-out-of-the-box"></a>Tutorial: Standardmäßig verfügbare Erkennung von Bedrohungen

Sobald Sie [die Datenquellen mit Azure Sentinel verbunden](quickstart-onboard.md) haben, sollten Sie dafür sorgen, dass Warnungen ausgelöst werden, wenn etwas Verdächtiges geschieht. Aus diesem Grund bietet Azure Sentinel integrierte Standardvorlagen, die Sie beim Erstellen von Regeln zur Bedrohungserkennung unterstützen. Diese Vorlagen wurden von den Sicherheitsexperten und Analysten von Microsoft basierend auf bekannten Bedrohungen, häufig verwendeten Angriffsvektoren und Eskalationsketten verdächtiger Aktivitäten entworfen. Mit den anhand dieser Vorlagen erstellten Regeln wird die gesamte Umgebung automatisch nach allen Aktivitäten durchsucht, die verdächtig erscheinen. Viele Vorlagen können so angepasst werden, dass gemäß Ihren Anforderungen nach Aktivitäten gesucht oder gefiltert werden kann. Mit den durch diese Regeln generierten Warnungen werden Incidents erstellt, die Sie in Ihrer Umgebung zuweisen und untersuchen können.

Dieses Tutorial unterstützt Sie bei der Erkennung von Bedrohungen mit Azure Sentinel:

> [!div class="checklist"]
> * Verwenden von standardmäßigen Bedrohungserkennungen
> * Automatisieren der Reaktionen auf Bedrohungen

## <a name="about-out-of-the-box-detections"></a>Informationen zu standardmäßig verfügbaren Erkennungen

Navigieren Sie zum Anzeigen aller standardmäßig verfügbaren Erkennungen zu **Analytics** und dann zu **Regelvorlagen**. Diese Registerkarte enthält alle integrierten Azure Sentinel-Regeln.

   :::image type="content" source="media/tutorial-detect-built-in/view-oob-detections.png" alt-text="Verwenden integrierter Erkennungen für die Suche nach Bedrohungen mit Azure Sentinel":::

In den folgenden Abschnitten werden die Typen der verfügbaren Standardvorlagen beschrieben:

### <a name="microsoft-security"></a>Microsoft-Sicherheit

Mit Microsoft-Sicherheitsvorlagen werden aus den Warnungen, die in anderen Microsoft-Sicherheitslösungen generiert werden, in Echtzeit Azure Sentinel-Incidents erstellt. Sie können Microsoft-Sicherheitsregeln als Vorlage verwenden, um neue Regeln mit ähnlicher Logik zu erstellen.

Weitere Informationen zu Sicherheitsregeln finden Sie unter [Automatisches Erstellen von Incidents aus Microsoft-Sicherheitswarnungen](create-incidents-from-alerts.md).

### <a name="fusion"></a>Fusion

Basierend auf der Fusion-Technologie werden für die erweiterte Erkennung mehrstufiger Angriffe in Azure Sentinel skalierbare Machine Learning-Algorithmen verwendet, mit denen viele Low-Fidelity-Warnungen und -Ereignisse über mehrere Produkte hinweg zu verwertbaren High-Fidelity-Incidents korreliert werden können. Fusion ist standardmäßig aktiviert. Da die Logik ausgeblendet und daher nicht anpassbar ist, können Sie nur eine Regel mit dieser Vorlage erstellen.

> [!IMPORTANT]
> Einige der Erkennungsfunktionen in der Fusion-Regelvorlage befinden sich aktuell in der **VORSCHAU**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.
>
> Unter [Erweiterte Erkennung von mehrstufigen Angriffen in Azure Sentinel](fusion.md) erfahren Sie, welche Erkennungsfunkionen als Vorschau verfügbar sind.

### <a name="machine-learning-behavioral-analytics"></a>Machine Learning Behavioral Analytics

Da diese Vorlagen auf proprietären Machine Learning-Algorithmen von Microsoft basieren, ist die interne Logik in Bezug auf die Funktionsweise und den Zeitpunkt der Ausführung für Sie nicht sichtbar. Da die Logik ausgeblendet und daher nicht anpassbar ist, können Sie mit jeder Vorlage dieses Typs nur eine Regel erstellen.

> [!IMPORTANT]
> - Die Regelvorlagen für Verhaltensanalysen mit maschinellem Lernen befinden sich aktuell in der **VORSCHAU**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.
>
> - Durch Erstellen und Aktivieren von Regeln, die auf den Vorlagen für die ML-Verhaltensanalyse basieren, **erteilen Sie Microsoft die Berechtigung, erfasste Daten nach Bedarf für die Verarbeitung durch die Machine Learning-Engines und -Modelle außerhalb der geografischen Region Ihres Azure Sentinel-Arbeitsbereichs zu kopieren**.

### <a name="scheduled"></a>Geplant

Geplante Analyseregeln basieren auf integrierten Abfragen, die von Microsoft-Sicherheitsexperten geschrieben wurden. Sie sehen die Abfragelogik und können Änderungen daran vornehmen. Sie können die Vorlage für geplante Regeln verwenden und die Abfragelogik- und Planungseinstellungen anpassen, um neue Regeln zu erstellen.

> [!TIP]
> Zu den Planungsoptionen für Regeln gehört es, die Regel so zu konfigurieren, dass sie jede angegebene Anzahl von Minuten, Stunden oder Tagen ausgeführt wird, wobei die Zeit beginnt, wenn Sie die Regel aktivieren.
>
> Es wird empfohlen, beim Aktivieren einer neuen oder bearbeiteten Analyseregel umsichtig vorzugehen, um sicherzustellen, dass die Regeln den neuen Stapel von Vorfällen rechtzeitig erhalten. Beispielsweise können Sie eine Regel synchron mit dem Arbeitszeitbeginn Ihrer SOC-Analysten ausführen, und die Regeln zu diesem Zeitpunkt aktivieren.
>

## <a name="use-out-of-the-box-detections"></a>Verwenden von standardmäßigen Erkennungen

1. Um eine integrierte Vorlage zu verwenden, klicken Sie auf den Vorlagennamen, und klicken Sie dann im Detailbereich auf die Schaltfläche **Regel erstellen**, um auf der Grundlage dieser Vorlage eine neue aktive Regel zu erstellen. Jede Vorlage enthält eine Liste der erforderlichen Datenquellen. Wenn Sie die Vorlage öffnen, werden die Datenquellen automatisch auf Verfügbarkeit überprüft. Wenn ein Verfügbarkeitsproblem vorliegt, ist möglicherweise die Schaltfläche **Regel erstellen** deaktiviert, oder es wird eine entsprechende Warnung angezeigt.

    :::image type="content" source="media/tutorial-detect-built-in/use-built-in-template.png" alt-text="Bereich für Vorschau der Erkennungsregel":::

1. Durch Klicken auf die Schaltfläche **Regel erstellen** wird je nach ausgewählter Vorlage der Assistent zum Erstellen von Regeln geöffnet. Alle Details werden automatisch ausgefüllt, und Sie können mit der Vorlage **Geplant** oder **Microsoft-Sicherheit** die Logik und andere Regeleinstellungen entsprechend Ihren speziellen Anforderungen anpassen. Sie können diesen Vorgang wiederholen, um zusätzliche Regeln basierend auf der integrierten Vorlage zu erstellen. Nach dem Ausführen aller Schritte im Assistenten zum Erstellen von Regeln haben Sie die Erstellung einer Regel basierend auf der Vorlage abgeschlossen. Die neuen Regeln werden auf der Registerkarte **Aktive Regeln** angezeigt.

    Weitere Informationen zum Anpassen der Regeln im Regelerstellungs-Assistenten finden Sie unter [Tutorial: Erstellen benutzerdefinierter Analyseregeln zum Erkennen von Bedrohungen](tutorial-detect-threats-custom.md)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Einführung erhalten, wie Bedrohungen mithilfe von Azure Sentinel erkannt werden.

Informationen zum Automatisieren der Reaktionen auf Bedrohungen finden Sie im Tutorial [Einrichten automatisierter Reaktionen auf Bedrohungen in Azure Sentinel](tutorial-respond-threats-playbook.md).
