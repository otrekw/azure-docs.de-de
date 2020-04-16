---
title: Einrichten von Warnungen und Benachrichtigungen für eine verwaltete Instanz (Azure-Portal)
description: Verwenden Sie das Azure-Portal, um Warnungen für eine verwaltete SQL-Datenbank-Instanz zu erstellen, die Benachrichtigungen oder eine Automatisierung auslösen, wenn die angegebenen Bedingungen erfüllt sind.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 04/02/2020
ms.openlocfilehash: a332627d149a36ba5d5beb2626023e58a221f0d6
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639185"
---
# <a name="create-alerts-for-azure-sql-managed-instance-using-the-azure-portal"></a>Erstellen von Warnungen für eine verwaltete Azure SQL-Datenbank-Instanz im Azure-Portal

## <a name="overview"></a>Übersicht

In diesem Artikel erfahren Sie, wie Sie im Azure-Portal Warnungen für Datenbank in einer verwaltete Azure SQL-Datenbank-Instanz einrichten können. Warnungen können Ihnen eine E-Mail senden oder einen Webhook aufrufen, wenn bei einer bestimmten Metrik (beispielsweise bei der Instanzspeichergröße oder CPU-Auslastung) der Schwellenwert erreicht wird. Dieser Artikel nennt auch bewährte Methoden für das Festlegen von Warnungszeiträumen.

Sie können auf der Grundlage von Überwachungsmetriken für Ihre Azure-Services oder von Ereignissen, die bei diesen auftreten, eine Warnung empfangen.

* **Metrikwerte** : Die Warnung wird ausgelöst, wenn der Wert einer angegebenen Metrik einen von Ihnen festgelegten Schwellenwert in beliebiger Richtung überschreitet. Das Auslösen erfolgt sowohl, wenn die Bedingung erstmals erfüllt wird, als auch danach, wenn diese Bedingung nicht mehr erfüllt wird.
* **Aktivitätsprotokollereignisse** : Eine Warnung kann für *jedes* Ereignis oder nur dann ausgelöst werden, wenn eine bestimmte Anzahl von Ereignissen erfolgt ist.

Sie können konfigurieren, dass bei einer Warnung Folgendes erfolgt, wenn sie ausgelöst wird:

* Senden von E-Mail-Benachrichtigungen an den Dienstadministrator und Co-Administratoren
* Senden von E-Mails an weitere von Ihnen angegebene Adressen
* Aufrufen eines Webhooks

Sie haben folgende Möglichkeiten zum Konfigurieren von Warnregeln und Abrufen zugehöriger Informationen:

* [Azure portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../azure-monitor/platform/alerts-classic-portal.md)
* [Befehlszeilenschnittstelle (CLI)](../azure-monitor/platform/alerts-classic-portal.md)
* [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Erstellen einer Warnungsregel anhand einer Metrik mit dem Azure-Portal

1. Suchen Sie im [Portal](https://portal.azure.com/)die Ressource, die Sie überwachen möchten, und wählen Sie sie aus.
2. Wählen Sie im Abschnitt „Überwachung“ **Warnungen** aus. Text und Symbol können je nach Ressource geringfügig variieren.  

   ![Überwachung](media/sql-database-insights-alerts-portal/Alerts.png)
  
3. Wählen Sie die Schaltfläche **Neue Warnungsregel** aus, um die Seite **Regel erstellen** zu öffnen.
   ![Regel erstellen](media/sql-database-insights-alerts-portal/create-rule.png)

4. Klicken Sie im Abschnitt **Bedingung** auf **Hinzufügen**.
   ![Bedingung definieren](media/sql-database-insights-alerts-portal/create-rule.png)
5. Wählen Sie auf der Seite **Signallogik konfigurieren** ein Signal aus.
   ![Signal auswählen](media/sql-database-insights-alerts-portal/select-signal.png)
6. Nach Auswahl eines Signals, z. B. **CPU-Prozentsatz**, wird die Seite **Signallogik konfigurieren** angezeigt.
   ![Signallogik konfigurieren](media/sql-database-insights-alerts-portal/configure-signal-logic.png)
7. Konfigurieren Sie auf dieser Seite den Schwellentyp, Operator, Aggregationstyp, Schwellenwert, die Aggregationsgranularität und die Häufigkeit der Auswertung. Klicken Sie anschließend auf **Fertig**.
8. Wählen Sie unter **Regel erstellen** eine vorhandene **Aktionsgruppe** aus, oder erstellen Sie eine neue Gruppe. Mithilfe einer Aktionsgruppe können Sie die Aktion definieren, die erfolgen soll, wenn eine Warnungsbedingung eintritt.
  ![Aktionsgruppe definieren](media/sql-database-insights-alerts-portal/action-group.png)

9. Legen Sie einen Namen für die Regel fest, geben Sie eine optionale Beschreibung an, und wählen Sie einen Schweregrad für die Regel. Wählen Sie, ob die Regel bei ihrer Erstellung aktiviert werden soll, und klicken Sie dann auf **Regelwarnung erstellen**, um die Metrikregelwarnung zu erstellen.

Innerhalb von 10 Minuten wird die Warnung aktiv und wie oben beschrieben ausgelöst.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Konfigurieren von Webhooks in Warnungen](../azure-monitor/platform/alerts-webhooks.md).
