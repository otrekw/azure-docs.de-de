---
title: Überwachen von Pipelineausführungen mithilfe von Synapse Studio
description: Verwenden Sie Synapse Studio, um Ihre Pipelineausführungen im Arbeitsbereich zu überwachen.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 10/27/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: 61d860def7209908e65e9456a4bcde87eed522fc
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92746377"
---
# <a name="use-synapse-studio-to-monitor-your-workspace-pipeline-runs"></a>Verwenden von Synapse Studio zum Überwachen Ihrer Pipelineausführungen im Arbeitsbereich

Mit Azure Synapse Analytics können Sie komplexe Pipelines erstellen, mit denen Sie Ihre Datenverschiebung, Datentransformation und Computeaktivitäten innerhalb Ihrer Lösung automatisieren und integrieren können. Sie können diese Pipelines mithilfe von Synapse Studio (Vorschau) erstellen und überwachen.

In diesem Artikel wird erläutert, wie Sie Ihre Pipelineausführungen überwachen, was es Ihnen gestattet, den aktuellen Status, die aktuellen Probleme und den Fortschritt ihrer Pipelines im Auge zu behalten.

## <a name="access-pipeline-runs-list"></a>Zugreifen auf die Liste der Pipelineausführungen

Um die Liste der Pipelineausführungen in Ihrem Arbeitsbereich anzuzeigen, [öffnen Sie zunächst Synapse Studio](https://web.azuresynapse.net/), und wählen Sie Ihren Arbeitsbereich aus.

![Anmelden beim Arbeitsbereich](./media/common/login-workspace.png)

Nachdem Sie Ihren Arbeitsbereich geöffnet haben, wählen Sie links den Abschnitt **Überwachen** aus.

![Auswählen des Hubs „Überwachen“](./media/common/left-nav.png)

Wählen Sie **Pipelineausführungen** aus, um die Liste der Pipelineausführungen anzuzeigen.

![Auswählen von Pipelineausführungen](./media/how-to-monitor-pipeline-runs/monitor-hub-nav-pipelineruns.png)

## <a name="filter-your-pipeline-runs"></a>Filtern Ihrer Pipelineausführungen

Sie können die Liste der Pipelineausführungen nach den für Sie interessanten Ausführungen filtern. Mit den Filtern am oberen Rand des Bildschirms können Sie ein Feld angeben, nach dem Sie filtern möchten.

Beispielsweise können Sie die Ansicht so filtern, dass nur die Pipelineausführungen für die Pipeline namens „Feiertag“ (holiday) angezeigt werden:

![Filterschaltfläche](./media/common/filter-button.png)

![Beispielfilter](./media/how-to-monitor-pipeline-runs/filter-example.png)

## <a name="view-details-about-a-specific-pipeline-run"></a>Anzeigen von Details zu einer bestimmten Pipelineausführung

Um Details zu Ihrer Pipelineausführung anzuzeigen, wählen Sie die Pipelineausführung aus. Zeigen Sie dann mit der Pipelineausführung verknüpften Aktivitätsausführungen an. Wenn die Pipeline noch ausgeführt wird, können Sie den Fortschritt überwachen. 
  
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Überwachen von Anwendungen finden Sie im Artikel [Überwachen von Apache Spark-Anwendungen](how-to-monitor-spark-applications.md). 
