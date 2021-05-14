---
title: Anpassen der App-Ansprüche von Azure AD-Mandanten (PowerShell)
titleSuffix: Microsoft identity platform
description: Auf dieser Seite wird die Anspruchszuordnung in Azure Active Directory beschreiben.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: how-to
ms.date: 08/25/2020
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin, jeedes, luleon
ms.openlocfilehash: e77155f8a6efd3916ae90fcb562d688bb5b5126f
ms.sourcegitcommit: 950e98d5b3e9984b884673e59e0d2c9aaeabb5bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2021
ms.locfileid: "107598889"
---
# <a name="how-to-customize-claims-emitted-in-tokens-for-a-specific-app-in-a-tenant-preview"></a>Gewusst wie: Anpassen von in Token ausgegebenen Ansprüchen für eine bestimmte App in einem Mandanten (Vorschau)

> [!NOTE]
> Diese Funktion ersetzt die [Anpassung von Ansprüchen](active-directory-saml-claims-customization.md), die zurzeit über das Portal bereitgestellt wird. Wenn Sie in derselben Anwendung Ansprüche über das Portal anpassen, zusätzlich zur Graph/PowerShell-Methode (in diesem Dokument erklärt), werden für diese Anwendung ausgestellte Token die Konfiguration im Portal ignorieren. Alle Konfigurationen, die mithilfe von in diesem Dokument aufgeführten Methoden vorgenommen werden, werden im Portal nicht übernommen.

> [!NOTE]
> Diese Funktion befindet sich derzeit in der Public Preview. Seien Sie darauf vorbereitet, Änderungen zurückzusetzen bzw. zu löschen. Während der Public Preview ist die Funktion in allen Azure Active Directory-Abonnements verfügbar. Sobald die Funktion aber allgemein verfügbar wird, ist für einige Aspekte des Features unter Umständen ein Azure AD Premium-Abonnement erforderlich. Dieses Feature unterstützt das Konfigurieren von Richtlinien für die Anspruchszuordnung für WS-Fed-, SAML-, OAuth- und OpenID Connect-Protokolle.

Diese Funktion wird von Mandantenadministratoren verwendet, um die in Token ausgegebenen Ansprüche für eine bestimmte Anwendung in ihrem Mandanten anzupassen. Mithilfe von Richtlinien für die Anspruchszuordnung können Sie Folgendes:

- Auswählen, welche Ansprüche in Token enthalten sind
- Anspruchstypen erstellen, die es noch nicht gibt
- Auswählen, aus welcher Quelle die in bestimmten Ansprüchen ausgegebenen Daten stammen, oder diese ändern

In diesem Artikel führen wir Sie ausführlich durch einige allgemeine Szenarios, anhand derer veranschaulicht wird, wie Sie den [Richtlinientyp „Anspruchszuordnung“](reference-claims-mapping-policy-type.md) verwenden können.

Beim Erstellen einer Richtlinie für die Anspruchszuordnung können Sie auch einen Anspruch aus Erweiterungsattribut eines Verzeichnisschemas in Token ausgeben. Verwenden Sie *ExtensionID* für das Erweiterungsattribut anstelle von *ID* im `ClaimsSchema`-Element.  Weitere Informationen zu Erweiterungsattributen finden Sie unter [Verwenden von Erweiterungsattributen des Verzeichnisschemas](active-directory-schema-extensions.md).

## <a name="prerequisites"></a>Voraussetzungen

In den folgenden Beispielen erstellen, aktualisieren, verknüpfen und löschen Sie Richtlinien für Dienstprinzipale. Anspruchszuordnungsrichtlinien können nur Dienstprinzipalobjekten zugewiesen werden. Wenn Azure AD neu für Sie ist, sollten Sie sich darüber informieren, wie Sie [einen Azure AD-Mandanten erhalten](quickstart-create-new-tenant.md), bevor Sie mit diesen Beispielen fortfahren.

Führen Sie die folgenden Schritte aus, um zu beginnen:

1. Laden Sie die aktuelle [Public Preview des Azure AD PowerShell-Moduls](https://www.powershellgallery.com/packages/AzureADPreview) herunter.
1. Führen Sie den Befehl „Connect“ aus, um sich in Ihrem Azure AD-Administratorkonto anzumelden. Führen Sie diesen Befehl bei jedem Start einer neuen Sitzung aus.

   ``` powershell
   Connect-AzureAD -Confirm
   ```
1. Führen Sie den folgenden Befehl aus, um alle Richtlinien anzuzeigen, die in Ihrer Organisation erstellt wurden. Es wird empfohlen, dass Sie diesen Befehl nach den meisten Vorgängen in den folgenden Szenarios ausführen, um zu überprüfen, dass Ihre Richtlinien wie erwartet erstellt wurden.

   ``` powershell
   Get-AzureADPolicy
   ```

## <a name="omit-the-basic-claims-from-tokens"></a>Weglassen der grundlegenden Ansprüche aus Token

In diesem Beispiel erstellen Sie eine Richtlinie, die den [grundlegenden Anspruchssatz](reference-claims-mapping-policy-type.md#claim-sets) aus den Token entfernt, die an den verknüpften Dienstprinzipal ausgestellte wurden.

1. Erstellen Sie eine Anspruchszuordnungsrichtlinie. Diese Richtlinie, verknüpft mit bestimmten Dienstprinzipalen, entfernt den grundlegenden Anspruchsatz aus den Token.
   1. Führen Sie diesen Befehl aus, um die Richtlinie zu erstellen:

      ``` powershell
      New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"false"}}') -DisplayName "OmitBasicClaims" -Type "ClaimsMappingPolicy"
      ```
   2. Führen Sie den folgenden Befehl aus, um Ihre neue Richtlinie anzuzeigen und deren „ObjectId“ abzurufen:

      ``` powershell
      Get-AzureADPolicy
      ```
1. Weisen Sie die Richtlinie Ihrem Dienstprinzipal zu. Rufen Sie außerdem die „ObjectId“ Ihres Dienstprinzipals ab.
   1. Wenn Sie alle Dienstprinzipale Ihrer Organisation anzeigen möchten, können Sie [die Microsoft Graph-API abfragen](/graph/traverse-the-graph). Oder melden Sie sich im [Microsoft Graph-Explorer](https://developer.microsoft.com/graph/graph-explorer) bei Ihrem Azure AD-Konto an.
   2. Führen Sie den folgenden Befehl aus, wenn Sie über die ObjectId Ihres Dienstprinzipals verfügen:

      ``` powershell
      Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
      ```

## <a name="include-the-employeeid-and-tenantcountry-as-claims-in-tokens"></a>Schließen Sie EmployeeID und TenantCountry als Ansprüche in Token ein

In diesem Beispiel erstellen Sie eine Richtlinie, die Token, die für verknüpfte Dienstprinzipale ausgestellt wurden, EmployeeID und TenantCountry hinzufügt. EmployeeID wird als Namensanspruchstyp in den SAML-Token und JWTs ausgegeben. „TenantCountry“ wird als Anspruchstyp für Land/Region in den SAML-Token und JWTs ausgegeben. In diesem Beispiel fahren wir mit dem Einschließen des grundlegenden Anspruchssatzes in Token fort.

1. Erstellen Sie eine Anspruchszuordnungsrichtlinie. Diese Richtlinie, die mit bestimmten Dienstprinzipalen verknüpft ist, fügt den Token die Ansprüche „EmployeeID“ und „TenantCountry“ hinzu.
   1. Führen Sie den folgenden Befehl aus, um die Richtlinie zu erstellen:

      ``` powershell
      New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema": [{"Source":"user","ID":"employeeid","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/employeeid","JwtClaimType":"name"},{"Source":"company","ID":"tenantcountry","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/country","JwtClaimType":"country"}]}}') -DisplayName "ExtraClaimsExample" -Type "ClaimsMappingPolicy"
      ```

   2. Führen Sie den folgenden Befehl aus, um Ihre neue Richtlinie anzuzeigen und deren „ObjectId“ abzurufen:

      ``` powershell
      Get-AzureADPolicy
      ```
1. Weisen Sie die Richtlinie Ihrem Dienstprinzipal zu. Rufen Sie außerdem die „ObjectId“ Ihres Dienstprinzipals ab.
   1. Wenn Sie alle Dienstprinzipale Ihrer Organisation anzeigen möchten, können Sie [die Microsoft Graph-API abfragen](/graph/traverse-the-graph). Oder melden Sie sich im [Microsoft Graph-Explorer](https://developer.microsoft.com/graph/graph-explorer) bei Ihrem Azure AD-Konto an.
   2. Führen Sie den folgenden Befehl aus, wenn Sie über die ObjectId Ihres Dienstprinzipals verfügen:

      ``` powershell
      Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
      ```

## <a name="use-a-claims-transformation-in-tokens"></a>Verwenden Sie eine Anspruchstransformation in Token

In diesem Beispiel erstellen Sie eine Richtlinie, die einen benutzerdefinierten Anspruch „JoinedData“ an JWTs ausgibt, die für verknüpfte Dienstprinzipale ausgestellt werden. Dieser Anspruch enthält einen Wert, der durch das Verknüpfen der im Attribut „extensionattribute1“ im Benutzerobjekt mit „.sandbox“ gespeicherten Daten erstellt wird. In diesem Beispiel schließen wir den grundlegenden Anspruchssatz in Token aus.

1. Erstellen Sie eine Anspruchszuordnungsrichtlinie. Diese Richtlinie, die mit bestimmten Dienstprinzipalen verknüpft ist, fügt den Token die Ansprüche „EmployeeID“ und „TenantCountry“ hinzu.
   1. Führen Sie den folgenden Befehl aus, um die Richtlinie zu erstellen:

      ``` powershell
      New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema":[{"Source":"user","ID":"extensionattribute1"},{"Source":"transformation","ID":"DataJoin","TransformationId":"JoinTheData","JwtClaimType":"JoinedData"}],"ClaimsTransformations":[{"ID":"JoinTheData","TransformationMethod":"Join","InputClaims":[{"ClaimTypeReferenceId":"extensionattribute1","TransformationClaimType":"string1"}], "InputParameters": [{"ID":"string2","Value":"sandbox"},{"ID":"separator","Value":"."}],"OutputClaims":[{"ClaimTypeReferenceId":"DataJoin","TransformationClaimType":"outputClaim"}]}]}}') -DisplayName "TransformClaimsExample" -Type "ClaimsMappingPolicy"
      ```

   2. Führen Sie den folgenden Befehl aus, um Ihre neue Richtlinie anzuzeigen und deren „ObjectId“ abzurufen:

      ``` powershell
      Get-AzureADPolicy
      ```
1. Weisen Sie die Richtlinie Ihrem Dienstprinzipal zu. Rufen Sie außerdem die „ObjectId“ Ihres Dienstprinzipals ab.
   1. Wenn Sie alle Dienstprinzipale Ihrer Organisation anzeigen möchten, können Sie [die Microsoft Graph-API abfragen](/graph/traverse-the-graph). Oder melden Sie sich im [Microsoft Graph-Explorer](https://developer.microsoft.com/graph/graph-explorer) bei Ihrem Azure AD-Konto an.
   2. Führen Sie den folgenden Befehl aus, wenn Sie über die ObjectId Ihres Dienstprinzipals verfügen:

      ``` powershell
      Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
      ```

## <a name="security-considerations"></a>Sicherheitshinweise

Anwendungen, die Token empfangen, basieren auf der Tatsache, dass die Anspruchswerte von Azure AD autoritativ ausgestellt werden und nicht manipuliert werden können. Wenn Sie jedoch den Tokeninhalt über Anspruchszuordnungsrichtlinien ändern, sind diese Annahmen möglicherweise nicht mehr korrekt. Anwendungen müssen explizit bestätigen, dass Token vom Ersteller der Anspruchszuordnungsrichtlinie geändert wurden, um sich vor Anspruchszuordnungsrichtlinien zu schützen, die von böswilligen Akteuren erstellt wurden. Dafür gibt es folgende Möglichkeiten:

- Konfigurieren eines benutzerdefinierten Signaturschlüssels
- Aktualisieren des Anwendungsmanifests, um zugeordnete Ansprüche zu akzeptieren
 
Andernfalls gibt Azure AD einen [`AADSTS50146`Fehlercode](reference-aadsts-error-codes.md#aadsts-error-codes) zurück.

### <a name="custom-signing-key"></a>Benutzerdefinierte Signaturschlüssel

Zum Hinzufügen eines benutzerdefinierten Signaturschlüssels zum Dienstprinzipalobjekt können Sie mit dem Azure PowerShell-Cmdlet [`New-AzureADApplicationKeyCredential`](/powerShell/module/Azuread/New-AzureADApplicationKeyCredential) Anmeldeinformationen für einen Zertifikatschlüssel für Ihr Anwendungsobjekt erstellen.

Apps mit aktivierter Anspruchszuordnung müssen ihre Tokensignaturschlüssel überprüfen, indem sie `appid={client_id}` an ihre [OpenID Connect-Metadatenanforderungen](v2-protocols-oidc.md#fetch-the-openid-connect-metadata-document) anfügen. Im Folgenden finden Sie das Format des OpenID Connect-Metadatendokuments, das Sie verwenden sollten:

```
https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration?appid={client-id}
```

### <a name="update-the-application-manifest"></a>Aktualisieren des Anwendungsmanifests

Alternativ können Sie die `acceptMappedClaims`-Eigenschaft im [Anwendungsmanifest](reference-app-manifest.md) auf `true` festlegen. Laut Dokumentation für den [apiApplication-Ressourcentyp](/graph/api/resources/apiapplication#properties) kann eine Anwendung die Anspruchszuordnung verwenden, ohne einen benutzerdefinierten Signaturschlüssel anzugeben.

Dazu muss die angeforderte Tokenzielgruppe einen verifizierten Domänennamen Ihres Azure AD-Mandanten verwenden. Dies bedeutet, dass Sie `Application ID URI` (im Anwendungsmanifest durch `identifierUris` dargestellt) beispielsweise auf `https://contoso.com/my-api` oder `https://contoso.onmicrosoft.com/my-api` (Verwendung des Standardmandantennamens) festlegen.

Wenn Sie keine überprüfte Domäne verwenden, gibt Azure AD einen `AADSTS501461`-Fehlercode mit der Meldung aus, dass *AcceptMappedClaims nur für eine Tokenzielgruppe, die mit der Anwendungs-GUID übereinstimmt, oder einer Zielgruppe in den überprüften Domänen des Mandanten unterstützt wird, und Sie entweder den Ressourcenbezeichner ändern oder einen anwendungsspezifischen Signaturschlüssel verwenden müssen*.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie im Referenzartikel zum [Richtlinientyp „Anspruchszuordnung“](reference-claims-mapping-policy-type.md).
- Weitere Informationen dazu, wie Sie über das Azure-Portal die im SAML-Token ausgestellten Ansprüche anpassen, finden Sie unter [Vorgehensweise: Anpassen von Ansprüchen im SAML-Token für Unternehmensanwendungen](active-directory-saml-claims-customization.md)
- Weitere Informationen zu Erweiterungsattributen finden Sie unter [Verwenden von Erweiterungsattributen des Verzeichnisschemas in Ansprüchen](active-directory-schema-extensions.md).
