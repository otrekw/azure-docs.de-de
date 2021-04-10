---
title: Neustarten von Servern – Azure-Portal – Azure Database for PostgreSQL (Einzelserver)
description: In diesem Artikel wird beschrieben, wie Sie im Azure-Portal einen Azure Database for PostgreSQL-Einzelserver neu starten können.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: how-to
ms.date: 12/20/2020
ms.openlocfilehash: 1a1afabd606df70ec60cf4fa7c8530ff95a0564f
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105604854"
---
# <a name="restart-azure-database-for-postgresql---single-server-using-the-azure-portal"></a>Neustarten eines Azure Database for PostgreSQL-Einzelservers im Azure-Portal
In diesem Thema wird erläutert, wie Sie einen Azure Database for PostgreSQL-Server neu starten können. Es kann vorkommen, dass Sie Ihren Server zu Wartungszwecken neu starten müssen. In diesem Fall kommt es zu einem kurzen Ausfall, weil der Vorgang vom Server ausgeführt wird.

Der Neustart des Servers wird blockiert, wenn der Dienst ausgelastet ist. Beispielsweise kann der Dienst einen zuvor angeforderten Vorgang (z. B. das Skalieren von virtuellen Kernen) verarbeiten.
 
> [!NOTE] 
> Die für einen Neustart benötigte Zeit hängt von dem PostgreSQL-Wiederherstellungsprozess ab. Um die Neustartzeit zu verkürzen, empfehlen wir, die Aktivitäten auf dem Server vor dem Neustart auf ein Minimum zu beschränken. Sie sollten zudem die Prüfpunkthäufigkeit erhöhen. Darüber hinaus können Sie prüfpunktbezogene Parameterwerte optimieren, einschließlich `max_wal_size`. Außerdem wird empfohlen, vor dem Neustart des Servers den Befehl `CHECKPOINT` auszuführen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:
- Einen [Azure-Datenbank für PostgreSQL-Server](quickstart-create-server-database-portal.md)

## <a name="perform-server-restart"></a>Ausführen des Serverneustarts

Mit den folgenden Schritten wird der PostgreSQL-Server neu gestartet:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihren Azure Database for PostgreSQL-Server aus.

2. Klicken Sie auf der Seite **Übersicht** des Servers auf der Symbolleiste auf **Neu starten**.

   :::image type="content" source="./media/howto-restart-server-portal/2-server.png" alt-text="Azure Database for PostgreSQL – Übersicht – Schaltfläche „Neu starten“":::

3. Klicken Sie auf **Ja**, um den Neustart des Servers zu bestätigen.

   :::image type="content" source="./media/howto-restart-server-portal/3-restart-confirm.png" alt-text="Azure Database for PostgreSQL – Neustart bestätigen":::

4. Beachten Sie, dass sich der Serverstatus in „Wird neu gestartet“ ändert.

   :::image type="content" source="./media/howto-restart-server-portal/4-restarting-status.png" alt-text="Azure Database for PostgreSQL – Status „Wird neu gestartet“":::

5. Vergewissern Sie sich, dass der Neustart des Servers erfolgreich war.

   :::image type="content" source="./media/howto-restart-server-portal/5-restart-success.png" alt-text="Azure Database for PostgreSQL – Erfolgreicher Neustart":::

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Festlegen von Parametern in Azure Database for PostgreSQL](howto-configure-server-parameters-using-portal.md)
