---
title: 'PowerShell-Beispiel: Anwendungsproxy-Apps ohne Zertifikat'
description: Hier finden Sie ein PowerShell-Beispiel, das alle Anwendungen für den Azure Active Directory-Anwendungsproxy (Azure AD) auflistet, die benutzerdefinierte Domänen verwenden, für die jedoch kein gültiges TLS/SSL-Zertifikat hochgeladen wurde.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97dc46a34e1bb5bb85dcb771a9720a19d356e6f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481210"
---
# <a name="get-all-azure-ad-proxy-application-apps-published-with-no-certificate-uploaded"></a>Abrufen aller Apps für den Azure AD-Anwendungsproxy, die ohne ein hochgeladenes Zertifikat veröffentlicht wurden

Dieses PowerShell-Skript listet alle Anwendungen für den Azure Active Directory-Anwendungsproxy (Azure AD) auf, die benutzerdefinierte Domänen verwenden, für die jedoch kein gültiges TLS/SSL-Zertifikat hochgeladen wurde.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Für dieses Beispiel ist das [Azure AD PowerShell V2-Modul für Graph](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) (AzureAD) oder das [Azure AD PowerShell V2-Modul für Graph in der Vorschauversion](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (AzureADPreview) erforderlich.

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-custom-domain-no-cert.ps1 "Get all Azure AD Proxy application apps published with no certificate uploaded")]

## <a name="script-explanation"></a>Erläuterung des Skripts

| Get-Help | Notizen |
|---|---|
|[Get-AzureADServicePrincipal](https://docs.microsoft.com/powershell/module/azuread/get-azureadserviceprincipal?view=azureadps-2.0) | Ruft einen Dienstprinzipal ab. |
|[Get-AzureADApplication](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplication?view=azureadps-2.0) | Ruft eine Azure AD-Anwendung ab. |
|[Get-AzureADApplicationProxyApplication](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplicationproxyapplication?view=azureadps-2.0) | Ruft eine für den Anwendungsproxy in Azure AD konfigurierte Anwendung ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

Weitere PowerShell-Beispiele für den Anwendungsproxy finden Sie unter [Azure AD PowerShell-Beispiele für Azure AD-Anwendungsproxy](../application-proxy-powershell-samples.md).