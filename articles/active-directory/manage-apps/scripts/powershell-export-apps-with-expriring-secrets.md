---
title: 'PowerShell-Beispiel: Exportieren von Apps mit ablaufenden Geheimnissen und Zertifikaten im Azure Active Directory-Mandanten'
description: PowerShell-Beispiel, das alle Apps mit ablaufenden Geheimnissen und Zertifikaten für die angegebenen Apps im Azure Active Directory-Mandanten exportiert
services: active-directory
author: iantheninja
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 03/09/2021
ms.author: iangithinji
ms.reviewer: mifarca
ms.openlocfilehash: 7f129e06904497b43eff8a3f0221fb57565ac112
ms.sourcegitcommit: 2654d8d7490720a05e5304bc9a7c2b41eb4ae007
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107375405"
---
# <a name="export-apps-with-expiring-secrets-and-certificates"></a>Exportieren von Apps mit ablaufenden Geheimnissen und Zertifikaten

Mit diesem PowerShell-Skriptbeispiel werden alle App-Registrierungen mit ablaufenden Geheimnissen und Zertifikaten und ihren Besitzern für die angegebenen Apps aus Ihrem Verzeichnis in eine CSV-Datei exportiert.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

Für dieses Beispiel ist das [Azure AD PowerShell V2-Modul für Graph](/powershell/azure/active-directory/install-adv2) (AzureAD) oder das [Azure AD PowerShell V2-Modul in der Vorschauversion für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) (AzureADPreview) erforderlich.

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-management/export-apps-with-expiring-secrets.ps1 "Exports all apps with expiring secrets and certificates for the specified apps in your directory.")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript kann direkt ohne Änderungen verwendet werden. Der Administrator wird nach dem Ablaufdatum und danach gefragt, ob bereits abgelaufene Geheimnisse oder Zertifikate angezeigt werden sollen.

Mit dem Befehl „Add-Member“ werden die Spalten in der CSV-Datei erstellt.
Mit dem Befehl „New-Object“ wird ein Objekt erstellt, das für die Spalten im CSV-Dateiexport verwendet werden soll.
Sie können die Variable „$Path“ direkt in PowerShell mit einem CSV-Dateipfad ändern, falls der Export nicht interaktiv sein soll.

| Get-Help | Notizen |
|---|---|
| [Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication?view=azureadps-2.0&preserve-view=true) | Ruft eine Anwendung aus Ihrem Verzeichnis ab. |
| [Get-AzureADApplicationOwner](/powershell/module/azuread/Get-AzureADApplicationOwner?view=azureadps-2.0&preserve-view=true) | Ruft die Besitzer einer Anwendung aus Ihrem Verzeichnis ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](/powershell/azure/active-directory/overview).

Weitere PowerShell-Beispiele für die Anwendungsverwaltung finden Sie unter [Azure AD PowerShell-Beispiele für die Anwendungsverwaltung](../app-management-powershell-samples.md).
