---
title: 'Aufbau eines direkten Verbunds mit AD FS für B2B: Azure AD | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie AD FS als Identitätsanbieter für den direkten Verbund einrichten können, damit sich Gäste bei Ihren Azure AD-Apps anmelden können.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 07/01/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1b3d7c47ff0a2c533bf12a67958a913b22915f75
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87907537"
---
# <a name="example-direct-federation-with-active-directory-federation-services-ad-fs-preview"></a>Beispiel: Direkter Verbund mit Active Directory-Verbunddienste (AD FS) (Vorschauversion)

> [!NOTE]
> Ein direkter Verbund ist eine Public Previewfunktion von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Dieser Artikel beschreibt, wie Sie einen [direkten Verbund](direct-federation.md) mit Active Directory-Verbunddienste (AD FS) als SAML 2.0- oder WS-Fed-Identitätsanbieter einrichten. Damit der direkte Verbund unterstützt wird, müssen beim Identitätsanbieter bestimmte Attribute und Ansprüche konfiguriert werden. Wir verwenden als Beispiel Active Directory-Verbunddienste (AD FS), um zu veranschaulichen, wie ein Identitätsanbieter für den direkten Verbund konfiguriert wird. Wir zeigen, wie man AD FS sowohl als SAML-Identitätsanbieter als auch als WS-Verbund-Identitätsanbieter einrichtet.

> [!NOTE]
> Dieser Artikel beschreibt, wie Sie AD FS sowohl für SAML als auch für WS-Verbund zur Veranschaulichung einrichten. Für die Integration eines direkten Verbunds, bei dem der Identitätsanbieter AD FS ist, empfehlen wir die Verwendung von WS-Verbund als Protokoll. 

## <a name="configure-ad-fs-for-saml-20-direct-federation"></a>Konfigurieren von AD FS für den direkten SAML 2.0-Verbund
Azure AD B2B kann so konfiguriert werden, dass es einen Verbund mit Identitätsanbietern bildet, die das SAML-Protokoll mit den unten aufgeführten spezifischen Anforderungen verwenden. Für die Veranschaulichung der SAML-Konfiguration wird in diesem Abschnitt gezeigt, wie AD FS für SAML 2.0 eingerichtet wird. 

Die folgenden Attribute müssen in der SAML 2.0-Antwort vom Identitätsanbieter empfangen werden, um einen direkten Verbund einzurichten. Diese Attribute können durch Verlinkung mit der XML-Datei des Online-Sicherheitstokendiensts oder durch manuelle Eingabe konfiguriert werden. Schritt 12 unter [Erstellen einer AD FS-Testinstanz](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed) beschreibt, wie Sie die AD FS-Endpunkte finden oder wie Sie Ihre Metadaten-URL generieren, z. B. `https://fs.iga.azure-test.net/federationmetadata/2007-06/federationmetadata.xml`. 

|attribute  |Wert  |
|---------|---------|
|AssertionConsumerService     |`https://login.microsoftonline.com/login.srf`         |
|Zielgruppe     |`urn:federation:MicrosoftOnline`         |
|Issuer (Aussteller)     |Der Aussteller-URI des Partneridentitätsanbieters, z. B. `http://www.example.com/exk10l6w90DHM0yi...`         |

Die folgenden Ansprüche müssen in dem vom Identitätsanbieter ausgegebenen SAML 2.0-Token konfiguriert werden:


|attribute  |Wert  |
|---------|---------|
|NameID-Format     |`urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |


Im nächsten Abschnitt wird veranschaulicht, wie Sie die erforderlichen Attribute und Ansprüche unter Verwendung von AD FS als Beispiel für einen SAML 2.0-Identitätsanbieter konfigurieren.

### <a name="before-you-begin"></a>Voraussetzungen

Ein AD FS-Server muss bereits eingerichtet und funktionsfähig sein, bevor Sie mit diesem Vorgang beginnen. Hilfe bei der Einrichtung eines AD FS-Servers finden Sie unter [Erstellen einer AD FS 3.0-Testinstanz auf einem virtuellen Azure-Computer](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed).

### <a name="add-the-claim-description"></a>Hinzufügen der Anspruchsbeschreibung

1. Wählen Sie auf dem AD FS-Server **Tools** > **AD FS-Verwaltung** aus.
2. Klicken Sie im Navigationsbereich auf **Dienst** > **Anspruchsbeschreibungen**.
3. Klicken Sie unter **Aktionen** auf **Anspruchsbeschreibung hinzufügen**.
4. Geben Sie im Fenster **Anspruchsbeschreibung hinzufügen** die folgenden Werte an:

   - **Anzeigename**: Persistenter Bezeichner
   - **Anspruchsbezeichner**: `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent` 
   - Aktivieren Sie das Kontrollkästchen **Diese Anspruchsbeschreibung in Verbundmetadaten als Anspruchstyp veröffentlichen, den dieser Verbunddienst akzeptiert**.
   - Aktivieren Sie das Kontrollkästchen **Diese Anspruchsbeschreibung in Verbundmetadaten als Anspruchstyp veröffentlichen, den dieser Verbunddienst senden kann**.

5. Klicken Sie auf **OK**.

### <a name="add-the-relying-party-trust-and-claim-rules"></a>Hinzufügen der Vertrauensstellung der vertrauenden Seite und der Anspruchsregeln

1. Wechseln Sie auf dem AD FS-Server zu **Tools** > **AD FS-Verwaltung**.
2. Wählen Sie im Navigationsbereich **Vertrauensstellungen** > **Vertrauensstellungen der vertrauenden Seite** aus.
3. Wählen Sie unter **Aktionen** die Option **Vertrauensstellung der vertrauenden Seite hinzufügen** aus. 
4. Verwenden Sie im Assistenten zum Hinzufügen von Vertrauensstellungen der vertrauenden Seite unter **Datenquelle auswählen** die Option **Online oder in einem lokalen Netzwerk veröffentlichte Daten über die vertrauende Seite importieren**. Geben Sie diese Verbundmetadaten-URL an: https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml. Belassen Sie ansonsten die Standardeinstellungen. Klicken Sie auf **Schließen**.
5. Der Assistent zum **Bearbeiten von Anspruchsregeln** wird geöffnet.
6. Wählen Sie im Assistenten zum **Bearbeiten von Anspruchsregeln** die Option **Regel hinzufügen** aus. Wählen Sie unter **Regeltyp auswählen** die Option **LDAP-Attribute als Ansprüche senden** aus. Wählen Sie **Weiter** aus.
7. Geben Sie unter **Anspruchsregel konfigurieren** die folgenden Werte an: 

   - **Name der Anspruchsregel**: E-Mail-Anspruchsregel 
   - **Attributspeicher**: Active Directory 
   - **LDAP-Attribut**: E-Mail-Adressen 
   - **Typ des ausgehenden Anspruchs**: E-Mail-Adresse

8. Wählen Sie **Fertig stellen** aus.
9. Im Fenster **Anspruchsregeln bearbeiten** wird die neue Regel angezeigt. Klicken Sie auf **Anwenden**. 
10. Klicken Sie auf **OK**.  

### <a name="create-an-email-transform-rule"></a>Erstellen einer E-Mail-Transformationsregel
1. Wechseln Sie zu **Anspruchsregeln bearbeiten**, und klicken Sie auf **Regel hinzufügen**. Wählen Sie unter **Regeltyp auswählen** die Option **Eingehenden Anspruch transformieren** aus, und klicken Sie auf **Weiter**. 
2. Geben Sie unter **Anspruchsregel konfigurieren** die folgenden Werte an: 

   - **Name der Anspruchsregel**: E-Mail-Transformationsregel 
   - **Typ des eingehenden Anspruchs**: E-Mail-Adresse 
   - **Typ des ausgehenden Anspruchs**: Namens-ID 
   - **Format der ausgehenden Namens-ID**: Persistenter Bezeichner 
   - Wählen Sie **Durchlauf aller Anspruchswerte**.

3. Klicken Sie auf **Fertig stellen**. 
4. Im Fenster **Anspruchsregeln bearbeiten** werden die neuen Regeln angezeigt. Klicken Sie auf **Anwenden**. 
5. Klicken Sie auf **OK**. Der AD FS-Server ist jetzt für den direkten Verbund mit dem SAML 2.0-Protokoll konfiguriert.

## <a name="configure-ad-fs-for-ws-fed-direct-federation"></a>Konfigurieren von AD FS für den direkten WS-Verbund 
Azure AD B2B kann so konfiguriert werden, dass es einen Verbund mit Identitätsanbietern bildet, die das WS-Verbund-Protokoll mit den unten aufgeführten spezifischen Anforderungen verwenden. Derzeit sind die beiden WS-Verbund-Anbieter auf Kompatibilität mit Azure AD getestet, einschließlich AD FS und Shibboleth. Hier werden wir Active Directory-Verbunddienste (AD FS) als Beispiel für den WS-Verbund-Identitätsanbieter verwenden. Weitere Informationen zum Aufbau einer Vertrauensstellung der vertrauenden Seite zwischen einem mit WS-Verbund kompatiblen Anbieter mit Azure AD finden Sie in den Dokumenten zur Kompatibilität des Azure AD-Identitätsanbieters.

Zum Einrichten eines direkten Verbunds müssen die folgenden Attribute in der WS-Verbund-Nachricht vom Identitätsanbieter empfangen werden. Diese Attribute können durch Verlinkung mit der XML-Datei des Online-Sicherheitstokendiensts oder durch manuelle Eingabe konfiguriert werden. Schritt 12 unter [Erstellen einer AD FS-Testinstanz](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed) beschreibt, wie Sie die AD FS-Endpunkte finden oder wie Sie Ihre Metadaten-URL generieren, z. B. `https://fs.iga.azure-test.net/federationmetadata/2007-06/federationmetadata.xml`.
 
|attribute  |Wert  |
|---------|---------|
|PassiveRequestorEndpoint     |`https://login.microsoftonline.com/login.srf`         |
|Zielgruppe     |`urn:federation:MicrosoftOnline`         |
|Issuer (Aussteller)     |Der Aussteller-URI des Partneridentitätsanbieters, z. B. `http://www.example.com/exk10l6w90DHM0yi...`         |

Erforderliche Ansprüche für das vom Identitätsanbieter ausgegebene WS-Verbund-Token:

|attribute  |Wert  |
|---------|---------|
|ImmutableID     |`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

Im nächsten Abschnitt wird veranschaulicht, wie Sie die erforderlichen Attribute und Ansprüche unter Verwendung von AD FS als Beispiel für einen WS-Verbund-Identitätsanbieter konfigurieren können.

### <a name="before-you-begin"></a>Voraussetzungen
Ein AD FS-Server muss bereits eingerichtet und funktionsfähig sein, bevor Sie mit diesem Vorgang beginnen. Hilfe bei der Einrichtung eines AD FS-Servers finden Sie unter [Erstellen einer AD FS 3.0-Testinstanz auf einem virtuellen Azure-Computer](https://medium.com/in-the-weeds/create-a-test-active-directory-federation-services-3-0-instance-on-an-azure-virtual-machine-9071d978e8ed).


### <a name="add-the-relying-party-trust-and-claim-rules"></a>Hinzufügen der Vertrauensstellung der vertrauenden Seite und der Anspruchsregeln 
1. Wechseln Sie auf dem AD FS-Server zu **Tools** > **AD FS-Verwaltung**. 
1. Wählen Sie im Navigationsbereich **Vertrauensstellungen** > **Vertrauensstellungen der vertrauenden Seite** aus. 
1. Wählen Sie unter **Aktionen** die Option **Vertrauensstellung der vertrauenden Seite hinzufügen** aus.  
1. Verwenden Sie im Assistenten zum Hinzufügen von Vertrauensstellungen der vertrauenden Seite unter **Datenquelle auswählen** die Option **Online oder in einem lokalen Netzwerk veröffentlichte Daten über die vertrauende Seite importieren**. Geben Sie diese Verbundmetadaten-URL an: `https://nexus.microsoftonline-p.com/federationmetadata/2007-06/federationmetadata.xml`.  Belassen Sie ansonsten die Standardeinstellungen. Klicken Sie auf **Schließen**.
1. Der Assistent zum **Bearbeiten von Anspruchsregeln** wird geöffnet. 
1. Wählen Sie im Assistenten zum **Bearbeiten von Anspruchsregeln** die Option **Regel hinzufügen** aus. Wählen Sie unter **Regeltyp auswählen** die Option **Ansprüche mithilfe einer benutzerdefinierten Regel senden** aus. Wählen Sie *Weiter* aus. 
1. Geben Sie unter **Anspruchsregel konfigurieren** die folgenden Werte an:

   - **Name der Anspruchsregel**: Unveränderliche ID herausgeben  
   - **Benutzerdefinierte Regel**: `c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"] => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), query = "samAccountName={0};objectGUID;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);`

1. Wählen Sie **Fertig stellen** aus. 
1. Im Fenster **Anspruchsregeln bearbeiten** wird die neue Regel angezeigt. Klicken Sie auf **Anwenden**.  
1. Wählen Sie im gleichen Assistenten zum **Bearbeiten von Anspruchsregeln** die Option **Regel hinzufügen** aus. Wählen Sie unter **Regeltyp auswählen** die Option **LDAP-Attribute als Ansprüche senden** aus. Wählen Sie **Weiter** aus.
1. Geben Sie unter **Anspruchsregel konfigurieren** die folgenden Werte an: 

   - **Name der Anspruchsregel**: E-Mail-Anspruchsregel  
   - **Attributspeicher**: Active Directory  
   - **LDAP-Attribut**: E-Mail-Adressen  
   - **Typ des ausgehenden Anspruchs**: E-Mail-Adresse 

1.  Wählen Sie **Fertig stellen** aus. 
1.  Im Fenster **Anspruchsregeln bearbeiten** wird die neue Regel angezeigt. Klicken Sie auf **Anwenden**.  
1.  Klicken Sie auf **OK**. Der AD FS-Server ist nun für den direkten Verbund mit WS-Verbund konfiguriert.

## <a name="next-steps"></a>Nächste Schritte
Als Nächstes [konfigurieren Sie den direkten Verbund in Azure AD](direct-federation.md#step-2-configure-direct-federation-in-azure-ad) entweder im Azure AD-Portal oder mithilfe von PowerShell. 
