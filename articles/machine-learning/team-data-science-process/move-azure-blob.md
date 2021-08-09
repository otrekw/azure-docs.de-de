---
title: Verschieben von Daten in und aus Azure Blob Storage – Team Data Science-Prozess
description: Verschieben Sie Daten in und aus Azure Blob Storage mithilfe des Azure Storage-Explorers, AzCopy, Python und SSIS.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 151375a37b80567aecf100ec3cd576882d06e3f3
ms.sourcegitcommit: f9e368733d7fca2877d9013ae73a8a63911cb88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111902247"
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>Verschieben von Daten in und aus Azure Blob Storage

Der Team Data Science-Prozess erfordert, dass Daten in einer Vielzahl von Speicherumgebungen erfasst oder geladen werden, um in jeder Phase des Prozesses auf die am besten geeignete Weise verarbeitet oder analysiert zu werden.

## <a name="different-technologies-for-moving-data"></a>Verschiedene Technologien zum Verschieben von Daten

In den folgenden Artikeln beschrieben, wie Sie mithilfe unterschiedlicher Technologien Daten in und aus Azure Blob Storage verschieben.

* [Azure Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
  * Storage-Explorer ist ein kostenloses Tool von Microsoft, das Ihnen das Arbeiten mit Azure Storage-Daten unter Windows, macOS und Linux ermöglicht.
  * Wenn Sie einen virtuellen Computer verwenden, der mit den von den virtuellen Data Science-Computern in Azure bereitgestellten Skripts eingerichtet wurde, ist der Azure Storage-Explorer bereits auf dem virtuellen Computer installiert.
* [AzCopy](../../storage/common/storage-use-azcopy-v10.md)
  * AzCopy ist ein Befehlszeilenhilfsprogramm, das Sie verwenden können, um Blobs oder Dateien in oder aus einem Speicherkonto zu kopieren. 
* [Python SDK](../../storage/blobs/storage-quickstart-blobs-python.md)
  * Mit der Azure Blob Storage-Clientbibliothek für Python können Sie Blobs verschieben.
* [SQL Server Integration Services-Feature Pack (SSIS) für Azure](/sql/integration-services/azure-feature-pack-for-integration-services-ssis)
  * Das SSIS bietet Komponenten zum Herstellen einer Verbindung mit Azure, Übertragen von Daten zwischen Azure und lokalen Datenquellen und Verarbeiten von in Azure gespeicherten Daten. Eine Beschreibung gängiger Szenarien, in denen SQL Server Integration Services (SSIS) zum Erfüllen geschäftlicher Anforderungen in hybriden Datenintegrationsszenarien zum Einsatz kommt, finden Sie im Blog [Weitere Aktionen mit dem SQL Server Integration Services-Feature Pack für Azure](https://techcommunity.microsoft.com/t5/sql-server-integration-services/doing-more-with-sql-server-integration-services-feature-pack-for/ba-p/388238).
  * Schulungsmaterialien für SSIS finden Sie unter [Praktische Schulung für SSIS](https://www.microsoft.com/sql-server/training-certification).
  * Informationen zur Inbetriebnahme von SSIS zum Erstellen von ETL-Paketen (Extrahieren, Transformieren, Laden) finden Sie im [SSIS-Tutorial: Erstellen eines einfachen ETL-Pakets](/sql/integration-services/ssis-how-to-create-an-etl-package).

Es hängt vom jeweiligen Szenario ab, welche Methode für Sie am besten geeignet ist. Mithilfe der Informationen im Artikel [Szenarios für erweiterte Analysen in Azure Machine Learning](plan-sample-scenarios.md) können Sie die Ressourcen ermitteln, die Sie für verschiedene, im erweiterten Analyseprozess verwendeten Data Science-Workflows benötigen.

> [!NOTE]
> Eine umfassende Einführung in Azure-Blobspeicher finden Sie unter [Erste Schritte mit Azure Blob Storage](../../storage/blobs/storage-quickstart-blobs-dotnet.md) und [Azure-Blobdienst](/rest/api/storageservices/Blob-Service-Concepts).
> 
> 

## <a name="using-azure-data-factory"></a>Verwenden von Azure Data Factory

Als Alternative können Sie [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) für folgende Zwecke verwenden: 

* Erstellen und Planen einer Pipeline, die Daten aus Azure Blob Storage herunterlädt
* Übergeben dieser Daten an einen veröffentlichten Azure Machine Learning-Webdienst 
* Abrufen der Predictive Analytics-Ergebnisse und 
* Hochladen der Ergebnisse in den Speicher 

Weitere Informationen finden Sie unter [Erstellen von Vorhersagepipelines mithilfe von Azure Data Factory und Azure Machine Learning](../../data-factory/transform-data-using-machine-learning.md).

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie über ein Azure-Abonnement, ein Speicherkonto und den zugehörigen Speicherschlüssel für dieses Konto verfügen. Um Daten hoch- und herunterladen zu können, müssen Sie den Namen Ihres Azure Storage-Kontos und den Kontoschlüssel kennen.

* Informationen zum Einrichten eines Azure-Abonnements finden Sie unter [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).
* Anleitungen zum Erstellen eines Speicherkontos und zum Abrufen von Konto- und Schlüsselinformationen finden Sie unter [Erstellen eines Azure-Speicherkontos](../../storage/common/storage-account-create.md).