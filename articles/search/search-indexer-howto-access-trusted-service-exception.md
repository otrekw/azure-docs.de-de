---
title: Indexer-Zugriff auf Azure Storage mithilfe der Ausnahme für vertrauenswürdige Dienste
titleSuffix: Azure Cognitive Search
description: Ermöglichen Sie den Datenzugriff durch einen Indexer in Azure Cognitive Search auf Daten, die sicher in Azure Storage gespeichert sind.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 10/14/2020
ms.openlocfilehash: e139c15ef6de00376a4e1a88000d263c3486994b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92101374"
---
# <a name="indexer-access-to-azure-storage-using-the-trusted-service-exception-azure-cognitive-search"></a>Indexer-Zugriff auf Azure Storage mithilfe der Ausnahme für vertrauenswürdige Dienste (Azure Cognitive Search)

Indexer in einem Azure Cognitive Search-Dienst, die auf Daten in Azure Storage-Konten zugreifen, können für den sicheren Zugriff auf Daten die Funktion [Ausnahme für vertrauenswürdige Dienste](../storage/common/storage-network-security.md#exceptions) nutzen. Dieser Mechanismus bietet Kunden, die [Indexer keinen Zugriff über IP-Firewallregeln](search-indexer-howto-access-ip-restricted.md) gewähren können, eine einfache, sichere und kostenlose Alternative für den Zugriff auf Daten in Speicherkonten.

> [!NOTE]
> Die Unterstützung für den Zugriff auf Daten in Speicherkonten über eine Ausnahme für vertrauenswürdiger Dienste ist auf Azure Blob Storage und Azure Data Lake Storage Gen2 beschränkt. Azure Table Storage wird nicht unterstützt.

## <a name="step-1-configure-a-connection-using-a-managed-identity"></a>Schritt 1: Konfigurieren einer Verbindung mithilfe einer verwalteten Identität

Befolgen Sie die Anweisungen unter [Einrichten einer Verbindung mit einem Azure Storage-Konto mithilfe einer verwalteten Identität](search-howto-managed-identities-storage.md). Wenn Sie fertig sind, haben Sie Ihren Suchdienst bei Azure Active Directory als vertrauenswürdigen Dienst registriert und Berechtigungen in Azure Storage erteilt, die der Suchidentität bestimmte Rechte für den Zugriff auf Daten oder Informationen bietet.

## <a name="step-2-allow-trusted-microsoft-services-to-access-the-storage-account"></a>Schritt 2: Gewähren des Zugriffs vertrauenswürdiger Microsoft-Dienste auf das Speicherkonto

Navigieren Sie im Azure-Portal zur Registerkarte **Firewalls und virtuelle Netzwerke** des Speicherkontos. Vergewissern Sie sich, dass die Option **Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben** aktiviert ist. Mit dieser Option wird nur dieser spezifischen Suchdienstinstanz ein geeigneter rollenbasierter Zugriff auf das Speicherkonto (starke Authentifizierung) gewährt, damit er auf Daten im Speicherkonto zugreifen kann, auch wenn diese durch IP-Firewallregeln geschützt sind.

![Ausnahme für vertrauenswürdige Dienste](media\search-indexer-howto-secure-access\exception.png "Ausnahme für vertrauenswürdige Dienste")

Indexer können nun auf Daten im Speicherkonto zugreifen, auch wenn das Konto über IP-Firewallregeln geschützt ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Storage-Indexern:

- [Azure-Blobindexer](search-howto-indexing-azure-blob-storage.md)
- [Azure Data Lake Storage Gen2-Indexer](search-howto-index-azure-data-lake-storage.md)
- [Azure-Tabellenindexer](search-howto-indexing-azure-tables.md)