---
title: Konfigurieren von Serverparametern – Azure-Portal – Azure Database for PostgreSQL – Einzelserver
description: In diesem Artikel wird beschrieben, wie Sie die Postgres-Parameter in Azure Database for PostgreSQL mithilfe des Azure-Portals konfigurieren.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: a9d078fe9aab12b9044733d17a1437801d5130a4
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74763673"
---
# <a name="configure-server-parameters-in-azure-database-for-postgresql---single-server-via-the-azure-portal"></a>Konfigurieren von Serverparametern in Azure Database for PostgreSQL (Einzelserver) über das Azure-Portal 
Sie können Konfigurationsparameter für einen Azure Database for PostgreSQL-Server mit dem Azure-Portal auflisten, anzeigen und aktualisieren.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- [Azure Database for PostgreSQL-Server](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>Anzeigen und Bearbeiten von Parametern
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

2. Wählen Sie Ihre Azure Database for PostgreSQL-Server aus.

3. Wählen Sie im Abschnitt **EINSTELLUNGEN** die Option **Serverparameter**. Auf der Seite werden eine Liste mit Parametern und die dazugehörigen Werte und Beschreibungen angezeigt.
![Übersichtsseite für Parameter](./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png)

4. Wählen Sie die **Dropdownschaltfläche** , um die möglichen Werte für Enumerierungsparameter wie „client_min_messages“ anzuzeigen.
![Dropdownliste für Enumerierung](./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png)

5. Wählen Sie die Schaltfläche **i** (Informationen), oder zeigen Sie mit der Maus darauf, um den Bereich mit den möglichen Werten für numerische Parameter wie „cpu_index_tuple_cost“ anzuzeigen.
![Schaltfläche „Informationen“](./media/howto-configure-server-parameters-in-portal/4-information-button.png)

6. Verwenden Sie bei Bedarf das **Suchfeld**, um nach einem bestimmten Parameter zu suchen. Die Suche wird für den Namen und die Beschreibung der Parameter durchgeführt.
![Suchergebnisse](./media/howto-configure-server-parameters-in-portal/5-search.png)

7. Ändern Sie die Parameterwerte, die Sie anpassen möchten. Alle in einer Sitzung vorgenommenen Änderungen werden violett hervorgehoben. Nachdem Sie die Werte geändert haben, können Sie **Speichern** wählen. Sie können Ihre Änderungen auch **verwerfen**.
![Änderungen speichern oder verwerfen](./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png)

8. Wenn Sie neue Werte für die Parameter gespeichert haben, können Sie jederzeit alles zurück auf die Standardwerte setzen, indem Sie die Option **Alle auf Standard zurücksetzen** wählen.
![Alle auf Standard zurücksetzen](./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png)

## <a name="next-steps"></a>Nächste Schritte
Sie erhalten Informationen zu folgenden Themen:
- [Azure Database for PostgreSQL-Server](concepts-servers.md)
- [Anpassen der Serverkonfigurationsparameter mithilfe der Azure CLI](howto-configure-server-parameters-using-cli.md)
