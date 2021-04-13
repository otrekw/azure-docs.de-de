---
title: Konfigurieren der aktiven Georeplikation für Azure Cache for Redis-Enterprise-Instanzen
description: Erfahren Sie, wie Ihre Azure Cache for Redis-Enterprise-Instanzen über Azure-Regionen hinweg repliziert werden.
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 02/08/2021
ms.author: yegu
ms.openlocfilehash: 3fe3131263d3cf1984eae1692854d8d6bcd2746a
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105109487"
---
# <a name="configure-active-geo-replication-for-enterprise-azure-cache-for-redis-instances-preview"></a>Konfigurieren der aktiven Georeplikation für Azure Cache for Redis-Enterprise-Instanzen (Vorschau)

In diesem Artikel erfahren Sie, wie Sie einen aktiven georeplizierten Azure Cache über das Azure-Portal konfigurieren.

Die aktive Georeplikation gruppiert zwei oder mehr Azure Cache for Redis-Enterprise-Instanzen in einem einzelnen Cache, der mehrere Azure-Regionen umfasst. Alle Instanzen fungieren als lokale primäre Replikate. Eine Anwendung entscheidet, welche Instanzen für Lese- und Schreibanforderungen verwendet werden sollen.

## <a name="create-or-join-an-active-geo-replication-group"></a>Erstellen von oder Beitreten zu einer aktiven Georeplikationsgruppe

> [!IMPORTANT]
> Die aktive Georeplikation muss aktiviert werden, wenn ein Azure Cache for Redis erstellt wird.
>
>

1. Wählen Sie auf der Registerkarte **Erweitert** der Erstellungsbenutzeroberfläche **Neuer Redis Cache** die Option **Enterprise** für **Clusterrichtlinie** aus.

    ![Konfigurieren der aktiven Georeplikation](./media/cache-how-to-active-geo-replication/cache-active-geo-replication-not-configured.png)

1. Klicken Sie auf **Konfigurieren**, um die **Aktive Georeplikation** einzurichten.

1. Erstellen Sie eine neue Replikationsgruppe für eine erste Cache-Instanz, oder wählen Sie eine vorhandene aus der Liste aus.

    ![Verknüpfen von Caches](./media/cache-how-to-active-geo-replication/cache-active-geo-replication-new-group.png)

1. Klicken Sie auf **Konfigurieren**, um den Vorgang abzuschließen.

    ![Konfigurierte aktive Georeplikation](./media/cache-how-to-active-geo-replication/cache-active-geo-replication-configured.png)

1. Warten Sie, bis der erste Cache erfolgreich erstellt wurde. Wiederholen Sie die obigen Schritte für jede zusätzliche Cache-Instanz in der Georeplikationsgruppe.

## <a name="remove-from-an-active-geo-replication-group"></a>Entfernen aus einer aktiven Georeplikationsgruppe

Um eine Cache-Instanz aus einer aktiven Georeplikationsgruppe zu entfernen, löschen Sie einfach die Instanz. Die verbleibenden Instanzen konfigurieren sich automatisch selbst neu.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Azure Cache for Redis-Features.

* [Azure Cache for Redis-Dienstebenen](cache-overview.md#service-tiers)
* [Hochverfügbarkeit für Azure Cache for Redis](cache-high-availability.md)
