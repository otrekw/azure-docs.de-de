---
title: Einrichten und Erstellen von Teams-Zugriffstoken
titleSuffix: An Azure Communication Services quickstart
description: Erstellen eines Diensts für die Bereitstellung von Teams-Zugriffstoken
author: tomaschladek
manager: nmurav
services: azure-communication-services
ms.author: tchladek
ms.date: 06/30/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: e9e58659cfaa5b459a28278362aac002a1a87db5
ms.sourcegitcommit: f4e04fe2dfc869b2553f557709afaf057dcccb0b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2021
ms.locfileid: "113223858"
---
# <a name="quickstart-set-up-and-manage-teams-access-tokens"></a>Schnellstart: Einrichten und Verwalten von Teams-Zugriffstoken

> [!IMPORTANT]
> Wenn Sie einen benutzerdefinierten Teams-Endpunkt aktivieren/deaktivieren möchten, füllen Sie [dieses Formular](https://forms.office.com/r/B8p5KqCH19) aus.

In diesem Schnellstart erstellen Sie eine .NET-Konsolenanwendung, um ein AAD-Benutzertoken mithilfe der MSAL-Bibliothek zu authentifizieren. Dieses Token wird dann mit dem Azure Communication Services-Identitäts-SDK gegen ein Teams-Zugriffstoken ausgetauscht. Das Teams-Zugriffstoken kann dann vom Azure Communication Services-Anruf-SDK verwendet werden, um einen benutzerdefinierten Teams-Endpunkt zu erstellen.

> [!NOTE]
> In Produktionsumgebungen wird empfohlen, diesen Austauschmechanismus in Back-End-Diensten zu implementieren, da Austauschanforderungen mit einem Geheimnis signiert werden.


## <a name="prerequisites"></a>Voraussetzungen
- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Eine aktive Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource](./create-communication-resource.md).
- Aktivieren Sie über [dieses Formular](https://forms.office.com/r/B8p5KqCH19) einen benutzerdefinierten Teams-Endpunkt.
- Sie benötigen eine Azure Active Directory-Instanz mit Benutzern mit einer Teams-Lizenz.

## <a name="introduction"></a>Einführung

Teams-Identitäten sind an Mandanten in Azure Active Directory gebunden. Ihre Anwendung kann von Benutzern desselben oder eines beliebigen Mandanten verwendet werden. In diesem Schnellstart wird ein Anwendungsfall mit mehreren Akteuren in mehreren Mandanten beschrieben: Benutzer, Entwickler und Administratoren der fiktiven Unternehmen Contoso und Fabrikam. In diesem Anwendungsfall ist Contoso ein Unternehmen, das eine SaaS-Lösung für Fabrikam erstellt. 

Die folgenden Abschnitte führen Sie durch die Schritte für Administratoren, Entwickler und Benutzer. Die Diagramme veranschaulichen den Anwendungsfall mit mehreren Mandanten. Wenn Sie nur einen Mandanten verwenden, führen Sie alle Schritte für Contoso und Fabrikam in diesem Mandanten aus.

## <a name="admin-actions"></a>Administratoraktionen

Die Administratorrolle verfügt in AAD über erweiterte Berechtigungen. Mitglieder dieser Rolle können Ressourcen bereitstellen und Informationen im Azure-Portal lesen. Im folgenden Diagramm werden alle Aktionen angezeigt, die von Administratoren ausgeführt werden müssen.

![Administratoraktionen zum Aktivieren eines benutzerdefinierten Teams-Endpunkts](./media/teams-identities/teams-identity-admin-overview.png)

1. Der Administrator von Contoso erstellt eine *Anwendung* in Azure Active Directory oder wählt eine vorhandene aus. Die Eigenschaft *Unterstützte Kontotypen* legt fest, ob Benutzer aus einem anderen Mandanten sich bei der *Anwendung* authentifizieren können. Die Eigenschaft *Umleitungs-URI* dient zur Umleitung einer erfolgreichen Authentifizierungsanforderung an den *Server* von Contoso.
1. Der Administrator von Contoso erweitert das Manifest der *Anwendung* mit der VoIP-Berechtigung von Azure Communication Services. 
1. Der Administrator von Contoso ermöglicht den öffentlichen Clientflow für die *Anwendung*.
1. Der Administrator von Contoso kann optional ein Update durchführen.
1. Der Administrator von Contoso aktiviert die Funktion über [dieses Formular](https://forms.office.com/r/B8p5KqCH19).
1. Der Administrator von Contoso erstellt die Communication Services-Dienste, die für die Authentifizierung der Austauschanforderungen verwendet werden, oder wählt vorhandene aus. AAD-Benutzertoken werden gegen Teams-Zugriffstoken ausgetauscht. Weitere Informationen zum Erstellen neuer Azure Communication Services-Ressourcen finden Sie [hier](./create-communication-resource.md).
1. Der Administrator von Fabrikam stellt einen neuen Dienstprinzipal für Azure Communication Services im Mandanten von Fabrikam bereit.
1. Der Administrator von Fabrikam gewährt der *Anwendung* von Contoso die VoIP-Berechtigung von Azure Communication Services. Dieser Schritt ist nur erforderlich, wenn die *Anwendung* von Contoso nicht überprüft wurde.

### <a name="1-create-aad-application-registration-or-select-aad-application"></a>1: Erstellen der AAD-Anwendungsregistrierung oder Auswählen einer AAD-Anwendung 

Benutzer müssen mit der `VoIP`-Berechtigung von Azure Communication Service für AAD-Anwendungen authentifiziert werden. Wenn Sie nicht über eine Anwendung verfügen, die Sie für diesen Schnellstart verwenden möchten, können Sie eine neue Anwendungsregistrierung erstellen. 

Die folgenden Anwendungseinstellungen haben Einfluss auf die Vorgehensweise:
- Die Eigenschaft *Unterstützte Kontotypen* legt fest, ob die *Anwendung* einen einzelnen Mandanten („Nur Konten in diesem Organisationsverzeichnis“) oder mehrere Mandanten („Konten in einem beliebigen Organisationsverzeichnis“) unterstützt. In diesem Szenario verwenden Sie mehrere Mandanten.
- Der *Umleitungs-URI* definiert den URI, an den die Authentifizierungsanforderung nach der Authentifizierung umgeleitet wird. In diesem Szenario können Sie „Öffentlicher Client/nativ (mobil und Desktop)“ verwenden und als URI http://localhost eingeben.

[Hier finden Sie eine ausführliche Dokumentation.](/azure/active-directory/develop/quickstart-register-app#register-an-application) 

Wenn die *Anwendung* registriert ist, wird in der Übersicht ein Bezeichner angezeigt. Dieser Bezeichner wird in den folgenden Schritten verwendet: **Anwendungs-ID (Client)** .

### <a name="2-allow-public-client-flows"></a>2: Zulassen öffentlicher Clientflows

Im Bereich *Authentifizierung* Ihrer *Anwendung* wird als konfigurierte Plattform *Öffentlicher Client/nativ (mobil und Desktop)* mit einem auf *localhost* verweisenden Umleitungs-URI angezeigt. Am unteren Bildschirmrand finden Sie die Umschaltfläche *Öffentliche Clientflows zulassen*, die für diesen Schnellstart auf **Ja** festgelegt wird.

### <a name="3-update-publisher-domain-optional"></a>3: Aktualisieren der Herausgeberdomäne (optional)
Im Bereich *Branding* können Sie Ihre Herausgeberdomäne für die *Anwendung* aktualisieren. Dies ist nützlich für Anwendungen mit mehreren Mandanten, bei denen die Anwendung als von Azure überprüft gekennzeichnet wird. Ausführliche Informationen zum Überprüfen des Herausgebers und zum Aktualisieren der Domäne Ihrer Anwendung finden Sie [hier](/azure/active-directory/develop/howto-configure-publisher-domain).

### <a name="4-define-azure-communication-services-voip-permission-in-application"></a>4: Definieren der VoIP-Berechtigung von Azure Communication Services in der Anwendung

Wechseln Sie zu den Details der *Anwendung*, und wählen Sie den Bereich „Manifest“ aus. Suchen Sie im Manifest die *requiredResourceAccess*-Eigenschaft. Es handelt sich um ein Array von Objekten, das die Berechtigungen der *Anwendung* definiert. Erweitern Sie das Manifest mit den VoIP-Berechtigungen für die Erstanbieteranwendung Azure Communication Services. Fügen Sie dem Array das folgende Objekt hinzu.

> [!NOTE] 
> Ändern Sie keinesfalls die GUIDs im Codeausschnitt, da sie die Anwendung und die Berechtigungen eindeutig identifizieren.

```json
{
   "resourceAppId": "1fd5118e-2576-4263-8130-9503064c837a",
   "resourceAccess": [
      {
         "id": "31f1efa3-6f54-4008-ac59-1bf1f0ff9958",
         "type": "Scope"
      }
   ]
}
```

Wählen Sie dann die Schaltfläche *Speichern* aus, um Ihre Änderungen dauerhaft zu speichern. Die VoIP-Berechtigung von Azure Communication Services wird jetzt im Bereich *API-Berechtigungen* angezeigt.

### <a name="5-enable-custom-teams-endpoint-experience-for-application"></a>5: Aktivieren des benutzerdefinierten Teams-Endpunkts für die *Anwendung*

Der AAD-Administrator muss [dieses Formular](https://forms.office.com/r/B8p5KqCH19) ausfüllen, um den benutzerdefinierten Teams-Endpunkt für die *Anwendung* zu aktivieren.

### <a name="6-create-or-select-communication-services-resource"></a>6: Erstellen oder Auswählen einer Communication Services-Ressource

Mithilfe Ihrer Azure Communication Services-Ressource werden alle Anforderungen für den Austausch von AAD-Benutzertoken mit Teams-Zugriffstoken authentifiziert. Dieser Austausch kann über das Azure Communication Services-Identitäts-SDK ausgelöst werden, das mit dem Zugriffsschlüssel oder über Azure RBAC authentifiziert wird. Sie können den Zugriffsschlüssel im Azure-Portal abrufen oder Azure RBAC über den Bereich *Zugriffssteuerung (IAM)* konfigurieren.

Wenn Sie neue Communication Services-Ressource erstellen möchten, befolgen Sie [diesen Leitfaden](./create-communication-resource.md).

### <a name="7-provision-azure-communication-services-service-principal"></a>7: Bereitstellen des Azure Communication Services-Dienstprinzipals

Um den benutzerdefinierten Teams-Endpunkt im Mandanten von Fabrikam zu aktivieren, muss der AAD-Administrator von Fabrikam einen Dienstprinzipal namens „Azure Communication Services“ mit der Anwendungs-ID *1fd5118e-2576-4263-8130-9503064c837a* bereitstellen. Wenn diese Anwendung nicht im Bereich für Unternehmensanwendungen in Azure Active Directory angezeigt wird, muss sie manuell hinzugefügt werden.

Der AAD-Administrator von Fabrikam stellt über PowerShell eine Verbindung mit dem Azure-Mandanten her. 

> [!NOTE]
> Ersetzen Sie [Tenant_ID] durch die ID Ihres Mandanten, die sie im Azure-Portal auf der Übersichtsseite von AAD finden.

```azurepowershell
Connect-AzureAD -TenantId "[Tenant_ID]"
```

Wenn der Befehl nicht gefunden werden kann, wird das AzureAD-Modul nicht in PowerShell installiert. Schließen Sie PowerShell, und führen Sie es mit Administratorrechten aus. Anschließend können Sie das AzureAD-Paket mit dem folgenden Befehl installieren:

```azurepowershell
Install-Module AzureAD
```

Führen Sie nach dem Herstellen einer Verbindung mit Azure und der Authentifizierung den folgenden Befehl aus, um den Dienstprinzipal von Communication Services bereitzustellen. 

> [!NOTE]
> Der Parameter „AppId“ bezieht sich auf die Erstanbieteranwendung Azure Communication Services. Ändern Sie diesen Wert nicht.

```azurepowershell
New-AzureADServicePrincipal -AppId "1fd5118e-2576-4263-8130-9503064c837a"
```

### <a name="8-provide-admin-consent"></a>8: Erteilen der Administratoreinwilligung

Wenn die *Anwendung* von Contoso nicht geprüft ist, muss der AAD-Administrator der *Anwendung* von Contoso die VoIP-Berechtigung von Azure Communication Services gewähren. Der AAD-Administrator von Fabrikam erteilt die Einwilligung über einen eindeutigen Link. Befolgen Sie die folgenden Anweisungen, um den Link zur Administratoreinwilligung zu erstellen:

1. Verwenden Sie den Link *https://login.microsoftonline.com/{Tenant_ID}/adminconsent?client_id={Application_ID}* .
1. Ersetzen Sie {Tenant_ID} durch die Mandanten-ID von Fabrikam.
1. Ersetzen Sie {Application_ID} durch die Anwendungs-ID von Contoso.
1. Der AAD-Administrator von Fabrikam navigiert im Browser zu diesem Link. 
1. Der AAD-Administrator von Fabrikam meldet sich an und erteilt Berechtigungen im Namen der Organisation.

Der Dienstprinzipal der *Anwendung* von Contoso im Mandanten von Fabrikam wird erstellt, sobald die Einwilligung erteilt wurde. Der Administrator von Fabrikam kann die Einwilligung in AAD überprüfen:

1. Melden Sie sich als Administrator beim Azure-Portal an.
1. Wechseln Sie zu Azure Active Directory.
1. Navigieren Sie zum Bereich „Unternehmensanwendungen“.
1. Legen Sie den Filter „Anwendungstyp“ auf „Alle Anwendungen“ fest.
1. Fügen Sie in das Feld zum Filtern von Anwendungen den Namen der Anwendung von Contoso ein.
1. Wählen Sie „Anwenden“ aus, um die Ergebnisse zu filtern.
1. Wählen Sie den Dienstprinzipal mit dem gewünschten Namen aus. 
1. Wechseln Sie zum Bereich *Berechtigungen*.

Wie Sie sehen, hat die VoIP-Berechtigung von Azure Communication Services den Status *Gewährt für {Verzeichnisname}* .

## <a name="developer-actions"></a>Entwickleraktionen

Der Entwickler von Contoso muss die *Clientanwendung* für die Authentifizierung von Benutzern einrichten. Anschließend muss er einen Endpunkt auf dem *Back-End-Server* erstellen, um AAD-Benutzertoken nach der Umleitung zu verarbeiten. Wenn ein AAD-Benutzertoken empfangen wurde, wird es gegen ein Teams-Zugriffstoken ausgetauscht und an die *Clientanwendung* zurückgegeben. Die Aktionen, die von Entwicklern ausgeführt werden müssen, sind im folgenden Diagramm dargestellt:

![Entwickleraktionen zum Aktivieren eines benutzerdefinierten Teams-Endpunkts](./media/teams-identities/teams-identity-developer-overview.png)

1. Der Entwickler von Contoso konfiguriert die MSAL-Bibliothek zum Authentifizieren von Benutzern für die *Anwendung*, die in den vorherigen Schritten vom Administrator für die VoIP-Berechtigung von Azure Communication Services erstellt wurde.
1. Der Entwickler von Contoso initialisiert das ACS-Identitäts-SDK und tauscht damit eingehende AAD-Benutzertoken gegen Teams-Zugriffstoken aus. Die Teams-Zugriffstoken werden dann an die *Clientanwendung* zurückgegeben.

Mithilfe der Microsoft Authentication Library (MSAL) können Entwickler AAD-Benutzertoken vom Microsoft Identity Platform-Endpunkt abrufen, um Benutzer zu authentifizieren und auf geschützte Web-APIs zuzugreifen. Sie kann verwendet werden, um sicheren Zugriff auf Azure Communication Services zu gewähren. MSAL unterstützt verschiedene Anwendungsarchitekturen und -plattformen einschließlich .NET, JavaScript, Java, Python, Android und iOS.

Weitere Informationen zum Einrichten verschiedener Umgebungen finden Sie in der öffentlichen Dokumentation. [Übersicht über die Microsoft Authentication Library (MSAL)](/azure/active-directory/develop/msal-overview).

> [!NOTE]
> In den folgenden Abschnitten wird beschrieben, wie Sie in .NET AAD-Zugriffstoken gegen Teams-Zugriffstoken für die Konsolenanwendung austauschen.

### <a name="create-new-application"></a>Erstellen einer neuen Anwendung

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl „dotnet new“ zum Erstellen einer neuen Konsolen-App mit dem Namen *TeamsAccessTokensQuickstart*. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: *Program.cs*.

```console
dotnet new console -o TeamsAccessTokensQuickstart
```

Wechseln Sie zum neu erstellten App-Ordner, und verwenden Sie den Befehl „dotnet build“, um Ihre Anwendung zu kompilieren.

```console
cd TeamsAccessTokensQuickstart
dotnet build
```
#### <a name="install-the-package"></a>Installieren des Pakets
Installieren Sie im Anwendungsverzeichnis mithilfe des Befehls „dotnet add package“ das .NET-Paket der Identitätsbibliothek von Azure Communication Services.

```console
dotnet add package Azure.Communication.Identity
dotnet add package Microsoft.Identity.Client
```

#### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

Über das Projektverzeichnis:

- Öffnen Sie die Datei Program.cs in einem Text-Editor.
- Fügen Sie eine using-Direktive hinzu, um die folgenden Namespaces hinzuzufügen: 
    - Azure.Communication
    - Azure.Communication.Identity
    - Microsoft.Identity.Client
- Aktualisieren Sie die Deklaration der Main-Methode so, dass asynchroner Code unterstützt wird.

Verwenden Sie zum Einstieg den folgenden Code:

```csharp
using System;
using System.Text;
using Azure.Communication;
using Azure.Communication.Identity;
using Microsoft.Identity.Client;

namespace TeamsAccessTokensQuickstart
{
    class Program
    {
        static async System.Threading.Tasks.Task Main(string[] args)
        {
            Console.WriteLine("Azure Communication Services – Teams access tokens quickstart");

            // Quickstart code goes here
        }
    }
}
```

### <a name="1-receive-aad-user-token-via-msal-library"></a>1: Empfangen von AAD-Benutzertoken über die MSAL-Bibliothek

Verwenden Sie die MSAL-Bibliothek, um Benutzer mit der VoIP-Berechtigung von Azure Communication Services in AAD für die *Anwendung* von Contoso zu authentifizieren. Konfigurieren Sie den Client für die *Anwendung* von Contoso (Parameter *applicationId*) in der öffentlichen Cloud (Parameter *authority*). Das AAD-Benutzertoken wird an den Umleitungs-URI (Parameter *redirectUri*) zurückgegeben. Die Anmeldeinformationen werden aus dem interaktiven Popupfenster übernommen, das in Ihrem Standardbrowser geöffnet wird.

> [!NOTE] 
> Der Umleitungs-URI muss mit dem in der *Anwendung* festgelegten Wert übereinstimmen. Lesen Sie den ersten Schritt im Administratorleitfaden, um zu erfahren, wie Sie den Umleitungs-URI konfigurieren.

```csharp
const string applicationId = "Contoso's_Application_ID";
const string authority = "https://login.microsoftonline.com/common";
const string redirectUri = "http://localhost";

var client = PublicClientApplicationBuilder
                .Create(applicationId)
                .WithAuthority(authority)
                .WithRedirectUri(redirectUri)
                .Build();

const string scope = "https://auth.msft.communication.azure.com/VoIP";

var aadUserToken = await client.AcquireTokenInteractive(new[] { scope }).ExecuteAsync();

Console.WriteLine("\nAuthenticated user: " + aadUserToken.Account.Username);
Console.WriteLine("AAD user token expires on: " + aadUserToken.ExpiresOn);
```

Die Variable *aadUserToken* enthält jetzt ein gültiges Azure Active Directory-Benutzertoken, das für den Austausch verwendet wird.

### <a name="2-exchange-aad-user-token-for-teams-access-token"></a>2: Austauschen von AAD-Benutzertoken gegen Teams-Zugriffstoken

Mit einem gültigen AAD-Benutzertoken werden Benutzer bei AAD über die VoIP-Berechtigung von Azure Communication Services für Drittanbieteranwendungen authentifiziert. Der folgende Code wird mit dem ACS-Identitäts-SDK für den Austausch von AAD-Benutzertoken gegen Teams-Zugriffstoken verwendet.

> [!NOTE]
> Ersetzen Sie den Wert „&lt;Connection-String&gt;“ durch eine gültige Verbindungszeichenfolge, oder verwenden Sie für die Authentifizierung Azure RBAC. Weitere Informationen finden Sie in [diesem Schnellstart](./access-tokens.md).

```csharp
var identityClient = new CommunicationIdentityClient("<Connection-String>");
var teamsAccessToken = identityClient.ExchangeTeamsToken(aadUserToken.AccessToken);

Console.WriteLine("\nTeams access token expires on: " + teamsAccessToken.Value.ExpiresOn);
```

Wenn alle in den Anforderungen definierten Bedingungen erfüllt sind, erhalten Sie ein gültiges Teams-Zugriffstoken, das 24 Stunden gültig ist.

#### <a name="run-the-code"></a>Ausführen des Codes
Führen Sie die Anwendung mit dem Befehl „dotnet run“ aus Ihrem Anwendungsverzeichnis aus.

```console
dotnet run
```

In der Ausgabe der App wird die jeweils abgeschlossene Aktion beschrieben:

```console
Azure Communication Services - Teams access tokens quickstart

Authenticated user: john.smith@contoso.com
AAD user token expires on: 6/10/2021 10:13:17 AM +00:00

Teams access token expires on: 6/11/2021 9:13:18 AM +00:00
```

## <a name="user-actions"></a>Benutzeraktionen

Benutzer sind in diesem Fall die Benutzer der *Anwendung* von Contoso bei Fabrikam. Die Benutzeraktionen sind im folgenden Diagramm dargestellt.

![Benutzeraktionen zum Aktivieren eines benutzerdefinierten Teams-Endpunkts](./media/teams-identities/teams-identity-user-overview.png)

1. Der Benutzer von Fabrikam verwendet die *Clientanwendung* von Contoso und wird zur Authentifizierung aufgefordert.
1. Die *Clientanwendung* von Contoso authentifiziert den Benutzer mithilfe der MSAL-Bibliothek und des Azure Active Directory-Mandanten von Fabrikam mit der VoIP-Berechtigung von Azure Communication Services für die *Anwendung* von Contoso. 
1. Die Authentifizierung wird an den *Server* umgeleitet, der in der Eigenschaft *Umleitungs-URI* in MSAL und in der *Anwendung* von Contoso definiert ist.
1. Der *Server* von Contoso tauscht das AAD-Benutzertoken mithilfe des ACS-Identitäts-SDK gegen das Teams-Zugriffstoken aus und gibt das Teams-Zugriffstoken an die *Clientanwendung* zurück.

Mit einem gültigen Teams-Zugriffstoken in der *Clientanwendung* können Entwickler das ACS-Anfruf-SDK integrieren und einen benutzerdefinierten Teams-Endpunkt erstellen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen und Konfigurieren einer Anwendung in Azure Active Directory
> * Verwenden der MSAL-Bibliothek zum Ausstellen von Azure Active Directory-Benutzertoken
> * Verwenden des ACS-Identitäts-SDK zum Austauschen von Azure Active Directory-Benutzertoken gegen Teams-Zugriffstoken

Die folgenden Dokumente könnten Sie auch interessieren:

- Weitere Informationen zu [benutzerdefinierten Teams-Endpunkten](../concepts/teams-endpoint.md)
- Weitere Informationen zur [Teams-Interoperabilität](../concepts/teams-interop.md)
