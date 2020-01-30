---
title: Benutzerdefinierte Tokencacheserialisierung (MSAL4j)
titleSuffix: Microsoft identity platform
description: In diesem Artikel erfahren Sie, wie Sie den Tokencache für MSAL für Java serialisieren.
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/07/2019
ms.author: sagonzal
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: bcb34d83365112b97769186ad74dfd762b05c2e8
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "76696162"
---
# <a name="custom-token-cache-serialization-in-msal-for-java"></a>Benutzerdefinierte Tokencacheserialisierung in MSAL für Java

Sie müssen die Serialisierung anpassen, um den Tokencache zwischen den Instanzen Ihrer Anwendung beizubehalten. Die Java-Klassen und -Schnittstellen, die an der Serialisierung über den Tokencache beteiligt sind, weisen die folgenden Typen auf:

- [ITokenCache](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCache.html):  Schnittstelle, die den Sicherheitstokencache darstellt
- [ITokenCacheAccessAspect](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCacheAccessAspect.html): Schnittstelle, die den Vorgang der Codeausführung vor und nach dem Zugriff darstellt. Dazu überschreiben (@Override) Sie *beforeCacheAccess* und *afterCacheAccess* mit der Logik für die Serialisierung und Deserialisierung des Caches.
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
