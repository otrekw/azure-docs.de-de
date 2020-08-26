---
title: Untersuchen von Warnungen mit Azure Sentinel | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie standardmäßig verfügbare integrierte Azure-Bedrohungserkennungsvorlagen verwenden, mit denen Sie bei verdächtigen Vorkommnissen eine Benachrichtigung erhalten.
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
ms.date: 07/06/2020
ms.author: yelevin
ms.openlocfilehash: 5d73337c25c812363b7a542bf42372ca3baa10e8
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88605420"
---
# <a name="tutorial-detect-threats-out-of-the-box"></a>Tutorial: Standardmäßig verfügbare Erkennung von Bedrohungen


> [!IMPORTANT]
> Die standardmäßig verfügbare Erkennung von Bedrohungen befindet sich derzeit in der öffentlichen Vorschauphase.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sobald Sie [die Datenquellen mit Azure Sentinel verbunden](quickstart-onboard.md)  haben, möchten Sie gewarnt werden, wenn etwas Verdächtiges geschieht. Aus diesem Grund bietet Azure Sentinel integrierte Standardvorlagen, die Sie beim Erstellen von Regeln zur Bedrohungserkennung unterstützen. Diese Vorlagen wurden von den Sicherheitsexperten und Analysten von Microsoft basierend auf bekannten Bedrohungen, häufig verwendeten Angriffsvektoren und Eskalationsketten verdächtiger Aktivitäten entworfen. Mit den anhand dieser Vorlagen erstellten Regeln wird die gesamte Umgebung automatisch nach allen Aktivitäten durchsucht, die verdächtig erscheinen. Viele Vorlagen können so angepasst werden, dass gemäß Ihren Anforderungen nach Aktivitäten gesucht oder gefiltert werden kann. Mit den durch diese Regeln generierten Warnungen werden Incidents erstellt, die Sie in Ihrer Umgebung zuweisen und untersuchen können.

Dieses Tutorial unterstützt Sie bei der Erkennung von Bedrohungen mit Azure Sentinel:

> [!div class="checklist"]
> * Verwenden von standardmäßigen Bedrohungserkennungen
> * Automatisieren der Reaktionen auf Bedrohungen

## <a name="about-out-of-the-box-detections"></a>Informationen zu standardmäßig verfügbaren Erkennungen

Navigieren Sie zum Anzeigen aller standardmäßig verfügbaren Erkennungen zu **Analytics** und dann zu **Regelvorlagen**. Diese Registerkarte enthält alle integrierten Azure Sentinel-Regeln.

   :::image type="content" source="media/tutorial-detect-built-in/view-oob-detections.png" alt-text="Verwenden integrierter Erkennungen für die Suche nach Bedrohungen mit Azure Sentinel":::

Folgende Vorlagentypen sind verfügbar:

- **Microsoft-Sicherheit**
   
   Mit Microsoft-Sicherheitsvorlagen werden aus den Warnungen, die in anderen Microsoft-Sicherheitslösungen generiert werden, in Echtzeit Azure Sentinel-Incidents erstellt. Sie können Microsoft-Sicherheitsregeln als Vorlage verwenden, um neue Regeln mit ähnlicher Logik zu erstellen. Weitere Informationen zu Sicherheitsregeln finden Sie unter [Automatisches Erstellen von Incidents aus Microsoft-Sicherheitswarnungen](create-incidents-from-alerts.md).

- **Fusion** 

    Basierend auf der Fusion-Technologie werden für die erweiterte Erkennung mehrstufiger Angriffe in Azure Sentinel skalierbare Machine Learning-Algorithmen verwendet, mit denen viele Low-Fidelity-Warnungen und -Ereignisse über mehrere Produkte hinweg zu verwertbaren High-Fidelity-Incidents korreliert werden können. Fusion ist standardmäßig aktiviert. Da die Logik ausgeblendet und daher nicht anpassbar ist, können Sie nur eine Regel mit dieser Vorlage erstellen.

- **Machine Learning-Verhaltensanalysen**

    Da diese Vorlagen auf proprietären Machine Learning-Algorithmen von Microsoft basieren, ist die interne Logik in Bezug auf die Funktionsweise und den Zeitpunkt der Ausführung für Sie nicht sichtbar. Da die Logik ausgeblendet und daher nicht anpassbar ist, können Sie mit jeder Vorlage dieses Typs nur eine Regel erstellen.

- **Geplant**

    Geplante Analyseregeln basieren auf integrierten Abfragen, die von Microsoft-Sicherheitsexperten geschrieben wurden. Sie sehen die Abfragelogik und können Änderungen daran vornehmen. Sie können die Vorlage für geplante Regeln verwenden und die Abfragelogik- und Planungseinstellungen anpassen, um neue Regeln zu erstellen.

## <a name="use-out-of-the-box-detections"></a>Verwenden von standardmäßigen Erkennungen

1. Um eine integrierte Vorlage zu verwenden, klicken Sie auf den Vorlagennamen, und klicken Sie dann im Detailbereich auf die Schaltfläche **Regel erstellen**, um auf der Grundlage dieser Vorlage eine neue aktive Regel zu erstellen. Jede Vorlage enthält eine Liste der erforderlichen Datenquellen. Wenn Sie die Vorlage öffnen, werden die Datenquellen automatisch auf Verfügbarkeit überprüft. Wenn ein Verfügbarkeitsproblem vorliegt, ist möglicherweise die Schaltfläche **Regel erstellen** deaktiviert, oder es wird eine entsprechende Warnung angezeigt.
  
    :::image type="content" source="media/tutorial-detect-built-in/use-built-in-template.png" alt-text="Bereich für Vorschau der Erkennungsregel":::
 
1. Durch Klicken auf die Schaltfläche **Regel erstellen** wird je nach ausgewählter Vorlage der Assistent zum Erstellen von Regeln geöffnet. Alle Details werden automatisch ausgefüllt, und Sie können mit der Vorlage **Geplant** oder **Microsoft-Sicherheit** die Logik und andere Regeleinstellungen entsprechend Ihren speziellen Anforderungen anpassen. Sie können diesen Vorgang wiederholen, um zusätzliche Regeln basierend auf der integrierten Vorlage zu erstellen. Nach dem Ausführen aller Schritte im Assistenten zum Erstellen von Regeln haben Sie die Erstellung einer Regel basierend auf der Vorlage abgeschlossen. Die neuen Regeln werden auf der Registerkarte **Aktive Regeln** angezeigt.

    Weitere Informationen zum Anpassen der Regeln im Regelerstellungs-Assistenten finden Sie unter [Tutorial: Erstellen benutzerdefinierter Analyseregeln zum Erkennen von Bedrohungen](tutorial-detect-threats-custom.md)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Einführung erhalten, wie Bedrohungen mithilfe von Azure Sentinel erkannt werden. 

Informationen zum Automatisieren der Reaktionen auf Bedrohungen finden Sie im Tutorial [Einrichten automatisierter Reaktionen auf Bedrohungen in Azure Sentinel](tutorial-respond-threats-playbook.md).

