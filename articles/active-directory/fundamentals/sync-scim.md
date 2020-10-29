---
title: SCIM-Synchronisierung mit Azure Active Directory
description: Architektonischer Leitfaden zum Erreichen dieses Synchronisierungsmusters
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
ms.openlocfilehash: d1dda465dd675e0f5f519f86289df2621be0b9bb
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2020
ms.locfileid: "92367855"
---
# <a name="scim-synchronization-with-azure-active-directory"></a>SCIM-Synchronisierung mit Azure Active Directory

System for Cross-Domain Identity Management (SCIM) ist ein offenes Standardprotokoll für die Automatisierung des Austauschs von Benutzeridentitätsinformationen zwischen Identitätsdomänen und IT-Systemen. SCIM stellt sicher, dass für die dem System für die Humankapitalverwaltung (Human Capital Management, HCM) hinzugefügten Mitarbeiter automatisch Konten in Azure Active Directory (Azure AD) oder Windows Server Active Directory erstellt werden. Benutzerattribute und -profile werden zwischen den beiden Systemen synchronisiert, wobei das Entfernen von Benutzern basierend auf dem Benutzerstatus oder der Rollenänderung aktualisiert wird.

SCIM ist eine standardisierte Definition von zwei Endpunkten: einem „/Users“- und einem „/Groups“-Endpunkt. Es verwendet allgemeine REST-Verben, um Objekte zu erstellen, zu aktualisieren und zu löschen. Außerdem verwendet es ein vordefiniertes Schema für allgemeine Attribute wie Gruppenname, Benutzername, Vorname, Nachname und E-Mail-Adresse. Anwendungen, die eine SCIM 2.0-REST-API bieten, können den Aufwand für die Arbeit mit proprietären Benutzerverwaltungs-APIs oder -produkten reduzieren oder eliminieren. So kann beispielsweise jeder SCIM-kompatible Client eine HTTP POST-Anforderung für ein JSON-Objekt an den „/Users“-Endpunkt senden, um einen neuen Benutzereintrag zu erstellen. Anstatt eine leicht abweichende API für dieselben grundlegenden Aktionen zu benötigen, können Apps, die dem SCIM-Standard entsprechen, sofort die Vorteile bereits vorhandener Clients, Tools und Codes nutzen. 

## <a name="use-when"></a>Zu verwenden in folgenden Fällen: 

Sie möchten Benutzerinformationen aus einem HCM-System automatisch für Azure AD und Windows Server Active Directory und dann ggf. für Zielsysteme bereitstellen. 

![Architekturdiagramm](./media/authentication-patterns/scim-auth.png)


## <a name="components-of-system"></a>Komponenten des Systems 

* **HCM-System** : Anwendungen und Technologien, die HCM-Prozesse und -Praktiken ermöglichen, die HR-Prozesse während des gesamten Mitarbeiterlebenszyklus unterstützen und automatisieren. 

* **Azure AD-Bereitstellungsdienst** : Verwendet das SCIM 2.0-Protokoll für die automatische Bereitstellung. Der Dienst stellt eine Verbindung mit dem SCIM-Endpunkt für die Anwendung her und verwendet das SCIM-Benutzerobjektschema und REST-APIs, um die Bereitstellung und Aufhebung der Bereitstellung von Benutzern und Gruppen zu automatisieren.  

* **Azure AD** : Das zum Verwalten des Lebenszyklus von Identitäten und deren Berechtigungen verwendete Benutzerrepository. 

* **Zielsystem** : Die Anwendung oder das System, die/das über den SCIM-Endpunkt verfügt und die Azure AD-Bereitstellung verwendet, um die automatische Bereitstellung von Benutzern und Gruppen zu ermöglichen.  

## <a name="implement-scim-with-azure-ad"></a>Implementieren von SCIM mit Azure AD 

* [Funktionsweise der Bereitstellung in Azure AD](https://docs.microsoft.com/azure/active-directory/app-provisioning/how-provisioning-works)

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps im Azure-Portal](https://docs.microsoft.com/azure/active-directory/app-provisioning/configure-automatic-user-provisioning-portal)

* [Erstellen eines SCIM-Endpunkts und Konfigurieren der Benutzerbereitstellung mit Azure AD](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups)

* [Bekannte Probleme und Lösungen bei der Einhaltung des SCIM 2.0-Protokolls des Azure AD-Benutzerbereitstellungsdiensts](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-config-problem-scim-compatibility)

