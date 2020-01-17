---
title: Cookie-Definitionen – Azure Active Directory B2C | Microsoft-Dokumentation
description: Enthält Definitionen für die Cookies, die in Azure Active Directory B2C verwendet werden.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 66de4559ed006735f53ff993cce29370428b9998
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "75930889"
---
# <a name="cookies-definitions-for-azure-active-directory-b2c"></a>Cookie-Definitionen für Azure Active Directory B2C

Die folgende Tabelle enthält die Cookies, die in Azure Active Directory B2C verwendet werden.

| Name | Domain | Ablauf | Zweck |
| ----------- | ------ | -------------------------- | --------- |
| x-ms-cpim-admin | main.b2cadmin.ext.azure.com | Ende der [Browsersitzung](session-behavior.md) | Enthält mandantenübergreifende Daten zur Benutzermitgliedschaft: Mandanten, zu denen ein Benutzer gehört, und die Ebene der Mitgliedschaft („Admin“ oder „Benutzer“). |
| x-ms-cpim-slice | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](session-behavior.md) | Zum Weiterleiten von Anforderungen an die entsprechende Produktionsinstanz. |
| x-ms-cpim-trans | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](session-behavior.md) | Zum Nachverfolgen von Transaktionen (Anzahl der Authentifizierungsanforderungen an Azure AD B2C) und der aktuellen Transaktion. |
| x-ms-cpim-sso:{Id} | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](session-behavior.md) | Zum Verwalten der Sitzung mit einmaligem Anmelden (SSO). |
| x-ms-cpim-cache:{id}_n | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](session-behavior.md), erfolgreiche Authentifizierung | Zum Verwalten des Anforderungsstatus. |
| x-ms-cpim-csrf | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](session-behavior.md) | Token der websiteübergreifenden Anforderungsfälschung zum Schutz vor CSRF-Angriffen. |
| x-ms-cpim-dc | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](session-behavior.md) | Für Azure AD B2C-Netzwerkrouting. |
| x-ms-cpim-ctx | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](session-behavior.md) | Kontext |
| x-ms-cpim-rp | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](session-behavior.md) | Zum Speichern der Mitgliedschaftsdaten für den Ressourceourcenanbieter-Mandanten. |
| x-ms-cpim-rc | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](session-behavior.md) | Zum Speichern des Relay-Cookies. |
