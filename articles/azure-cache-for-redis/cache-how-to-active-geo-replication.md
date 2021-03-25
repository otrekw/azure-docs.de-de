---
title: Konfigurieren der aktiven Georeplikation für Azure Cache for Redis-Enterprise-Instanzen
description: Erfahren Sie, wie Ihre Azure Cache for Redis-Enterprise-Instanzen über Azure-Regionen hinweg repliziert werden.
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 02/08/2021
ms.author: yegu
ms.openlocfilehash: fe777c3aa7b314dc56a42cc64712d18281a6ea7d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102121166"
---
# <a name="configure-active-geo-replication-for-enterprise-azure-cache-for-redis-instances-preview"></a>Konfigurieren der aktiven Georeplikation für Azure Cache for Redis-Enterprise-Instanzen (Vorschau)

In diesem Artikel erfahren Sie, wie Sie einen aktiven georeplizierten Azure Cache über das Azure-Portal konfigurieren.

Die aktive Georeplikation gruppiert zwei oder mehr Azure Cache for Redis-Enterprise-Instanzen in einem einzelnen Cache, der mehrere Azure-Regionen umfasst. Alle Instanzen fungieren als lokale primäre Replikate. Eine Anwendung entscheidet, welche Instanzen für Lese- und Schreibanforderungen verwendet werden sollen.

## <a name="create-or-join-an-active-geo-replication-group"></a>Erstellen von oder Beitreten zu einer aktiven Georeplikationsgruppe

> [!IMPORTANT]
> Die aktive Georeplikation muss aktiviert werden, wenn ein Azure Cache for Redis erstellt wird.
>
>

1. Klicken Sie in der Benutzeroberfläche zum Erstellen eines **Neuer Redis Cache** auf **Konfigurieren**, um **Aktive Georeplikation** auf der Registerkarte **Erweitert** einzurichten.

    ![Konfigurieren der aktiven Georeplikation](./media/cache-how-to-active-geo-replication/cache-active-geo-replication-not-configured.png)

1. Erstellen Sie eine neue Replikationsgruppe für eine erste Cache-Instanz, oder wählen Sie eine vorhandene aus der Liste aus.

    ![Verknüpfen von Caches](./media/cache-how-to-active-geo-replication/cache-active-geo-replication-new-group.png)

1. Klicken Sie auf **Konfigurieren**, um den Vorgang abzuschließen.

    ![Konfigurierte aktive Georeplikation](./media/cache-how-to-active-geo-replication/cache-active-geo-replication-configured.png)

1. Wiederholen Sie die obigen Schritte für jede zusätzliche Cache-Instanz in der Georeplikationsgruppe.

## <a name="remove-from-an-active-geo-replication-group"></a>Entfernen aus einer aktiven Georeplikationsgruppe

Um eine Cache-Instanz aus einer aktiven Georeplikationsgruppe zu entfernen, löschen Sie einfach die Instanz. Die verbleibenden Instanzen konfigurieren sich automatisch selbst neu.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Azure Cache for Redis-Features.

* [Azure Cache for Redis-Dienstebenen](cache-overview.md#service-tiers)
* [Hochverfügbarkeit für Azure Cache for Redis](cache-high-availability.md)
