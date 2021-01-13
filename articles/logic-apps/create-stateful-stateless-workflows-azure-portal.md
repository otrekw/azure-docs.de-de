---
title: Erstellen von Logic Apps-Workflows (Vorschau) im Azure-Portal
description: Erstellen Sie Workflows für Automatisierungs- und Integrationsszenarien mit „Azure Logic Apps (Vorschau)“ im Azure-Portal, und führen Sie sie aus.
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, logicappspm, az-logic-apps-dev
ms.topic: conceptual
ms.date: 12/07/2020
ms.openlocfilehash: d10689937a037469399863395e0190e399334bd3
ms.sourcegitcommit: fec60094b829270387c104cc6c21257826fccc54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/09/2020
ms.locfileid: "96924038"
---
# <a name="create-stateful-and-stateless-workflows-in-the-azure-portal-with-azure-logic-apps-preview"></a>Erstellen zustandsbehafteter und zustandsloser Workflows im Azure-Portal mit „Azure Logic Apps (Vorschau)“

> [!IMPORTANT]
> Diese Funktion befindet sich in der öffentlichen Vorschauphase, wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mit [Azure Logic Apps (Vorschau)](logic-apps-overview-preview.md) können Sie Automatisierungs- und Integrationslösungen für Apps, Daten, Clouddienste und Systeme erstellen, indem Sie Logik-Apps erstellen und ausführen, die [*zustandsbehaftete* und *zustandslose* Workflows](logic-apps-overview-preview.md#stateful-stateless) einbeziehen, indem Sie mit dem neuen Ressourcentyp **Logik-App (Vorschau)** beginnen. Mit diesem neuen Logik-App-Typ können Sie mehrere Workflows erstellen, die von der neu gestalteten Azure Logic Apps-Runtime (Vorschau) unterstützt werden. Diese Runtime bietet Portabilität, eine bessere Leistung und mehr Flexibilität bei der Bereitstellung und Ausführung in verschiedenen Hostingumgebungen – nicht nur in Azure, sondern auch in Docker-Containern. Weitere Informationen zum neuen Logik-App-Typ finden Sie unter [Übersicht über die Vorschauversion von Azure Logic Apps](logic-apps-overview-preview.md).

![Screenshot, der das Azure-Portal mit dem Workflow-Designer für die „Logik-App (Vorschau)“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/azure-portal-logic-apps-overview.png)

Im Azure-Portal können Sie zunächst eine neue **Logik-App (Vorschau)** -Ressource erstellen. Sie können auch mit dem [Erstellen eines Projekts in Visual Studio Code mit der Erweiterung „Azure Logic Apps (Vorschau)“](create-stateful-stateless-workflows-visual-studio-code.md) beginnen. Beide Ansätze bieten Ihnen die Möglichkeit, ihre Logik-App in denselben Hostingumgebungen bereitzustellen und auszuführen.

In der Zwischenzeit können Sie weiterhin den ursprünglichen Logik-App-Typ erstellen. Trotz der Unterschiede bei der Entwicklung im Portal zwischen dem ursprünglichen und dem neuen Logik-App-Typ kann Ihr Azure-Abonnement beide Typen enthalten. Sie können alle bereitgestellten Logik-Apps in Ihrem Azure-Abonnement anzeigen und darauf zugreifen, aber die Apps werden getrennt in ihren eigenen Kategorien und Abschnitten angeordnet.

In diesem Artikel erfahren Sie, wie Sie Ihre Logik-App und einen Workflow im Azure-Portal erstellen, indem Sie den Ressourcentyp **Logik-App (Vorschau)** verwenden und diese Aufgaben auf hoher Ebene ausführen:

* Erstellen der neuen Logik-App-Ressource und Hinzufügen eines leeren Workflows.

* Hinzufügen eines Triggers und einer Aktion.

* Auslösen einer Workflowausführung.

* Anzeigen des Ausführungsverlaufs des Workflows.

* Aktivieren oder Öffnen von Application Insights nach der Bereitstellung.

* Aktivieren des Ausführungsverlaufs für zustandslose Workflows.

> [!NOTE]
> Informationen zu aktuellen bekannten Problemen finden Sie in GitHub unter [Logic Apps Public Preview Known Issues](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md) (Bekannte Probleme der öffentlichen Vorschauversion von Logic Apps).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto und ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Ein [Azure Storage-Konto](../storage/common/storage-account-overview.md), weil die **Logik-App (Vorschau)** -Ressource von Azure Functions unterstützt wird und [Speicheranforderungen aufweist, die denen von Funktions-Apps ähneln](../azure-functions/storage-considerations.md). Sie können ein vorhandenes Speicherkonto verwenden oder ein Speicherkonto im Voraus bzw. im Zuge der Erstellung der Logik-App erstellen.

  > [!NOTE]
  > [Zustandsbehaftete Logik-Apps](logic-apps-overview-preview.md#stateful-stateless) führen Speichertransaktionen wie die Verwendung von Warteschlangen zum Planen und Speichern von Workflowstatus in Tabellen und Blobs aus. Diese Transaktionen verursachen [Azure Storage-Gebühren](https://azure.microsoft.com/pricing/details/storage/). Weitere Informationen dazu, wie zustandsbehaftete Logik-Apps Daten im externen Speicher speichern, finden Sie unter [Zustandsbehaftete und zustandslose Workflows](logic-apps-overview-preview.md#stateful-stateless).

* Um dieselbe Beispiel-Logik-App in diesem Artikel zu erstellen, benötigen Sie ein Office 365 Outlook-E-Mail-Konto, das ein Geschäfts-, Schul- oder Unikonto von Microsoft für die Anmeldung verwendet.

  Wenn Sie sich entschließen, einen anderen [E-Mail-Connector zu verwenden, der von Azure Logic Apps unterstützt wird](/connectors/), z. B. Outlook.com oder [Gmail](../connectors/connectors-google-data-security-privacy-policy.md), können Sie dennoch das Beispiel durcharbeiten, und die allgemeinen übergeordneten Schritte sind identisch, doch Ihre Benutzeroberfläche und deren Optionen können sich in gewisser Weise unterscheiden. Wenn Sie beispielsweise den Outlook.com-Connector verwenden, verwenden Sie stattdessen Ihr persönliches Microsoft-Konto, um sich anzumelden.

* Zum Testen der Beispiel-Logik-App, die Sie in diesem Artikel erstellen, benötigen Sie ein Tool, das Aufrufe an den Anforderungstrigger senden kann, wobei es sich um den ersten Schritt in der Beispiel-Logik-App handelt. Falls Sie nicht über ein solches Tool verfügen, können Sie [Postman](https://www.postman.com/downloads/) herunterladen, installieren und verwenden.

* Wenn Sie Ihre Logik-App mit Einstellungen erstellen, die die Verwendung von [Application Insights](../azure-monitor/app/app-insights-overview.md) unterstützen, können Sie optional die Diagnoseprotokollierung und Ablaufverfolgung für Ihre Logik-App aktivieren. Dies ist entweder beim Erstellen der Logik-App oder nach der Bereitstellung möglich. Sie benötigen eine Application Insights-Instanz, können aber diese Ressource entweder [im Voraus](../azure-monitor/app/create-workspace-resource.md), beim Erstellen Ihrer Logik-App oder nach der Bereitstellung erstellen.

## <a name="create-the-logic-app-resource"></a>Erstellen der Logik-App-Ressource

1. Melden Sie sich mit den Anmeldeinformationen Ihres Azure-Kontos beim [Azure-Portal](https://portal.azure.com) an.

1. Geben Sie `logic app preview` in das Suchfeld des Azure-Portals ein, und wählen Sie **Logik-App (Vorschau)** aus.

   ![Screenshot, der das Suchfeld des Azure-Portals mit dem Suchtext „Logik-App Vorschau“ und der ausgewählten Ressource „Logik-App (Vorschau)“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/find-logic-app-resource-template.png)

1. Wählen Sie auf der Seite **Logik-App (Vorschau)** die Option **Hinzufügen** aus.

1. Geben Sie auf der Seite **Logik-App erstellen (Vorschau)** auf der Registerkarte **Grundlagen** diese Informationen zu Ihrer Logik-App ein.

   | Eigenschaft | Erforderlich | Wert | Beschreibung |
   |----------|----------|-------|-------------|
   | **Abonnement** | Ja | <*Name des Azure-Abonnements*> | Das für Ihre Logik-App zu verwendende Azure-Abonnement. |
   | **Ressourcengruppe** | Ja | <*Name der Azure-Ressourcengruppe*> | Die Azure-Ressourcengruppe, in der Sie Ihre Logik-App und zugehörige Ressourcen erstellen. Dieser Ressourcenname muss regionsübergreifend eindeutig sein und darf nur Buchstaben, Ziffern, Bindestriche ( **-** ), Unterstriche ( **_** ), Klammern ( **()** ) und Punkte ( **.** ) enthalten. <p><p>In diesem Beispiel wird eine Ressourcengruppe namens `Fabrikam-Workflows-RG` erstellt. |
   | **Name der Logik-App** | Ja | <*logic-app-name*> | Der für Ihre Logik-App zu verwendende Name. Dieser Ressourcenname muss regionsübergreifend eindeutig sein und darf nur Buchstaben, Ziffern, Bindestriche ( **-** ), Unterstriche ( **_** ), Klammern ( **()** ) und Punkte ( **.** ) enthalten. <p><p>In diesem Beispiel wird eine Logik-App namens `Fabrikam-Workflows` erstellt. <p><p>**Hinweis**: Der Name Ihrer Logik-App erhält automatisch das Suffix `.azurewebsites.net`, da die **Logik-App (Vorschau)** -Ressource von Azure Functions unterstützt wird, wo dieselbe App-Benennungskonvention verwendet wird. |
   | **Veröffentlichen** | Ja | <*deployment-environment*> | Das Bereitstellungsziel für Ihre Logik-App. Zur Bereitstellung in Azure können Sie **Workflow** oder einen Docker-Container auswählen. <p><p>In diesem Beispiel wird **Workflow** verwendet, die **Logik-App (Vorschau)** -Ressource in Azure. <p><p>Wenn Sie **Docker-Container** auswählen, [geben Sie den Container an, der in den Einstellungen Ihrer Logik-App verwendet werden soll](#set-docker-container). |
   | **Region** | Ja | <*Azure-Region*> | Die Azure-Region, die für die Erstellung Ihrer Ressourcengruppe und Ressourcen verwendet werden soll. <p><p>In diesem Beispiel wird **USA, Westen** verwendet. |
   |||||

   Hier sehen Sie ein Beispiel:

   ![Screenshot, der das Azure-Portal und die Seite „Logik-App erstellen (Vorschau)“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/create-logic-app-resource-portal.png)

1. Geben Sie als Nächstes diese Informationen über die Speicherlösung und den Hostingplan, die für Ihre Logik-App verwendet werden sollen, auf der Registerkarte **Hosting** an.

   | Eigenschaft | Erforderlich | Wert | Beschreibung |
   |----------|----------|-------|-------------|
   | **Speicherkonto** | Ja | <*Azure-storage-account-name*> | Das [Azure Storage-Konto](../storage/common/storage-account-overview.md), das für Speichertransaktionen verwendet werden soll. Dieser Ressourcenname muss regionsübergreifend eindeutig sein und 3-24 Zeichen enthalten (nur Ziffern und Kleinbuchstaben). Wählen Sie entweder ein vorhandenes Konto aus, oder erstellen Sie ein neues Konto. <p><p>In diesem Beispiel wird ein Speicherkonto namens `fabrikamstorageacct` erstellt. |
   | **Plantyp** | Ja | <*Azure-hosting-plan*> | Der [Hostingplan](../app-service/overview-hosting-plans.md), der für die Bereitstellung Ihrer Logik-App verwendet werden soll. Dies ist entweder ein [**Premium**](../azure-functions/functions-scale.md#premium-plan)- oder [**App Service-Plan**](../azure-functions/functions-scale.md#app-service-plan). Ihre Auswahl wirkt sich auf die Tarife aus, die Sie später auswählen können. <p><p>In diesem Beispiel wird der **App Service-Plan** verwendet. <p><p>**Hinweis**: Ähnlich wie bei Azure Functions ist für den Ressourcentyp **Logik-App (Vorschau)** ein Hostingplan und Tarif erforderlich. Hostingpläne für den Verbrauch werden weder unterstützt, noch stehen sie für diesen Ressourcentyp zur Verfügung. Weitere Informationen finden Sie in diesen Themen: <p><p>- [Skalierung und Hosting von Azure Functions](../azure-functions/functions-scale.md) <br>- [App Service-Preisdetails](https://azure.microsoft.com/pricing/details/app-service/) <p><p> |
   | **Windows-Plan** | Ja | <*plan-name*> | Der zu verwendende Planname. Wählen Sie entweder einen vorhandenen Plan aus, oder geben Sie den Namen für einen neuen Plan an. <p><p>In diesem Beispiel wird der Name `Fabrikam-Service-Plan`verwendet. |
   | **SKU und Größe** | Ja | <*pricing-tier*> | Der [Tarif](../app-service/overview-hosting-plans.md), der zum Hosting Ihrer Logik-App verwendet werden soll. Ihre Auswahl hängt von dem zuvor gewählten Plantyp ab. Wählen Sie **Größe ändern**, um den Standardtarif zu ändern. Sie können dann basierend auf der benötigten Workload andere Tarife auswählen. <p><p>In diesem Beispiel wird der kostenlose **F1-Tarif** für **Dev/Test**-Workloads verwendet. Weitere Informationen finden Sie unter [App Service-Preisdetails](https://azure.microsoft.com/pricing/details/app-service/). |
   |||||

1. Wenn die Erstellungs- und Bereitstellungseinstellungen Ihrer Logik-App die Verwendung von [Application Insights](../azure-monitor/app/app-insights-overview.md) unterstützen, können Sie nun optional die Diagnoseprotokollierung und Ablaufverfolgung für Ihre Logik-App aktivieren.

   1. Legen Sie auf der Registerkarte **Überwachung** unter **Application Insights** für **Application Insights aktivieren** **Ja** fest, wenn diese Option nicht bereits ausgewählt ist.

   1. Wählen Sie für die Einstellung **Application Insights** entweder eine vorhandene Application Insights-Instanz aus, oder **Neue erstellen**, wenn Sie eine neue Instanz erstellen möchten, und geben Sie den gewünschten Namen an.

1. Nachdem Azure die Einstellungen ihrer Logik-App überprüft hat, wählen Sie auf der Registerkarte **Überprüfen + Erstellen** die Option **Erstellen** aus.

   Beispiel:

   ![Screenshot, der das Azure-Portal und die Einstellungen der neuen Logik-App-Ressource zeigt.](./media/create-stateful-stateless-workflows-azure-portal/check-logic-app-resource-settings.png)

   Nach dem Abschluss der Bereitstellung durch Azure ist Ihre Logik-App automatisch aktiv und wird ausgeführt, erledigt aber noch keine Aufgaben, da noch keine Workflows vorhanden sind.

1. Wählen Sie auf der Seite für den Abschluss der Bereitstellung die Option **Zu Ressource wechseln** aus, damit Sie mit dem Entwickeln des Workflows beginnen können.

   ![Screenshot, der das Azure-Portal und die fertige Bereitstellung zeigt.](./media/create-stateful-stateless-workflows-azure-portal/logic-app-completed-deployment.png)

<a name="set-docker-container"></a>

## <a name="specify-docker-container-for-deployment"></a>Angeben des Docker-Containers für die Bereitstellung

Wenn Sie beim Erstellen der Logik-App **Docker-Container** ausgewählt haben, stellen Sie sicher, dass Sie Informationen zu dem Container bereitstellen, den Sie für die Bereitstellung verwenden möchten, nachdem das Azure-Portal Ihre **Logik-App (Vorschau)** -Ressource erstellt hat.

1. Navigieren Sie im Azure-Portal zu Ihrer Logik-App-Ressource.

1. Wählen Sie im Menü der Logik-App unter **Einstellungen** die Option **Containereinstellungen** aus. Geben Sie die Details und den Speicherort für das Docker-Containerimage an.

   ![Screenshot, der das Logik-App-Menü mit der Auswahl „Containereinstellungen“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/logic-app-deploy-container-settings.png)

1. Speichern Sie anschließend Ihre Einstellungen.

<a name="add-workflow"></a>

## <a name="add-a-blank-workflow"></a>Hinzufügen eines leeren Workflows

1. Nachdem Azure die Ressource geöffnet hat, wählen Sie im Menü Ihrer Logik-App **Workflows** aus. Wählen Sie auf der Symbolleiste **Workflows** die Option **Hinzufügen** aus.

   ![Screenshot, der das Menü der Logik-App-Ressource mit aktivierter Option „Workflows“ und Auswahl „Hinzufügen“ auf der Symbolleiste zeigt.](./media/create-stateful-stateless-workflows-azure-portal/logic-app-add-blank-workflow.png)

1. Geben Sie nach dem Öffnen des Bereichs **Neuer Workflow** einen Namen für den Workflow an, und wählen Sie entweder den Workflowtyp [**Zustandsbehaftet** oder **Zustandslos**](logic-apps-overview-preview.md#stateful-stateless) aus. Wählen Sie **Erstellen**, wenn Sie fertig sind.

   In diesem Beispiel wird ein leerer zustandsbehafteter Workflow namens `Fabrikam-Stateful-Workflow` hinzugefügt. Standardmäßig ist der Workflow aktiviert, führt aber erst Aufgaben aus, nachdem Sie einen Trigger und Aktionen hinzugefügt haben.

   ![Screenshot, der den neu hinzugefügten, leeren zustandsbehafteten Workflow „Fabrikam-Stateful-Workflow“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/logic-app-blank-workflow-created.png)

1. Öffnen Sie als Nächstes den leeren Workflow im Designer, damit Sie einen Trigger und eine Aktion hinzufügen können.

   1. Wählen Sie in der Workflowliste den leeren Workflow aus.

   1. Wählen Sie im Workflowmenü unter **Entwickler** die Option **Designer** aus.

      Auf der Designeroberfläche wird bereits die Eingabeaufforderung **Vorgang auswählen** angezeigt und ist standardmäßig aktiviert, sodass der Bereich **Trigger hinzufügen** ebenfalls geöffnet angezeigt wird.

      ![Screenshot mit dem geöffneten Workflow-Designer, wobei auf der Designeroberfläche „Vorgang auswählen“ ausgewählt ist.](./media/create-stateful-stateless-workflows-azure-portal/opened-logic-app-designer-blank-workflow.png)

<a name="add-trigger-actions"></a>

## <a name="add-a-trigger-and-an-action"></a>Hinzufügen eines Triggers und einer Aktion

In diesem Beispiel wird ein Workflow mit den folgenden Schritten erstellt:

* Den integrierten [Anforderungstrigger](../connectors/connectors-native-reqres.md), **Beim Empfang einer HTTP-Anforderung**, der eingehende Aufrufe oder Anforderungen empfängt und einen Endpunkt erstellt, der von anderen Diensten oder Logik-Apps aufgerufen werden kann.

* Die [Office 365 Outlook-Aktion](../connectors/connectors-create-api-office365-outlook.md): **E-Mail senden**.

* Die integrierte [Antwortaktion](../connectors/connectors-native-reqres.md), mit der Sie eine Antwort gesendet und Daten an den Aufrufer zurückgegeben haben.

### <a name="add-the-request-trigger"></a>Hinzufügen des Anforderungstriggers

Bevor Sie einem leeren Workflow einen Trigger hinzufügen können, stellen Sie sicher, dass der Workflow-Designer geöffnet und die Eingabeaufforderung **Vorgang auswählen** auf der Designeroberfläche ausgewählt ist.

1. Überprüfen Sie, ob neben der Designeroberfläche im Bereich **Trigger hinzufügen** unter dem Suchfeld **Vorgang auswählen** die Registerkarte **Integriert** ausgewählt ist. Auf dieser Registerkarte werden Trigger angezeigt, die nativ in Azure Logic Apps ausgeführt werden.

1. Geben Sie in das Suchfeld **Vorgang auswählen** die Zeichenfolge `when a http request` ein, und wählen Sie den integrierten Anforderungstrigger namens **Beim Empfang einer HTTP-Anforderung** aus.

   ![Screenshot, der den Designer und den Bereich **Trigger hinzufügen** mit ausgewähltem Trigger „Beim Empfang einer HTTP-Anforderung“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/find-request-trigger.png)

   Wenn der Trigger im Designer angezeigt wird, wird der Detailbereich des Triggers geöffnet, um die Eigenschaften, Einstellungen und anderen Aktionen des Triggers anzuzeigen.

   ![Screenshot, der den Designer mit dem ausgewählten Trigger „Beim Empfang einer HTTP-Anforderung“ und geöffnetem Triggerdetailbereich zeigt.](./media/create-stateful-stateless-workflows-azure-portal/request-trigger-added-to-designer.png)

   > [!TIP]
   > Wenn der Detailbereich nicht angezeigt wird, stellen Sie sicher, dass der Trigger im Designer ausgewählt ist.

1. Wenn Sie ein Element aus dem Designer löschen müssen, [führen Sie diese Schritte aus](#delete-from-designer).

1. Um Ihre Arbeit zu speichern, wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

   Wenn Sie einen Workflow zum ersten Mal speichern und dieser Workflow mit einem Anforderungstrigger ausgelöst wird, generiert der Logic Apps-Dienst automatisch eine URL für einen Endpunkt, der durch den Anforderungstrigger erstellt wird. Wenn Sie den Workflow später testen, senden Sie eine Anforderung an diese URL, die den Trigger auslöst und die Workflowausführung startet.

### <a name="add-the-office-365-outlook-action"></a>Hinzufügen der Office 365 Outlook-Aktion

1. Wählen Sie im Logik-App-Designer unter dem von Ihnen hinzugefügten Trigger **Neuer Schritt** aus.

   Die Eingabeaufforderung **Vorgang auswählen** wird im Designer angezeigt, und der Bereich **Aktion hinzufügen** wird erneut geöffnet, sodass Sie die nächste Aktion auswählen können.

   > [!NOTE]
   > Wenn im Bereich **Aktion hinzufügen** die Fehlermeldung „Die Eigenschaft ‚Filter‘ kann nicht von ‚undefiniert‘ gelesen werden“ angezeigt wird, speichern Sie den Workflow, laden Sie die Seite erneut, öffnen Sie den Workflow erneut, und versuchen Sie es noch einmal.

1. Wählen Sie im Bereich **Aktion hinzufügen** unter dem Suchfeld **Vorgang auswählen** die Option **Azure** aus. Auf dieser Registerkarte werden die verwalteten Connectors angezeigt, die in Azure verfügbar sind und bereitgestellt werden.

   > [!NOTE]
   > Wenn im Bereich **Aktion hinzufügen** die Fehlermeldung `The access token expiry UTC time '{token-expiration-date-time}' is earlier than current UTC time '{current-date-time}'` angezeigt wird, speichern Sie den Workflow, laden Sie die Seite erneut, öffnen Sie den Workflow erneut, und versuchen Sie noch einmal, die Aktion hinzuzufügen.

   In diesem Beispiel wird die Office 365 Outlook-Aktion **E-Mail senden (V2)** verwendet.

   ![Screenshot, der den Designer und den Bereich **Aktion hinzufügen** mit ausgewählter Office 365 Outlook-Aktion „E-Mail senden“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/find-send-email-action.png)

1. Wählen Sie im Detailbereich der Aktion auf der Registerkarte **Verbindung erstellen** die Option **Anmelden** aus, damit Sie eine Verbindung mit Ihrem E-Mail-Konto herstellen können.

   ![Screenshot, der den Designer und den Detailbereich „E-Mail senden (V2)“ mit ausgewähltem „Anmelden“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/send-email-action-sign-in.png)

1. Wenn Sie zur Zustimmung zum Zugriff auf Ihr E-Mail-Konto aufgefordert werden, melden Sie sich mit Ihren Kontoanmeldeinformationen an.

   > [!NOTE]
   > Wenn Sie die Fehlermeldung `Failed with error: 'The browser is closed.'. Please sign in again` erhalten, überprüfen Sie, ob der Browser Cookies von Drittanbietern blockiert. Wenn diese Cookies blockiert werden, versuchen Sie, `https://portal.azure.com` der Liste der Websites hinzuzufügen, die Cookies verwenden können. Wenn Sie den Inkognitomodus verwenden, stellen Sie sicher, dass Cookies von Drittanbietern nicht blockiert werden, wenn Sie in diesem Modus arbeiten.
   > 
   > Laden Sie ggf. die Seite neu, öffnen Sie den Workflow, fügen Sie die E-Mail-Aktion erneut hinzu, und versuchen Sie, die Verbindung herzustellen.

   Nachdem Azure die Verbindung erstellt hat, wird die Aktion **E-Mail senden** auf der Designeroberfläche angezeigt und ist standardmäßig ausgewählt. Wenn die Aktion nicht ausgewählt ist, wählen Sie die Aktion aus, sodass der Detailbereich ebenfalls geöffnet ist.

1. Geben Sie im Detailbereich der Aktion auf der Registerkarte **Parameter** die erforderlichen Informationen für die Aktion an, z. B.:

   ![Screenshot, der den Designer und den Detailbereich „E-Mail senden“ mit ausgewählter Registerkarte „Parameter“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/send-email-action-details.png)

   | Eigenschaft | Erforderlich | Wert | BESCHREIBUNG |
   |----------|----------|-------|-------------|
   | **An** | Ja | <*Ihre E-Mail-Adresse*> | Der E-Mail-Empfänger, bei dem es sich um Ihre E-Mail-Adresse zu Testzwecken handeln kann. In diesem Beispiel wird die fiktive E-Mail-Adresse `sophiaowen@fabrikam.com` verwendet. |
   | **Subject** | Ja | `An email from your example workflow` | der E-Mail-Betreff |
   | **Text** | Ja | `Hello from your example workflow!` | Der Textinhalt der E-Mail |
   ||||

   > [!NOTE]
   > Wenn Sie Änderungen im Detailbereich auf den Registerkarten **Einstellungen**, **Statisches Ergebnis** oder **Ausführen nach** vornehmen, stellen Sie sicher, dass Sie **Fertig** ausgewählt haben, um die Änderungen zu übernehmen, bevor Sie zwischen Registerkarten wechseln oder den Fokus innerhalb des Designers verschieben. Andernfalls behält der Designer die Änderungen nicht bei.

1. Speichern Sie Ihre Arbeit. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

Um den Workflow zu testen, lösen Sie als Nächstes eine Ausführung manuell aus.

## <a name="trigger-the-workflow"></a>Auslösen des Workflows

In diesem Beispiel wird der Workflow ausgeführt, wenn der Anforderungstrigger eine eingehende Anforderung empfängt, die an die URL für den Endpunkt gesendet wird, der vom Trigger erstellt wird. Als Sie den Workflow zum ersten Mal gespeichert haben, hat der Logic Apps-Dienst diese URL automatisch generiert. Bevor Sie diese Anforderung zum Auslösen des Workflows senden können, müssen Sie diese URL suchen.

1. Wählen Sie im Workflow-Designer den Anforderungstrigger **Beim Empfang einer HTTP-Anforderung** aus.

1. Nach Öffnen des Detailbereichs suchen Sie auf der Registerkarte **Parameter** die **HTTP-POST-URL**-Eigenschaft. Um die generierte URL zu kopieren, wählen Sie **URL kopieren** aus („Datei kopieren“-Symbol), und speichern Sie die URL vorerst an einem anderen Ort. Die URL weist das folgende Format auf:

   `http://<logic-app-name>.azurewebsites.net:443/api/<workflow-name>/triggers/manual/invoke?api-version=2020-05-01-preview&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<shared-access-signature>`

   ![Screenshot, der den Designer mit Anforderungstrigger und Endpunkt-URL in der Eigenschaft „HTTP-POST-URL“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/find-request-trigger-url.png)

   Für dieses Beispiel sieht die URL wie folgt aus:

   `https://fabrikam-workflows.azurewebsites.net:443/api/Fabrikam-Stateful-Workflow/triggers/manual/invoke?api-version=2020-05-01-preview&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=xxxxxXXXXxxxxxXXXXxxxXXXXxxxxXXXX`

   > [!TIP]
   > Sie finden die Endpunkt-URL auch im Bereich **Übersicht** Ihrer Logik-App in der **Workflow-URL**-Eigenschaft.
   >
   > 1. Wählen Sie im Ressourcenmenü **Übersicht** aus.
   > 1. Suchen Sie im Bereich **Übersicht** nach der Eigenschaft **Workflow-URL**.
   > 1. Um die Endpunkt-URL zu kopieren, bewegen Sie den Mauszeiger über das Ende des Endpunkt-URL-Texts, und wählen Sie **In Zwischenablage kopieren** aus („Datei kopieren“-Symbol).

1. Um die URL durch Senden einer Anforderung zu testen, öffnen Sie [Postman](https://www.postman.com/downloads/) oder Ihr bevorzugtes Tool zum Erstellen und Senden von Anforderungen.

   Dieses Beispiel wird mit Postman fortgesetzt. Weitere Informationen finden Sie unter [Erste Schritte mit Postman](https://learning.postman.com/docs/getting-started/introduction/).

   1. Wählen Sie auf der Postman-Symbolleiste **Neu** aus.

      ![Screenshot, der Postman mit ausgewählter Schaltfläche „Neu“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/postman-create-request.png)

   1. Wählen Sie im Bereich **Neu erstellen** unter **Bausteine** die Option **Anforderung** aus.

   1. Geben Sie im Fenster **Anforderung speichern** unter **Anforderungsname** einen Namen für die Anforderung an, z. B. `Test workflow trigger`.

   1. Wählen Sie unter **Sammlung oder Ordner als Speicherziel auswählen** den Eintrag **Sammlung erstellen** aus.

   1. Geben Sie unter **Alle Sammlungen** einen Namen für die zu erstellende Sammlung an, um Ihre Anforderungen zu organisieren, und wählen Sie **Speichern unter <*Sammlungs-Name*>** aus. In diesem Beispiel wird `Logic Apps requests` als Name der Sammlung verwendet.

      Der Anforderungsbereich von Postman wird geöffnet, sodass Sie eine Anforderung an die Endpunkt-URL für den Anforderungstrigger senden können.

      ![Screenshot, der Postman mit geöffnetem Anforderungsbereich zeigt.](./media/create-stateful-stateless-workflows-azure-portal/postman-request-pane.png)

   1. Fügen Sie im Anforderungsbereich in das Adressfeld neben der Liste der Methoden, wo derzeit **GET** als Standardanforderungsmethode angezeigt wird, die zuvor kopierte URL ein, und wählen Sie **Senden** aus.

      ![Screenshot, der Postman und die Endpunkt-URL im Adressfeld mit ausgewählter Schaltfläche „Senden“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/postman-test-endpoint-url.png)

      Wenn der Trigger ausgelöst wird, wird der Beispielworkflow ausgeführt und sendet eine E-Mail, die in etwa dem folgenden Beispiel entspricht:

      ![Screenshot, der die wie im Beispiel beschriebene Outlook-E-Mail zeigt.](./media/create-stateful-stateless-workflows-azure-portal/workflow-app-result-email.png)

## <a name="review-run-history"></a>Überprüfen des Ausführungsverlaufs

Bei einem zustandsbehafteten Workflow können Sie nach jeder Workflowausführung den Ausführungsverlauf anzeigen, einschließlich des Status für die gesamte Ausführung, den Trigger und jede Aktion sowie die zugehörigen Ein- und Ausgaben.

1. Wählen Sie im Azure-Portal im Menü des Workflows **Überwachen** aus.

   Der Bereich **Überwachen** zeigt den Ausführungsverlauf für diesen Workflow an.

   ![Screenshot, der den Bereich „Überwachen“ des Workflows und den Ausführungsverlauf zeigt.](./media/create-stateful-stateless-workflows-azure-portal/find-run-history.png)

   > [!TIP]
   > Wenn der letzte Ausführungsstatus nicht angezeigt wird, wählen Sie auf der Bereichssymbolleiste **Überwachen** die Option **Aktualisieren** aus. Für Trigger, die aufgrund von nicht erfüllten Kriterien oder nicht gefundenen Daten ausgelassen werden, erfolgt keine Ausführung.

   | Ausführungsstatus | Beschreibung |
   |------------|-------------|
   | **Aborted** | Die Ausführung wurde aufgrund externer Probleme beendet oder nicht fertig gestellt, z. B. wegen eines Systemausfalls oder abgelaufenen Azure-Abonnements. |
   | **Abgebrochen** | Die Ausführung wurde ausgelöst und gestartet, es wurde jedoch eine Abbruchanforderung empfangen. |
   | **Fehler** | Mindestens eine Aktion in der Ausführung ist fehlgeschlagen. Es wurden keine nachfolgenden Aktionen im Workflow eingerichtet, um den Fehler zu verarbeiten. |
   | **Wird ausgeführt** | Die Ausführung wurde ausgelöst und wird gerade ausgeführt. Dieser Status kann jedoch auch für eine Ausführung angezeigt werden, die aufgrund von [Aktionslimits](logic-apps-limits-and-config.md) oder des [aktuellen Tarifs](https://azure.microsoft.com/pricing/details/logic-apps/) gedrosselt wird. <p><p>**Tipp**: Wenn Sie die [Diagnoseprotokollierung](monitor-logic-apps-log-analytics.md) einrichten, erhalten Sie Informationen zu ggf. aufgetretenen Drosselungsereignissen. |
   | **Erfolgreich** | Die Ausführung war erfolgreich. Wenn eine Aktion fehlgeschlagen ist, wurde dieser Fehler von einer nachfolgenden Aktion im Workflow verarbeitet. |
   | **Timeout** | Bei der Ausführung ist ein Timeout aufgetreten, weil die aktuelle Dauer die maximale Dauer von Ausführungen überschritten hat, die von der [Einstellung **Aufbewahrung des Ausführungsverlaufs in Tagen**](logic-apps-limits-and-config.md#run-duration-retention-limits) gesteuert wird. Die Dauer der Ausführung wird anhand der Startzeit der Ausführung und der maximalen Dauer von Ausführungen zu dieser Startzeit berechnet. <p><p>**Hinweis**: Wenn die Ausführungsdauer auch das aktuelle *Aufbewahrungslimit im Ausführungsverlauf* überschreitet, das ebenfalls von der [Einstellung **Aufbewahrung des Ausführungsverlaufs in Tagen**](logic-apps-limits-and-config.md#run-duration-retention-limits) gesteuert wird, wird die Ausführung durch einen täglichen Cleanupauftrag aus dem Ausführungsverlauf gelöscht. Unabhängig davon, ob bei der Ausführung ein Timeout auftritt oder ob diese fertig gestellt wird, wird die Aufbewahrungsdauer immer anhand der Startzeit der Ausführung und der *aktuellen* Aufbewahrungsdauer berechnet. Wenn Sie also die maximale Dauer einer aktiven Ausführung herabsetzen, tritt ein Timeout für die Ausführung auf. Je nachdem, ob die Ausführungsdauer die Aufbewahrungsdauer überschreitet, bleibt die Ausführung entweder unverändert oder wird aus dem Ausführungsverlauf gelöscht. |
   | **Wartet** | Die Ausführung wurde nicht gestartet oder wurde angehalten, z. B. aufgrund einer früheren Workflowinstanz, die noch ausgeführt wird. |
   |||

1. Um den Status der einzelnen Schritte in einer Ausführung zu überprüfen, wählen Sie die Ausführung aus, die Sie überprüfen möchten.

   Die Ausführungsdetailsansicht wird geöffnet und zeigt den Status der einzelnen Schritte in der Ausführung an.

   ![Screenshot, der die Ausführungsdetailsansicht mit dem Status für jeden Schritt im Workflow zeigt.](./media/create-stateful-stateless-workflows-azure-portal/review-run-details.png)

   In der folgenden Tabelle sind die möglichen Statuswerte aufgeführt, die in den einzelnen Workflowschritte aufweisen können:

   | Status einer Aktion | Symbol | BESCHREIBUNG |
   |---------------|------|-------------|
   | Aborted | ![Symbol für den Aktionsstatus „Abgebrochen“][aborted-icon] | Die Aktion wurde aufgrund externer Probleme beendet oder nicht fertig gestellt, z. B. wegen eines Systemausfalls oder abgelaufenen Azure-Abonnements. |
   | Abgebrochen | ![Symbol für den Aktionsstatus „Storniert“][cancelled-icon] | Die Aktion wurde ausgeführt, hat aber eine Abbruchanforderung erhalten. |
   | Fehler | ![Symbol für den Aktionsstatus „Fehlerhaft“][failed-icon] | Die Aktion ist fehlgeschlagen. |
   | Wird ausgeführt | ![Symbol für den Aktionsstatus „Wird ausgeführt“][running-icon] | Die Aktion wird zurzeit ausgeführt. |
   | Ausgelassen | ![Symbol für den Aktionsstatus „Übersprungen“][skipped-icon] | Die Aktion wurde übersprungen, weil die unmittelbar vorhergehende Aktion fehlgeschlagen ist. Eine Aktion weist eine `runAfter`-Bedingung auf, die erfordert, dass die vorherige Aktion erfolgreich abgeschlossen wurde, bevor die aktuelle Aktion ausgeführt werden kann. |
   | Erfolgreich | ![Symbol für den Aktionsstatus „Erfolgreich“][succeeded-icon] | Die Aktion war erfolgreich. |
   | Erfolgreich mit Wiederholungen | ![Symbol für den Aktionsstatus „Erfolgreich mit Wiederholungen“][succeeded-with-retries-icon] | Die Aktion war erfolgreich, jedoch erst nach mindestens einem Wiederholungsversuch. Wählen Sie diese Aktion zum Überprüfen des Wiederholungsverlaufs in der Detailansicht des Ausführungsverlaufs aus, damit Sie die Ein- und Ausgaben anzeigen können. |
   | Timeout | ![Symbol für Aktionsstatus „Timeout“][timed-out-icon] | Die Aktion wurde aufgrund des Timeoutlimits beendet, das durch die Einstellungen dieser Aktion gesteuert wird. |
   | Warten | ![Symbol für den Aktionsstatus „Wartend“][waiting-icon] | Gilt für eine Webhookaktion, die auf eine eingehende Anforderung eines Aufrufers wartet. |
   ||||

   [aborted-icon]: ./media/create-stateful-stateless-workflows-azure-portal/aborted.png
   [cancelled-icon]: ./media/create-stateful-stateless-workflows-azure-portal/cancelled.png
   [failed-icon]: ./media/create-stateful-stateless-workflows-azure-portal/failed.png
   [running-icon]: ./media/create-stateful-stateless-workflows-azure-portal/running.png
   [skipped-icon]: ./media/create-stateful-stateless-workflows-azure-portal/skipped.png
   [succeeded-icon]: ./media/create-stateful-stateless-workflows-azure-portal/succeeded.png
   [succeeded-with-retries-icon]: ./media/create-stateful-stateless-workflows-azure-portal/succeeded-with-retries.png
   [timed-out-icon]: ./media/create-stateful-stateless-workflows-azure-portal/timed-out.png
   [waiting-icon]: ./media/create-stateful-stateless-workflows-azure-portal/waiting.png

1. Um die Ein- und Ausgaben für einen bestimmten Schritt zu überprüfen, wählen Sie diesen Schritt aus.

   ![Screenshot, der die Ein- und Ausgaben in der ausgewählten Aktion „E-Mail senden“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/review-step-inputs-outputs.png)

1. Um die unformatierten Ein- und Ausgaben für diesen Schritt weiter zu überprüfen, wählen Sie **Unformatierte Eingaben anzeigen** oder **Unformatierte Ausgaben anzeigen** aus.

<a name="enable-open-application-insights"></a>

## <a name="enable-or-open-application-insights-after-deployment"></a>Aktivieren oder Öffnen von Application Insights nach der Bereitstellung

Während der Workflowausführung gibt Ihre Logik-App Telemetriedaten zusammen mit anderen Ereignissen aus. Sie können diese Telemetrie verwenden, um auf verschiedene Weise einen besseren Einblick in die Qualität der Workflowausführung und die Arbeitsweise der Logic Apps-Runtime zu bekommen. Sie können den Workflow überwachen, indem Sie [Application Insights](../azure-monitor/app/app-insights-overview.md) verwenden, wodurch nahezu in Echtzeit Telemetriedaten (Livemetriken) bereitgestellt werden. Mithilfe dieser Funktion können Sie Fehler und Leistungsprobleme leichter untersuchen, wenn Sie diese Daten zum Diagnostizieren von Problemen, Einrichten von Warnungen und Erstellen von Diagrammen verwenden.

Wenn die Erstellungs- und Bereitstellungseinstellungen Ihrer Logik-App die Verwendung von [Application Insights](../azure-monitor/app/app-insights-overview.md) unterstützen, können Sie optional die Diagnoseprotokollierung und Ablaufverfolgung für Ihre Logik-App aktivieren. Dies ist entweder beim Erstellen der Logik-App oder nach der Bereitstellung möglich. Sie benötigen eine Application Insights-Instanz, können aber diese Ressource entweder [im Voraus](../azure-monitor/app/create-workspace-resource.md), beim Erstellen Ihrer Logik-App oder nach der Bereitstellung erstellen.

Gehen Sie folgendermaßen vor, um Application Insights auf einer bereitgestellten Logik-App zu aktivieren oder das Application Insights-Dashboard zu öffnen, wenn Application Insights bereits aktiviert ist:

1. Suchen Sie im Azure-Portal nach Ihrer bereitgestellten Logik-App.

1. Wählen Sie im Menü der Logik-App unter **Einstellungen** die Option **Application Insights** aus.

1. Wenn Application Insights nicht aktiviert ist, wählen Sie im Bereich **Application Insights** die Option **Application Insights aktivieren** aus. Nachdem der Bereich aktualisiert wurde, wählen Sie unten **Anwenden** aus.

   Wenn Application Insights aktiviert ist, wählen Sie im Bereich **Application Insights** die Option **Application Insights-Daten anzeigen** aus.

Nachdem Application Insights geöffnet wurde, können Sie verschiedene Metriken für Ihre Logik-App überprüfen.

<a name="enable-run-history-stateless"></a>

## <a name="enable-run-history-for-stateless-workflows"></a>Aktivieren des Ausführungsverlaufs für zustandslose Workflows

Damit Sie einen zustandslosen Workflow debuggen können, können Sie den Ausführungsverlauf für diesen Workflow aktivieren und wieder deaktivieren, wenn Sie fertig sind. Führen Sie diese Schritte für das Azure-Portal aus. Wenn Sie in Visual Studio Code arbeiten, lesen Sie [Erstellen zustandsbehafteter und zustandsloser Workflows in Visual Studio Code](create-stateful-stateless-workflows-visual-studio-code.md#enable-run-history-stateless).

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) Ihre **Logik-App (Vorschau)** -Ressource, und öffnen Sie sie.

1. Wählen Sie im Menü der Logik-App unter **Einstellungen** die Option **Konfiguration** aus.

1. Wählen Sie auf der Registerkarte **Anwendungseinstellungen** die Option **Neue Anwendungseinstellung** aus.

1. Geben Sie im Bereich **Anwendungseinstellung hinzufügen/bearbeiten** in das Feld **Name** den Namen dieser Vorgangsoption ein: 

   `Workflows.{yourWorkflowName}.OperationOptions`

1. Geben Sie in dass Feld **Wert** den folgenden Wert ein: `WithStatelessRunHistory`

   Beispiel:

   ![Screenshot, der das Azure-Portal und die Logik-App-Ressource (Vorschau) mit dem geöffneten Bereich „Konfiguration“ > „Neue Anwendungseinstellung“ < „Anwendungseinstellung hinzufügen/bearbeiten“ und der auf „WithStatelessRunHistory“ festgelegten Option „Workflow.{NameIhresWorkflows}.OperationOptions“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/stateless-operation-options-run-history.png)

1. Wählen Sie **OK** aus, um diese Aufgabe abzuschließen. Wählen Sie auf der Symbolleiste des Bereichs **Konfiguration** die Option **Speichern** aus.

1. Um den Ausführungsverlauf zu deaktivieren, wenn Sie fertig sind, legen Sie entweder die Eigenschaft `Workflows.{yourWorkflowName}.OperationOptions` auf `None` fest oder löschen die Eigenschaft und ihren Wert.

<a name="delete-from-designer"></a>

## <a name="delete-items-from-the-designer"></a>Löschen von Elementen aus dem Designer

Führen Sie einen der folgenden Schritte aus, um ein Element in Ihrem Workflow aus dem Designer zu löschen:

* Wählen Sie das Element aus, öffnen Sie das Kontextmenü des Elements (UMSCHALT+F10), und wählen Sie **Löschen** aus. Klicken Sie auf **OK**, um dies zu bestätigen.

* Wählen Sie das Element aus, und drücken Sie dann die ENTF-Taste. Klicken Sie auf **OK**, um dies zu bestätigen.

* Wählen Sie das Element aus, sodass der Detailbereich für dieses Element geöffnet wird. Öffnen Sie in der oberen rechten Ecke des Bereichs das Menü mit den Auslassungspunkten ( **...** ), und wählen Sie **Löschen** aus. Klicken Sie auf **OK**, um dies zu bestätigen.

  ![Screenshot, der ein ausgewähltes Element im Designer mit geöffnetem Detailbereich sowie die ausgewählte Schaltfläche mit Auslassungspunkten und den Befehl „Löschen“ zeigt.](./media/create-stateful-stateless-workflows-azure-portal/delete-item-from-designer.png)

  > [!TIP]
  > Wenn das Menü mit den Auslassungspunkten nicht sichtbar ist, erweitern Sie das Fenster Ihres Browsers, sodass im Detailbereich die Schaltfläche mit den Auslassungspunkten ( **...** ) in der oberen rechten Ecke angezeigt wird.

<a name="troubleshoot"></a>

## <a name="troubleshoot-problems-and-errors"></a>Beheben von Problemen und Fehlern

<a name="missing-triggers-actions"></a>

### <a name="new-triggers-and-actions-are-missing-from-the-designer-picker-for-previously-created-workflows"></a>In der Designerauswahl für zuvor erstellte Workflows fehlen neue Trigger und Aktionen

„Azure Logic Apps (Vorschau)“ unterstützt integrierte Aktionen für Azure-Funktionsvorgänge, Liquid-Vorgänge und XML-Vorgänge wie **XML-Validierung** und **Transformieren von XML**. Allerdings werden diese Aktionen für zuvor erstellte Logik-Apps möglicherweise nicht für Sie zur Auswahl im Designer angezeigt, wenn Ihre Logik-App eine veraltete Version des Erweiterungspakets `Microsoft.Azure.Functions.ExtensionBundle.Workflows` verwendet.

Um dieses Problem zu beheben, führen Sie die folgenden Schritte aus, um die veraltete Version zu löschen, damit das Erweiterungspaket automatisch auf die neueste Version aktualisiert werden kann.

> [!NOTE]
> Diese spezifische Lösung gilt nur für **Logik-App (Vorschau)** -Ressourcen, die Sie im Azure-Portal erstellen, nicht die Logik-Apps, die Sie mit Visual Studio Code und der Erweiterung „Azure Logic Apps (Vorschau)“ erstellen und bereitstellen. Siehe [Neue Trigger und Aktionen fehlen in der Designerauswahl für früher erstellte Workflows](create-stateful-stateless-workflows-visual-studio-code.md#missing-triggers-actions).

1. Beenden Sie im Azure-Portal Ihre Logik-App.

   1. Wählen Sie in Ihrem Logik-App-Menü **Übersicht** aus.

   1. Wählen Sie in der Symbolleiste des Bereichs **Übersicht** die Option **Beenden** aus.

1. Wählen Sie im Menü Ihrer Logik-App unter **Entwicklungstools** die Option **Erweiterte Tools** aus.

1. Wählen Sie im Bereich **Erweiterte Tools** die Option **Los** aus, wodurch die Kudu-Umgebung für Ihre Logik-App geöffnet wird.

1. Öffnen Sie auf der Kudu-Symbolleiste das Menü **Debugkonsole**, und wählen Sie **CMD** aus. 

   Ein Konsolenfenster wird geöffnet, in dem Sie mithilfe der Eingabeaufforderung zum Paketordner navigieren können. Alternativ können Sie die Verzeichnisstruktur durchsuchen, die im Konsolenfenster angezeigt wird.

1. Navigieren Sie zum folgenden Ordner, der Ordner mit Versionsangaben für das vorhandene Paket enthält:

   `...\home\data\Functions\ExtensionBundles\Microsoft.Azure.Functions.ExtensionBundle.Workflows`

1. Löschen Sie den Versionsordner für das vorhandene Paket. Im Konsolenfenster können Sie diesen Befehl ausführen, wo Sie `{bundle-version}` durch die vorhandene Version ersetzen:

   `rm -rf {bundle-version}`

   Beispiel: `rm -rf 1.1.3`

   > [!TIP]
   > Wenn Sie eine Fehlermeldung wie „Berechtigung verweigert“ oder „Datei wird verwendet“ erhalten, aktualisieren Sie die Seite in Ihrem Browser, und wiederholen Sie die vorherigen Schritte, bis der Ordner gelöscht wird.

1. Kehren Sie im Azure-Portal zur Seite **Übersicht** der Logik-App zurück, und wählen Sie **Neu starten** aus.

   Im Portal wird automatisch das neueste Paket abgerufen und verwendet.

## <a name="next-steps"></a>Nächste Schritte

Bitte teilen Sie uns Ihre Erfahrungen mit dieser öffentlichen Vorschauversion mit!

* Bei Fehlern oder Problemen [erstellen Sie Ihre Probleme in GitHub](https://github.com/Azure/logicapps/issues).
* Bei Fragen, Anforderungen, Kommentaren und anderem Feedback [verwenden Sie dieses Feedbackformular](https://aka.ms/lafeedback).
