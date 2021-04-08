---
title: 'Tutorial: Integration des einmaligen Anmeldens von Azure Active Directory mit Maverics Identity Orchestrator SAML Connector | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Maverics Identity Orchestrator SAML Connector konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/17/2021
ms.author: jeedes
ms.openlocfilehash: 19f6b0601afe9ad84f02c93d7f6e1ae3a71a06a4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104585093"
---
# <a name="integrate-azure-ad-single-sign-on-with-maverics-identity-orchestrator-saml-connector"></a>Integration des einmaligen Anmeldens von Azure AD mit Maverics Identity Orchestrator SAML Connector

Maverics Identity Orchestrator von Strata bietet eine einfache Möglichkeit, lokale Anwendungen zur Authentifizierung und Zugriffssteuerung in Azure Active Directory (Azure AD) zu integrieren. Maverics Orchestrator ermöglicht die Modernisierung der Authentifizierung und Autorisierung für Apps, die derzeit mit Headern, Cookies und anderen proprietären Authentifizierungsmethoden arbeiten. Maverics Orchestrator-Instanzen können lokal oder in der Cloud bereitgestellt werden. 

In diesem Hybridzugriffstutorial erfahren Sie, wie Sie eine lokale Webanwendung migrieren, die derzeit durch ein Legacyprodukt für die Webzugriffsverwaltung geschützt wird. Außerdem lernen Sie, wie Sie Azure AD für die Authentifizierung und Zugriffssteuerung verwenden. Im Folgenden finden Sie die grundlegenden Schritte:

1. Einrichten von Maverics Orchestrator
1. Verwenden eines Proxys für eine Anwendung
1. Registrieren einer Unternehmensanwendung in Azure AD
1. Authentifizieren über Azure und Autorisieren des Zugriffs auf die Anwendung
1. Hinzufügen von Headern für nahtlosen Anwendungszugriff
1. Verwenden mehrerer Anwendungen

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein SSO-fähiges Maverics Identity Orchestrator SAML Connector-Abonnement. Wenden Sie sich an das [Strata-Vertriebsteam](mailto:sales@strata.io), um die Maverics-Software zu erhalten.
* Mindestens eine Anwendung mit headerbasierter Authentifizierung. In den Beispielen werden eine Anwendung namens Sonar (gehostet unter https://app.sonarsystems.com ) und eine Anwendung namens Connectulum (gehostet unter https://app.connectulum.com ) verwendet.
* Ein Linux-Computer zum Hosten von Maverics Orchestrator
  * Betriebssystem: RHEL 7.7 oder höher, CentOS 7 oder höher
  * Datenträger: mind. 10 GB
  * Arbeitsspeicher: mind. 4 GB
  * Ports: 22 (SSH/SCP), 443, 7474
  * Stammzugriff für Installations- und Verwaltungsaufgaben
  * Ausgehender Netzwerkdatenverkehr des Servers, auf dem Maverics Identity Orchestrator gehostet wird, an Ihre geschützte Anwendung

## <a name="step-1-set-up-the-maverics-orchestrator"></a>Schritt 1: Einrichten von Maverics Orchestrator

### <a name="install-maverics"></a>Installieren von Maverics

1. Besorgen Sie sich das aktuelle Maverics-RPM-Paket. Kopieren Sie das Paket auf das System, auf dem Sie die Maverics-Software installieren möchten.

1. Installieren Sie das Maverics-Paket, und ersetzen Sie dabei `maverics.rpm` durch den Namen Ihrer Datei.

   `sudo rpm -Uvf maverics.rpm`

   Nach der Installation wird Maverics als Dienst unter `systemd` ausgeführt. Führen Sie den folgenden Befehl aus, um sich zu vergewissern, dass der Dienst ausgeführt wird:

   `sudo systemctl status maverics`

1. Führen Sie den folgenden Befehl aus, um den Orchestrator neu zu starten und die Protokolle zu verfolgen:

   `sudo service maverics restart; sudo journalctl --identifier=maverics -f`

Nach der Installation von Maverics wird die Standarddatei `maverics.yaml` im Verzeichnis `/etc/maverics` erstellt. Vor dem Hinzufügen von `appgateways` und `connectors` sieht die Konfigurationsdatei wie folgt aus:

```yaml
# © Strata Identity Inc. 2020. All Rights Reserved. Patents Pending.

version: 0.1
listenAddress: ":7474"
```

### <a name="configure-dns"></a>Konfigurieren des DNS

DNS sorgt dafür, dass Sie sich die IP-Adresse des Orchestrator-Servers nicht merken müssen.

Bearbeiten Sie die Hostdatei des Browsercomputers (Ihr Laptop), und verwenden Sie dabei die hypothetische Orchestrator-IP-Adresse 12.34.56.78. Bei Linux-basierten Betriebssystemen befindet sich diese Datei unter `/etc/hosts`. Unter Windows befindet sich die Datei unter `C:\windows\system32\drivers\etc`.

```
12.34.56.78 sonar.maverics.com
12.34.56.78 connectulum.maverics.com
```

Vergewissern Sie sich, dass das DNS erwartungsgemäß konfiguriert ist, indem Sie eine Anforderung an den Statusendpunkt des Orchestrator senden. Fordern Sie über Ihren Browser Folgendes an: http://sonar.maverics.com:7474/status.

### <a name="configure-tls"></a>Konfigurieren von TLS

Aus Sicherheitsgründen ist es wichtig, dass die Kommunikation mit Ihrem Orchestrator über sichere Kanäle erfolgt. Dazu können Sie im Abschnitt `tls` ein Zertifikat-Schlüssel-Paar hinzufügen.

Führen Sie im Verzeichnis `/etc/maverics` den folgenden Befehl aus, um ein selbstsigniertes Zertifikat und einen Schlüssel für den Orchestrator-Server zu generieren:

`openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out maverics.crt -keyout maverics.key`

> [!NOTE]
> In Produktionsumgebungen empfiehlt sich in der Regel die Verwendung eines Zertifikats, das von einer bekannten Zertifizierungsstelle signiert wurde, um Warnungen im Browser zu vermeiden. [Let's Encrypt](https://letsencrypt.org/) ist eine gute und kostenlose Option für eine vertrauenswürdige Zertifizierungsstelle.

Verwenden Sie nun das neu generierte Zertifikat und den Schlüssel für den Orchestrator. Ihre Konfigurationsdatei sollte nun folgenden Code enthalten:

```yaml
version: 0.1
listenAddress: ":443"

tls:
  maverics:
    certFile: /etc/maverics/maverics.crt
    keyFile: /etc/maverics/maverics.key
```

Starten Sie den Maverics-Dienst neu, und senden Sie eine Anforderung an den Statusendpunkt, um sich zu vergewissern, dass TLS wie erwartet konfiguriert ist. Fordern Sie über Ihren Browser Folgendes an: https://sonar.maverics.com/status.

## <a name="step-2-proxy-an-application"></a>Schritt 2: Verwenden eines Proxys für eine Anwendung

Konfigurieren Sie als Nächstes im Orchestrator mithilfe von `appgateways` einen grundlegenden Proxy. Mit diesem Schritt können Sie sich vergewissern, dass der Orchestrator über die erforderliche Konnektivität mit der geschützten Anwendung verfügt.

Ihre Konfigurationsdatei sollte nun folgenden Code enthalten:

```yaml
version: 0.1
listenAddress: ":443"

tls:
  maverics:
    certFile: /etc/maverics/maverics.crt
    keyFile: /etc/maverics/maverics.key

appgateways:
  - name: sonar
    location: /
    # Replace https://app.sonarsystems.com with the address of your protected application
    upstream: https://app.sonarsystems.com
```

Starten Sie den Maverics-Dienst neu, und senden Sie über den Maverics-Proxy eine Anforderung an die Anwendung, um sich zu vergewissern, dass der Proxy wie erwartet funktioniert. Fordern Sie über Ihren Browser Folgendes an: https://sonar.maverics.com. Sie können optional auch eine Anforderung an bestimmte Anwendungsressourcen senden (beispielsweise an `https://sonar.maverics.com/RESOURCE`, wobei `RESOURCE` eine gültige Anwendungsressource der geschützten Upstream-App ist).

## <a name="step-3-register-an-enterprise-application-in-azure-ad"></a>Schritt 3: Registrieren einer Unternehmensanwendung in Azure AD

Erstellen Sie nun in Azure AD eine neue Unternehmensanwendung für die Authentifizierung von Endbenutzern.

> [!NOTE]
> Bei Verwendung von Azure AD-Features wie bedingtem Zugriff ist es wichtig, jeweils eine Unternehmensanwendung pro lokaler Anwendung zu erstellen. Dies ermöglicht unter anderem App-spezifischen bedingten Zugriff, App-spezifische Risikobewertungen sowie App-spezifische Berechtigungszuweisungen. Eine Unternehmensanwendung in Azure AD wird im Allgemeinen einem Azure-Connector in Maverics zugeordnet.

So registrieren Sie eine Unternehmensanwendung in Azure AD:

1. Navigieren Sie in Ihrem Azure AD-Mandanten zu **Unternehmensanwendungen**, und wählen Sie **Neue Anwendung** aus. Suchen Sie im Azure AD-Katalog nach **Maverics Identity Orchestrator SAML Connector**, und wählen Sie diese Option aus.

1. Legen Sie im Bereich **Eigenschaften** von „Maverics Identity Orchestrator SAML Connector“ die Option **Benutzerzuweisung erforderlich?** auf **Nein** fest, damit die Anwendung für alle Benutzer in Ihrem Verzeichnis verwendet werden kann.

1. Wählen Sie im Bereich **Übersicht** für den Maverics Identity Orchestrator SAML Connector die Option **Einmaliges Anmelden einrichten** und danach **SAML** aus.

1. Bearbeiten Sie im Bereich **SAML-basiertes einmaliges Anmelden** für den Maverics Identity Orchestrator SAML Connector die **grundlegende SAML-Konfiguration**, indem Sie auf die Schaltfläche **Bearbeiten** (Bleistiftsymbol) klicken.

   ![Screenshot der Bearbeitungsschaltfläche für die grundlegende SAML-Konfiguration](common/edit-urls.png)

1. Geben Sie unter **Entitäts-ID** die ID `https://sonar.maverics.com` ein. Die Entitäts-ID muss für alle Apps des Mandanten eindeutig sein. Ansonsten kann es sich aber um einen beliebigen Wert handeln. Dieser Wert wird im nächsten Abschnitt zum Definieren des Felds `samlEntityID` für Ihren Azure-Connector verwendet.

1. Geben Sie unter **Antwort-URL** die URL `https://sonar.maverics.com/acs` ein. Dieser Wert wird im nächsten Abschnitt zum Definieren des Felds `samlConsumerServiceURL` für Ihren Azure-Connector verwendet.

1. Geben Sie unter **Anmelde-URL** die URL `https://sonar.maverics.com/` ein. Dieses Feld wird von Maverics zwar nicht verwendet, es ist jedoch in Azure AD erforderlich, um Benutzern über das Azure AD-Portal „Meine Apps“ Zugriff auf die Anwendung ermöglichen zu können.

1. Wählen Sie **Speichern** aus.

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche **Kopieren**, um den Wert von **App-Verbundmetadaten-URL** zu kopieren. Speichern Sie die URL anschließend auf Ihrem Computer.

   ![Screenshot: „SAML-Signaturzertifikat“ mit hervorgehobener Kopierschaltfläche](common/copy-metadataurl.png)

## <a name="step-4-authenticate-via-azure-and-authorize-access-to-the-application"></a>Schritt 4: Authentifizieren über Azure und Autorisieren des Zugriffs auf die Anwendung

Konfigurieren Sie als Nächstes den Azure-Connector in Maverics, um die soeben erstellte Unternehmensanwendung zu nutzen. Die Kombination aus der Konfiguration `connectors` und dem Block `idps` ermöglicht es dem Orchestrator, Benutzer zu authentifizieren.

Ihre Konfigurationsdatei sollte nun den folgenden Code enthalten. Wichtig: Ersetzen Sie `METADATA_URL` durch die App-Verbundmetadaten-URL aus dem vorherigen Schritt.

```yaml
version: 0.1
listenAddress: ":443"

tls:
  maverics:
    certFile: /etc/maverics/maverics.crt
    keyFile: /etc/maverics/maverics.key

idps:
  - name: azureSonarApp

appgateways:
  - name: sonar
    location: /
    # Replace https://app.sonarsystems.com with the address of your protected application
    upstream: https://app.sonarsystems.com

    policies:
      - resource: /
        allowIf:
          - equal: ["{{azureSonarApp.authenticated}}", "true"]

connectors:
  - name: azureSonarApp
    type: azure
    authType: saml
    # Replace METADATA_URL with the App Federation Metadata URL
    samlMetadataURL: METADATA_URL
    samlConsumerServiceURL: https://sonar.maverics.com/acs
    samlEntityID: https://sonar.maverics.com
```

Starten Sie den Maverics-Dienst neu, und senden Sie über den Maverics-Proxy eine Anforderung an eine Anwendungsressource, um sich zu vergewissern, dass die Authentifizierung wie erwartet funktioniert. Sie sollten zwecks Authentifizierung zu Azure umgeleitet werden, bevor Sie auf die Ressource zugreifen können.

## <a name="step-5-add-headers-for-seamless-application-access"></a>Schritt 5: Hinzufügen von Headern für nahtlosen Anwendungszugriff

Bislang werden noch keine Header an die Upstreamanwendung gesendet. Fügen Sie der Anforderung `headers` hinzu, wenn sie den Maverics-Proxy durchläuft, damit die Upstreamanwendung den Benutzer identifizieren kann.

Ihre Konfigurationsdatei sollte nun folgenden Code enthalten:

```yaml
version: 0.1
listenAddress: ":443"

tls:
  maverics:
    certFile: /etc/maverics/maverics.crt
    keyFile: /etc/maverics/maverics.key

idps:
  - name: azureSonarApp

appgateways:
  - name: sonar
    location: /
    # Replace https://app.sonarsystems.com with the address of your protected application
    upstream: https://app.sonarsystems.com

    policies:
      - resource: /
        allowIf:
          - equal: ["{{azureSonarApp.authenticated}}", "true"]

    headers:
      email: azureSonarApp.name
      firstname: azureSonarApp.givenname
      lastname: azureSonarApp.surname

connectors:
  - name: azureSonarApp
    type: azure
    authType: saml
    # Replace METADATA_URL with the App Federation Metadata URL
    samlMetadataURL: METADATA_URL
    samlConsumerServiceURL: https://sonar.maverics.com/acs
    samlEntityID: https://sonar.maverics.com
```

Senden Sie über den Maverics-Proxy eine Anforderung an eine Anwendungsressource, um sich zu vergewissern, dass die Authentifizierung wie erwartet funktioniert. Die geschützte Anwendung sollte nun Header in der Anforderung empfangen. 

Sollten von Ihrer Anwendung andere Header erwartet werden, können Sie die Headerschlüssel bearbeiten. Alle Ansprüche, die im Rahmen des SAML-Flows von Azure AD zurückgegeben werden, können in Headern verwendet werden. So können Sie beispielsweise einen Header vom Typ `secondary_email: azureSonarApp.email` hinzufügen. `azureSonarApp` ist hierbei der Connectorname und `email` ein von Azure AD zurückgegebener Anspruch. 

## <a name="step-6-work-with-multiple-applications"></a>Schritt 6: Verwenden mehrerer Anwendungen

In diesem Abschnitt erfahren Sie, was zur Verwendung eines Proxys für mehrere Anwendungen auf verschiedenen Hosts erforderlich ist. Konfigurieren Sie hierzu eine weitere App Gateway-Instanz, eine weitere Unternehmensanwendung in Azure AD und einen weiteren Connector.

Ihre Konfigurationsdatei sollte nun folgenden Code enthalten:

```yaml
version: 0.1
listenAddress: ":443"

tls:
  maverics:
    certFile: /etc/maverics/maverics.crt
    keyFile: /etc/maverics/maverics.key

idps:
  - name: azureSonarApp
  - name: azureConnectulumApp

appgateways:
  - name: sonar
    host: sonar.maverics.com
    location: /
    # Replace https://app.sonarsystems.com with the address of your protected application
    upstream: https://app.sonarsystems.com

    policies:
      - resource: /
        allowIf:
          - equal: ["{{azureSonarApp.authenticated}}", "true"]

    headers:
      email: azureSonarApp.name
      firstname: azureSonarApp.givenname
      lastname: azureSonarApp.surname

  - name: connectulum
    host: connectulum.maverics.com
    location: /
    # Replace https://app.connectulum.com with the address of your protected application
    upstream: https://app.connectulum.com

    policies:
      - resource: /
        allowIf:
          - equal: ["{{azureConnectulumApp.authenticated}}", "true"]

    headers:
      email: azureConnectulumApp.name
      firstname: azureConnectulumApp.givenname
      lastname: azureConnectulumApp.surname

connectors:
  - name: azureSonarApp
    type: azure
    authType: saml
    # Replace METADATA_URL with the App Federation Metadata URL
    samlMetadataURL: METADATA_URL
    samlConsumerServiceURL: https://sonar.maverics.com/acs
    samlEntityID: https://sonar.maverics.com

  - name: azureConnectulumApp
    type: azure
    authType: saml
    # Replace METADATA_URL with the App Federation Metadata URL
    samlMetadataURL: METADATA_URL
    samlConsumerServiceURL: https://connectulum.maverics.com/acs
    samlEntityID: https://connectulum.maverics.com
```

Wie Sie sehen, wird Ihren App Gateway-Definitionen durch den Code ein Feld vom Typ `host` hinzugefügt. Anhand des Felds `host` kann Maverics Orchestrator ermitteln, an welchen Upstreamhost Datenverkehr über den Proxy geleitet werden soll.

Senden Sie eine Anforderung an https://connectulum.maverics.com , um sich zu vergewissern, dass die neu hinzugefügte App Gateway-Instanz wie erwartet funktioniert.

## <a name="advanced-scenarios"></a>Erweiterte Szenarien

### <a name="identity-migration"></a>Identitätsmigration

Sie sind mit Ihrem veralteten Tool für Webzugriffsverwaltung nicht zufrieden, haben aber keine Möglichkeit, Ihre Benutzer ohne umfangreiche Kennwortzurücksetzungen zu migrieren? Maverics Orchestrator unterstützt die Migration von Identitäten mithilfe von `migrationgateways`.

### <a name="web-server-gateways"></a>Webservergateways

Sie möchten Ihren Netzwerk- und Proxydatenverkehr nicht mit Maverics Orchestrator überarbeiten? Kein Problem. Maverics Orchestrator kann mit Webservergateways (Modulen) kombiniert werden, um die gleichen Lösungen ohne Proxy zu erhalten.

## <a name="wrap-up"></a>Zusammenfassung

Sie haben nun Maverics Orchestrator installiert, eine Unternehmensanwendung in Azure AD erstellt und konfiguriert sowie den Orchestrator als Proxy für eine geschützte Anwendung mit erforderlicher Authentifizierung und Richtlinienerzwingung konfiguriert. Weitere Informationen zur Verwendung von Maverics Orchestrator für Anwendungsfälle mit verteilter Identitätsverwaltung erhalten Sie von [Strata](mailto:sales@strata.io).

## <a name="next-steps"></a>Nächste Schritte

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
- [Was ist der bedingte Zugriff in Azure Active Directory?](../conditional-access/overview.md)
