---
title: Datensicherheits- und Datenschutzrichtlinien für Google-Connectors
description: Erfahren Sie mehr über die Auswirkungen von Sicherheits- und Datenschutzrichtlinien von Google auf Google-Connectors wie Gmail in Azure Logic Apps.
services: logic-apps
ms.suite: integration
ms.reviewer: divswa, logicappspm
ms.topic: conceptual
ms.date: 06/05/2020
ms.openlocfilehash: b82b79d076871b961d5c496bc70beac1b5067b2d
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288115"
---
# <a name="data-security-and-privacy-policies-for-google-connectors-in-azure-logic-apps"></a>Datensicherheits- und Datenschutzrichtlinien für Google-Connectors in Azure Logic Apps

Ab dem **1. Mai 2020** wirken sich Änderungen aufgrund der [Datensicherheits- und Datenschutzrichtlinien](https://www.blog.google/technology/safety-security/project-strobe/) von Google möglicherweise auf Ihre Logik-App-Workflows aus, wenn diese den [Gmail-Connector](/connectors/gmail/) verwenden. Wenn Ihre Logik-Apps den Gmail-Connector mit einem Gmail-Endbenutzerkonto verwenden (E-Mail-Adresse, die mit @gmail.com oder @googlemail.com endet), können Ihre Logik-Apps nur bestimmte, [von Google genehmigte Trigger, Aktionen und Connectors](#approved-connectors) verwenden.

> [!NOTE]
> Wenn Ihre Logik-Apps den Gmail-Connector mit einem G Suite-Geschäftskonto verwenden (E-Mail-Adresse mit benutzerdefinierter Domäne), sind Ihre Logik-Apps nicht betroffen, und es gibt keine Einschränkungen für die Verwendung des Gmail-Connectors.

## <a name="affected-logic-apps"></a>Betroffene Logik-Apps

Wenn Sie über Logik-Apps verfügen, die den Gmail-Connector verwenden, erhalten Sie eine E-Mail zu potenziell betroffenen Logik-Apps. Ab dem **15. Juni 2020** werden allerdings alle nicht konformen Workflows deaktiviert. Sie können dann eine der folgenden Maßnahmen ergreifen:

* Aktualisieren Sie die betroffenen Logik-Apps, indem Sie die [in diesem Thema beschriebenen Schritte](#update-affected-workflows) ausführen. Sie müssen hierfür eine Google-Client-App erstellen, die eine Client-ID und einen geheimen Clientschlüssel bereitstellt, die Sie in Ihrem Gmail-Trigger oder Ihrer Gmail-Aktion für die Authentifizierung verwenden.

* Aktualisieren Sie die betroffenen Logik-Apps so, dass diese nur die [von Google genehmigten Connectors](#approved-connectors) verwenden, und aktivieren Sie dann die deaktivierten Logik-Apps wieder.

<a name="approved-connectors"></a>

## <a name="google-approved-connectors"></a>Von Google genehmigte Connectors

Gemäß dieser Richtlinie können Sie, wenn Sie ein Gmail-Endbenutzerkonto verwenden, den Gmail-Connector nur mit bestimmten, von Google genehmigten Diensten verwenden, wobei sich diese Dienste auch ändern können. Unsere Entwicklerteams arbeiten weiterhin mit Google zusammen, um weitere Dienste zu dieser Liste hinzuzufügen. Die derzeit von Google genehmigten Trigger, Aktionen und Connectors, die Sie bei einem Gmail-Endbenutzerkonto im selben Logik-App-Workflow wie den Gmail-Connector verwenden können, sind die folgenden:

* In Logic Apps integrierte Trigger und Aktionen: Batch, Steuerelement, Datenvorgänge, Datum/Uhrzeit, Flatfile, Liquid, Anforderung, Terminplanung, Variablen und XML

  Von Google nicht genehmigte integrierte Trigger und Aktionen, z. B. HTTP, Azure Functions und Azure Logic Apps, sorgen dafür, dass eine Logik-App nicht mit dem Gmail-Connector kompatibel ist, da die App Daten überall senden und empfangen kann.

* Google-Dienste: Gmail, Google Kalender, Google Kontakte, Google Drive, Google Tabellen und Google Tasks

* Genehmigte Microsoft-Dienste: Dynamics 365, Excel Online, Microsoft Teams, Office 365, OneDrive und SharePoint Online

* Connectors für kundenseitig verwaltete Datenquellen: FTP, RSS, SFTP, SMTP und SQL Server

## <a name="non-compliant-examples"></a>Nicht kompatible Beispiele

Im Folgenden sind einige Beispiele aufgeführt, bei denen der Gmail-Connector mit integrierten Triggern und Aktionen oder verwalteten Connectors verwendet wird, die nicht von Google genehmigt sind:

* Diese Logik-App verwendet den Gmail-Connector mit dem integrierten HTTP-Trigger:

  ![Nicht kompatible Logik-App: Beispiel 1](./media/connectors-google-data-security-privacy-policy/not-compliant-logic-app-1.png)
  
  Diese Logik-App verwendet auch den Google Kalender-Connector, der genehmigt ist.

* Die folgende Logik-App verwendet den Gmail-Connector mit dem Azure Blob Storage-Connector:

  ![Nicht kompatible Logik-App: Beispiel 2](./media/connectors-google-data-security-privacy-policy/not-compliant-logic-app-2.png)

* Diese Logik-App verwendet den Gmail-Connector mit dem Twitter-Connector:

  ![Nicht kompatible Logik-App: Beispiel 3](./media/connectors-google-data-security-privacy-policy/not-compliant-logic-app-3.png)

Die jeweils aktuellen Informationen finden Sie in der [technischen Referenzdokumentation für den Gmail-Connector](/connectors/gmail/).

<a name="update-affected-workflows"></a>

## <a name="steps-for-affected-logic-apps"></a>Schritte bei betroffenen Logik-Apps

Wenn Sie den Gmail-Connector mit einem Gmail-Endbenutzerkonto und nicht von Google genehmigten Connectors in einer Logik-App verwenden müssen, können Sie Ihre eigene Google-App zur persönlichen Verwendung oder zur Verwendung innerhalb Ihres Unternehmens erstellen. Im Folgenden sind die allgemeinen Schritte aufgeführt, die Sie in diesem Szenario ausführen müssen:

1. Erstellen Sie mit der [Google API Console](https://console.developers.google.com) eine Google-Client-App.

1. Verwenden Sie in Ihrem Gmail-Connector die Client-ID und den geheimen Clientschlüssel aus Ihrer Google-Client-App.

Weitere Informationen finden Sie in der [technischen Referenzdokumentation für den Gmail-Connector](/connectors/gmail/#authentication-and-bring-your-own-application).

### <a name="create-google-client-app"></a>Erstellen einer Google-Client-App

Verwenden Sie den [Assistenten für die Google API Console](https://console.developers.google.com/start/api?id=gmail&credential=client_key), und befolgen Sie die Anweisungen, um ein Projekt für Ihre Client-App zu erstellen. Alternativ finden Sie detaillierte Schritte in den Anweisungen in der [technischen Referenzdokumentation für den Gmail-Connector](/connectors/gmail/#authentication-and-bring-your-own-application).

Wenn Sie fertig sind, sieht Ihr Bildschirm wie in diesem Beispiel aus, mit dem Unterschied, dass Sie über Ihre eigene **Client-ID** und Ihren eigenen **geheimen Clientschlüssel** verfügen, die Sie später in Ihrer Logik-App verwenden.

![Client-ID und geheimer Clientschlüssel für Ihre Google-Client-App](./media/connectors-google-data-security-privacy-policy/google-api-console.png)

### <a name="use-client-app-settings-in-logic-app"></a>Verwenden der Client-App-Einstellungen in der Logik-App

Gehen Sie folgendermaßen vor, um die Client-ID und den geheimen Clientschlüssel aus Ihrer Google-Client-App in Ihrem Gmail-Trigger oder Ihrer Gmail-Aktion zu verwenden:

1. Öffnen Sie Ihre Logik-App über das [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

1. Wenn Sie einen neuen Gmail-Trigger oder eine neue Gmail-Aktion hinzufügen und eine komplett neue Verbindung erstellen, fahren Sie mit dem nächsten Schritt fort. Klicken Sie andernfalls im Gmail-Trigger oder der Gmail-Aktion auf **Verbindung ändern** > **Neu hinzufügen**, z. B.:

   ![Klicken auf „Verbindung ändern“ > „Neu hinzufügen“](./media/connectors-google-data-security-privacy-policy/change-gmail-connection.png)

1. Geben Sie Ihre Verbindungsinformationen ein, z. B.:

   ![Angeben von Verbindungsinformationen](./media/connectors-google-data-security-privacy-policy/authentication-type-bring-your-own.png)

   | Eigenschaft | Wert | BESCHREIBUNG |
   |----------|-------|-------------|
   | **Authentifizierungstyp** | **Bring Your Own Application** | Mit diesem Wert geben Sie an, dass Sie Ihre eigene Client-App für die Authentifizierung verwenden. |
   | **Client-ID** | <*client-ID*> | Geben Sie hier die Client-ID aus Ihrer Google-Client-App an. |
   | **Geheimer Clientschlüssel** | <*client-secret*> | Geben Sie hier den geheimen Clientschlüssel aus Ihrer Google-Client-App an. |
   ||||

1. Klicken Sie auf **Anmelden**, wenn Sie fertig sind.

   Es wird eine Seite mit der von Ihnen erstellten Client-App angezeigt. Wenn Sie ein Gmail-Endbenutzerkonto verwenden, wird möglicherweise eine Seite angezeigt, auf der steht, dass Ihre Client-App nicht von Google verifiziert ist, und Sie werden dazu aufgefordert, zunächst den Zugriff auf Ihr Google-Konto zuzulassen.

   ![Aufforderung zum Zulassen des Zugriffs auf Ihr Google-Konto](./media/connectors-google-data-security-privacy-policy/allow-access-authorized-domain.png)

1. Klicken Sie ggf. auf **Allow** (Zulassen).

   Sie können nun den Gmail-Connector ohne Einschränkungen in Ihrer Logik-App verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Gmail-Connector](/connectors/gmail/)

