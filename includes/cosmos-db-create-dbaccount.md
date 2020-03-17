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
ms.openlocfilehash: bfce10b0c9c55012c3ba2fb25121b28157f203b8
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78672073"
---
1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/), um ein Azure Cosmos DB-Konto zu erstellen. Suchen Sie nach **Azure Cosmos DB**, und wählen Sie die Option aus.

   ![Die Bereich „Datenbanken“ im Azure-Portal](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

1. Wählen Sie **Hinzufügen**.
1. Geben Sie auf der Seite **Azure Cosmos DB-Konto erstellen** die grundlegenden Einstellungen für das neue Azure Cosmos-Konto ein. 

    |Einstellung|value|BESCHREIBUNG |
    |---|---|---|
    |Subscription|Abonnementname|Wählen Sie das Azure-Abonnement aus, das Sie für dieses Azure Cosmos-Konto verwenden möchten. |
    |Ressourcengruppe|Ressourcengruppenname|Wählen Sie eine Ressourcengruppe aus, oder wählen Sie **Neu erstellen** aus, und geben Sie einen eindeutigen Namen für die Ressourcengruppe ein. |
    |Kontoname|Ein eindeutiger Name|Geben Sie einen Namen ein, der Ihr Azure Cosmos-Konto identifiziert. Da *documents.azure.com* an den Namen angefügt wird, die Sie für die URI-Erstellung angeben, muss der Name eindeutig sein.<br><br>Der Name darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten. Sie muss zwischen drei und 31 Zeichen lang sein.|
    |API|Der Typ des zu erstellenden Kontos|Wählen Sie **Core (SQL)** aus, um eine Dokumentdatenbank und eine Abfrage mit SQL-Syntax zu erstellen. <br><br>Die API bestimmt den Typ des zu erstellenden Kontos. Azure Cosmos DB stellt fünf APIs bereit: Core (SQL) und MongoDB für Dokumentdaten, Gremlin für Diagrammdaten sowie Azure Table und Cassandra. Derzeit müssen Sie ein separates Konto für jede API erstellen. <br><br>[Weitere Informationen zur SQL-API](../articles/cosmos-db/documentdb-introduction.md)|
    |Tarifspezifischen Rabatt für den Free-Tarif anwenden|„Anwenden“ oder „Nicht anwenden“|Mit dem Azure Cosmos DB-Tarif „Free“ erhalten Sie die ersten 400 RUs/Sek. sowie 5 GB Speicher kostenlos in einem Konto. Weitere Informationen zum [Tarif „Free“](https://azure.microsoft.com/pricing/details/cosmos-db/)|
    |Location|Die Region, die Ihren Benutzern am nächsten liegt|Wählen Sie einen geografischen Standort aus, an dem Ihr Azure Cosmos DB-Konto gehostet werden soll. Verwenden Sie den Standort, der Ihren Benutzern am nächsten ist, damit sie möglichst schnell auf die Daten zugreifen können.|
    |Kontotyp|„Produktionsbezogen“ oder „Nicht produktionsbezogen“|Wählen Sie **Produktionsbezogen** aus, wenn das Konto für eine Produktionsworkload verwendet wird. Wählen Sie **Nicht produktionsbezogen** aus, wenn das Konto nicht für die Produktion, sondern beispielsweise für Entwicklung, Tests, Qualitätssicherung oder Staging verwendet wird. Hierbei handelt es sich um eine Azure-Ressourcentageinstellung, die das Portal optimiert, aber keine Auswirkungen auf das zugrunde liegende Azure Cosmos DB-Konto hat. Sie können diesen Wert jederzeit ändern.|


> [!NOTE]
> Sie können pro Azure-Abonnement maximal ein Azure Cosmos DB-Konto im Free-Tarif einrichten und müssen sich beim Erstellen des Kontos anmelden. Wird die Option zum Anwenden des tarifspezifischen Rabatts für den Free-Tarif nicht angezeigt, bedeutet dies, dass bereits ein anderes Konto im Abonnement mit dem Free-Tarif aktiviert wurde.
   
   ![Die Seite „Neues Konto“ für Azure Cosmos DB](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account-detail.png)

1. Klicken Sie auf **Überprüfen + erstellen**. Sie können die Abschnitte **Netzwerk** und **Tags** überspringen.

1. Überprüfen Sie die Kontoeinstellungen, und wählen Sie anschließend **Erstellen** aus. Die Erstellung des Kontos dauert einige Minuten. Warten Sie, bis auf der Portalseite **Ihre Bereitstellung wurde abgeschlossen.** angezeigt wird. 

    ![Der Bereich „Benachrichtigungen“ im Azure-Portal](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

1. Wählen Sie **Zu Ressource wechseln** aus, um zur Seite des Azure Cosmos DB-Kontos zu wechseln. 

    ![Seite des Azure Cosmos DB-Kontos](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-2.png)
