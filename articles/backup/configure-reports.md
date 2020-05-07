---
title: Konfigurieren von Azure Backup-Berichten
description: Konfigurieren und Anzeigen von Berichten für Azure Backup mithilfe von Log Analytics und Azure-Arbeitsmappen
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: c1af9a532b390b428e74957c455988dfd4df3967
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82184944"
---
# <a name="configure-azure-backup-reports"></a>Konfigurieren von Azure Backup-Berichten

Eine häufige Anforderung an Sicherungsadministratoren besteht darin, basierend auf Daten, die sich über einen längeren Zeitraum erstrecken, Einblicke in Sicherungen zu erhalten. Anwendungsfälle für eine solche Lösung sind:

- Zuordnung und Vorhersagen des verbrauchten Cloudspeichers.
- Überwachung von Sicherungen und Wiederherstellungen.
- Identifizierung wichtiger Trends auf verschiedenen Granularitätsebenen.

Heute bietet Azure Backup eine Berichterstellungslösung, bei der [Azure Monitor-Protokolle](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) und [Azure-Arbeitsmappen](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) verwendet werden. Mithilfe dieser Ressourcen erhalten Sie umfassende Einblicke in Ihre Sicherungen in Ihrem gesamten Sicherungsumfeld. In diesem Artikel wird das Konfigurieren und Anzeigen von Azure Backup-Berichten erläutert.

## <a name="supported-scenarios"></a>Unterstützte Szenarios

- Backup-Berichte werden für Azure-VMs, SQL in Azure-VMs, SAP HANA/ASE in Azure-VMS, Microsoft Azure Recovery Services (MARS)-Agent, Microsoft Azure Backup Server (MABS) und System Center Data Protection Manager (DPM) unterstützt. Daten für die Sicherung von Azure-Dateifreigaben werden derzeit in Sicherungsberichten nicht angezeigt.
- Bei DPM-Workloads werden Backup-Berichte für DPM, Version 5.1.363.0 und höher, sowie Agent, Version 2.0.9127.0 und höher, unterstützt.
- Bei MABS-Workloads werden Backup-Berichte für MABS, Version 13.0.415.0 und höher, sowie Agent, Version 2.0.9170.0 und höher, unterstützt.
- Backup-Berichte können für alle Sicherungselemente, Tresore, Abonnements und Regionen angezeigt werden, solange deren Daten an einen Log Analytics-Arbeitsbereich gesendet werden, auf den der Benutzer Zugriff hat. Zum Anzeigen von Berichten für eine Gruppe von Tresoren müssen Sie nur über Lesezugriff auf den Log Analytics-Arbeitsbereich verfügen, an den die Tresore ihre Daten senden. Sie benötigen keinen Zugriff auf die einzelnen Tresore.
- Wenn Sie ein [Azure Lighthouse](https://docs.microsoft.com/azure/lighthouse/)-Benutzer mit delegiertem Zugriff auf die Abonnements Ihrer Kunden sind, können Sie diese Berichte mit Azure Lighthouse verwenden, um Berichte zu allen Ihren Mandanten anzuzeigen.
- Daten für Protokollsicherungsaufträge werden in den Berichten zurzeit nicht angezeigt.

## <a name="get-started"></a>Erste Schritte

Führen Sie die folgenden Schritte aus, um die Berichte verwenden zu können:

### <a name="1-create-a-log-analytics-workspace-or-use-an-existing-one"></a>1. Erstellen eines Log Analytics-Arbeitsbereichs oder Verwenden eines vorhandenen Arbeitsbereichs

Richten Sie einen oder mehrere Log Analytics-Arbeitsbereiche zum Speichern Ihrer Backup-Berichtsdaten ein. Der Ort und das Abonnement, an dem bzw. in dem dieser Log Analytics-Arbeitsbereich erstellt werden kann, sind unabhängig von dem Ort und dem Abonnement, der bzw. das Ihre Tresore enthält.

Eine Anleitung zum Einrichten eines Log Analytics-Arbeitsbereichs finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

Standardmäßig werden die Daten in einem Log Analytics-Arbeitsbereich 30 Tage lang aufbewahrt. Wenn Sie Daten für einen längeren Zeithorizont anzeigen möchten, ändern Sie den Aufbewahrungszeitraum des Log Analytics-Arbeitsbereichs. Informationen zum Ändern des Aufbewahrungszeitraums finden Sie unter [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage).

### <a name="2-configure-diagnostics-settings-for-your-vaults"></a>2. Konfigurieren von Diagnoseeinstellungen für Ihre Tresore

Azure Resource Manager-Ressourcen, etwa Azure Recovery-Tresore, zeichnen Informationen zu geplanten Vorgängen und benutzergesteuerten Vorgängen als Diagnosedaten auf.

Wählen Sie im Überwachungsabschnitt Ihres Recovery Services-Tresors **Diagnoseeinstellungen** aus, und geben Sie das Ziel für die Diagnosedaten des Tresors an. Weitere Informationen zur Verwendung von Diagnoseereignissen finden Sie unter [Verwenden von Diagnoseeinstellungen für Recovery Services-Tresore](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events).

![Bereich „Diagnoseeinstellungen“](./media/backup-azure-configure-backup-reports/resource-specific-blade.png)

Azure Backup bietet auch eine integrierte Azure-Richtliniendefinition, die die Konfiguration der Diagnoseeinstellungen für alle Tresore in einem bestimmten Bereich automatisiert. Informationen dazu, wie Sie diese Richtlinie verwenden können, finden Sie unter [Bedarfsgerechtes Konfigurieren von Tresordiagnoseeinstellungen](https://docs.microsoft.com/azure/backup/azure-policy-configure-diagnostics).

> [!NOTE]
> Nachdem Sie die Diagnose konfiguriert haben, dauert es möglicherweise bis zu 24 Stunden, bis der erste Datenpush abgeschlossen ist. Nachdem die Übertragung von Daten in den Log Analytics-Arbeitsbereich begonnen hat, werden Daten in den Berichten möglicherweise nicht sofort angezeigt, weil es sich dabei um Daten für den aktuellen Teiltag handelt. Weitere Informationen finden Sie unter [In Backup-Berichten verwendete Konventionen](https://docs.microsoft.com/azure/backup/configure-reports#conventions-used-in-backup-reports). Wir empfehlen, dass Sie erst zwei Tage nach dem Konfigurieren Ihrer Tresore zum Senden von Daten an Log Analytics beginnen sollten, die Berichte anzuzeigen.

#### <a name="3-view-reports-in-the-azure-portal"></a>3. Anzeigen von Berichten im Azure-Portal

Nachdem Sie Ihre Tresore zum Senden von Daten an Log Analytics konfiguriert haben, zeigen Sie Ihre Backup-Berichte an, indem zum Bereich eines beliebigen Tresors wechseln und **Sicherungsberichte** auswählen.

![Tresordashboard](./media/backup-azure-configure-backup-reports/vault-dashboard.png)

Wählen Sie diesen Link aus, um die Arbeitsmappe „Backup-Bericht“ zu öffnen.

> [!NOTE]
>
> - Zurzeit könnte das anfängliche Laden des Berichts bis zu einer Minute dauern.
> - Der Recovery Services-Tresor ist nur ein Einstiegspunkt für Backup-Berichte. Nachdem die Arbeitsmappe „Backup-Bericht“ aus dem Bereich eines Tresors geöffnet wurde, wählen Sie die entsprechende Gruppe von Log Analytics-Arbeitsbereichen aus, um Daten tresorübergreifend aggregiert anzuzeigen.

Der Bericht enthält verschiedene Registerkarten:

- **Zusammenfassung**: Verwenden Sie diese Registerkarte, um eine allgemeine Übersicht über Ihr Sicherungsumfeld zu erhalten. Sie erhalten einen schnellen Überblick über die Gesamtzahl der Sicherungselemente, den gesamten verbrauchten Cloudspeicher, die Anzahl der geschützten Instanzen und die Auftragserfolgsrate pro Workloadtyp. Für detailliertere Informationen zu einem bestimmten Sicherungsartefakttyp wechseln Sie zu den jeweiligen Registerkarten.

   ![Registerkarte „Zusammenfassung“](./media/backup-azure-configure-backup-reports/summary.png)

- **Sicherungselemente**: Auf dieser Registerkarte können Sie Informationen und Trends für Cloudspeicher anzeigen, der auf einer Sicherungselementebene verbraucht wird. Wenn Sie beispielsweise SQL in einer Azure-VM-Sicherung verwenden, können Sie den für jede zu sichernde SQL-Datenbank verbrauchten Cloudspeicher anzeigen. Sie können auch Daten für Sicherungselemente mit einem bestimmten Schutzstatus anzeigen. Wenn Sie beispielsweise oben auf der Registerkarte die Kachel **Schutz beendet** auswählen, werden alle Widgets darunter so gefiltert, dass nur Daten für Sicherungselemente mit dem Status „Schutz beendet“ angezeigt werden.

   ![Registerkarte „Sicherungselemente“](./media/backup-azure-configure-backup-reports/backup-items.png)

- **Verwendung**: Verwenden Sie diese Registerkarte zum Anzeigen von wichtigen Abrechnungsparametern für Ihre Sicherungen. Auf dieser Registerkarte werden Informationen auf Abrechnungsentitätsebene (geschützter Container) angezeigt. Beispielsweise können Sie im Fall eines DPM-Servers, der in Azure gesichert wird, den Trend der geschützten Instanzen und den für den DPM-Server verbrauchten Cloudspeicher anzeigen. Wenn Sie SQL in Azure Backup oder SAP HANA in Azure Backup verwenden, erhalten Sie auf dieser Registerkarte nutzungsbezogene Informationen auf der Ebene des virtuellen Computers, in dem diese Datenbanken gespeichert sind.

   ![Registerkarte „Verwendung“](./media/backup-azure-configure-backup-reports/usage.png)

- **Aufträge**: Verwenden Sie diese Registerkarte zum Anzeigen von langfristigen Trends für Aufträge, z. B. die Anzahl der fehlerhaften Aufträge pro Tag und die Hauptgründe für Auftragsfehler. Sie können diese Informationen sowohl auf einer aggregierten Ebene als auch auf der Ebene eines Sicherungselements anzeigen. Wählen Sie ein bestimmtes Sicherungselement in einem Raster aus, um detaillierte Informationen zu jedem Auftrag anzuzeigen, der für dieses Element im ausgewählten Zeitbereich ausgelöst wurde.

   ![Registerkarte „Aufträge“](./media/backup-azure-configure-backup-reports/jobs.png)

- **Richtlinien**: Verwenden Sie diese Registerkarte zum Anzeigen von Informationen zu allen Ihren aktiven Richtlinien, z. B. der Anzahl der zugeordneten Elemente und des gesamten Cloudspeicherplatzes, der von Elementen verbraucht wird, die im Rahmen einer bestimmten Richtlinie gesichert wurden. Wählen Sie eine bestimmte Richtlinie aus, um Informationen zu jedem der zugeordneten Sicherungselemente anzuzeigen.

   ![Registerkarte „Richtlinien“](./media/backup-azure-configure-backup-reports/policies.png)

## <a name="export-to-excel"></a>Exportieren in Excel

Wählen Sie bei einem beliebigen Widget wie einer Tabelle oder einem Diagramm rechts oben die Schaltfläche mit dem Pfeil nach unten aus, um dessen Inhalt wie vorhanden mit angewendeten Filtern als Excel-Tabelle zu exportieren. Wenn Sie weitere Zeilen einer Tabelle in Excel exportieren möchten, können Sie über den Dropdownpfeil **Zeilen pro Seite** am oberen Rand jedes Rasters die Anzahl der Zeilen erhöhen, die auf der Seite angezeigt werden.

## <a name="pin-to-dashboard"></a>An Dashboard anheften

Wählen Sie am oberen Rand jedes Widgets die Schaltfläche zum Anheften aus, um das Widget an das Dashboard Ihres Azure-Portals anzuheften. Mithilfe dieses Features können Sie angepasste Dashboards erstellen, die auf die Anzeige der wichtigsten benötigten Informationen zugeschnitten sind.

## <a name="cross-tenant-reports"></a>Mandantenübergreifende Berichte

Wenn Sie [Azure Lighthouse](https://docs.microsoft.com/azure/lighthouse/) mit delegiertem Zugriff auf Abonnements in mehreren Mandantenumgebungen nutzen, können Sie den Standardabonnementfilter verwenden. Wählen Sie in der oberen rechten Ecke des Azure-Portals die Filterschaltfläche aus, um alle Abonnements auszuwählen, für die Sie Daten anzeigen möchten. Auf diese Weise können Sie Log Analytics-Arbeitsbereiche für Ihre Mandanten auswählen, um mehrinstanzenfähige Berichte anzuzeigen.

## <a name="conventions-used-in-backup-reports"></a>In Backup-Berichten verwendete Konventionen

- Filter funktionieren auf jeder Registerkarte von links nach rechts und von oben nach unten. Dies bedeutet: Jeder Filter gilt nur für alle diejenigen Widgets, die auf seiner rechten Seite oder unter ihm positioniert sind.
- Wenn Sie eine farbige Kachel auswählen, werden die Widgets darunter nach Datensätzen gefiltert, die sich auf den Wert dieser Kachel beziehen. Wenn Sie beispielsweise auf der Registerkarte **Sicherungselemente** die der Kachel **Schutz beendet** auswählen, werden die Raster und Diagramme darunter so gefiltert, dass Daten für Sicherungselemente mit dem Status „Schutz beendet“ angezeigt werden.
- Auf nicht farbige Kacheln kann nicht geklickt werden.
- Daten für den aktuellen Teiltag werden in den Berichten nicht angezeigt. Wenn beispielsweise in **Zeitbereich** der Wert **Letzte 7 Tage** ausgewählt wurde, werden im Bericht Datensätze für die letzten sieben abgeschlossenen Tage angezeigt. Der aktuelle Tag ist darin nicht enthalten.
- Der Bericht zeigt Einzelheiten zu Aufträgen (mit Ausnahme von Protokollaufträgen), die im ausgewählten Zeitbereich *ausgelöst* wurden.
- Die für **Cloudspeicher** und *Geschützte Instanzen* angezeigten Werte stehen am **Ende** des ausgewählten Zeitbereichs.
- Die in den Berichten angezeigten Sicherungselemente sind die Elemente am *Ende* des ausgewählten Zeitbereichs. Sicherungselemente, die mitten im ausgewählten Zeitbereich gelöscht wurden, werden nicht angezeigt. Dieselbe Konvention gilt auch für Sicherungsrichtlinien.

## <a name="query-load-times"></a>Querladezeiten

Die Widgets im Backup-Bericht werden von Kusto-Abfragen unterstützt, die in den Log Analytics-Arbeitsbereichen des Benutzers ausgeführt werden. Diese Abfragen umfassen normalerweise die Verarbeitung großer Datenmengen mit mehreren Verknüpfungen, um umfangreichere Einblicke zu ermöglichen. Deshalb werden die Widgets vielleicht nicht sofort geladen, wenn der Benutzer Berichte in einem großen Sicherungsumfeld anzeigt. Diese Tabelle enthält eine grobe Schätzung der Zeit, die die verschiedenen Widgets zum Laden benötigen – basierend auf der Anzahl der Sicherungselemente und dem Zeitbereich, für den der Bericht angezeigt wird.

| **# Datenquellen**                         | **Zeithorizont** | **Ungefähre Ladezeiten**                                              |
| --------------------------------- | ------------- | ------------------------------------------------------------ |
| ~ 5 K                       | 1 Monat          | Kacheln: 5-10 Sek. <br> Raster: 5-10 Sek. <br> Diagramme: 5-10 Sek. <br> Filter auf Berichtsebene: 5-10 Sek.|
| ~ 5 K                       | 3 Monate          | Kacheln: 5-10 Sek. <br> Raster: 5-10 Sek. <br> Diagramme: 5-10 Sek. <br> Filter auf Berichtsebene: 5-10 Sek.|
| ~ 10 K                       | 3 Monate          | Kacheln: 15-20 Sek. <br> Raster: 15-20 Sek. <br> Diagramme: 1-2 Min. <br> Filter auf Berichtsebene: 25-30 Sek.|
| ~ 15 K                       | 1 Monat          | Kacheln: 15-20 Sek. <br> Raster: 15-20 Sek. <br> Diagramme: 50-60 Sek. <br> Filter auf Berichtsebene: 20-25 Sek.|
| ~ 15 K                       | 3 Monate          | Kacheln: 20-30 Sek. <br> Raster: 20-30 Sek. <br> Diagramme: 2-3 Min. <br> Filter auf Berichtsebene: 50-60 Sek. |

## <a name="what-happened-to-the-power-bi-reports"></a>Was ist mit den Power BI-Berichten geschehen?

- Die frühere Power BI-Vorlagen-App für die Berichterstellung, die Daten aus einem Azure Storage-Konto bezogen hat, wird bald eingestellt. Wir empfehlen, dass Sie mit dem Senden von Tresordiagnosedaten an Log Analytics beginnen, um Berichte anzeigen zu können.

- * Darüber hinaus wird auch das [V1-Schema](https://docs.microsoft.com/azure/backup/backup-azure-diagnostics-mode-data-model#v1-schema-vs-v2-schema) zum Senden von Diagnosedaten an ein Speicherkonto oder einen LA-Arbeitsbereich bald eingestellt. Das heißt, wenn Sie benutzerdefinierte Abfragen oder Automatisierungen auf der Grundlage des V1-Schemas geschrieben haben, sollten Sie diese Abfragen so aktualisieren, dass sie das derzeit unterstützte V2-Schema verwenden.

## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen zu Überwachung und Berichterstellung mit Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-monitor-alert-faq)
