---
title: include file
description: Includedatei für vertrauliche Clientszenario-Landing Pages (Daemon, Web-App, Web-API)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2020
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: a3acdbb93dd20f0b89e4f99d64f5f7a30ce40623
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87102678"
---
## <a name="register-secrets-or-certificates"></a>Registrieren von geheimen Schlüsseln oder Zertifikaten

Wie bei jeder vertraulichen Clientanwendung müssen Sie einen geheimen Schlüssel oder ein Zertifikat registrieren. Sie können Ihre geheimen Anwendungsschlüssel entweder über die interaktive Oberfläche im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) oder mithilfe von Befehlszeilentools (wie PowerShell) registrieren.

### <a name="register-client-secrets-by-using-the-application-registration-portal"></a>Registrieren von geheimen Clientschlüsseln über das Anwendungsregistrierungsportal

Die Verwaltung von Clientanmeldeinformationen geschieht auf der Seite **Zertifikate und Geheimnisse** einer Anwendung:

![Seite „Zertifikate und Geheimnisse“](../articles/active-directory/develop/media/quickstart-update-azure-ad-app-preview/credentials-certificates-secrets.png)

- Sie erstellen einen *geheimen Clientschlüssel*, indem Sie im Azure-Portal bei der Registrierung der App die Option **Neuer geheimer Clientschlüssel** auswählen. Wenn Sie einen geheimen Clientschlüssel erstellen, _müssen_ Sie die Zeichenfolge des geheimen Schlüssels aufzeichnen, bevor Sie den Bereich **Zertifikate und Geheimnisse** verlassen. Die Zeichenfolge des geheimen Schlüssels wird an keiner Stelle mehr angezeigt.
- Während der Anwendungsregistrierung verwenden Sie die Schaltfläche **Zertifikat hochladen** zum Hochladen des Zertifikats. Azure AD unterstützt nur Zertifikate, die in der Anwendung direkt registriert sind und keinen Zertifikatsketten folgen.

Weitere Informationen finden Sie unter [Schnellstart: Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs | Hinzufügen von Anmeldeinformationen zu Ihrer Anwendung](../articles/active-directory/develop/quickstart-configure-app-access-web-apis.md#add-credentials-to-your-web-application).

### <a name="register-client-secrets-by-using-powershell"></a>Registrieren von geheimen Clientschlüsseln mithilfe von PowerShell

Alternativ können Sie Ihre Anwendung in Azure AD mithilfe von Befehlszeilentools registrieren. Im Beispiel [active-directory-dotnetcore-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) wird gezeigt, wie Sie einen geheimen Anwendungsschlüssel oder ein Zertifikat in einer Azure AD-Anwendung registrieren:

- Weitere Informationen darüber, wie Sie einen geheimen Anwendungsschlüssel registrieren, finden Sie unter [AppCreationScripts/Configure.ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts/Configure.ps1#L190).
- Weitere Informationen darüber, wie Sie ein Zertifikat in einer Anwendung registrieren, finden Sie unter [AppCreationScripts-withCert/Configure.ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts-withCert/Configure.ps1#L162-L178).
