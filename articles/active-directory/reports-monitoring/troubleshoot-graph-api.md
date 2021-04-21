---
title: Problembehandlung von Fehlern in der Azure Active Directory-Berichterstellungs-API | Microsoft Docs
description: Stellt eine Lösung vor, wenn beim Aufrufen der Azure Active Directory-Berichterstellungs-APIs Fehler auftreten.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 0030c5a4-16f0-46f4-ad30-782e7fea7e40
ms.service: active-directory
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 445514297550210d80cd50895201d1129fac4f20
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107533707"
---
# <a name="troubleshoot-errors-in-azure-active-directory-reporting-api"></a>Problembehandlung von Fehlern in der Azure Active Directory-Berichterstellungs-API

In diesem Artikel werden die häufigsten Fehlermeldungen, die beim Zugreifen auf Aktivitätsberichte über die Microsoft Graph-API auftreten können, sowie Schritte zu deren Behebung aufgeführt.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>500 HTTP: Interner Serverfehler beim Zugriff auf den Microsoft Graph V2-Endpunkt.

Der Microsoft Graph v2-Endpunkt wird zurzeit nicht unterstützt. Stellen Sie sicher, dass Sie auf die Aktivitätsprotokolle mit dem Microsoft Graph v1-Endpunkt zugreifen.

### <a name="error-neither-tenant-is-b2c-or-tenant-doesnt-have-premium-license"></a>Fehler: Kein Mandant ist B2C, oder der Mandant besitzt keine Premium-Lizenz.

Für den Zugriff auf Anmeldeberichte ist eine Azure Active Directory Premium 1-Lizenz (P1) erforderlich. Wenn diese Fehlermeldung beim Zugriff auf Anmeldungen angezeigt wird, stellen Sie sicher, dass Ihr Mandant mit einer Azure AD P1-Lizenz lizenziert ist.

### <a name="error-user-is-not-in-the-allowed-roles"></a>Fehler: Der Benutzer gehört nicht den zulässigen Rollen an. 

Wenn Sie diese Fehlermeldung sehen, während Sie versuchen, über die API auf Überwachungsprotokolle oder Anmeldungen zuzugreifen, stellen Sie sicher, dass Ihr Konto Teil der Rolle **Sicherheitsleseberechtigter** oder **Berichtsleser** in Ihrem Azure Active Directory-Mandanten ist. 

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Error: Der Anwendung fehlt die AAD-Berechtigung „Verzeichnisdaten lesen“ 

Führen Sie die Schritte unter [Voraussetzungen für den Zugriff auf die Azure Active Directory-Berichterstellungs-API](howto-configure-prerequisites-for-reporting-api.md) aus, um sicherzustellen, dass die Anwendung mit dem richtigen Berechtigungssatz ausgeführt wird. 

### <a name="error-application-missing-microsoft-graph-api-read-all-audit-log-data-permission"></a>Error: Der Anwendung fehlt die Berechtigung „Alle Überwachungsprotokolldaten lesen“ der Microsoft Graph-API

Führen Sie die Schritte unter [Voraussetzungen für den Zugriff auf die Azure Active Directory-Berichterstellungs-API](howto-configure-prerequisites-for-reporting-api.md) aus, um sicherzustellen, dass die Anwendung mit dem richtigen Berechtigungssatz ausgeführt wird. 

## <a name="next-steps"></a>Nächste Schritte

[Verwenden der Referenz zur Überwachungs-API](/graph/api/resources/directoryaudit)
[Verwenden der Referenz zur Anmeldeaktivitätsbericht-API](/graph/api/resources/signin)