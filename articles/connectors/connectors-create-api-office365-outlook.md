---
title: Herstellen einer Verbindung mit Office 365 Outlook
description: Automatisieren von Aufgaben und Workflows, die E-Mails, Kontakte und Kalender in Office 365 Outlook mithilfe von Azure Logic Apps verwalten
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 01/08/2020
tags: connectors
ms.openlocfilehash: b0f2b8b9c369fdb42c7e0e7f77fc090424ae3729
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75732683"
---
# <a name="manage-email-contacts-and-calendars-in-office-365-outlook-by-using-azure-logic-apps"></a>Verwalten von E-Mails, Kontakten und Kalendern in Office 365 Outlook mithilfe von Azure Logic Apps

Mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) und dem [Office 365 Outlook-Connector](/connectors/office365connector/) können Sie durch die Erstellung von Logik-Apps automatisierte Aufgaben und Workflows zur erstellen, die Ihr Office 365-Konto verwalten. So können Sie beispielsweise folgende Aufgaben automatisieren:

* Abrufen, Senden, und Beantworten von E-Mails. 
* Planen von Besprechungen in Ihrem Kalender.
* Hinzufügen und Bearbeiten von Kontakten 

Sie können einen beliebigen Trigger verwenden, um Ihren Workflow zu starten – etwa den Eingang einer neuen E-Mail, die Aktualisierung eines Kalenderelements oder das Eintreten eines Ereignisses in einem anderen Dienst, z. B. Salesforce. Auf das Triggerereignis kann dann mit Aktionen wie dem Senden einer E-Mail oder dem Erstellen eines neuen Kalenderereignisses reagiert werden. 

> [!NOTE]
> Verwenden Sie zum Automatisieren von Aufgaben für ein @outlook.com- oder @hotmail.com-Konto den [Outlook.com-Connector](../connectors/connectors-create-api-outlook.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein [Office 365-Konto](https://www.office.com/)

* ein Azure-Abonnement Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/). 

* Die Logik-App, in der Sie auf Ihr Office 365 Outlook-Konto zugreifen möchten. Um Ihren Workflow mit einem Office 365 Outlook-Trigger starten zu können, benötigen Sie eine [leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md). Wenn Sie Ihrem Workflow eine Office 365 Outlook-Aktion hinzufügen möchten, muss Ihre Logik-App bereits über einen-Trigger verfügen.

## <a name="add-a-trigger"></a>Hinzufügen eines Triggers

Ein [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) ist ein Ereignis, das den Workflow in Ihrer Logik-App startet. In dieser exemplarischen Logik-App wird ein Abfragetrigger verwendet, um Ihr E-Mail-Konto im angegebenen Intervall und mit der angegebenen Frequenz auf aktualisierte Kalenderereignisse zu überprüfen.

1. Öffnen Sie Ihre leere Logik-App im [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

1. Geben Sie im Suchfeld den Begriff `office 365 outlook` als Filter ein. In diesem Beispiel wird **Wenn ein anstehender Termin in Kürze startet** ausgewählt.
   
   ![Auswählen des Triggers zum Starten Ihrer Logik-App](./media/connectors-create-api-office365-outlook/office365-trigger.png)

1. Wenn eine Anmeldeanforderung angezeigt wird, geben Sie Ihre Office 365 Outlook-Anmeldeinformationen an, damit Ihre Logik-App eine Verbindung mit Ihrem Konto herstellen kann. Falls die Verbindung bereits besteht, geben Sie die folgenden Informationen für die Eigenschaften des Triggers an.

   In diesem Beispiel wird der Kalender ausgewählt, den der Trigger überprüft, beispielsweise:

   ![Konfigurieren der Eigenschaften des Triggers](./media/connectors-create-api-office365-outlook/select-calendar.png)

1. Legen Sie im Trigger die Werte für **Frequenz** und **Intervall** fest. Um weitere verfügbare Trigger wie **Zeitzone** hinzuzufügen, wählen Sie diese Eigenschaften aus der Liste **Neuen Parameter hinzufügen** aus.

   Wenn also der Trigger beispielsweise den Kalender alle 15 Minuten überprüfen soll, legen Sie **Häufigkeit** auf **Minute** und **Intervall** auf `15` fest. 

   ![Festlegen von Häufigkeit und Intervall für den Trigger](./media/connectors-create-api-office365-outlook/calendar-settings.png)

1. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

Fügen Sie nun eine Aktion hinzu, die ausgeführt wird, nachdem der Trigger ausgelöst wurde. Beispielsweise können Sie die Twilio-Aktion **Nachricht senden** hinzufügen, die einen Text sendet, wenn ein Kalenderereignis in 15 Minuten beginnt.

## <a name="add-an-action"></a>Hinzufügen einer Aktion

Eine [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) ist ein Vorgang, der durch den Workflow in Ihrer Logik-App ausgeführt wird. Diese Beispiel-Logik-App erstellt einen neuen Kontakt in Office 365 Outlook. Für die Erstellung des Kontakts können Sie die Ausgabe eines anderen Triggers oder einer Aktion verwenden. Ein Beispiel: Angenommen, Ihre Logik-App verwendet den Dynamics 365-Trigger **Wenn ein Datensatz erstellt wird**. Sie können die Office 365 Outlook-Aktion **Kontakt erstellen** hinzufügen und die Ausgaben des SalesForce-Triggers verwenden, um den neuen Kontakt zu erstellen.

1. Öffnen Sie Ihre Logik-App über das [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

1. Wenn Sie eine Aktion als letzten Schritt in Ihrem Workflow hinzufügen möchten, wählen Sie **Neuer Schritt** aus. 

   Wenn Sie zwischen Schritten eine Aktion einfügen möchten, bewegen Sie den Mauszeiger über den Pfeil zwischen diesen Schritten. Wählen Sie das angezeigte Pluszeichen ( **+** ) aus, und wählen Sie dann **Aktion hinzufügen** aus.

1. Geben Sie im Suchfeld den Begriff `office 365 outlook` als Filter ein. In diesem Beispiel wird **Kontakt erstellen** ausgewählt.

   ![Auswählen der Aktion, die in ihrer Logik-App ausgeführt werden soll](./media/connectors-create-api-office365-outlook/office365-actions.png) 

1. Wenn eine Anmeldeanforderung angezeigt wird, geben Sie Ihre Office 365 Outlook-Anmeldeinformationen an, damit Ihre Logik-App eine Verbindung mit Ihrem Konto herstellen kann. Falls die Verbindung bereits besteht, geben Sie die folgenden Informationen für die Eigenschaften der Aktion an.

   In diesem Beispiel wird der Ordner „Kontakte“ ausgewählt, in dem die Aktion den neuen Kontakt erstellt, z. B.:

   ![Konfigurieren der Eigenschaften der Aktion](./media/connectors-create-api-office365-outlook/select-contacts-folder.png)

   Um weitere verfügbare Aktionseigenschaften hinzuzufügen, wählen Sie diese Eigenschaften aus der Liste **Neuen Parameter hinzufügen** aus.

1. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

## <a name="connector-specific-details"></a>Connectorspezifische Details

Technische Details zu Triggern, Aktionen und Grenzwerten, wie sie in der Swagger-Datei des Connectors beschrieben werden, finden Sie auf der [Referenzseite des Connectors](/connectors/office365connector/). 

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
