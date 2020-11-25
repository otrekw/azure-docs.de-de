---
title: 'Hauptversionsupgrade: Azure-Portal: Azure Database for MySQL Single Server'
description: In diesem Artikel wird beschrieben, wie Sie im Azure-Portal ein Upgrade der Hauptversion für Azure Database for MySQL Single Server durchführen.
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 11/16/2020
ms.openlocfilehash: 78c35e42cefa8897d9f93c3a941b4c0e8b81e5f9
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94686491"
---
# <a name="major-version-upgrade-in-azure-database-for-mysql-single-server-using-the-azure-portal"></a>Hauptversionsupgrade für Azure Database for MySQL Single Server im Azure-Portal

> [!IMPORTANT]
> Das Hauptversionsupgrade für Azure Database for MySQL Single Server befindet sich in der öffentlichen Vorschauphase.

In diesem Artikel wird beschrieben, wie Sie im Azure-Portal ein direktes Upgrade Ihrer MySQL-Hauptversion für Azure Database for MySQL Single Server durchführen.

Mit diesem Feature können Kunden einfach per Klick direkte Upgrades ihrer MySQL 5.6-Server auf MySQL-Version 5.7 durchführen, ohne dass Daten verschoben oder Verbindungszeichenfolgen für Anwendungen geändert werden müssen.

> [!Note]
> * Hauptversionsupgrade ist nur für das Upgrade von MySQL 5.6 auf MySQL 5.7 verfügbar<br>
> * Das Hauptversionsupgrade wird auf dem Replikatserver noch nicht unterstützt.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:
- [Azure Database for MySQL Single Server](quickstart-create-mysql-server-database-using-azure-portal.md)-Instanz

## <a name="perform-major-version-upgrade-from-mysql-56-to-mysql-57"></a>Durchführung des Hauptversionsupgrades von MySQL 5.6 auf MySQL 5.7

Führen Sie die folgenden Schritte aus, um ein Hauptversionsupgrade für Ihren Azure Database for MySQL-Server 5.6 durchzuführen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihren vorhandenen Azure Database for MySQL-Server 5.6 aus.

2. Klicken Sie auf der Seite **Übersicht** in der Symbolleiste auf die Schaltfläche **Upgrade**.

3. Wählen Sie im Abschnitt **Upgrade** die Option **OK** aus, um den Azure Database for MySQL-Server 5.6 auf Version 5.7 zu aktualisieren.

    :::image type="content" source="./media/how-to-major-version-upgrade-portal/upgrade.png" alt-text="Azure Database for MySQL: Übersicht über Upgrade":::

4. Mit einer Benachrichtigung wird bestätigt, dass der Upgradevorgang erfolgreich war.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich unter [Azure Database for MySQL: Versionsrichtlinie](concepts-version-policy.md).