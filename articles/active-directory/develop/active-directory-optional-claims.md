---
title: Bereitstellen optionaler Ansprüche für Azure AD-Apps | Azure
titleSuffix: Microsoft identity platform
description: Hinzufügen benutzerdefinierter oder zusätzlicher Ansprüche zu den SAML 2.0- und JWT-Token (JSON Web Token), die von Azure Active Directory ausgestellt werden.
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 12/08/2019
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin, keyam
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: bcaf347eb91f8777b56bb2ea4d26985b2d75f645
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74967212"
---
# <a name="how-to-provide-optional-claims-to-your-azure-ad-app"></a>Gewusst wie: Bereitstellen optionaler Ansprüche für Ihre Azure AD-App

Anwendungsentwickler können optionale Ansprüche in ihren Azure AD-Anwendungen verwenden, um anzugeben, welche Ansprüche in Token vorhanden sein sollen, die an ihre Anwendungen gesendet werden. 

Sie können optionale Ansprüche zu folgenden Zwecken verwenden:

- Auswählen zusätzlicher Ansprüche, die in Token für Ihre Anwendung aufgenommen werden sollen
- Ändern des Verhaltens bestimmter Ansprüche, die von Azure AD in Token zurückgegeben werden
- Hinzufügen und Zugreifen auf benutzerdefinierte Ansprüche für Ihre Anwendung

Die Listen der Standardansprüche finden Sie in der Anspruchsdokumentation unter [Zugriffstoken](access-tokens.md) und [id_token](id-tokens.md). 

Zwar werden optionale Ansprüche von Token der Formate v1.0 und v2.0 sowie von SAML-Token unterstützt, sie besitzen jedoch den größten Wert bei der Umstellung von v1.0 auf v2.0. Eines der Ziele des [v2.0 Microsoft Identity Platform-Endpunkts](active-directory-appmodel-v2-overview.md) ist eine geringere Tokengröße, um die optimale Leistung von Clients zu gewährleisten. Daher sind mehrere Ansprüche, die zuvor in den Zugriffs- und ID-Token enthalten waren, nicht mehr in v2.0-Token vorhanden und müssen für einzelne Anwendungen speziell angefordert werden.

**Tabelle 1: Anwendbarkeit**

| Kontotyp | v1.0-Token | v2.0-Token  |
|--------------|---------------|----------------|
| Persönliches Microsoft-Konto  | –  | Unterstützt |
| Azure AD-Konto      | Unterstützt | Unterstützt |

## <a name="v10-and-v20-optional-claims-set"></a>Gruppe optionaler Ansprüche in v1.0 und v2.0

Die Gruppe optionaler Ansprüche, die standardmäßig zur Verwendung in Anwendungen bereitstehen, sind nachfolgend aufgeführt. Informationen zum Hinzufügen benutzerdefinierter optionaler Ansprüche für Ihre Anwendung finden Sie unter [Verzeichniserweiterungen](#configuring-directory-extension-optional-claims) weiter unten. Beim Hinzufügen von Ansprüchen zum **Zugriffstoken** gilt dies für Zugriffstoken, die *für* die Anwendung (eine Web-API) und nicht *von* der Anwendung angefordert werden. Dadurch wird sichergestellt, dass unabhängig von dem Client, der auf Ihre API zugreift, die richtigen Daten im Zugriffstoken vorhanden sind, die zum Authentifizieren bei Ihrer API verwendet werden.

> [!NOTE]
> Die Mehrzahl dieser Ansprüche kann in JWTs für v1.0- und v2.0-Token, jedoch nicht in SAML-Token eingeschlossen werden. Ausnahmen stellen die Ansprüche dar, die einen entsprechenden Hinweis in der Spalte „Tokentyp“ enthalten. Consumerkonten unterstützen eine Teilmenge dieser Ansprüche, die in der Spalte als „Benutzertyp“ gekennzeichnet sind.  Viele der aufgelisteten Ansprüche gelten nicht für Consumerbenutzer (sie besitzen keinen Mandanten, daher weist `tenant_ctry` keinen Wert auf).  

**Tabelle 2: Gruppe optionaler Ansprüche in v1.0 und v2.0**

| NAME                       |  BESCHREIBUNG   | Tokentyp | Benutzertyp | Notizen  |
|----------------------------|----------------|------------|-----------|--------|
| `auth_time`                | Zeitpunkt der letzten Authentifizierung des Benutzers. Siehe OpenID Connect-Spezifikation.| JWT        |           |  |
| `tenant_region_scope`      | Region des Ressourcenmandanten | JWT        |           | |
| `home_oid`                 | Bei Gastbenutzern: Die Objekt-ID des Benutzers im Home-Mandanten des Benutzers.| JWT        |           | |
| `sid`                      | Sitzungs-ID, die zur sitzungsbezogenen Abmeldung des Benutzers verwendet wird. | JWT        |  Persönliche Konten und Azure AD-Konten   |         |
| `platf`                    | Geräteplattform    | JWT        |           | Beschränkt auf verwaltete Geräte, die den Gerätetyp überprüfen können.|
| `verified_primary_email`   | Stammt von „PrimaryAuthoritativeEmail“ des Benutzers.      | JWT        |           |         |
| `verified_secondary_email` | Stammt von „SecondaryAuthoritativeEmail“ des Benutzers.   | JWT        |           |        |
| `enfpolids`                | IDs erzwungener Richtlinien. Eine Liste der Richtlinien-IDs, die für den aktuellen Benutzer ausgewertet wurden. | JWT |  |  |
| `vnet`                     | Informationen zum VNET-Spezifizierer | JWT        |           |      |
| `fwd`                      | IP-Adresse.| JWT    |   | Fügt die ursprüngliche IPv4-Adresse des anfordernden Clients hinzu (wenn innerhalb eines VNET). |
| `ctry`                     | Land des Benutzers | JWT |  | Azure AD gibt den optionalen Anspruch `ctry` zurück, wenn er vorhanden ist und der Wert des Anspruchs aus einem standardmäßigen Ländercode mit zwei Buchstaben besteht, z.B. FR, JP oder SZ. |
| `tenant_ctry`              | Land des Ressourcenmandanten | JWT | | |
| `xms_pdl`          | Bevorzugter Datenspeicherort   | JWT | | Für Multi-Geo-Mandanten ist dies ein aus drei Buchstaben bestehender Code, der die geografische Region anzeigt, in der sich der Benutzer befindet. Weitere Informationen finden Sie unter [Azure Active Directory Connect-Synchronisierung: Konfigurieren des bevorzugten Datenspeicherorts für Office 365-Ressourcen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-preferreddatalocation).<br/>Zum Beispiel: `APC` für Asien-Pazifik. |
| `xms_pl`                   | Bevorzugte Benutzersprache  | JWT ||Die bevorzugte Sprache des Benutzers, falls festgelegt. Wird in Szenarios mit Gastzugriff aus dem Basismandanten abgerufen. Format: Sprachraum-Land (z.B.: en-us) |
| `xms_tpl`                  | Bevorzugte Mandantensprache| JWT | | Die bevorzugte Sprache des Ressourcenmandanten, falls festgelegt. Format: Sprachraum (z.B.: en) |
| `ztdid`                    | ID der Bereitstellung ohne manuelles Eingreifen | JWT | | Die für [Windows Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) verwendete Geräteidentität |
| `email`                    | Die adressierbaren E-Mail-Adresse dieses Benutzers, wenn der Benutzer über eine verfügt.  | JWT, SAML | MSA, Azure AD | Dieser Wert ist standardmäßig enthalten, wenn der Benutzer ein Gast im Mandanten ist.  Für verwaltete Benutzer (Benutzer innerhalb des Mandanten) muss er über diese optionale Anforderung oder – nur in V2. 0 – mit dem OpenID-Bereich angefordert werden.  Für verwaltete Benutzer muss die E-Mail-Adresse im [Office-Verwaltungsportal](https://portal.office.com/adminportal/home#/users) festgelegt sein.| 
| `groups`| Optionale Formatierung für Gruppenansprüche |JWT, SAML| |Wird in Verbindung mit der Einstellung „GroupMembershipClaims“ im [Anwendungsmanifest](reference-app-manifest.md) verwendet, das ebenfalls festgelegt sein muss. Weitere Informationen finden Sie weiter unten unter [Gruppenansprüche](#configuring-groups-optional-claims). Weitere Informationen zu Gruppenansprüchen finden Sie unter [Konfigurieren von Gruppenansprüchen](../hybrid/how-to-connect-fed-group-claims.md).
| `acct`             | Benutzerkontostatus im Mandanten. | JWT, SAML | | Wenn der Benutzer dem Mandanten angehört, lautet der Wert `0`. Bei einem Gastbenutzer lautet der Wert `1`. |
| `upn`                      | Anspruch „UserPrincipalName“. | JWT, SAML  |           | Obwohl dieser Anspruch automatisch hinzugefügt wird, können Sie ihn als einen optionalen Anspruch angeben, um zusätzliche Eigenschaften zum Ändern des Verhaltens im Fall eines Gastbenutzer anzufügen.  |

## <a name="v20-specific-optional-claims-set"></a>v2.0-spezifischer optionaler Anspruchssatz

Diese Ansprüche sind in Azure AD v1.0-Token immer enthalten, jedoch nie in v2.0-Token, sofern nicht angefordert. Diese Ansprüche gelten nur für JWTs (ID-Token und Zugriffstoken). 

**Tabelle 3: Nur in v2.0 enthaltene optionale Ansprüche**

| JWT-Anspruch     | NAME                            | BESCHREIBUNG                                | Notizen |
|---------------|---------------------------------|-------------|-------|
| `ipaddr`      | IP-Adresse                      | Die IP-Adresse, von der aus sich der Client angemeldet hat.   |       |
| `onprem_sid`  | Lokale Sicherheits-ID |                                             |       |
| `pwd_exp`     | Kennwortablaufzeit        | Datum und Uhrzeit, zu der das Kennwort abläuft. |       |
| `pwd_url`     | Kennwortänderungs-URL             | Eine URL, die der Benutzer besuchen kann, um sein Kennwort zu ändern.   |   |
| `in_corp`     | Innerhalb des Unternehmensnetzwerks        | Signalisiert, ob sich der Client aus dem Unternehmensnetzwerk anmeldet. Andernfalls ist der Anspruch nicht enthalten.   |  Basierend auf den Einstellungen für [vertrauenswürdige IP-Adressen](../authentication/howto-mfa-mfasettings.md#trusted-ips) in der MFA.    |
| `nickname`    | Spitzname                        | Ein zusätzlicher Name für den Benutzer, der vom Vor- oder Nachnamen abweicht. | 
| `family_name` | Last Name (Nachname)                       | Gibt den Nachnamen des Benutzers entsprechend der Definition im Benutzerobjekt an. <br>„family_name“: „Miller“ | In MSA und Azure AD unterstützt   |
| `given_name`  | Vorname                      | Gibt den Vornamen des Benutzers entsprechend der Definition im Benutzerobjekt an.<br>"given_name": "Frank"                   | In MSA und Azure AD unterstützt  |
| `upn`         | Benutzerprinzipalname | Ein Bezeichner für den Benutzer, der mit dem Parameter „username_hint“ verwendet werden kann.  Dies ist kein dauerhafter Bezeichner für den Benutzer und sollte möglichst nicht zur Datenzuordnung verwendet werden. | Informationen zur Konfiguration des Anspruchs finden Sie weiter unten unter [Zusätzliche Eigenschaften](#additional-properties-of-optional-claims). |

### <a name="additional-properties-of-optional-claims"></a>Zusätzliche Eigenschaften optionaler Ansprüche

Einige optionale Ansprüche können so konfiguriert werden, dass sie auf andere Art zurückgegeben werden. Diese zusätzlichen Eigenschaften unterstützen in erster Linie die Migration lokaler Anwendungen mit unterschiedlichen Datenerwartungen. So ist `include_externally_authenticated_upn_without_hash` beispielsweise hilfreich bei Clients, die keine Hashzeichen (`#`) im UPN verarbeiten können.

**Tabelle 4: Werte zum Konfigurieren optionaler Ansprüche**

| Eigenschaftenname  | Name der zusätzlichen Eigenschaft | BESCHREIBUNG |
|----------------|--------------------------|-------------|
| `upn`          |                          | Kann für SAML- und JWT-Antworten und für v1.0- und v2.0-Token verwendet werden. |
|                | `include_externally_authenticated_upn`  | Bezieht den Gast-UPN ein, wie er im Ressourcenmandanten gespeichert ist. Zum Beispiel, `foo_hometenant.com#EXT#@resourcetenant.com` |             
|                | `include_externally_authenticated_upn_without_hash` | Wie oben, außer dass die Hashzeichen (`#`) durch Unterstriche (`_`) ersetzt werden, z.B. `foo_hometenant.com_EXT_@resourcetenant.com` |

#### <a name="additional-properties-example"></a>Beispiel für zusätzliche Eigenschaften

    ```json
        "optionalClaims": 
         {
             "idToken": [ 
            { 
                      "name": "upn", 
                      "essential": false,
                  "additionalProperties": [ "include_externally_authenticated_upn"]  
                    }
                 ]
        }
    ```

Dieses OptionalClaims-Objekt bewirkt, dass das an den Client zurückgegebene ID-Token einen weiteren UPN mit den zusätzlichen Informationen zum Home- und zum Ressourcenmandanten enthält. Dies ändert den `upn`-Anspruch im Token nur dann, wenn der Benutzer ein Gastbenutzer im Mandanten ist (und einen anderen Identitätsanbieter für die Authentifizierung verwendet). 

## <a name="configuring-optional-claims"></a>Konfigurieren optionaler Ansprüche

> [!IMPORTANT]
> Zugriffstoken werden **immer** mit dem Manifest der Ressource und nicht des Clients generiert.  Daher entspricht die Ressource in der Anforderung `...scope=https://graph.microsoft.com/user.read...` dem Graph.  Folglich wird das Zugriffstoken unter Verwendung des Graph-Manifests und nicht des Clientmanifests erstellt.  Wenn Sie das Manifest für Ihre Anwendung ändern, hat das daher keinen Einfluss auf die Token für den Graph.  Um zu überprüfen, ob Ihre Änderungen an `accessToken` wirksam sind, fordern Sie ein Token für Ihre Anwendung, aber keine andere App an.  

Sie können optionale Ansprüche für Ihre Anwendung über die Benutzeroberfläche oder das Anwendungsmanifest konfigurieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie nach der Authentifizierung Ihren Azure AD-Mandanten aus, indem Sie ihn in der oberen rechten Ecke der Seite auswählen.
1. Klicken Sie im Menü auf der linken Seite auf **Azure Active Directory**.
1. Wählen Sie im Abschnitt **Verwalten** die Option **App-Registrierungen** aus.
1. Wählen Sie in der Liste die Anwendung aus, für die Sie optionale Ansprüche konfigurieren möchten.

**Konfigurieren optionaler Ansprüche über die Benutzeroberfläche:**

[![Zeigt, wie optionale Ansprüche mithilfe der Benutzeroberfläche konfiguriert werden.](./media/active-directory-optional-claims/token-configuration.png)](./media/active-directory-optional-claims/token-configuration.png)

1. Wählen Sie im Abschnitt **Verwalten** die Option **Tokenkonfiguration (Vorschau)** aus.
2. Wählen Sie **Optionalen Anspruch hinzufügen** aus.
3. Wählen Sie den Tokentyp aus, den Sie konfigurieren möchten.
4. Wählen Sie die hinzuzufügenden optionalen Ansprüche aus.
5. Klicken Sie auf **Hinzufügen**.

**Konfigurieren optionaler Ansprüche über das Anwendungsmanifest:**

[![Zeigt, wie optionale Ansprüche mithilfe des Anwendungsmanifests konfiguriert werden.](./media/active-directory-optional-claims/app-manifest.png)](./media/active-directory-optional-claims/app-manifest.png)

1. Wählen Sie im Abschnitt **Verwalten** die Option **Manifest** aus. Ein webbasierter Manifest-Editor wird geöffnet, mit dem Sie das Manifest bearbeiten können. Optional können Sie **Herunterladen** wählen und das Manifest lokal bearbeiten und dann **Hochladen** verwenden, um es wieder auf Ihre Anwendung anzuwenden. Weitere Informationen zum Anwendungsmanifest finden Sie im Artikel [Grundlegendes zum Azure AD-Anwendungsmanifest](reference-app-manifest.md).

    Der folgende Eintrag des Anwendungsmanifests fügt ID-, Zugriffs- und SAML-Token die optionalen Ansprüche „auth_time“, „ipaddr“ und „upn“ hinzu.

    ```json
        "optionalClaims":  
           {
              "idToken": [
                    {
                          "name": "auth_time", 
                          "essential": false
                     }
              ],
              "accessToken": [
                     {
                            "name": "ipaddr", 
                            "essential": false
                      }
              ],
              "saml2Token": [
                      {
                            "name": "upn", 
                            "essential": false
                       },
                       {
                            "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                            "source": "user", 
                            "essential": false
                       }
               ]
           }
       ```

2. When finished, click **Save**. Now the specified optional claims will be included in the tokens for your application.    


### OptionalClaims type

Declares the optional claims requested by an application. An application can configure optional claims to be returned in each of three types of tokens (ID token, access token, SAML 2 token) it can receive from the security token service. The application can configure a different set of optional claims to be returned in each token type. The OptionalClaims property of the Application entity is an OptionalClaims object.

**Table 5: OptionalClaims type properties**

| Name        | Type                       | Description                                           |
|-------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Collection (OptionalClaim) | The optional claims returned in the JWT ID token. |
| `accessToken` | Collection (OptionalClaim) | The optional claims returned in the JWT access token. |
| `saml2Token`  | Collection (OptionalClaim) | The optional claims returned in the SAML token.   |

### OptionalClaim type

Contains an optional claim associated with an application or a service principal. The idToken, accessToken, and saml2Token properties of the [OptionalClaims](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) type is a collection of OptionalClaim.
If supported by a specific claim, you can also modify the behavior of the OptionalClaim using the AdditionalProperties field.

**Table 6: OptionalClaim type properties**

| Name                 | Type                    | Description                                                                                                                                                                                                                                                                                                   |
|----------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | The name of the optional claim.                                                                                                                                                                                                                                                                           |
| `source`               | Edm.String              | The source (directory object) of the claim. There are predefined claims and user-defined claims from extension properties. If the source value is null, the claim is a predefined optional claim. If the source value is user, the value in the name property is the extension property from the user object. |
| `essential`            | Edm.Boolean             | If the value is true, the claim specified by the client is necessary to ensure a smooth authorization experience for the specific task requested by the end user. The default value is false.                                                                                                             |
| `additionalProperties` | Collection (Edm.String) | Additional properties of the claim. If a property exists in this collection, it modifies the behavior of the optional claim specified in the name property.                                                                                                                                               |
## Configuring directory extension optional claims

In addition to the standard optional claims set, you can also configure tokens to include directory schema extensions. For more info, see [Directory schema extensions](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions). This feature is useful for attaching additional user information that your app can use – for example, an additional identifier or important configuration option that the user has set. See the bottom of this page for an example.

> [!NOTE]
> - Directory schema extensions are an Azure AD-only feature, so if your application manifest requests a custom extension and an MSA user logs into your app, these extensions will not be returned.
> - Azure AD optional claims only work with Azure AD Graph extensions and do not work with Microsoft Graph directory extensions. Both APIs require the `Directory.ReadWriteAll` permission, which can only be consented by admins.

### Directory extension formatting

When configuring directory extension optional claims using the application manifest, use the full name of the extension (in the format: `extension_<appid>_<attributename>`). The `<appid>` must match the ID of the application requesting the claim. 

Within the JWT, these claims will be emitted with the following name format:  `extn.<attributename>`.

Within the SAML tokens, these claims will be emitted with the following URI format: `http://schemas.microsoft.com/identity/claims/extn.<attributename>`

## Configuring groups optional claims

   > [!NOTE]
   > The ability to emit group names for users and groups synced from on-premises is Public Preview.

This section covers the configuration options under optional claims for changing the group attributes used in group claims from the default group objectID to attributes synced from on-premises Windows Active Directory. You can configure groups optional claims for your application through the UI or application manifest.

> [!IMPORTANT]
> See [Configure group claims for applications with Azure AD](../hybrid/how-to-connect-fed-group-claims.md) for more details including important caveats for the public preview of group claims from on-premises attributes.

**Configuring groups optional claims through the UI:**
1. Sign in to the [Azure portal](https://portal.azure.com)
1. After you've authenticated, choose your Azure AD tenant by selecting it from the top right corner of the page
1. Select **Azure Active Directory** from the left hand menu
1. Under the **Manage** section, select **App registrations**
1. Select the application you want to configure optional claims for in the list
1. Under the **Manage** section, select **Token configuration (preview)**
2. Select **Add groups claim**
3. Select the group types to return (**All Groups**, **SecurityGroup** or **DirectoryRole**). The **All Groups** option includes **SecurityGroup**, **DirectoryRole** and **DistributionList**
4. Optional: click on the specific token type properties to modify the groups claim value to contain on premises group attributes or to change the claim type to a role
5. Click **Save**

**Configuring groups optional claims through the application manifest:**
1. Sign in to the [Azure portal](https://portal.azure.com)
1. After you've authenticated, choose your Azure AD tenant by selecting it from the top right corner of the page
1. Select **Azure Active Directory** from the left hand menu
1. Select the application you want to configure optional claims for in the list
1. Under the **Manage** section, select **Manifest**
3. Add the following entry using the manifest editor:

   The valid values are:

   - "All" (this option includes SecurityGroup, DirectoryRole and DistributionList)
   - "SecurityGroup"
   - "DirectoryRole"

   For example:

    ```json
        "groupMembershipClaims": "SecurityGroup"
    ```

   Standardmäßig werden ObjectIDs von Gruppen im Wert des Gruppenanspruchs ausgegeben.  Um den Anspruchswert so zu ändern, dass er lokale Gruppenattribute enthält, oder um den Anspruchstyp zu „role“ zu ändern, verwenden Sie die Konfiguration für optionale Ansprüche wie folgt:

3. Legen Sie optionale Ansprüche für die Gruppennamenkonfiguration fest.

   Wenn die Gruppen im Token die lokalen AD-Gruppenattribute im Abschnitt mit optionalen Ansprüchen enthalten sollen, geben Sie an, welcher optionale Anspruch des Tokentyps angewendet werden soll. Geben Sie außerdem den Namen des angeforderten optionalen Anspruchs sowie weitere gewünschte Eigenschaften an.  Es können mehrere Tokentypen aufgelistet werden:

   - idToken für das OIDC-ID-Token
   - accessToken für das OAuth/OIDC-Zugriffstoken
   - Saml2Token für SAML-Token

   > [!NOTE]
   > Der Typ „Saml2Token“ gilt für Token sowohl im SAML1.1- als auch im SAML2.0-Format.

   Ändern Sie für jeden relevanten Tokentyp den Gruppenanspruch so, dass der Abschnitt „Optionale Ansprüche“ im Manifest verwendet wird. Das Schema für optionale Ansprüche sieht folgendermaßen aus:

    ```json
       {
       "name": "groups",
       "source": null,
       "essential": false,
       "additionalProperties": []
       }
    ```

   | Schema für optionale Ansprüche | Wert |
   |----------|-------------|
   | **name:** | Muss „groups“ lauten. |
   | **source:** | Wird nicht verwendet. Auslassen oder NULL angeben. |
   | **essential:** | Wird nicht verwendet. Auslassen oder FALSE angeben. |
   | **additionalProperties:** | Liste zusätzlicher Eigenschaften.  Gültige Optionen sind „sam_account_name“, „dns_domain_and_sam_account_name“, „netbios_domain_and_sam_account_name“, „emit_as_roles“. |

   In „additionalProperties“ ist nur eine dieser Optionen erforderlich: „sam_account_name“, „dns_domain_and_sam_account_name“, „netbios_domain_and_sam_account_name“.  Wenn mehrere dieser Optionen vorhanden sind, wird die erste verwendet, alle weiteren werden ignoriert.

   Einige Anwendungen erfordern Gruppeninformationen über Benutzer im Rollenanspruch.  Um den Anspruchstyp von einem Gruppenanspruch zu einem Rollenanspruch zu ändern, fügen Sie „emit_as_roles“ zu den zusätzlichen Eigenschaften hinzu.  Die Gruppenwerte werden im Rollenanspruch ausgegeben.

   > [!NOTE]
   > Wenn „emit_as_roles“ verwendet wird, sind konfigurierte Anwendungsrollen, denen der Benutzer zugewiesen ist, nicht im Rollenanspruch enthalten.

**Beispiele:**

1) Ausgeben von Gruppen als Gruppennamen in OAuth-Zugriffstoken im Format „dnsDomainName\sAMAccountName“

    
    **Benutzeroberflächenkonfiguration:**

    [![Zeigt, wie optionale Ansprüche mithilfe der Benutzeroberfläche konfiguriert werden.](./media/active-directory-optional-claims/groups-example-1.png)](./media/active-directory-optional-claims/groups-example-1.png)


    **Eintrag des Anwendungsmanifests:**
    ```json
        "optionalClaims": {
            "accessToken": [{
            "name": "groups",
            "additionalProperties": ["dns_domain_and_sam_account_name"]
            }]
        }
    ```

 
    
2) Ausgeben von Gruppennamen, die im Format „netbiosDomain\sAMAccountName“ als Rollenanspruch in SAML- und OIDC-ID-Token zurückgegeben werden sollen

    **Benutzeroberflächenkonfiguration:**

    [![Zeigt, wie optionale Ansprüche mithilfe der Benutzeroberfläche konfiguriert werden.](./media/active-directory-optional-claims/groups-example-2.png)](./media/active-directory-optional-claims/groups-example-2.png)

    **Eintrag des Anwendungsmanifests:**
    
    ```json
        "optionalClaims": {
        "saml2Token": [{
            "name": "groups",
            "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
        }],
        "idToken": [{
            "name": "groups",
            "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
        }]
    ``` 
     

## <a name="optional-claims-example"></a>Beispiel für optionale Ansprüche

In diesem Abschnitt können Sie ein Szenario durchgehen, um zu erfahren, wie Sie optionale Ansprüche für Ihre Anwendung verwenden können.
Es sind mehrere Optionen für das Aktualisieren der Eigenschaften in der Identitätskonfiguration einer Anwendung zum Aktivieren und Konfigurieren optionaler Ansprüche verfügbar:
-    Sie können die Benutzeroberfläche **Tokenkonfiguration (Vorschau)** verwenden (siehe Beispiel unten).
-    Sie können das **Manifest** verwenden (siehe Beispiel unten). Lesen Sie zuerst das Dokument [Grundlegendes zum Azure AD-Anwendungsmanifest](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest), um eine Einführung in das Manifest zu erhalten.
-   Sie können zum Aktualisieren Ihrer Anwendung auch eine Anwendung schreiben, die die [Graph-API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api) nutzt. Die [Referenz für Entitäten und komplexe Typen](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#optionalclaims-type) im Graph-API-Referenzhandbuch kann beim Konfigurieren der optionalen Ansprüche hilfreich sein.

**Beispiel:** Im folgenden Beispiel verwenden Sie die Benutzeroberfläche **Tokenkonfiguration (Vorschau)** und das **Manifest**, um den Zugriffs-, ID-und SAML-Token optionale Ansprüche hinzuzufügen, die für Ihre Anwendung vorgesehen sind. Verschiedene optionale Ansprüche wurden den einzelnen Tokentypen hinzugefügt, die von der Anwendung empfangen werden können:
-    Die ID-Token enthalten jetzt den UPN für Verbundbenutzer in vollständiger Form (`<upn>_<homedomain>#EXT#@<resourcedomain>`).
-    Die Zugriffstoken, die andere Clients für diese Anwendung anfordern, enthalten jetzt den Anspruch „auth_time“.
-    Die SAML-Token enthalten jetzt die Verzeichnisschemaerweiterung „skypeId“ (in diesem Beispiel lautet die App-ID für diese App „ab603c56068041afb2f6832e2a17e237“). Die SAML-Token stellen die Skype-ID als `extension_skypeId` bereit.

**Benutzeroberflächenkonfiguration:**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)

1. Wählen Sie nach der Authentifizierung Ihren Azure AD-Mandanten aus, indem Sie ihn in der oberen rechten Ecke der Seite auswählen.

1. Klicken Sie im Menü auf der linken Seite auf **Azure Active Directory**.

1. Wählen Sie im Abschnitt **Verwalten** die Option **App-Registrierungen** aus.

1. Suchen Sie in der Liste nach der Anwendung, für die Sie optionale Ansprüche konfigurieren möchten, und klicken Sie darauf.

1. Klicken Sie im Abschnitt **Verwalten** auf die Option **Tokenkonfiguration (Vorschau)** .

1. Wählen Sie **Optionalen Anspruch hinzufügen** aus, wählen Sie den Tokentyp **ID** aus, wählen Sie **upn** aus der Liste der Ansprüche aus, und klicken Sie dann auf **Hinzufügen**.

1. Wählen Sie **Optionalen Anspruch hinzufügen** aus, wählen Sie den Tokentyp **Zugriff** aus, wählen Sie **auth_time** aus der Liste der Ansprüche aus, und klicken Sie dann auf **Hinzufügen**.

1. Klicken Sie auf dem Übersichtsbildschirm „Tokenkonfiguration“ auf das Stiftsymbol neben **upn**, klicken Sie auf den Umschalter **Extern authentifiziert**, und klicken Sie dann auf **Speichern**.

1. Wählen Sie **Optionalen Anspruch hinzufügen** aus, wählen Sie den Tokentyp **SAML** aus, wählen Sie **extn.skypeID** aus der Liste der Ansprüche aus (gilt nur, wenn Sie ein Azure AD-Benutzerobjekt mit dem Namen „skypeID“ erstellt haben), und klicken Sie dann auf **Hinzufügen**.

    [![Zeigt, wie optionale Ansprüche mithilfe der Benutzeroberfläche konfiguriert werden.](./media/active-directory-optional-claims/token-config-example.png)](./media/active-directory-optional-claims/token-config-example.png)

**Manifestkonfiguration:**
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie nach der Authentifizierung Ihren Azure AD-Mandanten aus, indem Sie ihn in der oberen rechten Ecke der Seite auswählen.
1. Klicken Sie im Menü auf der linken Seite auf **Azure Active Directory**.
1. Suchen Sie in der Liste nach der Anwendung, für die Sie optionale Ansprüche konfigurieren möchten, und klicken Sie darauf.
1. Klicken Sie im Abschnitt **Verwalten** auf **Manifest**, um den Inline-Manifest-Editor zu öffnen.
1. Das Manifest kann mit diesem Editor direkt bearbeitet werden. Das Manifest folgt dem Schema für [Anwendungsentität].(https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest). Das Manifest wird nach dem Speichern automatisch formatiert. Neue Elemente werden der `OptionalClaims`-Eigenschaft hinzugefügt.

    ```json
            "optionalClaims": {
                "idToken": [ 
                     { 
                        "name": "upn", 
                        "essential": false, 
                        "additionalProperties": [ "include_externally_authenticated_upn"]  
                     }
                     ],
                "accessToken": [ 
                      {
                        "name": "auth_time", 
                        "essential": false
                      }
                     ],
            "saml2Token": [ 
                  { 
                    "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                    "source": "user", 
                    "essential": true
                  }
                 ]
        ``` 


1. When you're finished updating the manifest, click **Save** to save the manifest.

## Next steps

Learn more about the standard claims provided by Azure AD.

- [ID tokens](id-tokens.md)
- [Access tokens](access-tokens.md)
