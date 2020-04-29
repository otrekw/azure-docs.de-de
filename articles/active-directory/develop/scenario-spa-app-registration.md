---
title: Registrieren von Single-Page-Apps – Microsoft Identity Platform | Azure
description: Informieren Sie sich, wie Sie eine Single-Page-Webanwendung (App-Registrierung) erstellen.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 6f690a8b3436a45d434ccad2bbaa7d2a1b0b76aa
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "80882147"
---
# <a name="single-page-application-app-registration"></a>Single-Page-Webanwendung: App-Registrierung

Auf dieser Seite werden die Besonderheiten der App-Registrierung für eine Single-Page-Webanwendung (Single-Page App, SPA) erläutert.

Befolgen Sie die Schritte, um [eine neue Anwendung bei der Microsoft Identity Platform zu registrieren](quickstart-register-app.md), und wählen Sie die unterstützten Konten für Ihre Anwendung aus. Das SPA-Szenario kann die Authentifizierung mit Konten in Ihrer Organisation oder in anderen Organisation und persönlichen Microsoft-Konten unterstützen.

Erfahren Sie nun mehr zu den Punkten der Anwendungsregistrierung, die für Single-Page-Webanwendungen gelten.

## <a name="register-a-redirect-uri"></a>Registrieren eines Umleitungs-URI

Der implizite Flow sendet die Token in einer Umleitung an die Single-Page-Webanwendung, die in einem Webbrowser ausgeführt wird. Es ist daher wichtig, dass Sie einen Umleitungs-URI registrieren, über den Ihre Anwendung die Token empfangen kann. Achten Sie darauf, dass der Umleitungs-URI genau mit dem URI Ihrer Anwendung übereinstimmt.

Navigieren Sie im [Azure-Portal](https://go.microsoft.com/fwlink/?linkid=2083908) zur registrierten Anwendung. Klicken Sie auf der Seite **Authentifizierung** auf die Plattform **Web**. Geben Sie den Wert des Umleitungs-URIs für Ihre Anwendung in das Feld **Umleitungs-URI** ein.

## <a name="enable-the-implicit-flow"></a>Aktivieren des impliziten Flows

Aktivieren Sie außerdem auf der Seite **Authentifizierung** unter **Erweiterte Einstellungen** die Option **Implicit grant** (Implizite Genehmigung). Wenn Ihre Anwendung nur Benutzer anmeldet und ID-Token abruft, genügt es, das Kontrollkästchen **ID tokens** (ID-Token) zu aktivieren.

Wenn Ihre Anwendung zudem Zugriffstoken zum Aufrufen von APIs benötigt, aktivieren Sie zusätzlich die Option **Zugriffstoken**. Weitere Informationen finden Sie unter [ID-Token](./id-tokens.md) und [Zugriffstoken](./access-tokens.md).

## <a name="api-permissions"></a>API-Berechtigungen

SPAs können APIs im Namen angemeldeter Benutzer aufrufen. Sie müssen delegierte Berechtigungen anfordern. Weitere Informationen finden Sie unter [Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Codekonfiguration der App](scenario-spa-app-configuration.md)
