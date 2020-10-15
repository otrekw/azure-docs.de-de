---
title: Probleme bei der Anmeldung bei benutzerdefiniert entwickelten Anwendungen | Microsoft-Dokumentation
description: Allgemeine Fehler, die dazu führen können, dass Sie sich nicht bei einer mit Azure AD entwickelten Anwendung anmelden können
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 07/11/2017
ms.author: kenwith
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: cdfc2a39c6bd3b68df7feb978d2548ad67631235
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84759129"
---
# <a name="problems-signing-in-to-a-custom-developed-application"></a>Probleme bei der Anmeldung bei benutzerdefiniert entwickelten Anwendungen

Es gibt mehrere Fehler, die dazu führen können, dass Sie sich nicht einer App anmelden können. Der Hauptgrund für das Auftreten dieses Problems sind falsch konfigurierte Apps.

## <a name="errors-related-to--misconfigured-apps"></a>Fehler im Zusammenhang mit falsch konfigurierten Apps

* Stellen Sie sicher, dass die Konfigurationen im Portal mit der App übereinstimmen. Vergleichen Sie insbesondere Client-/Anwendungs-ID, Antwort-URLs, Clientgeheimnisse/-schlüssel und App-ID-URI.

* Vergleichen Sie die Ressource, auf die Sie zugreifen möchten, im Code mit den konfigurierten Berechtigungen auf der Registerkarte **Erforderliche Ressourcen**, um sicherzustellen, dass Sie nur Ressourcen anfordern, die Sie konfiguriert haben.

* Weitere Informationen zu ähnlichen Fehlern oder Problemen finden Sie unter [Azure AD bei Stack Overflow](https://stackoverflow.com/questions/tagged/azure-active-directory).

## <a name="next-steps"></a>Nächste Schritte

[Entwicklerhandbuch zu Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Genehmigen und Integrieren von Apps in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[Berechtigungen und Zustimmung im Microsoft Identity Platform-Endpunkt](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD bei Stack Overflow](https://stackoverflow.com/questions/tagged/azure-active-directory>)
