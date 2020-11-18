---
title: 'PowerShell-Beispiel: Auflisten aller Anwendungsproxy-Connectorgruppen'
description: Hier finden Sie ein PowerShell-Beispiel, das alle in Ihrem Verzeichnis enthaltenen Connectorgruppen und Connectors des Anwendungsproxys von Azure Active Directory (Azure AD) auflistet.
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
ms.openlocfilehash: aad28b7b6e44d2f0e9ef8bb5638f8e80010f1284
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94654047"
---
# <a name="get-all-connector-groups-and-connectors-in-the-directory"></a>Abrufen aller Connectorgruppen und Connectors im Verzeichnis

Mit diesem PowerShell-Beispielskript werden alle in Ihrem Verzeichnis enthaltenen Connectorgruppen und Connectors des Anwendungsproxys von Azure Active Directory (Azure AD) aufgelistet.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Für dieses Beispiel ist das [Azure AD PowerShell V2-Modul für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) (AzureAD) oder das [Azure AD PowerShell V2-Modul in der Vorschauversion für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (AzureADPreview) erforderlich.

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-connectors.ps1 "Get all connector groups and connectors in the directory")]

## <a name="script-explanation"></a>Erläuterung des Skripts

| Get-Help | Notizen |
|---|---|
| [Get-AzureADApplicationProxyConnectorGroup](/powershell/module/azuread/get-azureadapplicationproxyconnectorgroup?view=azureadps-2.0) | Ruft eine Liste aller Connectorgruppen oder (bei entsprechender Angabe) Details der angegebenen Connectorgruppe ab. |
| [Get-AzureADApplicationProxyConnectorGroupMembers](/powershell/module/azuread/get-azureadapplicationproxyconnectorgroupmembers?view=azureadps-2.0) | Ruft alle Anwendungsproxyconnectors ab, die den einzelnen Connectorgruppen zugeordnet sind.|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](/powershell/azure/active-directory/overview?view=azureadps-2.0).

Weitere PowerShell-Beispiele für den Anwendungsproxy finden Sie unter [Azure AD PowerShell-Beispiele für Azure AD-Anwendungsproxy](../application-proxy-powershell-samples.md).