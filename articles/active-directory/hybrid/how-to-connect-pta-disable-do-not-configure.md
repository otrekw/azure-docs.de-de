---
title: Deaktivieren der PTA bei Verwendung der Azure AD Connect-Funktion„Nicht konfigurieren“ | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die PTA deaktivieren, wenn Sie die Azure AD Connect-Funktion „Nicht konfigurieren“ verwenden.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.date: 04/20/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f5db99f5e8ed2ea5844acba5500bc94d8fb0db2b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85358343"
---
# <a name="disable-pta-when-using-azure-ad-connect-do-not-configure"></a>Deaktivieren der PTA bei Verwendung der Azure AD Connect-Funktion „Nicht konfigurieren“

Wenn Sie die Passthrough-Authentifizierung (PTA) mit Azure AD Connect verwenden und diese auf „Nicht konfigurieren“ festgelegt haben, können Sie sie deaktivieren. Zum Deaktivieren der PTA können Sie die folgenden Cmdlets verwenden. 

## <a name="prerequisites"></a>Voraussetzungen
Die folgenden Voraussetzungen müssen erfüllt sein:
- Ein Windows-Computer, auf dem der PTA-Agent installiert ist. 
- Der Agent muss die Version 1.5.1742.0 oder höher aufweisen. 
- Ein globales Azure-Administratorkonto, um die PowerShell-Cmdlets zum Deaktivieren der PTA auszuführen.

>[!NOTE]
> Wenn Ihr Agent älter ist, verfügt er möglicherweise nicht über die Cmdlets, die zum Ausführen dieses Vorgangs erforderlich sind. Sie können im Azure-Portal einen neuen Agent abrufen und ihn auf einem beliebigen Windows-Computer installieren und Administratoranmeldeinformationen angeben. (Die Installation des Agents wirkt sich nicht auf den PTA-Status in der Cloud aus.)

> [!IMPORTANT]
> Wenn Sie die Azure Government-Cloud verwenden, müssen Sie den Parameter ENVIRONMENTNAME mit dem folgenden Wert übergeben. 
>
>| Umgebungsname | Cloud |
>| - | - |
>| AzureUSGovernment | US Gov|


## <a name="to-disable-pta"></a>So deaktivieren Sie die PTA
Verwenden Sie in einer PowerShell-Sitzung den folgenden Befehl, um die PTA zu deaktivieren:
1. PS C:\Programme\Microsoft Azure AD Connect Authentication Agent> `Import-Module .\Modules\PassthroughAuthPSModule`
2. `Get-PassthroughAuthenticationEnablementStatus -Feature PassthroughAuth` oder `Get-PassthroughAuthenticationEnablementStatus -Feature PassthroughAuth -EnvironmentName <identifier>`
3. `Disable-PassthroughAuthentication  -Feature PassthroughAuth` oder `Disable-PassthroughAuthentication -Feature PassthroughAuth -EnvironmentName <identifier>`

## <a name="if-you-dont-have-access-to-an-agent"></a>Sie haben keinen Zugriff auf einen Agent

Wenn Sie keinen Agent-Computer haben, können Sie mit dem folgenden Befehl einen Agent installieren.

1. Laden Sie den aktuellen Auth-Agent von der Website „portal.azure.com“ herunter.
2. Installieren Sie das Feature: `.\AADConnectAuthAgentSetup.exe` oder `.\AADConnectAuthAgentSetup.exe ENVIRONMENTNAME=<identifier>`


## <a name="next-steps"></a>Nächste Schritte

- [Benutzeranmeldung mit der Azure Active Directory-Passthrough-Authentifizierung](how-to-connect-pta.md)
