---
title: include file
description: include file
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.date: 12/06/2019
ms.openlocfilehash: 4d32980e825f12c76b5c8bf8df0673fa82065751
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75460425"
---
1. Wählen Sie im [Azure-Portal](https://ms.portal.azure.com/) nacheinander **Azure Active Directory** > **App-Registrierungen** > **Neue Registrierung** aus.

   [![Registrierung einer neuen Anwendung in Azure Active Directory](media/time-series-insights-aad-registration/active-directory-new-application-registration.png)](media/time-series-insights-aad-registration/active-directory-new-application-registration.png#lightbox)

    Ihre App wird hier aufgelistet, nachdem Sie sie registriert haben.

1. Geben Sie der Anwendung einen Namen, und wählen Sie **Nur Konten in diesem Organisationsverzeichnis** aus, um **Unterstützten Kontotypen** anzugeben, die auf die API zugreifen dürfen. Wählen Sie einen gültigen URI, an den die Benutzer nach der Authentifizierung weitergeleitet werden sollen, und sich dann **registrieren**.

   [![Erstellen der Anwendung in Azure Active Directory](media/time-series-insights-aad-registration/active-directory-registration.png)](media/time-series-insights-aad-registration/active-directory-registration.png#lightbox)

1. Wichtige Informationen zu Azure Active Directory-Apps werden auf dem Blatt **Übersicht** Ihrer aufgeführten App angezeigt. Wählen Sie Ihre App unter **Anwendungen mit Besitzer** aus, und klicken Sie dann auf **Übersicht**.

   [![Kopieren der Anwendungs-ID](media/time-series-insights-aad-registration/active-directory-copy-application-id.png)](media/time-series-insights-aad-registration/active-directory-copy-application-id.png#lightbox)

   Kopieren Sie Ihre **Anwendungs-ID (Client)** , um sie in Ihrer Clientanwendung zu verwenden.

1. Auf dem Blatt **Authentifizierung** sind wichtige Konfigurationseinstellungen für die Authentifizierung angegeben. 

    1. **Umleitungs-URIs** müssen mit der in der Authentifizierungsanforderung angegebenen Adresse übereinstimmen:

        * Wählen Sie für Anwendungen, die in einer lokalen Entwicklungsumgebung gehostet werden, **Öffentlicher Client (Mobilgerät und Desktop)** aus. Stellen Sie sicher, dass **Standardclienttyp** auf „Ja“ festgelegt ist.
        * Wählen Sie für in Azure App Service gehostete Einzelseiten-Apps **Web** aus.

    1. Aktivieren Sie den Ablauf für die implizite Genehmigung, indem Sie die Option **Zugriffstoken** oder **ID-Token** aktivieren.

   [![Erstellen von Umleitungs-URIs und Konfigurieren der impliziten Gewährung](media/time-series-insights-aad-registration/active-directory-auth-blade.png)](media/time-series-insights-aad-registration/active-directory-auth-blade.png#lightbox)

   Klicken Sie auf **Speichern**.

1. Wählen Sie **Zertifikate & Geheimnisse** und dann **Neuer geheimer Clientschlüssel** aus, um ein Anwendungskennwort zu erstellen, die der Client zum Nachweis seiner Identität verwenden kann.

   [![Erstellen eines neuen geheimen Clientschlüssels](media/time-series-insights-aad-registration/active-directory-application-keys-save.png)](media/time-series-insights-aad-registration/active-directory-application-keys-save.png#lightbox)

   Das Kennwort für den geheimen Clientschlüssel wird angezeigt. Kopieren Sie den Schlüssel in einen Text-Editor.

   > [!NOTE]
   > Sie können stattdessen auch ein Zertifikat importieren. Die Verwendung eines Zertifikats wird empfohlen, um die Sicherheit zu erhöhen. Wenn Sie ein Zertifikat verwenden möchten, wählen Sie **Zertifikat hochladen** aus.

1. Ordnen Sie Ihre Azure Active Directory-App Azure Time Series Insights zu. Wählen Sie **API-Berechtigungen** > **Berechtigung hinzufügen** > **Von meiner Organisation verwendete APIs** aus. 

    [![Zuordnen einer API zu Ihrer Azure Active Directory-App](media/time-series-insights-aad-registration/active-directory-app-api-permission.png)](media/time-series-insights-aad-registration/active-directory-app-api-permission.png#lightbox)

   Geben in die Suchleiste `Azure Time Series Insights` ein, und wählen Sie dann `Azure Time Series Insights` aus.

1. Geben Sie als Nächstes die Art von API-Berechtigung an, die Ihre App benötigt. Standardmäßig ist **Delegierte Berechtigungen** hervorgehoben. Wählen Sie einen Berechtigungstyp und dann **Berechtigungen hinzufügen**.

    [![Angeben der Art der von Ihrer App benötigten API-Berechtigung](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png)](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png#lightbox)