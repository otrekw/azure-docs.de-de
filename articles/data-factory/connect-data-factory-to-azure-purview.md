---
title: Herstellen einer Verbindung zwischen einer Data Factory und Azure Purview
description: Erfahren Sie, wie Sie eine Verbindung zwischen einer Data Factory und Azure Purview herstellen.
services: data-factory
ms.author: lle
author: lrtoyou1223
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom:
- seo-lt-2019
- references_regions
ms.date: 12/3/2020
ms.openlocfilehash: 7dc05c88416bb2a23221029bc04c506271a86652
ms.sourcegitcommit: 48e5379c373f8bd98bc6de439482248cd07ae883
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2021
ms.locfileid: "98108346"
---
# <a name="connect-data-factory-to-azure-purview-preview"></a>Herstellen einer Verbindung zwischen einer Data Factory und Azure Purview (Vorschau)
[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

In diesem Artikel wird erläutert, wie Sie eine Data Factory mit Azure Purview verbinden und wie Sie Herkunftsdaten der ADF-Aktivitäten „Copy“, „Dataflow“ und „SSIS-Paket ausführen“ übermitteln.

## <a name="connect-data-factory-to-azure-purview"></a>Herstellen einer Verbindung zwischen einer Data Factory und Azure Purview
Azure Purview ist ein neuer Clouddienst, den Datenbenutzer für die zentrale Verwaltung der Datengovernance aller ihrer Daten in Cloud- und lokalen Umgebungen verwenden können. Sie können Ihre Data Factory mit Azure Purview verbinden und über diese Verbindung dann mit Azure Purview Herkunftsdaten von Copy- und Dataflow-Aktivitäten sowie von der Aktivität „SSIS-Paket ausführen“ aufzeichnen. Informationen zum Registrieren einer Data Factory in Azure Purview finden Sie unter [Verbinden von Azure Data Factory und Azure Purview](https://docs.microsoft.com/azure/purview/how-to-link-azure-data-factory). 

## <a name="report-lineage-data-to-azure-purview"></a>Übermitteln von Herkunftsdaten an Azure Purview
Beim Ausführen der Aktivitäten für Kopiervorgänge, Datenflüsse oder das Ausführen eines SSIS-Pakets in Azure Data Factory können Kunden die Abhängigkeitsbeziehung abrufen und sich eine allgemeine Übersicht über den gesamten Workflowprozess zwischen den Datenquellen und dem Ziel verschaffen.
Informationen zum Sammeln von Herkunftsinformationen aus Azure Data Factory finden Sie unter [Data Factory-Herkunftsdaten](https://docs.microsoft.com/azure/purview/how-to-link-azure-data-factory#supported-azure-data-factory-activities).

## <a name="next-steps"></a>Nächste Schritte
[Benutzerhandbuch zur Katalogherkunft](https://docs.microsoft.com/azure/purview/catalog-lineage-user-guide)

[Tutorial: Pushen von Data Factory-Herkunftsdaten an Azure Purview](turorial-push-lineage-to-purview.md)
