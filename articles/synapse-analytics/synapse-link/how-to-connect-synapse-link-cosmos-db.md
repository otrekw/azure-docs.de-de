---
title: Herstellen einer Verbindung mit Azure Synapse Link (Vorschau) für Azure Cosmos DB
description: Hier erfahren Sie, wie Sie mithilfe von Azure Synapse Link eine Azure Cosmos DB-Instanz mit einem Synapse-Arbeitsbereich verbinden.
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: synapse-link
ms.date: 04/21/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: f453c074126e448eb54d432532b9064b3eb3cb35
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91287891"
---
# <a name="connect-to-azure-synapse-link-preview-for-azure-cosmos-db"></a>Herstellen einer Verbindung mit Azure Synapse Link (Vorschau) für Azure Cosmos DB

In diesem Artikel wird beschrieben, wie Sie über Azure Synapse Analytics Studio mit Azure Synapse Link auf eine Azure Cosmos DB-Datenbank zugreifen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie eine Azure Cosmos DB-Datenbank mit Ihrem Arbeitsbereich verbinden können, benötigen Sie Folgendes:

> [!IMPORTANT]
> Azure Synapse Link für Azure Cosmos DB wird derzeit für Arbeitsbereiche unterstützt, in denen kein verwaltetes virtuelles Netzwerk aktiviert ist. 

* Eine vorhandene Azure Cosmos DB-Datenbank. Erstellen Sie alternativ anhand [dieser Schnellstartanleitung](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-database-account) ein neues Konto.
* Einen vorhandenen Synapse-Arbeitsbereich. Erstellen Sie alternativ anhand [dieser Schnellstartanleitung](https://docs.microsoft.com/azure/synapse-analytics/quickstart-create-workspace) einen neuen Arbeitsbereich. 

## <a name="enable-azure-cosmos-db-analytical-store"></a>Aktivieren des Azure Cosmos DB-Analysespeichers

Es empfiehlt sich, Synapse Link für Azure Cosmos DB zu aktivieren, wenn Sie umfangreiche Analysen in Azure Cosmos DB ohne Beeinträchtigung der betrieblichen Leistung ausführen möchten. Synapse Link bietet HTAP-Funktionen für Container sowie integrierte Unterstützung in Azure Synapse.

## <a name="navigate-to-synapse-studio"></a>Navigieren zu Synapse Studio

Wählen Sie in Ihrem Synapse-Arbeitsbereich die Option **Synapse Studio starten** aus. Wählen Sie auf der Synapse Studio-Startseite „Daten“ aus. Dadurch gelangen Sie zum **Datenobjekt-Explorer**.

## <a name="connect-an-azure-cosmos-db-database-to-a-synapse-workspace"></a>Herstellen einer Verbindung zwischen einer Azure Cosmos DB-Datenbank und einem Synapse-Arbeitsbereich

Die Verbindung mit einer Azure Cosmos DB-Datenbank wird über einen verknüpften Dienst hergestellt. Ein verknüpfter Azure Cosmos DB-Dienst ermöglicht Benutzern das Durchsuchen und Untersuchen von Daten sowie Lese- und Schreibvorgänge von Apache Spark für Azure Synapse Analytics oder SQL in Azure Cosmos DB.

Über den Datenobjekt-Explorer können Sie mithilfe der folgenden Schritte direkt eine Verbindung mit einer Azure Cosmos DB-Datenbank herstellen:

1. Wählen Sie neben „Daten“ das Symbol ***+*** aus.
2. Wählen Sie **Connect to external data** (Verbindung mit externen Daten herstellen) aus.
3. Wählen Sie die API aus, mit der Sie eine Verbindung herstellen möchten: SQL-API oder API für MongoDB
4. Wählen Sie ***Weiter***.
5. Geben Sie einen Namen für den verknüpften Dienst ein. Der Name wird im Objekt-Explorer angezeigt und von Synapse-Laufzeiten verwendet, um eine Verbindung mit der Datenbank und den Containern herzustellen. Es wird empfohlen, einen Anzeigenamen zu verwenden.
6. Wählen Sie den **Azure Cosmos DB-Kontonamen** und den **Datenbanknamen** aus.
7. (Optional) Wird keine Region angegeben, werden Synapse-Laufzeitvorgänge an die nächstgelegene Region weitergeleitet, in der der Analysespeicher aktiviert ist. Sie können jedoch manuell festlegen, in welcher Region Benutzer auf den Azure Cosmos DB-Analysespeicher zugreifen sollen. Wählen Sie **Additional connection properties** (Zusätzliche Verbindungseigenschaften) und dann **Neu** aus. Geben Sie unter **Eigenschaftenname** den Namen ***PreferredRegions*** ein, und legen Sie unter **Wert** die gewünschte Region fest (etwa „WestUS2“, ohne Leerstelle zwischen Wort und Zahl).
8. Klicken Sie auf ***Erstellen***

Azure Cosmos DB-Datenbanken werden im Azure Cosmos DB-Abschnitt auf der Registerkarte **Verknüpft** angezeigt. Mit Azure Cosmos DB können Sie anhand der folgenden Symbole einen HTAP-fähigen Container von einem reinen OLTP-Container unterscheiden:

**Reiner OLTP-Container:**

![OLTP-Container](../media/quickstart-connect-synapse-link-cosmosdb/oltp-container.png)

**HTAP-fähiger Container:**

![HTAP-Container](../media/quickstart-connect-synapse-link-cosmosdb/htap-container.png)

## <a name="quickly-interact-with-code-generated-actions"></a>Schnelles Interagieren mit durch Code generierte Aktionen

Wenn Sie mit der rechten Maustaste in einen Container klicken, wird eine Liste mit Gesten angezeigt, die eine Spark- oder SQL-Laufzeit auslösen. Das Schreiben in einen Container erfolgt über den Transaktionsspeicher von Azure Cosmos DB und verbraucht Anforderungseinheiten.  

## <a name="next-steps"></a>Nächste Schritte

* [Informationen zu den unterstützten Features zwischen Synapse und Azure Cosmos DB](./concept-synapse-link-cosmos-db-support.md)
* [Informationen zum Abfragen des Analysespeichers mit Spark](./how-to-query-analytical-store-spark.md)