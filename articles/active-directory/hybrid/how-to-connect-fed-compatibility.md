---
title: Azure AD-Verbund – Kompatibilitätsliste
description: Auf dieser Seite sind Identitätsanbieter anderer Anbieter aufgeführt, die zur Implementierung des einmaligen Anmeldens verwendet werden können.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/23/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 895e7347feea3190ba9bdc273200d2985dfaa0d1
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89661821"
---
# <a name="azure-ad-federation-compatibility-list"></a>Azure AD-Verbund – Kompatibilitätsliste
Azure Active Directory ermöglicht das einmalige Anmelden und bietet eine höhere Sicherheit beim Zugriff auf Anwendungen für Microsoft 365 und andere Microsoft-Onlinedienste sowohl für Hybridimplementierungen als auch für reine Cloudimplementierungen, ohne dass eine Drittanbieterlösung erforderlich ist. Wie die meisten Microsoft-Onlinedienste ist auch Microsoft 365 mit Azure Active Directory integriert, um die Verzeichnisdienste sowie die Authentifizierungs- und Autorisierungsfeatures nutzen zu können. Azure Active Directory ermöglicht zudem das einmalige Anmelden für Tausende SaaS-Anwendungen und lokale Webanwendungen. Die unterstützten SaaS-Anwendungen sind im Azure Active Directory-[Anwendungskatalog](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) aufgeführt. 

## <a name="idp-validation"></a>IDP-Validierung
Wenn Ihre Organisation eine Verbundlösung eines Drittanbieters verwendet, können Sie das einmalige Anmelden für Ihre lokalen Active Directory-Benutzer bei Microsoft-Onlinediensten wie Microsoft 365 konfigurieren, sofern die Verbundlösung des Drittanbieters mit Azure Active Directory kompatibel ist.  Bei Fragen bezüglich der Kompatibilität wenden Sie sich bitte an Ihren Identitätsanbieter.  Eine Liste der Identitätsanbieter, die bereits von Microsoft auf Kompatibilität mit Azure AD getestet wurden, finden Sie in den [Dokumenten zur Kompatibilität von Identitätsanbietern mit Azure AD](https://www.microsoft.com/download/details.aspx?id=56843). 

>[!NOTE]
>Microsoft stellt unabhängigen Identitätsanbietern keine Tests mehr für die Validierung der Kompatibilität mit Azure Active Directory zur Verfügung. Wenn Sie Ihr Produkt auf Interoperabilität testen möchten, lesen Sie [diese Richtlinien](https://www.microsoft.com/download/details.aspx?id=56843). 

## <a name="next-steps"></a>Nächste Schritte

- [Integrieren Ihrer lokalen Verzeichnisse in Azure Active Directory](whatis-hybrid-identity.md)
- [Azure AD Connect und Verbund](how-to-connect-fed-whatis.md)
