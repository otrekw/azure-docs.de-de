---
title: 'Web-App-Tutorial: Einrichten einer Clientanwendung'
description: Dieses Tutorial führt Sie Schritte für Schritt durch das Registrieren einer öffentlichen Anwendung als Vorbereitung für das Bereitstellen einer Webanwendung.
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: mihansen
ms.author: cavoeg
author: caitlinv39
ms.date: 01/03/2020
ms.openlocfilehash: 58e21e46edfe1d03d42bf2202bcf1f22282631a9
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "84870453"
---
# <a name="client-application-registration"></a>Registrieren einer Clientanwendung
Im vorherigen Tutorial haben Sie Azure API for FHIR bereitgestellt und eingerichtet. Nachdem Sie Azure API for FHIR eingerichtet haben, registrieren Sie nun eine öffentliche Clientanwendung. Weitere Details und Informationen zur Problembehandlung finden Sie in der vollständigen Schrittanleitung zum [Registrieren einer öffentlichen Client-App](register-public-azure-ad-client-app.md). Die wichtigsten Schritte für dieses Tutorial sind jedoch nachfolgend aufgeführt.

1. Navigieren Sie zu Azure Active Directory.
1. Wählen Sie **App-Registrierung** --> **Neue Registrierung** aus.
1. Geben Sie der Anwendung einen Namen, und legen Sie den Umleitungs-URI auf https://www.getpostman.com/oauth2/callback fest.


![Registrieren einer Clientanwendung](media/tutorial-web-app/reg-public-app.png)

## <a name="client-application-settings"></a>Clientanwendungseinstellungen
Kopieren Sie nach der Registrierung der Clientanwendung die Anwendungs-ID (Client) von der Übersichtsseite. Sie benötigen diesen Wert später beim Zugreifen auf den Client.

![Kopieren der App-ID](media/tutorial-web-app/app-id.png)

Legen Sie als Nächstes die richtigen Authentifizierungsoptionen fest. Wählen Sie links die Option **Authentifizierung** aus. Aktivieren Sie die Kontrollkästchen **Zugriffstoken** und **ID-Token**. Sie können auch den Umleitungs-URI einrichten. Dies dient als Vorbereitung für das Erstellen Ihrer Webanwendung im vierten Teil dieses Tutorials. Fügen Sie hierzu „https://\<WEB-APP-NAME>.azurewebsites.net“ zur Liste der Umleitungs-URIs hinzu. Wenn Sie in dem Schritt, in dem Sie die [Web-App schreiben](tutorial-web-app-write-web-app.md), einen anderen Namen wählen, müssen Sie zurückkehren und diesen Eintrag aktualisieren.

![App-Authentifizierungseinstellungen](media/tutorial-web-app/app-authentication.png)

Nachdem Sie die richtige Authentifizierung eingerichtet haben, legen Sie die API-Berechtigungen fest. 
1. Wählen Sie **API-Berechtigungen** aus, und klicken Sie auf **Berechtigung hinzufügen**.
1. Suchen Sie unter **Von meiner Organisation verwendete APIs** nach Azure-APIs für das Gesundheitswesen.
1. Wählen Sie **user_impersonation** aus, und klicken Sie auf **Berechtigungen hinzufügen**.

## <a name="next-steps"></a>Nächste Schritte
Sie verfügen jetzt über eine öffentlichen Clientanwendung. Im nächsten Tutorial wird das Testen und der Zugriff auf diese Anwendung über Postman beschrieben.

>[!div class="nextstepaction"]
>[Testen der Clientanwendung in Postman](tutorial-web-app-test-postman.md)
