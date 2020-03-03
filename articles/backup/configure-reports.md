---
title: Konfigurieren von Azure Backup-Berichten
description: Konfigurieren und Anzeigen von Berichten für Azure Backup mithilfe von Log Analytics und Azure-Arbeitsmappen
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: cefe81e53e89b8d7903469e836f3c5d2665febea
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77582703"
---
# <a name="configure-azure-backup-reports"></a>Konfigurieren von Azure Backup-Berichten

Eine häufige Anforderung an Sicherungsadministratoren besteht darin, basierend auf Daten, die sich über einen längeren Zeitraum erstrecken, Einblicke in Sicherungen zu erhalten. Es gibt mehrere Anwendungsfälle für eine solche Lösung: Zuordnen und Vorhersagen des genutzten Cloudspeichers, Überwachen von Sicherungen und Wiederherstellungen sowie Identifizieren wichtiger Trends auf unterschiedlichen Granularitätsebenen.

Heute bietet Azure Backup eine Berichterstellungslösung, die [Azure Monitor-Protokolle](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) und [Azure-Arbeitsmappen](https://docs.microsoft.com/azure/azure-monitor/app/usage-workbooks) nutzt. Dadurch erhalten Sie umfassende Einblicke in Ihre Sicherungen über den gesamten Sicherungsbestand. In diesem Artikel wird das Konfigurieren und Anzeigen von Sicherungsberichten erläutert.

## <a name="supported-scenarios"></a>Unterstützte Szenarios

* Sicherungsberichte werden für Azure-VMs, SQL auf Azure-VMs, SAP HANA/ASE auf Azure-VMs, Azure Backup Agent (MARS), Azure Backup Server (MABS) und System Center DPM unterstützt.
* Für DPM-Workloads werden Sicherungsberichte für DPM Version 5.1.363.0 und höher sowie Agent-Version 2.0.9127.0 und höher unterstützt.
* Für MABS-Workloads werden Sicherungsberichte für MABS Version 13.0.415.0 und höher sowie Agent-Version 2.0.9170.0 und höher unterstützt.
* Sicherungsberichte können für alle Sicherungselemente, Tresore, Abonnements und Regionen angezeigt werden, solange deren Daten an einen Log Analytics (LA)-Arbeitsbereich gesendet werden, auf den der Benutzer Zugriff hat. 
* Wenn Sie [Azure Lighthouse](https://docs.microsoft.com/azure/lighthouse/)-Benutzer mit delegiertem Zugriff auf die Abonnements Ihrer Kunden sind, können Sie diese Berichte mit Azure Lighthouse verwenden, um Berichte zu allen Ihren Mandanten anzuzeigen.
* Daten für Protokollsicherungsaufträge werden derzeit in den Berichten nicht angezeigt.

## <a name="getting-started"></a>Erste Schritte

Um mit der Verwendung der Berichte zu beginnen, führen Sie die drei unten beschriebenen Schritte aus:

1. **Erstellen Sie einen Log Analytics (LA)-Arbeitsbereich (oder verwenden Sie eine vorhandenen Arbeitsbereich):**

Sie müssen einen oder mehrere LA-Arbeitsbereiche zum Speichern Ihrer Sicherungsberichtsdaten einrichten. Der Ort und das Abonnement, an dem bzw. in dem dieser LA-Arbeitsbereich erstellt werden kann, sind unabhängig von dem Ort und dem Abonnement, der bzw. das Ihre Tresore enthält. 

Sehen Sie sich den folgenden Artikel an: [Erstellen Sie einen Log Analytics-Arbeitsbereich im Azure-Portal](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace), um einen LA-Arbeitsbereich einzurichten.

Standardmäßig werden die Daten in einem LA-Arbeitsbereich 30 Tage lang aufbewahrt. Um Daten für einen längeren Zeithorizont zu erhalten, ändern Sie den Aufbewahrungszeitraum des LA-Arbeitsbereichs. Informationen zum Ändern des Aufbewahrungszeitraums finden Sie im folgenden Artikel: [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage).

2. **Konfigurieren Sie Diagnoseeinstellungen für Ihre Tresore:**

Azure Resource Manager-Ressourcen, etwa Azure Recovery-Tresore, zeichnen Informationen zu geplanten Vorgängen und benutzergesteuerten Vorgängen als Diagnosedaten auf. 

Wählen Sie im Überwachungsabschnitt Ihres Recovery Services-Tresors **Diagnoseeinstellungen** aus, und geben Sie das Ziel für die Diagnosedaten des Recovery Services-Tresors an. [Erfahren Sie mehr über die Verwendung von Diagnoseereignissen](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events).

![Blatt „Diagnoseeinstellungen“](./media/backup-azure-configure-backup-reports/resource-specific-blade.png)

Azure Backup bietet auch eine integrierte Azure-Richtlinie, die die Konfiguration der Diagnoseeinstellungen für alle Tresore in einem bestimmten Bereich automatisiert. Im folgenden Artikel erfahren Sie, wie Sie diese Richtlinie verwenden: [Bedarfsgerechtes Konfigurieren von Tresordiagnoseeinstellungen](https://docs.microsoft.com/azure/backup/azure-policy-configure-diagnostics)

3. **Zeigen Sie Berichte im Azure-Portal an:**

Nachdem Sie Ihre Tresore für das Senden von Daten an LA konfiguriert haben, können Sie Ihre Sicherungsberichte anzeigen, indem Sie zum Blatt eines beliebigen Tresors navigieren und auf den Menüpunkt **Sicherungsberichte** klicken. 

![Tresordashboard](./media/backup-azure-configure-backup-reports/vault-dashboard.png)

Durch Klicken auf diesen Link wird die Arbeitsmappe des Sicherungsberichts geöffnet.

> [!NOTE]
> Derzeit kann das anfängliche Laden des Berichts bis zu einer Minute dauern.

Nachfolgend finden Sie eine Beschreibung der verschiedenen Registerkarten, die der Bericht enthält:

* **Zusammenfassung** – Die Registerkarte „Zusammenfassung“ bietet einen allgemeinen Überblick über Ihren Sicherungsbestand. Unter der Registerkarte „Zusammenfassung“ erhalten Sie einen schnellen Überblick über die Gesamtzahl der Sicherungselemente, den gesamten verbrauchten Cloudspeicher, die Anzahl der geschützten Instanzen und die Auftragserfolgsrate pro Workloadtyp. Für detailliertere Informationen zu einem bestimmten Sicherungsartefakttyp navigieren Sie zu den entsprechenden Registerkarten.

![Registerkarte „Zusammenfassung“](./media/backup-azure-configure-backup-reports/summary.png)

* **Sicherungselemente** – Auf der Registerkarte „Sicherungselemente“ finden Sie Informationen und Trends über den verbrauchten Cloudspeicher auf Sicherungselementebene. Wenn Sie beispielsweise eine „SQL in Azure-VM“-Sicherung verwenden, können Sie den für jede zu sichernde SQL-Datenbank verbrauchten Cloudspeicher anzeigen. Sie können auch Daten für Sicherungselemente mit einem bestimmten Schutzstatus anzeigen. Wenn Sie beispielsweise oben auf der Registerkarte auf die Kachel **Schutz beendet** klicken, werden alle unten stehenden Widgets so gefiltert, dass nur Daten für Sicherungselemente mit dem Status „Schutz beendet“ angezeigt werden.

![Registerkarte „Sicherungselemente“](./media/backup-azure-configure-backup-reports/backup-items.png)

* **Verwendung** – Auf der Registerkarte „Verwendung“ werden die wichtigsten Abrechnungsparameter für Ihre Sicherungen angezeigt. Auf dieser Registerkarte werden Informationen auf Abrechnungsentitätsebene (geschützter Container) angezeigt. Beispielsweise können Sie im Fall eines DPM-Servers, der in Azure gesichert wird, den Trend der geschützten Instanzen und den für den DPM-Server verbrauchten Cloudspeicher anzeigen. Wenn Sie eine „SQL in Azure“- oder „SAP HANA in Azure“-Sicherung verwenden, erhalten Sie auf dieser Registerkarte nutzungsbezogene Informationen auf der Ebene des virtuellen Computers, in dem diese Datenbanken enthalten sind.

![Registerkarte „Verwendung“](./media/backup-azure-configure-backup-reports/usage.png)

* **Aufträge** – Auf der Registerkarte „Aufträge“ können Sie langfristige Trends für Aufträge anzeigen, z. B. die Anzahl der fehlerhaften Aufträge pro Tag und die Hauptgründe für Auftragsfehler. Sie können diese Informationen sowohl auf einer aggregierten Ebene als auch auf der Ebene eines Sicherungselements anzeigen. Wenn Sie auf ein bestimmtes Sicherungselement in einem Raster klicken, können Sie detaillierte Informationen zu jedem Auftrag anzeigen, der für dieses Sicherungselement im ausgewählten Zeitbereich ausgelöst wurde.

![Registerkarte „Aufträge“](./media/backup-azure-configure-backup-reports/jobs.png)

* **Richtlinien** – Auf der Registerkarte „Richtlinien“ werden Informationen zu all Ihren aktiven Richtlinien angezeigt, z. B. Anzahl der zugehörigen Elemente und gesamter Cloudspeicherplatz, der von Elementen verbraucht wird, die im Rahmen einer bestimmten Richtlinie gesichert wurden. Indem Sie auf eine bestimmte Richtlinie klicken, können Sie Informationen zu jedem der zugehörigen Sicherungselemente anzeigen.

![Registerkarte „Richtlinien“](./media/backup-azure-configure-backup-reports/policies.png)

## <a name="exporting-to-excel"></a>Exportieren nach Excel

Wenn Sie in einem beliebigen Widget (Tabelle/Diagramm) rechts oben auf die Schaltfläche mit dem Pfeil nach unten klicken, wird der Inhalt des Widgets als Excel-Tabelle exportiert, und zwar genau wie angezeigt, also mit angewandten Filtern. Wenn Sie zusätzliche Zeilen einer Tabelle nach Excel exportieren möchten, können Sie über die Dropdownliste **Zeilen pro Seite** am oberen Rand jedes Rasters die Anzahl der Zeilen erhöhen, die auf der Seite angezeigt werden.

## <a name="pinning-to-dashboard"></a>Anheften an das Dashboard

Klicken Sie am oberen Rand jedes Widgets auf das Symbol „Anheften“, um das Widget an Ihr Azure-Portal-Dashboard anzuheften. Auf diese Weise können Sie angepasste Dashboards erstellen, die auf die Anzeige der wichtigsten benötigten Informationen zugeschnitten sind.

## <a name="cross-tenant-reports"></a>Mandantenübergreifende Berichte

Wenn Sie [Azure Lighthouse](https://docs.microsoft.com/azure/lighthouse/)-Benutzer mit delegiertem Zugriff auf Abonnements in mehreren Mandantenumgebungen sind, können Sie den Standardabonnementfilter verwenden (durch Klicken auf das Filtersymbol rechts oben im Azure-Portal), um alle Abonnements auszuwählen, für die Sie die Daten anzeigen möchten. Auf diese Weise können Sie die LA-Arbeitsbereiche Ihrer Mandanten auswählen, um Berichte für mehrere Mandanten anzuzeigen.

## <a name="conventions-used-in-backup-reports"></a>In Sicherungsberichten verwendete Konventionen

* Die Filter werden auf jeder Registerkarte von links nach rechts und von oben nach unten angewendet, d. h. jeder Filter gilt nur für all diejenigen Widgets, die sich entweder rechts von diesem Filter oder unter diesem Filter befinden. 
* Wenn Sie auf eine farbige Kachel klicken, werden die Widgets unter der Kachel nach Datensätzen gefiltert, die sich auf den Wert dieser Kachel beziehen. Wenn Sie beispielsweise auf der Registerkarte „Sicherungselemente“ auf die Kachel *Schutz beendet* klicken, werden die Raster und Diagramme darunter so gefiltert, dass Daten für Sicherungselemente mit dem Status „Schutz beendet“ angezeigt werden.
* Kacheln, die nicht farbig sind, können nicht angeklickt werden.
* Daten für den aktuellen Teiltag werden in den Berichten nicht angezeigt. Wird für **Zeitbereich** also der Wert ***Letzte 7 Tage*** ausgewählt, zeigt der Bericht Datensätze für die letzten 7 abgeschlossenen Tage an (wobei der aktuelle Tag nicht berücksichtigt wird).
* Der Bericht zeigt Einzelheiten zu den Aufträgen an (mit Ausnahme der Protokollaufträge), die im ausgewählten Zeitbereich **ausgelöst** wurden. 
* Die für „Cloudspeicher“ und „Geschützte Instanzen“ angezeigten Werte befinden sich am **Ende** des ausgewählten Zeitbereichs.
* Die in den Berichten angezeigten Sicherungselemente sind die Elemente, die am **Ende** des ausgewählten Zeitbereichs vorhanden sind. Sicherungselemente, die in der Mitte des ausgewählten Zeitbereichs gelöscht wurden, werden nicht angezeigt. Dieselbe Konvention gilt auch für Sicherungsrichtlinien.

## <a name="query-load-times"></a>Querladezeiten

Die Widgets im Sicherungsbericht werden von Kusto-Abfragen unterstützt, die in den LA-Arbeitsbereichen des Benutzers ausgeführt werden. Da diese Abfragen typischerweise die Verarbeitung großer Datenmengen mit mehreren Joins beinhalten, um umfassendere Einblicke zu ermöglichen, werden die Widgets möglicherweise nicht sofort geladen, wenn der Benutzer Berichte über einen großen Datensicherungsbestand anzeigt. Die folgende Tabelle enthält eine grobe Schätzung der Zeit, die die verschiedenen Widgets zum Laden benötigen, basierend auf der Anzahl der Sicherungselemente und dem Zeitbereich, für den der Bericht angezeigt wird:

| **Anzahl Datenquellen**                         | **Zeithorizont** | **Ladezeiten (Schätzung)**                                              |
| --------------------------------- | ------------- | ------------------------------------------------------------ |
| ~ 5 K                       | 1 Monat          | Kacheln: 5-10 Sek. <br> Raster: 5-10 Sek. <br> Diagramme: 5-10 Sek. <br> Filter auf Berichtsebene: 5-10 Sek.|
| ~ 5 K                       | 3 Monate          | Kacheln: 5-10 Sek. <br> Raster: 5-10 Sek. <br> Diagramme: 5-10 Sek. <br> Filter auf Berichtsebene: 5-10 Sek.|
| ~ 10 K                       | 3 Monate          | Kacheln: 15-20 Sek. <br> Raster: 15-20 Sek. <br> Diagramme: 1-2 Min. <br> Filter auf Berichtsebene: 25-30 Sek.|
| ~ 15 K                       | 1 Monat          | Kacheln: 15-20 Sek. <br> Raster: 15-20 Sek. <br> Diagramme: 50-60 Sek. <br> Filter auf Berichtsebene: 20-25 Sek.|
| ~ 15 K                       | 3 Monate          | Kacheln: 20-30 Sek. <br> Raster: 20-30 Sek. <br> Diagramme: 2-3 Min. <br> Filter auf Berichtsebene: 50-60 Sek. |

## <a name="what-happened-to-the-power-bi-reports"></a>Was ist mit den Power BI-Berichten geschehen?
* Unsere frühere Power BI-Vorlagen-App für die Berichterstellung (die Daten von einem Azure Storage-Konto bezogen hat) wird bald eingestellt. Es wird empfohlen, mit dem Senden von Tresordiagnosedaten an Log Analytics zu beginnen, wie oben beschrieben, um Berichte anzuzeigen.

* Darüber hinaus wird auch das V1-Schema zum Senden von Diagnosedaten an ein Speicherkonto oder einen LA-Arbeitsbereich bald eingestellt. Das heißt, wenn Sie benutzerdefinierte Abfragen oder Automatisierungen auf der Grundlage des V1-Schemas geschrieben haben, sollten Sie diese Abfragen so aktualisieren, dass sie das derzeit unterstützte V2-Schema verwenden.

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zu Überwachung und Berichterstellung mit Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-monitor-alert-faq)