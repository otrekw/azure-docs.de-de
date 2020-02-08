---
title: Verbindung einer verwalteten Azure SQL-Datenbank-Instanz für die Suchindizierung
titleSuffix: Azure Cognitive Search
description: Aktivieren Sie den öffentlichen Endpunkt, um Verbindungen mit verwalteten SQL-Datenbank-Instanzen von einem Indexer in der kognitiven Azure-Suche zuzulassen.
manager: nitinme
author: vl8163264128
ms.author: victliu
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 65e483fd772e20daa73b465ea17dfa6ecde42233
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964888"
---
# <a name="configure-a-connection-from-an-azure-cognitive-search-indexer-to-sql-managed-instance"></a>Konfigurieren einer Verbindung eines Indexers der kognitiven Azure-Suche mit einer verwalteten SQL-Datenbank-Instanz

Wie unter [Verbinden von Azure SQL-Datenbank mit der kognitiven Azure-Suche mithilfe von Indexern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq) beschrieben, wird das Erstellen von Indexern für **verwaltete SQL-Datenbank-Instanzen** in der kognitiven Azure-Suche über den öffentlichen Endpunkt unterstützt.

## <a name="create-azure-sql-managed-instance-with-public-endpoint"></a>Erstellen einer verwalteten Azure SQL-Instanz mit öffentlichem Endpunkt
Erstellen Sie eine verwaltete SQL-Instanz mit der ausgewählten Option **Öffentlichen Endpunkt aktivieren**.

   ![Öffentlichen Endpunkt aktivieren](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/enable-public-endpoint.png "Aktivieren des öffentlichen Endpunkts")

## <a name="enable-azure-sql-managed-instance-public-endpoint"></a>Aktivieren einer verwalteten Azure SQL-Instanz mit öffentlichem Endpunkt
Sie können den öffentlichen Endpunkt auch auf einer vorhandenen verwalteten SQL-Instanz unter **Sicherheit** > **Virtuelles Netzwerk** > **Öffentlicher Endpunkt**  > **Aktivieren** aktivieren.

   ![Öffentlichen Endpunkt aktivieren](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-vnet.png "Aktivieren des öffentlichen Endpunkts")

## <a name="verify-nsg-rules"></a>Überprüfen von NSG-Regeln
Überprüfen Sie, ob die Netzwerksicherheitsgruppe die richtigen **Eingangssicherheitsregeln** hat, die Verbindungen von Azure-Diensten erlauben.

   ![NSG-Eingangssicherheitsregel](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/nsg-rule.png "NSG-Eingangssicherheitsregel")

> [!NOTE]
> Indexer erfordern weiterhin, dass die verwaltete SQL-Instanz mit einem öffentlichen Endpunkt konfiguriert wird, um Daten lesen zu können.
> Sie können jedoch festlegen, dass der eingehende Zugriff auf diesen öffentlichen Endpunkt eingeschränkt werden soll, indem Sie die aktuelle Regel (`public_endpoint_inbound`) durch die folgenden zwei Regeln ersetzen:
>
> * Eingehenden Zugriff über das `AzureCognitiveSearch`-[Diensttag](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags) zulassen ("SOURCE" = `AzureCognitiveSearch`, "NAME" = `cognitive_search_inbound`)
>
> * Eingehenden Zugriff von der IP-Adresse des Suchdienstanbieters zulassen; kann durch Pingen des vollqualifizierten Domänennamens abgerufen werden (z. B. `<your-search-service-name>.search.windows.net`). ("SOURCE" = `IP address`, "NAME" = `search_service_inbound`)
>
> Legen Sie für jede dieser beiden Regeln "PORT" = `3342`, "PROTOCOL" = `TCP`, "DESTINATION" = `Any`, "ACTION" = `Allow` fest.

## <a name="get-public-endpoint-connection-string"></a>Abrufen der Verbindungszeichenfolge für den öffentlichen Endpunkt
Stellen Sie sicher, dass Sie die Verbindungszeichenfolge für den **öffentlichen Endpunkt** verwenden (Port 3342, nicht Port 1433).

   ![Verbindungszeichenfolge für den öffentlichen Endpunkt](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-connection-string.png "Verbindungszeichenfolge für den öffentlichen Endpunkt")

## <a name="next-steps"></a>Nächste Schritte
Da die Konfiguration nun abgeschlossen ist, können Sie jetzt entweder über das Portal oder mit der REST-API eine verwaltete SQL-Datenbank-Instanz als Datenquelle für einen Indexer der kognitiven Azure-Suche angeben. Weitere Informationen finden Sie unter [Verbinden von Azure SQL-Datenbank mit der kognitiven Azure-Suche mithilfe von Indexern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md).
