---
title: include file
description: include file
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 05/19/2021
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: baf0a33f136e54bdc0d31706892fa19afb30df03
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110486664"
---
1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Ressource erstellen** aus.

1. Suchen Sie auf der Seite **Neu** nach **Azure Cosmos DB**, und wählen Sie die Option aus.

1. Wählen Sie auf der Seite **Azure Cosmos DB** die Option **Erstellen** aus.

1. Geben Sie auf der Seite **Azure Cosmos DB-Konto erstellen** die grundlegenden Einstellungen für das neue Azure Cosmos-Konto ein.

   |Einstellung|Wert|Beschreibung |
   |---|---|---|
   | Subscription|Ihr Abonnement|Wählen Sie das Azure-Abonnement aus, das Sie für dieses Azure Cosmos DB-Konto verwenden möchten. |
   | Ressourcengruppe|Neu erstellen<br><br>Geben Sie dann den gleichen Namen als Kontonamen ein.|Wählen Sie **Neu erstellen**. Geben Sie dann einen neuen Ressourcengruppenname für Ihr Konto ein. Verwenden Sie der Einfachheit halber den gleichen Namen als Azure Cosmos-Kontonamen. |
   | Kontoname|Geben Sie einen eindeutigen Namen ein.|Geben Sie einen eindeutigen Namen ein, der Ihr Azure Cosmos DB-Konto identifiziert. Der Konto-URI lautet *cassandra.cosmos.azure.com* und wird an Ihren eindeutigen Kontonamen angehängt.<br><br>Der Kontoname darf nur Kleinbuchstaben, Ziffern und Bindestriche (-) enthalten und muss zwischen 3 und 31 Zeichen lang sein.|
   | API|Cassandra|Die API bestimmt den Typ des zu erstellenden Kontos. Azure Cosmos DB stellt fünf APIs bereit: Kern-API (SQL) für Dokumentdatenbanken, Gremlin-API für Graphdatenbanken, MongoDB-API für Dokumentdatenbanken, Azure-Tabellen-API und Cassandra-API. Sie müssen ein separates Konto für jede API erstellen. <br><br>Wählen Sie **Cassandra** aus, da Sie in diesem Schnellstart eine Tabelle erstellen, die mit der Cassandra-API verwendet werden kann. <br><br>[Weitere Informationen zur Cassandra-API](../articles/cosmos-db/cassandra-introduction.md)|
   |Position|Die Region, die Ihren Benutzern am nächsten liegt|Wählen Sie einen geografischen Standort aus, an dem Ihr Azure Cosmos DB-Konto gehostet werden soll. Verwenden Sie den Standort, der Ihren Benutzern am nächsten ist, damit sie möglichst schnell auf die Daten zugreifen können.|
   |Kapazitätsmodus|Bereitgestellter Durchsatz oder serverlos|Wählen Sie **Bereitgestellter Durchsatz** aus, um ein Konto im Modus [Bereitgestellter Durchsatz](../articles/cosmos-db/set-throughput.md) zu erstellen. Wählen Sie **Serverlos** aus, um ein Konto im Modus [Serverlos](../articles/cosmos-db/serverless.md) zu erstellen.|
   |Anwenden des Rabatts für den Free-Tarif von Azure Cosmos DB|**Anwenden** oder **Nicht anwenden**|Mit dem Azure Cosmos DB-Tarif „Free“ erhalten Sie die ersten 1000 RUs/Sek. sowie 25 GB Speicher kostenlos in einem Konto. Weitere Informationen zum [Tarif „Free“](https://azure.microsoft.com/pricing/details/cosmos-db/)|

   > [!NOTE]
   > Sie können pro Azure-Abonnement maximal ein Azure Cosmos DB-Konto im Free-Tarif einrichten und müssen sich beim Erstellen des Kontos anmelden. Wird die Option zum Anwenden des tarifspezifischen Rabatts für den Free-Tarif nicht angezeigt, bedeutet dies, dass bereits ein anderes Konto im Abonnement mit dem Free-Tarif aktiviert wurde.

   :::image type="content" source="./media/cosmos-db-create-dbaccount-cassandra/azure-cosmos-db-create-new-account.png" alt-text="Die Seite zum Erstellen eines neuen Kontos in Azure Cosmos DB für die Cassandra-API":::

1. Konfigurieren Sie auf der Registerkarte **Globale Verteilung** die folgenden Details. Für diese Schnellstartanleitung können Sie die Standardwerte übernehmen:

   |Einstellung|Wert|Beschreibung |
   |---|---|---|
   |Georedundanz|Deaktivieren|Aktivieren oder deaktivieren Sie die globale Verteilung für Ihr Konto, indem Sie Ihre Region mit einer Region koppeln. Sie können später weitere Regionen zu Ihrem Konto hinzufügen.|
   |Schreibvorgänge in mehreren Regionen|Deaktivieren|Mit der Funktion zum Schreiben in mehreren Regionen können Sie den bereitgestellten Durchsatz für Ihre Datenbanken und Container in der ganzen Welt nutzen.|

   > [!NOTE]
   > Die folgenden Optionen sind nicht verfügbar, wenn Sie als **Kapazitätsmodus** die Option **Serverlos** auswählen:
   > - Tarifspezifischen Rabatt für den Free-Tarif anwenden
   > - Georedundanz
   > - Schreibvorgänge in mehreren Regionen

1. Optional können Sie auf den folgenden Registerkarten zusätzliche Details konfigurieren:

   * **Netzwerk**: Konfigurieren Sie den [Zugriff über ein virtuelles Netzwerk](../articles/cosmos-db/how-to-configure-vnet-service-endpoint.md).
   * **Sicherungsrichtlinie**: Konfigurieren Sie eine Richtlinie für [regelmäßige](../articles/cosmos-db/configure-periodic-backup-restore.md) oder [fortlaufende](../articles/cosmos-db/continuous-backup-restore-portal.md) Sicherungen.
   * **Verschlüsselung**: Verwenden Sie entweder einen vom Dienst verwalteten Schlüssel oder einen [kundenseitig verwalteten Schlüssel](../articles/cosmos-db/how-to-setup-cmk.md#create-a-new-azure-cosmos-account).
   * **Tags**: Tags sind Name/Wert-Paare, mit denen Sie Ressourcen kategorisieren und eine konsolidierte Abrechnung anzeigen können, indem Sie dasselbe Tag auf mehrere Ressourcen und Ressourcengruppen anwenden.

1. Klicken Sie auf **Überprüfen + erstellen**.

1. Überprüfen Sie die Kontoeinstellungen, und wählen Sie anschließend **Erstellen** aus. Die Erstellung des Kontos dauert einige Minuten. Warten Sie, bis auf der Portalseite **Ihre Bereitstellung wurde abgeschlossen.** angezeigt wird.

   :::image type="content" source="./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png" alt-text="Der Bereich „Benachrichtigungen“ im Azure-Portal":::

1. Wählen Sie **Zu Ressource wechseln** aus, um zur Seite des Azure Cosmos DB-Kontos zu wechseln. 