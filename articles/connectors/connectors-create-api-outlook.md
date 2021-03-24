---
title: Verbinden mit Outlook.com
description: Automatisieren von Aufgaben und Workflows zur Verwaltung von E-Mails, Kalendern und Kontakten in Outlook.com mithilfe von Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/18/2016
tags: connectors
ms.openlocfilehash: 8d3b180b6f1e9dc4ec4b09dd81786cc81e8588da
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "75707185"
---
# <a name="manage-email-calendars-and-contacts-in-outlookcom-by-using-azure-logic-apps"></a>Verwalten von E-Mails, Kalendern und Kontakten in Outlook.com mithilfe von Azure Logic Apps

Mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) und dem [Outlook.com-Connector](/connectors/outlook/) können Sie durch die Erstellung von Logik-Apps automatisierte Aufgaben und Workflows zur Verwaltung Ihres @outlook.com- oder @hotmail.com-Kontos erstellen. So können Sie beispielsweise folgende Aufgaben automatisieren:

* Abrufen, Senden, und Beantworten von E-Mails.
* Planen von Besprechungen in Ihrem Kalender.
* Hinzufügen und Bearbeiten von Kontakten

Sie können einen beliebigen Trigger verwenden, um Ihren Workflow zu starten – etwa den Eingang einer neuen E-Mail, die Aktualisierung eines Kalenderelements oder ein Ereignis in einem anderen Dienst. Auf das Triggerereignis kann dann mit Aktionen wie dem Senden einer E-Mail oder dem Erstellen eines neuen Kalenderereignisses reagiert werden.

> [!NOTE]
> Wenn Sie Aufgaben für ein Microsoft-Geschäftskonto (z. B. @fabrikam.onmicrosoft.com) automatisieren möchten, verwenden Sie den [Office 365 Outlook-Connector](../connectors/connectors-create-api-office365-outlook.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein [Outlook.com-Konto](https://outlook.live.com/owa/)

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/). 

* Die Logik-App, in der Sie auf Ihr Outlook.com-Konto zugreifen möchten. Um Ihren Workflow mit einem Outlook.com-Trigger starten zu können, benötigen Sie eine [leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md). Wenn Sie Ihrem Workflow eine Outlook.com-Aktion hinzufügen möchten, muss Ihre Logik-App bereits über einen-Trigger verfügen.

## <a name="add-a-trigger"></a>Hinzufügen eines Triggers

Ein [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) ist ein Ereignis, das den Workflow in Ihrer Logik-App startet. In dieser exemplarischen Logik-App wird ein Abfragetrigger verwendet, um Ihr E-Mail-Konto im angegebenen Intervall und mit der angegebenen Frequenz auf neue E-Mails zu überprüfen.

1. Öffnen Sie Ihre leere Logik-App im [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

1. Geben Sie im Suchfeld „outlook.com“ als Filter ein. Wählen Sie in diesem Beispiel die Option **Wenn eine neue E-Mail empfangen wird** aus.

1. Wenn eine Anmeldeanforderung angezeigt wird, geben Sie Ihre Outlook.com-Anmeldeinformationen an, damit Ihre Logik-App eine Verbindung mit Ihrem Konto herstellen kann. Falls die Verbindung bereits besteht, geben Sie die folgenden Informationen für die Triggereigenschaften an:

1. Legen Sie im Trigger die Werte für **Häufigkeit** und **Intervall** fest.

   Falls die Abfrage durch den Trigger also beispielsweise alle 15 Minuten erfolgen soll, legen Sie **Frequenz** auf **Minute** und **Intervall** auf **15** fest.

1. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus, um Ihre Logik-App zu speichern.

Fügen Sie eine weitere Aktion hinzu, um auf den Trigger zu reagieren. Sie können beispielsweise die Twilio-Aktion **Nachricht senden** hinzufügen, um einen Text zu senden, wenn eine E-Mail eingeht.

## <a name="add-an-action"></a>Hinzufügen einer Aktion

Eine [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) ist ein Vorgang, der durch den Workflow in Ihrer Logik-App ausgeführt wird. In dieser exemplarischen Logik-App wird eine E-Mail von Ihrem Outlook.com-Konto aus gesendet. Die Aktion kann mit der Ausgabe eines anderen Triggers aufgefüllt werden. Ein Beispiel: Angenommen, Ihre Logik-App verwendet den Salesforce-Trigger **Wenn ein Objekt erstellt wird**. In diesem Fall können Sie die Outlook.com-Aktion **E-Mail senden** hinzufügen und die Ausgaben des Salesforce-Triggers in der E-Mail verwenden.

1. Öffnen Sie Ihre Logik-App über das [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

1. Wenn Sie eine Aktion als letzten Schritt in Ihrem Workflow hinzufügen möchten, wählen Sie **Neuer Schritt** aus. 

   Wenn Sie zwischen Schritten eine Aktion einfügen möchten, bewegen Sie den Mauszeiger über den Pfeil zwischen diesen Schritten. Wählen Sie das angezeigte Pluszeichen ( **+** ) aus, und wählen Sie dann **Aktion hinzufügen** aus.

1. Geben Sie im Suchfeld „outlook.com“ als Filter ein. Wählen Sie in diesem Beispiel die Option **E-Mail senden** aus. 

1. Wenn eine Anmeldeanforderung angezeigt wird, geben Sie Ihre Outlook.com-Anmeldeinformationen an, damit Ihre Logik-App eine Verbindung mit Ihrem Konto herstellen kann. Falls die Verbindung bereits besteht, geben Sie die folgenden Informationen für die Aktionseigenschaften an.

1. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus, um Ihre Logik-App zu speichern.

## <a name="connector-reference"></a>Connector-Referenz

Technische Details, z.B. Trigger, Aktionen und Grenzwerte, wie sie in der Swagger-Datei des Connectors beschrieben werden, finden Sie auf der [Referenzseite des Connectors](/connectors/outlook/). 

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
