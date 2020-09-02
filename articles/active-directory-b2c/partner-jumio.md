---
title: Tutorial zum Konfigurieren von Azure Active Directory B2C mit Jumio
titleSuffix: Azure AD B2C
description: Tutorial zum Konfigurieren von Azure Active Directory B2C mit Jumio für automatisierte ID-Überprüfung und Schutz von Kundendaten
services: active-directory-b2c
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/20/2020
ms.author: gasinh
ms.subservice: B2C
ms.openlocfilehash: 47e06c84eb2e0463b31b7bdea5897ceca4339419
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88817091"
---
# <a name="tutorial-for-configuring-jumio-with-azure-active-directory-b2c"></a>Tutorial zum Konfigurieren von Jumio mit Azure Active Directory B2C

In diesem Beispieltutorial erhalten Sie eine Anleitung zum Integrieren von Azure AD B2C mit [Jumio](https://www.jumio.com/). Jumio ist ein ID-Überprüfungsdienst, der eine automatische ID-Überprüfung in Echtzeit und den Schutz von Kundendaten ermöglicht.

## <a name="prerequisites"></a>Voraussetzungen

Zunächst benötigen Sie Folgendes:

- Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.

- Einen [Azure AD B2C-Mandanten](https://review.docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant). Der Mandant ist mit Ihrem Azure-Abonnement verknüpft.

## <a name="scenario-description"></a>Beschreibung des Szenarios

Die Jumio-Integration umfasst die folgenden Komponenten:

- Azure AD B2C: Der Autorisierungsserver, der für die Überprüfung der Anmeldeinformationen des Benutzers verantwortlich ist und auch als Identitätsanbieter bezeichnet wird.

- Jumio: Der Dienst, der die vom Benutzer bereitgestellten ID-Details übernimmt und überprüft.

- REST-API der mittleren Schicht: Mit dieser API wird die Integration zwischen Azure AD B2C und dem Jumio-Dienst implementiert.

- Blobspeicher: Wird verwendet, um den Azure AD B2C-Richtlinien benutzerdefinierte Dateien für die Benutzeroberfläche bereitzustellen.

Im folgenden Architekturdiagramm ist die Implementierung dargestellt.

![Screenshot für das Jumio-Architekturdiagramm](./media/partner-jumio/jumio-architecture-diagram.png)

|Schritt | BESCHREIBUNG |
|:-----| :-----------|
| 1. | Der Benutzer gelangt auf eine Anmeldeseite. Der Benutzer wählt die Registrierung zum Erstellen eines neuen Kontos aus und gibt Informationen auf der Seite ein. Azure AD B2C erfasst die Benutzerattribute.
| 2. | Azure AD B2C ruft die API der mittleren Ebene auf und übergibt die Benutzerattribute.
| 3. | Die API der mittleren Schicht sammelt die Benutzerattribute und wandelt sie in ein Format um, das von der Jumio-API verarbeitet werden kann. Anschließend werden sie an Jumio gesendet.
| 4. | Nachdem Jumio die Informationen genutzt und verarbeitet hat, wird das Ergebnis an die API der mittleren Ebene zurückgegeben.
| 5. | Die API der mittleren Ebene verarbeitet die Informationen und sendet relevante Informationen zurück an Azure AD B2C.
| 6. | Azure AD B2C empfängt die Informationen von der API der mittleren Ebene. Wenn eine Antwort mit einem Fehler empfangen wird, wird für den Benutzer eine Fehlermeldung angezeigt. Bei einer Erfolgsantwort wird der Benutzer authentifiziert und in das Verzeichnis geschrieben.

## <a name="onboard-with-jumio"></a>Durchführen des Onboardings mit Jumio

1. Zum Erstellen eines Jumio-Kontos wenden Sie sich an [Jumio](https://www.jumio.com/contact/)

2. Nachdem ein Konto erstellt wurde, werden Informationen in der API-Konfiguration verwendet. In den folgenden Abschnitten wird der Prozess beschrieben.

## <a name="configure-azure-ad-b2c-with-jumio"></a>Konfigurieren von Azure AD B2C mit Jumio

### <a name="part-1---deploy-the-api"></a>Teil 1: Bereitstellen der API

Stellen Sie den angegebenen [API-Code](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/Jumio/API/Jumio.Api) für einen Azure-Dienst bereit. Der Code kann mit [dieser Anleitung](https://docs.microsoft.com/visualstudio/deployment/quickstart-deploy-to-azure?view=vs-2019) aus Visual Studio veröffentlicht werden.

>[!NOTE]
>Sie benötigen die URL des bereitgestellten Diensts, um Azure AD mit den erforderlichen Einstellungen zu konfigurieren.

### <a name="part-2---deploy-the-client-certificate"></a>Teil 2: Bereitstellen des Clientzertifikats

1. Der Aufruf der Jumio-API ist durch ein Clientzertifikat geschützt. Erstellen Sie mithilfe des folgenden PowerShell-Beispielcodes ein selbstsigniertes Zertifikat:

``` PowerShell
$cert = New-SelfSignedCertificate -Type Custom -Subject "CN=Demo-SigningCertificate" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3") -KeyUsage DigitalSignature -KeyAlgorithm RSA -KeyLength 2048 -NotAfter (Get-Date).AddYears(2) -CertStoreLocation "Cert:\CurrentUser\My"
$cert.Thumbprint
$pwdText = "Your password"
$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText
Export-PfxCertificate -Cert $Cert -FilePath "{your-local-path}\Demo-SigningCertificate.pfx" -Password $pwd.

```

2. Das Zertifikat wird dann an den für {``your-local-path``} angegebenen Speicherort exportiert.

3. Importieren Sie das Zertifikat gemäß den Anweisungen in diesem [Dokument](https://docs.microsoft.com/azure/app-service/configure-ssl-certificate#upload-a-private-certificate) in Azure App Service.

### <a name="part-3---create-a-signingencryption-key"></a>Teil 3: Erstellen eines Signatur-/Verschlüsselungsschlüssels

Erstellen Sie eine zufällige Zeichenfolge mit einer Länge von mehr als 64 Zeichen, die nur Buchstaben oder Zahlen enthält.

Beispiel: ``C9CB44D98642A7062A0D39B94B6CDC1E54276F2E7CFFBF44288CEE73C08A8A65``

Verwenden Sie das folgende PowerShell-Skript, um einen alphanumerischen Wert mit einer Länge von 64 Zeichen zu erstellen.

```PowerShell
-join ((0x30..0x39) + ( 0x41..0x5A) + ( 0x61..0x7A) + ( 65..90 ) | Get-Random -Count 64  | % {[char]$_})

```

### <a name="part-4---configure-the-api"></a>Teil 4: Konfigurieren der API

Die Anwendungseinstellungen können im [App-Dienst in Azure konfiguriert](https://docs.microsoft.com/azure/app-service/configure-common#configure-app-settings) werden. Mit dieser Methode können Einstellungen sicher konfiguriert werden, ohne sie in ein Repository einzuchecken. Sie müssen die folgenden Einstellungen für die REST-API angeben:

| Anwendungseinstellungen | `Source` | Notizen |
| :-------- | :------------| :-----------|
|JumioSettings:AuthUsername | Jumio-Kontokonfiguration |     |
|JumioSettings:AuthPassword | Jumio-Kontokonfiguration |     |
|AppSettings:SigningCertThumbprint|Fingerabdruck des erstellten selbstsignierten Zertifikats|  |
|AppSettings:IdTokenSigningKey| Mithilfe von PowerShell erstellter Signaturschlüssel | |
| AppSettings:IdTokenEncryptionKey |Mithilfe von PowerShell erstellter Verschlüsselungsschlüssel
| AppSettings:IdTokenIssuer | Für das JWT-Token zu verwendender Aussteller (ein GUID-Wert wird bevorzugt) |
| AppSettings:IdTokenAudience  | Für das JWT-Token zu verwendende Zielgruppe (ein GUID-Wert wird bevorzugt) |
|AppSettings:BaseRedirectUrl | Basis-URL der B2C-Richtlinie | https://{Ihr Mandantenname}.b2clogin.com/{Ihre Anwendungs-ID}|
| WEBSITE_LOAD_CERTIFICATES| Fingerabdruck des erstellten selbstsignierten Zertifikats |

### <a name="part-5---deploy-the-ui"></a>Teil 5: Bereitstellen der Benutzeroberfläche

1. Richten Sie einen [Blob Storage-Container in Ihrem Speicherkonto](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal#create-a-container) ein.

2. Speichern Sie die Dateien für die Benutzeroberfläche aus dem [Benutzeroberflächenordner](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/Jumio/UI) in Ihrem Blobcontainer.

#### <a name="update-ui-files"></a>Aktualisieren von Dateien für die Benutzeroberfläche

1. Wechseln Sie in den Dateien für die Benutzeroberfläche zum Ordner [ocean_blue](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/Jumio/UI/ocean_blue).

2. Öffnen Sie jede HTML-Datei.

3. Suchen und ersetzen Sie {your-ui-blob-container-url} durch Ihre Blobcontainer-URL.

4. Suchen Sie {your-intermediate-api-url}, und ersetzen diese Zeichenfolge durch die URL des API-App-Diensts der mittleren Schicht.

>[!NOTE]
> Als bewährte Methode empfehlen wir Kunden, auf der Seite für die Erfassung der Attribute eine Benachrichtigung zur Einwilligung hinzufügen. Informieren Sie die Benutzer darüber, dass Informationen zur Identitätsüberprüfung an Drittanbieterdienste gesendet werden.

### <a name="part-6---configure-the-azure-ad-b2c-policy"></a>Teil 6: Konfigurieren der Azure AD B2C-Richtlinie

1. Wechseln Sie im Richtlinienordner zur [Azure AD B2C-Richtlinie](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/Jumio/Policies).

2. Folgen Sie den Anweisungen in diesem [Dokument](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started?tabs=applications#custom-policy-starter-pack), um das [LocalAccounts Starter Pack](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/LocalAccounts) herunterzuladen.

3. Konfigurieren Sie die Richtlinie für den Azure AD B2C-Mandanten.

>[!NOTE]
>Aktualisieren Sie die bereitgestellten Richtlinien, sodass sie sich auf Ihren genauen Mandanten beziehen.

## <a name="test-the-user-flow"></a>Testen des Benutzerflows

1. Öffnen Sie den Azure AD B2C-Mandanten, und wählen Sie unter „Richtlinien“ die Option **Identity Experience Framework** aus.

2. Wählen Sie den zuvor erstellten Flow **SignUpSignIn** aus.

3. Wählen Sie **Benutzerflow ausführen** und dann die Einstellungen aus:

   a. **Anwendung:** Wählen Sie die registrierte App aus (Beispiel: JWT).

   b. **Antwort-URL:** Wählen Sie die **Umleitungs-URL** aus.

   c. Wählen Sie **Benutzerflow ausführen** aus.

4. Führen Sie den Registrierungsflow aus, und erstellen Sie ein Konto.

5. Der Jumio-Dienst wird während des Flows aufgerufen, nachdem das Benutzerattribut erstellt wurde. Wenn der Flow unvollständig ist, stellen Sie sicher, dass der Benutzer nicht im Verzeichnis gespeichert ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Benutzerdefinierte Richtlinien in Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-overview)

- [Erste Schritte mit benutzerdefinierten Richtlinien in Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started?tabs=applications)
