---
title: Überwachen von Apache Spark-Anwendungen
description: Verwenden Sie Azure Synapse Studio, um Ihre Apache Spark-Anwendungen zu überwachen.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: e646f1bc1a25f58dd54437cbd77750479ac9ae99
ms.sourcegitcommit: f684589322633f1a0fafb627a03498b148b0d521
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2020
ms.locfileid: "85970897"
---
# <a name="use-the-azure-synapse-studio-preview-to-monitor-your-apache-spark-applications"></a>Verwenden Sie Azure Synapse Studio (Vorschau), um Ihre Apache Spark-Anwendungen zu überwachen.

Mit Azure Synapse Analytics können Sie mithilfe von Spark Notebooks, Aufträge und andere Arten von Anwendungen in ihren Spark-Pools in Ihrem Arbeitsbereich ausführen.

In diesem Artikel wird erläutert, wie Sie Ihre Apache Spark-Anwendungen überwachen, was es Ihnen gestattet, den aktuellen Status, die aktuellen Probleme und den Fortschritt im Auge zu behalten.

## <a name="accessing-the-list-of-apache-spark-applications"></a>Zugreifen auf die Liste der Apache Spark-Anwendungen

Um die Liste der Apache Spark-Anwendungen in Ihrem Arbeitsbereich anzuzeigen, [öffnen Sie zunächst Azure Synapse Studio](https://web.azuresynapse.net/), und wählen Sie Ihren Arbeitsbereich aus.

![Anmelden beim Arbeitsbereich](./media/common/login-workspace.png)

Nachdem Sie Ihren Arbeitsbereich geöffnet haben, wählen Sie links den Abschnitt **Überwachen** aus.

![Auswählen des Hubs „Überwachen“](./media/common/left-nav.png)

Wählen Sie **Apache Spark-Anwendungen** aus, um die Liste der Apache Spark-Anwendungen anzuzeigen.

 ![Auswählen von Spark-Anwendungen](./media/how-to-monitor-spark-applications/monitor-hub-nav-sparkapplications.png)

## <a name="filtering-your-apache-spark-applications"></a>Filtern Ihrer Apache Spark-Anwendungen

Sie können die Liste der Apache Spark-Anwendungen nach den für Sie interessanten Anwendungen filtern. Mit den Filtern am oberen Rand des Bildschirms können Sie ein Feld angeben, nach dem Sie filtern möchten.

Beispielsweise können Sie die Ansicht so filtern, dass nur die Apache Spark-Anwendungen angezeigt werden, die den Namen „Vertrieb“ (sales) enthalten:

![Filterschaltfläche](./media/common/filter-button.png)

![Beispielfilter](./media/how-to-monitor-spark-applications/filter-example.png)

## <a name="viewing-details-about-a-specific-apache-spark-application"></a>Anzeigen von Details zu einer bestimmten Apache Spark-Anwendung

Um die Details zu einer Ihrer Apache Spark-Anwendungen anzuzeigen, wählen Sie die Apache Spark-Anwendung aus, und zeigen Sie die Details an. Wenn die Apache Spark-Anwendung noch ausgeführt wird, können Sie den Fortschritt überwachen. [Weitere Informationen](apache-spark-applications.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Überwachen von Pipelineausführungen finden Sie im Artikel [Überwachen von Pipelineausführungen mit Azure Synapse Studio](how-to-monitor-pipeline-runs.md). 

Weitere Informationen zum Debuggen von Apache Spark-Anwendungen finden Sie im Artikel [Überwachen von Apache Spark-Anwendungen in Azure Synapse Studio](apache-spark-applications.md).