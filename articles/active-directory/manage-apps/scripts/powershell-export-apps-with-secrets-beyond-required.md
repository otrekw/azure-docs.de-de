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
ms.openlocfilehash: daeea48758a9f08e7eedbfcaddcde3815f5c1e16
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105729098"
---
# <a name="export-apps-with-secrets-and-certificates-expiring-beyond-the-required-date"></a>Exportieren von Apps mit Geheimnissen und Zertifikaten, die nach dem erforderlichen Datum ablaufen

Mit diesem PowerShell-Skriptbeispiel werden alle App-Registrierungsgeheimnisse und -Zertifikate, die nach einem erforderlichen Zeitraum für die angegebenen Apps ablaufen, nicht interaktiv aus Ihrem Verzeichnis in eine CSV-Datei exportiert.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-management/export-apps-with-secrets-beyond-required.ps1 "Exports all apps with secrets and certificates expiring beyond the required date for the specified apps in your directory.")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript funktioniert nicht interaktiv. Der Administrator, der es verwendet, muss die Werte im Abschnitt „#PARAMETERS TO CHANGE“ durch die eigene App-ID, das Anwendungsgeheimnis, den Mandantennamen, den Ablaufzeitraum für die App-Anmeldeinformationen und den Pfad ersetzen, in den die CSV-Datei exportiert wird.
Dieses Skript verwendet den [OAuth-Flow „Client_Credential“](../../develop/v2-oauth2-client-creds-grant-flow.md). Die Funktion „RefreshToken“ erstellt das Zugriffstoken basierend auf den Werten der Parameter, die vom Administrator geändert wurden.

Mit dem Befehl „Add-Member“ werden die Spalten in der CSV-Datei erstellt.

| Get-Help | Notizen |
|---|---|
| [Invoke-WebRequest](/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7.1&preserve-view=true) | Sendet HTTP- und HTTPS-Anforderungen an eine Webseite oder einen Webdienst. Er analysiert die Antwort und gibt Auflistungen von Links, Bildern und anderen wichtigen HTML-Elementen zurück. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](/powershell/azure/active-directory/overview).

Weitere PowerShell-Beispiele für die Anwendungsverwaltung finden Sie unter [Azure AD PowerShell-Beispiele für die Anwendungsverwaltung](../app-management-powershell-samples.md).
