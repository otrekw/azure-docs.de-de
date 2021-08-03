---
title: Verwaltete Identitäten
description: Media Services kann mit verwalteten Azure-Identitäten verwendet werden.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.date: 05/17/2021
ms.author: inhenkel
ms.openlocfilehash: 02f832c7fc25003950d6a112d951c07d1332e08a
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110454366"
---
# <a name="managed-identities"></a>Verwaltete Identitäten

Die Verwaltung von Geheimnissen und Anmeldeinformationen für eine sichere Kommunikation zwischen verschiedenen Diensten stellt für Entwickler eine häufige Herausforderung dar. In Azure brauchen Entwickler dank verwalteter Identitäten keine Anmeldeinformationen mehr zu verwalten. Für die Azure-Ressource in Azure AD wird eine Identität bereitgestellt, mit der Azure Active Directory (Azure AD)-Token abgerufen werden.

## <a name="media-services-managed-identity-scenarios"></a>Szenarien mit verwalteter Media Services-Identität

Es gibt drei Szenarien, in denen verwaltete Identitäten mit Media Services verwendet werden können:

- [Gewähren des Zugriffs für ein Media Services-Konto auf Key Vault zum Aktivieren von kundenseitig verwalteten Schlüsseln](security-encrypt-data-managed-identity-cli-tutorial.md)
- [Gewähren des Zugriffs für ein Media Services-Konto auf Speicherkonten, um Media Services das Umgehen von Zugriffssteuerungslisten für Azure Storage-Netzwerke zu gestatten](security-access-storage-managed-identity-cli-tutorial.md)
- Anderen Diensten (z. B. virtuellen Computern oder [Azure Functions](security-function-app-managed-identity-cli-tutorial.md)) den Zugriff auf Media Services gestatten

In den ersten beiden Szenarien wird die verwaltete Identität verwendet, um dem *Media Services*-Konto Zugriff auf andere Dienste zu gewähren.  Im dritten Szenario verfügt der *Dienst* über eine verwaltete Identität, die für den Zugriff auf Media Services verwendet wird.

:::image type="content" source="media/diagrams/managed-identities-scenario-comparison.svg" alt-text="Vergleich des Szenarios für verwaltete Identitäten":::

> [!NOTE]
> Diese Szenarien können kombiniert werden. Sie können verwaltete Identitäten sowohl für das Media Services-Konto (z. B. für den Zugriff auf kundenseitig verwaltete Schlüssel) als auch für die Azure Functions-Ressource für den Zugriff auf das Media Services-Konto erstellen.

## <a name="tutorials-and-how-tos"></a>Tutorials und Vorgehensweisen

Probieren Sie diese Tutorials aus, um praktische Erfahrungen mit der Verwendung einer verwalteten Identität mit Media Services zu sammeln.

- [CLI: Verschlüsseln von Daten in einem Media Services-Konto mithilfe eines Key Vault-Schlüssels](security-encrypt-data-managed-identity-cli-tutorial.md)
- [CLI: Gestatten des Zugriffs für Media Services auf ein Speicherkonto, das für das Blockieren von Anforderungen von unbekannten IP-Adressen konfiguriert ist](security-access-storage-managed-identity-cli-tutorial.md)
- [CLI: Gewähren des Zugriffs auf ein Media Services-Konto für eine Funktions-App](security-function-app-managed-identity-cli-tutorial.md)
- [PORTAL: Verwenden von kundenseitig verwalteten Schlüsseln oder BYOK mit Media Services über das Azure-Portal](security-customer-managed-keys-portal-tutorial.md)
- [POSTMAN/REST: Verwenden von kundenseitig verwalteten Schlüsseln (BYOK) mit der Media Services-REST-API](security-customer-managed-keys-rest-postman-tutorial.md)

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Funktionen verwalteter Identitäten für Sie und Ihre Azure-Anwendungen finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../../active-directory/managed-identities-azure-resources/overview.md).

Weitere Informationen zu Azure Functions finden Sie unter [Informationen zu Azure Functions](../../azure-functions/functions-overview.md).
