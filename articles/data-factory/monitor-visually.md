---
title: Visuelles Überwachen von Azure Data Factory
description: Erfahren Sie, wie Sie Azure Data Factorys visuell überwachen.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: 44a95430ece9c55bb088ae5b968c33691f25b4c5
ms.sourcegitcommit: 95269d1eae0f95d42d9de410f86e8e7b4fbbb049
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83860786"
---
# <a name="visually-monitor-azure-data-factory"></a>Visuelles Überwachen von Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Nachdem Sie eine Pipeline in Azure Data Factory erstellt und veröffentlicht haben, können Sie ihr einen Trigger zuordnen oder manuell eine Ad-hoc-Ausführung starten. Sie können Ihre gesamten Pipelineausführungen nativ über die Azure Data Factory-Benutzeroberfläche überwachen. Um die Benutzeroberfläche für die Überwachung zu öffnen, wählen Sie die Kachel **Überwachen und verwalten** im Blatt „Data Factory“ des [Azure-Portals](https://portal.azure.com/) aus. Wenn Sie sich bereits auf der Benutzeroberfläche von ADF befinden, klicken Sie auf der linken Seitenleiste auf das Symbol **Überwachen**.

Alle Data Factory-Ausführungen werden in der lokalen Zeitzone des Browsers angezeigt. Wenn Sie die Zeitzone ändern, werden alle Datums- und Zeitfelder auf die ausgewählte Zeitzone ausgerichtet.

## <a name="monitor-pipeline-runs"></a>Überwachen der Pipelineausführungen

Die standardmäßige Überwachungsansicht ist eine Liste der Pipelineausführungen im ausgewählten Zeitraum. Die folgenden Spalten werden angezeigt:

| **Spaltenname** | **Beschreibung** |
| --- | --- |
| Pipeline Name | Name der Pipeline |
| Aktionen | Symbole, mit denen Sie Aktivitätsdetails anzeigen, die Pipeline abbrechen oder erneut ausführen können. |
| Run Start | Startdatum und -uhrzeit der Pipelineausführung (MM/TT/JJJJ, HH:MM:SS AM/PM) |
| Duration | Dauer der Ausführung (hh:mm:ss) |
| Triggered By | Name des Triggers, der die Pipeline gestartet hat |
| Status | **Fehlerhaft**, **Erfolgreich**, **In Bearbeitung**, **Abgebrochen** oder **In Warteschlange** |
| Anmerkungen | Einer Pipeline zugeordnete filterbare Tags  |
| Parameter | Parameter für die Pipelineausführung (Name-Wert-Paare) |
| Fehler | Bei einem Fehler der Pipeline ist dies der Ausführungsfehler |
| Run ID | ID der Pipelineausführung |

![Listenansicht zum Überwachen von Pipelineausführungen](media/monitor-visually/pipeline-runs.png)

Sie müssen die Schaltfläche **Aktualisieren** manuell auswählen, um die Liste der Pipeline- und Aktivitätsausführungen zu aktualisieren. Eine automatische Aktualisierung wird derzeit nicht unterstützt.

![Schaltfläche „Aktualisieren“](media/monitor-visually/refresh.png)

## <a name="monitor-activity-runs"></a>Überwachung der Aktivitätsausführungen

Wählen Sie das Symbol **Aktivitätsausführungen anzeigen** unter der Spalte **Aktionen** aus, um die Aktivitätsausführungen für jede Pipelineausführung anzuzeigen. In der Listenansicht werden die Aktivitätsausführungen angezeigt, die den einzelnen Pipelineausführungen entsprechen.

| **Spaltenname** | **Beschreibung** |
| --- | --- |
| Activity Name | Name der Aktivität innerhalb der Pipeline |
| Activity Type | Der Typ der Aktivität, z. B. **Copy**, **ExecuteDataFlow** oder **AzureMLExecutePipeline** |
| Aktionen | Symbole, die es Ihnen ermöglichen, JSON-Eingabeinformationen, JSON-Ausgabeinformationen oder detaillierte aktivitätsspezifische Benutzeroberflächen für die Überwachung anzuzeigen. | 
| Run Start | Startdatum und -uhrzeit der Aktivitätsausführung (MM/TT/JJJJ, HH:MM:SS AM/PM) |
| Duration | Dauer der Ausführung (hh:mm:ss) |
| Status | **Fehlerhaft**, **Erfolgreich**, **In Bearbeitung** oder **Abgebrochen** |
| Integration Runtime | Für welche Integration Runtime die Aktivität ausgeführt wurde |
| Benutzereigenschaften | Benutzerdefinierte Eigenschaften der Aktivität |
| Fehler | Bei einer fehlerhaften Aktivität ist dies der Ausführungsfehler |
| Run ID | ID der Aktivitätsausführung |

![Listenansicht zum Überwachen von Aktivitätsausführungen](media/monitor-visually/activity-runs.png)

### <a name="promote-user-properties-to-monitor"></a>Heraufstufen von Benutzereigenschaften für die Überwachung

Stufen Sie jede Pipelineaktivitätseigenschaft als Benutzereigenschaft höher, sodass sie zu einer von Ihnen überwachten Entität wird. Sie können z. B. die Eigenschaften **Quelle** und **Ziel** der Kopieraktivität in der Pipeline als Benutzereigenschaften höher stufen. Wählen Sie **Automatisch generieren** aus, um die Benutzereigenschaften **Quelle** und **Ziel** für eine Kopieraktivität zu generieren.

![Erstellen von Benutzereigenschaften](media/monitor-visually/monitor-user-properties-image1.png)

> [!NOTE]
> Sie können nur bis zu fünf Pipelineaktivitätseigenschaften als Benutzereigenschaften höher stufen.

Nachdem Sie die Benutzereigenschaften erstellt haben, können Sie sie in den Überwachungslistenansichten überwachen. Wenn ein Tabellenname als Quelle für die Kopieraktivität dient, können Sie den Namen der Quelltabelle als Spalte in der Listenansicht für Aktivitätsausführungen überwachen.

![Aktivitätsausführungenliste ohne Benutzereigenschaften](media/monitor-visually/monitor-user-properties-image2.png)

![Hinzufügen von Spalten für Benutzereigenschaften zur Liste der Aktivitätsausführungen](media/monitor-visually/monitor-user-properties-image3.png)

![Aktivitätsausführungenliste mit Spalten für Benutzereigenschaften](media/monitor-visually/monitor-user-properties-image4.png)

## <a name="configure-the-list-view"></a>Konfigurieren der Listenansicht

### <a name="order-and-filter"></a>Sortieren und filtern

Legen Sie fest, ob Pipelineausführungen entsprechend der Startzeit der Ausführung in ab- oder aufsteigender Reihenfolge angezeigt werden. Sie können die Pipelineausführungen nach den folgenden Spalten filtern:

| **Spaltenname** | **Beschreibung** |
| --- | --- |
| Pipeline Name | Nach dem Namen der Pipeline filtern. |
| Run Start |  Bestimmt den Zeitbereich der angezeigten Pipelineausführungen. Die Optionen enthalten Schnellfilter für **Letzte 24 Stunden**, **Letzte Woche** und **Letzte 30 Tage** oder zum Auswählen eines benutzerdefinierten Zeitraums. |
| Run Status | Filtern der Ausführungen nach Status: **Erfolgreich**, **Fehlerhaft** **In Warteschlange**, **Abgebrochen** oder **In Bearbeitung**. |
| Anmerkungen | Nach Tags filtern, die auf die einzelnen Pipelines angewendet werden |
| Ausführungen | Filtern, ob erneut ausgeführte Pipelines angezeigt werden sollen |

![Optionen zum Filtern](media/monitor-visually/filter.png)

### <a name="add-or-remove-columns"></a>Hinzufügen oder Entfernen von Spalten
Klicken Sie mit der rechten Maustaste auf den Header der Listenansicht, und wählen Sie die Spalten aus, die in der Listenansicht angezeigt werden sollen.

![Optionen für Spalten](media/monitor-visually/columns.png)

### <a name="adjust-column-widths"></a>Anpassen der Spaltenbreite
Erhöhen oder verringern Sie die Spaltenbreiten in der Listenansicht, indem Sie mit der Maus auf die Spaltenüberschrift zeigen.

## <a name="rerun-activities-inside-a-pipeline"></a>Erneutes Ausführen von Aktivitäten innerhalb einer Pipeline

Aktivitäten innerhalb einer Pipeline können erneut ausgeführt werden. Wählen Sie **View Activity Runs** (Aktivitätsausführungen anzeigen) und dann die Aktivität in Ihrer Pipeline aus, ab der die Pipeline erneut ausgeführt werden soll.

![Anzeigen der Aktivitätsausführungen](media/monitor-visually/rerun-activities-image1.png)

![Auswählen einer Aktivitätsausführung](media/monitor-visually/rerun-activities-image2.png)

### <a name="rerun-from-failed-activity"></a>Ab fehlgeschlagener Aktivität erneut ausführen

Wenn eine Aktivität fehlschlägt, das Zeitlimit überschritten wird oder die Aktivität abgebrochen wird, können Sie die Pipeline ab der fehlgeschlagenen Aktivität erneut ausführen, indem Sie **Ab fehlgeschlagener Aktivität erneut ausführen** auswählen.

![Fehlgeschlagene Aktivität erneut ausführen](media/monitor-visually/rerun-failed-activity.png)

### <a name="view-rerun-history"></a>Anzeigen des Verlaufs für erneute Ausführungen

Der Verlauf für erneute Ausführungen kann für alle Pipelineausführungen in der Listenansicht angezeigt werden.

![Anzeigen des Verlaufs](media/monitor-visually/rerun-history-image1.png)

Sie können aber auch den Verlauf für erneute Ausführungen für eine bestimmte Pipelineausführung anzeigen.

![Anzeigen des Verlaufs für eine Pipelineausführung](media/monitor-visually/rerun-history-image2.png)

## <a name="monitor-consumption"></a>Überwachen des Verbrauchs

Sie können die durch eine Pipelineausführung verbrauchten Ressourcen anzeigen, indem Sie neben der Ausführung auf das Symbol für Verbrauch klicken. 

![Überwachen des Verbrauchs](media/monitor-visually/monitor-consumption-1.png)

Wenn Sie auf das Symbol klicken, wird ein Verbrauchsbericht zu den durch diese Pipelineausführung genutzten Ressourcen geöffnet. 

![Überwachen des Verbrauchs](media/monitor-visually/monitor-consumption-2.png)

Sie können diese Werte mit dem [Azure-Preisrechner](https://azure.microsoft.com/pricing/details/data-factory/) verbinden, um die Kosten für die Pipelineausführung zu schätzen. Weitere Informationen zu Azure Data Factory-Preisen finden Sie unter [Grundlegendes zu Preisen](pricing-concepts.md).

> [!NOTE]
> Diese vom Preisrechner zurückgegebenen Werte sind eine Schätzung. Sie geben nicht den genauen Betrag an, der Ihnen von Azure Data Factory in Rechnung gestellt wird. 

## <a name="gantt-views"></a>Gantt-Ansichten

Mit Gantt-Ansichten können Sie schnell Ihre Pipelines und Aktivitätsausführungen anzeigen.

![Beispiel für ein Gantt-Diagramm](media/monitor-visually/gantt1.png)

Die Gantt-Ansicht kann pro Pipeline oder nach Gruppen über von Ihnen erstellte Anmerkungen und Tags für Ihre Pipelines angezeigt werden.

![Gantt-Diagrammanmerkungen](media/monitor-visually/gantt2.png)

Die Länge des Balkens informiert über die Dauer der Pipeline. Sie können auch den Balken auswählen, um weitere Details anzuzeigen.

![Gantt-Diagramm zur Dauer](media/monitor-visually/gantt3.png)

## <a name="guided-tours"></a>Führungen
Wählen Sie links unten das **Informationssymbol** aus. Wählen Sie dann **Guided Tours** (Führungen) aus, um eine ausführliche Anleitung zum Überwachen der Pipeline- und Aktivitätsausführungen zu erhalten.

![Führungen](media/monitor-visually/guided-tours.png)

## <a name="alerts"></a>Alerts

Sie können Warnungen auf der Grundlage unterstützter Metriken in Data Factory auslösen. Wählen Sie dazu auf der Überwachungsseite von Data Factory die Option **Überwachen** > **Warnungen und Metriken** aus.

![Data Factory-Überwachungsseite](media/monitor-visually/start-page.png)

Das folgende Video enthält eine siebenminütige Einführung und Demonstration dieses Features:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Monitor-your-Azure-Data-Factory-pipelines-proactively-with-alerts/player]

### <a name="create-alerts"></a>Erstellen von Warnungen

1.  Wählen Sie **Neue Warnungsregel** aus, um eine neue Warnung zu erstellen.

    ![Schaltfläche „Neue Warnungsregel“](media/monitor-visually/new-alerts.png)

1.  Geben Sie den Namen der Warnungsregel an, und wählen Sie den Schweregrad der Warnung aus.

    ![Felder für Regelname und Schweregrad](media/monitor-visually/name-and-severity.png)

1.  Wählen Sie die Warnungskriterien aus.

    ![Feld für Zielkriterien](media/monitor-visually/add-criteria-1.png)

    ![Liste der Kriterien](media/monitor-visually/add-criteria-2.png)

1.  Konfigurieren Sie die Warnungslogik. Sie können eine Warnung für die ausgewählte Metrik für alle Pipelines und entsprechenden Aktivitäten erstellen. Sie können außerdem einen bestimmten Aktivitätstyp, Aktivitätsnamen, Pipelinenamen oder Fehlertyp auswählen.

    ![Optionen zum Konfigurieren der Warnungslogik](media/monitor-visually/alert-logic.png)

1.  Konfigurieren Sie E-Mail-, SMS-, Push- und Sprachbenachrichtigungen für die Warnung. Erstellen Sie eine Aktionsgruppe für die Warnungsbenachrichtigungen, oder wählen Sie eine vorhandene Aktionsgruppe aus.

    ![Optionen zum Konfigurieren von Benachrichtigungen](media/monitor-visually/configure-notification-1.png)

    ![Optionen zum Hinzufügen einer Benachrichtigung](media/monitor-visually/configure-notification-2.png)

1.  Erstellen Sie die Warnungsregel.

    ![Optionen zum Erstellen einer Warnungsregel](media/monitor-visually/create-alert-rule.png)

## <a name="next-steps"></a>Nächste Schritte

Im Artikel [Programmgesteuertes Überwachen und Verwalten von Pipelines](https://docs.microsoft.com/azure/data-factory/monitor-programmatically) erfahren Sie mehr über das Überwachen und Verwalten von Pipelines.
