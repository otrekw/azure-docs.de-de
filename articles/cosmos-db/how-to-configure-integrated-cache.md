---
title: Konfigurieren des integrierten Azure Cosmos DB-Caches
description: Erfahren Sie, wie Sie den integrierten Azure Cosmos DB-Cache konfigurieren.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/25/2021
ms.author: tisande
ms.openlocfilehash: f857d9945cc52aa192838d58066a7fcc005a622d
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385492"
---
# <a name="how-to-configure-the-azure-cosmos-db-integrated-cache-preview"></a>Konfigurieren des integrierten Azure Cosmos DB-Caches (Vorschau)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

In diesem Artikel wird beschrieben, wie Sie ein dediziertes Gateway bereitstellen, den integrierten Cache konfigurieren und eine Verbindung mit Ihrer Anwendung herstellen. 

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie kein [Azure-Abonnement](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing) besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), bevor Sie beginnen.
- Eine vorhandene Anwendung, die Azure Cosmos DB nutzt. Wenn Sie noch keine haben, finden Sie [hier einige Beispiele](https://github.com/AzureCosmosDB/labs).
- Ein vorhandenes [Azure Cosmos DB-SQL-(Core)-API-Konto](create-cosmosdb-resources-portal.md)

## <a name="provision-a-dedicated-gateway-cluster"></a>Bereitstellen eines dedizierten Gatewayclusters

1. Navigieren Sie im Azure-Portal zu einem Azure Cosmos DB-Konto, und wählen Sie die Registerkarte **Dediziertes Gateway** aus.

   :::image type="content" source="./media/how-to-configure-integrated-cache/dedicated-gateway-tab.png" alt-text="Abbildung der Navigation zur Registerkarte „Dediziertes Gateway“" lightbox="./media/how-to-configure-integrated-cache/dedicated-gateway-tab.png" border="false":::

2. Füllen Sie das Formular **Dediziertes Gateway** mit den folgenden Details aus:

   * **Dediziertes Gateway:** Aktivieren Sie den Umschalter **Bereitgestellt**. 
   * **SKU:** Wählen Sie eine SKU mit der benötigten Compute- und Arbeitsspeichergröße aus. 
   *  **Anzahl von Instanzen:** Anzahl der Knoten. Für die Entwicklung wird zu Beginn ein Knoten der Größe D4 empfohlen. Sie können die Knotengröße nach den ersten Tests basierend auf der Datenmenge erhöhen, die Sie zwischenspeichern müssen.

   :::image type="content" source="./media/how-to-configure-integrated-cache/dedicated-gateway-input.png" alt-text="Abbildung mit Beispieleingabeeinstellungen zum Erstellen eines dedizierten Gatewayclusters" lightbox="./media/how-to-configure-integrated-cache/dedicated-gateway-input.png" border="false":::

3. Wählen Sie **Speichern** aus, und warten Sie ca. 5 bis 10 Minuten, bis die Bereitstellung des dedizierten Gateways abgeschlossen ist. Nach Abschluss der Bereitstellung wird die folgende Benachrichtigung angezeigt:

   :::image type="content" source="./media/how-to-configure-integrated-cache/dedicated-gateway-notification.png" alt-text="Abbildung der Überprüfung, ob die Bereitstellung des dedizierten Gateways abgeschlossen ist" lightbox="./media/how-to-configure-integrated-cache/dedicated-gateway-notification.png" border="false":::

## <a name="configuring-the-integrated-cache"></a>Konfigurieren des integrierten Caches

1. Wenn Sie ein dediziertes Gateway erstellen, wird automatisch auch integrierter Cache bereitgestellt. Für den integrierten Cache werden ungefähr 70 % des Arbeitsspeichers des dedizierten Gateways genutzt. Die verbleibenden 30 % des Arbeitsspeichers des dedizierten Gateways werden zum Weiterleiten von Anforderungen an die Back-End-Partitionen verwendet.

2.  Ändern Sie die Verbindungszeichenfolge Ihrer Anwendung, damit sie den neuen dedizierten Gatewayendpunkt verwendet.

      Sie finden die aktualisierte Verbindungszeichenfolge des dedizierten Gateways auf dem Blatt **Schlüssel**:
   
      :::image type="content" source="./media/how-to-configure-integrated-cache/dedicated-gateway-connection-string.png" alt-text="Abbildung der Verbindungszeichenfolge eines dedizierten Gateways" lightbox="./media/how-to-configure-integrated-cache/dedicated-gateway-connection-string.png" border="false":::

      Alle Verbindungszeichenfolgen für dedizierte Gateways folgen dem gleichen Muster. Entfernen Sie `documents.azure.com` aus der ursprünglichen Verbindungszeichenfolge, und ersetzen Sie sie durch `sqlx.cosmos.azure.com`. Ein dediziertes Gateway hat immer dieselbe Verbindungszeichenfolge, selbst wenn Sie es entfernen und neu bereitstellen.

      Sie müssen die Verbindungszeichenfolge in Anwendungen, die dasselbe Azure Cosmos DB-Konto verwenden, nicht ändern. Beispielsweise können Sie einen `CosmosClient` im Gatewaymodus über den dedizierten Gatewayendpunkt verbinden, während ein anderer `CosmosClient` den direkten Modus verwendet. Anders ausgedrückt: Das Hinzufügen eines dedizierten Gateways wirkt sich nicht auf die vorhandenen Verbindungsmöglichkeiten mit Azure Cosmos DB aus.

3. Wenn Sie das .NET oder Java SDK verwenden, legen Sie als Verbindungsmodus den [Gatewaymodus](sql-sdk-connection-modes.md#available-connectivity-modes) fest. Dieser Schritt ist für die Python und Node.js SDKs nicht erforderlich, da sie neben dem Gatewaymodus keine weiteren Optionen zum Herstellen von Verbindungen bieten.

## <a name="adjust-request-consistency"></a>Anpassen der Anforderungskonsistenz

Sie müssen die Anforderungskonsistenz auf „Letztliche Konsistenz“ festlegen. Andernfalls wird der integrierte Cache immer von der Anforderung umgangen. Die einfachste Vorgehensweise zum Konfigurieren der letztlichen Konsistenz für alle Lesevorgänge besteht darin, [diese auf Kontoebene festzulegen](consistency-levels.md#configure-the-default-consistency-level). Sie können die Konsistenz auch auf [Anforderungsebene](how-to-manage-consistency.md#override-the-default-consistency-level) konfigurieren. Dies wird empfohlen, wenn der integrierte Cache nur von einem Teil Ihrer Lesezugriffe verwendet werden soll.

> [!NOTE]
> Wenn Sie das Python SDK verwenden, **müssen** Sie die Konsistenzebene für jede Anforderung explizit festlegen. Die Standardeinstellung auf Kontoebene wird nicht automatisch angewandt.

## <a name="verify-cache-hits"></a>Überprüfen von Cachetreffern

Zum Abschluss starten Sie Ihre Anwendung neu und führen eine Überprüfung auf Treffer im integrierten Cache für wiederholte Punktlesevorgänge oder Abfragen durch. Nachdem Sie Ihren `CosmosClient` so geändert haben, dass er den dedizierten Gatewayendpunkt verwendet, werden alle Anforderungen über das dedizierte Gateway weitergeleitet.

Damit eine Leseanforderung (Punktlesevorgang oder Abfrage) den integrierten Cache verwendet, müssen **alle** der folgenden Kriterien erfüllt sein:

-   Ihr Client stellt eine Verbindung mit dem dedizierten Gatewayendpunkt her.
-  Ihr Client verwendet den Gatewaymodus (beim Python und Node.js SDK wird immer der Gatewaymodus verwendet).
-   Die Konsistenz für die Anforderung muss auf „Letztliche Konsistenz“ festgelegt sein.

## <a name="next-steps"></a>Nächste Schritte

- [Häufig gestellte Fragen zum integrierten Cache](integrated-cache-faq.md)
- [Übersicht über den integrierten Cache](integrated-cache.md)
- [Dediziertes Gateway](dedicated-gateway.md)
