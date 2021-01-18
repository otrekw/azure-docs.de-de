---
title: 'Schnellstart: Erstellen eines Azure Active Directory-Mandanten'
titleSuffix: Microsoft identity platform
description: In diesem Schnellstart erfahren Sie, wie Sie einen Azure Active Directory-Mandanten für die Entwicklung von Anwendungen erstellen, die Microsoft Identity Platform zur Authentifizierung und Autorisierung verwenden.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: quickstart
ms.date: 03/12/2020
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev, identityplatformtop40, fasttrack-edit
ms.openlocfilehash: 869b37aea823cf91dc59211b23fcaccd7646afb9
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98012018"
---
# <a name="quickstart-set-up-a-tenant"></a>Schnellstart: Einrichten eines Mandanten

Die Microsoft Identity Platform ermöglicht es Entwicklern, Anwendungen für eine Vielzahl von benutzerdefinierten Microsoft 365-Umgebungen und -Identitäten zu erstellen. Um mit der Verwendung von Microsoft Identity Platform zu beginnen, müssen Sie auf eine Umgebung zugreifen, die auch als Azure AD-Mandant bezeichnet wird und die Apps registrieren und verwalten kann, Zugriff auf Microsoft 365-Daten besitzt und benutzerdefinierten bedingten Zugriff und Mandanteneinschränkungen bereitstellt.

Ein Mandant ist eine Darstellung einer Organisation. Es handelt sich um eine dedizierte Instanz von Azure AD, die ein Unternehmen oder ein App-Entwickler erhält, wenn das Unternehmen oder der App-Entwickler eine Beziehung zu Microsoft eingeht (z.B. die Registrierung für Azure, Microsoft Intune oder Microsoft 365).

Jeder Azure AD-Mandant ist getrennt und separat von anderen Azure AD-Mandanten und verfügt über eine eigene Darstellung von Geschäfts-, Schul- oder Uniidentitäten, Consumeridentitäten (wenn es sich um einen Azure AD B2C-Mieter handelt) und App-Registrierungen. Eine App-Registrierung in Ihrem Mandanten kann Authentifizierungen von Konten nur in Ihrem Mandanten oder von allen Mandanten zulassen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="determining-environment-type"></a>Bestimmen des Umgebungstyps

Zwei Arten von Umgebungen können erstellt werden. Die Entscheidung, welche Umgebung Sie benötigen, basiert ausschließlich auf den Benutzertypen, die Ihre App authentifiziert.

* Geschäfts-, Schul- oder Unikonten (Azure AD-Konten) oder Microsoft-Konten (z.B. outlook.com und live.com)
* Soziale und lokale Konten (Azure AD B2C)

Der Schnellstart ist in zwei Szenarien unterteilt, je nachdem, welche Art von App Sie erstellen möchten.

## <a name="work-and-school-accounts-or-personal-microsoft-accounts"></a>Geschäfts-, Schul- und Unikonten oder persönliche Microsoft-Konten

### <a name="use-an-existing-tenant"></a>Verwenden eines vorhandenen Mandanten

Viele Entwickler verfügen bereits aufgrund von Diensten oder Abonnements, die mit Azure AD-Mandanten verknüpft sind (beispielsweise Microsoft 365- oder Azure-Abonnements), über Mandanten.

1. Um den Mandanten zu überprüfen, melden Sie sich beim <a href="https://portal.azure.com/" target="_blank">Azure-Portal<span class="docon docon-navigate-external x-hidden-focus"></span></a> mit dem Konto an, das Sie für die Verwaltung Ihrer Anwendung verwenden möchten.
1. Überprüfen Sie die obere rechte Ecke. Falls Sie einen Mandanten besitzen, werden Sie automatisch angemeldet, und der Mandantenname wird direkt unter dem Kontonamen angezeigt.
   * Zeigen Sie rechts oben im Azure-Portal auf Ihren Kontonamen, um Ihren Namen, Ihre E-Mail-Adresse, Ihr Verzeichnis und Ihre Mandanten-ID (GUID) sowie Ihre Domäne anzuzeigen.
   * Falls Ihr Konto mehreren Mandanten zugeordnet ist, können Sie durch Klicken auf Ihren Kontonamen ein Menü öffnen, über das Sie zwischen Mandanten wechseln können. Jeder Mandant besitzt eine eigene Mandanten-ID.

> [!TIP]
> Wenn Sie die Mandanten-ID ermitteln müssen, gehen Sie folgendermaßen vor:
> * Zeigen Sie mit der Maus auf den Namen Ihres Kontos, um das Verzeichnis bzw. die Mandanten-ID abzurufen, oder
> * Wählen Sie im Azure-Portal **Azure Active Directory > Eigenschaften > Mandanten-ID** aus.

Sollte Ihrem Konto kein vorhandener Mandant zugeordnet sein, wird unter Ihrem Kontonamen eine GUID angezeigt. In diesem Fall sind Aktionen wie das Registrieren von Apps erst möglich, nachdem Sie die im nächsten Abschnitt beschriebenen Schritte ausgeführt haben.

### <a name="create-a-new-azure-ad-tenant"></a>Erstellen eines neuen Azure AD-Mandanten

Falls Sie noch nicht über einen Azure AD-Mandanten verfügen oder einen neuen Mandanten für die Entwicklung erstellen möchten, sehen sie sich die [Schnellstartanleitung](../fundamentals/active-directory-access-create-new-tenant.md) an, oder verwenden Sie einfach die [Benutzeroberfläche für die Verzeichniserstellung](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory). Sie müssen die folgende Informationen zum Erstellen Ihres neuen Mandanten angeben:

- **Name der Organisation**
- **Anfangsdomäne**: Diese ist Bestandteil von *.onmicrosoft.com. Sie können die Domäne später weiter anpassen.
- **Land oder Region**

> [!NOTE]
> Verwenden Sie bei der Benennung Ihres Mandanten alphanumerische Zeichen. Sonderzeichen sind nicht zulässig. Der Name darf maximal 256 Zeichen lang sein.

## <a name="social-and-local-accounts"></a>Soziale und lokale Konten

Um mit dem Erstellen von Apps zu beginnen, die sich bei sozialen und lokalen Konten anmelden, müssen Sie einen Azure AD B2C-Mandanten erstellen. Befolgen Sie dazu die Anweisungen unter [Erstellen eines Azure AD B2C-Mandanten](../../active-directory-b2c/tutorial-create-tenant.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Registrieren Sie eine App](quickstart-register-app.md), um diese in Microsoft Identity Platform zu integrieren.
