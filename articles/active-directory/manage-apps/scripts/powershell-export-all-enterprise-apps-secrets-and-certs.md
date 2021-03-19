---
title: 'PowerShell-Beispiel: Exportieren von Geheimnissen und Zertifikaten für Unternehmens-Apps im Azure Active Directory-Mandanten'
description: PowerShell-Beispiel, das alle Geheimnisse und Zertifikate für die angegebenen Unternehmens-Apps im Azure Active Directory-Mandanten exportiert
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 03/09/2021
ms.author: kenwith
ms.reviewer: mifarca
ms.openlocfilehash: 20caefe74a7c047fb8690bb1d9e6f4eb9da7e9b7
ms.sourcegitcommit: b572ce40f979ebfb75e1039b95cea7fce1a83452
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "102635194"
---
# <a name="export-secrets-and-certificates-for-enterprise-apps"></a>Exportieren von Geheimnissen und Zertifikaten für Unternehmens-Apps
Mit diesem PowerShell-Skriptbeispiel werden alle Geheimnisse, Zertifikate und Besitzer für die angegebenen Unternehmens-Apps aus Ihrem Verzeichnis in eine CSV-Datei exportiert.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

Für dieses Beispiel ist das [Azure AD PowerShell V2-Modul für Graph](/powershell/azure/active-directory/install-adv2) (AzureAD) oder das [Azure AD PowerShell V2-Modul in der Vorschauversion für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) (AzureADPreview) erforderlich.

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-management/export-all-enterprise-apps-secrets-and-certs.ps1 "Exports all secrets and certificates for the specified enterprise apps in your directory.")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Mit dem Befehl „Add-Member“ werden die Spalten in der CSV-Datei erstellt.
Sie können die Variable „$Path“ direkt in PowerShell mit einem CSV-Dateipfad ändern, falls der Export nicht interaktiv sein soll.

| Get-Help | Notizen |
|---|---|
| [Get-AzureADServicePrincipal](/powershell/module/azuread/Get-azureADServicePrincipal?view=azureadps-2.0&preserve-view=true) | Ruft eine Unternehmensanwendung aus Ihrem Verzeichnis ab. |
| [Get-AzureADServicePrincipalOwner](/powershell/module/azuread/Get-AzureADServicePrincipalOwner?view=azureadps-2.0&preserve-view=true) | Ruft den Besitzer einer Unternehmensanwendung aus Ihrem Verzeichnis ab. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](/powershell/azure/active-directory/overview).

Weitere PowerShell-Beispiele für die Anwendungsverwaltung finden Sie unter [Azure AD PowerShell-Beispiele für die Anwendungsverwaltung](../app-management-powershell-samples.md).
