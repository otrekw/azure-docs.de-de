---
title: Akzeptiert meine Azure AD-Anmeldeseite Microsoft-Konten? | Microsoft-Dokumentation
description: Informationen dazu, wie Meldungen auf dem Bildschirm das Nachschlagen des Benutzernamens bei der Anmeldung widerspiegeln
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: enterprise-users
ms.topic: overview
ms.date: 11/15/2020
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: b2a3c2257311f208ae704a54f1369e10b2ff160b
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94646847"
---
# <a name="sign-in-options-for-microsoft-accounts-in-azure-active-directory"></a>Anmeldeoptionen für Microsoft-Konten in Azure Active Directory

Die Anmeldeseite von Microsoft 365 für Azure Active Directory (Azure AD) unterstützt Geschäfts-, Schul- oder Unikonten und Microsoft-Konten. Je nach Situation des Benutzers werden allerdings möglicherweise nur eine oder auch beide Kontoarten unterstützt. Die Azure AD-Anmeldeseite unterstützt beispielsweise folgende Kontoarten:

* Apps, bei denen die Anmeldung über beide Kontotypen möglich ist
* Organisationen, die Gäste akzeptieren

## <a name="identification"></a>Identifikation
Sie können feststellen, ob die Anmeldeseite Ihrer Organisation Microsoft-Konten unterstützt, indem Sie sich den Hinweistext im Feld für den Benutzernamen ansehen. Wenn im Hinweistext „E-Mail, Telefon oder Skype“ angezeigt wird, unterstützt die Anmeldeseite Microsoft-Konten.

![Unterschied zwischen Konten für Anmeldeseiten](./media/signin-account-support/ui-prompt.png)

[Zusätzliche Anmeldeoptionen können nur für persönliche Microsoft-Konten](https://azure.microsoft.com/updates/microsoft-account-signin-options/ ), jedoch nicht für die Anmeldung bei Geschäfts-, Schul- oder Unikonten verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

[Anpassen des Brandings Ihrer Anmeldung](../fundamentals/add-custom-domain.md)