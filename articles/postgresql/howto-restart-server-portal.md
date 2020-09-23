---
title: Neustarten von Servern – Azure-Portal – Azure Database for PostgreSQL (Einzelserver)
description: In diesem Artikel wird beschrieben, wie Sie im Azure-Portal einen Azure Database for PostgreSQL-Einzelserver neu starten können.
author: ajlam
ms.author: andrela
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.openlocfilehash: d25c8908b46608ac64bcf135404a070740413fe0
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90907385"
---
# <a name="restart-azure-database-for-postgresql---single-server-using-the-azure-portal"></a>Neustarten eines Azure Database for PostgreSQL-Einzelservers im Azure-Portal
In diesem Thema wird erläutert, wie Sie einen Azure Database for PostgreSQL-Server neu starten können. Es kann vorkommen, dass Sie Ihren Server zu Wartungszwecken neu starten müssen. In diesem Fall kommt es zu einem kurzen Ausfall, weil der Vorgang vom Server ausgeführt wird.

Der Neustart des Servers wird blockiert, wenn der Dienst ausgelastet ist. Beispielsweise kann der Dienst einen zuvor angeforderten Vorgang (z. B. das Skalieren von virtuellen Kernen) verarbeiten.
 
Die für einen Neustart benötigte Zeit hängt von dem PostgreSQL-Wiederherstellungsprozess ab. Um die Neustartzeit zu verkürzen, empfehlen wir, die Aktivitäten auf dem Server vor dem Neustart auf ein Minimum zu beschränken.

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