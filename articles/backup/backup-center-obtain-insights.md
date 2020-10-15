---
title: Abrufen von Erkenntnissen mithilfe von Backup Center
description: Erfahren Sie, wie Sie mit Backup Center historische Trends analysieren und tiefere Erkenntnisse aus Ihren Sicherungen gewinnen.
ms.topic: conceptual
ms.date: 09/01/2020
ms.openlocfilehash: 5964f285089feea721a0b452efed884e905b89cc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90992800"
---
# <a name="obtain-insights-using-backup-center"></a>Abrufen von Erkenntnissen mithilfe von Backup Center

Zum Analysieren von historischen Trends und Gewinnen von tieferen Erkenntnissen aus Ihren Sicherungen bietet Backup Center eine Schnittstelle zu [Sicherungsberichten](configure-reports.md), die [Azure Monitor-Protokolle](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs) und [Azure-Arbeitsmappen](https://docs.microsoft.com/azure/azure-monitor/platform/workbooks-overview) verwendet. Sicherungsberichte bieten die folgenden Funktionen:

- Zuordnung und Vorhersagen des verbrauchten Cloudspeichers.

- Überwachung von Sicherungen und Wiederherstellungen.

- Identifizierung wichtiger Trends auf verschiedenen Granularitätsebenen.

- Erhalten von Einblicken in Kostenoptimierungsmöglichkeiten für Ihre Sicherungen und Gewinnen von Erkenntnissen dazu

## <a name="supported-scenarios"></a>Unterstützte Szenarien

- Sicherungsberichte sind derzeit für Sicherungen von Azure Database for PostgreSQL-Servern nicht verfügbar.

- Eine ausführliche Liste der unterstützten und nicht unterstützten Szenarien finden Sie in der [Supportmatrix](backup-center-support-matrix.md).

## <a name="get-started"></a>Erste Schritte

### <a name="configure-your-vaults-to-send-data-to-a-log-analytics-workspace"></a>Konfigurieren Ihrer Tresore zum Senden von Daten an einen Log Analytics-Arbeitsbereich

[Informationen zum Konfigurieren von Diagnoseeinstellungen für Ihre Tresore im großen Stil](https://docs.microsoft.com/azure/backup/configure-reports#get-started)

### <a name="view-backup-reports-in-the-backup-center-portal"></a>Anzeigen von Sicherungsberichten im Backup Center-Portal

Durch Auswählen der Menüoption **Sicherungsberichte** in Backup Center werden die Berichte geöffnet. Wählen Sie mindestens einen Log Analytics Arbeitsbereich aus, um zentrale Informationen zu Ihren Sicherungen anzuzeigen und zu analysieren.

![Sicherungsberichte in Backup Center](./media/backup-center-obtain-insights/backup-center-backup-reports.png)

Im Folgenden finden Sie die verfügbaren Ansichten:

1. **Zusammenfassung:** Verwenden Sie diese Registerkarte, um eine allgemeine Übersicht über Ihr Sicherungsumfeld zu erhalten. [Weitere Informationen](https://docs.microsoft.com/azure/backup/configure-reports#summary)

1. **Sicherungselemente:** Auf dieser Registerkarte können Sie Informationen und Trends für Cloudspeicher anzeigen, der auf einer Sicherungselementebene verarbeitet wird. [Weitere Informationen](https://docs.microsoft.com/azure/backup/configure-reports#backup-items)

1. **Nutzung:** Verwenden Sie diese Registerkarte zum Anzeigen von wichtigen Abrechnungsparametern für Ihre Sicherungen. [Weitere Informationen](https://docs.microsoft.com/azure/backup/configure-reports#usage)

1. **Aufträge:** Verwenden Sie diese Registerkarte zum Anzeigen von langfristigen Trends für Aufträge, z. B. der Anzahl fehlerhafter Aufträge pro Tag und der Hauptgründe für Auftragsfehler. [Weitere Informationen](https://docs.microsoft.com/azure/backup/configure-reports#jobs)

1. **Richtlinien:** Verwenden Sie diese Registerkarte zum Anzeigen von Informationen zu allen aktiven Richtlinien, z. B. der Anzahl zugeordneter Elemente und des gesamten Cloudspeicherplatzes, der von Elementen verbraucht wird, die im Rahmen einer bestimmten Richtlinie gesichert wurden. [Weitere Informationen](https://docs.microsoft.com/azure/backup/configure-reports#policies)

1. **Optimieren:** Verwenden Sie diese Registerkarte, um potenzielle Möglichkeiten zur Kostenoptimierung Ihrer Sicherungen anzuzeigen. [Weitere Informationen](https://docs.microsoft.com/azure/backup/configure-reports#optimize)

## <a name="next-steps"></a>Nächste Schritte

- [Überwachen und Betreiben von Sicherungen](backup-center-monitor-operate.md)
- [Steuern Ihres Sicherungsbestands](backup-center-govern-environment.md)
- [Ausführen von Aktionen mithilfe des Backup Centers](backup-center-actions.md)
