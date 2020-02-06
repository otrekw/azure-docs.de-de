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
ms.openlocfilehash: fc58651bcb3b266b981fb953fd7341427d47fb2c
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76717577"
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>Verschieben von Daten in und aus Azure Blob Storage

Der Team Data Science-Prozess erfordert, dass Daten in einer Vielzahl von Speicherumgebungen erfasst oder geladen werden, um in jeder Phase des Prozesses auf die am besten geeignete Weise verarbeitet oder analysiert zu werden.

## <a name="different-technologies-for-moving-data"></a>Verschiedene Technologien zum Verschieben von Daten

In den folgenden Artikeln beschrieben, wie Sie mithilfe unterschiedlicher Technologien Daten in und aus Azure Blob Storage verschieben.

* [Azure Storage-Explorer](move-data-to-azure-blob-using-azure-storage-explorer.md)
* [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)
* [Python](move-data-to-azure-blob-using-python.md)
* [SSIS](move-data-to-azure-blob-using-ssis.md)

Es hängt vom jeweiligen Szenario ab, welche Methode für Sie am besten geeignet ist. Mithilfe der Informationen im Artikel [Szenarien für erweiterte Analysen in Azure Machine Learning](plan-sample-scenarios.md) können Sie die Ressourcen ermitteln, die Sie für verschiedene, im erweiterten Analyseprozess verwendeten Data Science-Workflows benötigen.

> [!NOTE]
> Eine umfassende Einführung in Azure-Blobspeicher finden Sie unter [Erste Schritte mit Azure Blob Storage](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) und [Azure-Blobdienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).
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
* Anleitungen zum Erstellen eines Speicherkontos und zum Abrufen von Konto- und Schlüsselinformationen finden Sie unter [Erstellen eines Azure-Speicherkontos](../../storage/common/storage-create-storage-account.md).

