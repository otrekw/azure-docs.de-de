---
title: Planen von Azure-Wartungsereignissen
description: Es wird beschrieben, wie Sie Ereignisse der geplanten Wartung für Azure SQL-Datenbank und für verwaltete Azure SQL-Instanzen vorbereiten.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: sstein
ms.date: 1/21/2021
ms.openlocfilehash: d38ac9731959cf9a23052753b09c9e7819846705
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101664116"
---
# <a name="plan-for-azure-maintenance-events-in-azure-sql-database-and-azure-sql-managed-instance"></a>Planen von Azure-Wartungsereignissen in Azure SQL-Datenbank und Azure SQL Managed Instance
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Es wird beschrieben, wie Sie Ereignisse der geplanten Wartung in Ihrer Datenbank für Azure SQL-Datenbank und für verwaltete Azure SQL-Instanzen vorbereiten.

## <a name="what-is-a-planned-maintenance-event"></a>Was ist ein geplantes Wartungsereignis?

Damit die Dienste Azure SQL-Datenbank und Azure SQL Managed Instance sicher, konform, stabil und leistungsfähig bleiben, werden nahezu fortlaufend Updates über die Dienstkomponenten ausgeführt. Dank der modernen und robusten Dienstarchitektur und innovativer Technologien wie [Hotpatching](https://aka.ms/azuresqlhotpatching) sind die meisten Updates in Bezug auf die Dienstverfügbarkeit vollständig transparent und stellen keine Beeinträchtigung dar. Trotzdem verursachen einige Arten von Updates kurze Dienstunterbrechungen und erfordern eine besondere Behandlung. 

Für jede Datenbank halten Azure SQL-Datenbank und verwaltete Azure SQL-Instanzen ein bestimmtes Quorum an Datenbankreplikaten vor, von denen eines das primäre Replikat ist. Zu jeder Zeit muss ein primäres Replikat online in Betrieb sein, und mindestens ein sekundäres Replikat muss fehlerfrei sein. Während der geplanten Wartung werden die Mitglieder des Datenbankquorums nacheinander mit der Absicht offline geschaltet, dass ein antwortendes primäres Replikat und mindestens ein sekundäres Replikat online vorhanden ist, um sicherzustellen, dass es keine Ausfallzeiten des Clients auftreten. Wenn das primäre Replikat offline geschaltet werden muss, erfolgt ein Neukonfigurations-/Failoverprozess, bei dem ein sekundäres Replikat zum neuen primären Replikat wird.  

## <a name="what-to-expect-during-a-planned-maintenance-event"></a>Was Sie bei einem geplanten Wartungsereignis erwarten können

Das Wartungsereignis kann je nach der Konstellation der primären und sekundären Replikate zu Beginn des Wartungsereignisses einen einzelnen oder mehrere Failover erzeugen. Im Durchschnitt treten pro geplantem Wartungsereignis 1,7 Failover auf. Neukonfigurationen/Failover werden in der Regel innerhalb von 30 Sekunden abgeschlossen. Der Mittelwert beträgt acht Sekunden. Wenn sie bereits verbunden ist, muss Ihre Anwendung mit dem neuen primären Replikat Ihrer Datenbank erneut eine Verbindung herstellen. Wenn eine neue Verbindung versucht wird, während für die Datenbank eine Neukonfiguration durchgeführt wird, bevor das neue primäre Replikat online ist, erhalten Sie Fehler 40613 (Datenbank nicht verfügbar): *Die Datenbank "{Datenbankname}" auf Server "{Servername}" ist zurzeit nicht verfügbar. Wiederholen Sie den Verbindungsversuch später.“* Wenn Ihre Datenbank gerade eine zeitintensive Abfrage ausführt, wird diese bei einer Neukonfiguration unterbrochen und muss neu gestartet werden.

## <a name="how-to-simulate-a-planned-maintenance-event"></a>Simulieren eines geplanten Wartungsereignisses

Wenn Sie sicherstellen, dass Ihre Clientanwendung vor der Bereitstellung für die Produktionsumgebung hinsichtlich der Wartungsereignisse resilient ist, tragen Sie dazu bei, das Risiko von Anwendungsfehlern zu reduzieren und die Verfügbarkeit der Anwendung für Ihre Endbenutzer zu erhöhen. Sie können das Verhalten Ihrer Clientanwendung während geplanter Wartungsereignisse testen, indem Sie [einen manuellen Failover](https://aka.ms/mifailover-techblog) über PowerShell, CLI oder REST-API initiieren. Es wird ein identisches Verhalten wie ein Wartungsereignis erzeugen, bei dem die primäre Replik offline geschaltet wird.

## <a name="retry-logic"></a>Wiederholungslogik

Jede Clientproduktionsanwendung, die eine Verbindung mit einem Clouddatenbankdienst herstellt, sollte eine stabile [Wiederholungslogik](troubleshoot-common-connectivity-issues.md#retry-logic-for-transient-errors) für Verbindungen implementieren. Dies wird dazu beitragen, Failover für die Endbenutzer transparent zu gestalten oder zumindest negative Auswirkungen zu minimieren.

## <a name="resource-health"></a>Ressourcenintegrität

Wenn für Ihre Datenbank Anmeldefehler auftreten, sollten Sie im Fenster [Resource Health](../../service-health/resource-health-overview.md#get-started) im [Azure-Portal](https://portal.azure.com) den aktuellen Status überprüfen. Der Abschnitt „Integritätsverlauf“ enthält den Grund für die Ausfallzeit für jedes Ereignis (wenn verfügbar).

## <a name="maintenance-window-feature"></a>Funktion „Wartungsfenster“

Die Wartungsfensterfunktion bietet die Möglichkeit, Zeitpläne für vorhersehbare Wartungsfenster für berechtigte Azure SQL-Datenbanken und verwaltete SQL-Instanzen zu konfigurieren. Weitere Informationen finden Sie unter [Wartungsfenster](maintenance-window.md).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Resource Health](resource-health-to-troubleshoot-connectivity.md) für Azure SQL-Datenbank und Azure SQL Managed Instance.
- Weitere Informationen zur Wiederholungslogik finden Sie unter [Wiederholungslogik für vorübergehende Fehler](troubleshoot-common-connectivity-issues.md#retry-logic-for-transient-errors).
- Konfigurieren von Zeitplänen für Wartungsfenster mit der Funktion [Wartungsfenster](maintenance-window.md).
