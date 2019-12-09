---
title: Aufrufen und Entfernen von Konten aus dem Tokencache mithilfe von MSAL für Java (MSAL4j)
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie den Tokencache mit der Microsoft Authentication Library für Java löschen anzeigen und Konten daraus entfernen können.
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
ms.openlocfilehash: fa6c4c53d04f227db7a9a83946182c109dc06d39
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/24/2019
ms.locfileid: "74452485"
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
