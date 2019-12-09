---
title: Benutzerdefinierte Tokencacheserialisierung in MSAL für Java
titleSuffix: Microsoft identity platform
description: In diesem Artikel erfahren Sie, wie Sie den Tokencache für MSAL für Java serialisieren.
services: active-directory
documentationcenter: dev-center-name
author: sangonzal
manager: henrikm
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/07/2019
ms.author: sagonzal
ms.reviewer: navyasri.canumalla
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7323a47095c58958503cecf16189ed875fac3b4d
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/24/2019
ms.locfileid: "74452602"
---
# <a name="custom-token-cache-serialization-in-msal-for-java"></a>Benutzerdefinierte Tokencacheserialisierung in MSAL für Java

Sie müssen die Serialisierung anpassen, um den Tokencache zwischen den Instanzen Ihrer Anwendung beizubehalten. Die Java-Klassen und -Schnittstellen, die an der Serialisierung über den Tokencache beteiligt sind, weisen die folgenden Typen auf:

- [ITokenCache](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCache.html):  Schnittstelle, die den Sicherheitstokencache darstellt
- [ITokenCacheAccessAspect](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCacheAccessAspect.html): Schnittstelle, die den Vorgang der Codeausführung vor und nach dem Zugriff darstellt. Dazu überschreiben Sie (@Override) *beforeCacheAccess*und *afterCacheAccess* mit der Logik, die für die Serialisierung und Deserialisierung des Caches verantwortlich ist.
- [ITokenCacheContext](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCacheAccessContext.html): Schnittstelle, die den Kontext darstellt, in dem auf den Tokencache zugegriffen wird. 

Unten sehen Sie eine native Implementierung der benutzerdefinierten Serialisierung der Tokencacheserialisierung bzw. -deserialisierung. Kopieren und fügen Sie dies nicht in eine Produktionsumgebung ein.

```Java
static class TokenPersistence implements ITokenCacheAccessAspect {
String data;

TokenPersistence(String data) {
        this.data = data;
}

@Override
public void beforeCacheAccess(ITokenCacheAccessContext iTokenCacheAccessContext) {
        iTokenCacheAccessContext.tokenCache().deserialize(data);
}

@Override
public void afterCacheAccess(ITokenCacheAccessContext iTokenCacheAccessContext) {
        data = iTokenCacheAccessContext.tokenCache().serialize();
}
```

```Java
// Loads cache from file
String dataToInitCache = readResource(this.getClass(), "/cache_data/serialized_cache.json");

ITokenCacheAccessAspect persistenceAspect = new TokenPersistence(dataToInitCache);

// By setting *TokenPersistence* on the PublicClientApplication, MSAL will call *beforeCacheAccess()* before accessing the cache and *afterCacheAccess()* after accessing the cache. 
PublicClientApplication app = 
PublicClientApplication.builder("my_client_id").setTokenCacheAccessAspect(persistenceAspect).build();
```

## <a name="learn-more"></a>Weitere Informationen

Weitere Informationen finden Sie unter [Aufrufen und Entfernen von Konten aus dem Tokencache mithilfe von MSAL für Java](msal-java-get-remove-accounts-token-cache.md).
