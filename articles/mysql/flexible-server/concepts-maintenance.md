---
title: Geplante Wartung – Azure Database for MySQL – Flexible Server
description: In diesem Artikel wird das Feature für die geplante Wartung in Azure Database for MySQL – Flexible Server beschrieben.
author: niklarin
ms.author: nlarin
ms.service: mysql
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: 67529ce3546046eaba75bd3beacd7bb9b96445fe
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931369"
---
# <a name="scheduled-maintenance-in-azure-database-for-mysql--flexible-server"></a>Geplante Wartung in Azure Database for MySQL: Flexible Server

In Azure Database for MySQL – Flexible Server werden regelmäßige Wartungen durchgeführt, um die Sicherheit, Stabilität und Aktualität Ihrer verwalteten Datenbank sicherzustellen. Während der Wartung erhält der Server neue Features, Updates und Patches.

> [!IMPORTANT]
> Azure Database for MySQL – Flexible Server befindet sich in der Vorschau.

## <a name="select-a-maintenance-window"></a>Auswählen eines Wartungsfensters

Sie können die Wartung für einen bestimmten Wochentag und ein bestimmtes Zeitfenster innerhalb dieses Tages planen. Alternativ können Sie dem System gestatten, einen Tag und ein Zeitfenster automatisch auszuwählen. In beiden Fällen werden Sie vom System fünf Tage vor einer Wartung benachrichtigt. Das System informiert Sie außerdem, wenn die Wartung startet und erfolgreich abgeschlossen wird.

Benachrichtigungen zu bevorstehenden geplanten Wartungsarbeiten können in folgenden Formaten vorliegen:

* E-Mail an eine bestimmte Adresse
* E-Mail an eine Azure Resource Manager-Rolle
* SMS an ein mobiles Gerät
* Pushbenachrichtigung an eine Azure-App
* Sprachnachricht

Beim Konfigurieren der Einstellungen für den Wartungszeitplan können Sie einen Wochentag und ein Zeitfenster auswählen. Wenn Sie nichts angeben, wird vom System je nach Zeitzone des Servers ein Zeitfenster zwischen 23:00 und 7:00 Uhr ausgewählt. Sie können für jeden flexiblen Server in Ihrem Azure-Abonnement einen eigenen Zeitplan definieren.

> [!IMPORTANT]
> Normalerweise vergehen zwischen erfolgreichen geplanten Wartungen eines Servers 30 Tage.
>
> Bei einem kritischen Notfallupdate, z. B. zur Behebung eines schwerwiegenden Sicherheitsrisikos, kann das Benachrichtigungsfenster aber auch kürzer als fünf Tage sein. Das kritische Update kann auch dann auf Ihren Server angewendet werden, wenn innerhalb der letzten 30 Tage eine erfolgreiche geplante Wartung durchgeführt wurde.

Sie können die Zeitplaneinstellungen jederzeit aktualisieren. Wenn für Ihren flexiblen Server eine Wartung geplant ist und Sie die Zeitplaneinstellungen aktualisieren, wird das aktuelle Ereignis planungsgemäß fortgesetzt, und die Änderungen an den Zeitplaneinstellungen werden nach dem erfolgreichen Abschluss wirksam.

In seltenen Fällen kann ein Wartungsereignis vom System abgebrochen oder möglicherweise nicht erfolgreich abgeschlossen werden. In diesem Fall erstellt das System eine Benachrichtigung über das abgebrochene bzw. fehlerhafte Wartungsereignis. Der nächste Wartungsversuch wird gemäß den aktuellen Zeitplaneinstellungen geplant, und Sie erhalten fünf Tage im Voraus eine Benachrichtigung.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum [Bearbeiten eines Wartungszeitplans](how-to-maintenance-portal.md)
* Weitere Informationen zum Abonnieren von [Benachrichtigungen zu bevorstehenden Wartungsarbeiten](/azure/service-health/service-notifications.md) mit Azure Service Health
* Weitere Informationen zur [Einrichtung von Benachrichtigungen zu bevorstehenden geplanten Wartungsereignissen](/azure/service-health/resource-health-alert-monitor-guide.md)
