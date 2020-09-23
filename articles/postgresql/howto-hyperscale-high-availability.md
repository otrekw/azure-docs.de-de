---
title: Konfigurieren von Hochverfügbarkeit – Hyperscale (Citus) – Azure Database for PostgreSQL
description: Aktivieren oder Deaktivieren von Hochverfügbarkeit
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 07/27/2020
ms.openlocfilehash: 46b842994cbcf7efe66d5992c79246d77626e268
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90907395"
---
# <a name="configure-hyperscale-citus-high-availability"></a>Konfigurieren von Hochverfügbarkeit für Hyperscale (Citus)

Azure Database for PostgreSQL – Hyperscale (Citus) bietet Hochverfügbarkeit (High Availability, HA), um Datenbankausfälle zu vermeiden. Wenn Hochverfügbarkeit aktiviert ist, erhält jeder Knoten in einer Servergruppe einen Standknoten. Wenn der ursprüngliche Knoten einen Fehler aufweist, wird der Standbyknoten höher gestuft, um ihn zu ersetzen.

> [!IMPORTANT]
> Da Hochverfügbarkeit die Anzahl der Server in der Gruppe verdoppelt, werden auch die Kosten verdoppelt.

Die Aktivierung von Hochverfügbarkeit ist während der Erstellung der Servergruppe oder danach im Azure-Portal auf der Registerkarte **Compute + Speicher** für Ihre Servergruppe möglich. Die Benutzeroberfläche sieht in beiden Fällen ähnlich aus. Ziehen Sie den Schieberegler für **Hochverfügbarkeit** von „NEIN“ auf „JA“:

:::image type="content" source="./media/howto-hyperscale-high-availability/01-ha-slider.png" alt-text="Schieberegler für Hochverfügbarkeit":::

Klicken Sie auf die Schaltfläche **Speichern**, um Ihre Auswahl zu übernehmen. Das Aktivieren von Hochverfügbarkeit kann einige Zeit in Anspruch nehmen, da die Servergruppe Standbyknoten bereitstellen und Daten an diese streamen muss.

Auf der Registerkarte **Übersicht** für die Servergruppe werden alle Knoten und deren Standbyknoten zusammen mit einer Spalte **Hochverfügbarkeit** aufgelistet, die anzeigt, ob Hochverfügbarkeit für jeden Knoten erfolgreich aktiviert wurde.

:::image type="content" source="./media/howto-hyperscale-high-availability/02-ha-column.png" alt-text="Spalte „Hochverfügbarkeit“ in der Übersicht der Servergruppe":::

### <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Hochverfügbarkeit](concepts-hyperscale-high-availability.md).
