---
title: Konfigurieren der Firewall der Azure Blockchain Workbench-Datenbank
description: In diesem Artikel erfahren Sie, wie Sie die Firewall der Datenbank der Azure Blockchain Workbench (Vorschauversion) so konfigurieren, dass externe Clients und Anwendungen eine Verbindung herstellen können.
ms.date: 09/09/2019
ms.topic: article
ms.reviewer: mmercuri
ms.openlocfilehash: e6739b7ead9299f020465d3c50ed01826334ca76
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74326003"
---
# <a name="configure-the-azure-blockchain-workbench-database-firewall"></a>Konfigurieren der Firewall der Datenbank von Azure Blockchain Workbench

In diesem wird Artikel gezeigt, wie eine Firewallregel mit dem Azure-Portal konfiguriert wird. Firewallregeln ermöglichen, dass externe Clients oder Clientanwendungen eine Verbindung mit Ihrer Azure Blockchain Workbench-Datenbank herstellen können.

## <a name="connect-to-the-blockchain-workbench-database"></a>Herstellen einer Verbindung mit der Blockchain Workbench-Datenbank

So stellen Sie die Verbindung mit der Datenbank her, für die Sie eine Regel konfigurieren möchten:

1. Melden Sie sich beim Azure-Portal mit einem Konto an, das über die **Besitzer**-Berechtigungen für die Azure Blockchain Workbench-Ressourcen verfügt.
2. Wählen Sie im linken Navigationsbereich die Option **Ressourcengruppen** aus.
3. Wählen Sie den Namen der Ressourcengruppe für die Blockchain Workbench-Bereitstellung aus.
4. Wählen Sie **Typ** aus, um die Liste der Ressourcen zu sortieren, und wählen Sie dann Ihren **SQL-Server** aus.
5. Das Beispiel für die Ressourcenliste im folgenden Screenshot zeigt zwei Datenbanken: *master* und *lsgn-sdk*. Sie konfigurieren die Firewallregel für *lsgn-sdk*.

![Auflisten der Blockchain Workbench-Ressourcen](./media/database-firewall/list-database-resources.png)

## <a name="create-a-database-firewall-rule"></a>Erstellen einer Datenbankfirewallregel

So erstellen Sie eine Firewallregel:

1. Wählen Sie den Link zur Datenbank „lsgn-sdk“ aus.
2. Wählen Sie auf der Menüleiste **Serverfirewall festlegen** aus.

   ![Festlegen der Serverfirewall](./media/database-firewall/configure-server-firewall.png)

3. So erstellen Sie eine Regel für Ihre Organisation:

   * Geben Sie einen **REGELNAMEN** ein.
   * Geben Sie eine IP-Adresse für die **START-IP** des Adressbereichs ein.
   * Geben Sie eine IP-Adresse für die **END-IP** des Adressbereichs ein.

   ![Erstellen einer Firewallregel](./media/database-firewall/create-firewall-rule.png)

    > [!NOTE]
    > Wenn Sie nur die IP-Adresse Ihres Computers hinzufügen möchten, wählen Sie **+ Client-IP hinzufügen** aus.
        
1. Wählen Sie zum Speichern der Firewallkonfiguration **Speichern** aus.
2. Testen Sie den IP-Adressbereich, den Sie für die Datenbank konfiguriert haben, durch Herstellen einer Verbindung von einer Anwendung oder einem Tool aus. Beispielsweise aus SQL Server Management Studio.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Datenbanksichten in Azure Blockchain Workbench](database-views.md)