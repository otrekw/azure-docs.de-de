---
title: 'Azure Synapse Analytics (früher SQL DW): Häufig gestellte Fragen'
description: Dieser Artikel enthält häufig gestellte Fragen von Kunden und Entwicklern zu Azure Synapse Analytics (früher SQL DW).
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 11/04/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 766615a92e616a254cca4b2b3ce1ccde40118275
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350249"
---
# <a name="azure-synapse-analytics-formerly-sql-dw-frequently-asked-questions"></a>Azure Synapse Analytics (früher SQL DW) – Häufig gestellte Fragen

## <a name="general"></a>Allgemein

Q. Was ist Azure Synapse?

A. Azure Synapse ist ein grenzenloser Analysedienst, der Data Warehousing mit Big Data-Analysen kombiniert. Er ermöglicht flexible Datenabfragen nach Ihren Vorstellungen unter Verwendung von serverlosen On-Demand-Ressourcen oder bereitgestellten Ressourcen im gewünschten Umfang. Azure Synapse kombiniert diese beiden Welten in einer vereinheitlichten Oberfläche zur Erfassung, Vorbereitung, Verwaltung und Verarbeitung von Daten für sofortige BI- und Machine Learning-Anforderungen. Weitere Informationen finden Sie unter [Was ist Azure Synapse Analytics?](sql-data-warehouse-overview-what-is.md).

Q. Was ist mit Azure SQL Data Warehouse geschehen?

A. Bei Azure Synapse handelt es sich um ein weiterentwickeltes Azure SQL Data Warehouse (SQL DW). Wir haben dasselbe branchenführende Data Warehouse auf ein ganz neues Leistungs-und Funktionsniveau gebracht. Sie können Ihre vorhandenen Data Warehouse-Workloads in der Produktion mit Azure Synapse weiterhin ausführen und gleichzeitig automatisch von den neuen Funktionen (derzeit in der Vorschauversion) profitieren. Weitere Informationen finden Sie unter [Was ist Azure Synapse Analytics?](sql-data-warehouse-overview-what-is.md).

Q. Was ist SQL Analytics?

A. SQL Analytics bezieht sich auf die Data Warehousing-Features für Unternehmen, die in Azure Synapse allgemein zur Verfügung stehen. Weitere Informationen finden Sie unter [Was ist Azure Synapse Analytics?](sql-data-warehouse-overview-what-is.md).

Q. Wie beginne ich mit der Nutzung von Azure Synapse?

A. Sie können mit einem [kostenlosen Azure-Konto](https://azure.microsoft.com/free/sql-data-warehouse/) beginnen oder [den Vertrieb kontaktieren, um weitere Informationen zu erhalten](https://info.microsoft.com/ww-landing-azure-sql-data-warehouse-contactme.html). 

Q. Was bietet Azure Synapse für die Datensicherheit?

A. Azure Synapse bietet verschiedene Lösungen für den Schutz von Daten, z.B. transparente Datenverschlüsselung (Transparent Data Encryption, TDE) und Überwachung. Weitere Informationen finden Sie unter [Sicherheit](sql-data-warehouse-overview-manage-security.md).

Q. Wo finde ich Informationen dazu, welche rechtlichen oder geschäftsspezifischen Standards Azure Synapse erfüllt?

A. Auf der Seite [Microsoft-Compliance](https://www.microsoft.com/trustcenter/compliance/complianceofferings) finden Sie verschiedene nach Produkt sortierte Compliance-Angebote (beispielsweise für SOC und ISO). Wählen Sie zunächst einen Compliancetitel aus. Erweitern Sie anschließend rechts auf der Seite im Abschnitt mit den konformen Microsoft-Clouddiensten den Knoten „Azure“, um die richtlinienkonformen Azure Synapse-Dienste anzuzeigen.

Q. Kann ich eine Verbindung mit Power BI herstellen?

A. Ja. Power BI unterstützt zwar direkte Abfragen mit Azure Synapse, ist für eine große Anzahl von Benutzern oder eine große Menge von Echtzeitdaten aber nicht vorgesehen. Um die Leistung von Power BI weiter zu steigern, wird die Nutzung von Power BI zusätzlich zu Azure Analysis Services oder Analysis Service IaaS empfohlen.

Q. Welche Kapazitätsgrenzen gibt es für SQL Analytics?

A. Informationen finden Sie auf der Seite mit den [Kapazitätsgrenzen](sql-data-warehouse-service-capacity-limits.md). 

Q. Warum erfordert Skalieren/Anhalten/Fortsetzen so viel Zeit?

A. Die Dauer der Computeverwaltungsvorgänge wird durch verschiedene Faktoren beeinflusst. Ein zeitintensiver Vorgang ist beispielsweise das Transaktionsrollback. Wenn Skalierung oder Anhalten initiiert wird, werden alle eingehenden Sitzungen blockiert, und Abfragen werden ausgeglichen. Damit das System stabil bleibt, muss ein Rollback von Transaktionen erfolgen, bevor ein Vorgang beginnen kann. Je größer die Anzahl der Transaktionen und je größer ihre Protokollgröße, desto länger wird der Vorgang angehalten, um einen stabilen Zustand des Systems wiederherzustellen.

## <a name="user-support"></a>Unterstützung für Benutzer

Q. Ich habe einen Funktionswunsch. Wie kann ich diesen übermitteln?

A. Wenn Sie einen Funktionswunsch haben, können Sie ihn über die Seite [UserVoice](https://feedback.azure.com/forums/307516-sql-data-warehouse) einsenden.

Q. Wo finde ich Informationen zu x?

A. Wenn Sie Hilfe bei der Entwicklung mit Azure Synapse benötigen, können Sie auf unserer Seite [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw) Fragen stellen. 

Q. Wie übermittle ich ein Supportticket?

A. [Supporttickets](sql-data-warehouse-get-started-create-support-ticket.md) können über das Azure-Portal erstellt werden.

## <a name="sql-languagefeature-support"></a>Unterstützung von SQL-Sprache/-Features 

Q. Welche Datentypen werden unterstützt?

A. Lesen Sie dazu [Datentypen](sql-data-warehouse-tables-data-types.md).

Q. Welche Tabellenfunktionen werden unterstützt?

A. Zahlreiche Funktionen werden unterstützt. Nicht unterstützte Funktionen finden Sie unter [Nicht unterstützte Tabellenfunktionen](sql-data-warehouse-tables-data-types.md).

## <a name="tooling-and-administration"></a>Tools und Verwaltung

Q. Unterstützt SQL Analytics Rest-APIs?

A. Ja. Die meisten REST-Funktionen, die bei SQL-Datenbank verwendet werden können, sind auch für SQL Analytics verfügbar. API-Informationen finden Sie auf den REST-Dokumentationsseiten oder auf [MSDN](https://msdn.microsoft.com/library/azure/mt163685.aspx).


## <a name="loading"></a>Laden

Q. Welche Clienttreiber werden unterstützt?

A. Treiberunterstützung für die SQL-Analyse finden Sie auf der Seite [Verbindungszeichenfolgen](sql-data-warehouse-connection-strings.md).

F: Welche Dateiformate werden von PolyBase unterstützt?

A: Orc, RC, Parquet und flacher, durch Trennzeichen getrennter Text

F: Mit welchen Datenquellen kann ich mithilfe von PolyBase eine Verbindung herstellen? 

A: [Azure Data Lake Storage](sql-data-warehouse-load-from-azure-data-lake-store.md) und [Azure Storage Blobs](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

F: Ist ein Berechnungs-Pushdown möglich, wenn ich eine Verbindung mit Azure Storage Blobs oder ADLS herstelle? 

A: Nein. PolyBase interagiert nur mit den Speicherkomponenten. 

F: Kann ich eine Verbindung mit HDI herstellen?

A: HDI kann entweder ADLS oder WASB als HDFS-Ebene verwenden. Wenn Sie über eine der HDFS-Ebenen verfügen, können Sie diese Daten in ein SQL Analytics-Data Warehouse laden. Sie können jedoch keine Pushdown-Berechnung für die HDI-Instanz ausführen. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Synapse im Allgemeinen finden Sie auf unserer Seite mit der [Übersicht](sql-data-warehouse-overview-faq.md).
