---
title: Nachverfolgen von Datenänderungen in Azure SQL Edge (Vorschau)
description: Informationen zur Änderungsnachverfolgung und zu Change Data Capture in Azure SQL Edge (Vorschau)
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 3e3a6c3e171383d1812f63b945735d1b82f70b9c
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235100"
---
# <a name="tracking-data-changes-in-azure-sql-edge-preview"></a>Nachverfolgen von Datenänderungen in Azure SQL Edge (Vorschau)

Azure SQL Edge unterstützt die beiden SQL Server-Features, mit denen Änderungen an Daten in einer Datenbank nachverfolgt werden: [Änderungsnachverfolgung](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#Tracking) und [Change Data Capture](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#Capture). Mit diesen Funktionen können Anwendungen die DML-Änderungen (Einfüge-, Aktualisierungs- und Löschvorgänge) ermitteln, die an Benutzertabellen in einer Datenbank vorgenommen wurden. Change Data Capture und die Änderungsnachverfolgung können auf derselben Datenbank aktiviert werden, d. h., es sind keine zusätzlichen Überlegungen erforderlich.

Damit bestimmte Anwendungen effizient ausgeführt werden können, muss eine wichtige Anforderung erfüllt sein: Die Anwendungen müssen in der Lage sein, Daten abzufragen, die in einer Datenbank geändert wurden. Zum Ermitteln von Datenänderungen mussten Anwendungsentwickler normalerweise eine benutzerdefinierte Nachverfolgungsmethode in ihren Anwendungen implementieren, wobei sie eine Kombination von Triggern, Zeitstempelspalten und zusätzlichen Tabellen verwendeten. Die Erstellung solcher Anwendungen ist normalerweise sehr arbeitsintensiv, führt zu Schemaupdates und ist häufig mit hohem Verwaltungsaufwand verbunden. Bei einer IoT-Lösung, bei der Daten regelmäßig aus der Edgeumgebung in eine Cloud oder ein Rechenzentrum verschoben werden müssen, kann Änderungsnachverfolgung sehr nützlich sein. Diese ermöglicht eine schnelle und effektive Abfrage nur der Deltaänderungen aus der letzten Synchronisierung und das Hochladen dieser Änderungen in das Cloud- oder Rechenzentrumsziel. Weitere Informationen zu den Vorteilen der Verwendung von Änderungsnachverfolgung und Change Data Capture finden Sie unter [Vorteile der Verwendung von Change Data Capture oder Änderungsnachverfolgung](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#benefits-of-using-change-data-capture-or-change-tracking). 

Informationen zu den Funktionsunterschieden zwischen Änderungsnachverfolgung und Change Data Capture finden Sie unter [Funktionsunterschiede zwischen Change Data Capture und Änderungsnachverfolgung](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#feature-differences-between-change-data-capture-and-change-tracking).

## <a name="change-data-capture"></a>Change Data Capture

Einzelheiten zur Funktionsweise von Change Data Capture finden Sie unter [Informationen zu Change Data Capture](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server).

Informationen zum Aktivieren oder Deaktivieren von Change Data Capture finden Sie unter [Aktivieren und Deaktivieren von Change Data Capture](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server).

Informationen zum Verwalten und Überwachen von Change Data Capture finden Sie unter [Verwalten und Überwachen von Change Data Capture](https://docs.microsoft.com/sql/relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server).

Informationen dazu, wie Sie die geänderten Daten abfragen und mit ihnen arbeiten, finden Sie unter [Arbeiten mit Änderungsdaten](https://docs.microsoft.com/sql/relational-databases/track-changes/work-with-change-data-sql-server).

## <a name="change-tracking"></a>Change Tracking

Einzelheiten zur Funktionsweise von Änderungsnachverfolgung finden Sie unter [Informationen zur Änderungsnachverfolgung](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server).

Informationen zum Aktivieren oder Deaktivieren von Änderungsnachverfolgung finden Sie unter [Aktivieren und Deaktivieren von Änderungsnachverfolgung](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-tracking-sql-server).

Informationen zum Verwalten und Überwachen von Änderungsnachverfolgung finden Sie unter [Verwalten und Überwachen der Änderungsnachverfolgung](https://docs.microsoft.com/sql/relational-databases/track-changes/manage-change-tracking-sql-server).

Informationen dazu, wie Sie die geänderten Daten abfragen und mit ihnen arbeiten, finden Sie unter [Arbeiten mit Änderungsdaten](https://docs.microsoft.com/sql/relational-databases/track-changes/work-with-change-tracking-sql-server).

## <a name="temporal-tables"></a>Temporale Tabellen

Zusätzlich zur Unterstützung von Änderungsnachverfolgungs- und Change Data Capture-Funktionen von SQL Server unterstützt Azure SQL Edge auch das Feature „Temporale Tabellen“ von SQL Server. Temporale Tabellen (auch als „temporale Tabellen mit Versionsverwaltung durch das System“ bezeichnet) sind ein Datenbankfeature, das integrierte Unterstützung für das Bereitstellen von Informationen über die zu jedem Zeitpunkt in der Tabelle gespeicherten Daten mit sich bringt, anstatt nur die aktuell in einer Tabelle gespeicherten Daten zu unterstützen.

Eine temporale Tabelle mit Systemversionsverwaltung ist ein Benutzertabellentyp, der darauf ausgelegt ist, den Verlauf aller Datenänderungen lückenlos zu speichern und einfache Zeitpunktanalysen zu ermöglichen. Bei diesem Typ von temporaler Tabelle spricht man von Versionsverwaltung durch das System, da die Gültigkeitsdauer für jede Zeile vom System (d. h. von der Datenbank-Engine) verwaltet wird.

Jede temporale Tabelle weist zwei explizit definierte Spalten auf, beide vom Datentyp „datetime2“. Diese Spalten werden als Zeitraumspalten bezeichnet. Diese Zeitraumspalten werden bei jeder Änderung einer Zeile ausschließlich vom System zum Aufzeichnen des Gültigkeitszeitraums verwendet.

Über diese Zeitraumspalten hinaus enthält eine temporale Tabelle außerdem einen Verweis auf eine weitere Tabelle mit einem gespiegelten Schema. Das System verwendet diese Tabelle, um bei jeder Aktualisierung oder Löschung einer Zeile in der temporalen Tabelle automatisch die Vorversion der Zeile zu speichern. Diese zusätzliche Tabelle wird als die Verlaufstabelle bezeichnet, während die Haupttabelle, die die aktuellen (gültigen) Zeilenversionen speichert, als die aktuelle Tabelle oder einfach als die temporale Tabelle bezeichnet wird. Während der Erstellung von temporalen Tabellen können Benutzer eine vorhandene Verlaufstabelle (deren Schema kompatibel sein muss) angeben oder vom System eine standardmäßige Verlaufstabelle erstellen lassen.

Weitere Informationen zu temporalen Tabellen finden Sie unter [Temporale Tabellen](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables).

## <a name="see-also"></a>Weitere Informationen

- [Datenstreaming in Azure SQL Edge (Vorschau)](stream-data.md)
- [Machine Learning und KI mit ONNX in Azure SQL Edge (Vorschau)](onnx-overview.md)
- [Konfigurieren der Replikation in Azure SQL Edge (Vorschau)](configure-replication.md)
- [Sichern und Wiederherstellen von Datenbanken in Azure SQL Edge (Vorschau)](backup-restore.md)



