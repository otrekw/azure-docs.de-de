---
title: Zulassen des Zugriffs auf Speicher über eine Ausnahme für einen vertrauenswürdigen Dienst
titleSuffix: Azure Cognitive Search
description: In dieser Anleitung wird beschrieben, wie Sie eine Ausnahme für einen vertrauenswürdigen Dienst einrichten, um diesem den sicheren Zugriff auf Daten in Speicherkonten zu ermöglichen.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 1400f3c3d15698a5f1a145e8e0750ad7c4e9cec8
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90971408"
---
# <a name="accessing-data-in-storage-accounts-securely-via-trusted-service-exception"></a>Sicheres Zugreifen auf Daten in Speicherkonten über eine Ausnahme für einen vertrauenswürdigen Dienst

Indexer, die auf Daten in Speicherkonten zugreifen, können für den sicheren Zugriff auf Daten die Funktion [Ausnahme für vertrauenswürdige Dienste](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) nutzen. Dieser Mechanismus bietet Kunden, die [Indexer keinen Zugriff über IP-Firewallregeln](search-indexer-howto-access-ip-restricted.md) gewähren können, eine einfache, sichere und kostenlose Alternative für den Zugriff auf Daten in Speicherkonten.

## <a name="step-1-configure-connection-to-the-storage-account-via-identity"></a>Schritt 1: Konfigurieren der Verbindung mit dem Speicherkonto über die Identität

Befolgen Sie die Informationen im [Leitfaden für den Zugriff verwalteter Identitäten](search-howto-managed-identities-storage.md), um Indexer für den Zugriff auf Speicherkonten über die verwaltete Identität des Suchdiensts zu konfigurieren.

## <a name="step-2-allow-trusted-microsoft-services-to-access-the-storage-account"></a>Schritt 2: Gewähren des Zugriffs vertrauenswürdiger Microsoft-Dienste auf das Speicherkonto

Navigieren Sie im Azure-Portal zur Registerkarte „Firewalls und virtuelle Netzwerke“ des Speicherkontos. Vergewissern Sie sich, dass die Option „Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben“ aktiviert ist. Mit dieser Option wird nur dieser spezifischen Suchdienstinstanz ein geeigneter rollenbasierter Zugriff auf das Speicherkonto (starke Authentifizierung) gewährt, damit er auf Daten im Speicherkonto zugreifen kann, auch wenn diese durch IP-Firewallregeln geschützt sind.

![Ausnahme für vertrauenswürdige Dienste](media\search-indexer-howto-secure-access\exception.png "Ausnahme für vertrauenswürdige Dienste")

Indexer können nun auf Daten im Speicherkonto zugreifen, auch wenn das Konto über IP-Firewallregeln geschützt ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Storage-Indexern:

- [Azure-Blobindexer](search-howto-indexing-azure-blob-storage.md)
- [Azure Data Lake Storage Gen2-Indexer](search-howto-index-azure-data-lake-storage.md)
- [Azure-Tabellenindexer](search-howto-indexing-azure-tables.md)
