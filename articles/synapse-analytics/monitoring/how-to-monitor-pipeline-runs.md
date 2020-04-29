---
title: Überwachen von Pipelineausführungen mit Azure Synapse Studio (Vorschau)
description: Verwenden Sie Azure Synapse Studio, um Ihre Pipelineausführungen im Arbeitsbereich zu überwachen.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: 712dc62e29229f03dec12afdf18edbf55667dbdf
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427310"
---
# <a name="use-azure-synapse-studio-to-monitor-your-workspace-pipeline-runs"></a>Verwenden von Azure Synapse Studio zum Überwachen Ihrer Pipelineausführungen im Arbeitsbereich

Mit Azure Synapse Analytics können Sie komplexe Pipelines erstellen, mit denen Sie Ihre Datenverschiebung, Datentransformation und Compute-Aktivitäten innerhalb Ihrer Lösung automatisieren und orchestrieren können. Sie können diese Pipelines mithilfe von Azure Synapse Studio (Vorschau) erstellen und überwachen.

In diesem Artikel wird erläutert, wie Sie Ihre Pipelineausführungen überwachen, was es Ihnen gestattet, den aktuellen Status, die aktuellen Probleme und den Fortschritt ihrer Pipelines im Auge zu behalten.

## <a name="access-the-list-of-pipeline-runs"></a>Zugreifen auf die Liste der Pipelineausführungen

Um die Liste der Pipelineausführungen in Ihrem Arbeitsbereich anzuzeigen, [öffnen Sie zunächst Azure Synapse Studio](https://web.azuresynapse.net/), und wählen Sie Ihren Arbeitsbereich aus.

![Anmelden beim Arbeitsbereich](./media/common/login-workspace.png)

Nachdem Sie Ihren Arbeitsbereich geöffnet haben, wählen Sie links den Abschnitt **Überwachen** aus.

![Auswählen des Hubs „Überwachen“](./media/common/left-nav.png)

Wählen Sie **Pipelineausführungen** aus, um die Liste der Pipelineausführungen anzuzeigen.

![Auswählen von Pipelineausführungen](./media/how-to-monitor-pipeline-runs/monitor-hub-nav-pipelineruns.png)

## <a name="filtering-your-pipeline-runs"></a>Filtern Ihrer Pipelineausführungen

Sie können die Liste der Pipelineausführungen nach den für Sie interessanten Ausführungen filtern. Mit den Filtern am oberen Rand des Bildschirms können Sie ein Feld angeben, nach dem Sie filtern möchten.

Beispielsweise können Sie die Ansicht so filtern, dass nur die Pipelineausführungen für die Pipeline namens „Feiertag“ (holiday) angezeigt werden:

![Filterschaltfläche](./media/common/filter-button.png)

![Beispielfilter](./media/how-to-monitor-pipeline-runs/filter-example.png)

## <a name="viewing-details-about-a-specific-pipeline-run"></a>Anzeigen von Details zu einer bestimmten Pipelineausführung

Um Details zu Ihrer Pipelineausführung anzuzeigen, wählen Sie die Pipelineausführung aus. Zeigen Sie dann mit der Pipelineausführung verknüpften Aktivitätsausführungen an. Wenn die Pipeline noch ausgeführt wird, können Sie den Fortschritt überwachen. 
  
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Überwachen von Anwendungen finden Sie im Artikel [Überwachen von Apache Spark-Anwendungen](how-to-monitor-spark-applications.md). 
