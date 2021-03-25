---
title: Azure Database for MySQL – Flexible Server (Vorschau) – Geplante Wartung – Azure-Portal
description: In diesem Artikel erfahren Sie, wie Sie die Einstellungen für die geplante Wartung für eine Instanz von Azure Database for MySQL – Flexible Server über das Azure-Portal konfigurieren.
author: niklarin
ms.author: nlarin
ms.service: mysql
ms.topic: how-to
ms.date: 9/21/2020
ms.openlocfilehash: c8251eb2a89a7481ebc981f2b89668c363651b39
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91315013"
---
# <a name="manage-scheduled-maintenance-settings-for-azure-database-for-mysql--flexible-server"></a>Verwalten der Einstellungen für die geplante Wartung für Azure Database for MySQL – Flexible Server
 
Sie können für jede Flexible Server-Instanz in Ihrem Azure-Abonnement eigene Wartungsoptionen festlegen. Zu diesen Optionen zählt der Wartungszeitplan sowie die Benachrichtigungseinstellungen für anstehende und abgeschlossene Wartungsereignisse.

> [!IMPORTANT]
> Azure Database for MySQL – Flexible Server befindet sich in der Vorschau.
 
## <a name="prerequisites"></a>Voraussetzungen
Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:
- Eine [Azure Database for MySQL Flexible Server-Instanz](quickstart-create-server-portal.md)
 
## <a name="specify-maintenance-schedule-options"></a>Einen Wartungszeitplan mit festgelegten Optionen
 
1. Klicken Sie auf der Seite des MySQL-Servers unter der Überschrift **Einstellungen** auf **Wartung**, um die Optionen für die geplante Wartung zu öffnen.
2. Der Standardzeitplan (vom System verwaltet) sieht einen zufälligen Wochentag sowie ein 60-minütiges Zeitfenster für die Wartung zwischen 23:00 und 7:00 Uhr Ortszeit vor. Wenn Sie diesen Zeitplan personalisieren möchten, klicken Sie auf **Benutzerdefinierter Zeitplan**. Sie können dann einen bevorzugten Wochentag und ein 60-minütiges Fenster für die Startzeit der Wartung auswählen.
 
## <a name="notifications-about-scheduled-maintenance-events"></a>Benachrichtigungen zu geplanten Wartungsereignissen
 
Mit Azure Service Health können Sie [Benachrichtigungen anzeigen](../../service-health/service-notifications.md), die bevorstehende und durchgeführte Wartungsarbeiten auf Ihrer Flexible Server-Instanz betreffen. Sie können auch Warnungen in Azure Service Health [einrichten](../../service-health/resource-health-alert-monitor-guide.md), um Benachrichtigungen zu Wartungsereignissen zu erhalten.
 
## <a name="next-steps"></a>Nächste Schritte  
 
* Informieren Sie sich über [Geplante Wartung in Azure Database for MySQL: Flexible Server](concepts-maintenance.md).
* Weitere Informationen zu [Azure Service Health](../../service-health/overview.md)
