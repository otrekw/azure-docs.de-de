---
title: 'PowerShell-Beispiel: Exportieren von Apps mit Geheimnissen und Zertifikaten, die nach dem erforderlichen Datum im Azure Active Directory-Mandanten ablaufen'
description: PowerShell-Beispiel, das alle Apps im Azure Active Directory-Mandanten mit Geheimnissen und Zertifikaten exportiert, die nach dem erforderlichen Datum für die angegebenen Apps ablaufen
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
ms.openlocfilehash: 3572f481cc2cbcb1df73b33eb2543e32256ad9fb
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102583263"
---
# <a name="export-apps-with-secrets-and-certificates-expiring-beyond-the-required-date"></a>Exportieren von Apps mit Geheimnissen und Zertifikaten, die nach dem erforderlichen Datum ablaufen

Mit diesem PowerShell-Skriptbeispiel werden alle App-Geheimnisse und -Zertifikate, die nach dem erforderlichen Datum für die angegebenen Apps ablaufen, aus Ihrem Verzeichnis in eine CSV-Datei exportiert.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

Für dieses Beispiel ist das [Azure AD PowerShell V2-Modul für Graph](/powershell/azure/active-directory/install-adv2) (AzureAD) oder das [Azure AD PowerShell V2-Modul in der Vorschauversion für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) (AzureADPreview) erforderlich.

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-management/export-apps-with-secrets-beyond-required.ps1 "Exports all apps with secrets and certificates expiring beyond the required date for the specified apps in your directory.")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Mit dem Befehl „Add-Member“ werden die Spalten in der CSV-Datei erstellt.
Sie können die Variable „$Path“ direkt in PowerShell mit einem CSV-Dateipfad ändern, falls der Export nicht interaktiv sein soll.

| Get-Help | Notizen |
|---|---|
| [Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication?view=azureadps-2.0&preserve-view=true) | Ruft eine Anwendung aus Ihrem Verzeichnis ab. |
| [Get-AzureADApplicationOwner](/powershell/module/azuread/Get-AzureADApplicationOwner?view=azureadps-2.0&preserve-view=true) | Ruft die Besitzer einer Anwendung aus Ihrem Verzeichnis ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](/powershell/azure/active-directory/overview).

Weitere PowerShell-Beispiele für die Anwendungsverwaltung finden Sie unter [Azure AD PowerShell-Beispiele für die Anwendungsverwaltung](../app-management-powershell-samples.md).
