---
title: 'Azure AD Connect: Funktionen in der Vorschau | Microsoft-Dokumentation'
description: In diesem Thema werden Funktionen detaillierter beschrieben, die sich in Azure AD Connect in der Preview befinden.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 05/15/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d48cd8f95ecaf3e537a5221d766af150a51aa31
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102517683"
---
# <a name="more-details-about-features-in-preview"></a>Weitere Informationen zu den Funktionen in der Vorschau
In diesem Thema wird beschrieben, wie Sie Funktionen verwenden, die sich derzeit in der Vorschau befinden.

## <a name="azure-ad-connect-sync-v2-endpoint-api"></a>Azure AD Connect-Synchronisierung: V2-Endpunkt-API

Wir haben einen neuen Endpunkt (API) für Azure AD Connect bereitgestellt, mit dem die Leistung der Synchronisierungsdienstvorgänge für Azure Active Directory verbessert wird. Wenn Sie den neuen V2-Endpunkt verwenden, treten beim Exportieren aus und Importieren in Azure AD spürbare Leistungssteigerungen auf. Dieser neue Endpunkt unterstützt auch das Synchronisieren von Gruppen mit bis zu 250.000 Mitgliedern. Mithilfe dieses Endpunkts können Sie auch einheitliche Microsoft 365-Gruppen ohne maximale Mitgliedschaftsbeschränkung in Ihr lokales Active Directory zurückschreiben, wenn Gruppenrückschreiben aktiviert ist. Weitere Informationen finden Sie unter [Azure AD Connect-Synchronisierung: V2-Endpunkt-API](how-to-connect-sync-endpoint-api-v2.md).

## <a name="user-writeback"></a>Rückschreiben von Benutzern
> [!IMPORTANT]
> Die Vorschaufunktion „Rückschreiben von Benutzern“ wurde im Azure AD Connect-Update vom August 2015 entfernt. Wenn Sie diese Funktion aktiviert haben, sollten Sie sie deaktivieren.
>
>

## <a name="next-steps"></a>Nächste Schritte
Fahren Sie mit Ihrer [benutzerdefinierten Installation von Azure AD Connect](how-to-connect-install-custom.md)fort.

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](whatis-hybrid-identity.md).
