---
title: Grundlegendes zu Device Update for IoT Hub-Authentifizierung und -Autorisierung | Microsoft-Dokumentation
description: Verstehen Sie, wie Device Update for IoT Hub Azure RBAC verwendet, um Authentifizierung und Autorisierung für Benutzer und Dienst-APIs bereitzustellen.
author: vimeht
ms.author: vimeht
ms.date: 2/11/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: 07310a5b6f275d4a35a3649c22aeea68045dde8b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101660352"
---
# <a name="azure-role-based-access-control-rbac-and-device-update"></a>Rollenbasierte Zugriffssteuerung in Azure (Role-Based Access Control, RBAC) und Device Update

Device Update verwendet Azure RBAC, um Authentifizierung und Autorisierung für Benutzer und Dienst-APIs bereitzustellen.

## <a name="configure-access-control-roles"></a>Konfigurieren von Zugriffssteuerungsrollen

Damit andere Benutzer und Anwendungen auf Device Update zugreifen können, muss den Benutzern oder Anwendungen Zugriff auf diese Ressource gewährt werden. Die folgenden Rollen werden von Device Update unterstützt:

|   Rollenname   | BESCHREIBUNG  |
| :--------- | :---- |
|  Device Update-Administrator | Besitzt Zugriff auf alle Geräteaktualisierungsressourcen.  |
|  Device Update-Leser| Kann alle Updates und Bereitstellungen anzeigen. |
|  Device Update-Inhaltsadministrator | Kann Updates anzeigen, importieren und löschen.  |
|  Device Update-Inhaltsleser | Kann Updates anzeigen.  |
|  Device Update-Bereitstellungsadministrator | Kann die Bereitstellung von Updates für Geräte verwalten.|
|  Device Update-Bereitstellungsleser| Kann Bereitstellungen von Updates für Geräte anzeigen. |

Eine Kombination von Rollen kann verwendet werden, um die richtige Zugriffsebene bereitzustellen. Beispielsweise kann ein Entwickler mit der Rolle „Device UpdateInhaltsadministrator“ Updates importieren und verwalten, er benötigt aber die Rolle „Device Update-Bereitstellungsleser“, um den Fortschritt eines Updates anzuzeigen. Umgekehrt kann ein Lösungsoperator mit der Rolle „Device Update-Leser“ alle Updates anzeigen, muss aber die Rolle „Device Update-Bereitstellungsadministrator“ verwenden, um ein bestimmtes Update auf Geräten bereitzustellen.


## <a name="authenticate-to-device-update-rest-apis-for-publishing-and-management"></a>Authentifizieren bei den Device Update-REST-APIs für Veröffentlichung und Verwaltung

Device Update verwendet auch Azure AD für die Authentifizierung, um Inhalte über Dienst-APIs zu veröffentlichen und zu verwalten. Zunächst müssen Sie eine Clientanwendung erstellen und konfigurieren.

### <a name="create-client-azure-ad-app"></a>Erstellen einer Azure AD-Client-App

Um eine Anwendung oder einen Dienst in Azure AD zu integrieren, [registrieren](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app) Sie zunächst eine Anwendung bei Azure AD. Das Setup der Clientanwendung ist je nach verwendetem Autorisierungsflow unterschiedlich.  Die folgende Konfiguration dient als Leitfaden bei Verwendung der Device Update-REST-APIs.

* Festlegen der Clientauthentifizierung: „Umleitungs-URIs für native Apps oder einen Webclient“.
* Festlegen von API-Berechtigungen: Device Update for IoT Hub stellt Folgendes bereit:
  * Delegierte Berechtigungen: „user_impersonation“
  * **Optional**: Administratoreinwilligung erteilen

[Nächste Schritte: Erstellen von Device Update-Ressourcen und Konfigurieren von Zugriffssteuerungsrollen](./create-device-update-account.md)
