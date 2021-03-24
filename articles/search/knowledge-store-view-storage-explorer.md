---
title: Anzeigen eines Wissensspeichers mit Storage-Explorer
titleSuffix: Azure Cognitive Search
description: Anzeigen und Analysieren eines Azure Cognitive Search-Wissensspeichers mit dem Storage-Explorer des Azure-Portals
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: ba0b02067c032f9038051c169866588ded44af73
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "85566021"
---
# <a name="view-a-knowledge-store-with-storage-explorer"></a>Anzeigen eines Wissensspeichers mit Storage-Explorer

In diesem Artikel erfahren Sie anhand eines Beispiels, wie Sie mit dem Storage-Explorer im Azure-Portal eine Verbindung mit einem Wissensspeicher herstellen und ihn anschließend erkunden.

## <a name="prerequisites"></a>Voraussetzungen

+ Führen Sie die unter [Erstellen eines Wissensspeichers im Azure-Portal](knowledge-store-create-portal.md) beschriebenen Schritte aus, um den Beispielwissensspeicher zu erstellen, der in dieser exemplarischen Vorgehensweise verwendet wird.

+ Sie benötigen außerdem den Namen des Azure-Speicherkontos, das Sie zum Erstellen des Wissensspeichers verwendet haben, sowie den Zugriffsschlüssel aus dem Azure-Portal.

## <a name="view-edit-and-query-a-knowledge-store-in-storage-explorer"></a>Anzeigen, Bearbeiten und Abfragen eines Wissensspeichers im Storage-Explorer

1. [Öffnen Sie das Speicherkonto](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/), das Sie zum Erstellen des Wissensspeichers verwendet haben, im Azure-Portal.

1. Klicken Sie im linken Navigationsbereich des Speicherkontos auf **Storage-Explorer**.

1. Erweitern Sie die Liste **TABELLEN**, um eine Liste der Azure-Tabellenprojektionen anzuzeigen, die erstellt wurden, als Sie den Assistenten zum **Importieren von Daten** für Ihre Beispieldaten zu Hotelrezensionen ausgeführt haben.

Wählen Sie eine beliebige Tabelle aus, um die angereicherten Daten, einschließlich Schlüsselbegriffen und Stimmungspunktzahlen, anzuzeigen.

   ![Anzeigen von Tabellen im Storage-Explorer](media/knowledge-store-view-storage-explorer/storage-explorer-tables.png "Anzeigen von Tabellen im Storage-Explorer")

Zum Ändern des Datentyps für einen beliebigen Tabellenwert oder zum Ändern einzelner Werte in der Tabelle klicken Sie auf **Bearbeiten**. Wenn Sie den Datentyp für eine Spalte in einer Tabellenzeile ändern, erfolgt die Änderung für alle Zeilen.

   ![Bearbeiten einer Tabelle im Storage-Explorer](media/knowledge-store-view-storage-explorer/storage-explorer-edit-table.png "Bearbeiten einer Tabelle im Storage-Explorer")

Klicken Sie zum Ausführen von Abfragen in der Befehlsleiste auf **Abfrage**, und geben Sie Ihre Bedingungen ein.  

   ![Abfragen einer Tabelle im Storage-Explorer](media/knowledge-store-view-storage-explorer/storage-explorer-query-table.png "Abfragen einer Tabelle im Storage-Explorer")

## <a name="clean-up"></a>Bereinigung

Wenn Sie in Ihrem eigenen Abonnement arbeiten, sollten Sie sich am Ende eines Projekts überlegen, ob Sie die erstellten Ressourcen noch benötigen. Ressourcen, die weiterhin ausgeführt werden, können Sie Geld kosten. Sie können entweder einzelne Ressourcen oder aber die Ressourcengruppe löschen, um den gesamten Ressourcensatz zu entfernen.

Ressourcen können im Portal über den Link **Alle Ressourcen** oder **Ressourcengruppen** im linken Navigationsbereich gesucht und verwaltet werden.

Denken Sie bei Verwendung eines kostenlosen Diensts an die Beschränkung auf maximal drei Indizes, Indexer und Datenquellen. Sie können einzelne Elemente über das Portal löschen, um unter dem Limit zu bleiben.

## <a name="next-steps"></a>Nächste Schritte

Verbinden Sie diesen Wissensspeicher zur detaillierteren Analyse mit Power BI, oder fahren Sie mit Code fort, und erstellen Sie mit der REST-API und Postman einen anderen Wissensspeicher.

> [!div class="nextstepaction"]
> [Verbinden mit Power BI](knowledge-store-connect-power-bi.md)
> [Erstellen eines Wissensspeichers in REST](knowledge-store-create-rest.md)
