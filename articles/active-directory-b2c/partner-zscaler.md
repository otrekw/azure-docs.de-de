---
title: Tutorial zum Konfigurieren von Azure Active Directory B2C mit Zscaler
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie die Azure AD B2C-Authentifizierung in Zscaler integrieren.
services: active-directory-b2c
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/09/2020
ms.author: gasinh
ms.subservice: B2C
ms.openlocfilehash: ff51c2a71dfcaec580733a92e265628ac816e229
ms.sourcegitcommit: 5db975ced62cd095be587d99da01949222fc69a3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2020
ms.locfileid: "97095829"
---
# <a name="tutorial-to-configure-zscaler-private-access-with-azure-active-directory-b2c-for-secure-hybrid-access"></a>Tutorial zum Konfigurieren von Zscaler Private Access mit Azure Active Directory B2C für den sicheren Hybridzugriff

In diesem Tutorial wird beschrieben, wie Sie die Azure AD B2C-Authentifizierung in [Zscaler Private Access (ZPA)](https://www.zscaler.com/products/zscaler-private-access) integrieren. ZPA ermöglicht einen richtlinienbasierten, sicheren Zugriff auf private Anwendungen und Ressourcen ohne die Kosten, den Aufwand oder die Sicherheitsrisiken eines virtuellen privaten Netzwerks (VPN). Das Angebot des sicheren Hybridzugriffs von Zscaler ermöglicht in Kombination mit Azure AD B2C eine unangreifbare Oberfläche für kundenorientierte Anwendungen.

## <a name="prerequisites"></a>Voraussetzungen

Zunächst benötigen Sie Folgendes:

- Ein Azure-Abonnement. Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.

- [Einen Azure AD B2C-Mandanten](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant), der mit Ihrem Azure-Abonnement verknüpft ist.

- [ZPA-Abonnement](https://azuremarketplace.microsoft.com/marketplace/apps/aad.zscalerprivateaccess?tab=Overview)

## <a name="scenario-description"></a>Beschreibung des Szenarios

Die ZPA-Integration umfasst die folgenden Komponenten:

- Azure AD B2C: Der Identitätsanbieter (IdP), mit dem die Anmeldeinformationen des Benutzers überprüft werden. Außerdem ist er für die Registrierung eines neuen Benutzers verantwortlich.

- ZPA: Der Dienst, der für die Sicherung der Webanwendung durch Erzwingen des [Zero Trust-Zugriffs](https://www.microsoft.com/security/blog/2018/12/17/zero-trust-part-1-identity-and-access-management/#:~:text=Azure%20Active%20Directory%20%28Azure%20AD%29%20provides%20the%20strong%2C,to%20express%20their%20access%20requirements%20in%20simple%20terms.) zuständig ist.

- Webanwendung: Hostet den Dienst, auf den der Benutzer zugreifen möchte.

Das folgende Diagramm zeigt, wie ZPA in Azure AD B2C integriert wird.

![Abbildung zeigt das Zscaler-Architekturdiagramm](media/partner-zscaler/zscaler-architecture-diagram.png)

|Schritt | Beschreibung |
|:-----| :-----------|
| 1. | Der Benutzer gelangt in ein ZPA-Benutzerportal oder zu einer ZPA-Browserzugriffsanwendung.
| 2. | ZPA benötigt die Benutzerkontextinformationen für die Entscheidung, ob der Benutzer auf die Webanwendung zugreifen darf. Zum Authentifizieren des Benutzers führt ZPA eine SAML-Umleitung zur Azure AD B2C-Anmeldeseite aus.  
| 3. | Der Benutzer gelangt zur Azure AD B2C-Anmeldeseite. Wenn es sich um einen neuen Benutzer handelt, registriert sich der Benutzer, um ein neues Konto zu erstellen. Vorhandene Benutzer melden sich mit ihren Anmeldeinformationen an. Azure AD B2C überprüft die Identität des Benutzers.
| 4. | Bei erfolgreicher Authentifizierung leitet Azure AD B2C den Benutzer zusammen mit der SAML-Assertion zurück an ZPA. ZPA überprüft die SAML-Assertion und legt den Benutzerkontext fest.
| 5. | ZPA wertet die Zugriffsrichtlinien für den Benutzer aus. Wenn der Benutzer Zugriff auf die Webanwendung hat, kann die Verbindung weitergeleitet werden.

## <a name="onboard-to-zpa"></a>Integrieren in ZPA

In diesem Tutorial wird davon ausgegangen, dass Sie bereits eine funktionierende ZPA-Einrichtung haben. Lesen Sie zum Einstieg in ZPA den [Step-by-Step Configuration Guide for ZPA](https://help.zscaler.com/zpa/step-step-configuration-guide-zpa) (Schritt-für-Schritt-Konfigurationshandbuch für ZPA).

## <a name="integrate-with-azure-ad-b2c"></a>Integration in Azure AD B2C

### <a name="part-1---configure-azure-ad-b2c-as-an-idp-on-zpa"></a>Teil 1: Konfigurieren von Azure AD B2C als IdP in ZPA

So konfigurieren Sie Azure AD B2C als [Identitätsanbieter (IdP) in ZPA](https://help.zscaler.com/zpa/configuring-idp-single-sign):

1. Melden Sie sich beim [ZPA Admin Portal](https://admin.private.zscaler.com) (ZPA-Verwaltungsportal) an.

2. Wechseln Sie zu **Administration** > **IdP Configuration** (Verwaltung > IdP-Konfiguration).

3. Wählen Sie **Add IdP Configuration** (IdP-Konfiguration hinzufügen) aus.

4. Geben Sie für **1 IdP Information** Folgendes ein:

   a. **Name**: Azure AD B2C

   b. **Single Sign-On** (Einmaliges Anmelden): Benutzer auswählen

   c. **Domains** (Domänen): Wählen Sie die Authentifizierungsdomänen aus, die Sie diesem IdP zuordnen möchten, und wählen Sie dann **Done** (Fertig).

   ![Abbildung zeigt die IdP-Informationen](media/partner-zscaler/add-idp-configuration.png)

5. Wählen Sie **Weiter** aus.

6. Für **2 SP Metadata** (SP-Metadaten):

   a. Kopieren Sie die „Service Provider URL“ (Dienstanbieter-URL), und notieren Sie sie zur späteren Verwendung.

   b. Kopieren Sie die „Service Provider Entity ID“ (Dienstanbieterentitäts-ID), und notieren Sie sie zur späteren Verwendung.

   ![Abbildung zeigt die SP-Metadateninformationen](media/partner-zscaler/sp-metadata.png)

7. Wählen Sie **Pause** (Anhalten) aus.

Der Rest der IdP-Konfiguration wird nach dem Konfigurieren von Azure AD B2C fortgesetzt.

### <a name="part-2---configure-custom-policy-in-azure-ad-b2c"></a>Teil 2: Konfigurieren der benutzerdefinierten Richtlinie in Azure AD B2C

>[!Note]
>Dieser Schritt ist nur erforderlich, wenn Sie nicht bereits über benutzerdefinierte Richtlinien verfügen. Wenn Sie bereits über eine oder mehrere benutzerdefinierte Richtlinien verfügen, können Sie diesen Schritt überspringen.

Der Artikel [Erste Schritte für benutzerdefinierte Richtlinien in Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started) enthält Anweisungen zum Konfigurieren von benutzerdefinierten Richtlinien für Ihren Azure AD B2C-Mandanten.

### <a name="part-3---register-zpa-as-a-saml-application-in-azure-ad-b2c"></a>Teil 3: Registrieren von ZPA als SAML-Anwendung in Azure AD B2C

Der Artikel [Registrieren einer SAML-Anwendung in Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/connect-with-saml-service-providers) enthält Anweisungen zum Konfigurieren einer SAML-Anwendung in Azure AD B2C. In [Schritt 3.2](https://docs.microsoft.com/azure/active-directory-b2c/connect-with-saml-service-providers#32-upload-and-test-your-policy-metadata) wird die von Azure AD B2C verwendete IdP-SAML-Metadaten-URL bereitgestellt. Die benötigen Sie später.

Befolgen Sie die Anweisungen dort bis [Schritt 4.2](https://docs.microsoft.com/azure/active-directory-b2c/connect-with-saml-service-providers#42-update-the-app-manifest). In Schritt 4.2 der Anleitung müssen Sie das App-Manifest aktualisieren. Aktualisieren Sie die Eigenschaften wie folgt:

- **identifierUris**: Verwenden Sie die in **Teil 1, Schritt 6a** notierte Entitäts-ID für den Dienstanbieter.

- **samlMetadataUrl**: Überspringen Sie diese Eigenschaft, da ZPA keine SAML-Metadaten-URL hostet.

- **replyUrlsWithType**: Verwenden Sie die in **Teil 1, Schritt 6b** notierte Dienstanbieter-URL.

- **logoutUrl**: Überspringen Sie diese Eigenschaft, da ZPA keine Abmelde-URL unterstützt.

Die restlichen Schritte sind für dieses Tutorial nicht relevant.

### <a name="part-4---extract-the-idp-saml-metadata-from-azure-ad-b2c"></a>Teil 4: Extrahieren der IdP-SAML-Metadaten aus Azure AD B2C

Aus dem vorherigen Schritt müssen Sie eine SAML-Metadaten-URL im folgenden Format abrufen:

```https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/<policy-name>/Samlp/metadata```

Dabei ist `<tenant-name>` der Name Ihres Azure AD B2C-Mandanten, und `<policy-name>` ist der Name der benutzerdefinierten SAML-Richtlinie, die Sie im letzten Schritt erstellt haben.

Beispiel: https://safemarch.b2clogin.com/safemarch.onmicrosoft.com/B2C_1A_signup_signin_saml//Samlp/metadata

Öffnen Sie einen Webbrowser, und navigieren Sie zur SAML-Metadaten-URL. Wenn die Seite geladen wird, klicken Sie mit der rechten Maustaste an beliebiger Stelle auf die Seite. Wählen Sie **Seite speichern unter** aus, und speichern Sie die Datei auf Ihrem Computer. Sie werden sie im nächsten Teil verwenden.

### <a name="part-5---complete-idp-configuration-on-zpa"></a>Teil 5: Abschluss der IdP-Konfiguration in ZPA

Schließen Sie die [IdP-Konfiguration im ZPA-Verwaltungsportal](https://help.zscaler.com/zpa/configuring-idp-single-sign) ab, die teilweise konfiguriert wurde in Teil 1:

1. Melden Sie sich beim [ZPA Admin Portal](https://admin.private.zscaler.com) (ZPA-Verwaltungsportal) an.

2. Wechseln Sie zu **Administration** > **IdP Configuration** (Verwaltung > IdP-Konfiguration).

3. Wählen Sie den in Teil 1 konfigurierten IdP und **Fortsetzen** aus.

4. Für **3 Create IdP** (3 IdP erstellen):

   a. Wechseln Sie zu **IdP Metadata File** > **Select File** (IdP-Metadatendatei > Datei auswählen), und laden Sie die Metadatendatei hoch, die Sie in Teil 4 gespeichert haben.

   b. Überprüfen Sie, ob der **Status** für die IDP-Konfiguration **Enabled** (Aktiviert) ist.

   c. Wählen Sie **Speichern** aus.

      ![Abbildung zeigt die Informationen zur IdP-Erstellung](media/partner-zscaler/create-idp.png)

## <a name="test-the-solution"></a>Testen der Lösung

Wechseln Sie zu einem ZPA-Benutzerportal oder einer Browserzugriffsanwendung, und testen Sie den Registrierungs- oder Anmeldevorgang. Das Ergebnis sollte eine erfolgreiche SAML-Authentifizierung sein.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Erste Schritte für benutzerdefinierte Richtlinien in Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started)

- [Registrieren einer SAML-Anwendung in Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/connect-with-saml-service-providers)

- [Step-by-Step Configuration Guide for ZPA](https://help.zscaler.com/zpa/step-step-configuration-guide-zpa) (Handbuch zur schrittweisen ZPA-Konfiguration)

- [Configuring an IdP for Single Sign-On](https://help.zscaler.com/zpa/configuring-idp-single-sign) (Konfigurieren eines IdP für einmaliges Anmelden)
