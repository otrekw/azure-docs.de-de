---
title: Konfigurieren von Protokollen für langsame Abfragen – Azure-Portal – Azure Database for MySQL – Flexible Server
description: In diesem Artikel wird beschrieben, wie Sie die Protokolle für langsame Abfragen in Azure Database for MySQL – Flexible Server im Azure-Portal konfigurieren und auf die Protokolle zugreifen.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 9/21/2020
ms.openlocfilehash: e2046673cda17c58153ceb12eee31edb83365092
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91565707"
---
# <a name="configure-and-access-slow-query-logs-for-azure-database-for-mysql---flexible-server-using-the-azure-portal"></a>Konfigurieren von Protokollen für langsame Abfragen für Azure Database for MySQL – Flexible Server und Zugreifen auf diese Protokolle im Azure-Portal

> [!IMPORTANT]
> Azure Database for MySQL Flexible Server befindet sich aktuell in der öffentlichen Vorschau.

Sie können die [Protokolle für langsame Abfragen](concepts-slow-query-logs.md) für Azure Database for MySQL Flexible Server im Azure-Portal konfigurieren und auflisten sowie aus dem Portal herunterladen.

## <a name="prerequisites"></a>Voraussetzungen
Die Schritte in diesem Artikel erfordern [Flexible Server](quickstart-create-server-portal.md).

## <a name="configure-logging"></a>Konfigurieren der Protokollierung
Konfigurieren Sie den Zugriff auf das MySQL-Protokoll für langsame Abfragen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie Ihre Flexible Server-Instanz aus.

1. Wählen Sie auf der Randleiste im Abschnitt **Einstellungen** den Eintrag **Serverparameter** aus.
   :::image type="content" source="./media/how-to-configure-slow-query-logs-portal/server-parameters.png" alt-text="Seite „Serverparameter“":::

1. Aktualisieren Sie den Parameter **slow_query_log** auf **ON** (EIN).
   :::image type="content" source="./media/how-to-configure-slow-query-logs-portal/slow-query-log-enable.png" alt-text="Seite „Serverparameter“":::

1. Ändern Sie alle anderen erforderlichen Parameter (z. B. `long_query_time`, `log_slow_admin_statements`). Weitere Parameter finden Sie in der Dokumentation für [Protokolle für langsame Abfragen](./concepts-slow-query-logs.md#configure-slow-query-logging).  
   :::image type="content" source="./media/how-to-configure-slow-query-logs-portal/long-query-time.png" alt-text="Seite „Serverparameter“":::

1. Wählen Sie **Speichern**. 
   :::image type="content" source="./media/how-to-configure-slow-query-logs-portal/save-parameters.png" alt-text="Seite „Serverparameter“":::

Von der Seite **Serverparameter** können Sie zur Liste der Protokolle zurückkehren, indem Sie die Seite schließen.

## <a name="set-up-diagnostics"></a>Einrichten der Diagnose

Protokolle für langsame Abfragen sind in die Diagnoseeinstellungen von Azure Monitor integriert, sodass Sie Ihre Protokolle an Azure Monitor-Protokolle, Event Hubs oder Azure Storage weiterleiten können.

1. Wählen Sie auf der Randleiste im Abschnitt **Überwachung** die Option **Diagnoseeinstellungen** > **Diagnoseeinstellungen hinzufügen** aus.

   :::image type="content" source="./media/how-to-configure-slow-query-logs-portal/add-diagnostic-setting.png" alt-text="Seite „Serverparameter“":::

1. Geben Sie einen Namen für die Diagnoseeinstellung ein.

1. Geben Sie an, an welche Ziele die langsamen Abfrageprotokolle gesendet werden sollen (Speicherkonto, Event Hub oder Log Analytics-Arbeitsbereich).

1. Wählen Sie als Protokolltyp **MySqlSlowLogs** aus.
    :::image type="content" source="./media/how-to-configure-slow-query-logs-portal/configure-diagnostic-setting.png" alt-text="Seite „Serverparameter“":::

1. Nachdem Sie die Datensenken für die langsamen Abfrageprotokolle konfiguriert haben, wählen Sie **Speichern** aus.
    :::image type="content" source="./media/how-to-configure-slow-query-logs-portal/save-diagnostic-setting.png" alt-text="Seite „Serverparameter“":::

1. Greifen Sie auf die langsamen Abfrageprotokolle zu, indem Sie sie in den von Ihnen konfigurierten Datensenken einsehen. Es kann bis zu 10 Minuten dauern, bis die Protokolle angezeigt werden.

Wenn Sie Ihre Protokolle über eine Pipeline an Azure Monitor-Protokolle (Log Analytics) weitergeleitet haben, können Sie sich unter [Analysieren von Protokollen in Azure Monitor-Protokollen](concepts-slow-query-logs.md#analyze-logs-in-azure-monitor-logs) einige Beispielabfragen ansehen, die Sie zur Analyse verwenden können. 

## <a name="next-steps"></a>Nächste Schritte
<!-- - See [Access slow query Logs in CLI](howto-configure-server-logs-in-cli.md) to learn how to download slow query logs programmatically.-->
- Erfahren Sie mehr über [Protokolle für langsame Abfragen](concepts-slow-query-logs.md).
- Weitere Informationen zu den Parameterdefinitionen und der MySQL-Protokollierung finden Sie in der MySQL-Dokumentation unter [Protokolle](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).
- Erfahren Sie mehr über [Überwachungsprotokolle](concepts-audit-logs.md).
