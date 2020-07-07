---
title: Zugriff auf öffentliches Netzwerk verweigern – Azure-Portal – Azure Database for PostgreSQL-Einzelserver
description: Erfahren Sie mehr über das Konfigurieren von „Zugriff auf öffentliches Netzwerk verweigern“ für Ihre Azure Database for PostgreSQL-Einzelserver-Instanz im Azure-Portal.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 4dff2321414721dbd415b468e59aea0ab4b3acee
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "79372732"
---
# <a name="deny-public-network-access-in-azure-database-for-postgresql-single-server-using-azure-portal"></a>„Zugriff auf öffentliches Netzwerk verweigern“ in Azure Database for PostgreSQL-Einzelserver im Azure-Portal

In diesem Artikel wird beschrieben, wie Sie eine Azure Database for PostgreSQL-Einzelserver-Instanz so konfigurieren können, dass alle öffentlichen Konfigurationen verweigert und nur Verbindungen über private Endpunkte zugelassen werden, um die Netzwerksicherheit weiter zu erhöhen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

* [Azure Database for PostgreSQL-Einzelserver](quickstart-create-PostgreSQL Single server-server-database-using-azure-portal.md)

## <a name="set-deny-public-network-access"></a>Festlegen von „Zugriff auf öffentliches Netzwerk verweigern“

Gehen Sie wie folgt vor, um „Zugriff auf öffentliches Netzwerk verweigern“ für den PostgreSQL-Einzelserver zu aktivieren:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihre vorhandene Azure Database for PostgreSQL-Einzelserver-Instanz aus.

1. Klicken Sie auf der Seite des PostgreSQL-Einzelservers unter **Einstellungen** auf **Verbindungssicherheit**, um die Seite zur Konfiguration der Verbindungssicherheit zu öffnen.

1. Wählen Sie in **Zugriff auf öffentliches Netzwerk verweigern** die Option **Ja** aus, um den öffentlichen Zugriff für Ihren PostgreSQL-Einzelserver zu verweigern.

    ![Azure Database for PostgreSQL-Einzelserver: Netzwerkzugriff verweigern](./media/howto-deny-public-network-access/deny-public-network-access.PNG)

1. Klicken Sie zum Speichern der Änderungen auf **Speichern**.

1. Eine Benachrichtigung bestätigt, dass die Verbindungssicherheitseinstellung erfolgreich aktiviert wurde.

    ![Azure Database for PostgreSQL-Einzelserver: Netzwerkzugriff erfolgreich verweigert](./media/howto-deny-public-network-access/deny-public-network-access-success.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Warnungen zu Metriken erstellen](howto-alert-on-metric.md).