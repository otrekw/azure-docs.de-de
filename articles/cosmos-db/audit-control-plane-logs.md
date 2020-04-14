---
title: Überwachen von Azure Cosmos DB-Vorgängen auf Steuerungsebene
description: Hier erfahren Sie, wie Sie Vorgänge wie das Hinzufügen einer Region, Aktualisieren des Durchsatzes, Durchführen von Failovern für Regionen und das Hinzufügen eines VNETs auf Steuerungsebene in Azure Cosmos DB überwachen.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: sngun
ms.openlocfilehash: 64ad8e6b1101d8486268c857b3a7752e1801f52c
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2020
ms.locfileid: "80420286"
---
# <a name="how-to-audit-azure-cosmos-db-control-plane-operations"></a>Überwachen von Azure Cosmos DB-Vorgängen auf Steuerungsebene

Vorgänge auf Steuerungsebene schließen Änderungen am Azure Cosmos-Konto oder -Container ein. Vorgänge auf Steuerungsebene sind beispielsweise das Erstellen eines Azure Cosmos-Kontos, das Hinzufügen einer Region, das Aktualisieren des Durchsatzes, das Durchführen von Failovern für Regionen und das Hinzufügen von VNETs. In diesem Artikel wird erläutert, wie die Vorgänge auf Steuerungsebene in Azure Cosmos DB überwacht werden.

## <a name="disable-key-based-metadata-write-access"></a>Deaktivieren des schlüsselbasierten Metadatenschreibzugriffs
 
Bevor Sie die Vorgänge auf Steuerungsebene in Azure Cosmos DB überwachen, deaktivieren Sie den schlüsselbasierten Metadatenschreibzugriff für Ihr Konto. Wenn der schlüsselbasierte Metadatenschreibzugriff deaktiviert ist, können Clients, die über Kontoschlüssel eine Verbindung mit dem Azure Cosmos-Konto herstellen, nicht auf das Konto zugreifen. Sie können den Schreibzugriff deaktivieren, indem Sie Eigenschaft `disableKeyBasedMetadataWriteAccess` auf „true“ festlegen. Nachdem Sie diese Eigenschaft festgelegt haben, können nur von einem Benutzer Änderungen an allen Ressourcen vorgenommen werden, der über die richtige RBAC-Rolle (rollenbasierte Zugriffssteuerung) und Anmeldeinformationen verfügt. Weitere Informationen zum Festlegen dieser Eigenschaft finden Sie im Artikel [Verhindern von Änderungen im Cosmos SDK](role-based-access-control.md#preventing-changes-from-cosmos-sdk).

 Beachten Sie die folgenden Punkte, wenn Sie den Metadatenschreibzugriff deaktivieren:

* Überprüfen Sie die Einstellungen, und stellen Sie sicher, dass Ihre Anwendungen mithilfe des SDK oder mit Kontoschlüsseln keine Metadatenaufrufe durchführen, die die oben genannten Ressourcen ändern (z. B. Sammlung erstellen, Durchsatz aktualisieren usw.).

* Derzeit verwendet das Azure-Portal Kontoschlüssel für Metadatenvorgänge, weshalb diese Vorgänge blockiert werden. Verwenden Sie alternativ die Azure CLI, SDKs oder Resource Manager-Vorlagenbereitstellungen, um solche Vorgänge auszuführen.

## <a name="enable-diagnostic-logs-for-control-plane-operations"></a>Aktivieren von Diagnoseprotokollen für Vorgänge auf Steuerungsebene

Mithilfe des Azure-Portals können Sie Diagnoseprotokolle für Vorgänge auf Steuerungsebene aktivieren. Führen Sie die folgenden Schritte aus, um die Protokollierung für Vorgänge auf Steuerungsebene zu aktivieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem Azure Cosmos-Konto.

1. Öffnen Sie den Bereich **Diagnoseeinstellungen**, und geben Sie einen **Namen** für die zu erstellenden Protokolle an.

1. Wählen Sie **ControlPlaneRequests** als Protokolltyp aus, und klicken Sie dann auf die Option **An Log Analytics senden**.

Sie können die Protokolle auch in einem Speicherkonto oder Stream in einem Event Hub speichern. In diesem Artikel wird gezeigt, wie Sie Protokolle an Log Analytics senden und diese Abfragen. Nach der Aktivierung dauert es einige Minuten, bis die Diagnoseprotokolle erstellt werden. Alle ab diesem Zeitpunkt durchgeführten Vorgänge auf Steuerungsebene können nachverfolgt werden. Der folgende Screenshot veranschaulicht die Aktivierung von Protokollen auf Steuerungsebene:

![Aktivieren der Anforderungsprotokollierung auf Steuerungsebene](./media/audit-control-plane-logs/enable-control-plane-requests-logs.png)

## <a name="view-the-control-plane-operations"></a>Anzeigen von Vorgängen auf Steuerungsebene

Nachdem Sie die Protokollierung aktiviert haben, führen Sie die folgenden Schritte aus, um Vorgänge für ein bestimmtes Konto nachzuverfolgen:

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com)an.
1. Öffnen Sie im linken Navigationsbereich die Registerkarte **Überwachen**, und klicken Sie dann auf den Bereich **Protokolle**. Es wird eine Benutzeroberfläche geöffnet, mit der Sie problemlos Abfragen mit diesem spezifischen Konto im Bereich ausführen können. Führen Sie die folgende Abfrage aus, um die Protokolle auf Steuerungsebene anzuzeigen:

   ```kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="ControlPlaneRequests"
   | where TimeGenerated >= ago(1h)
   ```

Die folgenden Screenshots zeigen Protokolle für den Fall, dass einem Azure Cosmos-Konto ein VNET hinzugefügt wird:

![Protokolle auf Steuerungsebene beim Hinzufügen eines VNET](./media/audit-control-plane-logs/add-ip-filter-logs.png)

Die folgenden Screenshots zeigen Protokolle für den Fall, dass der Durchsatz einer Cassandra-Tabelle aktualisiert wird:

![Protokolle auf Steuerungsebene beim Aktualisieren des Durchsatzes](./media/audit-control-plane-logs/throughput-update-logs.png)

## <a name="identify-the-identity-associated-to-a-specific-operation"></a>Identifizieren der einem bestimmten Vorgang zugeordneten Identität

Wenn Sie weitere debuggen möchten, können Sie einen bestimmten Vorgang im **Aktivitätsprotokoll** ermitteln, indem Sie die Aktivitäts-ID oder den Zeitstempel des Vorgangs verwenden. Der Zeitstempel wird für einige Resource Manager-Clients verwendet, bei denen die Aktivitäts-ID nicht explizit übermittelt wurde. Das Aktivitätsprotokoll enthält Details zur Identität, mit der der Vorgang initiiert wurde. Der folgende Screenshot veranschaulicht, wie Sie die Aktivitäts-ID verwenden und die damit verbundenen Vorgänge im Aktivitätsprotokoll ermitteln:

![Verwenden der Aktivitäts-ID und Suchen der Vorgänge](./media/audit-control-plane-logs/find-operations-with-activity-id.png)

## <a name="next-steps"></a>Nächste Schritte

* [Informationen zu Azure Monitor für Azure Cosmos DB (Vorschau)](../azure-monitor/insights/cosmosdb-insights-overview.md?toc=/azure/cosmos-db/toc.json&bc=/azure/cosmos-db/breadcrumb/toc.json)
* [Überwachen und Debuggen mit Metriken in Azure Cosmos DB](use-metrics.md)