---
title: 'Geplante Wartung: Azure Database for PostgreSQL – Hyperscale (Citus)'
description: In diesem Artikel wird das Feature für die geplante Wartung in Azure Database for PostgreSQL – Hyperscale (Citus) beschrieben.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 04/07/2021
ms.openlocfilehash: dee7257a296f523dbc9040d65fe68c14edf928f6
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107106784"
---
# <a name="scheduled-maintenance-in-azure-database-for-postgresql--hyperscale-citus"></a>Geplante Wartung in Azure Database for PostgreSQL – Hyperscale (Citus)

Azure Database for PostgreSQL – Hyperscale (Citus) führt regelmäßige Wartungen durch, um die Sicherheit, Stabilität und Aktualität Ihrer verwalteten Datenbank sicherzustellen.  Während der Wartung erhalten alle Knoten in der Servergruppe neue Features, Updates und Patches.

Die wichtigsten Features der planmäßigen Wartung für Hyperscale (Citus) sind:

* Updates werden gleichzeitig auf allen Knoten in der Servergruppe angewendet.
* Benachrichtigungen über anstehende Wartungsarbeiten werden fünf Tage im Voraus in Azure Service Health veröffentlicht.
* In der Regel liegen zwischen erfolgreichen Wartungsereignissen für eine Servergruppe 30 Tage.
* Der bevorzugte Wochentag und das Zeitfenster innerhalb dieses Tages für den Beginn der Wartung können für jede Servergruppe einzeln definiert werden.

## <a name="selecting-a-maintenance-window-and-notification-about-upcoming-maintenance"></a>Auswählen eines Wartungsfensters und Benachrichtigung zur bevorstehenden Wartung

Sie können die Wartung für einen bestimmten Wochentag und ein bestimmtes Zeitfenster innerhalb dieses Tages planen. Alternativ können Sie die Wahl eines Tages und eines Zeitfensters dem System überlassen. In beiden Fällen werden Sie vom System fünf Tage vor einer Wartung benachrichtigt. Darüber hinaus werden Sie informiert, wenn die Wartung beginnt und wenn sie erfolgreich abgeschlossen wurde.

Benachrichtigungen über anstehende geplante Wartungsarbeiten werden in Azure Service Health veröffentlicht und können wie folgt gesendet werden:

* E-Mail an eine bestimmte Adresse
* E-Mail an eine Azure Resource Manager-Rolle
* SMS an ein mobiles Gerät
* Pushbenachrichtigung an eine Azure-App
* Sprachnachricht

Beim Konfigurieren der Einstellungen für den Wartungszeitplan können Sie einen Wochentag und ein Zeitfenster auswählen. Ohne Angabe werden vom System je nach Regionszeit der Servergruppe Zeiten zwischen 23 Uhr und 7 Uhr ausgewählt. Sie können verschiedene Zeitpläne für jede Hyperscale (Citus)-Servergruppe in Ihrem Azure-Abonnement festlegen.

> [!IMPORTANT]
> Normalerweise liegen zwischen erfolgreichen Ereignissen vom Typ „Geplante Wartung“ für eine Servergruppe 30 Tage.
>
> Bei einem kritischen Notfallupdate, z. B. zur Behebung eines schwerwiegenden Sicherheitsrisikos, kann das Benachrichtigungsfenster aber auch kürzer als fünf Tage sein. Das kritische Update kann auch dann auf Ihren Server angewendet werden, wenn innerhalb der letzten 30 Tage eine erfolgreiche geplante Wartung durchgeführt wurde.

Sie können die Zeitplaneinstellungen jederzeit aktualisieren. Wenn eine Wartung für Ihre Hyperscale (Citus)-Servergruppe geplant ist und Sie den Zeitplan aktualisieren, werden bereits vorhandene Ereignisse wie ursprünglich geplant fortgesetzt. Die Einstellungsänderung wird erst nach erfolgreichem Abschluss vorhandener Ereignisse wirksam.

Wenn bei der Wartung ein Fehler auftritt oder sie abgebrochen wird, erstellt das System eine Benachrichtigung.
Es wird die Wartung gemäß den aktuellen Zeitplaneinstellungen erneut versuchen und Sie fünf Tage vor dem nächsten Wartungsereignis benachrichtigen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum [Bearbeiten eines Wartungszeitplans](howto-hyperscale-maintenance.md)
* Weitere Informationen zum Abonnieren von [Benachrichtigungen zu bevorstehenden Wartungsarbeiten](../service-health/service-notifications.md) mit Azure Service Health
* Weitere Informationen zur [Einrichtung von Benachrichtigungen zu bevorstehenden geplanten Wartungsereignissen](../service-health/resource-health-alert-monitor-guide.md)
