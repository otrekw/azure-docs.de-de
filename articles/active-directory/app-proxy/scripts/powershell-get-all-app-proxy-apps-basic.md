---
title: PowerShell-Beispiel – Auflisten von grundlegenden Informationen zu Anwendungsproxy-Apps
description: PowerShell-Beispiel, das Anwendungen für den Azure Active Directory-Anwendungsproxy (Azure AD) zusammen mit der Anwendungs-ID (AppID), dem Namen (Display Name) und der Objekt-ID (ObjId) auflistet.
services: active-directory
author: kenwith
manager: mtillman
ms.service: active-directory
ms.subservice: app-proxy
ms.workload: identity
ms.topic: sample
ms.date: 04/29/2021
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: 69264b70f814b238466e0e5d77e4af8b2b076412
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108293666"
---
# <a name="get-all-application-proxy-apps-and-list-basic-information"></a>Abrufen aller Anwendungen für den Anwendungsproxy und Auflisten grundlegender Informationen

Dieses PowerShell-Skriptbeispiel listet Informationen über alle Anwendungen für den Azure Active Directory-Anwendungsproxy (Azure AD) zusammen mit der Anwendungs-ID (AppID), dem Namen (Display Name) und der Objekt-ID (ObjId) auf.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Für dieses Beispiel ist das [Azure AD PowerShell V2-Modul für Graph](/powershell/azure/active-directory/install-adv2) (AzureAD) oder das [Azure AD PowerShell V2-Modul in der Vorschauversion für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) (AzureADPreview) erforderlich.
## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-appproxy-apps-basic.ps1 "Get all Application Proxy apps")]

## <a name="script-explanation"></a>Erläuterung des Skripts

| Get-Help | Notizen |
|---|---|
|[Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) | Ruft einen Dienstprinzipal ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](/powershell/azure/active-directory/overview).

Weitere PowerShell-Beispiele für den Anwendungsproxy finden Sie unter [Azure AD PowerShell-Beispiele für Azure AD-Anwendungsproxy](../application-proxy-powershell-samples.md).
