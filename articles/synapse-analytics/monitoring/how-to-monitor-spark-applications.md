---
title: Überwachen von Apache Spark-Anwendungen
description: Verwenden Sie Azure Synapse Studio, um Ihre Apache Spark-Anwendungen zu überwachen.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: f231693fdcf3519e29eed38e47db52d92acc00fa
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427298"
---
# <a name="use-the-azure-synapse-studio-preview-to-monitor-your-apache-spark-applications"></a>Verwenden Sie Azure Synapse Studio (Vorschau), um Ihre Apache Spark-Anwendungen zu überwachen.

Mit Azure Synapse Analytics können Sie mithilfe von Spark Notebooks, Aufträge und andere Arten von Anwendungen in ihren Spark-Pools in Ihrem Arbeitsbereich ausführen.

In diesem Artikel wird erläutert, wie Sie Ihre Spark-Anwendungen überwachen, was es Ihnen gestattet, den aktuellen Status, die aktuellen Probleme und den Fortschritt im Auge zu behalten.

## <a name="accessing-the-list-of-spark-applications"></a>Zugreifen auf die Liste der Spark-Anwendungen

Um die Liste der Spark-Anwendungen in Ihrem Arbeitsbereich anzuzeigen, [öffnen Sie zunächst Azure Synapse Studio](https://web.azuresynapse.net/), und wählen Sie Ihren Arbeitsbereich aus.

  > [!div class="mx-imgBorder"]
  > ![Anmelden beim Arbeitsbereich](./media/common/login-workspace.png)

Nachdem Sie Ihren Arbeitsbereich geöffnet haben, wählen Sie links den Abschnitt **Überwachen** aus.

  > [!div class="mx-imgBorder"]
  > ![Auswählen des Hubs „Überwachen“](./media/common/left-nav.png)

Wählen Sie **Spark-Anwendungen** aus, um die Liste der Spark-Anwendungen anzuzeigen.

  > [!div class="mx-imgBorder"]
  > ![Auswählen von „Spark-Anwendungen“](./media/how-to-monitor-spark-applications/monitor-hub-nav-sparkapplications.png)

## <a name="filtering-your-spark-applications"></a>Filtern Ihrer Spark-Anwendungen

Sie können die Liste der Spark-Anwendungen nach den für Sie interessanten Anwendungen filtern. Mit den Filtern am oberen Rand des Bildschirms können Sie ein Feld angeben, nach dem Sie filtern möchten.

Beispielsweise können Sie die Ansicht so filtern, dass nur die Spark-Anwendungen angezeigt werden, die den Namen „Vertrieb“ (sales) enthalten:

  > [!div class="mx-imgBorder"]
  > ![Filterschaltfläche](./media/common/filter-button.png)

  > [!div class="mx-imgBorder"]
  > ![Beispielfilter](./media/how-to-monitor-spark-applications/filter-example.png)

## <a name="viewing-details-about-a-specific-spark-application"></a>Anzeigen von Details zu einer bestimmten Spark-Anwendung

Um die Details zu einer Ihrer Spark-Anwendungen anzuzeigen, wählen Sie die Spark-Anwendung aus, und zeigen Sie die Details an. Wenn die Spark-Anwendung noch ausgeführt wird, können Sie den Fortschritt überwachen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Überwachen von Pipelineausführungen finden Sie im Artikel [Überwachen von Pipelineausführungen mit Azure Synapse Studio](how-to-monitor-pipeline-runs.md).  
