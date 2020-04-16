---
title: Merkmale der Interaktion mehrerer Mandanten – Azure AD | Microsoft-Dokumentation
description: Enthält grundlegende Informationen zu Ihren Azure Active Directory-Mandanten als vollständig unabhängige Organisationen.
services: active-tenant
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 04/07/2020
ms.author: curtand
ms.custom: it-pro
ms.reviewer: sumitp
ms.collection: M365-identity-device-management
ms.openlocfilehash: 175d9ce7db1657e0e654f46adaf8a8d8ef28c25e
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878118"
---
# <a name="understand-how-multiple-azure-active-directory-organizations-interact"></a>Grundlegendes zur Interaktion mehrerer Azure Active Directory-Organisationen

In Azure Active Directory (Azure AD) ist jeder Mandant eine vollständig unabhängige Organisation: gleichgestellt und logisch unabhängig von anderen Azure AD-Organisationen, die Sie verwalten. Diese Unabhängigkeit zwischen den Organisationen umfasst die Unabhängigkeit in Bezug auf Ressourcen, Verwaltung und Synchronisierung. Es gibt keine unter- und übergeordneten Beziehungen zwischen den Organisationen.

## <a name="resource-independence"></a>Ressourcenunabhängigkeit

* Wenn Sie eine Azure AD-Ressource in einer Organisation erstellen oder löschen, hat dies keine Auswirkungen auf Ressourcen in einer anderen Organisation (gilt teilweise nicht für externe Benutzer).
* Wenn Sie einen Ihrer Domänennamen bei einer Organisation registrieren, kann er nicht von einer anderen Organisation verwendet werden.

## <a name="administrative-independence"></a>Verwaltungsunabhängigkeit

Wenn ein regulärer Benutzer (kein Administrator) der Organisation „Contoso“ die Testorganisation „Test“ erstellt, gilt Folgendes:

* Standardmäßig wird der Benutzer, der eine Organisation erstellt, als externer Benutzer in dieser neuen Organisation hinzugefügt, und ihm wird die globale Administratorrolle für diese Organisation zugewiesen.
* Die Administratoren für die Organisation „Contoso“ haben keine direkten Administratorberechtigungen für die Organisation „Test“, sofern ihnen nicht ein Administrator dieser Organisation die Berechtigungen explizit erteilt. Die Administratoren von „Contoso“ können allerdings den Zugriff auf die Organisation „Test“ steuern, wenn sie das Benutzerkonto steuern, mit dem diese Organisation erstellt wurde.
* Wenn Sie eine Azure AD-Rolle für einen Benutzer in einer Organisation hinzufügen oder entfernen, wirkt sich diese Änderung nicht auf die Rollen aus, die dem Benutzer in einer anderen Azure AD-Organisation zugewiesen werden.

## <a name="synchronization-independence"></a>Synchronisierungsunabhängigkeit

Sie können jede Azure AD-Organisation einzeln konfigurieren, damit die Daten jeweils nur über eine Instanz synchronisiert werden, z. B.:

* Dem Tool Azure AD Connect, um Daten mit einer AD-Gesamtstruktur zu synchronisieren
* Dem Azure Active Directory Connector für Forefront Identity Manager, um Daten mit mindestens einer lokalen Gesamtstruktur und/oder anderen Datenquellen als Azure AD zu synchronisieren.

## <a name="add-an-azure-ad-organization"></a>Hinzufügen einer Azure AD-Organisation

Um eine Azure AD-Organisation im Azure-Portal hinzuzufügen, melden Sie sich mit einem globalen Azure AD-Administratorkonto beim [Azure-Portal](https://portal.azure.com) an und wählen die Option **Neu** aus.

> [!NOTE]
> Im Gegensatz zu anderen Azure-Ressourcen sind Ihre Azure AD-Organisationen keine untergeordneten Ressourcen eines Azure-Abonnements. Wenn Ihr Azure-Abonnement gekündigt wurde oder abgelaufen ist, können Sie per Azure PowerShell, Microsoft Graph-API oder Microsoft 365 Admin Center weiter auf die Daten Ihrer Azure AD-Organisation zugreifen. Sie können [der Organisation auch ein anderes Abonnement zuordnen](../fundamentals/active-directory-how-subscriptions-associated-directory.md).
>

## <a name="next-steps"></a>Nächste Schritte

Informationen zu Lizenzierungsaspekten und bewährten Methoden im Zusammenhang mit Azure AD finden Sie unter [Was ist Azure Active Directory-Lizenzierung?](../fundamentals/active-directory-licensing-whatis-azure-portal.md).
