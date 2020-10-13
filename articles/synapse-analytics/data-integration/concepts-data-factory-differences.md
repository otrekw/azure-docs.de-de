---
title: Unterschiede zu Azure Data Factory
description: Erfahren Sie, wie sich die Datenintegrationsfunktionen von Azure Synapse Analytics von denen in Azure Data Factory unterscheiden.
services: synapse-analytics
author: djpmsft
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 09/23/2020
ms.author: daperlov
ms.reviewer: jrasnick
ms.openlocfilehash: be098977ba51e529aaecfb5dc3b7a03444f55a7e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91339142"
---
# <a name="data-integration-in-azure-synapse-analytics-versus-azure-data-factory"></a>Datenintegration in Azure Synapse Analytics im Vergleich zu Azure Data Factory

In Azure Synapse Analytics basieren die Datenintegrationsfunktionen wie Synapse-Pipelines und Datenflüsse auf denen von Azure Data Factory. Weitere Informationen finden Sie unter [Was ist Azure Data Factory?](../../data-factory/introduction.md). Fast alle Funktionen sind identisch oder ähnlich, und die Dokumentation wird von den beiden Diensten gemeinsam genutzt. In diesem Artikel werden die aktuellen Unterschiede zwischen Azure Data Factory und Azure Synapse hervorgehoben und identifiziert.

Überprüfen Sie den Moniker am Anfang des Artikels, um zu sehen, ob eine Azure Data Factory-Funktion oder ein diesbezüglicher Artikel für Azure Synapse gilt.

![„Gilt für“-Moniker](../media/concepts-data-factory-differences/applies-to-moniker.png "„Gilt für“-Moniker")

## <a name="features-in-azure-data-factory-not-planned-for-azure-synapse"></a>Funktionen in Azure Data Factory, die nicht für Azure Synapse geplant sind

Die folgenden Funktionen sind in Azure Data Factory verfügbar, sie sind aber nicht für Azure Synapse geplant.

* Die Fähigkeit zum Übertragen von SSIS-Paketen per Lift & Shift.
* Snowflake als Senke bei der Kopieraktivität und dem Zuordnungsdatenfluss.
* Die Gültigkeitsdauereinstellung des Zuordnungsdatenflusses der Azure Integration Runtime.

## <a name="azure-synapse-features-not-supported-in-azure-data-factory"></a>In Azure Data Factory nicht unterstützte Azure Synapse-Funktionen

Die folgenden Funktionen sind in Azure Synapse verfügbar, sie sind aber nicht für Azure Data Factory geplant.

* Die Spark-Auftragsüberwachung von Zuordnungsdatenflüssen ist nur in Synapse verfügbar. In Synapse ist die Spark-Engine im Abonnement des Benutzers enthalten, sodass Benutzer ausführliche Spark-Protokolle anzeigen können. In Azure Data Factory erfolgt die Auftragsausführung auf einem von Azure Data Factory verwalteten Spark-Cluster. 

## <a name="azure-data-factory-features-that-behave-differently-in-synapse"></a>Azure Data Factory-Funktionen, die sich in Synapse anders verhalten

Die folgenden Funktionen verhalten sich entweder anders oder sind zurzeit in Azure Synapse nicht vorhanden. 

* Wranglingdatenflüsse
* Der Lösungsvorlagenkatalog
* Git-Integration und eine native CI/CD-Lösung
* Integration in Azure Monitor
* Umbenennen von Ressourcen nach der Veröffentlichung
* Konfiguration der Hybridintegration-Runtime innerhalb eines Synapse-Arbeitsbereichs. Ein Benutzer kann nicht sowohl über eine verwaltete VNet IR als auch über eine Azure IR verfügen.
* Gemeinsame Verwendung der Integration Runtime zwischen Synapse-Arbeitsbereichen

## <a name="next-steps"></a>Nächste Schritte

Erste Schritte mit der Datenintegration in Ihrem Synapse-Arbeitsbereich, indem Sie lernen, wie Sie [Daten in einem Azure Data Lake Storage Gen2-Konto erfassen](data-integration-data-lake.md).
