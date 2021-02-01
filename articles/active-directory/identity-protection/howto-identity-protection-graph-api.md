---
title: Microsoft Graph PowerShell SDK und Azure Active Directory Identity Protection
description: In diesem Artikel erfahren Sie, wie Sie Risikoerkennungen und zugehörige Informationen in Microsoft Graph über Azure Active Directory abfragen.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: how-to
ms.date: 01/25/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2db8cfe652c0fca4b68b00d846e345c1b60cd05d
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98880235"
---
# <a name="azure-active-directory-identity-protection-and-the-microsoft-graph-powershell-sdk"></a>Azure Active Directory Identity Protection und das Microsoft Graph PowerShell SDK

Microsoft Graph ist der einheitliche API-Endpunkt von Microsoft und stellt die [Azure Active Directory Identity Protection](./overview-identity-protection.md)-APIs zur Verfügung. In diesem Artikel wird beschrieben, wie Sie mit dem [Microsoft Graph PowerShell SDK](/graph/powershell/get-started) Details zu Risikobenutzern mithilfe von PowerShell abrufen. Organisationen, die die Microsoft Graph-APIs direkt abfragen möchten, können den Artikel [Tutorial: Identifizieren und Beheben von Risiken mithilfe von Microsoft Graph-APIs](/graph/tutorial-riskdetection-api) lesen, um mit dieser Journey zu beginnen.


## <a name="connect-to-microsoft-graph"></a>Herstellen einer Verbindung mit Microsoft Graph

Zum Zugreifen auf die Identity Protection-Daten über Microsoft Graph werden vier Schritte ausgeführt:

- [Ein Zertifikat erstellen](#create-a-certificate)
- [Erstellen einer neuen App-Registrierung](#create-a-new-app-registration)
- [Konfigurieren von API-Berechtigungen](#configure-api-permissions)
- [Konfigurieren von gültigen Anmeldeinformationen](#configure-a-valid-credential)

### <a name="create-a-certificate"></a>Erstellen eines Zertifikats

In einer Produktionsumgebung würden Sie ein Zertifikat Ihrer Produktionszertifizierungsstelle verwenden, aber in diesem Beispiel verwenden wir ein selbstsigniertes Zertifikat. Erstellen und exportieren Sie das Zertifikat mithilfe der folgenden PowerShell-Befehle.

```powershell
$cert = New-SelfSignedCertificate -Subject "CN=MSGraph_ReportingAPI" -CertStoreLocation "Cert:\CurrentUser\My" -KeyExportPolicy Exportable -KeySpec Signature -KeyLength 2048 -KeyAlgorithm RSA -HashAlgorithm SHA256
Export-Certificate -Cert $cert -FilePath "C:\Reporting\MSGraph_ReportingAPI.cer"
```

### <a name="create-a-new-app-registration"></a>Erstellen einer neuen App-Registrierung

1. Navigieren Sie im Azure-Portal zu **Azure Active Directory** > **App-Registrierungen**.
1. Wählen Sie **Neue Registrierung** aus.
1. Führen Sie auf der Seite **Erstellen** die folgenden Schritte aus:
   1. Geben Sie im Textfeld **Name** einen Namen für Ihre Anwendung ein (Beispiel: die Azure AD-API für die Risikoerkennung).
   1. Wählen Sie unter **Unterstützte Kontotypen** den Kontotyp aus, der die APIs verwenden wird.
   1. Wählen Sie **Registrieren**.
1. Notieren Sie sich die **Anwendungs-ID (Client-ID)** und die **Verzeichnis-ID (Mandanten-ID)** , da Sie diese Elemente später benötigen.

### <a name="configure-api-permissions"></a>Konfigurieren von API-Berechtigungen

In diesem Beispiel konfigurieren wir die Anwendungsberechtigungen so, dass dieses Beispiel unbeaufsichtigt verwendet werden kann. Wenn Sie einem Benutzer Berechtigungen erteilen, der angemeldet wird, wählen Sie stattdessen delegierte Berechtigungen aus. Weitere Informationen zu verschiedenen Berechtigungstypen finden Sie im Artikel [Berechtigungen und Zustimmung im Microsoft Identity Platform-Endpunkt](../develop/v2-permissions-and-consent.md#permission-types).

1. Klicken Sie in der von Ihnen erstellten **Anwendung** auf **API-Berechtigungen**.
1. Klicken Sie auf der Seite **Konfigurierte Berechtigungen** in der Symbolleiste oben auf **Berechtigung hinzufügen**.
1. Klicken Sie auf der Seite **API-Zugriff hinzufügen** auf **API auswählen**.
1. Wählen Sie auf der Seite **API auswählen** **Microsoft Graph** aus, und klicken Sie dann auf **Auswählen**.
1. Auf der Seite **API-Berechtigungen anfordern**: 
   1. Wählen Sie **Anwendungsberechtigungen**.
   1. Aktivieren Sie die Kontrollkästchen neben `IdentityRiskEvent.Read.All` und `IdentityRiskyUser.Read.All`.
   1. Wählen Sie **Berechtigungen hinzufügen** aus.
1. Klicken Sie auf **Grant admin consent for domain** (Administratoreinwilligung für Domäne gewähren). 

### <a name="configure-a-valid-credential"></a>Konfigurieren von gültigen Anmeldeinformationen

1. Klicken Sie in der von Ihnen erstellten **Anwendung** auf **Certificates & secrets** (Zertifikate und Geheimnisse).
1. Wählen Sie unter **Zertifikate** die Option **Zertifikat hochladen** aus.
   1. Wählen Sie im daraufhin geöffneten Fenster das zuvor exportierte Zertifikat aus.
   1. Wählen Sie **Hinzufügen**.
1. Notieren Sie sich den **Fingerabdruck** des Zertifikats, da Sie diese Informationen im nächsten Schritt benötigen.

## <a name="list-risky-users-using-powershell"></a>Auflisten von Risikobenutzern mithilfe von PowerShell

Damit Microsoft Graph abgefragt werden kann, müssen wir im PowerShell-Fenster das `Microsoft.Graph`-Modul mithilfe des Befehls `Install-Module Microsoft.Graph` installieren.

Ändern Sie die folgenden Variablen so, dass sie die in den vorherigen Schritten generierten Informationen enthalten, und führen Sie sie dann als Ganzes aus, um mithilfe von PowerShell Details zu Risikobenutzern abzurufen.

```powershell
$ClientID       = "<your client ID here>"        # Application (client) ID gathered when creating the app registration
$tenantdomain   = "<your tenant domain here>"    # Directory (tenant) ID gathered when creating the app registration
$Thumbprint     = "<your client secret here>"    # Certificate thumbprint gathered when configuring your credential

Select-MgProfile -Name "beta"
  
Connect-MgGraph -ClientId $ClientID -TenantId $tenantdomain -CertificateThumbprint $Thumbprint

Get-MgRiskyUser -All
```

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit dem Microsoft Graph PowerShell SDK](/graph/powershell/get-started)
- [Tutorial: Identifizieren und Beheben von Risiken mithilfe von Microsoft Graph-APIs](/graph/tutorial-riskdetection-api)
- [Übersicht über Microsoft Graph](https://developer.microsoft.com/graph/docs)
- [Ohne Benutzer zugreifen](/graph/auth-v2-service)
- [Azure AD Identity Protection Service Root](/graph/api/resources/identityprotectionroot)
- [Azure Active Directory Identity Protection](./overview-identity-protection.md)
