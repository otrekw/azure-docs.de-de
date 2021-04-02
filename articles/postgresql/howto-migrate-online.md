---
title: Migration zu Azure Database for PostgreSQL (Einzelserver) mit minimaler Ausfallzeit
description: In diesem Artikel wird beschrieben, wie Sie mit Azure Database Migration Service und minimaler Ausfallzeit eine Migration einer PostgreSQL-Datenbank-Instanz zu Azure Database for PostgreSQL (Einzelserver) durchführen.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.openlocfilehash: 27da5f1b731b2cdb0604f91f7f9e78b19ee2908b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92489794"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql---single-server"></a>Migration zu Azure Database for PostgreSQL (Einzelserver) mit minimaler Ausfallzeit
[!INCLUDE[applies-to-postgres-single-flexible-server](includes/applies-to-postgres-single-flexible-server-hyperscale.md)]

Mit der neu eingeführten **fortlaufenden Synchronisationsfunktion** für den [Azure Database Migration Service](https://aka.ms/get-dms) (DMS) können Sie PostgreSQL-Migrationen zu Azure Database for PostgreSQL mit minimaler Ausfallzeit ausführen. Diese Funktion beschränkt die Ausfallzeit der Anwendung.

## <a name="overview"></a>Übersicht
Azure DMS führt einen ersten Ladevorgang Ihrer lokalen Datenbankinstanz nach Azure Database for PostgreSQL durch und synchronisiert dann kontinuierlich alle neuen Transaktionen mit Azure, während die Anwendung weiterhin ausgeführt wird. Nachdem die Daten auf der Azure-Zielseite erfasst wurden, stoppen Sie die Anwendung für einen kurzen Moment (minimale Ausfallzeit), warten auf den letzten Datenbatch (von dem Zeitpunkt an, an dem Sie die Anwendung stoppen, bis zu dem Moment, in dem die Anwendung tatsächlich nicht mehr verfügbar ist, um neuen Datenverkehr aufzunehmen), um die Erfassung im Ziel aufzuholen. Aktualisieren Sie dann Ihre Verbindungszeichenfolge, um auf Azure zu verweisen. Danach ist Ihre Anwendung auf Azure live geschaltet!

:::image type="content" source="./media/howto-migrate-online/ContinuousSync.png" alt-text="Fortlaufende Synchronisierung mit dem Azure Database Migration Service":::

## <a name="next-steps"></a>Nächste Schritte
- Sehen Sie sich das Video [App Modernization with Microsoft Azure (App-Modernisierung mit Microsoft Azure)](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102) an, das eine Demo zum Migrieren von PostgreSQL-Apps zu Azure Database for PostgreSQL enthält.
- Sehen Sie sich das Tutorial [Migrieren von PostgreSQL zu Azure Database for PostgreSQL (online) mit DMS](../dms/tutorial-postgresql-azure-postgresql-online.md) an.