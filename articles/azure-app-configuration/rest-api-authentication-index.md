---
title: 'Azure App Configuration-REST-API: Authentifizierung'
description: Referenzseiten für die Authentifizierung über die Azure App Configuration-REST-API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 21a43a005b78c8916d06e97ca9d2ba21d5a585a3
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423760"
---
# <a name="authentication"></a>Authentifizierung

Alle HTTP-Anforderungen müssen authentifiziert werden. Die folgenden Authentifizierungsschemas werden unterstützt.

## <a name="hmac"></a>HMAC

Die [HMAC-Authentifizierung](./rest-api-authentication-hmac.md) verwendet ein zufällig generiertes Geheimnis zum Signieren von Anforderungspayloads. Informationen zur Autorisierung von Anforderungen, die diese Authentifizierungsmethode verwenden, finden Sie im Abschnitt [HMAC-Autorisierung](./rest-api-authorization-hmac.md).

## <a name="azure-active-directory"></a>Azure Active Directory

Die [Azure Active Directory-Authentifizierung](/azure/active-directory/authentication/overview-authentication) verwendet ein Bearertoken, das aus Azure Active Directory zum Authentifizieren von Anforderungen abgerufen wird. Informationen zur Autorisierung von Anforderungen, die diese Authentifizierungsmethode verwenden, finden Sie im Abschnitt [Azure Active Directory-Autorisierung](./rest-api-authorization-azure-ad.md).
