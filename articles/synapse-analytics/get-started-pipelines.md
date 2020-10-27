---
title: 'Tutorial: Erste Schritte zum Orchestrieren mit Pipelines'
description: In diesem Tutorial erfahren Sie, wie Pipelines und Aktivitäten mithilfe von Synapse Studio orchestriert werden.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: pipeline
ms.topic: tutorial
ms.date: 10/16/2020
ms.openlocfilehash: 42d2ac6cf6592f8e22b0a66aee84c3436d466572
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92329882"
---
# <a name="orchestrate-with-pipelines"></a>Orchestrieren mit Pipelines

In diesem Tutorial erfahren Sie, wie Pipelines und Aktivitäten mithilfe von Synapse Studio orchestriert werden. 

## <a name="overview"></a>Übersicht

Sie können eine Vielzahl verschiedener Aufgaben in Azure Synapse orchestrieren.

1. Navigieren Sie in Synapse Studio zum Hub **Integrieren** .
1. Wählen Sie **+**  > **Pipeline** aus, um eine neue Pipeline zu erstellen.
1. Navigieren Sie zum Hub **Entwickeln** , und wählen Sie eins der zuvor erstellten Notebooks aus.
1. Ziehen Sie dieses Notebook in die Pipeline. ( **Hinweis** : Schritt zum Importieren von Modulen im Notebook, wie in [diesem Dokument](https://docs.microsoft.com/azure/synapse-analytics/spark/synapse-spark-sql-pool-import-export#transfer-data-to-or-from-a-sql-pool-attached-with-the-workspace) angegeben. Erforderlich bei der Ausführung über die Pipeline.)
1. Wählen Sie in der Pipeline **Trigger hinzufügen** > **Neu/Bearbeiten** aus.
1. Wählen Sie unter **Choose trigger** (Trigger auswählen) die Option **Neu** aus, und legen Sie **Wiederholung** auf „1 Stunde“ fest.
1. Klicken Sie auf **OK** . 
1. Wählen Sie **Alle veröffentlichen** .
1. Wählen Sie zum sofortigen Ausführen der Pipeline, ohne den Ablauf der Stunde abzuwarten, die Option **Trigger hinzufügen** > **Jetzt auslösen** aus.



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Visualisieren von Daten mit Power BI](get-started-visualize-power-bi.md)
                                 
