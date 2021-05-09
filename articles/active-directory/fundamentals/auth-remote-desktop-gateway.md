---
title: Remotedesktop-Gatewaydienste mit Azure Active Directory
description: Architekturleitfaden zum Implementieren von Remotedesktop-Gatewaydiensten mit Azure Active Directory
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97f43ee22ade532ab8efd633c0c332b78e73263f
ms.sourcegitcommit: 516eb79d62b8dbb2c324dff2048d01ea50715aa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108181146"
---
# <a name="remote-desktop-gateway-services"></a>Remotedesktop-Gatewaydienste

Eine standardmäßige RDS-Bereitstellung (Remote Desktop Services) umfasst verschiedene [Remotedesktop-Rollendienste](/windows-server/remote/remote-desktop-services/Desktop-hosting-logical-architecture), die unter Windows Server ausgeführt werden. Die RDS-Bereitstellung mit dem Azure AD-Anwendungsproxy verfügt über eine permanente ausgehende Verbindung von dem Server, auf dem der Connectordienst ausgeführt wird. Bei anderen Bereitstellungen werden eingehende Verbindungen durch einen Lastenausgleich offen gelassen. Mit diesem Authentifizierungsmuster können Sie mehr Arten von Anwendung anbieten, indem Sie lokale Anwendungen über Remotedesktopdienste veröffentlichen. Das Muster verringert zudem die Angriffsfläche der Bereitstellung, da der Azure AD-Anwendungsproxy verwendet wird.

## <a name="use-when"></a>Wenn folgende Bedingungen vorliegen

Sie müssen Remotezugriff bereitstellen und Ihre Remotedesktopdienste-Bereitstellung per Vorauthentifizierung schützen.

![Architekturdiagramm](./media/authentication-patterns/rdp-auth.png)

## <a name="components-of-system"></a>Komponenten des Systems

* **Benutzer:** Der Benutzer greift auf die über den Anwendungsproxy bereitgestellten Remotedesktopdienste zu.

* **Webbrowser**: Die Komponente, mit der der Benutzer interagiert, um auf die externe URL der Anwendung zuzugreifen.

* **Azure AD**: Hiermit wird der Benutzer authentifiziert. 

* **Anwendungsproxydienst**: Dieser Dienst fungiert als Reverseproxy zum Weiterleiten von Anforderungen vom Benutzer an RDS. Der Anwendungsproxy kann auch Richtlinien für bedingten Zugriff erzwingen. 

* **Remotedesktopdienste**: RDS fungiert als Plattform für einzelne virtualisierte Anwendungen und bietet sicheren mobilen sowie Remotedesktopzugriff. Zudem bietet RDS Endbenutzern die Möglichkeit, ihre Anwendungen und Desktops in der Cloud auszuführen. 

## <a name="implement-remote-desktop-gateway-services-with-azure-ad"></a>Implementieren von Remotedesktop-Gatewaydiensten mit Azure AD

* [Veröffentlichen des Remotedesktops per Azure AD-Anwendungsproxy](../manage-apps/application-proxy-integrate-with-remote-desktop-services.md) 

* [Hinzufügen einer lokalen Anwendung für den Remotezugriff über den Anwendungsproxy in Azure AD](../app-proxy/application-proxy-add-on-premises-application.md)

