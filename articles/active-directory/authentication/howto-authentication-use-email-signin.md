---
title: Anmelden mit einer E-Mail-Adresse als alternative Anmelde-ID für Azure Active Directory
description: Hier erfahren Sie, wie Sie die Benutzeranmeldung bei Azure Active Directory über E-Mail-Adressen als alternative Anmelde-ID konfigurieren und aktivieren (Vorschau).
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 05/22/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: scottsta
ms.openlocfilehash: ed317039e683ef36054d5ace612e09ca75dfa11e
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83837309"
---
# <a name="sign-in-to-azure-using-email-as-an-alternate-login-id-preview"></a>Anmelden bei Azure per E-Mail-Adresse als alternativer Anmelde-ID (Vorschauversion)

Viele Organisationen möchten es Benutzern ermöglichen, sich mit denselben Anmeldeinformationen bei Azure anzumelden, die sie auch für ihre lokale Verzeichnisumgebung verwenden. Bei diesem Ansatz, der als Hybridauthentifizierung bezeichnet wird, müssen sich Benutzer nur eine Kombination von Anmeldeinformationen merken.

Einige Organisationen sind aus folgenden Gründen nicht auf die Hybridauthentifizierung umgestiegen:

* Standardmäßig wird der Benutzerprinzipalname (User Principal Name, UPN) von Azure Active Directory (Azure AD) auf den gleichen UPN wie für das lokale Verzeichnis festgelegt.
* Wenn Sie den Azure AD-UPN ändern, stimmen lokale und Azure-Umgebung nicht mehr überein, was zu Problemen mit bestimmten Anwendungen und Diensten führen kann.
* Aufgrund von geschäftlichen oder Konformitätsgründen möchte die Organisation den lokalen UPN nicht für die Anmeldung bei Azure verwenden.

Zur Unterstützung der Umstellung auf die Hybridauthentifizierung können Sie nun Azure AD konfigurieren, damit Benutzer sich mit einer E-Mail-Adresse als alternativer Anmelde-ID in ihrer verifizierten Domäne bei Azure anmelden können. Wenn *Contoso* beispielsweise in *Fabrikam*umbenannt wird, kann jetzt die E-Mail-Adresse als alternative Anmelde-ID anstelle des alten UPN `balas@contoso.com` verwendet werden. Für den Zugriff auf eine Anwendung oder Dienste melden sich Benutzer mit ihrer zugewiesenen E-Mail-Adresse wie `balas@fabrikam.com` bei Azure an.

|     |
| --- |
| Die Anmeldung bei Azure AD mit der E-Mail-Adresse als alternativer Anmelde-ID ist ein öffentliches Vorschaufeature von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

## <a name="overview-of-azure-ad-sign-in-approaches"></a>Übersicht über Ansätze für die Anmeldung bei Azure AD

Benutzerprinzipalnamen (User Principal Names, UPNs) sind eindeutige Bezeichner für ein Benutzerkonto in Ihrem lokalen Verzeichnis und in Azure AD. Jedes Benutzerkonto in einem Verzeichnis wird durch einen UPN wie `balas@contoso.com` dargestellt. Wenn Sie eine lokale Active Directory Domain Services-Umgebung (AD DS) mit Azure AD synchronisieren, muss der Azure AD-UPN standardmäßig so festgelegt werden, dass er dem lokalen UPN entspricht.

In vielen Organisationen ist es kein Problem, den lokalen UPN und den Azure AD-UPN auf den gleichen Namen festzulegen. Wenn sich Benutzer bei Azure-Anwendungen und -Diensten anmelden, verwenden sie ihren Azure AD-UPN. Allerdings können einige Organisationen aufgrund von Geschäftsrichtlinien oder Beeinträchtigungen der Benutzerfreundlichkeit keine übereinstimmenden UPNs für die Anmeldung verwenden.

Organisationen, die keine übereinstimmenden UPNs in Azure AD verwenden können, haben mehrere Möglichkeiten:

* Ein Ansatz besteht darin, den Azure AD-UPN entsprechend den geschäftlichen Anforderungen auf einen anderen Wert festzulegen, z. B. `balas@fabrikam.com`.
    * Allerdings sind nicht alle Anwendungen und Dienste mit der Verwendung unterschiedlicher Werte für den lokalen UPN und den Azure AD-UPN kompatibel.
* Ein besserer Ansatz besteht darin sicherzustellen, dass Azure AD- und lokale UPNs auf denselben Wert festgelegt sind, und Azure AD so konfigurieren, dass Benutzer sich mit Ihrer E-Mail-Adresse als alternativer Anmelde-ID bei Azure anmelden können.

Mit der E-Mail-Adresse als alternativer Anmelde-ID können sich Benutzer weiterhin bei Azure anmelden, indem sie ihren UPN eingeben, sie können sich aber auch mit ihrer E-Mail-Adresse anmelden. Um dies zu unterstützen, definieren Sie eine E-Mail-Adresse im *ProxyAddresses*-Attribut des Benutzers im lokalen Verzeichnis. Dieses *ProxyAddress*-Attribut unterstützt eine oder mehrere E-Mail-Adressen.

## <a name="synchronize-sign-in-email-addresses-to-azure-ad"></a>Synchronisieren von Anmelde-E-Mail-Adressen mit Azure AD

Die Authentifizierung herkömmlicher Active Directory Domain Services (AD DS) oder Active Directory-Verbunddienste (AD FS) erfolgt direkt in Ihrem Netzwerk und wird von Ihrer AD DS-Infrastruktur verarbeitet. Bei der Hybridauthentifizierung können sich Benutzer stattdessen direkt bei Azure AD anmelden.

Zur Unterstützung dieses Hybridauthentifizierungsansatzes synchronisieren Sie Ihre lokale AD DS-Umgebung mithilfe von [Azure AD Connect][azure-ad-connect] mit Azure AD und konfigurieren sie für die Verwendung der Kennworthashsynchronisierung (Password Hash Sync, PHS) oder der Passthrough-Authentifizierung (PTA).

Bei beiden Konfigurationsoptionen übermittelt der Benutzer seinen Benutzernamen und sein Kennwort an Azure AD, das die Anmeldeinformationen überprüft und ein Ticket ausstellt. Wenn sich Benutzer bei Azure AD anmelden, ist es nicht mehr erforderlich, dass Ihre Organisation eine AD FS-Infrastruktur hostet und verwaltet.

![Diagramm der Azure AD-Hybrididentität mit Kennworthashsynchronisierung](media/howto-authentication-use-email-signin/hybrid-password-hash-sync.png)

![Diagramm der Azure AD-Hybrididentität mit Passthrough-Authentifizierung](media/howto-authentication-use-email-signin/hybrid-pass-through-authentication.png)

Eines der Benutzerattribute, das automatisch von Azure AD Connect synchronisiert wird, ist *ProxyAddresses*. Wenn Benutzer eine E-Mail-Adresse besitzen, die in der lokalen AD DS-Umgebung als Teil des *ProxyAddresses*-Attributs definiert ist, wird diese automatisch mit Azure AD synchronisiert. Diese E-Mail-Adresse kann dann direkt beim Azure AD-Anmeldevorgang als alternative Anmelde-ID verwendet werden.

> [!IMPORTANT]
> Nur E-Mail-Adressen in für den Mandanten verifizierten Domänen werden mit Azure AD synchronisiert. Jeder Azure AD-Mandant verfügt über mindestens eine verifizierte Domäne, deren Eigentümer er nachweislich ist, und die eindeutig an ihren Mandanten gebunden ist.
>
> Weitere Informationen finden Sie unter [Hinzufügen und Verifizieren eines benutzerdefinierten Domänennamens in Azure][verify-domain].

Weitere Informationen finden Sie unter [Wählen der richtigen Authentifizierungsmethode für Ihre Azure AD-Hybrididentitätslösung][hybrid-auth-methods].

## <a name="enable-user-sign-in-with-an-email-address"></a>Aktivieren der Benutzeranmeldung mit einer E-Mail-Adresse

Nachdem Benutzer mit angewendetem *ProxyAddresses*-Attribut mithilfe von Azure AD Connect mit Azure AD synchronisiert wurden, müssen Sie die Funktion aktivieren, mit der sich Benutzer per E-Mail-Adresse als alternativer Anmelde-ID für Ihren Mandanten anmelden können. Diese Funktion weist die Azure AD-Anmeldeserver an, den Anmeldenamen nicht nur mit UPN-Werten, sondern auch mit *ProxyAddresses*-Werten für die E-Mail-Adresse zu vergleichen.

Während der Vorschauphase können Sie die Funktion zur Anmeldung per E-Mail-Adresse als alternativer Anmelde-ID derzeit nur mit PowerShell aktivieren. Sie benötigen die Berechtigungen als *Mandantenadministrator*, um die folgenden Schritte auszuführen:

1. Öffnen Sie eine PowerShell-Sitzung als Administrator, und installieren Sie dann das [AzureADPreview][Install-Module]-Modul mithilfe des Cmdlets *Install-Module*:

    ```powershell
    Install-Module AzureADPreview
    ```

    Wenn Sie dazu aufgefordert werden, wählen Sie **Y**, um NuGet zu installieren oder das Modul aus einem nicht vertrauenswürdigen Repository zu installieren.

1. Melden Sie sich bei Ihrem Azure AD-Mandanten als *Mandantenadministrator* an, indem Sie das Cmdlet [Connect-AzureAD][Connect-AzureAD] verwenden:

    ```powershell
    Connect-AzureAD
    ```

    Der Befehl gibt die Informationen zu Ihrem Konto, Ihrer Umgebung und Ihrer Mandanten-ID zurück.

1. Überprüfen Sie, ob die Richtlinie *HomeRealmDiscoveryPolicy* in Ihrem Mandanten bereits vorhanden ist, indem Sie das Cmdlet [Get-AzureADPolicy][Get-AzureADPolicy] wie folgt ausführen:

    ```powershell
    Get-AzureADPolicy | where-object {$_.Type -eq "HomeRealmDiscoveryPolicy"} | fl *
    ```

1. Wenn derzeit keine Richtlinie konfiguriert ist, gibt der Befehl nichts zurück. Wenn eine Richtlinie zurückgegeben wird, können Sie diesen Schritt überspringen und mit dem nächsten Schritt fortfahren, um eine vorhandene Richtlinie zu aktualisieren.

    Um die Richtlinie *HomeRealmDiscoveryPolicy* dem Mandanten hinzuzufügen, verwenden Sie das Cmdlet [New-AzureADPolicy][New-AzureADPolicy], und legen Sie das *AlternateIdLogin*-Attribut auf *"Enabled": true* wie im folgenden Beispiel fest:

    ```powershell
    New-AzureADPolicy -Definition @('{"HomeRealmDiscoveryPolicy" :{"AlternateIdLogin":{"Enabled": true}}}') `
        -DisplayName "BasicAutoAccelerationPolicy" `
        -IsOrganizationDefault $true `
        -Type "HomeRealmDiscoveryPolicy"
    ```

    Wenn die Richtlinie erfolgreich erstellt wurde, gibt der Befehl die Richtlinien-ID zurück, wie in der folgenden Beispielausgabe dargestellt:

    ```powershell
    Id                                   DisplayName                 Type                     IsOrganizationDefault
    --                                   -----------                 ----                     ---------------------
    5de3afbe-4b7a-4b33-86b0-7bbe308db7f7 BasicAutoAccelerationPolicy HomeRealmDiscoveryPolicy True
    ```

1. Wenn bereits eine Richtlinie konfiguriert ist, überprüfen Sie, ob das  *AlternateIdLogin* -Attribut aktiviert ist, wie in der folgenden Beispielrichtlinienausgabe dargestellt:

    ```powershell
    Id : 5de3afbe-4b7a-4b33-86b0-7bbe308db7f7
    OdataType :
    AlternativeIdentifier :
    Definition : {{"HomeRealmDiscoveryPolicy" :{"AlternateIdLogin":{"Enabled": true}}}}
    DisplayName : BasicAutoAccelerationPolicy
    IsOrganizationDefault : True
    KeyCredentials : {}
    Type : HomeRealmDiscoveryPolicy
    ```

    Wenn die Richtlinie vorhanden ist, das *AlternateIdLogin*-Attribut jedoch nicht vorhanden oder nicht aktiviert ist, oder wenn andere Attribute für die Richtlinie vorhanden sind, die Sie beibehalten möchten, aktualisieren Sie die vorhandene Richtlinie mithilfe des Cmdlets [Set-AzureADPolicy][Set-AzureADPolicy].

    > [!IMPORTANT]
    > Wenn Sie die Richtlinie aktualisieren, stellen Sie sicher, dass Sie alle alten Einstellungen und das neue *AlternateIdLogin*-Attribut einschließen.

    Im folgenden Beispiel wird das  *AlternateIdLogin*-Attribut hinzugefügt und das möglicherweise bereits festgelegte  *AllowCloudPasswordValidation*-Attribut beibehalten:

    ```powershell
    Set-AzureADPolicy -id b581c39c-8fe3-4bb5-b53d-ea3de05abb4b `
        -Definition @('{"HomeRealmDiscoveryPolicy" :{"AllowCloudPasswordValidation":true,"AlternateIdLogin":{"Enabled": true}}}') `
        -DisplayName "BasicAutoAccelerationPolicy" `
        -IsOrganizationDefault $true `
        -Type "HomeRealmDiscoveryPolicy"
    ```

    Vergewissern Sie sich, dass die aktualisierte Richtlinie ihre Änderungen aufweist und dass das *AlternateIdLogin*-Attribut jetzt aktiviert ist:

    ```powershell
    Get-AzureADPolicy | where-object {$_.Type -eq "HomeRealmDiscoveryPolicy"} | fl *
    ```

## <a name="test-user-sign-in-with-email"></a>Testen der Benutzeranmeldung per E-Mail-Adresse

Um zu testen, ob Benutzer sich mit ihrer E-Mail-Adresse anmelden können, navigieren Sie zu [https://myprofile.microsoft.com][my-profile], und melden Sie sich mit einem Benutzerkonto an, das auf ihrer E-Mail-Adresse basiert, z. B. `balas@fabrikam.com`, und nicht auf ihrem UPN, z. B. `balas@contoso.com`. Die Anmeldung sollte wie bei einem UPN-basierten Anmeldeereignis aussehen und funktionieren.

## <a name="troubleshoot"></a>Problembehandlung

Wenn Benutzer Probleme mit Anmeldeereignissen mit Ihrer E-Mail-Adresse haben, versuchen Sie es mit den folgenden Schritten zur Problembehandlung:

1. Stellen Sie sicher, dass für das Benutzerkonto in der lokalen AD DS-Umgebung eine E-Mail-Adresse für das *ProxyAddresses*-Attribut festgelegt ist.
1. Vergewissern Sie sich, dass Azure AD Connect konfiguriert ist und die Benutzerkonten aus der lokalen AD DS-Umgebung erfolgreich mit Azure AD synchronisiert.
1. Vergewissern Sie sich, dass für die Azure AD-Richtlinie *HomeRealmDiscoveryPolicy* das *AlternateIdLogin*-Attribut auf *"Enabled": true* festgelegt ist:

    ```powershell
    Get-AzureADPolicy | where-object {$_.Type -eq "HomeRealmDiscoveryPolicy"} | fl *
    ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Hybrididentität, z. B. mit Azure AD App Proxy oder Azure AD Domain Services, finden Sie unter [Azure AD Hybrididentität für den Zugriff und die Verwaltung lokaler Workloads][hybrid-overview].

Weitere Informationen zu Hybrididentitätsvorgängen finden Sie unter [Funktionsweise der Kennworthashsynchronisierung][phs-overview] und [Funktionsweise der Passthrough-Authentifizierung][pta-overview].

<!-- INTERNAL LINKS -->
[verify-domain]: ../fundamentals/add-custom-domain.md
[hybrid-auth-methods]: ../hybrid/choose-ad-authn.md
[azure-ad-connect]: ../hybrid/whatis-azure-ad-connect.md
[hybrid-overview]: ../hybrid/cloud-governed-management-for-on-premises.md
[phs-overview]: ../hybrid/how-to-connect-password-hash-synchronization.md
[pta-overview]: ../hybrid/how-to-connect-pta-how-it-works.md

<!-- EXTERNAL LINKS -->
[Install-Module]: /powershell/module/powershellget/install-module
[Connect-AzureAD]: /powershell/module/azuread/connect-azuread
[Get-AzureADPolicy]: /powershell/module/azuread/get-azureadpolicy
[New-AzureADPolicy]: /powershell/module/azuread/new-azureadpolicy
[Set-AzureADPolicy]: /powershell/module/azuread/set-azureadpolicy
[my-profile]: https://myprofile.microsoft.com
