---
title: Remotedesktop-Gatewaydienste mit Azure Active Directory
description: Architekturleitfaden zum Erreichen dieses Authentifizierungsmusters
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
ms.openlocfilehash: ff099d41970ad4d78d5c6035a60f448f29ab24b1
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92113927"
---
# <a name="remote-desktop-gateway-services"></a>Remotedesktop-Gatewaydienste

Eine standardmäßige RDS-Bereitstellung (Remote Desktop Services) umfasst verschiedene [Remotedesktop-Rollendienste](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture), die unter Windows Server ausgeführt werden. Die RDS-Bereitstellung mit dem Azure AD-Anwendungsproxy verfügt über eine permanente ausgehende Verbindung von dem Server, auf dem der Connectordienst ausgeführt wird. Bei anderen Bereitstellungen werden eingehende Verbindungen durch einen Lastenausgleich offen gelassen. Mit diesem Authentifizierungsmuster können Sie mehr Arten von Anwendung anbieten, indem Sie lokale Anwendungen über Remotedesktopdienste veröffentlichen. Das Muster verringert zudem die Angriffsfläche der Bereitstellung, da der Azure AD-Anwendungsproxy verwendet wird.

## <a name="use-when"></a>Wenn folgende Bedingungen vorliegen

Sie müssen Remotezugriff bereitstellen und Ihre Remotedesktopdienste-Bereitstellung per Vorauthentifizierung schützen.

![Architekturdiagramm](./media/authentication-patterns/rdp-auth.png)

## <a name="components-of-system"></a>Komponenten des Systems

* **Benutzer:** Der Benutzer greift auf die über den Anwendungsproxy bereitgestellten Remotedesktopdienste zu.

* **Webbrowser** : Die Komponente, mit der der Benutzer interagiert, um auf die externe URL der Anwendung zuzugreifen.

* **Azure AD** : Hiermit wird der Benutzer authentifiziert. 

* **Anwendungsproxydienst** : Dieser Dienst fungiert als Reverseproxy zum Weiterleiten von Anforderungen vom Benutzer an RDS. Der Anwendungsproxy kann auch Richtlinien für bedingten Zugriff erzwingen. 

* **Remotedesktopdienste** : RDS fungiert als Plattform für einzelne virtualisierte Anwendungen und bietet sicheren mobilen sowie Remotedesktopzugriff. Zudem bietet RDS Endbenutzern die Möglichkeit, ihre Anwendungen und Desktops in der Cloud auszuführen. 

## <a name="implement-remote-desktop-gateway-services-with-azure-ad"></a>Implementieren von Remotedesktop-Gatewaydiensten mit Azure AD

* [Veröffentlichen des Remotedesktops per Azure AD-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-integrate-with-remote-desktop-services) 

* [Hinzufügen einer lokalen Anwendung für den Remotezugriff über den Anwendungsproxy in Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application)

 
