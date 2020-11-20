---
title: Verzeichnissynchronisierung mit Azure Active Directory
description: Architekturleitfaden zum Erzielen der Verzeichnissynchronisierung mit Azure Active Directory
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
ms.openlocfilehash: c340f973193f9c46735423c86112816003fecfcd
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578908"
---
# <a name="directory-synchronization"></a>Verzeichnissynchronisierung

Viele Organisationen verfügen über eine Hybridinfrastruktur, die sowohl lokale Komponenten als auch Cloudkomponenten umfasst. Durch die Synchronisierung von Benutzeridentitäten zwischen lokalen und Cloudverzeichnissen können Benutzer mit einem einzigen Satz von Anmeldeinformationen auf Ressourcen zugreifen. 

Synchronisierung ist der Prozess der 

* Erstellung eines Objekts auf der Grundlage bestimmter Bedingungen
* Aktualisierung des Objekts
* Entfernung des Objekts, wenn die Bedingungen nicht mehr erfüllt sind. 

Die lokale Bereitstellung umfasst die Bereitstellung aus lokalen Quellen (wie Active Directory) in Azure Active Directory (Azure AD). 

## <a name="use-when"></a>Wenn folgende Bedingungen vorliegen

Sie müssen Identitätsdaten aus Ihren lokalen Active Directory-Umgebungen mit Azure AD synchronisieren.

![Architekturdiagramm](./media/authentication-patterns/dir-sync-auth.png)

## <a name="components-of-system"></a>Komponenten des Systems

* **Benutzer:** Greift mithilfe von Azure AD auf eine Anwendung zu.

* **Webbrowser**: Die Komponente, mit der der Benutzer interagiert, um auf die externe URL der Anwendung zuzugreifen.

* **Anwendung**: Web-App, die Azure AD zu Authentifizierungs- und Autorisierungszwecken verwendet.

* **Azure AD**: Synchronisiert Identitätsinformationen aus dem lokalen Verzeichnis der Organisation über Azure AD Connect. 

* **Azure AD Connect**: Ein Tool zum Verbinden von lokalen Identitätsinfrastrukturen mit Microsoft Azure AD. Der Assistent und die geführten Oberflächen unterstützen Sie beim Bereitstellen und Konfigurieren von Voraussetzungen und Komponenten, die für die Verbindung erforderlich sind, einschließlich der Synchronisierung und Anmeldung von Active Directory-Instanzen mit/bei Azure AD. 

* **Active Directory**: Active Directory ist ein Verzeichnisdienst, der in den meisten Windows Server-Betriebssystemen enthalten ist. Server, auf denen Active Directory Domain Services (AD DS) ausgeführt wird, werden als Domänencontroller bezeichnet. Sie authentifizieren und autorisieren alle Benutzer und Computer in der Domäne.

## <a name="implement-directory-synchronization-with-azure-ad"></a>Implementieren der Verzeichnissynchronisierung mit Azure AD

* [Was ist die Identitätsbereitstellung?](https://docs.microsoft.com/azure/active-directory/cloud-provisioning/what-is-provisioning) 

* [Tools für die Verzeichnisintegration für Hybrididentitäten](https://docs.microsoft.com/azure/active-directory/hybrid/plan-hybrid-identity-design-considerations-tools-comparison) 

* [Roadmap für die Installation von Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-roadmap)
