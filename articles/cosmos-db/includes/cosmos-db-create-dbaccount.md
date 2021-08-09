---
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.topic: include
ms.date: 07/02/2021
ms.custom: include file
ms.openlocfilehash: a295c5bda4c6d5822d762f23372d3b2b5d206a3a
ms.sourcegitcommit: f4e04fe2dfc869b2553f557709afaf057dcccb0b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2021
ms.locfileid: "113280090"
---
1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Ressource erstellen** aus.

1. Suchen Sie auf der Seite **Neu** nach **Azure Cosmos DB**, und wählen Sie die Option aus.

1. Wählen Sie auf der Seite **Azure Cosmos DB** die Option **Erstellen** aus.

1. Geben Sie auf der Seite **Azure Cosmos DB-Konto erstellen** die grundlegenden Einstellungen für das neue Azure Cosmos-Konto ein.

   |Einstellung|Wert|Beschreibung |
   |---|---|---|
   |Subscription|Abonnementname|Wählen Sie das Azure-Abonnement aus, das Sie für dieses Azure Cosmos-Konto verwenden möchten. |
   |Ressourcengruppe|Ressourcengruppenname|Wählen Sie eine Ressourcengruppe aus, oder wählen Sie **Neu erstellen** aus, und geben Sie einen eindeutigen Namen für die Ressourcengruppe ein. |
   |Kontoname|Ein eindeutiger Name|Geben Sie einen Namen ein, der Ihr Azure Cosmos-Konto identifiziert. Da *documents.azure.com* an den Namen angefügt wird, die Sie für die URI-Erstellung angeben, muss der Name eindeutig sein.<br><br>Der Name darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten. Er muss 3 bis 44 Zeichen umfassen.|
   |API|Der Typ des zu erstellenden Kontos|Wählen Sie **Core (SQL)** aus, um eine Dokumentdatenbank und eine Abfrage mit SQL-Syntax zu erstellen. <br><br>Die API bestimmt den Typ des zu erstellenden Kontos. Azure Cosmos DB stellt fünf APIs bereit: Core (SQL) und MongoDB für Dokumentdaten, Gremlin für Diagrammdaten sowie Azure Table und Cassandra. Derzeit müssen Sie ein separates Konto für jede API erstellen. <br><br>[Weitere Informationen zur SQL-API](../introduction.md)|
   |Standort|Die Region, die Ihren Benutzern am nächsten liegt|Wählen Sie einen geografischen Standort aus, an dem Ihr Azure Cosmos DB-Konto gehostet werden soll. Verwenden Sie den Standort, der Ihren Benutzern am nächsten ist, damit sie möglichst schnell auf die Daten zugreifen können.|
   |Kapazitätsmodus|Bereitgestellter Durchsatz oder serverlos|Wählen Sie **Bereitgestellter Durchsatz** aus, um ein Konto im Modus [Bereitgestellter Durchsatz](../set-throughput.md) zu erstellen. Wählen Sie **Serverlos** aus, um ein Konto im Modus [Serverlos](../serverless.md) zu erstellen.|
   |Anwenden des Rabatts für den Free-Tarif von Azure Cosmos DB|**Anwenden** oder **Nicht anwenden**|Mit dem Azure Cosmos DB-Tarif „Free“ erhalten Sie die ersten 1000 RUs/Sek. sowie 25 GB Speicher kostenlos in einem Konto. Weitere Informationen zum [Tarif „Free“](https://azure.microsoft.com/pricing/details/cosmos-db/)|

   > [!NOTE]
   > Sie können pro Azure-Abonnement maximal ein Azure Cosmos DB-Konto im Free-Tarif einrichten und müssen sich beim Erstellen des Kontos anmelden. Wird die Option zum Anwenden des tarifspezifischen Rabatts für den Free-Tarif nicht angezeigt, bedeutet dies, dass bereits ein anderes Konto im Abonnement mit dem Free-Tarif aktiviert wurde.

   :::image type="content" source="./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account-detail.png" alt-text="Die Seite „Neues Konto“ für Azure Cosmos DB":::

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

   * **Netzwerk**: Konfigurieren Sie den [Zugriff über ein virtuelles Netzwerk](../how-to-configure-vnet-service-endpoint.md).
   * **Sicherungsrichtlinie**: Konfigurieren Sie eine Richtlinie für [regelmäßige](../configure-periodic-backup-restore.md) oder [fortlaufende](../continuous-backup-restore-portal.md) Sicherungen.
   * **Verschlüsselung**: Verwenden Sie entweder einen vom Dienst verwalteten Schlüssel oder einen [kundenseitig verwalteten Schlüssel](../how-to-setup-cmk.md#create-a-new-azure-cosmos-account).
   * **Tags**: Tags sind Name-Wert-Paare, mit denen Sie Ressourcen kategorisieren und eine konsolidierte Abrechnung anzeigen können, indem Sie dasselbe Tag auf mehrere Ressourcen und Ressourcengruppen anwenden.

1. Klicken Sie auf **Überprüfen + erstellen**.

1. Überprüfen Sie die Kontoeinstellungen, und wählen Sie anschließend **Erstellen** aus. Die Erstellung des Kontos dauert einige Minuten. Warten Sie, bis auf der Portalseite **Ihre Bereitstellung wurde abgeschlossen.** angezeigt wird.

   :::image type="content" source="./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png" alt-text="Der Bereich „Benachrichtigungen“ im Azure-Portal":::

1. Wählen Sie **Zu Ressource wechseln** aus, um zur Seite des Azure Cosmos DB-Kontos zu wechseln. 

   :::image type="content" source="./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-2.png" alt-text="Seite des Azure Cosmos DB-Kontos":::