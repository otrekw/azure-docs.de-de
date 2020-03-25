---
title: include file
description: include file
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 02/03/2020
ms.custom: include file
ms.openlocfilehash: cfe3eb4c0ac1378b7c519b3b34094945612d8508
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "77029187"
---
>[!NOTE]
>Dieser Abschnitt enthält Anweisungen für die [Azure AD-App-Registrierung](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app).

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) im erweiterbaren Menü auf der linken Seite **Azure Active Directory** und anschließend den Bereich **App-Registrierungen**. 

    [![Auswählen des Bereichs „Azure Active Directory“](./media/digital-twins-permissions/azure-portal-select-aad-pane.png)](./media/digital-twins-permissions/azure-portal-select-aad-pane.png#lightbox)

1. Wählen Sie die Schaltfläche **+ Neue Registrierung** aus.

    [![Auswählen der Schaltfläche „Neue Registrierung“](./media/digital-twins-permissions/aad-app-register.png)](./media/digital-twins-permissions/aad-app-register.png#lightbox)

1. Geben Sie im Feld **Name** einen Anzeigenamen für diese App-Registrierung ein. 

    1. Geben Sie im Abschnitt **Umleitungs-URI (optional)** den Wert `https://microsoft.com` ins Textfeld ein.     

    1. Überprüfen Sie, welche Konten und Mandanten von Ihrer Azure Active Directory-App unterstützt werden.

    1. Wählen Sie **Registrieren**.

    [![Bereich zum Erstellen](./media/digital-twins-permissions/aad-app-reg-create.png)](./media/digital-twins-permissions/aad-app-reg-create.png#lightbox)

1. Auf dem Blatt **Authentifizierung** sind wichtige Konfigurationseinstellungen für die Authentifizierung angegeben. 

    1. Fügen Sie **Umleitungs-URIs** hinzu, und konfigurieren Sie **Zugriffstoken**, indem Sie **+ Plattform hinzufügen** auswählen.

    1. Wählen Sie **Ja** aus, um anzugeben, dass die App ein **öffentlicher Client** ist.

    1. Überprüfen Sie, welche Konten und Mandanten von Ihrer Azure Active Directory-App unterstützt werden.

    [![Konfigurationseinstellung für den öffentlichen Client](./media/digital-twins-permissions/aad-configure-public-client.png)](./media/digital-twins-permissions/aad-configure-public-client.png#lightbox)

1. Konfigurieren Sie nach der Auswahl der geeigneten Plattform die **Umleitungs-URIs** und **Zugriffstoken** im Seitenpanel rechts auf der Benutzeroberfläche.

    1. **Umleitungs-URIs** müssen mit der in der Authentifizierungsanforderung angegebenen Adresse übereinstimmen:

        * Wählen Sie für Anwendungen, die in einer lokalen Entwicklungsumgebung gehostet werden, **Öffentlicher Client (Mobilgerät und Desktop)** aus. Stellen Sie sicher, dass **Öffentlicher Client** auf **Ja** festgelegt ist.
        * Wählen Sie für in Azure App Service gehostete Einzelseiten-Apps **Web** aus.

    1. Legen Sie fest, ob eine **Abmelde-URL** geeignet ist.

    1. Aktivieren Sie den Ablauf für die implizite Genehmigung, indem Sie die Option **Zugriffstoken** oder **ID-Token** aktivieren.
                
    [![Konfigurieren von Umleitungs-URIs](./media/digital-twins-permissions/aad-app-configure-redirect-uris.png)](./media/digital-twins-permissions/aad-app-configure-redirect-uris.png#lightbox)

    Klicken Sie auf **Konfigurieren** und dann auf **Speichern**.

1.  Öffnen Sie den Bereich **Übersicht** Ihrer registrierten App, und kopieren Sie die Werte der folgenden Entitäten in eine temporäre Datei. Mit diesen Werten konfigurieren Sie in den folgenden Abschnitten Ihre Beispielanwendung.

    - **Anwendungs-ID (Client)**
    - **Verzeichnis-ID (Mandant)**

    [![ID der Azure Active Directory-Anwendung](./media/digital-twins-permissions/aad-app-reg-app-id.png)](./media/digital-twins-permissions/aad-app-reg-app-id.png#lightbox)

1. Öffnen Sie den Bereich **API-Berechtigungen** für Ihre App-Registrierung. Wählen Sie die Schaltfläche **+ Berechtigung hinzufügen** aus. Wählen Sie im Bereich **API-Berechtigungen anfordern** die Registerkarte **Von meiner Organisation verwendete APIs** aus, und suchen Sie nach einer der folgenden Optionen:
    
    1. [https://login.microsoftonline.com/consumers/](`Azure Digital Twins`). Wählen Sie die API **Azure Digital Twins** aus.

        [![Suchen nach der API oder nach Azure Digital Twins](./media/digital-twins-permissions/aad-aap-search-api-dt.png)](./media/digital-twins-permissions/aad-aap-search-api-dt.png#lightbox)

    1. Oder suchen Sie nach `Azure Smart Spaces Service`. Wählen Sie die API **Azure Smart Spaces Service** aus.

        [![Suchen nach der API für Azure Smart Spaces](./media/digital-twins-permissions/aad-app-search-api.png)](./media/digital-twins-permissions/aad-app-search-api.png#lightbox)

    > [!IMPORTANT]
    > Welcher Name und welche ID für die Azure AD-API angezeigt werden, richtet sich nach Ihrem Mandanten:
    > * Bei Testmandanten- und Kundenkonten sollte nach `Azure Digital Twins` gesucht werden.
    > * Bei anderen Microsoft-Konten sollte nach `Azure Smart Spaces Service` gesucht werden.

1. Die ausgewählte API wird als **Azure Digital Twins** im gleichen Bereich (**API-Berechtigungen anfordern**) angezeigt. Wählen Sie die Dropdownoption **Lesen** aus, und aktivieren Sie anschließend das Kontrollkästchen **Read.Write**. Wählen Sie die Schaltfläche **Berechtigungen hinzufügen** aus.

    [![Hinzufügen von API-Berechtigungen](./media/digital-twins-permissions/aad-app-req-permissions.png)](./media/digital-twins-permissions/aad-app-req-permissions.png#lightbox)

1. Je nach Einstellungen Ihrer Organisation müssen Sie möglicherweise zusätzliche Schritte unternehmen, um dem Administrator Zugriff auf diese API zu gewähren. Weitere Informationen erhalten Sie von Ihrem Administrator. Nach Genehmigung des Administratorzugriffs werden Ihre Berechtigungen im Bereich **API-Berechtigungen** in der Spalte **Administratoreinwilligung erforderlich** angezeigt: 

    [![Genehmigung durch Administratoreinwilligung](./media/digital-twins-permissions/aad-app-admin-consent.png)](./media/digital-twins-permissions/aad-app-admin-consent.png#lightbox)

    Vergewissern Sie sich, dass **Azure Digital Twins** angezeigt wird.
