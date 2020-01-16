---
title: include file
description: include file
services: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.topic: include
ms.date: 04/08/2019
ms.custom: include file
ms.openlocfilehash: 8f8b8384b0c570e2b4925b0e84480b19a632187e
ms.sourcegitcommit: 02160a2c64a5b8cb2fb661a087db5c2b4815ec04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2020
ms.locfileid: "75752411"
---
1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/), um ein Azure Cosmos DB-Konto zu erstellen. Suchen Sie nach **Azure Cosmos DB**, und wählen Sie die Option aus.

   ![Die Bereich „Datenbanken“ im Azure-Portal](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

1. Wählen Sie **Hinzufügen**.
1. Geben Sie auf der Seite **Azure Cosmos DB-Konto erstellen** die grundlegenden Einstellungen für das neue Azure Cosmos-Konto ein. 

    |Einstellung|value|Beschreibung |
    |---|---|---|
    |Subscription|Abonnementname|Wählen Sie das Azure-Abonnement aus, das Sie für dieses Azure Cosmos-Konto verwenden möchten. |
    |Ressourcengruppe|Ressourcengruppenname|Wählen Sie eine Ressourcengruppe aus, oder wählen Sie **Neu erstellen** aus, und geben Sie einen eindeutigen Namen für die Ressourcengruppe ein. |
    |Kontoname|Ein eindeutiger Name|Geben Sie einen Namen ein, der Ihr Azure Cosmos-Konto identifiziert. Da *documents.azure.com* an den Namen angefügt wird, die Sie für die URI-Erstellung angeben, muss der Name eindeutig sein.<br><br>Der Name darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten. Sie muss zwischen drei und 31 Zeichen lang sein.|
    |API|Der Typ des zu erstellenden Kontos|Wählen Sie **Core (SQL)** aus, um eine Dokumentdatenbank und eine Abfrage mit SQL-Syntax zu erstellen. <br><br>Die API bestimmt den Typ des zu erstellenden Kontos. Azure Cosmos DB stellt fünf APIs bereit: Core (SQL) und MongoDB für Dokumentdaten, Gremlin für Diagrammdaten sowie Azure Table und Cassandra. Derzeit müssen Sie ein separates Konto für jede API erstellen. <br><br>[Weitere Informationen zur SQL-API](../articles/cosmos-db/documentdb-introduction.md)|
    |Location|Die Region, die Ihren Benutzern am nächsten liegt|Wählen Sie einen geografischen Standort aus, an dem Ihr Azure Cosmos DB-Konto gehostet werden soll. Verwenden Sie den Standort, der Ihren Benutzern am nächsten ist, damit sie möglichst schnell auf die Daten zugreifen können.|

   ![Die Seite „Neues Konto“ für Azure Cosmos DB](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-account.png)

1. Klicken Sie auf **Überprüfen + erstellen**. Sie können die Abschnitte **Netzwerk** und **Tags** überspringen.

1. Überprüfen Sie die Kontoeinstellungen, und wählen Sie anschließend **Erstellen** aus. Die Erstellung des Kontos dauert einige Minuten. Warten Sie, bis auf der Portalseite **Ihre Bereitstellung wurde abgeschlossen.** angezeigt wird. 

    ![Der Bereich „Benachrichtigungen“ im Azure-Portal](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

1. Wählen Sie **Zu Ressource wechseln** aus, um zur Seite des Azure Cosmos DB-Kontos zu wechseln. 

    ![Seite des Azure Cosmos DB-Kontos](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-2.png)
