---
author: mmacy
ms.service: active-directory
ms.subservice: develop
ms.topic: include
ms.date: 07/27/2020
ms.author: marsma
ms.openlocfilehash: 99ca8e45da27f5bce5258d55f46bcfa25b358239
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87311539"
---
> [!IMPORTANT]
> MSAL.js 2.0 unterstützt derzeit kein Azure AD B2C zur Verwendung mit dem PKCE-Autorisierungscodeflow. Zum gegenwärtigen Zeitpunkt empfiehlt Azure AD B2C die Verwendung des impliziten Flows, wie in folgendem Tutorial beschrieben: [Tutorial: Registrieren einer Anwendung][implicit-flow]. Informationen zum Fortschritt bei diesem Problem finden Sie im [MSAL.js-Wiki][msal-wiki].

[github-issue]: https://github.com/AzureAD/microsoft-authentication-library-for-js/issues/1795
[implicit-flow]: ../articles/active-directory-b2c/tutorial-register-applications.md
[msal-wiki]: https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/MSAL-browser-B2C-CORS-issue
