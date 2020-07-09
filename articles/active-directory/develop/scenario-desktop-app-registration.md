---
title: Registrieren von Desktop-Apps, die Web-APIs aufrufen – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie eine Desktop-App erstellen, die Web-APIs aufruft (App-Registrierung)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/09/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 599603ba867e21694392e38e9692280f010e08eb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "80885156"
---
# <a name="desktop-app-that-calls-web-apis-app-registration"></a>Desktop-App, die Web-APIs aufruft: App-Registrierung

In diesem Artikel sind die Besonderheiten zur App-Registrierung bei einer Desktopanwendung aufgeführt.

## <a name="supported-account-types"></a>Unterstützte Kontotypen

Die in einer Desktopanwendung unterstützten Kontotypen sind von der gewünschten Funktion abhängig. Aufgrund dieser Beziehung sind die unterstützten Kontotypen von den Flows abhängig, die Sie verwenden möchten.

### <a name="audience-for-interactive-token-acquisition"></a>Zielgruppe für den interaktiven Tokenabruf

Wenn Ihre Desktopanwendung die interaktive Authentifizierung verwendet, können Sie Benutzer jedes [Kontotyps](quickstart-register-app.md#register-a-new-application-using-the-azure-portal) anmelden.

### <a name="audience-for-desktop-app-silent-flows"></a>Zielgruppe für automatische Desktop-App-Flüsse

- Um die integrierte Windows-Authentifizierung oder Benutzername und Kennwort verwenden zu können, muss Ihre Anwendung Benutzer bei Ihrem Mandanten anmelden, beispielsweise wenn Sie LOB-Entwickler (Line of Business) sind. In Azure Active Directory-Organisationen muss Ihre Anwendung Benutzer bei Ihrem Mandanten anmelden, wenn es sich um ein ISV-Szenario handelt. Diese Authentifizierungsflows werden für persönliche Microsoft-Konten nicht unterstützt.
- Wenn Sie den Gerätecodeflow verwenden möchten, können Sie noch keine Benutzer mit ihren persönlichen Microsoft-Konten anmelden.
- Wenn Sie Benutzer mit Identitäten sozialer Netzwerke anmelden, die eine B2C-Autorität und -Richtlinie übergeben, können Sie ausschließlich die interaktive Authentifizierung oder die Authentifizierung über Benutzername und Kennwort verwenden.

## <a name="redirect-uris"></a>Umleitungs-URIs

Die in einer Desktopanwendung zu verwendenden Umleitungs-URIs hängen von dem Flow ab, den Sie verwenden möchten.

- Bei Verwendung der interaktiven Authentifizierung oder des Gerätecodeflows verwenden Sie `https://login.microsoftonline.com/common/oauth2/nativeclient`. Um diese Konfiguration zu erzielen, wählen Sie im Abschnitt **Authentifizierung** für Ihre Anwendung die entsprechende URL aus.
  
  > [!IMPORTANT]
  > Aktuell verwendet MSAL.NET standardmäßig in Desktopanwendungen, die unter Windows ausgeführt werden (`urn:ietf:wg:oauth:2.0:oob`), einen anderen Umleitungs-URI. In Zukunft möchten wir diese Standardeinstellung ändern. Daher empfehlen wir Ihnen die Verwendung von `https://login.microsoftonline.com/common/oauth2/nativeclient`.

- Wenn Sie eine native Objective-C-oder Swift-App für macOS erstellen, sollten Sie den Umleitungs-URI basierend auf der Paket-ID Ihrer Anwendung im folgenden Format registrieren: msauth.<ihre.app.paket.id>://auth. Ersetzen Sie <ihre.app.paket.id> durch die Paket-ID Ihrer Anwendung.
- Wenn Ihre App nur die integrierte Windows-Authentifizierung oder Benutzername und Kennwort verwendet, müssen Sie für Ihre Anwendung keinen Umleitungs-URI registrieren. Diese Flows führen einen Roundtrip zum Microsoft Identity Platform v 2.0-Endpunkt aus. Ihre Anwendung wird nicht über einen bestimmten URI zurückgerufen.
- Damit Gerätecodeflow, integrierte Windows-Authentifizierung sowie Benutzername und Kennwort von einem vertraulichen Clientanwendungsflow unterschieden werden können, der ebenfalls über keine Umleitungs-URIs verfügt (der bei Daemon-Anwendungen verwendete Client-Anmeldeinformationsflow), müssen Sie ausdrücken, dass es sich bei Ihrer Anwendung um eine öffentliche Clientanwendung handelt. Um diese Konfiguration zu erreichen, wechseln Sie zum Abschnitt **Authentifizierung** für Ihre Anwendung. Wählen Sie im Unterabschnitt **Erweiterte Einstellungen** im Abschnitt **Standardclienttyp** bei **Hiermit wird eine Anwendung als öffentlicher Client eingestuft** die Option **Ja** aus.

  ![Zulassen eines öffentlichen Clients](media/scenarios/default-client-type.png)

## <a name="api-permissions"></a>API-Berechtigungen

Desktopanwendungen rufen APIs für den angemeldeten Benutzer auf. Sie müssen delegierte Berechtigungen anfordern. Sie können keine Anwendungsberechtigungen anfordern, die nur in [Daemon-Anwendungen](scenario-daemon-overview.md) verarbeitet werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Desktop-App: App-Konfiguration](scenario-desktop-app-configuration.md)
