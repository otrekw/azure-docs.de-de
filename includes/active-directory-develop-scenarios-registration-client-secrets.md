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
ms.date: 04/18/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: a5d34ac7eea50b67bd679d8cb8ddecf7ca277abd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "76773384"
---
## <a name="register-secrets-or-certificates"></a>Registrieren von geheimen Schlüsseln oder Zertifikaten

Wie bei jeder vertraulichen Clientanwendung müssen Sie einen geheimen Schlüssel oder ein Zertifikat registrieren. Sie können Ihre geheimen Anwendungsschlüssel entweder über die interaktive Oberfläche im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) oder mithilfe von Befehlszeilentools (wie PowerShell) registrieren.

### <a name="register-client-secrets-by-using-the-application-registration-portal"></a>Registrieren von geheimen Clientschlüsseln über das Anwendungsregistrierungsportal

Die Verwaltung von Clientanmeldeinformationen geschieht auf der Seite **Zertifikate und Geheimnisse** einer Anwendung:

![Seite „Zertifikate und Geheimnisse“](../articles/active-directory/develop/media/quickstart-update-azure-ad-app-preview/credentials-certificates-secrets.png)

- Der (auch als „geheimer Clientschlüssel“ bezeichnete) geheime Anwendungsschlüssel wird von Azure AD während der Registrierung der vertraulichen Clientanwendung generiert. Diese Generierung erfolgt, wenn Sie **Neuer geheimer Clientschlüssel** auswählen. An diesem Punkt müssen Sie die Zeichenfolge des geheimen Schlüssels zur Verwendung in Ihrer App in die Zwischenablage kopieren, bevor Sie **Speichern** auswählen. Diese Zeichenfolge wird anschließend nicht mehr angezeigt.
- Während der Anwendungsregistrierung verwenden Sie die Schaltfläche **Zertifikat hochladen** zum Hochladen des Zertifikats. Azure AD unterstützt nur Zertifikate, die in der Anwendung direkt registriert sind und keinen Zertifikatsketten folgen.

Weitere Informationen finden Sie unter [Schnellstart: Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs | Hinzufügen von Anmeldeinformationen zu Ihrer Anwendung](../articles/active-directory/develop/quickstart-configure-app-access-web-apis.md#add-credentials-to-your-web-application).



### <a name="register-client-secrets-by-using-powershell"></a>Registrieren von geheimen Clientschlüsseln mithilfe von PowerShell

Alternativ können Sie Ihre Anwendung in Azure AD mithilfe von Befehlszeilentools registrieren. Im Beispiel [active-directory-dotnetcore-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) wird gezeigt, wie Sie einen geheimen Anwendungsschlüssel oder ein Zertifikat in einer Azure AD-Anwendung registrieren:

- Weitere Informationen darüber, wie Sie einen geheimen Anwendungsschlüssel registrieren, finden Sie unter [AppCreationScripts/Configure.ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts/Configure.ps1#L190).
- Weitere Informationen darüber, wie Sie ein Zertifikat in einer Anwendung registrieren, finden Sie unter [AppCreationScripts-withCert/Configure.ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts-withCert/Configure.ps1#L162-L178).
