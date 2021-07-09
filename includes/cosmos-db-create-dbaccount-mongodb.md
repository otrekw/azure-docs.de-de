---
title: include file
description: include file
services: cosmos-db
ms.custom: include file
ms.openlocfilehash: a8d3e4171a761fd85c9b3f8035c7ea4eeedb5cb3
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110486563"
---
1. Melden Sie sich in einem neuen Browserfenster beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie im Menü auf der linken Seite **Ressource erstellen** aus.
   
   :::image type="content" source="./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-0.png" alt-text="Erstellen einer Ressource im Azure-Portal":::
   
3. Wählen Sie auf der Seite **Neu** die Optionen **Datenbanken** > **Azure Cosmos DB** aus.
   
   :::image type="content" source="./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png" alt-text="Der Bereich „Datenbanken“ im Azure-Portal":::
   
3. Geben Sie auf der Seite **Azure Cosmos DB-Konto erstellen** die Einstellungen für das neue Azure Cosmos DB-Konto ein: 

   |Einstellung|Wert|Beschreibung |
   |---|---|---|
   |Subscription|Abonnementname|Wählen Sie das Azure-Abonnement aus, das Sie für dieses Azure Cosmos-Konto verwenden möchten. |
   |Ressourcengruppe|Ressourcengruppenname|Wählen Sie eine Ressourcengruppe aus, oder wählen Sie **Neu erstellen** aus, und geben Sie einen eindeutigen Namen für die Ressourcengruppe ein. |
   |Kontoname|Geben Sie einen eindeutigen Namen ein.|Geben Sie einen eindeutigen Namen ein, der Ihr Azure Cosmos DB-Konto identifiziert. Der Konto-URI lautet *mongo.cosmos.azure.com* und wird an Ihren eindeutigen Kontonamen angehängt.<br><br>Der Kontoname darf nur Kleinbuchstaben, Ziffern und Bindestriche (-) enthalten und muss zwischen 3 und 31 Zeichen lang sein.|
   API|Azure Cosmos DB-API für Mongo DB|Die API bestimmt den Typ des zu erstellenden Kontos. Azure Cosmos DB stellt fünf APIs bereit: Kern-API (SQL) für Dokumentdatenbanken, Gremlin-API für Graphdatenbanken, API für Mongo DB von Azure Cosmos DB für Dokumentdatenbanken, Azure-Tabellen-API und Cassandra-API. Derzeit müssen Sie ein separates Konto für jede API erstellen. <br><br>Wählen Sie **Azure Cosmos DB-API für Mongo DB** aus, da Sie in dieser Schnellstartanleitung eine Sammlung erstellen, die mit MongoDB verwendet werden kann.<br><br>[Weitere Informationen zur Azure Cosmos DB-API für MongoDB](../articles/cosmos-db/mongodb-introduction.md)|
   |Position|Die Region, die Ihren Benutzern am nächsten liegt|Wählen Sie einen geografischen Standort aus, an dem Ihr Azure Cosmos DB-Konto gehostet werden soll. Verwenden Sie den Standort, der Ihren Benutzern am nächsten ist, damit sie möglichst schnell auf die Daten zugreifen können.|
   |Kapazitätsmodus|Bereitgestellter Durchsatz oder serverlos|Wählen Sie **Bereitgestellter Durchsatz** aus, um ein Konto im Modus [Bereitgestellter Durchsatz](../articles/cosmos-db/set-throughput.md) zu erstellen. Wählen Sie **Serverlos** aus, um ein Konto im Modus [Serverlos](../articles/cosmos-db/serverless.md) zu erstellen. .<br><br>**Hinweis**: Nur Version 3.6 der MongoDB-API wird von serverlosen Konten unterstützt. Wenn Sie 3.2 als Version auswählen, wird das Konto im Modus für bereitgestellten Durchsatz erzwungen.|
   |Anwenden des Rabatts für den Free-Tarif von Azure Cosmos DB|**Anwenden** oder **Nicht anwenden**|Mit dem Azure Cosmos DB-Tarif „Free“ erhalten Sie die ersten 1000 RUs/Sek. sowie 25 GB Speicher kostenlos in einem Konto. Weitere Informationen zum [Tarif „Free“](https://azure.microsoft.com/pricing/details/cosmos-db/)|
   | Version | Auswählen der erforderlichen Serverversion | Die Azure Cosmos DB-API für MongoDB ist mit der Serverversion 4.0, 3.6 und 3.2 kompatibel. Sie können ein Konto [upgraden oder herabstufen](../articles/cosmos-db/mongodb-version-upgrade.md), nachdem es erstellt wurde. |

   > [!NOTE]
   > Sie können pro Azure-Abonnement maximal ein Azure Cosmos DB-Konto im Free-Tarif einrichten und müssen sich beim Erstellen des Kontos anmelden. Wird die Option zum Anwenden des tarifspezifischen Rabatts für den Free-Tarif nicht angezeigt, bedeutet dies, dass bereits ein anderes Konto im Abonnement mit dem Free-Tarif aktiviert wurde.

   :::image type="content" source="./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-create-new-account.png" alt-text="Die Seite „Neues Konto“ für Azure Cosmos DB"::: 

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

4. Die Kontoerstellung dauert einige Minuten. Warten Sie, bis das Portal die Seite **Herzlichen Glückwunsch! Ihr Azure Cosmos DB für Mongo DB-API-Konto ist jetzt bereit.** anzeigt.

   :::image type="content" source="./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-account-created.png" alt-text="Der Bereich „Benachrichtigungen“ im Azure-Portal"::: 