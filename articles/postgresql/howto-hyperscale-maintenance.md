---
title: Azure Database for PostgreSQL – Hyperscale (Citus) – Geplante Wartung – Azure-Portal
description: Hier erfahren Sie, wie Sie die Einstellungen für geplante Wartung für eine Instanz von Azure Database for PostgreSQL – Hyperscale (Citus) über das Azure-Portal konfigurieren können.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 04/07/2021
ms.openlocfilehash: 8e22de5288380490490c91846322e418f98dfcd4
ms.sourcegitcommit: 52491b361b1cd51c4785c91e6f4acb2f3c76f0d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108317254"
---
# <a name="manage-scheduled-maintenance-settings-for-azure-database-for-postgresql--hyperscale-citus"></a>Verwalten der Einstellungen für geplante Wartung für Azure Database for PostgreSQL – Hyperscale (Citus)

Sie können Wartungsoptionen für jede Hyperscale (Citus)-Servergruppe in Ihrem Azure-Abonnement festlegen. Zu diesen Optionen zählt der Wartungszeitplan sowie die Benachrichtigungseinstellungen für anstehende und abgeschlossene Wartungsereignisse.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

- Eine [Azure Database for PostgreSQL – Hyperscale (Citus)-Servergruppe](quickstart-create-hyperscale-portal.md)

## <a name="specify-maintenance-schedule-options"></a>Einen Wartungszeitplan mit festgelegten Optionen

1. Wählen Sie auf der Seite „Hyperscale (Citus)-Servergruppe“ unter der Überschrift **Einstellungen** die Option **Wartung** aus, um die Optionen für die geplante Wartung zu öffnen.
2. Der Standardzeitplan (vom System verwaltet) sieht einen zufälligen Wochentag sowie ein 30-minütiges Zeitfenster für die Wartung zwischen 23:00 und 7:00 Uhr in der [Azure-Regionszeit](https://go.microsoft.com/fwlink/?linkid=2143646) der Servergruppe vor. Wenn Sie diesen Zeitplan personalisieren möchten, klicken Sie auf **Benutzerdefinierter Zeitplan**. Dann können Sie einen bevorzugten Wochentag und ein 30-minütiges Fenster für die Startzeit der Wartung auswählen.

## <a name="notifications-about-scheduled-maintenance-events"></a>Benachrichtigungen zu geplanten Wartungsereignissen

Mit Azure Service Health können Sie [Benachrichtigungen anzeigen](../service-health/service-notifications.md), die bevorstehende und erledigte Wartungsarbeiten in Ihrer Hyperscale (Citus)-Servergruppe betreffen. Sie können auch Warnungen in Azure Service Health [einrichten](../service-health/resource-health-alert-monitor-guide.md), um Benachrichtigungen zu Wartungsereignissen zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über [geplante Wartung in Azure Database for PostgreSQL – Hyperscale (Citus)](concepts-hyperscale-maintenance.md).
* Weitere Informationen zu [Azure Service Health](../service-health/overview.md)
