---
title: Herstellen einer Verbindung mit SMTP aus Azure Logic Apps
description: Automatisieren von Aufgaben und Workflows, die mithilfe von Azure Logic Apps E-Mails über Ihr SMTP-Konto (Simple Mail Transfer Protocol) senden
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: 9989d0ebd95bfe5ee49be2ba76b73e07630b519a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "87283944"
---
# <a name="send-email-from-your-smtp-account-with-azure-logic-apps"></a>Senden von E-Mails von Ihrem SMTP-Konto mithilfe von Azure Logic Apps

Mit Azure Logic Apps und dem SMTP-Connector (Simple Mail Transfer Protocol) können Sie automatisierte Aufgaben und Workflows erstellen, die E-Mails von Ihrem SMTP-Konto senden. Sie können die Ausgaben von SMTP-Aktionen auch in anderen Aktionen verwenden. Sie können beispielsweise Ihr Team in Slack über den Slack-Connector benachrichtigen, nachdem eine Mail von Ihrem SMTP-Konto gesendet wurde. Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/). 

* Ihr SMTP-Konto und Benutzeranmeldeinformationen.

  Ihre Anmeldeinformationen autorisieren Ihre Logik-App zur Erstellung einer Verbindung mit Ihrem SMTP-Konto sowie zum Zugriff auf das Konto.

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Die Logik-App, in der Sie auf Ihr SMTP-Konto zugreifen möchten. Um eine SMTP-Aktion zu verwenden, starten Sie Ihre Logik-App mit einem Trigger – z.B. einem Salesforce-Trigger, falls Sie ein Salesforce-Konto besitzen.

  Sie können Ihre Logik-App beispielsweise mit dem Salesforce-Trigger **Wenn ein Datensatz erstellt wird** starten. 
  Dieser Trigger wird jedes Mal ausgelöst, wenn in Salesforce ein neuer Datensatz erstellt wird, z.B. ein neuer Lead. 
  Auf diesen Trigger können Sie die SMTP-Aktion **E-Mail senden** folgen lassen. Wenn ein neuer Datensatz erstellt wurde, sendet Ihre Logik-App eine E-Mail zu diesem neuen Datensatz von Ihrem SMTP-Konto.

## <a name="connect-to-smtp"></a>Herstellen einer SMTP-Verbindung

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie Ihre Logik-App im Logik-App-Designer, sofern sie nicht bereits geöffnet ist.

1. Wählen Sie im letzten Schritt zum Hinzufügen einer SMTP-Aktion die Option **Neuer Schritt** aus. 

   Wenn Sie zwischen Schritten eine Aktion einfügen möchten, bewegen Sie den Mauszeiger über den Pfeil zwischen den Schritten. 
   Wählen Sie das daraufhin angezeigte Pluszeichen ( **+** ) und dann **Aktion hinzufügen** aus.

1. Geben Sie im Suchfeld den Begriff „smtp“ als Filter ein. Wählen Sie in der Liste mit den Aktionen die gewünschte Aktion aus.

1. Wenn Sie dazu aufgefordert werden, geben Sie die folgenden Verbindungsinformationen an:

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Verbindungsname** | Ja | Ein Name für die Verbindung mit Ihrem SMTP-Server | 
   | **SMTP-Serveradresse** | Ja | Die Adresse Ihres SMTP-Servers | 
   | **Benutzername** | Ja | Der Benutzername für Ihr SMTP-Konto | 
   | **Kennwort** | Ja | Das Kennwort für Ihr SMTP-Konto | 
   | **SMTP-Serverport** | Nein | Ein bestimmter Port auf Ihrem SMTP-Server, den Sie verwenden möchten | 
   | **SSL aktivieren?** | Nein | Aktivieren oder Deaktivieren der TLS/SSL-Verschlüsselung. | 
   |||| 

1. Geben Sie die erforderlichen Informationen zu Ihrer ausgewählten Aktion ein. 

1. Speichern Sie Ihre Logik-App, oder fahren Sie mit dem Erstellen des Workflows Ihrer Logik-App fort.

## <a name="connector-reference"></a>Connector-Referenz

Weitere technische Details zu diesem Connector, z. B. Trigger, Aktionen und Grenzwerte, wie sie in der Swagger-Datei des Connectors beschrieben werden, finden Sie auf der [Referenzseite des Connectors](/connectors/smtpconnector/).

> [!NOTE]
> Für Logik-Apps in einer [Integrationsdienstumgebung (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) verwendet die mit ISE bezeichnete Version dieses Connectors stattdessen die [ISE-Nachrichtengrenzwerte](../logic-apps/logic-apps-limits-and-config.md#message-size-limits).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
