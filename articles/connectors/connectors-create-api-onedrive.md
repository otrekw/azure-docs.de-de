---
title: Zugreifen auf Dateien in Microsoft OneDrive und Verwalten von diesen
description: Hochladen und Verwalten von Dateien in OneDrive durch Erstellen automatisierter Workflows in Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 10/18/2016
tags: connectors
ms.openlocfilehash: 8dd54fec963b8f4775a8ade6277b071d62ca3850
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86524204"
---
# <a name="access-and-manage-files-in-onedrive-connector-by-using-azure-logic-apps"></a>Zugreifen auf Dateien im OneDrive-Connector und Verwalten von diesen mithilfe von Azure Logic Apps

Mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) und dem [OneDrive-Connector](/connectors/onedriveconnector/) können Sie automatisierte Tasks und Workflows erstellen, um Ihre Dateien zu verwalten, einschließlich Hochladen, Abrufen, Löschen von Dateien usw. Mit OneDrive können Sie folgende Aufgaben ausführen:

* Erstellen Ihres Workflows durch Speichern von Dateien in OneDrive oder Aktualisieren vorhandener Dateien in OneDrive. 
* Verwenden von Triggern, um den Workflow zu starten, wenn eine Datei in OneDrive erstellt oder aktualisiert wird.
* Verwenden von Aktionen, um z. B. eine Datei zu kopieren oder zu löschen. So kann beispielsweise bei Eingang einer neuen Office 365-E-Mail mit einer Anlage (Trigger) eine neue Datei in OneDrive erstellt werden (Aktion).

Dieser Artikel beschreibt, wie Sie den OneDrive-Connector in einer Logik-App verwenden, und enthält eine Liste mit den Triggern und Aktionen.

Weitere Informationen zu Logik-Apps finden Sie unter [Was sind Logik-Apps](../logic-apps/logic-apps-overview.md) sowie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-onedrive"></a>Herstellen einer Verbindung mit OneDrive

Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine Verbindung stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Wenn Sie also beispielsweise eine Verbindung mit OneDrive herstellen möchten, müssen Sie zunächst eine entsprechende *Verbindung* erstellen. Geben Sie zum Erstellen einer Verbindung die Anmeldeinformationen ein, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Geben Sie für OneDrive also die Anmeldeinformationen Ihres OneDrive-Kontos ein, um die Verbindung zu erstellen.

### <a name="create-the-connection"></a>Erstellen der Verbindung

[!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]

## <a name="use-a-trigger"></a>Verwenden eines Triggers

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Trigger fragen den Dienst im gewünschten Intervall und mit der gewünschten Häufigkeit ab. Weitere Informationen zu Triggern finden Sie [hier](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Geben Sie im Logik-App-Designer `onedrive` ein, um eine Liste mit den Triggern abzurufen:  

   ![Ein Dialogfeld mit dem Titel „Von Microsoft verwaltete APIs anzeigen“ weist ein Feld auf, das „onedrive“ enthält. Darunter befindet sich eine Liste mit vier Triggern. Der erste ist „OneDrive – Wenn eine Datei erstellt wird“. Der zweite, „OneDrive – Beim Ändern einer Datei“, wurde ausgewählt.](./media/connectors-create-api-onedrive/onedrive-1.png)

2. Wählen Sie **When a file is modified** (Wenn eine Datei geändert wird) aus. Ist bereits eine Verbindung vorhanden, wählen Sie die Schaltfläche „Auswahl anzeigen“ aus, um einen Ordner auszuwählen.

   ![Ein Dialogfeld mit dem Titel „Beim Ändern einer Datei“ verfügt über ein Feld namens „FOLDER“ mit einer zugeordneten Schaltfläche zum Durchsuchen.](./media/connectors-create-api-onedrive/sample-folder.png)

   Wenn Sie zur Anmeldung aufgefordert werden, geben Sie die Anmeldeinformationen ein, um die Verbindung zu erstellen. Die erforderlichen Schritte sind in diesem Artikel unter [Erstellen der Verbindung](connectors-create-api-onedrive.md#create-the-connection) aufgeführt.

   In diesem Beispiel wird die Logik-App ausgeführt, wenn eine Datei im ausgewählten Ordner aktualisiert wird. Fügen Sie zum Anzeigen der Ergebnisse dieses Triggers eine weitere Aktion hinzu, die Ihnen eine E-Mail sendet. Fügen Sie beispielsweise die Office 365 Outlook-Aktion *E-Mail senden* hinzu, um eine E-Mail zu erhalten, wenn eine Datei aktualisiert wird.

3. Wählen Sie die Schaltfläche **Bearbeiten** aus, und legen Sie Werte für **Häufigkeit** und **Intervall** fest. Falls die Abfrage durch den Trigger also beispielsweise alle 15 Minuten erfolgen soll, legen Sie **Häufigkeit** auf **Minute** und **Intervall** auf **15** fest. 

   ![In einem Dialogfeld mit dem Titel „Beim Ändern einer Datei“ werden fünf bezeichnete Felder angezeigt: „FOLDER“, „FREQUENCY“, „INTERVAL“, „TIMEZONE“ und „START TIME“. Die Felder „FREQUENCY“ und „TIME ZONE“ weisen Dropdownlisten auf.](./media/connectors-create-api-onedrive/trigger-properties.png)

4. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.

## <a name="use-an-action"></a>Verwenden einer Aktion

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Wählen Sie das Pluszeichen aus. Sie sehen mehrere Auswahlmöglichkeiten: **Aktion hinzufügen**, **Bedingung hinzufügen** oder eine der Optionen unter **Mehr**.

   ![Ein Screenshot zeigt vier Schaltflächen: „+ Neuer Schritt“, „Aktion hinzufügen“, „Bedingung hinzufügen“ und „Mehr“.](./media/connectors-create-api-onedrive/add-action.png)

2. Wählen Sie **Aktion hinzufügen**aus.

3. Geben Sie im Suchfeld die Zeichenfolge `onedrive` ein, um eine Liste mit allen verfügbaren Aktionen zu erhalten.

   ![Ein Dialogfeld mit dem Titel „Von Microsoft verwaltete APIs anzeigen“ weist ein Feld auf, das „onedrive“ enthält. Im Folgenden finden Sie eine Liste mit acht Aktionen. Die erste ist „OneDrive – „Datei erstellen“ und bereits ausgewählt.](./media/connectors-create-api-onedrive/onedrive-actions.png) 

4. In unserem Beispiel wählen wir **OneDrive - Create file** (OneDrive – Datei erstellen) aus. Falls bereits eine Verbindung vorhanden ist, wählen Sie den Pfad des **Zielordners** für die Datei aus, geben Sie den **Dateinamen** ein, und wählen Sie den gewünschten **Dateiinhalt** aus:  

   ![In einem Dialogfeld mit der Bezeichnung „Datei erstellen“ werden drei Felder mit den Bezeichnungen „FOLDER PATH“, „FILE NAME“ und „FOLDER CONTENT“ angezeigt. Neben dem Feld „FOLDER PATH“ befindet sich eine Schaltfläche zum Durchsuchen des Verzeichnisses.](./media/connectors-create-api-onedrive/sample-action.png)

   Wenn Sie zur Eingabe der Verbindungsinformationen aufgefordert werden, geben Sie die Details zum [Herstellen der Verbindung](#create-the-connection) wie in diesem Thema beschrieben ein.

   In diesem Beispiel erstellen Sie eine neue Datei in einem OneDrive-Ordner. Für die Erstellung der OneDrive-Datei kann die Ausgabe eines anderen Triggers verwendet werden. Fügen Sie beispielsweise den Office 365 Outlook-Trigger *When a new email arrives* (Wenn eine neue E-Mail eingeht) hinzu. Fügen Sie dann die OneDrive-Aktion *Datei erstellen* hinzu, die die Felder „Anlagen“ und „Inhaltstyp“ innerhalb einer ForEach-Schleife verwendet, um die neue Datei in OneDrive zu erstellen.

   ![Ein Dialogfeld mit dem Titel „For each“ verfügt über ein Feld mit der Bezeichnung „Ausgabe von vorherigen Schritten auswählen“, das „Anhänge“ enthält. Es gibt das Dialogfeld „Datei erstellen“, das den Rest des Felds „For each“ mit den Feldern „FOLDER PATH“, „FILE NAME“ und „FILE CONTENT“ abdeckt. ](./media/connectors-create-api-onedrive/foreach-action.png)

5. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/onedriveconnector/) an.

## <a name="next-steps"></a>Nächste Schritte

* [Connectors für Azure Logic Apps](apis-list.md)