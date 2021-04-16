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
ms.date: 10/02/2020
ms.openlocfilehash: 22411e5a80f555a3ead05d39466a7a175923d9bc
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105105454"
---
* Konfigurieren Sie nach der Auswahl der geeigneten Plattform in Schritt 4 der Einstellungen zu [Plattform konfigurieren](../articles/active-directory/develop/quickstart-register-app.md#configure-platform-settings) die **Umleitungs-URIs** und **Zugriffstoken** im Seitenpanel rechts auf der Benutzeroberfläche.

    * **Umleitungs-URIs** müssen mit der in der Authentifizierungsanforderung angegebenen Adresse übereinstimmen:

        * Wählen Sie für Anwendungen, die in einer lokalen Entwicklungsumgebung gehostet werden, **Öffentlicher Client (Mobilgerät und Desktop)** aus. Stellen Sie sicher, dass **Öffentlicher Client** auf **Ja** festgelegt ist.
        * Wählen Sie für in Azure App Service gehostete Einzelseiten-Apps **Web** aus.

    * Legen Sie fest, ob eine **Abmelde-URL** geeignet ist.

    * Aktivieren Sie den Ablauf für die implizite Genehmigung, indem Sie die Option **Zugriffstoken** oder **ID-Token** aktivieren.

    [![Erstellen von Umleitungs-URIs](media/time-series-insights-aad-registration/active-directory-auth-redirect-uri.png)](media/time-series-insights-aad-registration/active-directory-auth-redirect-uri.png#lightbox)

    Klicken Sie auf **Konfigurieren** und dann auf **Speichern**.

* Ordnen Sie Ihre Azure Active Directory-App Azure Time Series Insights zu. Wählen Sie **API-Berechtigungen** > **Berechtigung hinzufügen** > **Von meiner Organisation verwendete APIs** aus.

    [![Zuordnen einer API zu Ihrer Azure Active Directory-App](media/time-series-insights-aad-registration/active-directory-app-api-permission.png)](media/time-series-insights-aad-registration/active-directory-app-api-permission.png#lightbox)

   Geben in die Suchleiste `Azure Time Series Insights` ein, und wählen Sie dann `Azure Time Series Insights` aus.

* Geben Sie als Nächstes die Art von API-Berechtigung an, die Ihre App benötigt. Standardmäßig ist **Delegierte Berechtigungen** hervorgehoben. Wählen Sie einen Berechtigungstyp und dann **Berechtigungen hinzufügen**.

    [![Angeben der Art der von Ihrer App benötigten API-Berechtigung](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png)](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png#lightbox)

* [Fügen Sie Anmeldeinformationen hinzu](../articles/active-directory/develop/quickstart-register-app.md#add-credentials), wenn die Anwendung selbst die APIs Ihrer Umgebung aufruft. Mit den Anmeldeinformationen kann sich Ihre Anwendung selbst authentifizieren und benötigt zur Laufzeit keine Interaktion durch einen Benutzer.