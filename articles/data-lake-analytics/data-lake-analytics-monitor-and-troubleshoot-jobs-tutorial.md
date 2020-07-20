---
title: Überwachen von Azure Data Lake Analytics – Azure-Portal
description: In diesem Artikel erfahren Sie, wie Sie Probleme mit Azure Data Lake Analytics-Aufträgen über das Azure-Portal behandeln.
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.topic: troubleshooting
ms.date: 12/05/2016
ms.openlocfilehash: 08e14765b2b70f1e2567bd6a9cd2d67ee9aed83b
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86121281"
---
# <a name="monitor-jobs-in-azure-data-lake-analytics-using-the-azure-portal"></a>Überwachen von Aufträgen in Azure Data Lake Analytics über das Azure-Portal

## <a name="to-see-all-the-jobs"></a>So zeigen Sie alle Aufträge an

1. Klicken Sie im Azure-Portal in der oberen linken Ecke auf **Microsoft Azure** .

2. Klicken Sie auf die Kachel mit Ihrem Data Lake Analytics-Kontonamen.  Die Zusammenfassung des Auftrags wird auf der Kachel **Auftragsverwaltung** gezeigt.

   ![Azure Data Lake Analytics – Auftragsverwaltung](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    Auf der Kachel „Auftragsverwaltung“ wird der Auftragsstatus gezeigt. Sie sehen, dass ein Auftrag nicht erfolgreich war.
3. Klicken Sie auf die Kachel **Auftragsverwaltung** , um die Aufträge anzuzeigen. Die Aufträge haben die Kategorien **Wird ausgeführt**, **In Warteschlange** und **Beendet**. Der fehlerhafte Auftrag sollte im Abschnitt **Beendet** angezeigt werden. Es muss der erste in der Liste sein. Wenn viele Aufträge vorhanden sind, können Sie auf **Filter** klicken, um Aufträge einfacher zu finden.

   ![Azure Data Lake Analytics – Filtern von Aufträgen](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)

4. Klicken Sie in der Liste auf den fehlerhaften Auftrag, um die Auftragsdetails zu öffnen:

   ![Azure Data Lake Analytics – Fehlerhafter Auftrag](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    Beachten Sie die Schaltfläche **Erneut senden** . Nachdem Sie das Problem behoben haben, können Sie den Auftrag erneut senden.

5. Klicken Sie auf den markierten Teil im vorigen Bildschirmfoto, um die Fehlerdetails zu öffnen.  Die Ausgabe sollte etwa so aussehen:

   ![Azure Data Lake Analytics – Details zum fehlerhaften Auftrag](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

   Sie werden informiert, dass der Quellordner nicht gefunden wurde.

6. Klicken Sie auf **Skript duplizieren**.

7. Ändern Sie den Pfad **FROM** in Folgendes: `/Samples/Data/SearchLog.tsv`

8. Klicken Sie auf **Auftrag senden**.

## <a name="next-steps"></a>Nächste Schritte

* [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md)
* [Erste Schritte mit Azure Data Lake Analytics mithilfe von Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md)
