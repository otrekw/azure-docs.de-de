---
title: Erstellen einer Azure AD-.NET-Web-API zur Authentifizierung und Autorisierung | Microsoft-Dokumentation
description: In diesem Thema erfahren Sie, wie eine .NET-MVC-Web-API erstellt wird, die sich für die Authentifizierung und Autorisierung in Azure AD integriert.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/17/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: abc4c05aab6efcfc55d5e4dc812ab41a3e3fc8aa
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74963272"
---
# <a name="quickstart-build-a-net-web-api-that-integrates-with-azure-ad-for-authentication-and-authorization"></a>Schnellstart: Erstellen einer.NET-Web-API, die zur Authentifizierung und Autorisierung in Azure AD integriert wird

[Microsoft Identity Platform](v2-overview.md) ist eine Weiterentwicklung der Azure AD-Entwicklerplattform (Azure Active Directory). Sie ermöglicht Entwicklern das Erstellen von Anwendungen, mit denen alle Microsoft-Identitäten angemeldet werden, sowie das Abrufen von Token zum Aufrufen von Microsoft-APIs, etwa Microsoft Graph oder von Entwicklern erstellte APIs.

Mithilfe der [Microsoft-Authentifizierungsbibliothek (MSAL)](msal-overview.md) können Entwickler Token vom Microsoft Identity Platform-Endpunkt abrufen, um auf geschützte Web-APIs zuzugreifen. Die Active Directory-Authentifizierungsbibliothek (ADAL) arbeitet mit dem Azure AD für Entwickler (v1.0)-Endpunkt zusammen, während MSAL und der Microsoft Identity Platform (v2.0)-Endpunkt integriert sind.

Für neue Web-APIs wird empfohlen, Microsoft Identity Platform (v2.0) und MSAL zu verwenden, um Token zu erhalten und auf sichere Web-APIs zuzugreifen: [Schnellstart: Hinzufügen von „Mit Microsoft anmelden“ zu einer ASP.NET-Web-App](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2#calling-an-aspnet-core-web-api-from-a-wpf-application-using-azure-ad-v2)
