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
ms.openlocfilehash: 98aa697c0120fd8a20adf11ad83e02406499a0d1
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101648491"
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

* [Was ist die Identitätsbereitstellung?](../cloud-sync/what-is-provisioning.md) 

* [Tools für die Verzeichnisintegration für Hybrididentitäten](../hybrid/plan-hybrid-identity-design-considerations-tools-comparison.md) 

* [Roadmap für die Installation von Azure AD Connect](../hybrid/how-to-connect-install-roadmap.md)