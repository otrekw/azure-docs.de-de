---
title: Verwaltete Identitäten und vertrauenswürdiger Speicher mit Media Services
description: Media Services können mit verwalteten Identitäten verwendet werden, um die Verwendung von vertrauenswürdigem Speicher zu ermöglichen.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.date: 11/04/2020
ms.author: inhenkel
ms.openlocfilehash: a0ded2a1ad5cd590ab5715edebde2ab19e399e8d
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94686436"
---
# <a name="managed-identities-and-trusted-storage-with-media-services"></a>Verwaltete Identitäten und vertrauenswürdiger Speicher mit Media Services

Media Services können mit [verwalteten Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) verwendet werden, um die Verwendung von vertrauenswürdigem Speicher zu ermöglichen. Wenn Sie ein Media Services-Konto erstellen, müssen Sie es einem Speicherkonto zuordnen. Von Media Services kann per Systemauthentifizierung auf dieses Speicherkonto zugegriffen werden. Media Services überprüft, ob sich das Media Services-Konto und das Speicherkonto im gleichen Abonnement befinden und ob der Benutzer, der die Zuordnung hinzufügt, Zugriff auf das Speicherkonto hat (mittels rollenbasierter Zugriffssteuerung für Azure Resource Manager).

## <a name="trusted-storage"></a>Vertrauenswürdiger Speicher

Wenn Sie zum Schutz Ihres Speicherkontos jedoch eine Firewall verwenden möchten, müssen Sie die Authentifizierung der verwalteten Identität verwenden. Dadurch kann von Media Services per vertrauenswürdigem Speicherzugriff auf das Speicherkonto zugegriffen werden, das mit einer Firewall oder mit einer VNET-Einschränkung konfiguriert wurde.  Weitere Informationen zu vertrauenswürdigen Microsoft-Diensten finden Sie unter [Konfigurieren von Azure Storage-Firewalls und virtuellen Netzwerken](https://docs.microsoft.com/azure/storage/common/storage-network-security#trusted-microsoft-services).

## <a name="media-services-managed-identity-scenarios"></a>Szenarien mit verwalteter Media Services-Identität

Es gibt derzeit zwei Szenarien, in denen die verwaltete Identität mit Media Services verwendet werden kann:

- Verwendung der verwalteten Identität des Media Services Kontos für den Zugriff auf Speicherkonten

- Verwendung der verwalteten Identität des Media Services Kontos für den Zugriff auf Kundenschlüssel über Key Vault

In den nächsten beiden Abschnitten werden die Unterschiede zwischen den beiden Szenarien beschrieben.

### <a name="use-the-managed-identity-of-the-media-services-account-to-access-storage-accounts"></a>Verwenden der verwalteten Identität des Media Services Kontos für den Zugriff auf Speicherkonten

1. Erstellen Sie ein Media Services-Kontos mit einer verwalteten Identität.
1. Gewähren Sie dem Prinzipal der verwalteten Identität Zugriff auf ein Speicherkonto, das Sie besitzen.
1. Von Media Services kann daraufhin unter Verwendung der verwalteten Identität in Ihrem Namen auf das Speicherkonto zugegriffen werden.

### <a name="use-the-managed-identity-of-the-media-services-account-to-access-key-vault-to-access-customer-keys"></a>Verwenden der verwalteten Identität des Media Services Kontos für den Zugriff auf Kundenschlüssel über Key Vault

1. Erstellen Sie ein Media Services-Kontos mit einer verwalteten Identität.
1. Gewähren Sie dem Prinzipal der verwalteten Identität Zugriff auf eine Key Vault-Instanz, die Sie besitzen.
1. Konfigurieren Sie das Media Services-Konto für die Verwendung der kundenschlüsselbasierten Kontoverschlüsselung.
1. Von Media Services wird unter Verwendung der verwalteten Identität in Ihrem Namen auf Key Vault zugegriffen.

Weitere Informationen zu kundenseitig verwalteten Schlüsseln und zu Key Vault finden Sie unter [Bring Your Own Key (vom Kunden verwaltete Schlüssel) in Media Services](concept-use-customer-managed-keys-byok.md).

## <a name="tutorials"></a>Tutorials

Die folgenden Tutorials enthalten beide oben erwähnten Szenarien:

- [Tutorial: Verwenden von kundenseitig verwalteten Schlüsseln (BYOK) mit Media Services über das Azure-Portal](tutorial-byok-portal.md)
- [Tutorial: Verwenden von kundenseitig verwalteten Schlüsseln (BYOK) mit der Media Services-REST-API](tutorial-byok-postman.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Funktionen verwalteter Identitäten für Sie und Ihre Azure-Anwendungen finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).
