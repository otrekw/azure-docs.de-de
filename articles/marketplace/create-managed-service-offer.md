---
title: Erstellen eines Angebots für einen verwalteten Dienst im kommerziellen Microsoft-Marketplace
description: Hier erfahren Sie, wie Sie mithilfe des kommerziellen Marketplace-Programms in Microsoft Partner Center ein neues Angebot für einen verwalteten Dienst für den Azure Marketplace planen.
author: Microsoft-BradleyWright
ms.author: brwrigh
ms.reviewer: anbene
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 12/23/2020
ms.openlocfilehash: 46a9c9d953a2311d83b5fd18b83727d6765734fa
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97917981"
---
# <a name="how-to-create-a-managed-service-offer-for-the-commercial-marketplace"></a>Erstellen eines Angebots für einen verwalteten Dienst für den kommerziellen Microsoft-Marketplace

In diesem Artikel wird erläutert, wie Sie über Partner Center ein Angebot für einen verwalteten Dienst für den kommerziellen Microsoft-Marketplace erstellen.

Zum Veröffentlichen eines Angebots für einen verwalteten Dienst müssen Sie über eine Gold- oder Silver-Kompetenz von Microsoft in der Kategorie „Cloudplattform“ verfügen. Lesen Sie den Artikel [Vorgehensweise beim Planen eines Angebots für verwaltete Dienste für den kommerziellen Microsoft-Marketplace](./plan-managed-service-offer.md), wenn Sie dies noch nicht getan haben. Anhand der Informationen dieses Artikels können Sie die Ressourcen vorbereiten, die Sie zum Erstellen des Angebots in Partner Center benötigen.

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Wählen Sie im linken Navigationsmenü **Kommerzieller Marketplace** > **Übersicht** aus.
3. Wählen Sie auf der Registerkarte „Übersicht“ die Option **+ Neues Angebot** > **Verwalteter Dienst** aus.

:::image type="content" source="./media/new-offer-managed-service.png" alt-text="Darstellung des Navigationsmenüs auf der linken Seite.":::

4. Geben Sie im Dialogfeld **Neues Angebot** eine **Angebots-ID** ein. Dies ist ein eindeutiger Bezeichner für jedes Angebot in Ihrem Konto. Diese ID ist in der URL der Auflistung im kommerziellen Marketplace und ggf. in den Azure Resource Manager-Vorlagen sichtbar. Wenn Sie in diesem Feld z. B. test-offer-1 eingeben, lautet die Webadresse für das Angebot `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.

    * Jedes Angebot in Ihrem Konto muss über eine eindeutige Angebots-ID verfügen.
    * Verwenden Sie nur Kleinbuchstaben und Zahlen. Sie kann Bindestriche und Unterstriche enthalten, jedoch keine Leerzeichen, und ist auf 50 Zeichen beschränkt.
    * Nachdem Sie **Erstellen** ausgewählt haben, kann die Angebots-ID nicht mehr geändert werden.

5. Geben Sie einen **Angebotsalias** ein. Dies ist der Name, der für das Angebot im Partner Center verwendet wird. Er wird nicht in den Onlinegeschäften angezeigt und unterscheidet sich vom Angebotsnamen, der Kunden angezeigt wird.
6. Wählen Sie **Erstellen** aus, um das Angebot zu generieren und fortzufahren.

## <a name="configure-lead-management"></a>Konfigurieren der Leadverwaltung

Verbinden Sie Ihr CRM-System (Customer Relationship Management) mit Ihrem Angebot im kommerziellen Marketplace, damit Sie Kundenkontaktinformationen erhalten, wenn ein Kunde Interesse an Ihrem Beratungsdienst äußert. Sie können diese Verbindung während oder nach der Erstellung des Angebots jederzeit ändern. Eine detaillierte Anleitung finden Sie unter [Kundenleads aus Ihrem Angebot im kommerziellen Marketplace](./partner-center-portal/commercial-marketplace-get-customer-leads.md).

So konfigurieren Sie die Leadverwaltung in Partner Center:

1. Navigieren Sie in Partner Center zur Registerkarte **Angebotseinrichtung**.
2. Wählen Sie unter **Kundenleads** den Link **Verbinden** aus.
3. Wählen Sie im Dialogfeld **Verbindungsdetails** eine Leadzielgruppe aus der Liste aus.
4. Füllen Sie alle angezeigten Felder aus. Ausführliche Schritte finden Sie in den folgenden Artikeln:

    * [Konfigurieren des Angebots zum Senden von Leads an die Azure-Tabelle](./partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table.md#configure-your-offer-to-send-leads-to-the-azure-table)
    * [Konfigurieren Ihres Angebots zum Senden von Leads an Dynamics 365 Customer Engagement](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md#configure-your-offer-to-send-leads-to-dynamics-365-customer-engagement) (früher Dynamics CRM Online)
    * [Konfigurieren Ihres Angebots zum Senden von Leads an den HTTPS-Endpunkt](./partner-center-portal/commercial-marketplace-lead-management-instructions-https.md#configure-your-offer-to-send-leads-to-the-https-endpoint)
    * [Konfigurieren Ihres Angebots zum Senden von Leads zu Marketo](./partner-center-portal/commercial-marketplace-lead-management-instructions-marketo.md#configure-your-offer-to-send-leads-to-marketo)
    * [Konfigurieren Ihres Angebots zum Senden von Leads zu Salesforce](./partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce.md#configure-your-offer-to-send-leads-to-salesforce)

5. Klicken Sie auf den Link **Überprüfen**, um die angegebene Konfiguration zu überprüfen.
6. Wenn Sie die Verbindungsdetails konfiguriert haben, klicken Sie auf **Verbinden**.
7. Wähen Sie **Entwurf speichern** aus.

Nachdem Sie Ihr Angebot zur Veröffentlichung in Partner Center eingereicht haben, wird die Verbindung überprüft und Ihnen wird ein Testlead gesendet. Testen Sie die Leadverbindung, während sich Ihr Angebot vor der Liveschaltung in der Vorschauphase befindet, indem Sie selbst versuchen, das Angebot in der Vorschauumgebung zu kaufen.

> [!TIP]
> Achten Sie darauf, dass die Verbindung mit dem Leadziel immer auf dem neuesten Stand ist, damit keine Leads verloren gehen.

## <a name="configure-offer-properties"></a>Konfigurieren von Angebotseigenschaften

Auf der Seite „Eigenschaften“ Ihres Angebots in Partner Center definieren Sie die für Ihr Angebot geltenden Kategorien sowie geltende rechtliche Vereinbarungen. Mit diesen Informationen stellen Sie sicher, dass Ihr verwalteter Dienst im Onlinestore korrekt angezeigt und den richtigen Kunden angeboten wird.

### <a name="select-a-category"></a>Kategorie auswählen

Wählen Sie unter **Kategorien** mindestens eine und höchstens fünf Kategorien aus, um Ihr Angebot in den entsprechenden Suchbereichen des kommerziellen Marketplace zu platzieren.

### <a name="provide-terms-and-conditions"></a>Angeben von Geschäftsbedingungen

Geben Sie unter **Rechtliche Hinweise** Ihre Nutzungsbedingungen für das Angebot an. Kunden müssen diese Bedingungen akzeptieren, bevor sie Ihr Angebot nutzen können. Sie können auch die URL angeben, unter der Ihre Geschäftsbedingungen zu finden sind.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.

## <a name="next-step"></a>Nächster Schritt

* [Konfigurieren des Listings Ihres Angebots für einen verwalteten Diensts](./create-managed-service-offer-listing.md)