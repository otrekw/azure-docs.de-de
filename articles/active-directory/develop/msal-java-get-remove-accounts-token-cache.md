---
title: Aufrufen und Entfernen von Konten aus dem Tokencache mithilfe von MSAL für Java (MSAL4j)
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie den Tokencache mit der Microsoft Authentication Library für Java löschen anzeigen und Konten daraus entfernen können.
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/07/2019
ms.author: sagonzal
ms.reviewer: navyasri.canumalla
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: a40c5f2e272a44727d4da91600093e8dc691a95f
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74916637"
---
# <a name="get-and-remove-accounts-from-the-token-cache-using-msal-for-java"></a>Aufrufen und Entfernen von Konten aus dem Tokencache mithilfe von MSAL für Java

MSAL für Java stellt standardmäßig einen Tokencache im Arbeitsspeicher bereit. Der In-Memory-Tokencache bleibt für die Lebensdauer der Anwendungsinstanz gültig.

## <a name="see-which-accounts-are-in-the-cache"></a>Anzeigen, welche Konten sich im Cache befinden

Sie können überprüfen, welche Konten sich im Cache befinden, indem Sie `PublicClientApplication.getAccounts()` aufrufen, wie im folgenden Beispiel gezeigt:

```java
PublicClientApplication pca = new PublicClientApplication.Builder(
                labResponse.getAppId()).
                authority(TestConstants.ORGANIZATIONS_AUTHORITY).
                build();

Set<IAccount> accounts = pca.getAccounts().join();
```

## <a name="remove-accounts-from-the-cache"></a>Entfernen von Konten aus dem Cache

Um ein Konto aus dem Cache zu entfernen, suchen Sie das Konto, das entfernt werden muss, und rufen Sie dann `PublicClientApplicatoin.removeAccount()` auf, wie im folgenden Beispiel gezeigt:

```java
Set<IAccount> accounts = pca.getAccounts().join();

IAccount accountToBeRemoved = accounts.stream().filter(
                x -> x.username().equalsIgnoreCase(
                        UPN_OF_USER_TO_BE_REMOVED)).findFirst().orElse(null);

pca.removeAccount(accountToBeRemoved).join();
```

## <a name="learn-more"></a>Weitere Informationen

Wenn Sie MSAL für Java verwenden, informieren Sie sich über [Benutzerdefinierte Tokencache-Serialisierung in MSAL für Java](msal-java-token-cache-serialization.md).
