---
title: 'Schnellstart: Erste Schritte zum Erstellen eines Synapse-Arbeitsbereichs'
description: In diesem Tutorial erfahren Sie, wie Sie einen Synapse-Arbeitsbereich, einen dedizierten SQL-Pool und einen serverlosen Apache Spark-Pool erstellen.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 10/07/2020
ms.openlocfilehash: 862d2a93058c63dbfad1db49346edcbfe3c02ad1
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2020
ms.locfileid: "94592446"
---
# <a name="creating-a-synapse-workspace"></a>Erstellen eines Synapse-Arbeitsbereichs

In diesem Tutorial erfahren Sie, wie Sie einen Synapse-Arbeitsbereich, einen dedizierten SQL-Pool und einen serverlosen Apache Spark-Pool erstellen. 

## <a name="prerequisites"></a>Voraussetzungen

Um die Schritte dieses Tutorials ausführen zu können, benötigen Sie Zugriff auf eine Ressourcengruppe, für die Ihnen die Rolle **Besitzer** zugewiesen wurde. Erstellen Sie den Synapse-Arbeitsbereich in dieser Ressourcengruppe.

## <a name="create-a-synapse-workspace-in-the-azure-portal"></a>Erstellen eines Synapse-Arbeitsbereichs im Azure-Portal

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und suchen Sie oben nach **Synapse**.
1. Wählen Sie in den Suchergebnissen unter **Dienste** den Eintrag **Azure Synapse Analytics (Arbeitsbereichsvorschau)** aus.
1. Wählen Sie **Hinzufügen** aus, um einen Arbeitsbereich zu erstellen.
1. Geben Sie unter **Grundlagen** die bevorzugten Werte für **Abonnement**, **Ressourcengruppe** und **Region** ein, und wählen Sie dann einen Arbeitsbereichsnamen aus. In diesem Tutorial wird **myworkspace** verwendet.
1. Navigieren Sie zu **Data Lake Storage Gen 2 auswählen**. 
1. Klicken Sie auf **Neu erstellen**, und geben Sie den Namen **contosolake** ein.
1. Klicken Sie auf **Dateisystem**, und nennen Sie es **Benutzer**. Dadurch wird ein Container namens **Benutzer** erstellt.
1. Der Arbeitsbereich verwendet dieses Speicherkonto als primäres Speicherkonto für Spark-Tabellen und Spark-Anwendungsprotokolle.
1. Wählen Sie **Bewerten + erstellen** > **Erstellen** aus. Ihr Arbeitsbereich steht nach wenigen Minuten zur Verfügung.

## <a name="open-synapse-studio"></a>Öffnen von Synapse Studio

Nachdem Ihr Azure Synapse-Arbeitsbereich erstellt wurde, haben Sie zwei Möglichkeiten zum Öffnen von Synapse Studio:

* Öffnen Sie Ihren Synapse-Arbeitsbereich im [Azure-Portal](https://portal.azure.com). Wählen Sie oben im Abschnitt **Übersicht** die Option **Synapse Studio starten** aus.
* Navigieren Sie zu `https://web.azuresynapse.net`, und melden Sie sich bei Ihrem Arbeitsbereich an.

## <a name="create-a-dedicated-sql-pool"></a>Erstellen eines dedizierten SQL-Pools

1. Wählen Sie in Synapse Studio im linken Bereich **Verwalten** > **SQL-Pools** aus.
1. Wählen Sie **Neu** aus.
1. Wählen Sie unter **Name des SQL-Pools** den Namen **SQLPOOL1** aus.
1. Wählen Sie für **Leistungsebene** die Option **DW100C** aus.
1. Wählen Sie **Bewerten + erstellen** > **Erstellen** aus. Ihr dedizierter SQL-Pool steht in wenigen Minuten zur Verfügung. Ihr dedizierter SQL-Pool ist einer dedizierten SQL-Pool-Datenbank zugeordnet, die auch als **SQLPOOL1** bezeichnet wird.

Ein dedizierter SQL-Pool nutzt abrechenbare Ressourcen, solange er aktiv ist. Sie können den Pool später anhalten, um die Kosten zu senken.

## <a name="create-a-serverless-apache-spark-pool"></a>Erstellen eines serverlosen Apache Spark-Pools

1. Wählen Sie in Synapse Studio im linken Bereich die Option **Verwalten** > **Apache Spark-Pools** aus.
1. Wählen Sie **Neu** aus. 
1. Geben Sie unter **Name des Apache Spark-Pools** den Namen **Spark1** ein.
1. Geben Sie unter **Knotengröße** die Größe **Klein** ein.
1. Legen Sie unter **Knotenanzahl** den Mindestwert auf 3 und den Höchstwert auf 3 fest.
1. Wählen Sie **Bewerten + erstellen** > **Erstellen** aus. Ihr Apache Spark-Pool steht in wenigen Sekunden zur Verfügung.

Der Spark-Pool informiert Azure Synapse, wie viele Spark-Ressourcen verwendet werden sollen. Sie zahlen nur für die Ressourcen, die Sie verwenden. Wenn Sie die Verwendung des Pools aktiv beenden, erfolgt automatisch ein Timeout für die Ressourcen, und sie werden wiederverwendet.

## <a name="the-serverless-sql-pool"></a>Serverloser SQL-Pool

Jeder Arbeitsbereich verfügt über einen vordefinierten Pool mit dem Namen **Integriert**. Dieser Pool kann nicht gelöscht werden. Der serverlose SQL-Pool ermöglicht es Ihnen, mit SQL zu arbeiten, ohne dass Sie einen serverlosen SQL-Pool in Azure Synapse erstellen oder sich über seine Verwaltung Gedanken machen müssen. Im Gegensatz zu dedizierten SQL-Pools basiert die Abrechnung für einen serverlosen SQL-Pool auf der Menge der Daten, die zum Ausführen der Abfrage durchsucht werden, und nicht auf der Anzahl von Ressourcen, die zum Ausführen der Abfrage verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Analysieren mithilfe eines dedizierten SQL-Pools](get-started-analyze-sql-pool.md)
