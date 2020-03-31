---
title: Einrichten der Registrierung und Anmeldung mit einem QQ-Konto mithilfe von Azure Active Directory B2C
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit QQ-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: b0f15124c64e5cca54112987d486ddadaca79452
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78187986"
---
# <a name="set-up-sign-up-and-sign-in-with-a-qq-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem QQ-Konto mithilfe von Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-qq-application"></a>Erstellen einer QQ-Anwendung

Um ein QQ-Konto als Identitätsanbieter in Azure Active Directory B2C (Azure AD B2C) verwenden zu können, müssen Sie in Ihrem Mandanten eine Anwendung erstellen, die es darstellt. Wenn Sie noch über kein QQ-Konto verfügen, können Sie sich unter [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033) registrieren.

### <a name="register-for-the-qq-developer-program"></a>Registrieren beim QQ-Entwicklerprogramm

1. Melden Sie sich beim [QQ-Entwicklerportal](http://open.qq.com) mit den Anmeldeinformationen für Ihr QQ-Konto an.
1. Rufen Sie nach der Anmeldung die Seite [https://open.qq.com/reg](https://open.qq.com/reg) auf, um sich als Entwickler zu registrieren.
1. Wählen Sie **个人** (Einzelner Entwickler) aus.
1. Geben Sie die erforderlichen Informationen ein, und wählen Sie **下一步** (Nächster Schritt) aus.
1. Schließen Sie den E-Mail-Überprüfungsprozess ab. Es dauert einige Tage, bis Ihre Registrierung als Entwickler genehmigt wird.

### <a name="register-a-qq-application"></a>Registrieren einer QQ-Anwendung

1. Wechseln Sie zu [https://connect.qq.com/index.html](https://connect.qq.com/index.html).
1. Wählen Sie **应用管理** (App-Verwaltung) aus.
1. Wählen Sie **创建应用** (App erstellen) aus, und geben Sie die erforderlichen Informationen ein.
1. Geben Sie `https://your-tenant-name.b2clogin.com/your-tenant-name}.onmicrosoft.com/oauth2/authresp` unter **授权回调域** (Rückruf-URL) ein. Wenn Ihr `tenant_name` beispielsweise „contoso“ lautet, legen Sie die URL auf `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp` fest.
1. Wählen Sie **创建应用** (App erstellen) aus.
1. Wählen Sie auf der Bestätigungsseite die Option **应用管理** (App-Verwaltung) aus, um zur Seite für die App-Verwaltung zurückzukehren.
1. Wählen Sie neben der erstellten App die Option **查看** (Anzeigen) aus.
1. Wählen Sie **修改** (Bearbeiten) aus.
1. Kopieren Sie die **APP-ID** und den **APP-SCHLÜSSEL**. Sie benötigen beide Werte, um den Identitätsanbieter Ihrem Mandanten hinzuzufügen.

## <a name="configure-qq-as-an-identity-provider"></a>Konfigurieren von QQ als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **Identitätsanbieter** und dann **QQ (Vorschau)** aus.
1. Geben Sie einen **Namen** ein. Beispiel: *QQ*.
1. Geben Sie für die **Client-ID** die App-ID der QQ-Anwendung ein, die Sie zuvor erstellt haben.
1. Geben Sie das zuvor notierte App-Geheimnis als **Geheimer Clientschlüssel** ein.
1. Wählen Sie **Speichern** aus.
