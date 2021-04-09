---
title: Zugriff auf öffentliches Netzwerk verweigern – Azure-Portal – Azure Database for PostgreSQL-Einzelserver
description: Erfahren Sie mehr über das Konfigurieren von „Zugriff auf öffentliches Netzwerk verweigern“ für Ihre Azure Database for PostgreSQL-Einzelserver-Instanz im Azure-Portal.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: how-to
ms.date: 03/10/2020
ms.openlocfilehash: e195c005676df27385e5e00736b04bdb689fafc5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101727106"
---
# <a name="deny-public-network-access-in-azure-database-for-postgresql-single-server-using-azure-portal"></a>„Zugriff auf öffentliches Netzwerk verweigern“ in Azure Database for PostgreSQL-Einzelserver im Azure-Portal

In diesem Artikel wird beschrieben, wie Sie eine Azure Database for PostgreSQL-Einzelserver-Instanz so konfigurieren können, dass alle öffentlichen Konfigurationen verweigert und nur Verbindungen über private Endpunkte zugelassen werden, um die Netzwerksicherheit weiter zu erhöhen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

* Einen [Azure Database for PostgreSQL-Einzelserver](quickstart-create-server-database-portal.md) mit Tarif „Universell“ oder „Arbeitsspeicheroptimiert“.

## <a name="set-deny-public-network-access"></a>Festlegen von „Zugriff auf öffentliches Netzwerk verweigern“

Gehen Sie wie folgt vor, um „Zugriff auf öffentliches Netzwerk verweigern“ für den PostgreSQL-Einzelserver zu aktivieren:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihre vorhandene Azure Database for PostgreSQL-Einzelserver-Instanz aus.

1. Klicken Sie auf der Seite des PostgreSQL-Einzelservers unter **Einstellungen** auf **Verbindungssicherheit**, um die Seite zur Konfiguration der Verbindungssicherheit zu öffnen.

1. Wählen Sie in **Zugriff auf öffentliches Netzwerk verweigern** die Option **Ja** aus, um den öffentlichen Zugriff für Ihren PostgreSQL-Einzelserver zu verweigern.

    :::image type="content" source="./media/howto-deny-public-network-access/deny-public-network-access.PNG" alt-text="Azure Database for PostgreSQL-Einzelserver: Netzwerkzugriff verweigern":::

1. Klicken Sie zum Speichern der Änderungen auf **Speichern**.

1. Eine Benachrichtigung bestätigt, dass die Verbindungssicherheitseinstellung erfolgreich aktiviert wurde.

    :::image type="content" source="./media/howto-deny-public-network-access/deny-public-network-access-success.png" alt-text="Azure Database for PostgreSQL-Einzelserver: Netzwerkzugriff erfolgreich verweigert":::

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Warnungen zu Metriken erstellen](howto-alert-on-metric.md).
