---
title: include file
description: include file
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 01/06/2020
ms.custom: include file
ms.openlocfilehash: a6adbe095b3ed486be8eb2e2611db5a40162d5dd
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2020
ms.locfileid: "75895482"
---
>[!NOTE]
>Dieser Abschnitt enthält Anweisungen für die [Azure AD-App-Registrierung](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app).

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) im erweiterbaren Menü auf der linken Seite **Azure Active Directory** und anschließend den Bereich **App-Registrierungen**. 

    [![Auswählen des Bereichs „Azure Active Directory“](./media/digital-twins-permissions/azure-portal-select-aad-pane.png)](./media/digital-twins-permissions/azure-portal-select-aad-pane.png#lightbox)

1. Wählen Sie die Schaltfläche **+ Neue Registrierung** aus.

    [![Auswählen der Schaltfläche „Neue Registrierung“](./media/digital-twins-permissions/aad-app-register.png)](./media/digital-twins-permissions/aad-app-register.png#lightbox)

1. Geben Sie im Feld **Name** einen Anzeigenamen für diese App-Registrierung ein. Wählen Sie im Abschnitt **Umleitungs-URI (optional)** im Dropdownmenü auf der linken Seite die Option **Öffentlicher Client/nativ (mobil und Desktop)** aus, und geben Sie `https://microsoft.com` in das Textfeld auf der rechten Seite ein. Wählen Sie **Registrieren**.

    [![Bereich zum Erstellen](./media/digital-twins-permissions/aad-app-reg-create.png)](./media/digital-twins-permissions/aad-app-reg-create.png#lightbox)

1. Um sicherzustellen, dass [die App als **öffentlicher Client**](https://docs.microsoft.com/azure/active-directory/develop/scenario-desktop-app-registration) registriert wird, öffnen Sie den Bereich **Authentifizierung** für Ihre App-Registrierung, und scrollen Sie in diesem Bereich nach unten. Wählen Sie im Abschnitt **Standardclienttyp** für **Anwendung als öffentlichen Client behandeln** die Option **Ja** und anschließend **Speichern** aus.

    Aktivieren Sie das Kontrollkästchen **Zugriffstoken**, um die Einstellung **oauth2AllowImplicitFlow** in der Datei „Manifest.json“ zu aktivieren.

    [![Konfigurationseinstellung für den öffentlichen Client](./media/digital-twins-permissions/aad-configure-public-client.png)](./media/digital-twins-permissions/aad-configure-public-client.png#lightbox)

1.  Öffnen Sie den Bereich **Übersicht** Ihrer registrierten App, und kopieren Sie die Werte der folgenden Entitäten in eine temporäre Datei. Mit diesen Werten konfigurieren Sie in den folgenden Abschnitten Ihre Beispielanwendung.

    - **Anwendungs-ID (Client)**
    - **Verzeichnis-ID (Mandant)**

    [![ID der Azure Active Directory-Anwendung](./media/digital-twins-permissions/aad-app-reg-app-id.png)](./media/digital-twins-permissions/aad-app-reg-app-id.png#lightbox)

1. Öffnen Sie den Bereich **API-Berechtigungen** für Ihre App-Registrierung. Wählen Sie die Schaltfläche **+ Berechtigung hinzufügen** aus. Wählen Sie im Bereich **API-Berechtigungen anfordern** die Registerkarte **Von meiner Organisation verwendete APIs** aus, und suchen Sie nach einer der folgenden Optionen:
    
    1. `Azure Digital Twins`. Wählen Sie die API **Azure Digital Twins** aus.

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
