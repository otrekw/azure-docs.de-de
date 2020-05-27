---
title: Langfristiges Aufbewahren von Sicherungen
description: Hier erfahren Sie, wie Azure SQL-Datenbank das Speichern vollständiger Datenbanksicherungen bei Verwendung der Richtlinie zur Langzeitaufbewahrung für bis zu zehn Jahre unterstützt.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 05/18/2019
ms.openlocfilehash: 6fc5fab9ae61c8c8ade9260b32078ffa430b077f
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83771399"
---
# <a name="azure-sql-database-long-term-retention"></a>Langzeitaufbewahrung von Azure SQL-Datenbank

Viele Anwendungen dienen gesetzlichen, ordnungsgemäßen oder anderen geschäftlichen Zwecken, die voraussetzen, dass Datenbanksicherungen länger als der Zeitraum von 7–35 Tagen, der für [automatischen Sicherungen](sql-database-automated-backups.md) von Azure SQL-Datenbank zur Verfügung gestellt wird, aufbewahrt werden. Mithilfe des Features für die langfristige Aufbewahrung (Long-Term Retention, LTR) können Sie angegebene vollständige Sicherungen von SQL-Datenbanken bis zu zehn Jahre lang in Azure-Blobspeicher mit georedundantem Speicher mit Lesezugriff speichern. Sie können dann jede Sicherung als neue Datenbank wiederherstellen. Weitere Informationen zur Azure Storage-Redundanz finden Sie unter [Azure Storage-Redundanz](../storage/common/storage-redundancy.md). 

Eine lange Aufbewahrungsdauer kann für einzelne Datenbanken und Pooldatenbanken aktiviert werden. Sie ist in der eingeschränkten öffentlichen Vorschau für Instanzen verfügbar, die über Azure SQL-Datenbank verwaltet werden. 

> [!NOTE]
> Sie können SQL-Agent-Aufträge verwenden, um [Kopiesicherungen von Datenbanken](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server) als Alternative zur Langzeitaufbewahrung über 35 Tage hinaus zu planen.


## <a name="how-sql-database-long-term-retention-works"></a>Funktionsweise der langfristigen Aufbewahrung von SQL-Datenbank

Die langfristige Sicherungsaufbewahrung (LTR) nutzt die [automatischen](sql-database-automated-backups.md) vollständigen Datenbanksicherungen für die Point-in-Time-Wiederherstellung (Point In Time Restore, PITR). Wenn eine LTR-Richtlinie konfiguriert wurde, werden diese Sicherungen in verschiedene Blobs für langfristige Speicherung kopiert. Der Kopiervorgang ist ein Hintergrundauftrag, der keine Auswirkung auf die Leistung der Datenbankworkload hat. Die LTR-Richtlinie für jede SQL-Datenbank kann auch angeben, wie oft die LTR-Sicherungen erstellt werden.

Um von LTR zu profitieren, können Sie eine Richtlinie mit einer Kombination aus vier Parametern definieren: wöchentliche Sicherungsaufbewahrung (W), monatliche Sicherungsaufbewahrung (M), jährliche Sicherungsaufbewahrung (Y) und Woche des Jahres (WeekOfYear). Wenn Sie den Parameter „W“ angeben, wird eine wöchentliche Sicherung in den langfristigen Speicher kopiert. Wenn Sie den Parameter „M“ angeben, wird die erste Sicherung jedes Monats in den langfristigen Speicher kopiert. Wenn Sie den Parameter „Y“ angeben, wird eine Sicherung, die in der durch „WeekOfYear“ angegebenen Woche erstellt wird, in den langfristigen Speicher kopiert. Wenn die angegebene WeekOfYear beim Konfigurieren der Richtlinie in der Vergangenheit liegt, wird die erste LTR-Sicherung im Folgejahr erstellt. Jede Sicherung wird entsprechend den Richtlinienparametern, die beim Erstellen der LTR-Sicherung konfiguriert werden, im langfristigen Speicher aufbewahrt.

> [!NOTE]
> Jede Änderung der LTR-Richtlinie gilt nur für zukünftige Sicherungen. Wenn Sie beispielsweise die wöchentliche Sicherungsaufbewahrung (W), monatliche Sicherungsaufbewahrung (M) oder jährliche Sicherungsaufbewahrung (Y) ändern, gilt die Aufbewahrungseinstellung nur für neue Sicherungen. Die Aufbewahrung vorhandener Sicherungen wird nicht geändert. Wenn Sie ältere LTR-Sicherungen vor Ablauf des Aufbewahrungszeitraums löschen möchten, müssen Sie die [Sicherungen manuell löschen](https://docs.microsoft.com/azure/sql-database/sql-database-long-term-backup-retention-configure#delete-ltr-backups).
> 

Beispiele für die LTR-Richtlinie:

-  W=0, M=0, Y=5, WeekOfYear=3

   Die dritte vollständige Sicherung jedes Jahres wird fünf Jahre lang aufbewahrt.
   
- W=0, M=3, Y=0

   Die erste vollständige Sicherung jedes Monats wird drei Monate lang aufbewahrt.

- W=12, M=0, Y=0

   Jede wöchentliche vollständige Sicherung wird zwölf Wochen lang aufbewahrt.

- W=6, M=12, Y=10, WeekOfYear=16

   Jede wöchentliche vollständige Sicherung wird sechs Wochen lang aufbewahrt. Eine Ausnahme stellt die erste vollständige Sicherung jeden Monats dar: Sie wird zwölf Monate lang aufbewahrt. Eine weitere Ausnahme stellt die vollständige Sicherung dar, die in der 16. Woche des Jahres erstellt wurde: Sie wird zehn Jahre lang aufbewahrt. 

Die folgende Tabelle veranschaulicht den Rhythmus und den Ablauf der langfristigen Sicherungen für die folgenden Richtlinie:

W=12 (12 Wochen bzw. 84 Tage), M=12 (12 Monate bzw. 365 Tage), Y=10 (10 Jahre bzw. 3.650 Tage), WeekOfYear=15 (Woche nach dem 15. April)

   ![LTR-Beispiel](./media/sql-database-long-term-retention/ltr-example.png)



Wenn Sie die vorstehende Richtlinie ändern und „W=0“ (keine wöchentlichen Sicherungen) festlegen, ändert sich der Rhythmus der Sicherungskopien, wie in der Tabelle oben durch die hervorgehobenen Daten gezeigt. Der zum Aufbewahren dieser Sicherungen benötigte Speicherplatz verringert sich entsprechend. 

> [!IMPORTANT]
> Die Zeitplanung für die einzelnen LTR-Sicherungen wird durch Azure SQL-Datenbank gesteuert. Sie können eine LTR-Sicherung nicht manuell erstellen oder die Zeitplanung für die Sicherungserstellung steuern. Nach dem Konfigurieren einer LTR-Richtlinie kann es bis zu 7 Tage dauern, bis die erste LTR-Sicherung in der Liste der verfügbaren Sicherungen angezeigt wird.  
> 

## <a name="geo-replication-and-long-term-backup-retention"></a>Georeplikation und langfristige Sicherungsaufbewahrung

Wenn Sie aktive Georeplikation oder Failovergruppen als Geschäftskontinuitätslösung verwenden, sollten Sie sich auf mögliche Failover vorbereiten und dieselbe LTR-Richtlinie für die sekundäre Geodatenbank konfigurieren. Ihre LTR-Speicherkosten werden sich nicht erhöhen, da keine Sicherungen von sekundären Datenbanken erstellt werden. Nur, wenn die sekundäre Datenbank zur primäre Datenbank wird, werden die Sicherungen erstellt. Dies sorgt für eine unterbrechungsfreie Erstellung der LTR-Sicherungen, wenn das Failover ausgelöst und die primäre Datenbank in den sekundären Bereich verschoben wird. 

> [!NOTE]
> Wenn die ursprüngliche primäre Datenbank nach dem Ausfall, der zu einem Failover geführt hat, wiederhergestellt wird, wird sie zu einer neuen sekundären Datenbank. Daher wird die Sicherungserstellung nicht fortgesetzt und die bestehende LTR-Richtlinie wird erst wieder wirksam, wenn sie wieder die primäre Datenbank ist. 

## <a name="managed-instance-support"></a>Unterstützung verwalteter Instanzen

Für die langfristige Sicherungsaufbewahrung bei Instanzen, die über Azure SQL-Datenbank verwaltet werden, gelten die folgenden Einschränkungen:

- **Eingeschränkte öffentliche Vorschau**: Diese Vorschauversion ist nur für EA- und CSP-Abonnements verfügbar. Es gelten die Bestimmungen der eingeschränkten Verfügbarkeit.  
- [**Nur PowerShell**](sql-database-managed-instance-long-term-backup-retention-configure.md): Das Azure-Portal wird derzeit nicht unterstützt. Die Langzeitaufbewahrung muss über PowerShell aktiviert werden. 

Wenn Sie eine Registrierung anfordern möchten, erstellen Sie ein [Azure-Supportticket](https://azure.microsoft.com/support/create-ticket/). Wählen Sie bei „Issuetyp“ die Option „Technisches Problem“, bei „Dienst“ die Option „Verwaltete SQL-Datenbank-Instanz“ und bei „Problemtyp“ die Option **Sicherung, Wiederherstellung und Geschäftskontinuität/Langfristige Sicherungsaufbewahrung**aus. Geben Sie in Ihrer Anforderung an, dass Sie in der eingeschränkten öffentlichen Vorschau von LTR für eine verwaltete Instanz registriert werden möchten.

## <a name="configure-long-term-backup-retention"></a>Konfigurieren der langfristigen Sicherungsaufbewahrung

Informationen zum Konfigurieren der langfristigen Aufbewahrung mit dem Azure-Portal oder PowerShell finden Sie unter [Verwalten der langfristigen Aufbewahrung von Sicherungen in Azure SQL-Datenbank](sql-database-long-term-backup-retention-configure.md).

## <a name="restore-database-from-ltr-backup"></a>Wiederherstellen einer Datenbank aus der LTR-Sicherung

Zum Wiederherstellen einer Datenbank aus dem LTR-Speicher können Sie eine bestimmte Sicherung basierend auf ihrem Zeitstempel auswählen. Die Datenbank kann auf einem beliebigen vorhandenen Server unter dem gleichen Abonnement wie die ursprüngliche Datenbank wiederhergestellt werden. Informationen zum Wiederherstellen Ihrer Datenbank aus einer LTR-Sicherung mit dem Azure-Portal oder PowerShell finden Sie unter [Verwalten der langfristigen Aufbewahrung von Sicherungen in Azure SQL-Datenbank](sql-database-long-term-backup-retention-configure.md). Geben Sie in Ihrer Anforderung an, dass Sie in der eingeschränkten öffentlichen Vorschau von LTR für eine verwaltete Instanz registriert werden möchten.

## <a name="next-steps"></a>Nächste Schritte

Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Weitere Informationen zu den anderen Geschäftskontinuitätslösungen von SQL-Datenbank finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
