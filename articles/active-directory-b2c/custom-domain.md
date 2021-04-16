---
title: Aktivieren von benutzerdefinierten Domänen für Azure AD B2C
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie benutzerdefinierte Domänen in Umleitungs-URLs für Azure Active Directory B2C aktivieren.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/17/2021
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 2de419885938b27ebce4a934db5ef966965b3dbd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104580163"
---
# <a name="enable-custom-domains-for-azure-active-directory-b2c"></a>Aktivieren von benutzerdefinierten Domänen für Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

In diesem Artikel wird beschrieben, wie Sie benutzerdefinierte Domänen in Umleitungs-URLs für Azure Active Directory B2C (Azure AD B2C) aktivieren. Die Verwendung einer benutzerdefinierten Domäne für Ihre Anwendung ermöglicht eine einheitliche Benutzeroberfläche. Aus der Perspektive der Benutzer bleiben diese während des Anmeldevorgangs in Ihrer Domäne und werden nicht auf die Azure AD B2C-Standarddomäne *<Mandantenname>.b2clogin.com* umgeleitet.

![Benutzeroberfläche der benutzerdefinierten Domäne](./media/custom-domain/custom-domain-user-experience.png)

## <a name="custom-domain-overview"></a>Übersicht über benutzerdefinierte Domänen

Sie können benutzerdefinierte Domänen für Azure AD B2C mithilfe von [Azure Front Door](https://azure.microsoft.com/services/frontdoor/) aktivieren. Azure Front Door ist ein globaler und skalierbarer Einstiegspunkt, der es ermöglicht, über das globale Microsoft-Edge-Netzwerk schnelle, sichere und umfassend skalierbare Webanwendungen zu erstellen. Sie können Azure AD B2C-Inhalte hinter Azure Front Door rendern und dann eine Option in Azure Front Door konfigurieren, um die Inhalte über eine benutzerdefinierte Domäne in der URL Ihrer Anwendung bereitzustellen.

Die Integration in Azure Front Door wird in der Abbildung unten veranschaulicht:

1. Ein Benutzer klickt in einer Anwendung auf die Anmeldeschaltfläche, über die er auf die Azure AD B2C-Anmeldeseite gelangt. Auf dieser Seite ist ein benutzerdefinierter Domänenname angegeben.
1. Der benutzerdefinierte Domänenname wird im Webbrowser in die IP-Adresse von Azure Front Door aufgelöst. Bei der DNS-Auflösung zeigt ein kanonischer Namenseintrag (CNAME-Eintrag) mit einem benutzerdefinierten Domänennamen auf den standardmäßigen Front Door-Front-End-Host (z. B. `contoso.azurefd.net`). 
1. Der an die benutzerdefinierte Domäne (z. B. `login.contoso.com`) gerichtete Datenverkehr wird an den angegebenen standardmäßigen Front Door-Front-End-Host (`contoso.azurefd.net`) weitergeleitet.
1. In Azure Front Door werden die Azure AD B2C-Inhalte über die Azure AD B2C-Standarddomäne `<tenant-name>.b2clogin.com` aufgerufen. Die Anforderung an den Azure AD B2C-Endpunkt umfasst einen benutzerdefinierten HTTP-Header, der den ursprünglichen benutzerdefinierten Domänennamen enthält.
1. In Azure AD B2C wird durch Anzeigen der relevanten Inhalte und der ursprünglichen benutzerdefinierten Domäne auf die Anforderung geantwortet.

![Netzwerkdiagramm für benutzerdefinierte Domäne](./media/custom-domain/custom-domain-network-flow.png)

> [!IMPORTANT]
> Die Verbindung zwischen dem Browser und Azure Front Door sollte immer über IPv4 und nicht über IPv6 erfolgen.

Bei der Verwendung von benutzerdefinierten Domänen ist Folgendes zu beachten:

- Sie können mehrere benutzerdefinierte Domänen einrichten. Informationen zur maximalen Anzahl unterstützter benutzerdefinierter Domänen finden Sie unter [Dienst- und andere Einschränkungen](../active-directory/enterprise-users/directory-service-limits-restrictions.md) für Azure AD B2C und unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-front-door-service-limits) für Azure Front Door.
- Azure Front Door ist ein separater Azure-Dienst, sodass zusätzliche Gebühren anfallen. Weitere Informationen finden Sie unter [Azure Front Door – Preise](https://azure.microsoft.com/pricing/details/frontdoor).
- Um [Web Application Firewall](../web-application-firewall/afds/afds-overview.md) für Azure Front Door verwenden zu können, müssen Sie prüfen, ob die Firewallkonfiguration und -regeln ordnungsgemäß mit den Azure AD B2C-Benutzerflows ausgeführt werden.
- Nachdem Sie benutzerdefinierte Domänen konfiguriert haben, können Benutzer weiterhin auf den Azure AD B2C-Standarddomänennamen *<Mandantenname>.b2clogin.com* zugreifen (es sei denn, Sie verwenden eine benutzerdefinierte Richtlinie und [blockieren den Zugriff](#block-access-to-the-default-domain-name)).
- Wenn Sie mehrere Anwendungen verwenden, migrieren Sie alle in die benutzerdefinierte Domäne, da die Azure AD B2C-Sitzung im Browser unter dem aktuell verwendeten Domänennamen gespeichert wird.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]


## <a name="add-a-custom-domain-name-to-your-tenant"></a>Hinzufügen eines benutzerdefinierten Domänennamens zu Ihrem Mandanten

Befolgen Sie die Anweisungen zum [Hinzufügen und Überprüfen Ihrer benutzerdefinierten Domäne in Azure AD](../active-directory/fundamentals/add-custom-domain.md). Löschen Sie nach dem Überprüfen der Domäne den erstellten DNS-TXT-Eintrag.

> [!IMPORTANT]
> Melden Sie sich zum Ausführen dieser Schritte bei Ihrem **Azure AD B2C**-Mandanten an, und wählen Sie den Dienst **Azure Active Directory** aus.

Überprüfen Sie jede Unterdomäne, die Sie verwenden möchten. Die Überprüfung der Domäne der obersten Ebene allein genügt nicht. Für die Anmeldung bei *login.contoso.com* und *account.contoso.com* müssen Sie beispielsweise beide Unterdomänen und nicht nur die Domäne der obersten Ebene *contoso.com* überprüfen.  

## <a name="create-a-new-azure-front-door-instance"></a>Erstellen einer neuen Azure Front Door-Instanz

Führen Sie die Schritte zum [Erstellen einer Front Door-Instanz für Ihre Anwendung](../frontdoor/quickstart-create-front-door.md#create-a-front-door-for-your-application) aus. Verwenden Sie dabei die Standardeinstellungen für den Front-End-Host und die Routingregeln. 

> [!IMPORTANT]
> Wählen Sie für diese Schritte nach der Anmeldung im Azure-Portal in Schritt 1 die Option **Verzeichnis + Abonnement** und dann das Verzeichnis aus, das das Azure-Abonnement enthält, das für Azure Front Door verwendet werden soll. Dabei sollte es sich *nicht* um das Verzeichnis handeln, das Ihren Azure AD B2C-Mandanten enthält. 

Verwenden Sie im Schritt **Hinzufügen eines Back-Ends** die folgenden Einstellungen:

* Wählen Sie unter **Back-End-Hosttyp** die Option **Benutzerdefinierter Host** aus.  
* Wählen Sie unter **Zielhostname** den Hostnamen für den Azure AD B2C-Endpunkt aus: <Mandantenname>.b2clogin.com. Beispiel: contoso.b2clogin.com 
* Wählen Sie unter **Back-End-Hostheader** den gleichen Wert aus, den Sie für **Zielhostname** ausgewählt haben.

![Hinzufügen eines Back-Ends](./media/custom-domain/add-a-backend.png)

Nachdem Sie das **Back-End** dem **Back-End-Pool** hinzugefügt haben, deaktivieren Sie die **Integritätstests**.

![Hinzufügen eines Back-End-Pools](./media/custom-domain/add-a-backend-pool.png)

## <a name="set-up-your-custom-domain-on-azure-front-door"></a>Einrichten der benutzerdefinierten Domäne in Azure Front Door

Führen Sie die Schritte zum [Hinzufügen einer benutzerdefinierten Domäne für Ihre Front Door-Instanz](../frontdoor/front-door-custom-domain.md) aus. Verwenden Sie beim Erstellen des `CNAME`-Eintrags für die benutzerdefinierte Domäne den benutzerdefinierten Domänennamen, den Sie zuvor im Schritt [Hinzufügen eines benutzerdefinierten Domänennamens zu Ihrem Mandanten](#add-a-custom-domain-name-to-your-tenant) überprüft haben. 

Nachdem der benutzerdefinierte Domänenname überprüft wurde, wählen Sie **HTTPS für benutzerdefinierte Domänen** aus. Wählen Sie dann unter **Zertifikatverwaltungstyp** die Option [Mit Front Door verwaltet](../frontdoor/front-door-custom-domain-https.md#option-1-default-use-a-certificate-managed-by-front-door) oder [Eigenes Zertifikat verwenden](../frontdoor/front-door-custom-domain-https.md#option-2-use-your-own-certificate) aus. 

Im folgenden Screenshot wird gezeigt, wie eine benutzerdefinierte Domäne hinzugefügt und HTTPS über ein Azure Front Door-Zertifikat aktiviert wird.

![Einrichten einer benutzerdefinierten Azure Front Door-Domäne](./media/custom-domain/azure-front-door-add-custom-domain.png) 

## <a name="configure-cors"></a>Konfigurieren von CORS

Wenn Sie [die Azure AD B2C-Benutzeroberfläche](customize-ui-with-html.md) mit einer HTML-Vorlage anpassen, müssen Sie [CORS](customize-ui-with-html.md?pivots=b2c-user-flow.md#3-configure-cors) mit Ihrer benutzerdefinierten Domäne konfigurieren.

Führen Sie die folgenden Schritte aus, um Azure Blob Storage für die Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) zu konfigurieren:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Speicherkonto.
1. Wählen Sie im Menü **CORS** aus.
1. Geben Sie für **Zulässige Ursprünge** den Wert `https://your-domain-name` ein. Ersetzen Sie `your-domain-name` durch Ihren Domänennamen. Beispiel: `https://login.contoso.com`. Verwenden Sie bei der Eingabe Ihres Mandantennamens ausschließlich Kleinbuchstaben.
1. Wählen Sie für **Zulässige Methoden** sowohl `GET` als auch `OPTIONS` aus.
1. Geben Sie für **Zulässige Header** ein Sternchen (*) ein.
1. Geben Sie für **Verfügbar gemachte Header** ein Sternchen (*) ein.
1. Für **Max. Alter** geben Sie 200 ein.
1. Wählen Sie **Speichern** aus.

## <a name="configure-your-identity-provider"></a>Konfigurieren des Identitätsanbieters

Wenn sich ein Benutzer mit einem sozialen Netzwerk als Identitätsanbieter anmelden möchte, wird in Azure AD B2C eine Autorisierungsanforderung initiiert und der Benutzer zum Durchführen des Anmeldevorgangs an den ausgewählten Identitätsanbieter weitergeleitet. In der Autorisierungsanforderung wird `redirect_uri` mit dem Azure AD B2C-Standarddomänennamen angegeben: 

```http
https://<tenant-name>.b2clogin.com/<tenant-name>/oauth2/authresp
```

Wenn Sie die Richtlinie so konfiguriert haben, dass die Anmeldung mit einem externen Identitätsanbieter zulässig ist, aktualisieren Sie die OAuth-Umleitungs-URIs mit der benutzerdefinierten Domäne. Bei den meisten Identitätsanbietern können Sie mehrere Umleitungs-URIs registrieren. Es wird empfohlen, Umleitungs-URIs hinzuzufügen, anstatt sie zu ersetzen, damit Sie die benutzerdefinierte Richtlinie testen können, ohne dass sich dies auf Anwendungen auswirkt, in denen der Azure AD B2C-Standarddomänenname verwendet wird. 

Gehen Sie beim folgenden Umleitungs-URI wie folgt vor:

```http
https://<custom-domain-name>/<tenant-name>/oauth2/authresp
``` 

- Ersetzen Sie **<custom-domain-name>** durch Ihren benutzerdefinierten Domänennamen.
- Ersetzen Sie **<tenant-name>** durch den Namen oder die ID Ihres Mandanten.

Das folgende Beispiel zeigt einen gültigen OAuth-Umleitungs-URI:

```http
https://login.contoso.com/contoso.onmicrosoft.com/oauth2/authresp
```

Wenn Sie die [Mandanten-ID](#optional-use-tenant-id) verwenden, sieht ein gültiger OAuth-Umleitungs-URI wie folgt aus:

```http
https://login.contoso.com/11111111-1111-1111-1111-111111111111/oauth2/authresp
```

Die Metadaten der [SAML-Identitätsanbieter](saml-identity-provider-technical-profile.md) sehen wie folgt aus:

```http
https://<custom-domain-name>.b2clogin.com/<tenant-name>/<your-policy>/samlp/metadata?idptp=<your-technical-profile>
```

## <a name="test-your-custom-domain"></a>Testen Ihrer benutzerdefinierten Domäne

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie unter **Richtlinien** die Option **Benutzerflows (Richtlinien)** aus.
1. Wählen Sie einen Benutzerflow und dann **Benutzerflow ausführen** aus.
1. Wählen Sie für **Anwendung** die Webanwendung *webapp1* aus, die Sie zuvor registriert haben. Als **Antwort-URL** sollte `https://jwt.ms` angezeigt werden.
1. Klicken Sie auf **In Zwischenablage kopieren**.

    ![Kopieren des URI der Autorisierungsanforderung](./media/custom-domain/user-flow-run-now.png)

1. Ersetzen Sie in der URL unter **Benutzerflow-Endpunkt ausführen** die Azure AD B2C-Domäne (<tenant-name>.b2clogin.com) durch Ihre benutzerdefinierte Domäne.  
    Verwenden Sie anstelle von

    ```http
    https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/v2.0/authorize?p=B2C_1_susi&client_id=63ba0d17-c4ba-47fd-89e9-31b3c2734339&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login
    ```

    sondern:

    ```http
    https://login.contoso.com/contoso.onmicrosoft.com/oauth2/v2.0/authorize?p=B2C_1_susi&client_id=63ba0d17-c4ba-47fd-89e9-31b3c2734339&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login    
    ```
1. Wählen Sie **Benutzerflow ausführen** aus. Die Azure AD B2C-Richtlinie sollte geladen werden.
1. Sie können sich mit lokalen und Social Media-Konten anmelden.
1. Wiederholen Sie den Test mit den restlichen Richtlinien.

## <a name="configure-your-application"></a>Konfigurieren der Anwendung 

Nach dem Konfigurieren und Testen der benutzerdefinierten Domäne können Sie Ihre Anwendungen aktualisieren, um die URL zu laden, die anstelle der Azure AD B2C-Domäne die benutzerdefinierte Domäne als Hostname angibt. 

Die Integration der benutzerdefinierten Domäne gilt für Authentifizierungsendpunkte, bei denen zur Authentifizierung von Benutzern Azure AD B2C-Richtlinien (Benutzerflows oder benutzerdefinierte Richtlinien) verwendet werden. Diese Endpunkte können wie folgt aussehen:

- <code>https://\<custom-domain\>/\<tenant-name\>/<b>\<policy-name\></b>/v2.0/.well-known/openid-configuration</code>

- <code>https://\<custom-domain\>/\<tenant-name\>/<b>\<policy-name\></b>/oauth2/v2.0/authorize</code>

- <code>https://\<custom-domain\>/<tenant-name\>/<b>\<policy-name\></b>/oauth2/v2.0/token</code>

Ersetzen Sie:
- **custom-domain** durch die benutzerdefinierte Domäne
- **tenant-name** durch den Namen oder die ID Ihres Mandanten
- **policy-name** durch den Namen Ihrer Richtlinie. [Erfahren Sie mehr über Azure AD B2C-Richtlinien](technical-overview.md#identity-experiences-user-flows-or-custom-policies). 


Die Metadaten der [SAML-Dienstanbieter](./saml-service-provider.md) können wie folgt aussehen: 

```html
https://custom-domain-name/tenant-name/policy-name/Samlp/metadata
```

### <a name="optional-use-tenant-id"></a>(Optional:) Verwenden der Mandanten-ID

Sie können den Namen des B2C-Mandanten in der URL durch die GUID der Mandanten-ID ersetzen, um alle Verweise auf „b2c“ in der URL zu entfernen. Die GUID der Mandanten ID finden Sie auf der B2C-Übersichtsseite im Azure-Portal.
Ändern Sie zum Beispiel `https://account.contosobank.co.uk/contosobank.onmicrosoft.com/` in `https://account.contosobank.co.uk/<tenant ID GUID>/`.

Wenn Sie die Mandanten-ID anstelle des Mandantennamens verwenden, müssen Sie die **OAuth-Umleitungs-URIs** des Identitätsanbieters entsprechend aktualisieren. Weitere Informationen finden Sie unter [Konfigurieren des Identitätsanbieters](#configure-your-identity-provider).

### <a name="token-issuance"></a>Tokenausstellung

Der Anspruch für den Namen des Tokenausstellers (iss) wird basierend auf der verwendeten benutzerdefinierten Domäne geändert. Beispiel:

```http
https://<domain-name>/11111111-1111-1111-1111-111111111111/v2.0/
```

::: zone pivot="b2c-custom-policy"

## <a name="block-access-to-the-default-domain-name"></a>Blockieren des Zugriffs auf den Standarddomänennamen

Nachdem Sie die benutzerdefinierte Domäne hinzugefügt und die Anwendung konfiguriert haben, können Benutzer weiterhin auf die Domäne <tenant-name>.b2clogin.com zugreifen. Um den Zugriff zu verhindern, können Sie die Richtlinie so konfigurieren, dass der Hostname in der Autorisierungsanforderung anhand einer zulässigen Liste von Domänen überprüft wird. Der Hostname ist der Domänenname, der in der URL angezeigt wird. Der Hostname ist über `{Context:HostName}`-[Anspruchskonfliktlöser](claim-resolver-overview.md) verfügbar. Anschließend kann eine benutzerdefinierte Fehlermeldung angezeigt werden. 

1. Rufen Sie über [GitHub](https://github.com/azure-ad-b2c/samples/blob/master/policies/check-host-name) das Beispiel für eine Richtlinie für bedingten Zugriff ab, mit der der Hostname überprüft wird.
1. Ersetzen Sie in jeder Datei die Zeichenfolge `yourtenant` durch den Namen Ihres Azure AD B2C-Mandanten. Wenn der Name des B2C-Mandanten z.B. *contosob2c* lautet, werden alle Instanzen von `yourtenant.onmicrosoft.com` zu `contosob2c.onmicrosoft.com`.
1. Laden Sie die Richtliniendateien in der folgenden Reihenfolge hoch: `B2C_1A_TrustFrameworkExtensions_HostName.xml` und dann `B2C_1A_signup_signin_HostName.xml`.

::: zone-end

## <a name="troubleshooting"></a>Problembehandlung

### <a name="azure-ad-b2c-returns-a-page-not-found-error"></a>In Azure AD B2C wird die Fehlermeldung „Seite nicht gefunden“ zurückgegeben

- **Symptom:** Wenn Sie sich nach dem Konfigurieren einer benutzerdefinierten Domäne bei der Domäne anmelden möchten, wird eine HTTP 404-Fehlermeldung ausgegeben.
- **Mögliche Ursachen:** Dieses Problem kann mit der DNS-Konfiguration oder der Konfiguration des Azure Front Door-Back-Ends zusammenhängen. 
- **Lösung:**  
    1. Stellen Sie sicher, dass die benutzerdefinierte Domäne in Ihrem Azure AD B2C-Mandanten [registriert und erfolgreich überprüft](#add-a-custom-domain-name-to-your-tenant) wurde.
    1. Vergewissern Sie sich, dass die [benutzerdefinierte Domäne](../frontdoor/front-door-custom-domain.md) ordnungsgemäß konfiguriert ist. Der `CNAME`-Eintrag für die benutzerdefinierte Domäne muss auf den standardmäßigen Azure Front Door-Front-End-Host zeigen (z. B. contoso.azurefd.net).
    1. Vergewissern Sie sich, dass in der [Konfiguration des Azure Front Door-Back-End-Pools](#set-up-your-custom-domain-on-azure-front-door) auf den Mandanten verwiesen wird, in dem Sie den benutzerdefinierten Domänennamen eingerichtet haben und in dem die Benutzerflows bzw. benutzerdefinierten Richtlinien gespeichert werden.

### <a name="identify-provider-returns-an-error"></a>Identitätsanbieter gibt einen Fehler zurück

- **Symptom:** Nach dem Konfigurieren einer benutzerdefinierten Domäne können Sie sich mit lokalen Konten anmelden. Wenn Sie sich jedoch mit den Anmeldeinformationen externer [Identitätsanbieter für soziale Netzwerke oder Unternehmen](add-identity-provider.md) anmelden, gibt der Identitätsanbieter eine Fehlermeldung aus.
- **Mögliche Ursachen:** Wenn der Benutzer in Azure AD B2C zur Anmeldung bei einem Verbundidentitätsanbieter weitergeleitet wird, wird der Umleitungs-URI angegeben. Der Umleitungs-URI ist der Endpunkt, an dem der Identitätsanbieter das Token zurückgibt. Der Umleitungs-URI ist die gleiche Domäne, die in Ihrer Anwendung für die Autorisierungsanforderung verwendet wird. Wenn der Umleitungs-URI noch nicht beim Identitätsanbieter registriert ist, wird der neue Umleitungs-URI möglicherweise nicht als vertrauenswürdig eingestuft, sodass eine Fehlermeldung ausgegeben wird. 
- **Lösung:** Führen Sie die Schritte unter [Konfigurieren des Identitätsanbieters](#configure-your-identity-provider) aus, um den neuen Umleitungs-URI hinzuzufügen. 


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="can-i-use-azure-front-door-advanced-configuration-such-as-web-application-firewall-rules"></a>Kann ich die erweiterte Konfiguration in Azure Front Door verwenden, z. B. *Web Application Firewall-Regeln*? 
  
Obwohl die erweiterten Konfigurationseinstellungen für Azure Front Door offiziell nicht unterstützt werden, können Sie sie auf eigene Gefahr verwenden. 

### <a name="when-i-use-run-now-to-try-to-run-my-policy-why-i-cant-see-the-custom-domain"></a>Warum wird die benutzerdefinierte Domäne nicht angezeigt, wenn ich die Richtlinie über „Jetzt ausführen“ ausführen möchte?

Kopieren Sie die URL, ändern Sie den Domänennamen manuell, und fügen Sie sie dann wieder im Browser ein.

### <a name="which-ip-address-is-presented-to-azure-ad-b2c-the-users-ip-address-or-the-azure-front-door-ip-address"></a>Welche IP-Adresse wird an Azure AD B2C übermittelt? Die IP-Adresse des Benutzers oder die IP-Adresse von Azure Front Door?

Mit Azure Front Door wird die ursprüngliche IP-Adresse des Benutzers übergeben. Dabei handelt es sich um die IP-Adresse, die in Überwachungsberichten oder in der benutzerdefinierten Richtlinie angezeigt wird.

### <a name="can-i-use-a-third-party-web-application-firewall-waf-with-b2c"></a>Kann eine Web Application Firewall (WAF) eines Drittanbieters mit B2C verwendet werden?

Zur Verwendung einer eigenen Web Application Firewall vor Azure Front Door müssen Sie die entsprechenden Vorgänge konfigurieren und überprüfen, ob sie ordnungsgemäß mit den Azure AD B2C-Benutzerflows durchgeführt werden.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu [OAuth-Autorisierungsanforderungen](protocols-overview.md)