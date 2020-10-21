---
title: 'Tutorial: Erste Schritte zum Erstellen eines Synapse-Arbeitsbereichs'
description: In diesem Tutorial erfahren Sie, wie Sie einen Synapse-Arbeitsbereich, einen SQL-Pool und einen Apache Spark-Pool erstellen.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 10/07/2020
ms.openlocfilehash: d74c3e42317b954a510f3276db38c0dcdf5e2362
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91850378"
---
# <a name="creating-a-synapse-workspace"></a>Erstellen eines Synapse-Arbeitsbereichs

In diesem Tutorial erfahren Sie, wie Sie einen Synapse-Arbeitsbereich, einen SQL-Pool und einen Apache Spark-Pool erstellen. 

## <a name="prerequisites"></a>Voraussetzungen

Um alle Schritte dieses Tutorials ausführen zu können, benötigen Sie Zugriff auf eine Ressourcengruppe, für die Ihnen die Rolle **Besitzer** zugewiesen wurde. Erstellen Sie den Synapse-Arbeitsbereich in dieser Ressourcengruppe.

## <a name="create-a-synapse-workspace-in-the-azure-portal"></a>Erstellen eines Synapse-Arbeitsbereichs im Azure-Portal

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und suchen Sie oben nach **Synapse**.
1. Wählen Sie in den Suchergebnissen unter **Dienste** den Eintrag **Azure Synapse Analytics (Arbeitsbereichsvorschau)** aus.
1. Wählen Sie **Hinzufügen** aus, um einen Arbeitsbereich zu erstellen.
1. Geben Sie unter **Grundlagen** die bevorzugten Werte für **Abonnement**, **Ressourcengruppe** und **Region** ein, und wählen Sie dann einen Arbeitsbereichsnamen aus. In diesem Tutorial wird **myworkspace** verwendet.
1. Zum Erstellen eines Arbeitsbereichs benötigen Sie ein ADLSGEN2-Konto sowie einen Container in diesem Konto. Am einfachsten ist es, ein solches Konto neu zu erstellen. Wenn Sie ein vorhandenes Konto verwenden möchten, müssen Sie einige zusätzliche Konfigurationsschritte ausführen. 
    1. Der Synapse-Arbeitsbereich verwendet diesen Container als Standardspeicherort zum Speichern von Spark-Protokollen und -Daten für Spark-Tabellen.
1. OPTION 1: Erstellen eines neuen ADLSGEN2-Kontos 
    1. Navigieren Sie zu **Data Lake Storage Gen 2 auswählen**. 
    1. Klicken Sie auf **Neu erstellen**, und geben Sie den Namen **contosolake** ein.
    1. Klicken Sie auf **Dateisystem**, und nennen Sie es **Benutzer**. Dadurch wird ein Container namens **Benutzer** erstellt.
1. OPTION 2: Verwenden eines vorhandenen ADLSGEN2-Kontos Anweisungen finden Sie unter **Vorbereiten eines ADLSGEN2-Speicherkontos** am Ende dieses Dokuments.
1. Ihr Azure Synapse-Arbeitsbereich verwendet dieses Speicherkonto als „primäres“ Speicherkonto und den Container zum Speichern von Arbeitsbereichsdaten. Im Arbeitsbereich werden Daten in Apache Spark-Tabellen gespeichert. Die Spark-Anwendungsprotokolle werden im Ordner **/synapse/workspacename** gespeichert.
1. Wählen Sie **Bewerten + erstellen** > **Erstellen** aus. Ihr Arbeitsbereich steht nach wenigen Minuten zur Verfügung.

## <a name="open-synapse-studio"></a>Öffnen von Synapse Studio

Nachdem Ihr Azure Synapse-Arbeitsbereich erstellt wurde, haben Sie zwei Möglichkeiten zum Öffnen von Synapse Studio:

* Öffnen Sie Ihren Synapse-Arbeitsbereich im [Azure-Portal](https://portal.azure.com). Wählen Sie oben im Abschnitt **Übersicht** die Option **Synapse Studio starten** aus.
* Navigieren Sie zu `https://web.azuresynapse.net`, und melden Sie sich bei Ihrem Arbeitsbereich an.

## <a name="create-a-sql-pool"></a>Erstellen eines SQL-Pools

1. Wählen Sie in Synapse Studio im linken Bereich **Verwalten** > **SQL-Pools** aus.
1. Wählen Sie **Neu** aus, und geben Sie diese Einstellungen ein:

    |Einstellung | Vorgeschlagener Wert | 
    |---|---|---|
    |**Name des SQL-Pools**| **SQLDB1**|
    |**Leistungsstufe**|**DW100C**|
    |||

1. Wählen Sie **Bewerten + erstellen** > **Erstellen** aus. Ihr SQL-Pool steht in wenigen Minuten zur Verfügung. Ihr SQL-Pool ist einer SQL-Pool-Datenbank zugeordnet, die auch als **SQLDB1** bezeichnet wird.

Ein SQL-Pool nutzt abrechenbare Ressourcen, solange er aktiv ist. Sie können den Pool später anhalten, um die Kosten zu senken.

## <a name="create-an-apache-spark-pool"></a>Erstellen eines Apache Spark-Pools

1. Wählen Sie in Synapse Studio im linken Bereich die Option **Verwalten** > **Apache Spark-Pools** aus.
1. Wählen Sie **Neu** aus, und geben Sie diese Einstellungen ein:

    |Einstellung | Vorgeschlagener Wert | 
    |---|---|---|
    |**Name des Apache Spark-Pools**|**Spark1**
    |**Knotengröße**| **Klein**|
    |**Anzahl von Knoten**| Legen Sie den Mindestwert auf 3 und den Höchstwert auf 3 fest.|

1. Wählen Sie **Bewerten + erstellen** > **Erstellen** aus. Ihr Apache Spark-Pool steht in wenigen Sekunden zur Verfügung.

> [!NOTE]
> Trotz des Namens ist ein Apache Spark-Pool nicht mit einem SQL-Pool vergleichbar. Es handelt sich nur um ein paar grundlegende Metadaten, mit denen Sie den Azure Synapse-Arbeitsbereich darüber informieren, wie mit Spark interagiert werden soll.

Da es sich hierbei um Metadaten handelt, können Spark-Pools nicht gestartet oder beendet werden.

Wenn Sie eine Spark-Aktivität in Azure Synapse durchführen, geben Sie den zu verwendenden Spark-Pool an. Der Pool informiert Azure Synapse, wie viele Spark-Ressourcen verwendet werden sollen. Sie zahlen nur für die Ressourcen, die Sie verwenden. Wenn Sie die Verwendung des Pools aktiv beenden, erfolgt automatisch ein Timeout für die Ressourcen, und sie werden wiederverwendet.

> [!NOTE]
> Spark-Datenbanken werden unabhängig von Spark-Pools erstellt. Ein Arbeitsbereich verfügt immer über eine Spark-Datenbank mit dem Namen **default**. Sie können auch zusätzliche Spark-Datenbanken erstellen.

## <a name="the-sql-on-demand-pool"></a>Der SQL On-Demand-Pool

Jeder Arbeitsbereich verfügt über einen vordefinierten Pool mit dem Namen **SQL On-Demand**. Dieser Pool kann nicht gelöscht werden. Der Pool „SQL On-Demand“ ermöglicht es Ihnen, mit SQL zu arbeiten, ohne dass Sie einen SQL-Pool in Azure Synapse erstellen oder sich darüber Gedanken machen müssen.

Im Gegensatz zu den anderen Arten von Pools basiert die Abrechnung für SQL On-Demand auf der Menge der Daten, die zum Ausführen der Abfrage durchsucht werden, und nicht auf der Anzahl von Ressourcen, die zum Ausführen der Abfrage verwendet werden.

* SQL On-Demand verfügt über eigene SQL On-Demand-Datenbanken, die unabhängig von einem SQL On-Demand-Pool vorhanden sind.
* Ein Arbeitsbereich besitzt immer nur genau einen SQL On-Demand-Pool mit dem Namen **SQL On-Demand**.

## <a name="preparing-a-adlsgen2-storage-account"></a>Vorbereiten eines ADLSGEN2-Speicherkontos

### <a name="perform-the-following-steps-before-you-create-your-workspace"></a>Führen Sie die folgenden Schritte aus, BEVOR Sie den Arbeitsbereich erstellen:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
1. Navigieren Sie zu Ihrem vorhandenen Speicherkonto.
1. Wählen Sie im linken Bereich **Zugriffssteuerung (IAM)** aus. 
1. Weisen Sie die folgenden Rollen zu, bzw. stellen Sie sicher, dass sie bereits zugewiesen sind:
    * Weisen Sie sich selbst die Rolle **Besitzer** zu.
    * Weisen Sie sich selbst die Rolle **Besitzer von Speicherblobdaten** zu.
1. Wählen Sie im linken Bereich die Option **Container** aus, und erstellen Sie einen Container.
1. Sie können dem Container einen Namen geben. In diesem Dokument wird der Name **users** verwendet.
1. Übernehmen Sie die Standardeinstellung **Öffentliche Zugriffsebene**, und wählen Sie anschließend **Erstellen** aus.

### <a name="perform-the-following-steps-after-you-create-your-workspace"></a>Führen Sie die folgenden Schritte aus, NACHDEM Sie den Arbeitsbereich erstellt haben:

Konfigurieren Sie den Zugriff auf das Speicherkonto über den Arbeitsbereich. Verwaltete Identitäten für Ihren Azure Synapse-Arbeitsbereich verfügen unter Umständen bereits über Zugriff auf das Speicherkonto. Führen Sie die folgenden Schritte aus, um dies zu überprüfen:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) und dann das primäre Speicherkonto, das Sie für Ihren Arbeitsbereich ausgewählt haben.
1. Wählen Sie im linken Bereich **Zugriffssteuerung (IAM)** aus.
1. Weisen Sie die folgenden Rollen zu, bzw. stellen Sie sicher, dass sie bereits zugewiesen sind. Wir verwenden für die Identität und den Namen des Arbeitsbereichs den gleichen Namen.
    * Weisen Sie für die Rolle **Mitwirkender an Storage-Blobdaten** des Speicherkontos **myworkspace** als Identität des Arbeitsbereichs zu.
    * Weisen Sie **myworkspace** als Namen für den Arbeitsbereich zu.
1. Wählen Sie **Speichern** aus.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Analysieren mithilfe eines SQL-Pools](get-started-analyze-sql-pool.md)
