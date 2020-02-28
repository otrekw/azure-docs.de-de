---
title: Authentifizierungsmethoden | Microsoft Azure Maps
description: In diesem Artikel wird die Authentifizierung per Azure Active Directory (Azure AD) und gemeinsam verwendetem Schlüssel beschrieben. Beide Verfahren werden für Microsoft Azure Maps-Dienste verwendet. Lesen Sie, wie Sie einen Azure Maps-Abonnementschlüssel erhalten.
author: farah-alyasari
ms.author: v-faalya
ms.date: 01/28/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 171219c001f43137a52f29b282fb1705b3d836aa
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77649822"
---
# <a name="authentication-with-azure-maps"></a>Authentifizierung mit Azure Maps

Azure Maps unterstützt zwei Möglichkeiten für die Authentifizierung von Anforderungen: Authentifizierung per gemeinsam verwendetem Schlüssel und Azure Active Directory-Authentifizierung. In diesem Artikel werden diese Authentifizierungsmethoden beschrieben, um Ihnen bei Ihrer Implementierung von Azure Maps-Diensten zu helfen.

> [!NOTE]
> Um die sichere Kommunikation mit Azure Maps zu verbessern, unterstützen wir nun TLS 1.2 (Transport Layer Security), und wir stellen die Unterstützung für TLS 1.0 und 1.1 ein. Um Dienstunterbrechungen zu vermeiden, **aktualisieren Sie Ihre Server und Anwendungen auf die Verwendung von TLS 1.2 vor dem 2. April 2020**.  Wenn Sie derzeit TLS 1.x verwenden, evaluieren Sie Ihre TLS 1.2-Bereitschaft, und entwickeln Sie einen Migrationsplan mit den in [Lösen des TLS 1.0-Problems](https://docs.microsoft.com/security/solving-tls1-problem) beschriebenen Tests.

## <a name="shared-key-authentication"></a>Authentifizierung mit gemeinsam verwendetem Schlüssel

 Nach dem Erstellen des Azure Maps-Kontos werden der Primär- und der Sekundärschlüssel generiert. Sie sollten den Primärschlüssel als Abonnementschlüssel verwenden, wenn Sie Azure Maps mithilfe der Authentifizierung mit gemeinsam verwendetem Schlüssel aufrufen. Bei der Authentifizierung mit einem gemeinsam verwendeten Schlüssel wird ein von einem Azure Maps-Konto generierter Schlüssel an einen Azure Maps-Dienst übergeben. Fügen Sie für jede Anforderung, die an Azure Maps-Dienste gesendet wird, der URL den *Abonnementschlüssel* als Parameter hinzu. Der Sekundärschlüssel kann in Szenarien wie Änderungen beim Schlüsselrollover verwendet werden.  

Informationen zum Anzeigen der Schlüssel im Azure-Portal finden Sie unter [Verwalten der Authentifizierung in Azure Maps](https://aka.ms/amauthdetails).

> [!Tip]
> Wir empfehlen Ihnen, Ihre Schlüssel regelmäßig neu zu generieren. Es werden zwei Schlüssel bereitgestellt, damit Sie die Verbindungen mit einem Schlüssel aufrechterhalten können, während Sie den anderen neu generieren. Bei der erneuten Generierung Ihrer Schlüssel müssen Sie alle Anwendungen, die auf Ihr Konto zugreifen, mit den neuen Schlüsseln aktualisieren.

## <a name="authentication-with-azure-active-directory-preview"></a>Authentifizierung mit Azure Active Directory (Vorschauversion)

Azure Maps verfügt jetzt über die Anforderungsauthentifizierung für Azure Maps-Dienste mit [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis). Azure AD ermöglicht die identitätsbasierte Authentifizierung, einschließlich der [rollenbasierten Zugriffssteuerung (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview). RBAC wird genutzt, um den Zugriff auf Azure Maps-Ressourcen auf Benutzerebene, Gruppenebene oder Anwendungsebene zu gewähren. In den nächsten Abschnitten werden die Konzepte und Komponenten der Azure Maps-Integration in Azure AD diskutiert.

## <a name="authentication-with-oauth-access-tokens"></a>Authentifizierung mit OAuth-Zugriffstoken

Azure Maps akzeptiert **OAuth 2.0**-Zugriffstoken für Azure AD-Mandanten, die einem Azure-Abonnement zugeordnet sind, in dem ein Azure Maps-Konto enthalten ist. Azure Maps akzeptiert auch Token für:

* Azure AD-Benutzer
* Partneranwendungen, für die von Benutzern delegierte Berechtigungen verwendet werden
* Verwaltete Identitäten für Azure-Ressourcen

Azure Maps generiert für jedes Azure Maps-Konto einen *eindeutigen Bezeichner (Client-ID)* . Sie können Token von Azure AD anfordern, wenn Sie diese Client-ID mit zusätzlichen Parametern kombinieren. Zum Anfordern eines Tokens geben Sie die Werte in der folgenden Tabelle an, basierend auf Ihrer Azure-Umgebung.

| Azure-Umgebung   | Azure AD-Tokenendpunkt |
| --------------------|-------------------------|
| Azure – Öffentlich        | https://login.microsoftonline.com |
| Azure Government    | https://login.microsoftonline.us |


Weitere Informationen zum Konfigurieren von Azure AD und Anforderungstoken für Azure Maps finden Sie unter [Verwalten der Authentifizierung in Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

Allgemeine Informationen zum Anfordern von Token aus Azure AD finden Sie unter [Was ist Authentifizierung?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).

## <a name="request-azure-map-resources-with-oauth-tokens"></a>Anfordern von Azure Maps-Ressourcen mit OAuth-Token

Nachdem Azure AD ein Token empfangen hat, sendet Azure Maps eine Anforderung mit dem folgenden Satz erforderlicher Anforderungsheader:

| Anforderungsheader    |    value    |
|:------------------|:------------|
| x-ms-client-id    | 30d7cc….9f55|
| Authorization     | Bearer eyJ0e….HNIVN |

> [!Note]
> `x-ms-client-id` ist die auf dem Azure Maps-Konto basierende GUID, die auf der Azure Maps-Authentifizierungsseite angezeigt wird.

Hier ist ein Beispiel für eine Azure Maps-Routenanforderung angegeben, für die ein OAuth-Token verwendet wird:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

Weitere Informationen zum Anzeigen Ihrer Client-ID finden Sie unter [Anzeigen von Authentifizierungsdetails](https://aka.ms/amauthdetails).

## <a name="control-access-with-rbac"></a>Steuern des Zugriffs mit RBAC

Verwenden Sie RBAC in Azure AD, um den Zugriff auf geschützte Ressourcen zu steuern. Richten Sie Ihr Azure Maps-Konto ein, und registrieren Sie Ihren Azure AD-Mandanten für Azure Maps. Azure Maps unterstützt die Steuerung des Lesezugriffs für einzelne Azure AD-Benutzer, -Gruppen und -Anwendungen, Azure-Ressourcen sowie Azure-Dienste über verwaltete Identitäten für Azure-Ressourcen. Auf der Azure Maps-Portalseite können Sie RBAC für Ihre ausgewählten Rollen einrichten.

![Azure Maps-Datenleser (Vorschauversion)](./media/azure-maps-authentication/concept.png)

Informationen zum Anzeigen Ihrer RBAC-Einstellungen finden Sie im Artikel zum Thema [Konfigurieren von RBAC für Azure Maps](https://aka.ms/amrbac).

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Verwaltete Identitäten für Azure-Ressourcen und Azure Maps

Von [verwalteten Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) wird für Azure-Dienste eine automatisch verwaltete Identität bereitgestellt, die für den Zugriff auf Azure Maps-Dienste autorisiert werden kann. Beispiele für verwaltete Identitäten sind: Azure App Service, Azure Functions und Azure Virtual Machines.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Authentifizierung einer Anwendung mit Azure AD und Azure Maps finden Sie im Artikel zum Thema [Verwalten der Authentifizierung in Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

* Weitere Informationen zur Authentifizierung von Azure Maps, des Kartensteuerelements und von Azure AD finden Sie unter [Verwenden des Azure Maps-Kartensteuerelements](https://aka.ms/amaadmc).
