---
title: Konfigurierbare Tokengültigkeitsdauer
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie die Gültigkeitsdauer für Zugriffs-, SAML- und ID-Token festlegen, die von Microsoft Identity Platform ausgestellt werden.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 04/08/2021
ms.author: ryanwi
ms.custom: aaddev, identityplatformtop40, content-perf, FY21Q1, contperf-fy21q1
ms.reviewer: hirsin, jlu, annaba
ms.openlocfilehash: e1753391c7b61b6e9bd9e6ac0d142b4ee94502d8
ms.sourcegitcommit: dddd1596fa368f68861856849fbbbb9ea55cb4c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107363970"
---
# <a name="configurable-token-lifetimes-in-the-microsoft-identity-platform-preview"></a>Konfigurierbare Tokengültigkeitsdauer in Microsoft Identity Platform (Vorschau)

Sie können die Gültigkeitsdauer eines Zugriffs-, ID- oder SAML-Tokens angeben, das von Microsoft Identity Platform ausgestellt wird. Die Tokengültigkeitsdauer können Sie für alle Apps Ihrer Organisation, für eine mehrinstanzenfähige Anwendung (Multiorganisationsanwendung) oder für einen bestimmten Dienstprinzipal in Ihrer Organisation festlegen. Allerdings unterstützen wir derzeit nicht das Konfigurieren der Tokenlebensdauer bei [Dienstprinzipalen für verwaltete Identitäten](../managed-identities-azure-resources/overview.md).

In Azure AD steht ein Richtlinienobjekt für eine Reihe von Regeln, die für einzelne Anwendungen oder alle Anwendungen in einer Organisation erzwungen werden. Jeder Richtlinientyp verfügt über eine eindeutige Struktur mit einem Satz von Eigenschaften, die auf Objekte angewendet werden, denen sie zugewiesen sind.

Sie können eine Richtlinie als Standardrichtlinie für Ihre Organisation festlegen. Die Richtlinie wird auf alle Anwendungen der Organisation angewendet, sofern sie nicht von einer Richtlinie mit einer höheren Priorität außer Kraft gesetzt wird. Sie können eine Richtlinie auch bestimmten Anwendungen zuweisen. Die Reihenfolge der Priorität variiert je nach Richtlinientyp.

Entsprechende Beispiele finden Sie unter [Beispiele zum Konfigurieren der Tokengültigkeitsdauer](configure-token-lifetimes.md).

> [!NOTE]
> Die konfigurierbare Richtlinie für die Tokengültigkeitsdauer gilt nur für mobile und Desktopclients, die auf SharePoint Online- und OneDrive for Business-Ressourcen zugreifen, und nicht für Webbrowsersitzungen.
> Zum Verwalten der Gültigkeitsdauer von Webbrowsersitzungen für SharePoint Online und OneDrive for Business verwenden Sie das Feature [Sitzungsdauer für bedingten Zugriff](../conditional-access/howto-conditional-access-session-lifetime.md). Im [SharePoint Online-Blog](https://techcommunity.microsoft.com/t5/SharePoint-Blog/Introducing-Idle-Session-Timeout-in-SharePoint-and-OneDrive/ba-p/119208) finden Sie weitere Informationen zum Konfigurieren von Timeouts für Leerlaufsitzungen.

## <a name="license-requirements"></a>Lizenzanforderungen

Für die Verwendung dieses Features ist eine Azure AD Premium P1-Lizenz erforderlich. Um die richtige Lizenz für Ihre Anforderungen zu ermitteln, lesen Sie [Vergleich der allgemein verfügbaren Features der Editionen Free und Premium](https://azure.microsoft.com/pricing/details/active-directory/).

Kunden mit [Microsoft 365 Business-Lizenzen](/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description) haben auch Zugriff auf Funktionen für bedingten Zugriff.

## <a name="token-lifetime-policies-for-access-saml-and-id-tokens"></a>Richtlinien für die Tokenlebensdauer für Zugriffs-, SAML- und ID-Token

Sie können die Tokenlebensdauer-Richtlinien für Zugriffstoken, SAML-Token und ID-Token festlegen.

### <a name="access-tokens"></a>Zugriffstoken

Clients nutzen Zugriffstoken, um auf eine geschützte Ressource zuzugreifen. Ein Zugriffstoken kann nur für eine bestimmte Kombination aus Benutzer, Client und Ressource verwendet werden. Zugriffstoken können nicht widerrufen werden und sind bis zu ihrem Ablauf gültig. Ein böswilliger Akteur, der ein Zugriffstoken abgerufen hat, kann es während seiner gesamten Lebensdauer verwenden. Das Anpassen der Gültigkeitsdauer eines Zugriffstokens erfordert einen Kompromiss. Hierbei steht eine Verbesserung der Systemleistung einer Verlängerung der Zeitspanne gegenüber, über die der Client weiterhin Zugriff hat, nachdem das Konto des Benutzers deaktiviert wurde. Eine verbesserte Systemleistung wird dadurch erzielt, dass ein Client weniger oft ein neues Zugriffstoken abrufen muss.  Der Standardwert variiert abhängig von der Clientanwendung, die das Token anfordert. Beispielsweise erhalten CAE-fähige Clients (Continuous Access Evaluation), die CAE-fähige Sitzungen aushandeln, eine lange Tokengültigkeitsdauer (bis zu 28 Stunden). Wenn das Token abläuft, muss der Client das Aktualisierungstoken verwenden, um (meist im Hintergrund) ein neues Aktualisierungstoken und Zugriffstoken abzurufen.

### <a name="saml-tokens"></a>SAML-Token

SAML-Token werden in vielen webbasierten SaaS-Anwendungen verwendet und über den SAML2-Protokollendpunkt von Azure Active Directory abgerufen. Sie werden auch in Anwendungen genutzt, in denen WS-Verbund verwendet wird. Die Standardlebensdauer des Tokens beträgt 1 Stunde. Für eine Anwendung wird die Lebensdauer des Tokens durch den Wert „NotOnOrAfter“ des `<conditions …>`-Elements im Token angegeben. Nach Ablauf der Lebensdauer des Tokens muss der Client eine neue Authentifizierungsanforderung initiieren, die häufig als Ergebnis des SSO-Sitzungstokens ohne interaktive Anmeldung erfüllt wird.

Der Wert von „NotOnOrAfter“ kann mithilfe des `AccessTokenLifetime`-Parameters in einer `TokenLifetimePolicy` geändert werden. Der Wert wird auf die in der Richtlinie konfigurierte Lebensdauer (sofern vorhanden) zuzüglich eines Zeitversatzfaktors von fünf Minuten festgelegt.

Die im Element `<SubjectConfirmationData>` angegebene Antragstellerbestätigung für „NotOnOrAfter“ ist von der Konfiguration der Tokenlebensdauer nicht betroffen. 

### <a name="id-tokens"></a>ID-Token

ID-Token werden an Websites und native Clients übergeben. ID-Token enthalten Profilinformationen zu einem Benutzer. Ein ID-Token ist an eine bestimmte Kombination von Benutzer und Client gebunden. ID-Token werden bis zu ihrem Ablaufdatum als gültig betrachtet. In der Regel passt eine Webanwendung die Gültigkeitsdauer der Sitzung eines Benutzers in der Anwendung an die Gültigkeitsdauer des für den Benutzer ausgegebenen ID-Tokens an. Sie können die Gültigkeitsdauer eines ID-Tokens anpassen, um zu steuern, wie oft die Webanwendung den Ablauf der Anwendungssitzung veranlasst und wie oft der Benutzer sich erneut bei Microsoft Identity Platform authentifizieren muss (entweder im Hintergrund oder interaktiv).

## <a name="token-lifetime-policies-for-refresh-tokens-and-session-tokens"></a>Richtlinien für die Tokenlebensdauer für Aktualisierungstoken und Sitzungstoken

Für Aktualisierungs- und Sitzungstoken können keine Richtlinien für die Tokenlebensdauer festgelegt werden.

> [!IMPORTANT]
> Ab dem 30. Januar 2021 können Sie die Gültigkeitsdauer von Aktualisierungs- und Sitzungstoken nicht mehr konfigurieren. Azure Active Directory berücksichtigt die Konfigurationen von Aktualisierungs- und Sitzungstoken in vorhandenen Richtlinien nicht mehr.  Für neue Token, die nach dem Ablauf vorhandener Token ausgegeben werden, wird jetzt die [Standardkonfiguration](#configurable-token-lifetime-properties) festgelegt. Die Gültigkeitsdauer von Zugriffs-, SAML- und ID-Token kann jedoch auch noch nach der Einstellung der Konfiguration von Aktualisierungs- und Sitzungstoken konfiguriert werden.
>
> Die Gültigkeitsdauer des vorhandenen Tokens wird nicht geändert. Nach dem Ablauf des Tokens wird auf Basis des Standardwerts ein neues Token ausgegeben.
>
> Wenn Sie weiterhin definieren möchten, nach welcher Zeit ein Benutzer zur erneuten Anmeldung aufgefordert werden soll, können Sie die Anmeldehäufigkeit im bedingten Zugriff konfigurieren. Weitere Informationen zum bedingten Zugriff finden Sie unter [Konfigurieren der Verwaltung von Authentifizierungssitzungen mit bedingtem Zugriff](../conditional-access/howto-conditional-access-session-lifetime.md).

## <a name="configurable-token-lifetime-properties"></a>Konfigurierbare Eigenschaften der Tokengültigkeitsdauer
Eine Tokengültigkeitsdauer-Richtlinie ist ein Richtlinienobjekt, das Regeln für die Tokengültigkeitsdauer enthält. Diese Richtlinie steuert, wie lange Zugriffstoken und ID-Token für diese Ressource als gültig angesehen werden. Richtlinien für die Tokengültigkeitsdauer können nicht für Aktualisierungs- und Sitzungstoken festgelegt werden. Wenn keine Richtlinie festgelegt ist, erzwingt das System den Standardwert für die Gültigkeitsdauer.

### <a name="access-id-and-saml2-token-lifetime-policy-properties"></a>Eigenschaften der Richtlinien für die Gültigkeitsdauer von Zugriffs-, ID- und SAML2-Token

Durch das Reduzieren des Werts für die Eigenschaft „Gültigkeitsdauer Zugriffstoken“ wird das Risiko verringert, dass ein Zugriffstoken oder ID-Token von einem böswilligen Akteur für einen längeren Zeitraum verwendet wird. (Diese Token können nicht widerrufen werden.) Der Nachteil hierbei ist, dass die Leistung beeinträchtigt wird, da die Token häufiger ersetzt werden müssen.

Ein Beispiel finden Sie unter [Erstellen einer Richtlinie für die Webanmeldung](configure-token-lifetimes.md#create-a-policy-for-web-sign-in).

Die folgenden Eigenschaften und die entsprechenden Werte haben Auswirkungen auf die Konfiguration von Zugriffs-, ID- und SAML2-Token:

- **Eigenschaft**: Gültigkeitsdauer des Zugriffstokens
- **Zeichenfolge der Richtlinieneigenschaft**: AccessTokenLifetime
- **Betrifft**: Zugriffstoken, ID-Token, SAML2-Token
- **Standardwert**:
    - Zugriffstoken: hängt von der Clientanwendung ab, die das Token anfordert. Beispielsweise erhalten CAE-fähige Clients (Continuous Access Evaluation), die CAE-fähige Sitzungen aushandeln, eine lange Tokengültigkeitsdauer (bis zu 28 Stunden).
    - ID-Token, SAML2-Token: 1 Stunde
- **Minimum**: 10 Minuten
- **Maximum:** 1 Tag

### <a name="refresh-and-session-token-lifetime-policy-properties"></a>Eigenschaften von Tokenlebensdauer-Richtlinien für Aktualisierungs- und Sitzungstoken

Die folgenden Eigenschaften und die entsprechenden Werte haben Auswirkungen auf die Konfiguration von Aktualisierungs- und Sitzungstoken. Nach der Ausmusterung der Konfiguration für Aktualisierungs- und Sitzungstoken am 30. Januar 2021 berücksichtigt Azure AD nur die unten beschriebenen Standardwerte. Wenn Sie sich entscheiden, zum Verwalten der Anmeldehäufigkeit nicht den [bedingten Zugriff](../conditional-access/howto-conditional-access-session-lifetime.md) zu verwenden, wird für Ihre Aktualisierungs- und Sitzungstoken an diesem Datum die Standardkonfiguration festgelegt, und Sie können ihre Gültigkeitsdauer nicht mehr ändern.  

|Eigenschaft   |Richtlinien-Eigenschaftszeichenfolge    |Betrifft |Standard |
|----------|-----------|------------|------------|
|Max. Zeit der Inaktivität für Aktualisierungstoken |MaxInactiveTime  |Aktualisierungstoken |90 Tage  |
|Max. Alter Single-Factor-Aktualisierungstoken  |MaxAgeSingleFactor  |Aktualisierungstoken (für alle Benutzer)  |Bis zum Widerruf  |
|Max. Alter Multi-Factor-Aktualisierungstoken  |MaxAgeMultiFactor  |Aktualisierungstoken (für alle Benutzer) |Bis zum Widerruf  |
|Max. Alter Single-Factor-Sitzungstoken  |MaxAgeSessionSingleFactor |Sitzungstoken (beständig und nicht beständig)  |Bis zum Widerruf |
|Max. Alter Multi-Factor-Sitzungstoken  |MaxAgeSessionMultiFactor  |Sitzungstoken (beständig und nicht beständig)  |Bis zum Widerruf |

Sie können PowerShell verwenden, um die Richtlinien zu suchen, die von der Ausmusterung betroffen sind.  Verwenden Sie die [PowerShell-Cmdlets](configure-token-lifetimes.md#get-started), um alle in Ihrer Organisation erstellten Richtlinien anzuzeigen oder zu ermitteln, welche Apps und Dienstprinzipale mit einer bestimmten Richtlinie verknüpft sind.

## <a name="policy-evaluation-and-prioritization"></a>Richtlinienauswertung und Priorisierung
Sie können eine Richtlinie für die Gültigkeitsdauer von Token erstellen und dann einer bestimmten Anwendung, Ihrer Organisation und Dienstprinzipalen zuweisen. Für eine bestimmte Anwendung können mehrere Richtlinien gelten. Folgende Regeln bestimmen, welche Tokengültigkeitsdauer-Richtlinie wirksam wird:

* Wenn eine Richtlinie explizit dem Dienstprinzipal zugewiesen ist, wird sie erzwungen.
* Wenn dem Dienstprinzipal nicht explizit eine Richtlinie zugewiesen ist, wird eine Richtlinie erzwungen, die explizit der übergeordneten Organisation des Dienstprinzipals zugewiesen ist.
* Wenn dem Dienstprinzipal oder der Organisation nicht explizit eine Richtlinie zugewiesen ist, wird die Richtlinie erzwungen, die der Anwendung zugewiesen ist.
* Wenn dem Dienstprinzipal, der Organisation oder dem Anwendungsobjekt keine Richtlinie zugewiesen ist, werden die Standardwerte erzwungen. (Siehe Tabelle unter [Konfigurierbare Eigenschaften der Tokengültigkeitsdauer](#configurable-token-lifetime-properties).)

Ausführliche Informationen zu den Beziehungen zwischen Anwendungsobjekten und Dienstprinzipalobjekten finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](app-objects-and-service-principals.md).

Die Gültigkeit des Tokens wird zum Zeitpunkt seiner Verwendung überprüft. Die Richtlinie mit der höchsten Priorität für die Anwendung, auf die zugegriffen wird, wird wirksam.

Alle hier verwendeten Zeiträume werden nach dem C#-Objekt [TimeSpan](/dotnet/api/system.timespan) (D.HH:MM:SS) formatiert.  Danach werden 80 Tage und 30 Minuten im Format `80.00:30:00` dargestellt.  Das führende D kann gelöscht werden, wenn der Wert 0 ist. Danach werden 90 Minuten im Format `00:90:00` dargestellt.  

## <a name="cmdlet-reference"></a>Cmdlet-Referenz

Dies sind die Cmdlets im [Modul „Azure Active Directory PowerShell für Graph (Vorschau)](/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true#service-principals).

### <a name="manage-policies"></a>Verwalten von Richtlinien

Sie können die folgenden Cmdlets zum Verwalten von Richtlinien verwenden.

| Cmdlet | Beschreibung | 
| --- | --- |
| [New-AzureADPolicy](/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Erstellt eine neue Richtlinie. |
| [Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Ruft alle Azure AD-Richtlinien oder eine angegebene Richtlinie ab. |
| [Get-AzureADPolicyAppliedObject](/powershell/module/azuread/get-azureadpolicyappliedobject?view=azureadps-2.0-preview&preserve-view=true) | Ruft alle Apps und Dienstprinzipale ab, die mit einer Richtlinie verknüpft sind. |
| [Set-AzureADPolicy](/powershell/module/azuread/set-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Aktualisiert eine vorhandene Richtlinie. |
| [Remove-AzureADPolicy](/powershell/module/azuread/remove-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Löscht die angegebene Richtlinie. |

### <a name="application-policies"></a>Anwendungsrichtlinien
Sie können die folgenden Cmdlets für Anwendungsrichtlinien verwenden.</br></br>

| Cmdlet | Beschreibung | 
| --- | --- |
| [Add-AzureADApplicationPolicy](/powershell/module/azuread/add-azureadapplicationpolicy?view=azureadps-2.0-preview&preserve-view=true) | Verknüpft die angegebene Richtlinie mit einer Anwendung. |
| [Get-AzureADApplicationPolicy](/powershell/module/azuread/get-azureadapplicationpolicy?view=azureadps-2.0-preview&preserve-view=true) | Ruft die Richtlinie ab, die einer Anwendung zugewiesen ist. |
| [Remove-AzureADApplicationPolicy](/powershell/module/azuread/remove-azureadapplicationpolicy?view=azureadps-2.0-preview&preserve-view=true) | Entfernt eine Richtlinie aus einer Anwendung. |

### <a name="service-principal-policies"></a>Dienstprinzipalrichtlinien
Sie können die folgenden Cmdlets für Dienstprinzipalrichtlinien verwenden.

| Cmdlet | Beschreibung | 
| --- | --- |
| [Add-AzureADServicePrincipalPolicy](/powershell/module/azuread/add-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) | Verknüpft die angegebene Richtlinie mit einem Dienstprinzipal. |
| [Get-AzureADServicePrincipalPolicy](/powershell/module/azuread/get-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) | Ruft alle Richtlinien ab, die mit dem angegebenen Dienstprinzipal verknüpft sind.|
| [Remove-AzureADServicePrincipalPolicy](/powershell/module/azuread/remove-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) | Entfernt die Richtlinie aus dem angegebenen Dienstprinzipal.|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Beispiele zum Konfigurieren der Tokengültigkeitsdauer](configure-token-lifetimes.md).
