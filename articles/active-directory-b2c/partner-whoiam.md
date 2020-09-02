---
title: Tutorial zum Konfigurieren von Azure Active Directory B2C mit WhoIAM
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie die Azure AD B2C-Authentifizierung mit WhoIAM für die Benutzerüberprüfung integrieren.
services: active-directory-b2c
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/20/2020
ms.author: gasinh
ms.subservice: B2C
ms.openlocfilehash: 6545c5f40004dc50904618a8ea734eb73eeee933
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88817129"
---
# <a name="tutorial-for-configuring-whoiam-with-azure-active-directory-b2c"></a>Tutorial zum Konfigurieren von WhoIAM mit Azure Active Directory B2C

In diesem Beispieltutorial erhalten Sie Anleitungen dazu, wie Sie BRIMS (Branded Identity Management System) von [WhoIAM](https://www.whoiam.ai/brims/) in Ihrer Umgebung konfigurieren und in Azure AD B2C integrieren.

Bei BRIMS von WhoIAM handelt es sich um eine Reihe von Apps und Diensten, die in Ihrer Umgebung bereitgestellt werden. Es ermöglicht eine Sprach-, SMS- und E-Mail-Überprüfung Ihrer Benutzerbasis. BRIMS erbeitet in Verbindung mit Ihrer vorhandenen Identitäts- und Zugriffsverwaltungslösung und ist plattformunabhängig.

## <a name="prerequisites"></a>Voraussetzungen

Zunächst benötigen Sie Folgendes:

- Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.

- [Einen Azure AD B2C-Mandanten](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant), der mit Ihrem Azure-Abonnement verknüpft ist.

- Ein WhoIAM-[Testkonto](https://www.whoiam.ai/contact-us/).

## <a name="scenario-description"></a>Beschreibung des Szenarios

Die WhoIAM-Integration umfasst die folgenden Komponenten:

- Azure AD B2C-Mandant: Autorisierungsserver, der für die Überprüfung der Anmeldeinformationen des Benutzers basierend auf den benutzerdefinierten Richtlinien im Mandanten zuständig ist. Er wird auch als Identitätsanbieter bezeichnet.

- Ein Verwaltungsportal zum Verwalten von Clients und deren Konfigurationen.

- Einen API-Dienst, der verschiedene Funktionen über Endpunkte verfügbar macht.  

- Azure Cosmos DB, das als Back-End für das BRIMS-Verwaltungsportal und den API-Dienst verwendet wird.

Im folgenden Architekturdiagramm ist die Implementierung dargestellt.

![Screenshot für das WhoIAM-Architekturdiagramm](media/partner-whoiam/whoiam-architecture-diagram.png)

|Schritt | BESCHREIBUNG |
|:-----| :-----------|
| 1. | Der Benutzer gelangt auf eine Anmeldeseite. Der Benutzer startet eine Registrierungs- oder Anmeldeanforderung für eine App, die Azure AD B2C als Identitätsanbieter verwendet.
| 2. | Im Rahmen der Authentifizierung fordert der Benutzer entweder eine Überprüfung des Besitzes seiner E-Mail-Adresse oder seines Telefons oder die Verwendung seiner Stimme als biometrischen Überprüfungsfaktor an.  
| 3. | Azure AD B2C ruft den BRIMS-API-Dienst auf und übergibt die E-Mail-Adresse, Telefonnummer und Sprachaufzeichnung des Benutzers.
| 4. | BRIMS verwendet vordefinierte Konfigurationen, wie z. B. vollständig anpassbare E-Mail- und SMS-Vorlagen, um mit dem Benutzer in Übereinstimmung mit dem Erscheinungsbild der App in der jeweiligen Sprache zu interagieren.
| 5. | Sobald die Identitätsüberprüfung eines Benutzers abgeschlossen ist, gibt BRIMS ein Token mit dem Ergebnis der Überprüfung an Azure AD B2C zurück. Azure AD B2C gewährt dem Benutzer dann entweder Zugriff auf die App oder lässt den Authentifizierungsversuch scheitern.  

## <a name="onboard-with-whoiam"></a>Durchführen des Onboardings mit WhoIAM

1. Nehmen Sie Kontakt mit [WhoIAM](https://www.whoiam.ai/contact-us/) auf, und erstellen Sie ein BRIMS-Konto.

2. Nachdem ein Konto erstellt wurde, verwenden Sie die bereitgestellten Anweisungen für das Onboarding, und konfigurieren Sie die folgenden Azure-Dienste:

    - [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) zur sicheren Speicherung von Kennwörtern, z. B Kennwörtern des E-Mail-Diensts

    - [Azure App Service](https://azure.microsoft.com/services/app-service/) zum Hosten der BRIMS-API- und Verwaltungsportaldienste

    - [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) zum Authentifizieren von Administratoren für das Verwaltungsportal

    - [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) zum Speichern und Abrufen von Einstellungen

    - [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview#:~:text=Application%20Insights%2C%20a%20feature%20of%20Azure%20Monitor%2C%20is,professionals.%20Use%20it%20to%20monitor%20your%20live%20applications.) (optional) zum Anmelden bei API und Verwaltungsportal

3. Stellen Sie die BRIMS-API und das BRIMS-Verwaltungsportal in Ihrer Azure-Umgebung bereit.

4. Beispiele benutzerdefinierter Richtlinien für Azure AD B2C stehen in der Dokumentation zum BRIMS-Onboarding bereit. Folgen Sie den Anweisungen im Dokument, um Ihre App zu konfigurieren und die BRIMS-Plattform zur Überprüfung der Benutzeridentität zu nutzen.  

Weitere Informationen zu BRIMS von WhoIAM finden Sie in der [Produktdokumentation](https://www.whoiam.ai/brims/).

## <a name="test-the-user-flow"></a>Testen des Benutzerflows

1. Öffnen Sie den Azure AD B2C-Mandanten, und wählen Sie unter „Richtlinien“ die Option **Identity Experience Framework** aus.

2. Wählen Sie den zuvor erstellten Flow **SignUpSignIn** aus.

3. Wählen Sie **Benutzerflow ausführen** und dann die Einstellungen aus:

   a. **Anwendung:** Wählen Sie die registrierte App aus (Beispiel: JWT).

   b. **Antwort-URL:** Wählen Sie die **Umleitungs-URL** aus.

   c. Wählen Sie **Benutzerflow ausführen** aus.

4. Führen Sie den Registrierungsflow aus, und erstellen Sie ein Konto.

5. Der BRIMS-Dienst wird während des Flows aufgerufen, nachdem das Benutzerattribut erstellt wurde. Wenn der Flow unvollständig ist, stellen Sie sicher, dass der Benutzer nicht im Verzeichnis gespeichert ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Benutzerdefinierte Richtlinien in Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-overview)

- [Erste Schritte mit benutzerdefinierten Richtlinien in Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started?tabs=applications)
