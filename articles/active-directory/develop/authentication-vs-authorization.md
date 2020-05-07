---
title: Authentifizierung im Vergleich zu Autorisierung | Azure
titleSuffix: Microsoft identity platform
description: Lernen Sie die Grundlagen von Authentifizierung und Autorisierung in Microsoft Identity Platform (v2.0) kennen.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/28/2020
ms.author: ryanwi
ms.reviewer: jmprieur, saeeda, sureshja, hirsin
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: 61c0202d12756201d45fe829078d84382e44b54e
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82584162"
---
# <a name="authentication-vs-authorization"></a>Authentifizierung im Vergleich zu Autorisierung

In diesem Artikel werden Authentifizierung und Autorisierung definiert und kurz erläutert, wie Sie Microsoft Identity Platform zum Authentifizieren und Autorisieren von Benutzern in Ihren Web-Apps, Web-APIs oder Apps, die geschützte Web-APIs aufrufen, verwenden können. Wenn Sie auf einen Begriff stoßen, mit dem Sie nicht vertraut sind, werfen Sie einen Blick in unser [Glossar](developer-glossary.md), oder sehen Sie sich unsere [Videos zu Microsoft Identity Platform](identity-videos.md) an, in denen grundlegende Konzepte behandelt werden.

## <a name="authentication"></a>Authentifizierung

Bei der **Authentifizierung** weisen Sie nach, dass Sie die Person sind, als die Sie sich ausgeben. Authentifizierung wird manchmal verkürzt als AuthN bezeichnet. Microsoft Identity Platform implementiert das [OpenID Connect](https://openid.net/connect/)-Protokoll für die Verarbeitung der Authentifizierung.

## <a name="authorization"></a>Authorization

Die **Autorisierung** ist die Gewährung einer Handlungsberechtigung für eine authentifizierte Person. Sie gibt an, auf welche Daten Sie zugreifen dürfen und welche Aktionen Sie damit ausführen können. Autorisierung wird manchmal verkürzt als AuthZ bezeichnet. Microsoft Identity Platform implementiert das [OAuth 2.0](https://oauth.net/2/)-Protokoll für die Verarbeitung der Autorisierung.

## <a name="authentication-and-authorization-using-the-microsoft-identity-platform"></a>Authentifizierung und Autorisierung mit Microsoft Identity Platform

Statt Apps zu erstellen, die jeweils über eigene Informationen in Bezug auf Benutzername und Kennwort verfügen, was mit einem hohen Verwaltungsaufwand verbunden ist, wenn Sie für mehrere Apps Benutzer hinzufügen oder entfernen müssen, besteht die Möglichkeit, dass Apps diese Aufgabe an einen zentralen Identitätsanbieter delegieren können.

Azure Active Directory (Azure AD) ist ein zentraler Identitätsanbieter in der Cloud. Die Delegierung der Authentifizierung und Autorisierung an den Anbieter ermöglicht beispielsweise die Verwendung von Richtlinien für bedingten Zugriff, die erfordern, dass sich ein Benutzer an einem bestimmten Ort befindet, die Verwendung der mehrstufigen Authentifizierung sowie die Schaffung der Möglichkeit, dass sich Benutzer nur einmal anmelden müssen und dann automatisch bei allen Web-Apps angemeldet werden, die dasselbe zentralisierte Verzeichnis nutzen. Diese Funktion wird als **einmaliges Anmelden** (Single Sign-On, SSO) bezeichnet.

Microsoft Identity Platform vereinfacht die Authentifizierung und Autorisierung für Anwendungsentwickler. Die Plattform verfügt über Identity-as-a-Service mit Unterstützung für branchenübliche Protokolle wie OAuth 2.0 und OpenID Connect sowie Open-Source-Bibliotheken für verschiedene Plattformen, damit Sie schnell mit der Programmierung beginnen können. Sie ermöglicht Entwicklern nicht nur das Erstellen von Anwendungen, mit denen alle Microsoft-Identitäten angemeldet werden, sondern auch das Abrufen von Token zum Aufrufen von [Microsoft Graph](https://developer.microsoft.com/graph/), anderen Microsoft-APIs oder von Entwicklern erstellten APIs. Weitere Informationen finden Sie unter [Entwicklung der Microsoft Identity Platform](about-microsoft-identity-platform.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Themen zu den Grundlagen von Authentifizierung und Autorisierung:

* Unter [Sicherheitstoken](security-tokens.md) erfahren Sie, wie Zugriffstoken, Aktualisierungstoken und ID-Token bei Authentifizierung und Autorisierung verwendet werden.
* Unter [Anwendungsmodell](application-model.md) erhalten Sie Informationen zum Registrierungsvorgang für Ihre Anwendung, damit diese in Microsoft Identity Platform integriert werden kann.
* Unter [App-Anmeldeflow](app-sign-in-flow.md) lernen Sie den Anmeldeflow von Web-, Desktop- und mobilen Apps in Microsoft Identity Platform kennen.

Weitere Informationen zu den von Microsoft Identity Platform implementierten Protokollen:

* Unter [OAuth 2.0 und OpenID Connect-Protokolle in Microsoft Identity Platform](active-directory-v2-protocols.md) finden Sie weitere Informationen zum OpenID Connect- und OAuth 2.0-Standard.
* Unter [SAML-Protokoll für einmaliges Anmelden](single-sign-on-saml-protocol.md) erfahren Sie, wie Microsoft Identity Platform das einmalige Anmelden unterstützt.
