---
title: 'Geplante Wartung: Azure Database for PostgreSQL – Hyperscale (Citus)'
description: In diesem Artikel wird das Feature für die geplante Wartung in Azure Database for PostgreSQL – Hyperscale (Citus) beschrieben.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/22/2021
ms.openlocfilehash: 0d11ec8815cc0f5082f95c5331321f043e86eece
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105027478"
---
# <a name="scheduled-maintenance-in-azure-database-for-postgresql--hyperscale-citus"></a>Geplante Wartung in Azure Database for PostgreSQL – Hyperscale (Citus)

Azure Database for PostgreSQL – Hyperscale (Citus) führt regelmäßige Wartungen durch, um die Sicherheit, Stabilität und Aktualität Ihrer verwalteten Datenbank sicherzustellen.  Während der Wartung erhalten alle Knoten in der Servergruppe neue Features, Updates und Patches.

Die wichtigsten Features der planmäßigen Wartung für Hyperscale (Citus) sind:

* Updates werden gleichzeitig auf allen Knoten in der Servergruppe angewendet.
* Benachrichtigungen über anstehende Wartungsarbeiten werden fünf Tage im Voraus in Azure Service Health veröffentlicht.
* In der Regel liegen zwischen erfolgreichen Wartungsereignissen für eine Servergruppe 30 Tage.

## <a name="notification-about-upcoming-maintenance"></a>Benachrichtigung zur bevorstehenden Wartung

Benachrichtigungen über anstehende geplante Wartungsarbeiten werden in Azure Service Health veröffentlicht und können wie folgt gesendet werden:

* E-Mail an eine bestimmte Adresse
* E-Mail an eine Azure Resource Manager-Rolle
* SMS an ein mobiles Gerät
* Pushbenachrichtigung an eine Azure-App
* Sprachnachricht

> [!IMPORTANT]
> Normalerweise liegen zwischen erfolgreichen Ereignissen vom Typ „Geplante Wartung“ für eine Servergruppe 30 Tage.
>
> Bei einem kritischen Notfallupdate, z. B. zur Behebung eines schwerwiegenden Sicherheitsrisikos, kann das Benachrichtigungsfenster aber auch kürzer als fünf Tage sein. Das kritische Update kann auch dann auf Ihren Server angewendet werden, wenn innerhalb der letzten 30 Tage eine erfolgreiche geplante Wartung durchgeführt wurde.

Wenn bei der Wartung ein Fehler auftritt oder sie abgebrochen wird, erstellt das System eine Benachrichtigung.
Es wird die Wartung gemäß den aktuellen Zeitplaneinstellungen erneut versuchen und Sie fünf Tage vor dem nächsten Wartungsereignis benachrichtigen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Abonnieren von [Benachrichtigungen zu bevorstehenden Wartungsarbeiten](../service-health/service-notifications.md) mit Azure Service Health
* Weitere Informationen zur [Einrichtung von Benachrichtigungen zu bevorstehenden geplanten Wartungsereignissen](../service-health/resource-health-alert-monitor-guide.md)
