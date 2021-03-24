---
title: Autorisieren des Zugriffs mit Azure Active Directory
description: Dieser Artikel bietet Informationen zum Autorisieren des Zugriffs auf Azure SignalR Service-Ressourcen mit Azure Active Directory.
author: terencefan
ms.author: tefa
ms.date: 08/03/2020
ms.service: signalr
ms.topic: conceptual
ms.openlocfilehash: 37c2e41b5c81f941245b895ecd144baee3b9db6d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97797396"
---
# <a name="authorize-access-to-azure-signalr-service-resources-using-azure-active-directory"></a>Autorisieren des Zugriffs auf Azure SignalR Service-Ressourcen mit Azure Active Directory
Azure SignalR Service unterstützt die Verwendung von Azure Active Directory (Azure AD), um Anforderungen an Azure SignalR Service-Ressourcen zu autorisieren. Mit Azure AD können Sie die rollenbasierte Zugriffssteuerung (RBAC) zum Gewähren von Berechtigungen für einen Sicherheitsprinzipal verwenden, bei dem es sich um einen Benutzer oder einen Anwendungsdienstprinzipal handeln kann. Weitere Informationen zu Rollen und Rollenzuweisungen finden Sie unter [Grundlegendes zu den verschiedenen Rollen](../role-based-access-control/overview.md).

## <a name="overview"></a>Übersicht
Wenn ein Sicherheitsprinzipal (eine Anwendung) versucht, auf eine Azure SignalR Service-Ressource zuzugreifen, muss die Anforderung autorisiert werden. Mit Azure AD ist der Zugriff auf eine Ressource ein zweistufiger Prozess. 

 1. Zunächst wird die Identität des Sicherheitsprinzipals authentifiziert, und ein OAuth 2.0-Token wird zurückgegeben. Der Ressourcenname zum Anfordern eines Tokens ist `https://signalr.azure.com/`.
 2. Anschließend wird das Token als Teil einer Anforderung an Azure SignalR Service übergeben, um den Zugriff auf die angegebene Ressource zu autorisieren.

Für den Authentifizierungsschritt ist es erforderlich, dass eine Anwendungsanforderung zur Laufzeit ein OAuth 2.0-Zugriffstoken enthält. Wenn Ihr Hubserver in einer Azure-Entität ausgeführt wird, z. B. einem virtuellen Azure-Computer, einer VM-Skalierungsgruppe oder einer Azure Functions-App, kann der Zugriff auf die Ressourcen über eine verwaltete Identität erfolgen. Informationen zum Authentifizieren von Anforderungen, die von einer verwalteten Identität an Azure SignalR Service übermittelt werden, finden Sie unter [Authentifizieren des Zugriffs auf Azure SignalR Service-Ressourcen mit Azure Active Directory und verwalteten Identitäten für Azure Ressourcen](authenticate-managed-identity.md). 

Für den Autorisierungsschritt ist es erforderlich, dem Sicherheitsprinzipal mindestens eine RBAC-Rolle zuzuweisen. Azure SignalR Service stellt RBAC-Rollen bereit, die Berechtigungen für Azure SignalR Service-Ressourcen enthalten. Die möglichen Berechtigungen eines Sicherheitsprinzipals sind durch die Rollen vorgegeben, die dem Prinzipal zugewiesen sind. Weitere Informationen zu RBAC-Rollen finden Sie unter [Integrierte Azure-Rollen für Azure SignalR Service](#azure-built-in-roles-for-azure-signalr-service). 

Ein SignalR-Hubserver, der nicht in einer Azure-Entität ausgeführt wird, kann auch mit Azure AD autorisiert werden. Informationen zum Anfordern eines Zugriffstokens und seiner Verwendung zum Autorisieren von Anforderungen für Azure SignalR Service-Ressourcen finden Sie unter [Authentifizieren des Zugriffs auf Azure SignalR Service mit Azure AD über eine Anwendung](authenticate-application.md). 

## <a name="azure-built-in-roles-for-azure-signalr-service"></a>Integrierte Azure-Rollen für Azure SignalR Service

- SignalR-App-Server
- SignalR Service-Leser
- SignalR Service-Besitzer

## <a name="assign-rbac-roles-for-access-rights"></a>Zuweisen von RBAC-Rollen für Zugriffsrechte
Azure Active Directory (Azure AD) autorisiert Rechte für den Zugriff auf abgesicherte Ressourcen über die [rollenbasierte Zugriffssteuerung (RBAC)](../role-based-access-control/overview.md). Azure SignalR Service definiert einige integrierte Azure-Rollen, die allgemeine Berechtigungen für den Zugriff auf Azure SignalR Service umfassen. Sie können auch benutzerdefinierte Rollen für den Zugriff auf die Ressource definieren.

Wenn einem Azure AD-Sicherheitsprinzipal eine RBAC-Rolle zugewiesen wird, gewährt Azure diesem Sicherheitsprinzipal Zugriff auf diese Ressourcen. Der Zugriff kann sich auf den Bereich des Abonnements, der Ressourcengruppe oder einer beliebigen Azure SignalR Service-Ressource beziehen. Ein Azure AD-Sicherheitsprinzipal kann ein Benutzer, eine Anwendung oder eine [verwaltete Identität für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) sein.

## <a name="built-in-roles-for-azure-signalr-service"></a>Integrierte Rollen für Azure SignalR Service
Azure stellt die folgenden integrierten Azure-Rollen zum Autorisieren des Zugriffs auf Azure SignalR Service-Ressourcen mittels Azure AD und OAuth bereit:

### <a name="signalr-app-server"></a>SignalR-App-Server

Verwenden Sie diese Rolle, um einen temporären Zugriffsschlüssel zum Signieren von Clienttoken abrufen zu können.

### <a name="signalr-serverless-contributor"></a>SignalR Serverless-Mitwirkender

Verwenden Sie diese Rolle, um Clienttoken direkt von Azure SignalR Service abrufen zu können.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden verwandten Artikeln:

- [Authentifizieren einer Anwendung mit Azure AD für den Zugriff auf Azure SignalR Service](authenticate-application.md)
- [Authentifizieren einer verwalteten Identität mit Azure AD für den Zugriff auf Azure SignalR Service](authenticate-managed-identity.md)