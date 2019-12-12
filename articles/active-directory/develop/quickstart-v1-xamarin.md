---
title: Erste Schritte in Azure AD Xamarin | Microsoft-Dokumentation
description: Erstellen Sie Xamarin-Anwendungen, die für die Anmeldung in Azure AD integriert sind und über OAuth durch Azure AD geschützte APIs aufrufen.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/17/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6197a27b9a94de0f9a10407215e5755689cd0b56
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2019
ms.locfileid: "74920819"
---
# <a name="quickstart-build-a-xamarin-app-that-integrates-microsoft-sign-in"></a>Schnellstart: Erstellen einer Xamarin-App, die die Microsoft-Anmeldung integriert

[Microsoft Identity Platform](v2-overview.md) ist eine Weiterentwicklung der Azure AD-Entwicklerplattform (Azure Active Directory). Sie ermöglicht Entwicklern das Erstellen von Anwendungen, mit denen alle Microsoft-Identitäten angemeldet werden, sowie das Abrufen von Token zum Aufrufen von Microsoft-APIs, etwa Microsoft Graph oder von Entwicklern erstellte APIs.

Mithilfe der [Microsoft-Authentifizierungsbibliothek (MSAL)](msal-overview.md) können Entwickler Token vom Microsoft Identity Platform-Endpunkt abrufen, um auf geschützte Web-APIs zuzugreifen. Die Active Directory-Authentifizierungsbibliothek (ADAL) arbeitet mit dem Azure AD für Entwickler (v1.0)-Endpunkt zusammen, während MSAL und der Microsoft Identity Platform (v2.0)-Endpunkt integriert sind.

## <a name="next-steps"></a>Nächste Schritte

Für neue Xamarin-Anwendungen wird empfohlen, Microsoft Identity Platform (v2.0) und MSAL zu verwenden, um Token zu erhalten und auf sichere Web-APIs zuzugreifen. Informationen zu den ersten Schritten finden Sie unter [Integrieren der Microsoft-Identität und von Microsoft Graph in eine Xamarin-Formularanwendung mit MSAL](https://github.com/azure-samples/active-directory-xamarin-native-v2#integrate-microsoft-identity-and-the-microsoft-graph-into-a-xamarin-forms-app-using-msal) (ohne die optionalen Schritte).
