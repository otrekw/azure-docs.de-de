---
title: Herstellen einer Verbindung mit Common Data Service (Microsoft Dataverse)
description: Erstellen und Verwalten von Common Data Service-Datensätzen (Microsoft Dataverse) mithilfe von Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: jdaly, logicappspm
ms.topic: conceptual
ms.date: 02/11/2021
tags: connectors
ms.openlocfilehash: bec3416195358121b85eb61679ab39647e664a9e
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100382349"
---
# <a name="create-and-manage-records-in-common-data-service-microsoft-dataverse-by-using-azure-logic-apps"></a>Erstellen und Verwalten von Datensätzen in Common Data Service (Microsoft Dataverse) mithilfe von Azure Logic Apps

> [!NOTE]
> Im November 2020 wurde Common Data Service in Microsoft Dataverse umbenannt.

Mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) und dem [Common Data Service-Connector](/connectors/commondataservice/) können Sie automatisierte Workflows erstellen, die Datensätze in Ihrer Datenbank in [Common Data Service (nun Microsoft Dataverse)](/powerapps/maker/common-data-service/data-platform-intro) verwalten. Diese Workflows können Datensätze erstellen und aktualisieren sowie andere Vorgänge ausführen. Sie können auch Informationen aus Ihrer Dataverse-Datenbank abrufen und die Ausgabe für andere Aktionen zur Verwendung in Ihrer Logik-App bereitstellen. Wenn ein Datensatz z. B. in der Dataverse-Datenbank aktualisiert wird, können Sie mithilfe des Outlook-Connectors von Office 365 eine E-Mail senden.

In diesem Artikel erfahren Sie, wie Sie eine Logik-App erstellen, die einen Aufgabendatensatz erstellt, sobald ein neuer Leaddatensatz erstellt wird.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Eine [Common Data Service-Umgebung](/power-platform/admin/environments-overview), bei der es sich um einen Bereich handelt, in dem Ihre Organisation Geschäftsdaten und eine Common Data Service-Datenbank speichert, verwaltet und freigibt. Weitere Informationen finden Sie in den folgenden Ressourcen:<p>

  * [Learn: Erste Schritte mit Common Data Service](/learn/modules/get-started-with-powerapps-common-data-service/)
  * [Übersicht über Power Platform-Umgebungen](/power-platform/admin/environments-overview)

* Grundlegende Informationen zum [Erstellen von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md) und zu der Logik-App, von der aus Sie auf die Datensätze in der Dataverse-Datenbank zugreifen möchten. Um Ihre Logik-App mit einem Common Data Service-Trigger starten zu können, benötigen Sie eine leere Logik-App. Falls Sie mit Azure Logic Apps noch nicht vertraut sind, lesen Sie zunächst das Dokument [Schnellstart: Erstellen Ihres ersten Workflows mithilfe von Azure Logic Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-common-data-service-trigger"></a>Hinzufügen eines Common Data Service-Triggers

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

Fügen Sie für dieses Beispiel den Common Data Service-Trigger hinzu, der ausgelöst wird, wenn ein neuer Datensatz erstellt wird.

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) Ihre leere Logik-App im Logik-App-Designer, sofern sie nicht bereits geöffnet ist.

1. Geben Sie im Suchfeld `common data service`ein. Wählen Sie für dieses Beispiel in der Triggerliste den folgenden Trigger aus: **When a record is created** (Wenn ein Datensatz erstellt wird)

   ![Auswählen des Triggers „When a record is created“ (Wenn ein Datensatz erstellt wird)](./media/connect-common-data-service/select-when-record-created-trigger.png)

1. Wenn Sie dazu aufgefordert werden, melden Sie sich bei Common Data Service an.

1. Geben Sie im Trigger Informationen zu der Umgebung an, in der Sie neue Datensätze vom Typ „Lead“ überwachen möchten, beispielsweise:

   ![Zu überwachende Triggerinformationen für die Umgebung](./media/connect-common-data-service/when-record-created-trigger-details.png)

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Umgebung** | Ja | Die zu überwachende Umgebung, z. B. „Fabrikam Sales Production“. Weitere Informationen finden Sie in der [Power Platform-Umgebungsübersicht](/power-platform/admin/environments-overview). |
   | **Name der Entität** | Ja | Die zu überwachende Entität, z. B. „Leads“. |
   | **Umfang** | Ja | Die Quelle, die den neuen Datensatz erstellt hat, z. B. ein Benutzer in ihrer Geschäftseinheit oder ein beliebiger Benutzer in Ihrer Organisation. In diesem Beispiel wird „Business Unit“ verwendet. |
   ||||

## <a name="add-common-data-service-action"></a>Hinzufügen einer Common Data Service-Aktion

Fügen Sie nun eine Common Data Service-Aktion hinzu, mit der ein Aufgabendatensatz für einen neuen Datensatz „Leads“ erstellt wird.

1. Wählen Sie unter dem Trigger **When a record is created** (Wenn ein Datensatz erstellt wird) die Option **New step** (Neuer Schritt) aus.

1. Geben Sie im Suchfeld `common data service`ein. Wählen Sie in der Liste mit den Aktionen diese Aktion aus: **Create a new record** (Neuen Datensatz erstellen)

   ![Auswählen der Aktion „Create a new record“ (Neuen Datensatz erstellen)](./media/connect-common-data-service/select-create-new-record-action.png)

1. Geben Sie in der Aktion Informationen zu der Umgebung an, in der Sie den neuen Aufgabendatensatz erstellen möchten. Falls verfügbar, werden auch andere Eigenschaften auf Grundlage der Entität angezeigt, die Sie für diese Aktion ausgewählt haben, beispielsweise:

   ![Aktionsinformationen für die Umgebung, in der der Datensatz erstellt werden soll](./media/connect-common-data-service/create-new-record-action-details.png)

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Name der Organisation** | Ja | Die Umgebung, in der Sie den Datensatz erstellen möchten, die nicht die gleiche Umgebung in Ihrem Trigger sein muss, aber in diesem Beispiel „Fabrikam Sales Production“ ist. |
   | **Name der Entität** | Ja | Die Entität, in der Sie den Datensatz erstellen möchten, z.B. „Aufgaben“ |
   | **Subject** | Ja, basierend auf der in diesem Beispiel ausgewählten Entität | Eine kurze Beschreibung des Ziels für diese Aufgabe |
   ||||

   1. Geben Sie für die Eigenschaft **Subject** (Betreff) den folgenden Text mit einem nachfolgenden Leerzeichen ein:

      `Follow up with new lead:`

   1. Belassen Sie den Mauszeiger im Feld **Subject** (Betreff), damit die Liste dynamischer Inhalte sichtbar bleibt.
   
   1. Wählen Sie in der Liste im Abschnitt **When a record is created** (Wenn ein Datensatz erstellt wird) die Triggerausgaben aus, die Sie in den Aufgabendatensatz einschließen möchten, beispielsweise:

      ![Auswählen der im Aufgabendatensatz zu verwendenden Triggerausgaben](./media/connect-common-data-service/create-new-record-action-select-trigger-outputs.png)

      | Triggerausgabe | BESCHREIBUNG |
      |----------------|-------------|
      | **Vorname** | Der Vorname des Leaddatensatzes, der als primärer Kontakt im Aufgabendatensatz verwendet werden soll. |
      | **Last Name** | Der Nachname aus dem Leaddatensatz, der als primärer Kontakt im Aufgabendatensatz verwendet werden soll. |
      | **Beschreibung** | Andere Ausgaben, die in den Aufgabendatensatz eingeschlossen werden sollen, z. B. E-Mail-Adresse und geschäftliche Telefonnummer. |
      |||

   Wenn Sie fertig sind, sieht die Aktion möglicherweise wie das folgende Beispiel aus:

   ![Fertiggestellte Aktion „Create a new record“ (Neuen Datensatz erstellen)](./media/connect-common-data-service/finished-create-record-action-details.png)

1. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

1. Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen** aus, um Ihre Logik-App manuell zu starten. Erstellen Sie einen neuen „Leads“datensatz, um Ihre Logik-App zu testen.

## <a name="trigger-only-on-updated-attributes"></a>Nur bei aktualisierten Attributen auslösen

Für Trigger, die ausgeführt werden, wenn Datensätze aktualisiert werden, z. B. die Aktion **When a record is updated** (Wenn ein Datensatz aktualisiert wird), können Sie Filterattribute verwenden, damit Ihre Logik-App nur ausgeführt wird, wenn die angegebenen Attribute aktualisiert werden. Diese Funktion hilft Ihnen, unnötige Ausführungen der Logik-App zu vermeiden.

1. Wählen Sie im Trigger aus der Liste **Add new parameter** (Neuen Parameter hinzufügen) **Attribute Filters** (Attributfilter) aus.

   ![Screenshot: Aktion „Wenn ein Datensatz aktualisiert wird“ und die geöffnete Liste „Add new parameter“ (Neuen Parameter hinzufügen) mit der ausgewählten Eigenschaft „Attribute Filters“ (Attributfilter)](./media/connect-common-data-service/when-record-updated-trigger-add-attribute-filters.png)

1. Wählen Sie für jedes **Attribute Filters Item** (Attributfilterelement) das Attribut aus, das Sie auf Aktualisierungen überwachen möchten, beispielsweise:

   ![Hinzufügen der Eigenschaft „Attribute Filters“ (Attributfilter)](./media/connect-common-data-service/when-record-updated-trigger-select-attribute-filter.png)

## <a name="list-records-based-on-a-filter"></a>Auflisten von Datensätzen auf Grundlage eines Filters

Für Aktionen, die Datensätze zurückgeben, z. B. die Aktion **List records** (Datensätze auflisten), können Sie eine ODATA-Abfrage verwenden, die Datensätze auf Grundlage des angegebenen Filters zurückgibt. Beispielsweise können Sie mit der Aktion nur die Datensätze für aktive Konten auflisten.

1. Öffnen Sie die Liste **Add new parameter** (Neuen Parameter hinzufügen) in der Aktion, und wählen Sie die Eigenschaft **Filter Query** (Filterabfrage) aus.

   ![Hinzufügen der Eigenschaft „Filter Query“ (Filterabfrage)](./media/connect-common-data-service/list-records-action-filter-query.png)

1. Geben Sie in der Eigenschaft **Filter Query** (Filterabfrage), die jetzt in der Aktion angezeigt wird, die folgende ODATA-Filterabfrage ein: `statuscode eq 1`

   ![Eingeben der ODATA-Filterabfrage zum Filtern von Datensätzen](./media/connect-common-data-service/list-records-action-filter-query-value.png)

Weitere Informationen zu `$filter`-Systemabfrageoptionen finden Sie unter [Common Data Service: Filterergebnisse](/powerapps/developer/common-data-service/webapi/query-data-web-api#filter-results).

## <a name="list-records-based-on-an-order"></a>Auflisten von Datensätzen auf Grundlage einer Reihenfolge

Für Aktionen, die Datensätze zurückgeben, z. B. die Aktion **List records** (Datensätze auflisten), können Sie eine ODATA-Abfrage verwenden, die Datensätze in einer bestimmten Reihenfolge zurückgibt, die basierend auf den von der Aktion zurückgegebenen Datensätzen unterschiedlich ist. Beispielsweise können Sie mit der Aktion die Datensätze basierend auf dem Kontonamen auflisten.

1. Öffnen Sie die Liste **Add new parameter** (Neuen Parameter hinzufügen) in der Aktion, und wählen Sie die Eigenschaft **Order By** (Sortieren nach) aus.

   ![Hinzufügen der Eigenschaft „Order by“ (Sortieren nach)](./media/connect-common-data-service/list-records-action-order-by.png)

1. Geben Sie in der Eigenschaft **Order By** (Sortieren nach), die jetzt in der Aktion angezeigt wird, die folgende ODATA-Filterabfrage ein: `name`

   ![Eingeben der ODATA-Filterabfrage zum Sortieren von Datensätzen](./media/connect-common-data-service/list-records-action-order-by-value.png)

Weitere Informationen zu `$orderby`-Systemabfrageoptionen finden Sie unter [Common Data Service: Soertierergebnisse](/powerapps/developer/common-data-service/webapi/query-data-web-api#order-results).

## <a name="field-data-types"></a>Felddatentypen

Unabhängig davon, ob Sie für ein Feld in einem Trigger oder einer Aktion manuell einen Wert eingeben oder einen Wert aus der Liste mit dynamischem Inhalt auswählen, muss der Datentyp des Werts mit dem erforderlichen Datentyp des Felds übereinstimmen.

Diese Tabelle beschreibt einige der Feldtypen und die erforderlichen Datentypen für die Werte dieser Felder.

| Feld | Datentyp | BESCHREIBUNG |
|-------|-----------|-------------|
| Textfeld | Einzelne Textzeile | Erfordert entweder eine einzelne Textzeile oder dynamischen Inhalt, der den Textdatentyp aufweist, z. B. die folgenden Eigenschaften: <p><p>- **Beschreibung** <br>- **Kategorie** |
| Ganzzahliges Feld | Ganze Zahl | Erfordert entweder einen Integerwert oder dynamischen Inhalt, der den Integerdatentyp aufweist, z. B. die folgenden Eigenschaften: <p><p>- **Percent Complete (Prozent abgeschlossen)** <br>- **Duration (Dauer)** |
| Datumsfeld | Datum und Uhrzeit | Erfordert entweder ein Datum im Format MM/TT/JJJ oder dynamischen Inhalt, der den Datumsdatentyp aufweist, z. B. die folgenden Eigenschaften: <p><p>- **Created On (Erstellt am)** <br>- **Start Date (Startdatum)** <br>- **Actual Start (Tatsächlicher Start)** <br>- **Actual End (Tatsächliches Ende)** <br>- **Due Date (Fälligkeitsdatum)** |
| Feld, das auf einen anderen Entitätsdatensatz verweist | Primary key (Primärschlüssel) | Erfordert sowohl eine Datensatz-ID, z. B. eine GUID, als auch einen Nachschlagetyp. Dies bedeutet, dass Werte aus der Liste dynamischer Inhalte nicht funktionieren, z. B. die folgenden Eigenschaften: <p><p>- **Owner (Besitzer)** : Muss eine gültige Benutzer-ID oder eine Teamdatensatz-ID sein. <br>- **Owner Type (Besitzertyp)** : Muss ein Nachsclagetyp sein, z. B. `systemusers` oder `teams`. <p><p>- **Regarding (Bezüglich)** : Muss eine gültige Datensatz-ID sein, beispielsweise eine Konto- oder Kontaktdatensatz-ID. <br>- **Regarding Type (Typ für „Bezüglich“)** : Muss ein Nachschlagetyp sein, z. B. `accounts` oder `contacts`. <p><p>- **Customer (Kunde)** : Muss eine gültige Datensatz-ID sein, beispielsweise eine Konto- oder Kontaktdatensatz-ID. <br>- **Customer Type (Kundentyp)** : Muss ein Nachsclagetyp sein, z. B. `accounts` oder `contacts`. |
||||

Dieses Beispiel zeigt, wie die Aktion **Create a new record** (Neuen Datensatz erstellen) einen neuen „Aufgaben“datensatz erstellt, der anderen Entitätsdatensätzen zugeordnet ist, insbesondere einem Benutzerdatensatz und einem Kontodatensatz. Die Aktion gibt die IDs und Nachschlagetypen für diese Entitätsdatensätze an, indem sie Werte verwendet, die den erwarteten Datentypen für die relevanten Eigenschaften entsprechen.

* Basierend auf der Eigenschaft **Owner** (Besitzer), die eine Benutzer-ID angibt, und der Eigenschaft **Owner Type** (Besitzertyp), die den `systemusers`-Nachschlagetyp angibt, ordnet die Aktion den neuen „Aufgaben“datensatz einem bestimmten Benutzer zu.

* Basierend auf der Eigenschaft **Regarding**  (Bezüglich), die eine Datensatz-ID angibt, und der Eigenschaft **Regarding Type** (Typ für „Bezüglich“), die den `accounts`-Nachschlagetyp angibt, ordnet die Aktion den neuen „Aufgaben“datensatz einem bestimmten Konto zu.

![Erstellen eines „Aufgaben“datensatzes, der IDs und Nachschlagetypen zugeordnet ist](./media/connect-common-data-service/create-new-record-task-properties.png)

## <a name="connector-reference"></a>Connector-Referenz

Technische Informationen, die auf der Swagger-Beschreibung des Connectors basieren (z. B. Trigger, Aktionen und Grenzwerte sowie andere Details), finden Sie auf der [Referenzseite des Connectors](/connectors/commondataservice/).

## <a name="troubleshooting-problems"></a>Problembehandlung

### <a name="calls-from-multiple-environments"></a>Aufrufe aus mehreren Umgebungen

Beide Connectors, Common Data Service und Common Data Service (aktuelle Umgebung), speichern Informationen zu den Logik-App-Workflows, die Benachrichtigungen zu Entitätsänderungen benötigen und erhalten, mithilfe der `callbackregistrations`-Entität in Ihrer Microsoft Dataverse-Instanz. Wenn Sie eine Dataverse-Organisation kopieren, werden auch alle Webhooks kopiert. Wenn Sie Ihre Organisation kopieren, bevor Sie Workflows deaktivieren, die Ihrer Organisation zugeordnet sind, verweisen alle kopierten Webhooks ebenfalls auf dieselben Logik-Apps, die dann Benachrichtigungen von mehreren Organisationen erhalten.

Um unerwünschte Benachrichtigungen zu verhindern, löschen Sie die Rückrufregistrierung aus der Organisation, die diese Benachrichtigungen sendet. Befolgen Sie dazu die folgenden Schritte:

1. Ermitteln Sie die Dataverse-Organisation, aus der Sie Benachrichtigungen entfernen möchten, und melden Sie sich bei dieser Organisation an.

1. Suchen Sie im Chrome-Browser nach der Rückrufregistrierung, die Sie löschen möchten, indem Sie die folgenden Schritte ausführen:

   1. Überprüfen Sie die generische Liste für alle Rückrufregistrierungen unter folgendem OData-URI, damit Sie die Daten in der `callbackregistrations`-Entität anzeigen können:

      `https://{organization-name}.crm{instance-number}.dynamics.com/api/data/v9.0/callbackregistrations`:

      > [!NOTE]
      > Wenn keine Werte zurückgegeben werden, verfügen Sie möglicherweise nicht über die Berechtigungen zum Anzeigen dieses Entitätstyps, oder Sie sind nicht bei der richtigen Organisation angemeldet.

   1. Filtern Sie nach dem logischen Namen der auslösenden Entität (`entityname`) und dem Benachrichtigungsereignis, das Ihrem Logik-App-Workflow entspricht (Nachricht). Jeder Ereignistyp ist dem Nachrichteninteger wie folgt zugeordnet:

      | Ereignistyp | Nachrichteninteger |
      |------------|-----------------|
      | Erstellen | 1 |
      | Löschen | 2 |
      | Aktualisieren | 3 |
      | CreateOrUpdate | 4 |
      | CreateOrDelete | 5 |
      | UpdateOrDelete | 6 |
      | CreateOrUpdateOrDelete | 7 |
      |||

      Dieses Beispiel zeigt, wie Sie nach `Create`-Benachrichtigungen für eine Entität mit dem Namen `nov_validation` filtern können, indem Sie den folgenden OData-URI einer Beispielorganisation verwenden:

      `https://fabrikam-preprod.crm1.dynamics.com/api/data/v9.0/callbackregistrations?$filter=entityname eq 'nov_validation' and message eq 1`

      ![Screenshot des Browserfensters mit dem OData-URI auf der Adressleiste](./media/connect-common-data-service/find-callback-registrations.png)

      > [!TIP]
      > Wenn für dieselbe Entität oder dasselbe Ereignis mehrere Trigger vorhanden sind, können Sie die Liste filtern, indem Sie zusätzliche Filter wie die Attribute `createdon` und `_owninguser_value` verwenden. Der Benutzername des Besitzers wird unter `/api/data/v9.0/systemusers({id})` angezeigt.

   1. Nachdem Sie die ID der zu löschenden Rückrufregistrierung gefunden haben, führen Sie die folgenden Schritte aus:
   
      1. Öffnen Sie in Ihrem Chrome-Browser die Chrome-Entwicklertools (Tastatur: F12).

      1. Wählen Sie im Fenster oben die Registerkarte **Konsole** aus.

      1. Geben Sie in der Eingabeaufforderung den folgenden Befehl ein. Dieser sendet eine Anforderung zum Löschen der angegebenen Rückrufregistrierung:

         `fetch('http://{organization-name}.crm{instance-number}.dynamics.com/api/data/v9.0/callbackregistrations({ID-to-delete})', { method: 'DELETE'})`

         > [!IMPORTANT]
         > Stellen Sie sicher, dass Sie die Anforderung von einer Nicht-UCI-Seite (Unified Client Interface) ausgeht, z. B. von der OData- oder API-Antwortseite selbst. Andernfalls führt die Logik in der Datei „app.js“ möglicherweise zu einem Konflikt mit diesem Vorgang.

   1. Überprüfen Sie die Liste der Rückrufregistrierungen, um sicherzustellen, dass die Rückrufregistrierung nicht mehr vorhanden ist.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über andere [Connectors für Azure Logic Apps](../connectors/apis-list.md).
