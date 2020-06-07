---
title: Bereitstellen optionaler Ansprüche für Azure AD-Apps
titleSuffix: Microsoft identity platform
description: Hinzufügen benutzerdefinierter oder zusätzlicher Ansprüche zu den SAML 2.0- und JWT-Token (JSON Web Token), die von Azure Active Directory ausgestellt werden.
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/22/2020
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin, keyam
ms.custom: aaddev
ms.openlocfilehash: 59f42f7c1fcdfef29becfb4a046753650ae9d14f
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83737553"
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

| Kontotyp               | v1.0-Token | v2.0-Token |
|----------------------------|-------------|-------------|
| Persönliches Microsoft-Konto | –         | Unterstützt   |
| Azure AD-Konto           | Unterstützt   | Unterstützt   |

## <a name="v10-and-v20-optional-claims-set"></a>Gruppe optionaler Ansprüche in v1.0 und v2.0

Die Gruppe optionaler Ansprüche, die standardmäßig zur Verwendung in Anwendungen bereitstehen, sind nachfolgend aufgeführt. Informationen zum Hinzufügen benutzerdefinierter optionaler Ansprüche für Ihre Anwendung finden Sie unter [Verzeichniserweiterungen](#configuring-directory-extension-optional-claims) weiter unten. Beim Hinzufügen von Ansprüchen zum **Zugriffstoken** gilt der Anspruch für Zugriffstoken, die *für* die Anwendung (eine Web-API) und nicht *von* der Anwendung angefordert werden. Unabhängig vom Client, der auf Ihre API zugreift, sind so die richtigen Daten im Zugriffstoken vorhanden, die zum Authentifizieren bei Ihrer API verwendet werden.

> [!NOTE]
> Die Mehrzahl dieser Ansprüche kann in JWTs für v1.0- und v2.0-Token, jedoch nicht in SAML-Token eingeschlossen werden. Ausnahmen stellen die Ansprüche dar, die einen entsprechenden Hinweis in der Spalte „Tokentyp“ enthalten. Consumerkonten unterstützen eine Teilmenge dieser Ansprüche, die in der Spalte als „Benutzertyp“ gekennzeichnet sind.  Viele der aufgelisteten Ansprüche gelten nicht für Consumerbenutzer (sie besitzen keinen Mandanten, daher weist `tenant_ctry` keinen Wert auf).

**Tabelle 2: Gruppe optionaler Ansprüche in v1.0 und v2.0**

| Name                       |  BESCHREIBUNG   | Tokentyp | Benutzertyp | Notizen  |
|----------------------------|----------------|------------|-----------|--------|
| `auth_time`                | Zeitpunkt der letzten Authentifizierung des Benutzers. Siehe OpenID Connect-Spezifikation.| JWT        |           |  |
| `tenant_region_scope`      | Region des Ressourcenmandanten | JWT        |           | |
| `home_oid`                 | Bei Gastbenutzern: Die Objekt-ID des Benutzers im Basismandanten des Benutzers.| JWT        |           | |
| `sid`                      | Sitzungs-ID, die zur sitzungsbezogenen Abmeldung des Benutzers verwendet wird | JWT        |  Persönliche Konten und Azure AD-Konten   |         |
| `platf`                    | Geräteplattform    | JWT        |           | Beschränkt auf verwaltete Geräte, die den Gerätetyp überprüfen können.|
| `verified_primary_email`   | Wird aus der „PrimaryAuthoritativeEmail“ des Benutzers abgerufen      | JWT        |           |         |
| `verified_secondary_email` | Wird aus der „SecondaryAuthoritativeEmail“ des Benutzers abgerufen   | JWT        |           |        |
| `enfpolids`                | IDs erzwungener Richtlinien. Eine Liste der Richtlinien-IDs, die für den aktuellen Benutzer ausgewertet wurden. | JWT |  |  |
| `vnet`                     | Informationen zum VNET-Spezifizierer | JWT        |           |      |
| `fwd`                      | IP-Adresse.| JWT    |   | Fügt die ursprüngliche IPv4-Adresse des anfordernden Clients hinzu (wenn innerhalb eines VNET). |
| `ctry`                     | Land/Region des Benutzers | JWT |  | Azure AD gibt den optionalen Anspruch `ctry` zurück, wenn er vorhanden ist und der Wert des Anspruchs ein standardmäßiger aus zwei Buchstaben bestehender Länder-/Regionscode (wie z. B. FR, JP oder SZ) ist. |
| `tenant_ctry`              | Land/Region des Ressourcenmandanten | JWT | | |
| `xms_pdl`             | Bevorzugter Datenspeicherort   | JWT | | Für Multi-Geo-Mandanten ist der bevorzugte Datenspeicherort ein aus drei Buchstaben bestehender Code, der die geografische Region anzeigt, in der sich der Benutzer befindet. Weitere Informationen finden Sie unter [Azure Active Directory Connect-Synchronisierung: Konfigurieren des bevorzugten Datenspeicherorts für Office 365-Ressourcen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-preferreddatalocation).<br/>Zum Beispiel: `APC` für Asien-Pazifik. |
| `xms_pl`                   | Bevorzugte Benutzersprache  | JWT ||Die bevorzugte Sprache des Benutzers, falls festgelegt. Wird in Szenarios mit Gastzugriff aus dem Basismandanten abgerufen. Sie wird im Format Sprachkürzel-Länderkürzel angegeben (z. B. en-us). |
| `xms_tpl`                  | Bevorzugte Mandantensprache| JWT | | Die bevorzugte Sprache des Ressourcenmandanten, falls festgelegt. Sie wird in Form des Sprachkürzels angegeben (z. B.: en). |
| `ztdid`                    | ID der Bereitstellung ohne manuelles Eingreifen | JWT | | Die für [Windows Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) verwendete Geräteidentität |
| `email`                    | Die adressierbaren E-Mail-Adresse dieses Benutzers, wenn der Benutzer über eine verfügt.  | JWT, SAML | MSA, Azure AD | Dieser Wert ist standardmäßig enthalten, wenn der Benutzer ein Gast im Mandanten ist.  Für verwaltete Benutzer (Benutzer innerhalb des Mandanten) muss er über diese optionale Anforderung oder – nur in v2.0 – mit dem OpenID-Bereich angefordert werden.  Für verwaltete Benutzer muss die E-Mail-Adresse im [Office-Verwaltungsportal](https://portal.office.com/adminportal/home#/users) festgelegt sein.|
| `groups`| Optionale Formatierung für Gruppenansprüche |JWT, SAML| |Wird in Verbindung mit der Einstellung „GroupMembershipClaims“ im [Anwendungsmanifest](reference-app-manifest.md) verwendet, das ebenfalls festgelegt sein muss. Weitere Informationen finden Sie weiter unten unter [Gruppenansprüche](#configuring-groups-optional-claims). Weitere Informationen zu Gruppenansprüchen finden Sie unter [Konfigurieren von Gruppenansprüchen](../hybrid/how-to-connect-fed-group-claims.md).
| `acct`                | Benutzerkontostatus im Mandanten. | JWT, SAML | | Wenn der Benutzer dem Mandanten angehört, lautet der Wert `0`. Bei einem Gastbenutzer lautet der Wert `1`. |
| `upn`                      | Anspruch „UserPrincipalName“. | JWT, SAML  |           | Obwohl dieser Anspruch automatisch hinzugefügt wird, können Sie ihn als einen optionalen Anspruch angeben, um zusätzliche Eigenschaften zum Ändern des Verhaltens im Fall eines Gastbenutzer anzufügen.  |

## <a name="v20-specific-optional-claims-set"></a>v2.0-spezifischer optionaler Anspruchssatz

Diese Ansprüche sind in Azure AD v1.0-Token immer enthalten, jedoch nie in v2.0-Token, sofern nicht angefordert. Diese Ansprüche gelten nur für JWTs (ID-Token und Zugriffstoken).

**Tabelle 3: Nur in v2.0 enthaltene optionale Ansprüche**

| JWT-Anspruch     | Name                            | BESCHREIBUNG                                | Notizen |
|---------------|---------------------------------|-------------|-------|
| `ipaddr`      | IP-Adresse                      | Die IP-Adresse, von der aus sich der Client angemeldet hat.   |       |
| `onprem_sid`  | Lokale Sicherheits-ID |                                             |       |
| `pwd_exp`     | Kennwortablaufzeit        | Datum und Uhrzeit, zu der das Kennwort abläuft. |       |
| `pwd_url`     | Kennwortänderungs-URL             | Eine URL, die der Benutzer besuchen kann, um sein Kennwort zu ändern.   |   |
| `in_corp`     | Innerhalb des Unternehmensnetzwerks        | Signalisiert, ob sich der Client aus dem Unternehmensnetzwerk anmeldet. Andernfalls ist der Anspruch nicht enthalten.   |  Basierend auf den Einstellungen für [vertrauenswürdige IP-Adressen](../authentication/howto-mfa-mfasettings.md#trusted-ips) in der MFA.    |
| `nickname`    | Spitzname                        | Ein zusätzlicher Name für den Benutzer. Der Spitzname unterscheidet sich vom Vor- oder Nachnamen. Erfordert den Bereich `profile`.|
| `family_name` | Last Name (Nachname)                       | Gibt den Nachnamen des Benutzers entsprechend der Definition im Benutzerobjekt an. <br>„family_name“: „Miller“ | Wird in MSA und Azure AD unterstützt. Erfordert den Bereich `profile`.   |
| `given_name`  | Vorname                      | Gibt den Vornamen des Benutzers entsprechend der Definition im Benutzerobjekt an.<br>"given_name": "Frank"                   | Wird in MSA und Azure AD unterstützt.  Erfordert den Bereich `profile`. |
| `upn`         | Benutzerprinzipalname | Ein Bezeichner für den Benutzer, der mit dem Parameter „username_hint“ verwendet werden kann.  Dies ist kein dauerhafter Bezeichner für den Benutzer und sollte möglichst nicht zur Datenzuordnung verwendet werden. | Informationen zur Konfiguration des Anspruchs finden Sie weiter unten unter [Zusätzliche Eigenschaften](#additional-properties-of-optional-claims). Erfordert den Bereich `profile`.|

### <a name="additional-properties-of-optional-claims"></a>Zusätzliche Eigenschaften optionaler Ansprüche

Einige optionale Ansprüche können so konfiguriert werden, dass sie auf andere Art zurückgegeben werden. Diese zusätzlichen Eigenschaften unterstützen in erster Linie die Migration lokaler Anwendungen mit unterschiedlichen Datenerwartungen. So ist `include_externally_authenticated_upn_without_hash` beispielsweise hilfreich bei Clients, die keine Hashzeichen (`#`) im UPN verarbeiten können.

**Tabelle 4: Werte zum Konfigurieren optionaler Ansprüche**

| Eigenschaftenname  | Name der zusätzlichen Eigenschaft | BESCHREIBUNG |
|----------------|--------------------------|-------------|
| `upn`          |                          | Kann für SAML- und JWT-Antworten und für v1.0- und v2.0-Token verwendet werden. |
|                | `include_externally_authenticated_upn`  | Bezieht den Gast-UPN ein, wie er im Ressourcenmandanten gespeichert ist. Zum Beispiel, `foo_hometenant.com#EXT#@resourcetenant.com` |
|                | `include_externally_authenticated_upn_without_hash` | Wie oben, außer dass die Hashzeichen (`#`) durch Unterstriche (`_`) ersetzt werden, z. B. `foo_hometenant.com_EXT_@resourcetenant.com` |

#### <a name="additional-properties-example"></a>Beispiel für zusätzliche Eigenschaften

```json
"optionalClaims": {
    "idToken": [
        {
            "name": "upn",
            "essential": false,
            "additionalProperties": [
                "include_externally_authenticated_upn"
            ]
        }
    ]
}
```

Dieses OptionalClaims-Objekt bewirkt, dass das an den Client zurückgegebene ID-Token einen UPN-Anspruch mit den zusätzlichen Informationen zum Home- und zum Ressourcenmandanten enthält. Der `upn`-Anspruch wird im Token nur dann geändert, wenn der Benutzer ein Gastbenutzer im Mandanten ist (und einen anderen Identitätsanbieter für die Authentifizierung verwendet).

## <a name="configuring-optional-claims"></a>Konfigurieren optionaler Ansprüche

> [!IMPORTANT]
> Zugriffstoken werden **immer** mit dem Manifest der Ressource und nicht des Clients generiert.  In der Anforderung `...scope=https://graph.microsoft.com/user.read...` ist die Ressource also die Microsoft Graph-API.  Folglich wird das Zugriffstoken unter Verwendung des Microsoft Graph-API-Manifests und nicht des Clientmanifests erstellt.  Eine Änderung des Manifests für Ihre Anwendung führt niemals dazu, dass Token für die Microsoft Graph-API anders aussehen.  Um zu überprüfen, ob Ihre Änderungen an `accessToken` wirksam sind, fordern Sie ein Token für Ihre Anwendung, aber keine andere App an.

Sie können optionale Ansprüche für Ihre Anwendung über die Benutzeroberfläche oder das Anwendungsmanifest konfigurieren.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Suchen Sie nach **Azure Active Directory**, und wählen Sie diese Option aus.
1. Wählen Sie im Abschnitt **Verwalten** die Option **App-Registrierungen** aus.
1. Wählen Sie in der Liste die Anwendung aus, für die Sie optionale Ansprüche konfigurieren möchten.

**Konfigurieren optionaler Ansprüche über die Benutzeroberfläche:**

[![Zeigt, wie optionale Ansprüche mithilfe der Benutzeroberfläche konfiguriert werden.](./media/active-directory-optional-claims/token-configuration.png)](./media/active-directory-optional-claims/token-configuration.png)

1. Wählen Sie im Abschnitt **Verwalten** die Option **Tokenkonfiguration** aus.
1. Wählen Sie **Optionalen Anspruch hinzufügen** aus.
1. Wählen Sie den Tokentyp aus, den Sie konfigurieren möchten.
1. Wählen Sie die hinzuzufügenden optionalen Ansprüche aus.
1. Wählen Sie **Hinzufügen**.

**Konfigurieren optionaler Ansprüche über das Anwendungsmanifest:**

[![Zeigt, wie optionale Ansprüche mithilfe des Anwendungsmanifests konfiguriert werden.](./media/active-directory-optional-claims/app-manifest.png)](./media/active-directory-optional-claims/app-manifest.png)

1. Wählen Sie im Abschnitt **Verwalten** die Option **Manifest** aus. Ein webbasierter Manifest-Editor wird geöffnet, mit dem Sie das Manifest bearbeiten können. Optional können Sie **Herunterladen** wählen und das Manifest lokal bearbeiten und dann **Hochladen** verwenden, um es wieder auf Ihre Anwendung anzuwenden. Weitere Informationen zum Anwendungsmanifest finden Sie im Artikel [Grundlegendes zum Azure AD-Anwendungsmanifest](reference-app-manifest.md).

    Der folgende Eintrag im Anwendungsmanifest fügt ID-, Zugriffs- und SAML-Token die optionalen Ansprüche „auth_time“, „ipaddr“ und „upn“ hinzu.

    ```json
    "optionalClaims": {
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

2. Wenn Sie fertig sind, wählen Sie **Speichern** aus. Nun werden die angegebenen optionalen Ansprüche in die Token für die Anwendung eingefügt.

### <a name="optionalclaims-type"></a>Typ „OptionalClaims“

Deklariert die von einer Anwendung angeforderten optionalen Ansprüche. Eine Anwendung kann optionale Ansprüche so konfigurieren, dass sie in jedem der drei Tokentypen (ID-Token, Zugriffstoken, SAML 2-Token) zurückgegeben werden, die vom Sicherheitstokendienst empfangen werden können. Die Anwendung kann einen anderen Satz optionaler Ansprüche für die Rückgabe im jeweiligen Tokentyp konfigurieren. Die „OptionalClaims“-Eigenschaft der Anwendungsentität ist ein „OptionalClaims“-Objekt.

**Tabelle 5: Eigenschaften des Typs „OptionalClaims“**

| Name          | type                       | BESCHREIBUNG                                           |
|---------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Sammlung (OptionalClaim) | Die optionalen Ansprüche, die im JWT-ID-Token zurückgegeben werden.     |
| `accessToken` | Sammlung (OptionalClaim) | Die optionalen Ansprüche, die im JWT-Zugriffstoken zurückgegeben werden. |
| `saml2Token`  | Sammlung (OptionalClaim) | Die optionalen Ansprüche, die im SAML-Token zurückgegeben werden.       |

### <a name="optionalclaim-type"></a>Typ „OptionalClaim“

Enthält einen optionalen Anspruch, der einer Anwendung oder einem Dienstprinzipal zugeordnet ist. Die „idToken“-, „accessToken“- und „saml2Token“-Eigenschaften des [OptionalClaims](https://docs.microsoft.com/graph/api/resources/optionalclaims?view=graph-rest-1.0)-Typs ist eine Sammlung von „OptionalClaim“.
Wenn durch einen bestimmten Anspruch unterstützt, können Sie auch das Verhalten von „OptionalClaim“ mithilfe des Felds „AdditionalProperties“ ändern.

**Tabelle 6: Eigenschaften des Typs „OptionalClaim“**

| Name                   | type                    | BESCHREIBUNG                                                                                                                                                                                                                                                                                                   |
|------------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | Der Name des optionalen Anspruchs.                                                                                                                                                                                                                                                                               |
| `source`               | Edm.String              | Die Quelle (Verzeichnisobjekt) des Anspruchs. Es gibt vordefinierte Ansprüche und benutzerdefinierte Ansprüche aus Erweiterungseigenschaften. Wenn der Quellwert „null“ ist, ist der Anspruch ein vordefinierter optionaler Anspruch. Wenn der Quellwert „user“ ist, ist der Wert in der „name“-Eigenschaft die Erweiterungseigenschaft aus dem Benutzerobjekt. |
| `essential`            | Edm.Boolean             | Wenn der Wert „true“ ist, ist der vom Client angegebene Anspruch erforderlich, um eine reibungslose Autorisierung für die jeweilige vom Endbenutzer angeforderte Aufgabe sicherzustellen. Der Standardwert ist „FALSE“.                                                                                                                 |
| `additionalProperties` | Sammlung (Edm.String) | Zusätzliche Eigenschaften des Anspruchs. Wenn eine Eigenschaft in dieser Sammlung vorhanden ist, ändert sie das Verhalten des optionalen Anspruchs, der in der „name“-Eigenschaft angegeben ist.                                                                                                                                                   |

## <a name="configuring-directory-extension-optional-claims"></a>Konfigurieren von optionalen Ansprüchen für die Verzeichniserweiterung

Zusätzlich zu den optionalen Standardansprüchen können Sie Token auch so konfigurieren, dass sie Erweiterungen enthalten. Weitere Informationen finden Sie in der [Microsoft Graph-Dokumentation im Teil über extensionProperty](https://docs.microsoft.com/graph/api/resources/extensionproperty?view=graph-rest-1.0).

Optionale Ansprüche unterstützen keine Schemaerweiterungen und offenen Erweiterungen, sondern nur AAD Graph-Verzeichniserweiterungen. Diese Funktion ist nützlich, um zusätzliche Benutzerinformationen anzufügen, die von Ihrer App verwendet werden können, z. B. einen zusätzlichen Bezeichner oder eine wichtige Konfigurationsoption, die vom Benutzer festgelegt wurde. Ein Beispiel finden Sie unten auf dieser Seite.

> [!NOTE]
> Verzeichnisschemaerweiterungen sind ein auf Azure AD beschränktes Feature. Wenn Ihr Anwendungsmanifest eine benutzerdefinierte Erweiterung erfordert und sich ein MSA-Benutzer bei Ihrer App anmeldet, werden diese Erweiterungen nicht zurückgegeben.

### <a name="directory-extension-formatting"></a>Formatierung der Verzeichniserweiterung

Verwenden Sie beim Konfigurieren der optionalen Ansprüche der Verzeichniserweiterung mithilfe des Anwendungsmanifests den vollständigen Namen der Erweiterung (im Format `extension_<appid>_<attributename>`). Die `<appid>` muss mit der ID der Anwendung übereinstimmen, die den Anspruch anfordert.

Innerhalb des JWT werden diese Ansprüche mit dem folgenden Namensformat ausgegeben: `extn.<attributename>`

Innerhalb der SAML-Token werden diese Ansprüche mit dem folgenden URI-Format ausgegeben: `http://schemas.microsoft.com/identity/claims/extn.<attributename>`

## <a name="configuring-groups-optional-claims"></a>Konfigurieren von optionalen Gruppenansprüchen

   > [!NOTE]
   > Die Fähigkeit, Gruppennamen für Benutzer und Gruppen auszugeben, die lokal synchronisiert werden, befindet sich in der Phase der öffentlichen Vorschau.

Dieser Abschnitt behandelt die Konfigurationsoptionen unter den optionalen Ansprüchen, um die in Gruppenansprüchen verwendeten Gruppenattribute von der Standardgruppen-ObjectID in von lokalem Windows Active Directory synchronisierte Attribute zu ändern. Sie können optionale Gruppenansprüche für Ihre Anwendung über die Benutzeroberfläche oder das Anwendungsmanifest konfigurieren.

> [!IMPORTANT]
> Unter [Konfigurieren von Gruppenansprüchen für Anwendungen mit Azure AD](../hybrid/how-to-connect-fed-group-claims.md) finden Sie weitere Informationen, einschließlich wichtiger Einschränkungen bei der öffentlichen Vorschau von Gruppenansprüchen aus lokalen Attributen.

**Konfigurieren optionaler Gruppenansprüche über die Benutzeroberfläche:**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
1. Wählen Sie nach der Authentifizierung Ihren Azure AD-Mandanten aus, indem Sie ihn in der rechten oberen Ecke der Seite auswählen.
1. Wählen Sie im Menü auf der linken Seite **Azure Active Directory** aus.
1. Wählen Sie im Abschnitt **Verwalten** die Option **App-Registrierungen** aus.
1. Wählen Sie in der Liste die Anwendung aus, für die Sie optionale Ansprüche konfigurieren möchten.
1. Klicken Sie im Abschnitt **Verwalten** auf **Tokenkonfiguration**.
1. Wählen Sie **Anspruchsgruppen hinzufügen** aus.
1. Wählen Sie die Gruppentypen für die Rückgabe aus (**Alle Gruppen**, **SecurityGroup** oder **DirectoryRole**). Die Option **Alle Gruppen** umfasst **SecurityGroup**, **DirectoryRole** und **DistributionList**.
1. Optional: Klicken Sie auf die Eigenschaften des jeweiligen Tokentyps, um den Wert des Gruppenanspruchs so zu ändern, dass er lokale Gruppenattribute enthält, oder um den Anspruchstyp in eine Rolle zu ändern.
1. Wählen Sie **Speichern** aus.

**Konfigurieren optionaler Gruppenansprüche über das Anwendungsmanifest:**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
1. Wählen Sie nach der Authentifizierung Ihren Azure AD-Mandanten aus, indem Sie ihn in der rechten oberen Ecke der Seite auswählen.
1. Wählen Sie im Menü auf der linken Seite **Azure Active Directory** aus.
1. Wählen Sie in der Liste die Anwendung aus, für die Sie optionale Ansprüche konfigurieren möchten.
1. Wählen Sie im Abschnitt **Verwalten** die Option **Manifest** aus.
1. Fügen Sie den folgenden Eintrag mit dem Manifest-Editor hinzu:

   Gültige Werte sind:

   - „Alle“ (umfasst „SecurityGroup“, „DirectoryRole“ und „DistributionList“)
   - SecurityGroup
   - DirectoryRole

   Beispiel:

    ```json
    "groupMembershipClaims": "SecurityGroup"
    ```

   Standardmäßig werden ObjectIDs von Gruppen im Wert des Gruppenanspruchs ausgegeben.  Um den Anspruchswert so zu ändern, dass er lokale Gruppenattribute enthält, oder um den Anspruchstyp zu „role“ zu ändern, verwenden Sie die Konfiguration für optionale Ansprüche wie folgt:

1. Legen Sie optionale Ansprüche für die Gruppennamenkonfiguration fest.

   Wenn die Gruppen im Token die lokalen AD-Gruppenattribute im Abschnitt mit optionalen Ansprüchen enthalten sollen, geben Sie an, welcher optionale Anspruch des Tokentyps angewendet werden soll. Geben Sie außerdem den Namen des angeforderten optionalen Anspruchs sowie weitere gewünschte Eigenschaften an.  Es können mehrere Tokentypen aufgelistet werden:

   - idToken für das OIDC-ID-Token
   - accessToken für das OAuth-Zugriffstoken
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
   | **additionalProperties:** | Liste zusätzlicher Eigenschaften.  Gültige Optionen sind "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name" und "emit_as_roles". |

   In „additionalProperties“ ist nur eine dieser Optionen erforderlich: "sam_account_name", "dns_domain_and_sam_account_name" oder "netbios_domain_and_sam_account_name".  Wenn mehrere dieser Optionen vorhanden sind, wird die erste verwendet, alle weiteren werden ignoriert.

   Einige Anwendungen erfordern Gruppeninformationen über Benutzer im Rollenanspruch.  Fügen Sie "emit_as_roles" zu den zusätzlichen Eigenschaften hinzu, um den Anspruchstyp von einem Gruppenanspruch in einen Rollenanspruch zu ändern.  Die Gruppenwerte werden im Rollenanspruch ausgegeben.

   > [!NOTE]
   > Wenn „emit_as_roles“ verwendet wird, sind konfigurierte Anwendungsrollen, denen der Benutzer zugewiesen ist, nicht im Rollenanspruch enthalten.

**Beispiele:**

1) Ausgeben von Gruppen als Gruppennamen in OAuth-Zugriffstoken im Format „dnsDomainName\sAMAccountName“

    **Benutzeroberflächenkonfiguration:**

    [![Zeigt, wie optionale Ansprüche mithilfe der Benutzeroberfläche konfiguriert werden.](./media/active-directory-optional-claims/groups-example-1.png)](./media/active-directory-optional-claims/groups-example-1.png)

    **Eintrag des Anwendungsmanifests:**

    ```json
    "optionalClaims": {
        "accessToken": [
            {
                "name": "groups",
                "additionalProperties": [
                    "dns_domain_and_sam_account_name"
                ]
            }
        ]
    }
    ```

2) Ausgeben von Gruppennamen, die im Format „netbiosDomain\sAMAccountName“ als Rollenanspruch in SAML- und OIDC-ID-Token zurückgegeben werden sollen

    **Benutzeroberflächenkonfiguration:**

    [![Zeigt, wie optionale Ansprüche mithilfe der Benutzeroberfläche konfiguriert werden.](./media/active-directory-optional-claims/groups-example-2.png)](./media/active-directory-optional-claims/groups-example-2.png)

    **Eintrag des Anwendungsmanifests:**

    ```json
    "optionalClaims": {
        "saml2Token": [
            {
                "name": "groups",
                "additionalProperties": [
                    "netbios_name_and_sam_account_name",
                    "emit_as_roles"
                ]
            }
        ],
        "idToken": [
            {
                "name": "groups",
                "additionalProperties": [
                    "netbios_name_and_sam_account_name",
                    "emit_as_roles"
                ]
            }
        ]
    }
    ```

## <a name="optional-claims-example"></a>Beispiel für optionale Ansprüche

In diesem Abschnitt können Sie ein Szenario durchgehen, um zu erfahren, wie Sie optionale Ansprüche für Ihre Anwendung verwenden können.
Es sind mehrere Optionen für das Aktualisieren der Eigenschaften in der Identitätskonfiguration einer Anwendung zum Aktivieren und Konfigurieren optionaler Ansprüche verfügbar:

- Sie können die Benutzeroberfläche für die **Tokenkonfiguration** verwenden (siehe Beispiel unten).
- Sie können das **Manifest** verwenden (siehe Beispiel unten). Lesen Sie zuerst das Dokument [Grundlegendes zum Azure AD-Anwendungsmanifest](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest), um eine Einführung in das Manifest zu erhalten.
- Sie können zum Aktualisieren Ihrer Anwendung auch eine Anwendung schreiben, die die [Microsoft Graph-API](https://docs.microsoft.com/graph/use-the-api?context=graph%2Fapi%2F1.0&view=graph-rest-1.0) nutzt. Der Typ [OptionalClaims](https://docs.microsoft.com/graph/api/resources/optionalclaims?view=graph-rest-1.0) im Referenzhandbuch zur Microsoft Graph-API kann beim Konfigurieren der optionalen Ansprüche hilfreich sein.

**Beispiel:**

Im folgenden Beispiel verwenden Sie die Benutzeroberfläche für die **Tokenkonfiguration** und das **Manifest**, um den Zugriffstoken, ID-Token und SAML-Token optionale Ansprüche hinzuzufügen, die für Ihre Anwendung vorgesehen sind. Verschiedene optionale Ansprüche wurden den einzelnen Tokentypen hinzugefügt, die von der Anwendung empfangen werden können:

- Die ID-Token enthalten jetzt den UPN für Verbundbenutzer in vollständiger Form (`<upn>_<homedomain>#EXT#@<resourcedomain>`).
- Die Zugriffstoken, die andere Clients für diese Anwendung anfordern, enthalten jetzt den Anspruch „auth_time“.
- Die SAML-Token enthalten jetzt die Verzeichnisschemaerweiterung „skypeId“ (in diesem Beispiel lautet die App-ID für diese App „ab603c56068041afb2f6832e2a17e237“). Die SAML-Token stellen die Skype-ID als `extension_skypeId` bereit.

**Benutzeroberflächenkonfiguration:**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)

1. Wählen Sie nach der Authentifizierung Ihren Azure AD-Mandanten aus, indem Sie ihn in der rechten oberen Ecke der Seite auswählen.

1. Wählen Sie im Menü auf der linken Seite **Azure Active Directory** aus.

1. Wählen Sie im Abschnitt **Verwalten** die Option **App-Registrierungen** aus.

1. Suchen Sie in der Liste nach der Anwendung, für die Sie optionale Ansprüche konfigurieren möchten, und klicken Sie darauf.

1. Klicken Sie im Abschnitt **Verwalten** auf **Tokenkonfiguration**.

1. Klicken Sie auf **Optionalen Anspruch hinzufügen**, wählen Sie den Tokentyp **ID** und dann in der Liste der Ansprüche **upn** aus, und klicken Sie anschließend auf **Hinzufügen**.

1. Klicken Sie auf **Optionalen Anspruch hinzufügen**, wählen Sie den Tokentyp **Zugriff** und dann in der Liste der Ansprüche **auth_time** aus, und klicken Sie anschließend auf **Hinzufügen**.

1. Klicken Sie auf dem Übersichtsbildschirm „Tokenkonfiguration“ auf das Stiftsymbol neben **upn**, anschließend auf den Umschalter **Extern authentifiziert** und dann auf **Speichern**.

1. Klicken Sie auf **Optionalen Anspruch hinzufügen**, wählen Sie den Tokentyp **SAML** und dann in der Liste der Ansprüche **extn.skypeID** aus (gilt nur, wenn Sie ein Azure AD-Benutzerobjekt mit dem Namen „skypeID“ erstellt haben), und klicken Sie anschließend auf **Hinzufügen**.

    [![Zeigt, wie optionale Ansprüche mithilfe der Benutzeroberfläche konfiguriert werden.](./media/active-directory-optional-claims/token-config-example.png)](./media/active-directory-optional-claims/token-config-example.png)

**Manifestkonfiguration:**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie nach der Authentifizierung Ihren Azure AD-Mandanten aus, indem Sie ihn in der rechten oberen Ecke der Seite auswählen.
1. Wählen Sie im Menü auf der linken Seite **Azure Active Directory** aus.
1. Suchen Sie in der Liste nach der Anwendung, für die Sie optionale Ansprüche konfigurieren möchten, und klicken Sie darauf.
1. Klicken Sie im Abschnitt **Verwalten** auf **Manifest**, um den Inline-Manifest-Editor zu öffnen.
1. Das Manifest kann mit diesem Editor direkt bearbeitet werden. Das Manifest folgt dem Schema für die [Anwendungsentität](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest). Das Manifest wird nach dem Speichern automatisch formatiert. Neue Elemente werden der `OptionalClaims`-Eigenschaft hinzugefügt.

    ```json
    "optionalClaims": {
        "idToken": [
            {
                "name": "upn",
                "essential": false,
                "additionalProperties": [
                    "include_externally_authenticated_upn"
                ]
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
    }
    ```

1. Wenn Sie das Manifest aktualisiert haben, klicken Sie auf **Speichern**, um das Manifest zu speichern.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Standardansprüche, die von Azure AD bereitgestellt werden.

- [ID-Token](id-tokens.md)
- [Zugriffstoken](access-tokens.md)
