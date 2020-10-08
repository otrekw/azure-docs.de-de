---
title: Erstellen eines automatisierten Integrationsworkflows
description: 'Schnellstart: Erstellen Ihres ersten automatisierten Workflows mithilfe von Azure Logic Apps für Systemintegrationslösungen und EAI-Lösungen (Enterprise Application Integration)'
services: logic-apps
ms.suite: integration
ms.reviewer: logicappspm
ms.topic: quickstart
ms.custom: mvc
ms.date: 07/23/2020
ms.openlocfilehash: c40bec80d9f61cf46221cbfe7dde80f3a7b46f6f
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "89658302"
---
# <a name="quickstart-create-your-first-automated-integration-workflow-by-using-azure-logic-apps---azure-portal"></a>Schnellstart: Erstellen Ihres ersten automatisierten Integrationsworkflows mit Azure Logic Apps – Azure-Portal

In dieser Schnellstartanleitung werden die grundlegenden allgemeinen Konzepte vorgestellt, auf denen das Erstellen Ihres ersten Workflows mithilfe von [Azure Logic Apps](logic-apps-overview.md) beruht. Dazu gehören z.B. das Erstellen einer leeren Logik-App, das Hinzufügen eines Triggers und einer Aktion sowie das anschließende Testen Ihrer Logik-App. In diesem Schnellstart erstellen Sie eine Logik-App, die den RSS-Feed einer Website regelmäßig auf neue Elemente überprüft. Wenn neue Elemente vorhanden sind, sendet die Logik-App eine E-Mail für jedes Element. Am Ende entspricht Ihre Logik-App grob dem folgenden Workflow:

![Konzeptionelle Grafik, die einen allgemeinen Logik-App-Beispielworkflow darstellt.](./media/quickstart-create-first-logic-app-workflow/quickstart-workflow-overview.png)

Für dieses Szenario benötigen Sie ein Azure-Abonnement oder müssen sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). Darüber hinaus ist ein E-Mail-Konto von einem von Azure Logic Apps unterstützten Dienst wie Office 365 Outlook, Outlook.com oder Gmail erforderlich. Weitere unterstützte E-Mail-Dienste finden Sie im [Artikel zu den Connectors](/connectors/). Die Logik-App verwendet in diesem Beispiel ein Geschäfts-, Schul- oder Unikonto. Wenn Sie einen anderen E-Mail-Dienst verwenden, sind die allgemeinen Schritte identisch, doch kann die Benutzeroberfläche geringfügig abweichen.

> [!IMPORTANT]
> Wenn Sie den Gmail-Connector verwenden möchten, können nur G-Suite-Geschäftskonten diesen Connector ohne Einschränkung in Logik-Apps verwenden. Wenn Sie über ein Gmail-Consumerkonto verfügen, können Sie diesen Connector nur mit bestimmten von Google genehmigten Diensten verwenden, oder Sie können [eine Google-Client-App erstellen, die für die Authentifizierung mit Ihrem Gmail-Connector verwendet werden soll](/connectors/gmail/#authentication-and-bring-your-own-application). Weitere Informationen finden Sie unter [Datensicherheit und Datenschutzrichtlinien für Google-Connectors in Azure Logic Apps](../connectors/connectors-google-data-security-privacy-policy.md).

## <a name="create-your-logic-app"></a>Erstellen Ihrer Logik-App

1. Melden Sie sich mit den Anmeldeinformationen Ihres Azure-Kontos beim [Azure-Portal](https://portal.azure.com) an.

1. Geben Sie in das Suchfeld des Azure-Portals `logic apps` ein, und klicken Sie auf **Logic Apps**.

   ![Screenshot des Suchfelds im Azure-Portal mit „Logic Apps“ als Suchbegriff und „Logic Apps“ als ausgewähltem Suchergebnis.](./media/quickstart-create-first-logic-app-workflow/find-select-logic-apps.png)

1. Wählen Sie auf der Seite **Logic Apps** die Option **Hinzufügen** aus.

   ![Screenshot mit der Liste der Logik-Apps und ausgewählter Schaltfläche „Hinzufügen“.](./media/quickstart-create-first-logic-app-workflow/add-new-logic-app.png)

1. Geben Sie im Bereich **Logik-App** Details zu Ihrer Logik-App wie nachfolgend gezeigt ein.

   ![Screenshot des Erstellungsbereichs für Logik-Apps mit Details für eine neue Logik-App.](./media/quickstart-create-first-logic-app-workflow/create-logic-app-settings.png)

   | Eigenschaft | Wert | BESCHREIBUNG |
   |----------|-------|-------------|
   | **Name** | <*logic-app-name*> | Der Name Ihrer Logik-App, der regionsübergreifend eindeutig sein muss und nur Buchstaben, Ziffern, Bindestriche (`-`), Unterstriche (`_`), Klammern (`(`, `)`) und Punkte (`.`) enthalten darf. In diesem Beispiel wird „My-First-Logic-App“ verwendet. |
   | **Abonnement** | <*Name des Azure-Abonnements*> | Der Name Ihres Azure-Abonnements |
   | **Ressourcengruppe** | <*Name der Azure-Ressourcengruppe*> | Der Name der [Azure-Ressourcengruppe](../azure-resource-manager/management/overview.md), der regionsübergreifend eindeutig sein muss, und die zum Organisieren verwandter Ressourcen verwendet wird. In diesem Beispiel wird „My-First-LA-RG“ verwendet. |
   | **Location** | <*Azure-Region*> | Die Region, in der die Informationen zu Ihrer Logik-App gespeichert werden sollen. In diesem Beispiel wird „USA, Westen“ verwendet. |
   | **Log Analytics** | Aus | Behalten Sie die Einstellung **Aus** für die Diagnoseprotokollierung bei. |
   ||||

1. Wählen Sie, wenn Sie bereit sind, **Überprüfen + erstellen** aus. Bestätigen Sie die von Ihnen angegebenen Details, und wählen Sie **Erstellen** aus.

1. Nachdem Azure Ihre App erfolgreich bereitgestellt hat, wählen Sie **Zu Ressource wechseln** aus.

   ![Screenshot mit der Ressourcenbereitstellungsseite und ausgewählter Schaltfläche für „Zu Ressource wechseln“.](./media/quickstart-create-first-logic-app-workflow/go-to-new-logic-app-resource.png)

   Sie können zum Suchen und Auswählen Ihrer Logik-App auch den Namen in das Suchfeld eingeben.

   Der Logik-App-Designer wird geöffnet, und es wird eine Seite mit einem Einführungsvideo und häufig verwendeten Triggern angezeigt. Wählen Sie unter **Vorlagen** die Option **Leere Logik-App**.

   ![Screenshot mit dem Vorlagenkatalog des Designers für Logic Apps und ausgewählter Vorlage „Leere Logik-App“.](./media/quickstart-create-first-logic-app-workflow/choose-logic-app-template.png)

Fügen Sie als Nächstes einen [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, der bei einem neuen RSS-Feedelement ausgelöst wird. Jede Logik-App muss mit einem Trigger beginnen, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder eine bestimmte Bedingung erfüllt wird. Bei jeder Auslösung des Triggers erstellt die Azure Logic Apps-Engine eine Logik-App-Instanz, mit der Ihr Workflow gestartet und ausgeführt wird.

<a name="add-rss-trigger"></a>

## <a name="add-the-rss-trigger"></a>Hinzufügen des RSS-Triggers

1. Wählen Sie im **Logik-App-Designer** unterhalb des Suchfelds die Option **Alle** aus.

1. Um den RSS-Connector zu suchen, geben Sie `rss` in das Suchfeld ein. Wählen Sie aus der Triggerliste den RSS-Trigger **Beim Veröffentlichen eines Feedelements** aus.

   ![Screenshot des Designers für Logic Apps mit „rss“ im Suchfeld und ausgewähltem Trigger „Beim Veröffentlichen eines Feedelements“.](./media/quickstart-create-first-logic-app-workflow/add-rss-trigger-new-feed-item.png)

1. Geben Sie die Informationen für Ihren Trigger wie in diesem Schritt beschrieben an:

   ![Screenshot des Designers für Logic Apps mit RSS-Triggereinstellungen, einschließlich RSS-URL, -Häufigkeit und -Intervall.](./media/quickstart-create-first-logic-app-workflow/add-rss-trigger-settings.png)

   | Eigenschaft | Wert | BESCHREIBUNG |
   |----------|-------|-------------|
   | **Die URL des RSS-Feeds** | <*RSS-feed-URL*> | Der Link für den RSS-Feed, den Sie überwachen möchten. Dieses Beispiel verwendet den RSS-Feed des Wall Street Journals unter `https://feeds.a.dj.com/rss/RSSMarketsMain.xml`, doch wenn Sie möchten, können Sie Ihre eigene RSS-Feed-URL verwenden. |
   | **Intervall** | 1 | Die Anzahl von Warteintervallen zwischen Überprüfungen |
   | **Frequency** | Minute | Die Zeiteinheit für die Intervalle zwischen Überprüfungen. |
   ||||

   Das Intervall und die Häufigkeit definieren zusammen den Zeitplan für den Trigger Ihrer Logik-App. Diese Logik-App überprüft den Feed minütlich.

1. Klicken Sie auf die Titelleiste des Triggers, um die Triggerdetails vorerst auszublenden.

   ![Screenshot des Designers für Logic Apps mit reduzierter Logik-App-Form.](./media/quickstart-create-first-logic-app-workflow/collapse-trigger-shape.png)

1. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

Ihre Logik-App befindet sich jetzt im Livemodus, aber es wird vorerst nur der RSS-Feed überprüft. Fügen Sie daher eine Aktion hinzu, die reagiert, wenn der Trigger ausgelöst wird.

## <a name="add-the-send-email-action"></a>Hinzufügen der Aktion „E-Mail senden“

Fügen Sie nun eine [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, die eine E-Mail sendet, wenn im RSS-Feed ein neues Element erscheint.

1. Wählen Sie unterhalb des Triggers **Beim Veröffentlichen eines Feedelements** die Option **+ Neuer Schritt** aus.

   ![Screenshot des Designers für Logic Apps mit „Neuer Schritt“.](./media/quickstart-create-first-logic-app-workflow/add-new-step-under-trigger.png)

1. Wählen Sie unterhalb von **Aktion auswählen** und unterhalb des Suchfelds die Option **Alle** aus.

1. Geben Sie im Suchfeld `send an email` ein, um Connectors zu finden, die diese Aktion anbieten. Um die Liste der Aktionen nach einer bestimmten App oder einem Dienst zu filtern, können Sie zuerst die App oder den Dienst auswählen.

   Wenn Sie z. B. ein Geschäfts-, Schul- oder Unikonto von Microsoft verwenden und Office 365 Outlook verwenden möchten, wählen Sie **Office 365 Outlook** aus. Oder wenn Sie ein persönliches Microsoft-Konto verwenden, können Sie „Outlook.com“ auswählen. Dieses Beispiel fährt mit Office 365 Outlook fort:

   ![Screenshot des Designers für Logic Apps mit ausgewähltem „Office 365 Outlook“-Connector.](./media/quickstart-create-first-logic-app-workflow/select-connector.png)

   Sie können die gewünschte zu verwendende Aktion jetzt leichter finden und auswählen, z. B. `send an email`:

   ![Screenshot des Designers für Logic Apps mit einer Liste mit gefilterten Aktionen.](./media/quickstart-create-first-logic-app-workflow/filtered-actions-list.png)

1. Wenn der ausgewählte E-Mail-Connector Sie auffordert, Ihre Identität zu authentifizieren, führen Sie diesen Schritt jetzt aus, um eine Verbindung zwischen Ihrer Logik-App und Ihrem E-Mail-Dienst herzustellen.

   > [!NOTE]
   > In diesem speziellen Beispiel authentifizieren Sie Ihre Identität manuell. Connectors, die eine Authentifizierung erfordern, unterscheiden sich jedoch in Hinsicht auf die jeweils unterstützten Authentifizierungstypen. Außerdem stehen Optionen bereit, mit denen Sie die Handhabung der Authentifizierung einrichten können. Wenn Sie beispielsweise Azure Resource Manager-Vorlagen für die Bereitstellung verwenden, können Sie Eingaben, die Sie häufig oder einfach ändern möchten (z. B. Verbindungsinformationen), parametrisieren und die Sicherheit dafür erhöhen. Weitere Informationen finden Sie in den folgenden Themen:
   >
   > * [Vorlagenparameter für die Bereitstellung](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#template-parameters)
   > * [Autorisieren von OAuth-Verbindungen](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md#authorize-oauth-connections)
   > * [Authentifizieren des Zugriffs mit verwalteten Identitäten](../logic-apps/create-managed-service-identity.md)
   > * [Authentifizieren von Verbindungen für die Logik-App-Bereitstellung](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#authenticate-connections)

1. Geben Sie in der Aktion **E-Mail senden** die Informationen an, die in E-Mail aufgenommen werden sollen.

   1. Geben Sie im Feld **An** die E-Mail-Adresse des Empfängers ein. Zu Testzwecken können Sie hier Ihre E-Mail-Adresse angeben.

      Ignorieren Sie vorerst die angezeigte Liste **Dynamischen Inhalt hinzufügen**. Wenn Sie in einige Bearbeitungsfelder klicken, wird diese Liste mit verfügbaren Ausgaben des vorherigen Schritts angezeigt, die Sie als Eingaben für die aktuelle Aktion verwenden können.

   1. Geben Sie im Feld **Betreff** den folgenden Text mit einem nachstehenden Leerzeichen ein: `New RSS item: `.

      ![Screenshot des Designers für Logic Apps mit der Aktion „E-Mail senden“ und dem im Eigenschaftenfeld „Betreff“ platzierten Cursor.](./media/quickstart-create-first-logic-app-workflow/send-email-subject.png)

   1. Wählen Sie in der Liste **Dynamischen Inhalt hinzufügen** den Eintrag **Feedtitel** aus, der vom Trigger „Beim Veröffentlichen eines Feedelements“ ausgegeben wird, der Ihnen den RSS-Elementtitel zur Verwendung verfügbar macht.

      ![Screenshot des Designers für Logic Apps mit der Aktion „E-Mail senden“ und dem im Eigenschaftenfeld „Betreff“ platzierten Cursor mit einer geöffneten Liste dynamischer Inhalte und der ausgewählten Ausgabe „Feedtitel“.](./media/quickstart-create-first-logic-app-workflow/send-email-subject-dynamic-content.png)

      > [!TIP]
      > Wählen Sie in der Liste dynamischer Inhalt, wenn keine Ausgaben des Triggers „Beim Veröffentlichen eines Feedelements“ angezeigt werden, neben der Kopfzeile der Aktion **Mehr anzeigen** aus.
      > 
      > ![Screenshot des Designers für Logic Apps mit geöffneter Liste dynamischer Inhalte und für den Trigger ausgewähltem „Mehr anzeigen“.](./media/quickstart-create-first-logic-app-workflow/dynamic-content-list-see-more-actions.png)

      Wenn der Vorgang abgeschlossen ist, sieht der Betreff der E-Mail wie in diesem Beispiel aus:

      ![Screenshot des Designers für Logic Apps mit der Aktion „E-Mail senden“ und einem E-Mail-Beispielbetreff mit der enthaltenen Eigenschaft „Feedtitel“.](./media/quickstart-create-first-logic-app-workflow/send-email-feed-title.png)

      Wenn im Designer eine For each-Schleife angezeigt wird, haben Sie ein Token für ein Array ausgewählt, etwa das Token **categories-Item**. Für diese Art von Token fügt der Designer automatisch diese Schleife für die Aktion hinzu, die auf das Token verweist. Auf diese Weise führt Ihre Logik-App die gleiche Aktion für jedes Arrayelement durch. Wählen Sie zum Entfernen der Schleife auf der Titelleiste der Schleife die **Auslassungspunkte** ( **...** ) und dann **Löschen** aus.

   1. Geben Sie im Feld **Text** den folgenden Text ein, und wählen Sie die folgenden Token für den E-Mail-Text aus. Drücken Sie UMSCHALT+EINGABETASTE, um in einem Bearbeitungsfeld leere Zeilen hinzuzufügen.

      ![Screenshot des Designers für Logic Apps mit der Aktion „E-Mail senden“ und im Feld „Text“ ausgewählten Eigenschaften.](./media/quickstart-create-first-logic-app-workflow/send-email-body.png)

      | Eigenschaft | BESCHREIBUNG |
      |----------|-------------|
      | **Feedtitel** | Der Titel des Elements |
      | **Feed veröffentlicht am** | Datum und Uhrzeit der Elementveröffentlichung |
      | **Link zum primären Feed** | Die URL für das Element |
      |||

1. Speichern Sie Ihre Logik-App.

Testen Sie als Nächstes Ihre Logik-App.

## <a name="run-your-logic-app"></a>Ausführen Ihrer Logik-App

Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen** aus, um Ihre Logik-App manuell zu starten. Oder warten Sie, bis Ihre Logik-App den RSS-Feed gemäß dem festgelegten Zeitplan (minütlich) überprüft.

![Screenshot des Designers für Logic Apps mit ausgewählter Schaltfläche „Ausführen“ in der Designer-Symbolleiste.](./media/quickstart-create-first-logic-app-workflow/run-logic-app-test.png)

Falls der RSS-Feed über neue Elemente verfügt, sendet Ihre Logik-App für jedes neue Element eine E-Mail. Andernfalls wartet Ihre Logik mit einer erneuten Prüfung bis zum nächsten Intervall. Überprüfen Sie Ihren Ordner mit den Junk-E-Mails, falls Sie keine E-Mails erhalten.

Im Anschluss sehen Sie eine Beispiel-E-Mail, die von dieser Logik-App gesendet wird.

![Screenshot mit einer Beispiel-E-Mail, die empfangen wird, wenn ein neues RSS-Feedelement angezeigt wird.](./media/quickstart-create-first-logic-app-workflow/monitor-rss-feed-email.png)

Aus technischer Sicht passiert Folgendes: Wenn der Trigger den RSS-Feed prüft und neue Elemente findet, wird der Trigger ausgelöst, und die Azure Logic Apps-Engine erstellt eine Instanz Ihres Logik-App-Workflows, mit der Aktionen im Workflow ausgeführt werden. Falls der Trigger keine neuen Elemente findet, wird er nicht ausgelöst und überspringt das Instanziieren des Workflows.

Herzlichen Glückwunsch! Sie haben Ihre erste Logik-App über das Azure-Portal erstellt und ausgeführt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie dieses Beispiel nicht mehr benötigen, löschen Sie die Ressourcengruppe mit Ihrer Logik-App und den dazugehörigen Ressourcen.

1. Geben Sie in das Azure-Suchfeld `resource groups` ein, und wählen Sie dann **Ressourcengruppen** aus.

   ![Screenshot des Suchfelds im Azure-Portal mit dem Suchbegriff „Ressourcengruppen“.](./media/quickstart-create-first-logic-app-workflow/find-resource-groups.png)

1. Suchen Sie die Ressourcengruppe Ihrer Logik-App, und wählen Sie sie aus. Wählen Sie im Bereich **Übersicht** die Option **Ressourcengruppe löschen** aus.

   ![Screenshot des Azure-Portals mit ausgewählter Ressourcengruppe und Schaltfläche für „Ressourcengruppe löschen“.](./media/quickstart-create-first-logic-app-workflow/delete-resource-group.png)

1. Wenn der Bestätigungsbereich angezeigt wird, geben Sie den Ressourcengruppennamen ein, und wählen Sie **Löschen** aus.

   ![Screenshot des Azure-Portals mit Bestätigungsbereich und zum Löschen eingegebenem Ressourcengruppennamen.](./media/quickstart-create-first-logic-app-workflow/delete-resource-group-2.png)

> [!NOTE]
> Wenn Sie eine Logik-App löschen, werden keine neuen Ausführungen instanziiert. Alle in Bearbeitung befindlichen und ausstehenden Ausführungen werden abgebrochen. Bei Tausenden von Ausführungen kann der Abbruch möglicherweise erhebliche Zeit in Anspruch nehmen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Ihre erste Logik-App erstellt, die gemäß einem festgelegten Zeitplan (minütlich) nach RSS-Updates sucht und eine Aktion ausführt (E-Mail senden), wenn ein Update vorhanden ist. Weitere Informationen finden Sie im folgenden Tutorial, in dem komplexere zeitplanbasierte Workflows erstellt werden:

> [!div class="nextstepaction"]
> [Überprüfen der Verkehrslage mit einer planerbasierten Logik-App](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)
