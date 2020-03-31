---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/14/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 3417bf0bd4ae1e0aa670f9fbfcc1fbbfeb372972
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471518"
---
Wenn Sie möchten, dass Benutzer vor der Gewährung des Zugriffs zur Angabe eines zweiten Authentifizierungsfaktor aufgefordert werden, können Sie Azure Multi-Factor Authentication (MFA) konfigurieren. Sie können MFA auf Benutzerbasis konfigurieren oder MFA über [bedingten Zugriff](../articles/active-directory/conditional-access/overview.md) nutzen.

* MFA auf Benutzerbasis kann ohne zusätzliche Kosten aktiviert werden. Wenn jedoch MFA auf Benutzerbasis aktiviert wird, wird der Benutzer zur Authentifizierung mit dem zweiten Faktor für alle Anwendungen aufgefordert, die an den Azure AD-Mandanten gebunden sind. Informationen zu den erforderlichen Schritten finden Sie unter [Option 1](#peruser).
* Der bedingte Zugriff ermöglicht eine differenziertere Kontrolle darüber, wie ein zweiter Faktor höher gestuft werden soll. Er kann die Zuweisung von MFA nur an VPN ermöglichen und andere Anwendungen ausschließen, die an den Azure AD-Mandanten gebunden sind. Informationen zu den erforderlichen Schritten finden Sie unter [Option 2](#conditional).