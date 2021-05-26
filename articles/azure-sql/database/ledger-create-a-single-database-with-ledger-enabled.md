---
title: Erstellen einer Einzeldatenbank mit aktiviertem Ledger
description: Erstellen Sie eine Einzeldatenbank in Azure SQL-Datenbank mit aktiviertem Ledger über das Azure-Portal.
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: quickstart
author: JasonMAnderson
ms.author: janders
ms.reviewer: vanto
ms.date: 05/25/2021
ms.openlocfilehash: d03d104a5b0d9ad9d83cbecdeeab8614cf0ee728
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386365"
---
# <a name="quickstart-create-an-azure-sql-database-with-ledger-enabled"></a>Schnellstart: Erstellen einer Azure SQL-Datenbank mit aktiviertem Ledger

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Der Azure SQL-Datenbank-Ledger ist zurzeit als **öffentliche Vorschau** verfügbar.

In dieser Schnellstartanleitung erstellen Sie eine [Ledgerdatenbank](ledger-overview.md#ledger-database) in Azure SQL-Datenbank und konfigurieren den [automatischen Digestspeicher mit Azure Blob Storage](ledger-digest-management-and-database-verification.md#automatic-generation-and-storage-of-database-digests) mithilfe des Azure-Portals. Weitere Informationen zum Ledger finden Sie unter [Azure SQL-Datenbank-Ledger.](ledger-overview.md)

## <a name="prerequisite"></a>Voraussetzung

- Ein aktives Azure-Abonnement. Falls Sie nicht über ein Abonnement verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).

## <a name="create-a-ledger-database-and-configure-digest-storage"></a>Erstellen einer Ledgerdatenbank und Konfigurieren des Digestspeichers

Erstellen Sie eine einzelne Ledgerdatenbank auf der [serverlosen Computeebene](serverless-tier-overview.md), und konfigurieren Sie den Upload der Ledgerdigestspeicher in ein Azure Storage-Konto.

### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Diese Schnellstarteinleitung beginnt für die Erstellung eines Singletons im Azure-Portal auf der Azure SQL-Seite.

1. Navigieren Sie zur Seite [SQL-Bereitstellungsoption auswählen](https://portal.azure.com/#create/Microsoft.AzureSQL).

1. Behalten Sie unter **SQL-Datenbanken** für **Einzeldatenbank** den festgelegten Wert **Ressourcentyp** bei, und wählen Sie **Erstellen** aus.

   ![Hinzufügen zu Azure SQL](./media/single-database-create-quickstart/select-deployment.png)

1. Wählen Sie auf der Registerkarte **Grundeinstellungen** des Formulars **SQL-Datenbank erstellen** unter **Projektdetails** das gewünschte **Abonnement** für Azure aus.

1. Wählen Sie bei **Ressourcengruppe** die Option **Neu erstellen** aus, geben Sie *myResourceGroup* ein, und wählen Sie **OK** aus.

1. Geben Sie für **Datenbankname** *demo* ein.

1. Wählen Sie unter **Server** die Option **Neu erstellen** aus, und füllen Sie das Formular **Neuer Server** mit den folgenden Werten aus:
   - **Servername**: Geben Sie *mysqlserver* ein, und fügen Sie einige weitere Zeichen hinzu, um einen eindeutigen Wert zu erhalten. Wir können keinen exakten Servernamen zur Verwendung angeben, weil Servernamen für alle Server in Azure global eindeutig sein müssen (und nicht nur innerhalb eines Abonnements eindeutig sind). Geben Sie also beispielsweise „mysqlserver12345“ ein. Dann werden Sie vom Portal informiert, ob dieser Server verfügbar ist oder nicht.
   - **Serveradministratoranmeldung**: Geben Sie *azureuser* ein.
   - **Kennwort**: Geben Sie ein geeignetes Kennwort ein, und wiederholen Sie die Eingabe im Feld **Kennwort bestätigen**.
   - **Standort**: Wählen Sie in der Dropdownliste einen Standort aus.
   - Wählen Sie **Allow Azure services to access this server** (Azure-Dienste für den Zugriff auf diesen Server zulassen) aus, um den Zugriff auf den Digestspeicher zuzulassen.
   
   Klicken Sie auf **OK**.
   
1. Behalten Sie für **Möchten Sie einen Pool für elastische SQL-Datenbanken verwenden?** den Wert **Nein** bei.

1. Wählen Sie unter **Compute + Speicher** die Option **Datenbank konfigurieren** aus.

1. Weil in dieser Schnellstartanleitung eine serverlose Datenbank verwendet wird, wählen Sie **Serverlos** und dann **Anwenden** aus. 

      ![Serverlose Datenbank konfigurieren](./media/single-database-create-quickstart/configure-database.png)

1. Wählen Sie auf der Registerkarte **Netzwerk** als **Konnektivitätsmethode** die Option **Öffentlicher Endpunkt** aus.
1. Legen Sie bei **Firewallregeln** die Option **Aktuelle Client-IP-Adresse hinzufügen** auf **Ja** fest. Behalten Sie für **Azure-Diensten und -Ressourcen den Zugriff auf diese Servergruppe gestatten** den Wert **Nein** bei.
1. Wählen Sie **Weiter: Sicherheit** unten auf der Seite aus.

   :::image type="content" source="media/ledger/ledger-create-database-networking-tab.png" alt-text="Registerkarte „Netzwerk“ von „Create Database in Azure portal“ (Datenbank im Azure-Portal erstellen)":::

1. Wählen Sie auf der Registerkarte **Sicherheit** im Abschnitt **Ledger** die Option **Configure ledger** (Ledger konfigurieren) aus.

    :::image type="content" source="media/ledger/ledger-configure-ledger-security-tab.png" alt-text="Konfigurieren des Ledgers auf Registerkarte „Sicherheit“ im Azure-Portal":::

1. Aktivieren Sie im Bereich **Configure ledger** (Ledger konfigurieren) im Abschnitt **Ledger** das Kontrollkästchen **Enable for all future tables in this database** (Für alle zukünftigen Tabellen in dieser Datenbank konfigurieren). Mit dieser Einstellung wird sichergestellt, dass alle zukünftigen Tabellen in der Datenbank Ledgertabellen sind. Dies bedeutet, dass alle Daten in der Datenbank manipulationsgeschützt sind. Standardmäßig werden neue Tabellen als aktualisierbare Ledgertabellen erstellt, auch wenn Sie `LEDGER = ON` in [CREATE TABLE](/sql/t-sql/statements/create-table-transact-sql) nicht angeben. Alternativ können Sie diese Option deaktiviert lassen, wodurch Sie aber beim Erstellen neuer Tabellen mit Transact-SQL die Ledgerfunktionalität auf Tabellenbasis aktivieren müssen.

1. Im Abschnitt **Digest storage** (Digestspeicher) wird **Enable automatic digest storage** (Automatischen Digestspeicher aktivieren) automatisch ausgewählt. Anschließend werden ein neues Azure Storage-Konto sowie -Container erstellt, in denen Ihre Digests gespeichert werden.

1. Klicken Sie auf die Schaltfläche **Übernehmen**.

    :::image type="content" source="media/ledger/ledger-configure-ledger-pane.png" alt-text="Bereich „Ledger konfigurieren“ im Azure-Portal":::

1. Wählen Sie unten auf der Seite die Option **Überprüfen + erstellen** aus:

    :::image type="content" source="media/ledger/ledger-review-security-tab.png" alt-text="Überprüfen und Erstellen einer Ledgerdatenbank auf der Registerkarte „Sicherheit“ im Azure-Portal":::

1. Wählen Sie nach Überprüfung auf der Seite **Überprüfen + erstellen** die Option **Erstellen** aus.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Behalten Sie die Ressourcengruppe, den Server und die Einzeldatenbank für die nächsten Schritte bei, und informieren Sie sich darüber, wie Sie das Ledgerfeature Ihrer Datenbank mit unterschiedlichen Methoden verwenden können.

Wenn Sie die Verwendung dieser Ressourcen beendet haben, können Sie die erstellte Ressourcengruppe löschen. Hierbei werden auch der Server und die darin enthaltene Einzeldatenbank gelöscht.

### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Löschen Sie **myResourceGroup** und alle zugehörigen Ressourcen wie folgt über das Azure-Portal:

1. Suchen Sie im Portal nach **Ressourcengruppen**, und wählen Sie die Option aus. Wählen Sie anschließend in der Liste die Option **myResourceGroup** aus.
1. Wählen Sie auf der Ressourcengruppenseite die Option **Ressourcengruppe löschen** aus.
1. Geben Sie unter **Geben Sie den Ressourcengruppennamen ein** den Namen *myResourceGroup* ein, und wählen Sie anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

Führen Sie für Ihre Datenbank die Verbindungsherstellung und Abfragen mit unterschiedlichen Tools und Sprachen durch:

- [Erstellen und Verwenden von aktualisierbaren Ledgertabellen](ledger-how-to-updatable-ledger-tables.md)
- [Erstellen und Verwenden von Ledgertabellen, die nur Anfügevorgänge unterstützen](ledger-how-to-append-only-ledger-tables.md) 
