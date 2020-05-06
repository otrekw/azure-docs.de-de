---
title: Konfigurieren der Leadverwaltung mithilfe eines HTTPS-Endpunkts | Azure Marketplace
description: Hier erfahren Sie, wie Sie einen HTTP-Endpunkt zur Handhabung von Microsoft AppSource- und Azure Marketplace-Leads verwenden.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/21/2020
ms.author: dsindona
ms.openlocfilehash: f56cc5aaad7d77ff8dc753115ef1becb08ddde73
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81770200"
---
# <a name="configure-lead-management-using-an-https-endpoint"></a>Konfigurieren der Leadverwaltung zur Verwendung eines HTTPS-Endpunkts

Sie können einen HTTPS-Endpunkt zur Handhabung von Microsoft AppSource- und Azure Marketplace-Leads verwenden. Diese Leads können in ein CRM-System (Customer Relationship Management) geschrieben oder als E-Mail-Benachrichtigung gesendet werden. In diesem Artikel erfahren Sie, wie Sie den Automatisierungsdienst [Microsoft Power Automate](https://powerapps.microsoft.com/automate-processes/) zum Konfigurieren der Leadverwaltung verwenden.

## <a name="create-a-flow-using-microsoft-power-automate"></a>Erstellen eines Flows mit Microsoft Power Automate

1. Rufen Sie die [Power Automate](https://flow.microsoft.com/)-Webseite auf. Wählen Sie **Anmelden** aus, oder wählen Sie **Kostenlos registrieren** aus, um ein kostenloses Flow-Konto zu erstellen.

1. Melden Sie sich an, und wählen Sie **Meine Flows** in der Menüleiste aus.
    > [!div class="mx-imgBorder"]
    > ![Meine Flows](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows.png)

1. Wählen Sie unter **+ Neu** die Option **+ Sofort – ohne Vorlage** aus.
    > [!div class="mx-imgBorder"]
    > ![Ohne Vorlage erstellen](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-create-fromblank.png)

1. Geben Sie dem Flow einen Namen, und wählen Sie dann unter **Auslöser für diesen Flow auswählen** die Option **Beim Empfang einer HTTP-Anforderung** aus.

    > [!div class="mx-imgBorder"]
    > ![Auswählen des Auslösers „Beim Empfang einer HTTP-Anforderung“](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-pick-request-trigger.png)

1. Klicken Sie auf den Flowschritt, um ihn zu erweitern.

    > [!div class="mx-imgBorder"]
    > ![Erweitern des Flowschritts](./media/cloud-partner-portal-lead-management-instructions-https/expand-flow-step.png)

1. Verwenden Sie eine der folgenden Methoden, um das **JSON-Schema für Anforderungstext** zu konfigurieren:

   - Kopieren Sie das [JSON-Schema](#json-schema) am Ende dieses Artikels in das Textfeld **JSON-Schema für Anforderungstext**.
   - Wählen Sie **Beispielnutzlast zum Generieren eines Schemas verwenden** aus. Fügen Sie im Textfeld **Geben oder fügen Sie eine JSON-Beispielnutzlast ein.** das [JSON-Beispiel](#json-example) ein. Wählen Sie **Fertig** aus, um das Schema zu erstellen.

   >[!Note]
   >An dieser Stelle können Sie entweder eine Verbindung mit einem CRM-System herstellen oder eine E-Mail-Benachrichtigung konfigurieren.

### <a name="to-connect-to-a-crm-system"></a>So stellen Sie eine Verbindung mit einem CRM-System her

1. Wählen Sie **+ Neuer Schritt**aus.
2. Wählen Sie das gewünschte CRM-System mit der Aktion zum Erstellen eines neuen Datensatzes aus. Auf dem folgenden Screenshot wird beispielsweise **Dynamics 365 – Neuen Datensatz erstellen** verwendet.

    ![Create a new record (Neuen Datensatz erstellen)](./media/cloud-partner-portal-lead-management-instructions-https/https-image009.png)

3. Geben Sie den **Namen der Organisation** an, der als Verbindungseingabe für Ihren Connector fungiert. Wählen Sie in der Dropdownliste **Entitätsname** die Option **Leads** aus.

    ![Leads auswählen](./media/cloud-partner-portal-lead-management-instructions-https/https-image011.png)

4. Flow zeigt ein Formular für die Eingabe von Leadinformationen an. Sie können Elemente aus der Eingabeanforderung zuordnen, indem Sie auswählen, dass dynamischer Inhalt hinzugefügt werden soll. Im folgenden Screenshot wird beispielsweise **OfferTitle** verwendet.

    ![Dynamischen Inhalt hinzufügen](./media/cloud-partner-portal-lead-management-instructions-https/https-image013.png)

5. Ordnen Sie die gewünschten Felder zu, und wählen Sie dann **Speichern** aus, um den Flow zu speichern.

6. In der Anforderung wird eine HTTP-POST-URL erstellt. Kopieren Sie diese URL, und verwenden Sie sie als HTTPS-Endpunkt.

    ![HTTP-POST-URL](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-get-post-url.png)

### <a name="to-set-up-email-notification"></a>So richten Sie E-Mail-Benachrichtigungen ein

1. Wählen Sie **+ Neuer Schritt**aus.
2. Wählen Sie unter **Aktion auswählen** die Option **Aktionen** aus.
3. Wählen Sie unter **Aktionen** die Option **E-Mail senden** aus.

    ![Hinzufügen einer E-Mail-Aktion](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-add-email-action.png)

4. Konfigurieren Sie unter **E-Mail senden** die folgenden Pflichtfelder:

   - **An**: Geben Sie mindestens eine gültige E-Mail-Adresse ein.
   - **Betreff**: Flow ermöglicht das Hinzufügen dynamischer Inhalte (wie etwa **LeadSource** im folgenden Screenshot).

     ![Hinzufügen einer E-Mail-Aktion mit dynamischem Inhalt](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-configure-email-dynamic-content.png)

   - **Text**: Fügen Sie über die Liste für dynamische Inhalte die gewünschten Informationen in den E-Mail-Text ein. Verwenden Sie beispielsweise „LastName“ (Nachname), „FirstName“ (Vorname), „Email“ (E-Mail-Adresse) und „Company“ (Unternehmen).

   Ein Beispiel für eine fertig eingerichtete E-Mail-Benachrichtigung sehen Sie auf dem folgenden Screenshot.

   ![Hinzufügen einer E-Mail-Aktion](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-configure-email-action.png)

5. Wählen Sie **Speichern** aus, um den Flow fertigzustellen.

6. In der Anforderung wird eine HTTP-POST-URL erstellt. Kopieren Sie diese URL, und verwenden Sie sie als HTTPS-Endpunkt.

    ![HTTP-POST-URL](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-get-post-url.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Konfigurieren Sie Ihr Angebot so, dass Leads an den HTTPS-Endpunkt gesendet werden

Wenn Sie die Informationen zur Leadverwaltung für Ihr Angebot konfigurieren, wählen Sie **HTTPS-Endpunkt** als **Leadziel** aus, und fügen Sie die HTTP-POST-URL ein, die Sie im vorherigen Schritt kopiert haben.  

![Dynamischen Inhalt hinzufügen](./media/cloud-partner-portal-lead-management-instructions-https/https-image017.png)

Generierte Leads werden von Microsoft an Ihren Power Automate-Flow gesendet und anschließend an das konfigurierte CRM-System oder an die konfigurierte E-Mail-Adresse weitergeleitet.

## <a name="json-schema-and-example"></a>JSON-Schema und Beispiel

Das JSON-Testbeispiel verwendet folgendes Schema:

### <a name="json-schema"></a>JSON-Schema

``` json
{
  "$schema": "https://json-schema.org/draft-04/schema#",
  "definitions": {},
  "id": "http://example.com/example.json",
  "properties": {
    "ActionCode": {
      "id": "/properties/ActionCode",
      "type": "string"
    },
    "OfferTitle": {
      "id": "/properties/OfferTitle",
      "type": "string"
    },
    "LeadSource": {
      "id": "/properties/LeadSource",
      "type": "string"
    },
    "Description": {
      "id": "/properties/Description",
      "type": "string"
    },
    "UserDetails": {
      "id": "/properties/UserDetails",
      "properties": {
        "Company": {
          "id": "/properties/UserDetails/properties/Company",
          "type": "string"
        },
        "Country": {
          "id": "/properties/UserDetails/properties/Country",
          "type": "string"
        },
        "Email": {
          "id": "/properties/UserDetails/properties/Email",
          "type": "string"
        },
        "FirstName": {
          "id": "/properties/UserDetails/properties/FirstName",
          "type": "string"
        },
        "LastName": {
          "id": "/properties/UserDetails/properties/LastName",
          "type": "string"
        },
        "Phone": {
          "id": "/properties/UserDetails/properties/Phone",
          "type": "string"
        },
        "Title": {
          "id": "/properties/UserDetails/properties/Title",
          "type": "string"
        }
      },
      "type": "object"
    }
  },
  "type": "object"
}
```

Sie können das folgende JSON-Beispiel kopieren und bearbeiten, um es als Test in Ihrem Flow zu verwenden.

### <a name="json-example"></a>JSON-Beispiel

```json
{
  "UserDetails": {
    "FirstName": "Some",
    "LastName": "One",
    "Email": "someone@contoso.com",
    "Phone": "16175555555",
    "Country": "USA",
    "Company": "Contoso",
    "Title": "Esquire"
 },
  "LeadSource": "AzureMarketplace",
  "ActionCode": "INS",
  "OfferTitle": "Test Microsoft",
  "Description": "Test run through Power Automate"
}
```

## <a name="next-steps"></a>Nächste Schritte

Konfigurieren Sie [Kundenleads](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads) im Cloud-Partnerportal (sofern noch nicht geschehen).
