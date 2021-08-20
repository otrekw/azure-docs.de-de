---
title: Neues in Azure Data Factory
description: Auf dieser Seite werden neue Features und Verbesserungen bei Azure Data Factory beschrieben.
author: pennyzhou-msft
ms.author: xupzhou
ms.reviewer: xupzhou
ms.service: data-factory
ms.topic: overview
ms.date: 07/14/2021
ms.openlocfilehash: fe4c5fb72ce3cd32e14bad211683e01ac6edf3ab
ms.sourcegitcommit: 92dd25772f209d7d3f34582ccb8985e1a099fe62
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2021
ms.locfileid: "114341936"
---
# <a name="whats-new-in-azure-data-factory"></a>Neues in Azure Data Factory

Azure Data Factory wird kontinuierlich verbessert. Damit Sie bezüglich der neuesten Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

- Neueste Versionen
- Bekannte Probleme
- Behebung von Programmfehlern
- Veraltete Funktionen
- Pläne für Änderungen

Besuchen Sie regelmäßig diese Seite. Sie wird monatlich aktualisiert. 

## <a name="june-2021"></a>Juni 2021
<br>
<table>
<tr><td><b>Dienstkategorie</b></td><td><b>Verbesserungen beim Dienst</b></td><td><b>Details</b></td></tr>
<tr><td rowspan=4 valign="middle"><b>Datenverschiebung</b></td><td>Neue Benutzeroberfläche mit Azure Data Factory-Tool „Daten kopieren“</td><td>Das neu gestaltete Tool „Daten kopieren“ bietet jetzt eine bessere Datenerfassung.<br><a href="https://techcommunity.microsoft.com/t5/azure-data-factory/a-re-designed-copy-data-tool-experience/ba-p/2380634">Weitere Informationen</a></td></tr>
<tr><td>MongoDB und MongoDB Atlas werden sowohl als Quelle als auch als Senke unterstützt.</td><td>Durch diese Verbesserung wird das Kopieren von Daten zwischen einem unterstützten Datenspeicher und einer MongoDB- oder MongoDB Atlas-Datenbank unterstützt.<br><a href="https://techcommunity.microsoft.com/t5/azure-data-factory/new-connectors-available-in-adf-mongodb-and-mongodb-atlas-are/ba-p/2441482">Weitere Informationen</a></td></tr>
<tr><td>Always Encrypted wird für Azure SQL-Datenbank-, Azure SQL Managed Instance- und SQL Server-Connectors als Quelle und als Senke unterstützt.</td><td>Always Encrypted ist in Azure Data Factory für Azure SQL-Datenbank-, Azure SQL Managed Instance- und SQL Server-Connectors für die Copy-Aktivität verfügbar.<br><a href="https://techcommunity.microsoft.com/t5/azure-data-factory/azure-data-factory-copy-now-supports-always-encrypted-for-both/ba-p/2461346">Weitere Informationen</a></td></tr>
<tr><td>Das Festlegen benutzerdefinierter Metadaten wird in der Copy-Aktivität unterstützt, wenn als Senke ADLS Gen2 oder Azure Blob verwendet werden.</td><td>Beim Schreiben in ADLS Gen2 oder Azure-Blobs unterstützt die Copy-Aktivität das Festlegen benutzerdefinierter Metadaten oder die Speicherung der Informationen zur letzten Änderung der Quelldatei als Metadaten.<br><a href="https://techcommunity.microsoft.com/t5/azure-data-factory/support-setting-custom-metadata-when-writing-to-blob-adls-gen2/ba-p/2545506#M490">Weitere Informationen</a></td></tr>
<tr><td rowspan=4 valign="middle"><b>Datenfluss</b></td><td>Unterstützung für SQL Server als Quelle und Senke in Datenflüssen</td><td>SQL Server wird jetzt als Quelle und Senke in Datenflüssen unterstützt. Unter dem Link finden Sie Anweisungen zum Konfigurieren Ihres Netzwerks mithilfe der Azure Integration Runtime-Funktion für ein verwaltetes VNET, die die Kommunikation mit Ihren lokalen SQL Server- und Cloud-VM-Instanzen ermöglicht.<br><a href="https://techcommunity.microsoft.com/t5/azure-data-factory/new-data-flow-connector-sql-server-as-source-and-sink/ba-p/2406213">Weitere Informationen</a></td></tr>
<tr><td>Die schnelle Wiederverwendung von Dataflowclustern ist jetzt standardmäßig für alle neuen Azure Integration Runtime-Instanzen aktiviert.</td><td>ADF freut sich, die allgemeine Verfügbarkeit des beliebten Features für die schnelle Wiederverwendung von Dataflowstarts bekannt zu geben. Die schnelle Wiederverwendung ist ab sofort für alle neuen Azure Integration Runtime-Instanzen standardmäßig aktiviert.<br><a href="https://techcommunity.microsoft.com/t5/azure-data-factory/how-to-startup-your-data-flows-execution-in-less-than-5-seconds/ba-p/2267365">Weitere Informationen</a></td></tr>
<tr><td>Power Query-Aktivität in ADF als öffentliche Vorschau</td><td>Sie können nun mithilfe von Data Wrangling in Azure Data Factory komplexe Feldzuordnungen zu Ihrer Power Query-Senke erstellen. Die Senke wird jetzt in der Pipeline in der Power Query-Aktivität (Vorschau) konfiguriert, um dieses Update umzusetzen.<br><a href="wrangling-tutorial.md">Weitere Informationen</a></td></tr>
<tr><td>Aktualisierte Benutzeroberfläche für die Überwachung von Datenflüssen in Azure Data Factory</td><td>Ein neues Update für Azure Data Factory betrifft die Benutzeroberfläche für die Überwachung. Es vereinfacht die Anzeige der ETL-Auftragsausführungen in Ihrem Datenfluss und ermöglicht die schnelle Ermittlung von Bereichen für die Leistungsoptimierung.<br><a href="https://techcommunity.microsoft.com/t5/azure-data-factory/updated-data-flows-monitoring-ui-in-adf-amp-synapse/ba-p/2432199">Weitere Informationen</a></td></tr>
<tr><td><b>SQL Server Integration Services (SSIS)</b></td><td>Ausführen von SQL an beliebigen Orten in drei einfachen Schritten mit SSIS in Azure Data Factory</td><td>Dieser Beitrag enthält drei einfache Schritte zum Ausführen beliebiger SQL-Anweisungen/-Skripts an einer beliebigen Stelle mit SSIS in Azure Data Factory.<ol><li>Bereiten Sie Ihre selbstgehostete Integration Runtime/SSIS Integration Runtime vor.</li><li>Bereiten Sie eine Aktivität „SSIS-Paket ausführen“ in der Azure Data Factory-Pipeline vor.</li><li>Führen Sie die Aktivität „SSIS-Paket ausführen“ in Ihrer selbstgehosteten Integration Runtime/SSIS Integration Runtime aus.</li></ol><a href="https://techcommunity.microsoft.com/t5/sql-server-integration-services/run-any-sql-anywhere-in-3-easy-steps-with-ssis-in-azure-data/ba-p/2457244">Weitere Informationen</a></td></tr>
</table>

## <a name="more-information"></a>Weitere Informationen

- [Blog: Azure Data Factory](https://techcommunity.microsoft.com/t5/azure-data-factory/bg-p/AzureDataFactoryBlog)
- [Stack Overflow-Forum](https://stackoverflow.com/questions/tagged/azure-data-factory)
- [Twitter](https://twitter.com/AzDataFactory?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor)
- [Videos](https://www.youtube.com/channel/UC2S0k7NeLcEm5_IhHUwpN0g/featured)





