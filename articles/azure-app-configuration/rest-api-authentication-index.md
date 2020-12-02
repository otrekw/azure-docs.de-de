---
title: 'Azure App Configuration-REST-API: Authentifizierung'
description: Referenzseiten für die Authentifizierung über die Azure App Configuration-REST-API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 56416009395ebf8270ad0fa8d141277424dd6d9a
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "96183463"
---
# <a name="authentication"></a>Authentifizierung

Alle HTTP-Anforderungen müssen authentifiziert werden. Die folgenden Authentifizierungsschemas werden unterstützt.

## <a name="hmac"></a>HMAC

Die [HMAC-Authentifizierung](./rest-api-authentication-hmac.md) verwendet ein zufällig generiertes Geheimnis zum Signieren von Anforderungspayloads. Informationen zur Autorisierung von Anforderungen, die diese Authentifizierungsmethode verwenden, finden Sie im Abschnitt [HMAC-Autorisierung](./rest-api-authorization-hmac.md).

## <a name="azure-active-directory"></a>Azure Active Directory

Die [Azure Active Directory-Authentifizierung](../active-directory/authentication/overview-authentication.md) verwendet ein Bearertoken, das aus Azure Active Directory zum Authentifizieren von Anforderungen abgerufen wird. Informationen zur Autorisierung von Anforderungen, die diese Authentifizierungsmethode verwenden, finden Sie im Abschnitt [Azure Active Directory-Autorisierung](./rest-api-authorization-azure-ad.md).