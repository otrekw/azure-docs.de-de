---
title: 'Azure App Configuration-REST-API: Authentifizierung'
description: Referenzseiten für die Authentifizierung über die Azure App Configuration-REST-API
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 58fba309f4cf7e4fc4364d075500c02e0ed45259
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "96932692"
---
# <a name="authentication"></a>Authentifizierung

Alle HTTP-Anforderungen müssen authentifiziert werden. Die folgenden Authentifizierungsschemas werden unterstützt.

## <a name="hmac"></a>HMAC

Die [HMAC-Authentifizierung](./rest-api-authentication-hmac.md) verwendet ein zufällig generiertes Geheimnis zum Signieren von Anforderungspayloads. Informationen zur Autorisierung von Anforderungen, die diese Authentifizierungsmethode verwenden, finden Sie im Abschnitt [HMAC-Autorisierung](./rest-api-authorization-hmac.md).

## <a name="azure-active-directory"></a>Azure Active Directory

Die [Azure Active Directory-Authentifizierung](../active-directory/authentication/overview-authentication.md) verwendet ein Bearertoken, das aus Azure Active Directory zum Authentifizieren von Anforderungen abgerufen wird. Informationen zur Autorisierung von Anforderungen, die diese Authentifizierungsmethode verwenden, finden Sie im Abschnitt [Azure Active Directory-Autorisierung](./rest-api-authorization-azure-ad.md).