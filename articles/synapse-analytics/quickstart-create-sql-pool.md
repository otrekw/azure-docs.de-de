---
title: 'Schnellstart: Erstellen eines Synapse SQL-Pools'
description: Erstellen Sie anhand der Schritte in diesem Leitfaden einen neuen Synapse SQL-Pool für einen Synapse-Arbeitsbereich in Azure Synapse Analytics.
services: synapse-analytics
author: malvenko
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: josels
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: a50f203b8057dd4ea51eff1cf09fc97fb598cb3e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82096434"
---
# <a name="quickstart-create-a-synapse-sql-pool"></a>Schnellstart: Erstellen eines Synapse SQL-Pools

Azure Synapse Analytics beinhaltet verschiedene Analysemodule, mit denen Sie Ihre Daten erfassen, transformieren, modellieren, analysieren und bereitstellen können. Ein SQL-Pool bietet T-SQL-basierte Compute- und Speicherfunktionen. Nach der Erstellung eines SQL-Pools in Ihrem Synapse-Arbeitsbereich können Daten geladen, modelliert, verarbeitet und bereitgestellt werden, um Erkenntnisse zu gewinnen.

In dieser Schnellstartanleitung erfahren Sie, wie Sie über das Azure-Portal einen SQL-Pool in einem Synapse-Arbeitsbereich erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

[Synapse-Arbeitsbereich](quickstart-create-workspace.md)

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)

## <a name="create-new-sql-pool"></a>Erstellen eines neuen SQL-Pools

1. Klicken Sie in dem Synapse-Arbeitsbereich, in dem Sie den SQL-Pool erstellen möchten, auf der oberen Leiste auf den Befehl **New SQL pool** (Neuer SQL-Pool).
![Übersicht über den Synapse-Arbeitsbereich mit dem rot umrandeten Befehl zum Erstellen eines neuen SQL-Pools](media/quickstart-create-sql-pool/quickstart-create-sql-pool-01.png)
2. Geben Sie auf der Registerkarte **Grundeinstellungen** die folgenden Informationen ein:

    | Einstellung | Vorgeschlagener Wert | BESCHREIBUNG |
    | :------ | :-------------- | :---------- |
    | **Name des SQL-Pools** | Ein gültiger Name | Name für den SQL-Pool |
    | **Leistungsstufe** | DW100c | Legen Sie die kleinste Größe fest, um die Kosten für diesen Schnellstart zu senken. |
    ||||
  
    ![Erstellungsflow für den SQL-Pool: Registerkarte „Grundeinstellungen“](media/quickstart-create-sql-pool/quickstart-create-sql-pool-02.png)
    > [!IMPORTANT]
    > Beachten Sie, dass für die Namen, die von SQL-Pools verwendet werden können, bestimmte Einschränkungen gelten. Namen dürfen keine Sonderzeichen enthalten, höchstens 15 Zeichen lang sein und keine reservierten Wörter enthalten und müssen im Arbeitsbereich eindeutig sein.

3. Klicken Sie auf **Weiter: Zusätzliche Einstellungen**.
4. Wählen Sie **Keine** aus, um den SQL-Pool ohne Daten bereitzustellen. Übernehmen Sie die ausgewählte Standardsortierung.
![Erstellungsflow für den SQL-Pool: Registerkarte „Zusätzliche Einstellungen“](media/quickstart-create-sql-pool/quickstart-create-sql-pool-03.png)

5. Klicken Sie auf **Überprüfen + erstellen**.
6. Vergewissern Sie sich auf der Grundlage der vorherigen Angaben, dass die Details korrekt sind, und klicken Sie auf **Erstellen**.
![Flow für die SQL-Poolerstellung: Registerkarte zum Überprüfen der Einstellungen](media/quickstart-create-sql-pool/quickstart-create-sql-pool-04.png)

7. Der Flow für die Ressourcenbereitstellung wird nun gestartet.
 ![Flow für die SQL-Poolerstellung: Ressourcenbereitstellung](media/quickstart-create-sql-pool/quickstart-create-sql-pool-06.png)

8. Wenn Sie nach Abschluss der Bereitstellung zurück zum Arbeitsbereich navigieren, wird ein neuer Eintrag für den neu erstellten SQL-Pool angezeigt.
 ![Flow für die SQL-Poolerstellung: Ressourcenbereitstellung](media/quickstart-create-sql-pool/quickstart-create-sql-pool-07.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Führen Sie die folgenden Schritte aus, um den SQL-Pool aus dem Arbeitsbereich zu löschen.
> [!WARNING]
> Wenn Sie einen SQL-Pool löschen, werden die Analyse-Engine und die in der Datenbank des gelöschten SQL-Pools gespeicherten Daten aus dem Arbeitsbereich entfernt. Sie können keine Verbindung mehr mit dem SQL-Pool herstellen, und Abfragen, Pipelines und Notebooks, die aus diesem SQL-Pool lesen oder in diesen SQL-Pool schreiben, funktionieren nicht mehr.

Führen Sie die folgenden Schritte aus, wenn Sie den SQL-Pool löschen möchten:

1. Navigieren Sie auf dem Blatt des Arbeitsbereichs zum Blatt mit den SQL-Pools.
1. Wählen Sie den zu löschenden SQL-Pool aus (in diesem Fall **contosoedw**).
1. Wählen Sie ihn aus, und klicken Sie auf **Löschen**.
1. Bestätigen Sie den Löschvorgang, und klicken Sie auf die Schaltfläche **Löschen**.
 ![Übersicht über SQL-Pool: hervorgehobene Löschbestätigung](media/quickstart-create-sql-pool/quickstart-create-sql-pool-11.png)
1. Wenn der Vorgang erfolgreich abgeschlossen wurde, wird der SQL-Pool nicht mehr in den Arbeitsbereichsressourcen aufgeführt.

Nach der Erstellung des SQL-Pools ist er im Arbeitsbereich zum Laden von Daten, Verarbeiten von Datenströmen, Lesen aus dem Data Lake usw. verfügbar.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines Apache Spark-Pools (Vorschauversion) in Synapse Analytics mithilfe von Webtools](quickstart-apache-spark-notebook.md).
- Weitere Informationen finden Sie unter [Schnellstart: Erstellen eines neuen Apache Spark-Pools (Vorschauversion)](quickstart-create-apache-spark-pool.md).
