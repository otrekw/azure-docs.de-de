---
title: Herstellen einer Verbindung mit einer IBM Informix-Datenbank
description: Automatisieren von Aufgaben und Workflows, die in IBM Informix gespeicherte Ressourcen mithilfe von Azure Logic Apps verwalten
services: logic-apps
ms.suite: integration
author: gplarsen
ms.author: daberry
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 01/07/2020
tags: connectors
ms.openlocfilehash: 4995a91783c2302f3bda5cc9409f017248ca29fa
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88761643"
---
# <a name="manage-ibm-informix-database-resources-by-using-azure-logic-apps"></a>Verwalten von IBM Informix-Datenbankressourcen mithilfe von Azure Logic Apps

Mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) und dem [Informix-Connector](/connectors/informix/) können Sie automatisierte Aufgaben und Workflows erstellen, die Ressourcen in einer IBM Informix-Datenbank verwalten. Dieser Connector umfasst einen Microsoft-Client, der mit Informix-Remoteservercomputern über ein TCP/IP-Netzwerk kommuniziert, einschließlich cloudbasierter Datenbanken wie IBM Informix für Windows, die in Azure-Virtualisierungs- und lokalen Datenbanken ausgeführt werden, wenn Sie das [lokale Datengateway](../logic-apps/logic-apps-gateway-connection.md) verwenden. Sie können eine Verbindung mit diesen Informix-Plattformen und -Versionen herstellen, wenn sie für die Unterstützung von DRDA-Clientverbindungen (Distributed Relational Database Architecture) konfiguriert sind.

* IBM Informix 12.1
* IBM Informix 11.7

In diesem Thema wird veranschaulicht, wie Sie den Connector in einer Logik-App zum Verarbeiten von Datenbankvorgängen verwenden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Für lokale Datenbanken [laden Sie das lokale Datengateway herunter, und installieren Sie es](../logic-apps/logic-apps-gateway-install.md) auf einem lokalen Computer, und [erstellen Sie dann eine Azure-Datengatewayressource im Azure-Portal](../logic-apps/logic-apps-gateway-connection.md).

* Die Logik-App, mit der Sie Zugriff auf Ihre Informix-Datenbank benötigen. Dieser Connector stellt nur Aktionen bereit, daher muss Ihre Logik-App bereits mit einem Trigger gestartet werden, z. B. dem [Wiederholungstrigger](../connectors/connectors-native-recurrence.md). 

## <a name="add-an-informix-action"></a>Informix-Aktion hinzufügen

1. Öffnen Sie Ihre Logik-App über das [Azure-Portal](https://portal.azure.com) im Logik-App-Designer, wenn sie nicht bereits geöffnet ist.

1. Wählen Sie im Schritt zum Hinzufügen der Informix-Aktion die Option **Neuer Schritt** aus.

   Um eine Aktion zwischen vorhandenen Schritten hinzuzufügen, bewegen Sie den Mauszeiger über den Verbindungspfeil. Wählen Sie das angezeigte Pluszeichen ( **+** ) aus, und wählen Sie dann **Aktion hinzufügen** aus.

1. Geben Sie im Suchfeld den Begriff `informix` als Filter ein. Wählen Sie in der Liste mit den Aktionen die gewünschte Aktion aus, beispielsweise:

   ![Die auszuführende Informix-Aktion auswählen](./media/connectors-create-api-informix/select-informix-connector-action.png)

   Der Connector stellt diese Aktionen bereit, die die entsprechenden Datenbankvorgänge ausführen:

   * Tabellen abrufen: Listet Datenbanktabellen mithilfe einer `CALL`-Anweisung auf.
   * Zeilen abrufen: Liest alle Zeilen mithilfe einer `SELECT *`-Anweisung.
   * Zeile abrufen: Liest eine Zeile mithilfe einer `SELECT WHERE`-Anweisung.
   * Eine Zeile hinzufügen können Sie mit einer `INSERT`-Anweisung.
   * Eine Zeile bearbeiten können Sie mit einer `UPDATE`-Anweisung.
   * Eine Zeile löschen können Sie mit einer `DELETE`-Anweisung.

1. Wenn Sie aufgefordert werden, Verbindungsdetails für Ihre Informix-Datenbank anzugeben, führen Sie die [Schritte aus, um die Verbindung zu erstellen](#create-connection), und fahren Sie dann mit dem nächsten Schritt fort.

1. Geben Sie die Informationen für Ihrem ausgewählte Aktion ein:

   | Aktion | BESCHREIBUNG | Eigenschaften und Beschreibungen |
   |--------|-------------|-----------------------------|
   | **Tabellen abrufen** | Auflisten von Datenbanktabellen durch Ausführen einer Informix-CALL-Anweisung. | Keine |
   | **Zeilen abrufen** | Abrufen aller Zeilen aus der angegebenen Tabelle durch Ausführen einer Informix-`SELECT *`-Anweisung. | **Tabellenname**: Der Name der gewünschten Informix-Tabelle. <p><p>Um dieser Aktion weitere Eigenschaften hinzuzufügen, wählen Sie diese aus der Liste **Neuen Parameter hinzufügen** aus. Weitere Informationen finden Sie im [Referenzthema des Connectors](/connectors/informix/). |
   | **Zeile abrufen** | Abrufen einer Zeile aus der angegebenen Tabelle durch Ausführen einer Informix-`SELECT WHERE`-Anweisung. | - **Tabellenname**: Der Name der gewünschten Informix-Tabelle. <br>- **Zeilen-ID**: Die eindeutige ID für die Zeile, z. B. `9999`. |
   | **Zeile einfügen** | Hinzufügen einer Zeile in der angegebenen Informix-Tabelle durch Ausführen einer Informix-`INSERT`-Anweisung. | - **Tabellenname**: Der Name der gewünschten Informix-Tabelle. <br>- **Element**: Die Zeile mit den hinzuzufügenden Werten. |
   | **Zeile aktualisieren** | Ändern einer Zeile in der angegebenen Informix-Tabelle durch Ausführen einer Informix-`UPDATE`-Anweisung. | - **Tabellenname**: Der Name der gewünschten Informix-Tabelle. <br>- **Zeilen-ID**: Die eindeutige ID für die zu aktualisierende Zeile, z. B. `9999`. <br>- **Zeile**: Die Zeile mit den aktualisierten Werten, z. B. `102`. |
   | **Zeile löschen** | Entfernen einer Zeile aus der angegebenen Informix-Tabelle durch Ausführen einer Informix-`DELETE`-Anweisung. | - **Tabellenname**: Der Name der gewünschten Informix-Tabelle. <br>- **Zeilen-ID**: Die eindeutige ID für die zu löschende Zeile, z. B. `9999`. |
   ||||

1. Speichern Sie Ihre Logik-App. [Entweder testen Sie nun Ihre Logik-App](#test-logic-app), oder Sie fahren mit dem Erstellen Ihrer Logik-App fort.

<a name="create-connection"></a>

## <a name="connect-to-informix"></a>Mit Informix verbinden

1. Wie Ihre Logik-App eine Verbindung mit einer lokalen Datenbank herstellt, wählen Sie **Über lokales Datengateway verbinden** aus.

1. Geben Sie diese Verbindungsinformationen an, und wählen Sie dann **Erstellen** aus.

   | Eigenschaft | JSON-Eigenschaft | Erforderlich | Beispielwert | BESCHREIBUNG |
   |----------|---------------|----------|---------------|-------------|
   | Verbindungsname | `name` | Ja | `informix-demo-connection` | Der für die Verbindung mit Ihrer Informix-Datenbank zu verwendende Name. |
   | Server | `server` | Ja | - Cloud: `informixdemo.cloudapp.net:9089` <br>- Lokal: `informixdemo:9089` | Die TCP/IP-Adresse oder der Alias, im IPv4- oder IPv6-Format, gefolgt von einem Doppelpunkt und einer TCP/IP-Portnummer. |
   | Datenbank | `database` | Ja | `nwind` | Der Name der relationalen DRDA-Datenbank (RDBNAM) oder der Name der Informix-Datenbank (dbname). Informix akzeptiert eine 128-Byte-Zeichenfolge. |
   | Authentifizierung | `authentication` | Nur lokal | **Standard** oder **Windows** (Kerberos). | Der Authentifizierungstyp, der für Ihre Informix-Datenbank erforderlich ist. Diese Eigenschaft wird nur angezeigt, wenn Sie **Über lokales Datengateway verbinden** auswählen. |
   | Username | `username` | Nein | <*database-user-name*> | Ein Benutzername für die Datenbank. |
   | Kennwort | `password` | Nein | <*database-password*> | Ein Kennwort für die Datenbank. |
   | Gateway | `gateway` | Nur lokal | - <*Azure-subscription*> <br>- <*Azure-on-premises-data-gateway-resource*> | Der Name des Azure-Abonnements und der Azure-Ressource für das lokale Datengateway, das Sie im Azure-Portal erstellt haben. Die Eigenschaft **Gateway** und die Untereigenschaften werden nur angezeigt, wenn Sie **Über lokales Datengateway verbinden** auswählen. |
   ||||||

   Beispiel:

   * **Clouddatenbank**

     ![Verbindungsinformationen der Clouddatenbank.](./media/connectors-create-api-informix/informix-cloud-connection.png)

   * **Lokale Datenbank**

     ![Verbindungsinformationen der lokalen Datenbank.](./media/connectors-create-api-informix/informix-on-premises-connection.png)

1. Speichern Sie Ihre Logik-App.

<a name="test-logic-app"></a>

## <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

1. Wählen Sie auf der Symbolleiste des Logik-App-Designers **Ausführen** aus. Nachdem Ihre Logik-App ausgeführt wurde, können Sie die Ausgaben dieser Ausführung anzeigen.

1. Wählen Sie im Menü Ihrer Logik-App die Option **Übersicht** aus. Wählen Sie im Übersichtsbereich unter **Zusammenfassung** > **Ausführungsverlauf** die jüngste Ausführung aus.

1. Wählen Sie unter **Logik-App-Ausführung** die Option **Ausführungsdetails** aus.

1. Wählen Sie in der Liste der Aktionen die Aktion mit den Ausgaben aus, die Sie anzeigen möchten, z. B. **Get_tables**.

   Wenn die Aktion erfolgreich war, wird deren Eigenschaft **Status** als **Erfolgreich** gekennzeichnet.

1. Um die Eingaben anzuzeigen, wählen Sie unter **Eingabelink** den URL-Link aus. Um die Ausgaben anzuzeigen, wählen Sie unter **Ausgabelink** den URL-Link aus. Hier sehen Sie ein paar Beispielausgaben:

   * **Get_tables** zeigt eine Liste von Tabellen an:

     ![Ausgaben der Aktion „Tabellen abrufen“.](./media/connectors-create-api-informix/InformixconnectorGetTablesLogicAppRunOutputs.png)

   * **Get_rows** zeigt eine Liste von Zeilen an:

     ![Ausgaben der Aktion „Zeilen abrufen“.](./media/connectors-create-api-informix/InformixconnectorGetRowsOutputs.png)

   * **Get_row** zeigt die angegebene Zeile an:

     ![Ausgaben der Aktion „Zeile abrufen“.](./media/connectors-create-api-informix/InformixconnectorGetRowOutputs.png)

   * **Insert_row** zeigt die neue Zeile an:

     ![Ausgaben der Aktion „Zeile einfügen“.](./media/connectors-create-api-informix/InformixconnectorInsertRowOutputs.png)

   * **Update_row** zeigt die aktualisierte Zeile an:

     ![Ausgaben der Aktion „Zeile aktualisieren“.](./media/connectors-create-api-informix/InformixconnectorUpdateRowOutputs.png)

   * **Delete_row** zeigt die gelöschte Zeile an:

     ![Ausgaben der Aktion „Zeile löschen“.](./media/connectors-create-api-informix/InformixconnectorDeleteRowOutputs.png)

## <a name="connector-specific-details"></a>Connectorspezifische Details

Technische Details zu Triggern, Aktionen und Beschränkungen aus der Swagger-Beschreibung des Connectors finden Sie auf der [Referenzseite des Connectors](/connectors/informix/).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](apis-list.md)
