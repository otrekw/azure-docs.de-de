---
title: Erhalten der Einwilligung für verschiedene Ressourcen (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie ein Benutzer mit der Microsoft Authentication Library für .NET (MSAL.NET) die Vorabzustimmung für mehrere Ressourcen abrufen kann.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 04/30/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: af9dbcbb6c32969b66cd446db634e83ce81f3c92
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98754728"
---
# <a name="user-gets-consent-for-several-resources-using-msalnet"></a>Benutzer erhält per MSAL.NET die Zustimmung für mehrere Ressourcen
Microsoft Identity Platform lässt das gleichzeitige Abrufen eines Tokens für mehrere Ressourcen nicht zu. Wenn Sie die Microsoft Authentication Library für .NET (MSAL.NET) verwenden, darf der Bereichsparameter in der Tokenabrufmethode nur Bereiche für eine einzelne Ressource enthalten. Sie können allerdings vorab mehreren Ressourcen zustimmen, indem Sie mit der Generatormethode `.WithExtraScopeToConsent` zusätzliche Bereiche angeben.

> [!NOTE]
> Das Abrufen der Zustimmung für mehrere Ressourcen funktioniert für Microsoft Identity Platform, nicht aber für Azure AD B2C. Azure AD B2C unterstützt nur die Administrator- und nicht die Benutzereinwilligung.

Beispiel: Sie verfügen über zwei Ressourcen mit jeweils zwei Bereichen:

- https:\//mytenant.onmicrosoft.com/customerapi (mit den beiden Bereichen `customer.read` und `customer.write`)
- https:\//mytenant.onmicrosoft.com/vendorapi (mit den beiden Bereichen `vendor.read` und `vendor.write`)

Verwenden Sie dann den `.WithExtraScopeToConsent`-Modifizierer, der über den im folgenden Beispiel gezeigten *extraScopesToConsent*-Parameter verfügt:

```csharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.onmicrosoft.com/customerapi/customer.read",
  "https://mytenant.onmicrosoft.com/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

Auf diese Weise erhalten Sie ein Zugriffstoken für die erste Web-API. Wenn Sie dann auf die zweite Web-API zugreifen müssen, können Sie das Token automatisch aus dem Tokencache abrufen:

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```
