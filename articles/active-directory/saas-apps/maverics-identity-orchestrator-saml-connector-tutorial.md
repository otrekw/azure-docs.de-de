---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Maverics Identity Orchestrator SAML Connector | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Maverics Identity Orchestrator SAML Connector konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/12/2020
ms.author: jeedes
ms.openlocfilehash: ec5368427f50f548be965bb883683c859759bbf3
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88518927"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-maverics-identity-orchestrator-saml-connector"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Maverics Identity Orchestrator SAML Connector

## <a name="introduction"></a>Einführung

Strata bietet eine einfache Azure AD-Integration für lokale Anwendungen, um die Nutzung von Authentifizierungs- und Zugriffssteuerungsfunktionen zu ermöglichen.

In diesem Leitfaden erfahren Sie, wie Sie Maverics Identity Orchestrator&trade; für Folgendes konfigurieren:
* Inkrementelles Migrieren von Benutzern aus einem lokalen Identitätssystem zu Azure AD im Rahmen der Anmeldung bei einer lokalen Legacyanwendung
* Weiterleiten von Anmeldeanforderungen eines Legacyprodukts für die Webzugriffsverwaltung (beispielsweise CA SiteMinder oder Oracle Access Manager) an Azure AD
* Authentifizieren von Benutzern bei lokalen Anwendungen, die durch HTTP-Header oder proprietäre Sitzungscookies geschützt werden, nachdem der Benutzer über Azure AD authentifiziert wurde

Software von Strata kann lokal oder in der Cloud bereitgestellt werden und ermöglicht die Verwendung von Ermittlungs-, Verbindungs- und Orchestrierungsfunktionen für verschiedene Identitätsanbieter, um eine verteilte Identitätsverwaltung für Unternehmen mit Hybridumgebungen und Umgebungen mit mehreren Clouds zu erstellen.

In diesem Tutorial erfahren Sie, wie Sie eine lokale Webanwendung migrieren, die derzeit mit einem Legacyprodukt für die Webzugriffsverwaltung (CA SiteMinder) geschützt wird, und wie Sie Azure AD für die Authentifizierung und Zugriffssteuerung verwenden.
1. Installieren von Maverics Identity Orchestrator&trade;
2. Registrieren Ihrer Unternehmensanwendung bei Azure AD und konfigurieren der Anwendung für die Verwendung von Maverics Azure AD SAML Zero Code Connector&trade; für SAML-basiertes einmaliges Anmelden
3. Integrieren von Maverics in SiteMinder und in den LDAP-Benutzerspeicher
4. Einrichten von Azure Key Vault und Konfigurieren von Maverics für die Verwendung von Key Vault als Geheimnisverwaltungsanbieter
5. Veranschaulichen der Benutzermigration und Sitzungsabstraktion unter Verwendung von Maverics, um den Zugriff auf eine lokale Java-Webanwendung zu ermöglichen

Weitere Installations- und Konfigurationsanweisungen finden Sie unter https://strata.io/docs.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
- SSO-fähiges Maverics Identity Orchestrator SAML Connector-Abonnement. Wenden Sie sich an sales@strata.io, um die Maverics-Software zu beziehen.

## <a name="install-maverics-identity-orchestratortrade"></a>Installieren von Maverics Identity Orchestrator&trade;

Informationen zur Installation von Maverics Identity Orchestrator finden Sie in der Installationsanleitung unter https://strata.io/docs.

## <a name="system-requirements"></a>Systemanforderungen
### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
* RHEL 7+
* CentOS 7+

### <a name="dependencies"></a>Abhängigkeiten
* systemd

## <a name="installation"></a>Installation

1. Besorgen Sie sich das aktuelle Maverics-RPM-Paket. Kopieren Sie das Paket auf das System, auf dem Sie die Maverics-Software installieren möchten.

2. Installieren Sie das Maverics-Paket. Ersetzen Sie dabei `maverics.rpm` durch Ihren Dateinamen:

    `sudo rpm -Uvf maverics.rpm`

3. Nach der Installation wird Maverics als Dienst unter `systemd` ausgeführt. Führen Sie den folgenden Befehl aus, um sich zu vergewissern, dass der Dienst ausgeführt wird:

    `sudo systemctl status maverics`

Maverics wird standardmäßig im Verzeichnis `/usr/local/bin` installiert.

Nach der Installation von Maverics wird im Verzeichnis `/etc/maverics` die Standarddatei `maverics.yaml` erstellt. Bevor Sie die Konfiguration bearbeiten, um `workflows` und `connectors` einzuschließen, sieht die Konfigurationsdatei wie folgt aus:

```yaml
# © Strata Identity Inc. 2020. All Rights Reserved. Patents Pending.

version: 0.1
listenAddress: ":7474"
```
## <a name="config-options"></a>Konfigurationsoptionen
### <a name="version"></a>Version
`version` deklariert, welche Version der Konfigurationsdatei verwendet wird. Ohne Angabe wird die aktuelle Konfigurationsversion verwendet.

```yaml
version: 0.1
```
### <a name="listen-address"></a>Listen Address
`listenAddress` deklariert, an welcher Adresse der Orchestrator lauscht. Ist der Hostabschnitt der Adresse leer, lauscht der Orchestrator an allen verfügbaren Unicast- und Anycast-IP-Adressen des lokalen Systems. Ist der Portabschnitt der Adresse leer, wird automatisch eine Portnummer ausgewählt.

```yaml
listenAddress: ":453"
```
### <a name="tls"></a>TLS

`tls` deklariert eine Zuordnung von TLS-Objekten (Transport Layer Security). Die TLS-Objekte können sowohl von Connectors als auch vom Orchestrator-Server verwendet werden. Informationen zu den verfügbaren TLS-Optionen finden Sie in der Paketdokumentation für `transport`.

Bei Verwendung von SAML-basiertem einmaligem Anmelden muss Microsoft Azure über TLS kommunizieren können. Weitere Informationen zur Zertifikatgenerierung finden Sie [hier](https://letsencrypt.org/getting-started/).

Der Schlüssel `maverics` ist für den Orchestrator-Server reserviert. Alle anderen Schlüssel sind verfügbar und können verwendet werden, um ein TLS-Objekt in einen bestimmten Connector einzufügen.

```yaml
tls:
  maverics:
    certFile: /etc/maverics/maverics.cert
    keyFile: /etc/maverics/maverics.key
```  
### <a name="include-files"></a>Includedateien

`connectors` und `workflows` können in eigenen, separaten Konfigurationsdateien definiert werden, auf die in `maverics.yaml` mithilfe von `includeFiles` verwiesen werden kann, wie im folgenden Beispiel zu sehen:
```yaml
includeFiles:
  - workflow/sessionAbstraction.yaml
  - connector/AzureAD-saml.yaml
  - connector/siteminder.yaml
  ```

In diesem Tutorial wird eine einzelne Konfigurationsdatei (`maverics.yaml`) verwendet.

## <a name="using-azure-key-vault-as-your-secrets-provider"></a>Verwenden von Azure Key Vault als Geheimnisanbieter

### <a name="secret-management"></a>Verwaltung von Geheimnissen

Maverics kann zum Laden von Geheimnissen in verschiedene Geheimnisverwaltungslösungen integriert werden. Die aktuellen Integrationen umfassen eine Datei, HashiCorp Vault und Azure Key Vault. Ohne Angabe einer Geheimnisverwaltungslösung werden Geheimnisse von Maverics standardmäßig im Klartext aus `maverics.yaml` geladen.
Wenn Sie einen Wert in einer Konfigurationsdatei (`maverics.yaml`) als Geheimnis deklarieren möchten, umschließen Sie ihn mit spitzen Klammern:

  ```yaml
  connectors:
  - name: AzureAD
    type: AzureAD
    apiToken: <AzureADAPIToken>
    oauthClientID: <AzureADOAuthClientID>
    oauthClientSecret: <AzureADOAuthClientSecret>
  ```

### <a name="file"></a>Datei

Wenn Sie Geheimnisse aus einer Datei laden möchten, fügen Sie in der Datei `/etc/maverics/maverics.env` wie folgt die Umgebungsvariable `MAVERICS_SECRET_PROVIDER` hinzu:

`MAVERICS_SECRET_PROVIDER=secretfile:///<PATH TO SECRETS FILE>`

Starten Sie dann den Maverics-Dienst neu: `sudo systemctl restart maverics`

Der Inhalt der Datei `secrets.yaml` kann mit einer beliebigen Anzahl von Geheimnissen (`secrets`) gefüllt werden.
```yaml
secrets:
  AzureADAPIToken: aReallyGoodToken
  AzureADOAuthClientID: aReallyUniqueID
  AzureADOAuthClientSecret: aReallyGoodSecret
```
### <a name="azure-key-vault"></a>Azure-Schlüsseltresor

In den folgenden Schritten wird gezeigt, wie Sie über das [Azure-Portal](https://portal.azure.com) oder mithilfe der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) eine Azure Key Vault-Instanz einrichten:

1. [Melden Sie sich an](https://portal.azure.com) – entweder über das Azure-Portal oder unter Verwendung des folgenden CLI-Befehls:
    ```shell
    az login
    ```

2. [Erstellen Sie einen neuen Tresor](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#create-a-vault), oder verwenden Sie den folgenden CLI-Befehl:
    ```shell
    az keyvault create --name "[VAULT_NAME]" --resource-group "[RESOURCE_GROUP]" --location "[REGION]"
    ```

3. [Fügen Sie Key Vault die Geheimnisse hinzu](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#add-a-secret-to-key-vault), oder verwenden Sie einen CLI-Befehl:
    ```shell
    az keyvault secret set --vault-name "[VAULT_NAME]" --name "[SECRET_NAME]" --value "[SECRET_VALUE]"
    ```

4. [Registrieren Sie eine Anwendung bei Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application), oder verwenden Sie einen CLI-Befehl:
    ```shell
    az ad sp create-for-rbac -n "MavericsKeyVault" --skip-assignment > azure-credentials.json
    ```

5. [Autorisieren Sie eine Anwendung für die Verwendung eines Geheimnisses](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#add-a-secret-to-key-vault), oder verwenden Sie einen CLI-Befehl:
    ```shell
    az keyvault set-policy --name "[VAULT_NAME]" --spn [APPID] --secret-permissions list get
    #APPID can be found in the azure-credentials.json
    generated in the previous step
    ```

Wenn Sie Geheimnisse aus Azure Key Vault laden möchten, legen Sie in der Datei `/etc/maverics/maverics.env` die Umgebungsvariable `MAVERICS_SECRET_PROVIDER` fest, und verwenden Sie dabei die Anmeldeinformationen aus der Datei „azure-credentials.json“ im folgenden Format: `MAVERICS_SECRET_PROVIDER='azurekeyvault://<KEYVAULT NAME>.vault.azure.net?clientID=<APPID>&clientSecret=<PASSWORD>&tenantID=<TENANT>'`

Starten Sie dann den Maverics-Dienst neu: `sudo systemctl restart maverics`

## <a name="configure-your-application-in-azure-ad-for-saml-based-sso"></a>Konfigurieren Ihrer Anwendung in Azure AD für SAML-basiertes einmaliges Anmelden

1. Navigieren Sie in Ihrem Azure Active Directory-Mandanten zu `Enterprise applications`, suchen Sie nach `Maverics Identity Orchestrator SAML Connector`, und wählen Sie die entsprechende Option aus.

2. Legen Sie auf der Eigenschaftenseite für „Maverics Identity Orchestrator SAML Connector“ die Option `User assignment required?` auf „Nein“ fest, damit die Anwendung für neu migrierte Benutzer verwendet werden kann.

3. Wählen Sie auf der Übersichtsseite für „Maverics Identity Orchestrator SAML Connector“ die Option `Setup single sign-on` und anschließend `SAML` aus.

4. Bearbeiten Sie im SAML-basierten einmaligen Anmelden für „Maverics Identity Orchestrator SAML Connector“ die grundlegende SAML-Konfiguration.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

5. Legen Sie die Entitäts-ID (`Entity ID`) fest, indem Sie eine URL im folgenden Format eingeben: `https://<SUBDOMAIN>.maverics.org`. Die Angabe für `Entity ID` muss für alle Apps im Mandanten eindeutig sein. Speichern Sie den hier eingegebenen Wert, um ihn in die Konfiguration von Maverics einschließen zu können.

6. Legen Sie die Antwort-URL im folgenden Format fest: `https://<AZURECOMPANY.COM>/<MY_APP>/`. 

7. Legen Sie die Anmelde-URL im folgenden Format `https://<AZURE-COMPANY.COM>/<MY_APP>/<LOGIN PAGE>` fest, und klicken Sie anschließend auf „Speichern“.

8. Klicken Sie im Abschnitt „SAML-Signaturzertifikat“ auf die Kopierschaltfläche, um die App-Verbundmetadaten-URL zu kopieren, und speichern Sie sie auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/copy-metadataurl.png)

## <a name="maverics-identity-orchestrator-azure-ad-saml-connector-configuration"></a>Konfiguration des Azure AD-SAML-Connectors für Maverics Identity Orchestrator

Vom Azure AD-SAML-Connector für Maverics Identity Orchestrator wird Folgendes unterstützt: 
- OpenID Connect
- SAML Connect 

1. Legen Sie zum Aktivieren des SAML-basierten einmaliges Anmeldens `authType: saml` fest.

1. Erstellen Sie den Wert für `samlMetadataURL`: `samlMetadataURL:https://login.microsoftonline.com/<TENANT ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<APP ID>`

1. Definieren Sie nun die URL, an die Benutzer in Ihrer App durch Azure zurückgeleitet werden, nachdem sie sich mit ihren Azure-Anmeldeinformationen angemeldet haben.
`samlRedirectURL: https://<AZURECOMPANY.COM>/<MY_APP>`

1. Kopieren Sie den Wert der weiter oben definierten Entitäts-ID: `samlEntityID: https://<SUBDOMAIN>.maverics.org`

1. Kopieren Sie den Wert der Antwort-URL, die von Azure AD verwendet wird, um den POST-Vorgang für die SAML-Antwort auszuführen.
`samlConsumerServiceURL: https://<AZURE-COMPANY.COM>/<MY_APP>`

1. Generieren Sie einen JWT-Signaturschlüssel zum Schutz der Sitzungsinformationen für Maverics Identity Orchestrator&trade;. Verwenden Sie dazu das [OpenSSL-Tool](https://www.openssl.org/source/):

    ```shell 
    openssl rand 64 | base64
    ```
1. Kopieren Sie die Antwort in die Konfigurationseigenschaft `jwtSigningKey`: `jwtSigningKey: TBHPvTtu6NUqU84H3Q45grcv9WDJLHgTioqRhB8QGiVzghKlu1mHgP1QHVTAZZjzLlTBmQwgsSoWxGHRcT4Bcw==`

## <a name="attributes-and-attribute-mapping"></a>Attribute und Attributzuordnung
Die Attributzuordnung wird verwendet, um bei der Benutzerbereitstellung die Zuordnung zwischen Benutzerattributen aus einem lokalen Quellbenutzerverzeichnis und Azure AD zu definieren.

Attribute bestimmen die Benutzerdaten, die in einem Anspruch an eine Anwendung zurückgegeben, an Sitzungscookies übergeben oder in HTTP-Headervariablen an die Anwendung übergeben werden können.

## <a name="configure-maverics-identity-orchestrator-azure-ad-saml-connector-yaml"></a>Konfigurieren des Azure AD-SAML-Connectors für Maverics Identity Orchestrator (YAML)

Die Konfiguration Ihres Azure AD-SAML-Connectors für Maverics Identity Orchestrator sieht wie folgt aus:
```yaml
- name: AzureAD
  type: azure
  authType: saml
  samlMetadataURL: https://login.microsoftonline.com/<TENANT ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<APP ID>
  samlRedirectURL: https://<AZURECOMPANY.COM>/<MY_APP>
  samlConsumerServiceURL: https://<AZURE-COMPANY.COM>/<MY_APP>
  jwtSigningKey: <SIGNING KEY>
  samlEntityID: https://<SUBDOMAIN>.maverics.org
  attributeMapping:
    displayName: username
    mailNickname: givenName
    givenName: givenName
    surname: sn
    userPrincipalName: mail
    password: password
```

## <a name="migrate-users-to-azure-ad"></a>Migrieren von Benutzern zu Azure AD

Gehen Sie wie hier beschrieben vor, um Benutzer inkrementell aus einem Produkt für die Webzugriffsverwaltung (beispielsweise CA SiteMinder, Oracle Access Manager oder IBM Tivoli), aus einem LDAP-Verzeichnis oder aus einer SQL-Datenbank zu migrieren.

## <a name="configure-your-application-permissions-in-azure-ad-to-create-users"></a>Konfigurieren Ihrer Anwendungsberechtigungen in Azure AD für die Benutzererstellung

1. Navigieren Sie in Ihrem Azure Active Directory-Mandanten zu `App registrations`, und wählen Sie die Anwendung „Maverics Identity Orchestrator SAML Connector“ aus.

2. Wählen Sie in den Zertifikaten und Geheimnissen für „Maverics Identity Orchestrator SAML Connector“ die Option `New client secret` und anschließend eine Ablaufoption aus. Klicken Sie auf die Kopierschaltfläche, um das Geheimnis zu kopieren, und speichern Sie es auf Ihrem Computer.

3. Wählen Sie in den API-Berechtigungen für „Maverics Identity Orchestrator SAML Connector“ die Option `Add permission` und anschließend unter „API-Berechtigungen anfordern“ Folgendes aus: `Microsoft Graph` > `Application permissions`. Wählen Sie auf dem nächsten Bildschirm `User.ReadWrite.All` und anschließend `Add permissions` aus. Daraufhin werden wieder die API-Berechtigungen angezeigt. Wählen Sie hier `Grant admin consent` aus.


## <a name="configure-the-maverics-identity-orchestrator-saml-connector-yaml-for-user-migration"></a>Konfigurieren des SAML-Connectors für Maverics Identity Orchestrator (YAML) für die Benutzermigration

Fügen Sie der Konfigurationsdatei die folgenden zusätzlichen Eigenschaften hinzu, um den Workflow für die Benutzermigration zu aktivieren:
1. Legen Sie die Azure Graph-URL fest: `graphURL: https://graph.microsoft.com`
1. Legen Sie die OAuth-Token-URL im folgenden Format fest: `oauthTokenURL: https://login.microsoftonline.com/<TENANT ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<APP ID>`
1. Legen Sie den weiter oben generierten geheimen Clientschlüssel fest: `oauthClientSecret: <CLIENT SECRET>`


Die finale Konfiguration Ihres Azure AD-SAML-Connectors für Maverics Identity Orchestrator sieht wie folgt aus:
```yaml
- name: AzureAD
  type: azure
  authType: saml
  samlMetadataURL: https://login.microsoftonline.com/<TENANT ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<APP ID>
  samlRedirectURL: https://<AZURECOMPANY.COM>/<MY_APP>
  samlConsumerServiceURL: https://<AZURE-COMPANY.COM>/<MY_APP>
  jwtSigningKey: TBHPvTtu6NUqU84H3Q45grcv9WDJLHgTioqRhB8QGiVzghKlu1mHgP1QHVTAZZjzLlTBmQwgsSoWxGHRcT4Bcw==
  samlEntityID: https://<SUBDOMAIN>.maverics.org
  graphURL: https://graph.microsoft.com
  oauthTokenURL: https://login.microsoftonline.com/<TENANT ID>/oauth2/v2.0/token
  oauthClientID: <APP ID>
  oauthClientSecret: <NEW CLIENT SECRET>
  attributeMapping:
    displayName: username
    mailNickname: givenName
    givenName: givenName
    surname: sn
    userPrincipalName: mail
    password: password
```

## <a name="configure-the-maverics-zero-code-connectortrade-for-siteminder"></a>Konfigurieren von Maverics Zero Code Connector&trade; für SiteMinder

Der SiteMinder-Connector wird verwendet, um Benutzer zu Azure AD zu migrieren und unter Verwendung der neu erstellten Azure AD-Identitäten und -Anmeldeinformationen bei lokalen, durch SiteMinder geschützten Legacyanwendungen anzumelden.

Bei der SiteMinder-Konfiguration für dieses Tutorial wird die Legacyanwendung mittels formularbasierter Authentifizierung und unter Verwendung des Cookies `SMSESSION` geschützt. Wenn Sie eine Integration für eine App mit auf HTTP-Headern basierender Authentifizierung und Sitzung benötigen, muss dem Connector die Headeremulationskonfiguration hinzugefügt werden.

In diesem Beispiel wird das Attribut `username` dem HTTP-Header `SM_USER` zugeordnet:
```yaml
  headers:
    SM_USER: username
```

Legen Sie `proxyPass` auf den Speicherort fest, an den Anforderungen über einen Proxy gesendet werden. Hierbei handelt es sich in der Regel um den Host der geschützten Anwendung.

`loginPage` muss der URL des Anmeldeformulars entsprechen, das zurzeit von SiteMinder verwendet wird, um Benutzer für die Authentifizierung umzuleiten.

```yaml
connectors:
- name: siteminder-login-form
  type: siteminder
  loginType: form
  loginPage: /siteminderagent/forms/login.fcc
  proxyPass: http://host.company.com
```

## <a name="configure-the-maverics-zero-code-connectortrade-for-ldap"></a>Konfigurieren von Maverics Zero Code Connector&trade; für LDAP

Wenn Anwendungen durch ein WAM-Produkt wie SiteMinder geschützt sind, werden Benutzeridentitäten und Attribute in der Regel in einem LDAP-Verzeichnis gespeichert.

Diese Connectorkonfiguration veranschaulicht, wie Sie eine Verbindung mit dem LDAP-Verzeichnis herstellen, das als Benutzerspeicher für SiteMinder konfiguriert ist, damit im Rahmen des Migrationsworkflows die korrekten Benutzerprofilinformationen erfasst werden können und in Azure AD ein entsprechender Benutzer erstellt werden kann.

* `baseDN` gibt den Ort in dem Verzeichnis an, für das die LDAP-Suche durchgeführt werden soll.

* `url` dient zum Angeben der Adresse und des Ports des LDAP-Servers für die Verbindungsherstellung.

* `serviceAccountUsername` ist der Benutzername für die Verbindungsherstellung mit dem LDAP-Server und wird in der Regel als Bindungs-DN ausgedrückt (Beispiel: `CN=Directory Manager`).

* `serviceAccountPassword` ist das Kennwort für die Verbindungsherstellung mit dem LDAP-Server. Dieser Wert wird in der zuvor konfigurierten Azure Key Vault-Instanz gespeichert.  

* `userAttributes` definiert die Liste der abzufragenden benutzerbezogenen Attribute. Diese Attribute werden später den entsprechenden Azure AD-Attributen zugeordnet.

```yaml
- name: company-ldap
  type: ldap
  url: "ldap://ldap.company.com:389"
  baseDN: ou=People,o=company,c=US
  serviceAccountUsername: uid=admin,ou=Admins,o=company,c=US
  serviceAccountPassword: <vaulted-password>
  userAttributes:
    - uid
    - cn
    - givenName
    - sn
    - mail
    - mobile
```

## <a name="configure-the-migration-workflow"></a>Konfigurieren des Migrationsworkflows

Die Konfiguration des Migrationsworkflows bestimmt, wie Benutzer durch Maverics aus SiteMinder/LDAP zu Azure AD migriert werden.

Für diesen Workflow gilt Folgendes:
- Er verwendet den SiteMinder-Connector als Proxy für die SiteMinder-Anmeldung. Benutzeranmeldeinformationen werden per SiteMinder-Authentifizierung überprüft und anschließend an weitere Schritte des Workflows übergeben.
- Er ruft Benutzerprofilattribute aus dem SiteMinder-Benutzerspeicher ab.
- Er sendet eine Anforderung an die Microsoft Graph-API, um den Benutzer in Ihrem Azure AD-Mandanten zu erstellen.

Schritte:
1. Geben Sie dem Workflow einen Namen (beispielsweise „SiteMinder to Azure AD Migration“).
2. Geben Sie den Endpunkt (`endpoint`) an. Hierbei handelt es sich um einen HTTP-Pfad, über den der Workflow verfügbar gemacht, der die angeforderten Aktionen (`actions`) des Workflows auslöst. Der Endpunkt (`endpoint`) entspricht in der Regel der App, für die ein Proxy verwendet wird (beispielsweise `/my_app`). Der Wert muss sowohl den vorangestellten als auch den nachgestellten Schrägstrich enthalten.
3. Fügen Sie dem Workflow die passenden Aktionen (`actions`) hinzu.
    - Definieren Sie die Methode `login` für den SiteMinder-Connector. Der Connectorwert muss dem Namenswert in der Connectorkonfiguration entsprechen.
     - Definieren Sie die Methode `getprofile` für den LDAP-Connector.
     - Definieren Sie `createuser` für den Azure AD-Connector.

    ```yaml
      workflows:
      - name: SiteMinder to Azure AD Migration
        endpoint: /my_app/
        actions:
        - connector: siteminder-login-form
          method: login
        - connector: company-ldap
          method: getprofile
        - connector: AzureAD
          method: createuser
    ```
### <a name="verify-the-migration-workflow"></a>Überprüfen des Migrationsworkflows

1. Sollte der Maverics-Dienst noch nicht ausgeführt werden, starten Sie ihn mithilfe des folgenden Befehls: `sudo systemctl start maverics`

2. Navigieren Sie zu der Anmelde-URL, für die ein Proxy eingerichtet wurde: `http://host.company.com/my_app`.
3. Geben Sie Benutzeranmeldeinformationen für die Anmeldung bei der durch SiteMinder geschützten Anwendung an.
4. Navigieren Sie zu „Start“ > „Benutzer“ > „Alle Benutzer“, und vergewissern Sie sich, dass Ihr Benutzer im Azure AD-Mandanten erstellt wurde.  

## <a name="configure-the-session-abstraction-workflow"></a>Konfigurieren des Workflows für die Sitzungsabstraktion

Durch den Workflow für die Sitzungsabstraktion werden Authentifizierung und Zugriffssteuerung für die lokale Legacywebanwendung zu Azure AD migriert.

Der Azure-Connector verwendet die Methode `login`, um den Benutzer zur Anmelde-URL umzuleiten, sofern keine Sitzung vorhanden ist.

Nach der Authentifizierung wird das erstellte Sitzungstoken an Maverics übergeben. Anschließend wird die Methode `emulate` des SiteMinder-Connectors verwendet, um die cookie- und/oder headerbasierte Sitzung zu emulieren und die Anforderung mit allen zusätzlichen Attributen zu versehen, die für die Anwendung erforderlich sind.

1. Geben Sie dem Workflow einen Namen (beispielsweise „SiteMinder Session Abstraction“).
2. Geben Sie den Endpunkt (`endpoint`) an, der der App entspricht, für die ein Proxy verwendet wird. Der Wert muss sowohl den vorangestellten als auch den nachgestellten Schrägstrich enthalten (Beispiel: `/my_app/`).
3. Fügen Sie dem Workflow die passenden Aktionen (`actions`) hinzu.
    - Definieren Sie die Methode `login` für den Azure-Connector. Der Connectorwert (`connector`) muss dem Namenswert (`name`) in der Connectorkonfiguration entsprechen.
    - Definieren Sie die Methode `emulate` für den SiteMinder-Connector.

     ```yaml
      - name: SiteMinder Session Abstraction
        endpoint: /my_app/
        actions:
      - connector: azure
        method: login
      - connector: siteminder-login-form
        method: emulate
     ```
### <a name="verify-the-session-abstraction-workflow"></a>Überprüfen des Workflows für die Sitzungsabstraktion

1. Navigieren Sie zur URL der Anwendung mit dem Proxy: `https://<AZURECOMPANY.COM>/<MY_APP>`. Der Benutzer wird zur Proxyanmeldeseite umgeleitet.
2. Geben Sie die Azure AD-Benutzeranmeldeinformationen ein.
3. Der Benutzer sollte zur Anwendung umgeleitet werden, als hätte er sich direkt bei SiteMinder authentifiziert.
