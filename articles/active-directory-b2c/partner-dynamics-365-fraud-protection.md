---
title: Tutorial zum Konfigurieren von Azure Active Directory B2C mit Microsoft Dynamics 365 Fraud Protection
titleSuffix: Azure AD B2C
description: Tutorial zum Konfigurieren von Azure Active Directory B2C mit Microsoft Dynamics 365 Fraud Protection zum Identifizieren von riskanten und betrügerischen Konten
services: active-directory-b2c
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 02/10/2021
ms.author: gasinh
ms.subservice: B2C
ms.openlocfilehash: 629daa968d548c06d176e6349382ad51349a37a0
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100416544"
---
# <a name="tutorial-configure-microsoft-dynamics-365-fraud-protection-with-azure-active-directory-b2c"></a>Tutorial: Konfigurieren von Microsoft Dynamics 365 Fraud Protection mit Azure Active Directory B2C

In diesem Beispieltutorial finden Sie eine Anleitung zum Integrieren von [Microsoft Dynamics 365 Fraud Protection](https://docs.microsoft.com/dynamics365/fraud-protection/overview) (DFP) in Azure Active Directory (AD) B2C.

Microsoft DFP bietet Kunden die Möglichkeit der Risikobewertung von Versuchen, neue Konten zu erstellen und sich beim Ökosystem des Kunden anzumelden, wenn diese betrügerisch sind. Microsoft DFP-Bewertung kann vom Kunden verwendet werden, um verdächtige Versuche, neue gefälschte Konten zu erstellen oder vorhandene Konten zu kompromittieren, zu blockieren oder abzuwehren. Der Kontoschutz umfasst einen durch künstliche Intelligenz unterstützten Gerätefingerabdruck, APIs für die Risikobewertung in Echtzeit, Regeln und Listen zur Optimierung der Risikostrategie gemäß den Geschäftsanforderungen des Kunden sowie eine Scorecard zum Überwachen der Effektivität des Betrugsschutzes und der entsprechenden Trends im Ökosystem des Kunden.

In diesem Beispiel integrieren wir die Kontoschutzfunktionen von Microsoft DFP in einen Azure AD B2C-Benutzerflow. Der Dienst erstellt extern Fingerabdrücke von jedem Anmelde- oder Registrierungsversuch und überwacht alle früheren oder aktuellen verdächtigen Verhaltensweisen. Azure AD B2C ruft einen Entscheidungsendpunkt von Microsoft DFP auf, der ein Ergebnis zurückgibt, das auf allen früheren und aktuellen Verhaltensweisen des identifizierten Benutzers sowie auf den im Microsoft DFP-Dienst angegebenen benutzerdefinierten Regeln basiert. Azure AD B2C trifft basierend auf diesem Ergebnis eine Genehmigungsentscheidung und übergibt diese an Microsoft DFP.

## <a name="prerequisites"></a>Voraussetzungen

Zunächst benötigen Sie Folgendes:

- Ein Azure-Abonnement. Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.

- Einen [Azure AD B2C-Mandanten](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant). Der Mandant ist mit Ihrem Azure-Abonnement verknüpft.

- Erwerben Sie ein Microsoft DFP-[Abonnement](https://dynamics.microsoft.com/pricing/#Sales). Sie können auch eine [Clienttestversion](https://dynamics.microsoft.com/ai/fraud-protection/signin/?RU=https%3A%2F%2Fdfp.microsoft.com%2Fsignin) einrichten.

## <a name="scenario-description"></a>Beschreibung des Szenarios

Die Microsoft DFP-Integration umfasst die folgenden Komponenten:

- **Azure AD B2C-Mandant**: Authentifiziert den Benutzer und fungiert als Client von Microsoft DFP. Hostet ein Fingerabdruckskript, das Identifikations- und Diagnosedaten jedes Benutzers sammelt, der eine Zielrichtlinie ausführt. Später werden Anmelde- oder Registrierungsversuche blockiert oder abgewehrt, wenn Microsoft DFP sie als verdächtig einstuft.

- **Benutzerdefinierter App-Dienst**: Eine Webanwendung, die zwei Zwecken dient.

  - Stellt HTML-Seiten bereit, die als Benutzeroberfläche von Identity Experience Framework verwendet werden. Ist für das Einbetten des Microsoft Dynamics 365-Fingerabdruckskripts verantwortlich.

  - Ein API-Controller mit RESTful-Endpunkten, der Microsoft DFP mit Azure AD B2C verbindet. Übernimmt die Datenverarbeitung und Struktur und erfüllt die Sicherheitsanforderungen von beiden.

- **Microsoft DFP-Fingerabdruckdienst**: Dynamisch eingebettetes Skript, das Gerätetelemetriedaten und selbstbestätigte Benutzerdetails protokolliert, um einen eindeutig identifizierbaren Fingerabdruck des Benutzers zu erstellen, der später im Entscheidungsprozess verwendet werden kann.

- **Microsoft DFP-API-Endpunkte**: Stellt das Entscheidungsergebnis bereit und akzeptiert einen endgültigen Status, der den von der Clientanwendung ausgeführten Vorgang widerspiegelt. Azure AD B2C kommuniziert aufgrund variierender Sicherheits- und API-Nutzdatenanforderungen nicht direkt mit den Endpunkten, sondern verwendet stattdessen den App-Dienst als Zwischeninstanz.

Im folgenden Architekturdiagramm ist die Implementierung dargestellt.

![Abbildung des Microsoft Dynamics 365 Fraud Protection-Architekturdiagramms](./media/partner-dynamics365-fraud-protection/microsoft-dynamics-365-fraud-protection-diagram.png)

|Schritt | BESCHREIBUNG |
|:-----| :-----------|
| 1. | Der Benutzer gelangt auf eine Anmeldeseite. Der Benutzer wählt die Registrierung zum Erstellen eines neuen Kontos aus und gibt Informationen auf der Seite ein. Azure AD B2C erfasst die Benutzerattribute.
| 2. | Azure AD B2C ruft die API der mittleren Ebene auf und übergibt die Benutzerattribute.
| 3. | Die API der mittleren Ebene erfasst die Benutzerattribute und wandelt sie in ein Format um, das von der Microsoft DFP-API verarbeitet werden kann. Danach werden die Daten an die Microsoft DFP-API gesendet.
| 4. | Nachdem die Microsoft DFP-API die Informationen verwendet und verarbeitet hat, wird das Ergebnis an die API der mittleren Ebene zurückgegeben.
| 5. | Die API der mittleren Ebene verarbeitet die Informationen und sendet relevante Informationen an Azure AD B2C zurück.
| 6. | Azure AD B2C empfängt die Informationen von der API der mittleren Ebene. Bei einer Fehlerantwort wird dem Benutzer eine Fehlermeldung angezeigt. Bei einer Erfolgsantwort wird der Benutzer authentifiziert und in das Verzeichnis geschrieben.

## <a name="set-up-the-solution"></a>Einrichten der Lösung

1. [Erstellen Sie eine Facebook-Anwendung](https://docs.microsoft.com/azure/active-directory-b2c/identity-provider-facebook#create-a-facebook-application), die so konfiguriert ist, dass sie den Verbund mit Azure AD B2C zulässt.
2. [Fügen Sie das von Ihnen erstellte Facebook-Geheimnis](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started#create-the-facebook-key) als Identity Experience Framework-Richtlinienschlüssel hinzu.

## <a name="configure-your-application-under-microsoft-dfp"></a>Konfigurieren Ihrer Anwendung unter Microsoft DFP

[Richten Sie Ihren Azure AD-Mandanten](https://docs.microsoft.com/dynamics365/fraud-protection/integrate-real-time-api) für die Verwendung von Microsoft DFP ein.

## <a name="deploy-to-the-web-application"></a>Bereitstellen für die Webanwendung

### <a name="implement-microsoft-dfp-service-fingerprinting"></a>Implementieren des Microsoft DFP-Fingerabdruckdiensts

Der [Microsoft DFP-Gerätefingerabdruck](https://docs.microsoft.com/dynamics365/fraud-protection/device-fingerprinting) ist eine Voraussetzung für den Microsoft DFP-Kontoschutz.

>[!NOTE]
>Zusätzlich zur Verwendung der Seiten der Azure AD B2C-Benutzeroberfläche können Benutzer den Fingerabdruckdienst auch innerhalb des App-Codes implementieren, um eine umfassendere Geräteprofilerstellung zu ermöglichen. Die Implementierung des Fingerabdruckdiensts im App-Code ist nicht Bestandteil dieses Beispiels.

### <a name="deploy-the-azure-ad-b2c-api-code"></a>Bereitstellen des Azure AD B2C-API-Codes

Stellen Sie den [bereitgestellten API-Code](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/Dynamics-Fraud-Protection/API) für einen Azure-Dienst bereit. Der Code kann [mithilfe von Visual Studio veröffentlicht](https://docs.microsoft.com/visualstudio/deployment/quickstart-deploy-to-azure?view=vs-2019) werden.

Richten Sie CORS ein, und fügen Sie den **zulässigen Ursprung** `https://{your_tenant_name}.b2clogin.com` hinzu.

>[!NOTE]
>Sie benötigen die URL des bereitgestellten Diensts später, um Azure AD mit den erforderlichen Einstellungen zu konfigurieren.

Weitere Informationen finden Sie in der [App Service-Dokumentation](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api).

### <a name="add-context-dependent-configuration-settings"></a>Hinzufügen kontextabhängiger Konfigurationseinstellungen

Konfigurieren Sie die Anwendungseinstellungen in [Azure App Service](https://docs.microsoft.com/azure/app-service/configure-common#configure-app-settings). Auf diese Weise können Einstellungen sicher konfiguriert werden, ohne sie in ein Repository einchecken zu müssen. Für die REST-API müssen die folgenden Einstellungen angegeben werden:

| Anwendungseinstellungen | `Source` | Notizen |
| :-------- | :------------| :-----------|
|FraudProtectionSettings:InstanceId | Microsoft DFP-Konfiguration |     |
|FraudProtectionSettings:DeviceFingerprintingCustomerId | Ihre Kunden-ID für den Microsoft-Gerätefingerabdruck |     |
| FraudProtectionSettings:ApiBaseUrl |  Ihre Basis-URL aus dem Microsoft DFP-Portal   | Entfernen Sie „-int“, um stattdessen die Produktions-API aufzurufen.
|  TokenProviderConfig: Resource | https://api.dfp.dynamics-int.com |   Entfernen Sie „-int“, um stattdessen die Produktions-API aufzurufen.  |
|   TokenProviderConfig:ClientId       |Die Azure AD-Client-App-ID des Fraud Protection-Händlers      |       |
| TokenProviderConfig:Authority | https://login.microsoftonline.com/<directory_ID> | Die Azure AD-Mandantenautorität des Fraud Protection-Händlers |
| TokenProviderConfig:CertificateThumbprint* | Der Fingerabdruck des Zertifikats, das für die Authentifizierung bei der Azure AD-Client-App des Händlers verwendet werden soll |
| TokenProviderConfig:ClientSecret* | Das Geheimnis für die Azure AD-Client-App des Händlers | Es wird empfohlen, einen Geheimnis-Manager zu verwenden. |

*Legen Sie nur einen der beiden gekennzeichneten Parameter fest, je nachdem, ob Sie sich mit einem Zertifikat oder einem Geheimnis wie einem Kennwort authentifizieren.

## <a name="azure-ad-b2c-configuration"></a>Azure AD B2C-Konfiguration

### <a name="replace-the-configuration-values"></a>Ersetzen der Konfigurationswerte

Suchen Sie in den bereitgestellten [benutzerdefinierten Richtlinien](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/Dynamics-Fraud-Protection/Policies) die folgenden Platzhalter, und ersetzen Sie diese durch die entsprechenden Werte aus Ihrer Instanz.

| Platzhalter | Ersetzen durch | Notizen |
| :-------- | :------------| :-----------|
|{your_tenant_name} | Kurzname Ihres Mandanten |  „yourtenant“ aus yourtenant.onmicrosoft.com   |
|{your_tenantId} | Mandanten-ID Ihres Azure AD B2C-Mandanten |  01234567-89ab-cdef-0123-456789abcdef   |
|  {your_tenant_IdentityExperienceFramework_appid}    |   App-ID der IdentityExperienceFramework-App, die in Ihrem Azure AD B2C-Mandanten konfiguriert ist    |  01234567-89ab-cdef-0123-456789abcdef   |
|  {your_tenant_ ProxyIdentityExperienceFramework _appid}     |  App-ID der ProxyIdentityExperienceFramework-App, die in Ihrem Azure AD B2C-Mandanten konfiguriert ist      |   01234567-89ab-cdef-0123-456789abcdef     |
|  {your_tenant_extensions_appid}   |  App-ID der Speicheranwendung Ihres Mandanten   |  01234567-89ab-cdef-0123-456789abcdef  |
|   {your_tenant_extensions_app_objectid}  | Objekt-ID der Speicheranwendung Ihres Mandanten    | 01234567-89ab-cdef-0123-456789abcdef   |
|   {your_app_insights_instrumentation_key}  |   Instrumentierungsschlüssel Ihrer App Insights-Instanz*  |   01234567-89ab-cdef-0123-456789abcdef |
|  {your_ui_base_url}   | Endpunkt in Ihrem App-Dienst, von dem aus Ihre UI-Dateien bedient werden    | https://yourapp.azurewebsites.net/B2CUI/GetUIPage   |
|   {your_app_service_url}  | URL Ihres App-Diensts    |  https://yourapp.azurewebsites.net  |
|   {your-facebook-app-id}  |  App-ID der Facebook-App, die Sie für den Verbund mit Azure AD B2C konfiguriert haben   | 000000000000000   |
|  {your-facebook-app-secret}   |  Name des Richtlinienschlüssels, unter dem Sie das Geheimnis der Facebook-App gespeichert haben   | B2C_1A_FacebookAppSecret   |

\* App Insights kann sich in einem anderen Mandanten befinden. Dieser Schritt ist optional. Entfernen Sie ggf. die entsprechenden TechnicalProfiles und OrchestrationSteps.

### <a name="call-microsoft-dfp-label-api"></a>Aufrufen der Microsoft DFP-Label-API

Kunden müssen [die Label-API implementieren](https://docs.microsoft.com/dynamics365/fraud-protection/integrate-ap-api). Weitere Informationen finden Sie unter [Microsoft DFP-API](https://apidocs.microsoft.com/services/dynamics365fraudprotection#/AccountProtection/v1.0).

`URI: < API Endpoint >/v1.0/label/account/create/<userId>`

Der Wert der Benutzer-ID (userId) muss mit dem entsprechenden Wert in der Azure AD B2C-Konfiguration (ObjectID) übereinstimmen.

>[!NOTE]
>Fügen Sie der Seite für die Attributsammlung eine Einwilligungsbenachrichtigung hinzu. Informieren Sie darüber, dass die Telemetrie- und Benutzeridentitätsinformationen der Benutzer zu Kontoschutzzwecken aufgezeichnet werden.

## <a name="configure-the-azure-ad-b2c-policy"></a>Konfigurieren der Azure AD B2C-Richtlinie

1. Wechseln Sie im Richtlinienordner zur [Azure AD B2C-Richtlinie](https://github.com/azure-ad-b2c/partner-integrations/tree/master/samples/Dynamics-Fraud-Protection/Policies).

2. Folgen Sie den Anweisungen in diesem [Dokument](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started?tabs=applications#custom-policy-starter-pack), um das [LocalAccounts Starter Pack](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/LocalAccounts) herunterzuladen.

3. Konfigurieren Sie die Richtlinie für den Azure AD B2C-Mandanten.

>[!NOTE]
>Aktualisieren Sie die bereitgestellten Richtlinien so, dass sie sich auf Ihren spezifischen Mandanten beziehen.

## <a name="test-the-user-flow"></a>Testen des Benutzerflows

1. Öffnen Sie den Azure AD B2C-Mandanten, und wählen Sie unter „Richtlinien“ die Option **Identity Experience Framework** aus.

2. Wählen Sie den zuvor erstellten Flow **SignUpSignIn** aus.

3. Wählen Sie **Benutzerflow ausführen** und dann die Einstellungen aus:

   a. **Anwendung:** Wählen Sie die registrierte App aus (Beispiel: JWT).

   b. **Antwort-URL:** Wählen Sie die **Umleitungs-URL** aus.

   c. Wählen Sie **Benutzerflow ausführen** aus.

4. Führen Sie den Registrierungsflow aus, und erstellen Sie ein Konto.

5. Der Microsoft DFP-Dienst wird während des Flows aufgerufen, nachdem das Benutzerattribut erstellt wurde. Wenn der Flow unvollständig ist, stellen Sie sicher, dass der Benutzer nicht im Verzeichnis gespeichert ist.

>[!NOTE]
>Aktualisieren Sie Regeln direkt im Microsoft DFP-Portal, wenn Sie das [Microsoft DFP-Regelmodul](https://docs.microsoft.com/dynamics365/fraud-protection/rules) verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Microsoft DFP-Beispiele](https://github.com/Microsoft/Dynamics-365-Fraud-Protection-Samples)

- [Benutzerdefinierte Richtlinien in Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-overview)

- [Erste Schritte mit benutzerdefinierten Richtlinien in Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started?tabs=applications)
