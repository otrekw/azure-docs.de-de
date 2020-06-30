---
title: 'Codebeispiele für API-Connectors für Benutzerflows: Azure AD'
description: Hier finden Sie Codebeispiele für API-Connectors in Flows zur Self-Service-Registrierung für externe Azure Active Directory-Identitäten.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: sample
ms.date: 06/16/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d0814312bb12582dd9e9ebfafc60fba470f6a9a9
ms.sourcegitcommit: 34eb5e4d303800d3b31b00b361523ccd9eeff0ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2020
ms.locfileid: "84905156"
---
# <a name="samples-for-external-identities-self-service-sign-up"></a>Beispiele für die Self-Service-Registrierung externer Identitäten

Die folgenden Tabellen enthalten Links zu Codebeispielen für die Nutzung von Web-APIs in den Benutzerflows zur Self-Service-Registrierung mithilfe von [API-Connectors](api-connectors-overview.md).

## <a name="api-connector-azure-function-quickstarts"></a>Schnellstartanleitungen zu Azure-Funktionen für API-Connectors

| Beispiel                                                                                                                          | BESCHREIBUNG                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [.NET Core](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-api-connector-azure-function-validate) | Dieses .NET Core-Beispiel für eine Azure-Funktion veranschaulicht, wie Sie Registrierungen auf bestimmte Mandantendomänen beschränken und vom Benutzer bereitgestellte Informationen überprüfen. |
| [Node.js](https://github.com/Azure-Samples/active-directory-nodejs-external-identities-api-connector-azure-function-validate)   | Dieses Node.js-Beispiel für eine Azure-Funktion veranschaulicht, wie Sie Registrierungen auf bestimmte Mandantendomänen beschränken und vom Benutzer bereitgestellte Informationen überprüfen.  |
| [Python](https://github.com/Azure-Samples/active-directory-python-external-identities-api-connector-azure-function-validate)    | Dieses Python-Beispiel für eine Azure-Funktion veranschaulicht, wie Sie Registrierungen auf bestimmte Mandantendomänen beschränken und vom Benutzer bereitgestellte Informationen überprüfen.    |

<!-- \| [Java](../../azure-docs-pr/articles/active-directory/b2b/invite-internal-users.md#use-the-invitation-api-to-send-a-b2b-invitation) |  The sample below illustrates how to call the invitation API to invite an internal user as a B2B user. | -->

## <a name="custom-approval-workflows"></a>Benutzerdefinierte Genehmigungsworkflows

| Beispiel | BESCHREIBUNG |
|--------| ----------- |
| [Manueller Genehmigungsworkflow](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-api-connectors-approvals) | In diesem Beispiel wird ein End-to-End-Genehmigungsworkflow zum Verwalten der Erstellung von Gastbenutzerkonten bei der Self-Service-Registrierung veranschaulicht. |

## <a name="identity-verification"></a>Identitätsüberprüfung

| Beispiel                                                                                                            | BESCHREIBUNG                                                                                                                          |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| [IDology](https://github.com/Azure-Samples/active-directory-dotnet-external-identities-idology-identity-verification) | Dieses Beispiel zeigt, wie Sie eine Benutzeridentität im Rahmen der Self-Service-Registrierung überprüfen, indem Sie einen API-Connector für die Integration mit IDology verwenden. |

<!-- | [Experian](https://github.com/Azure-Samples/) | This sample shows how add identity verification to your self-service sign-up user flow by using an API connector to integrate with Experian. | -->
