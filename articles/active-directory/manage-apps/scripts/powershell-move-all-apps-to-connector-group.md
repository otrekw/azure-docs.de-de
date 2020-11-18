---
title: PowerShell-Beispiel – Verschieben von Anwendungsproxy-Apps in eine andere Gruppe
description: PowerShell-Beispiel für den Azure Active Directory-Anwendungsproxy (Azure AD), das verwendet wird, um alle derzeit einer Connectorgruppe zugewiesenen Anwendungen in eine andere Connectorgruppe zu verschieben.
services: active-directory
author: kenwith
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 12/05/2019
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: 24bbb0d6133ecf95debb97b673b984f18481803e
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94653877"
---
# <a name="move-all-apps-assigned-to-a-connector-group-to-another-connector-group"></a>Verschieben aller einer Connectorgruppe zugewiesenen Apps in eine andere Connectorgruppe

In diesem PowerShell-Skriptbeispiel werden alle Anwendungen für den Azure Active Directory-Anwendungsproxy (Azure AD), die derzeit einer Connectorgruppe zugewiesen sind, in eine andere Connectorgruppe verschoben.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Für dieses Beispiel ist das [Azure AD PowerShell V2-Modul für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) (AzureAD) oder das [Azure AD PowerShell V2-Modul in der Vorschauversion für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (AzureADPreview) erforderlich.

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/move-all-apps-to-a-connector-group.ps1 "Move all apps assigned to a connector group to another connector group")]

## <a name="script-explanation"></a>Erläuterung des Skripts

| Get-Help | Notizen |
|---|---|
|[Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal?view=azureadps-2.0) | Ruft einen Dienstprinzipal ab. |
|[Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication?view=azureadps-2.0) | Ruft eine Azure AD-Anwendung ab. |
| [Get-AzureADApplicationProxyConnectorGroup](/powershell/module/azuread/get-azureadapplicationproxyconnectorgroup?view=azureadps-2.0) | Ruft eine Liste aller Connectorgruppen oder (bei entsprechender Angabe) Details der angegebenen Connectorgruppe ab. |
| [Set-AzureADApplicationProxyConnectorGroup](/powershell/module/azuread/set-azureadapplicationproxyapplicationconnectorgroup?view=azureadps-2.0) | Weist die angegebenen Connectorgruppe einer angegebenen Anwendung zu.|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](/powershell/azure/active-directory/overview?view=azureadps-2.0).

Weitere PowerShell-Beispiele für den Anwendungsproxy finden Sie unter [Azure AD PowerShell-Beispiele für Azure AD-Anwendungsproxy](../application-proxy-powershell-samples.md).