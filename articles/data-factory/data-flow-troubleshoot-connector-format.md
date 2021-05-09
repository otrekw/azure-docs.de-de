---
title: Behandeln von Connector- und Formatproblemen in Zuordnungsdatenflüssen
description: Hier erfahren Sie, wie Sie connector- und formatbezogene Datenflussprobleme in Azure Data Factory beheben.
author: linda33wj
ms.author: jingwang
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 04/20/2021
ms.openlocfilehash: 1991436e2cc890b5c6a339f79e42536ced2fbd36
ms.sourcegitcommit: 425420fe14cf5265d3e7ff31d596be62542837fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107739474"
---
# <a name="troubleshoot-connector-and-format-issues-in-mapping-data-flows-in-azure-data-factory"></a>Behandeln von Connector- und Formatproblemen in Zuordnungsdatenflüssen in Azure Data Factory


In diesem Artikel werden Methoden zur Behandlung von connector- und formatbezogenen Problemen für Zuordnungsdatenflüsse in Azure Data Factory (ADF) behandelt.


## <a name="cosmos-db--json"></a>Cosmos DB und JSON

### <a name="support-customized-schemas-in-the-source"></a>Unterstützen benutzerdefinierter Schemas in der Quelle

#### <a name="symptoms"></a>Symptome
Wenn Sie den ADF-Datenfluss verwenden möchten, um Daten aus Cosmos DB/JSON in andere Datenspeicher zu verschieben oder zu übertragen, werden möglicherweise einige Spalten der Quelldaten ausgelassen. 

#### <a name="cause"></a>Ursache 
Bei den schemafreien Connectors (Spaltennummer, Spaltenname und Spaltendatentyp der einzelnen Zeilen können sich im Vergleich zu anderen unterscheiden) werden von ADF standardmäßig Beispielzeilen (etwa die Daten der ersten 100 oder 1.000 Zeilen) verwendet, um das Schema abzuleiten, und das abgeleitete Ergebnis wird als Schema zum Lesen von Daten verwendet. Wenn Ihre Datenspeicher also über zusätzliche Spalten verfügen, die nicht in Beispielzeilen vorkommen, werden die Daten dieser zusätzlichen Spalten nicht gelesen, verschoben oder in Senkendatenspeicher übertragen.

#### <a name="recommendation"></a>Empfehlung
ADF bietet Optionen zum Anpassen des Quellschemas, um das Standardverhalten zu überschreiben und zusätzliche Felder aufzunehmen. Zum Lesen der Daten können Sie zusätzliche Spalten angeben, die im Ergebnis der Schemaableitung in der Datenfluss-Quellenprojektion ggf. fehlen, und Sie können eine der folgenden Optionen anwenden, um das benutzerdefinierte Schema festzulegen. In der Regel wird die **1. Option** bevorzugt.

- **1. Option:** Verglichen mit den ursprünglichen Quelldaten, bei denen es sich ggf. um eine große Datei, eine große Tabelle oder einen großen Container mit Millionen von Zeilen und komplexen Schemas handelt, können Sie eine temporäre Tabelle/einen temporären Container mit wenigen Zeilen erstellen, die alle zu lesenden Spalten enthalten, und dann mit dem folgenden Vorgang fortfahren: 

    1. Verwenden Sie die **Debugeinstellungen** für die Datenflussquelle, um **Import projection** (Projektion importieren) mit Beispieldateien/-tabellen zu konfigurieren und somit das vollständige Schema zu erhalten. Sie können die in der folgenden Abbildung gezeigten Schritte ausführen:<br/>

        ![Screenshot: Erster Teil der ersten Option zum Anpassen des Quellschemas](./media/data-flow-troubleshoot-connector-format/customize-schema-option-1-1.png)<br/>
         1. Wählen Sie auf der Datenflusscanvas die Option **Debugeinstellungen** aus.
         1. Wählen Sie im Popupbereich unter der Registerkarte **cosmosSource** die Option **Beispieltabelle** aus, und geben Sie den Namen Ihrer Tabelle in den Block **Tabelle** ein.
         1. Klicken Sie auf **Save** (Speichern), um Ihre Einstellungen zu speichern.
         1. Wählen Sie **Import projection** (Projektion importieren) aus.<br/>  
    
    1. Ändern Sie die **Debugeinstellungen** zurück, um das Quelldataset für die restliche Datenverschiebung/-transformation zu verwenden. Fahren Sie mit den Schritten aus der folgenden Abbildung fort:<br/>

        ![Screenshot: Zweiter Teil der ersten Option zum Anpassen des Quellschemas](./media/data-flow-troubleshoot-connector-format/customize-schema-option-1-2.png) <br/>   
         1. Wählen Sie auf der Datenflusscanvas die Option **Debugeinstellungen** aus.
         1. Wählen Sie im Popupbereich unter der Registerkarte **cosmosSource** die Option **Quelldataset** aus.
         1. Klicken Sie auf **Save** (Speichern), um Ihre Einstellungen zu speichern.<br/>
    
    Das angepasste Schema wird nun von der ADF-Datenflussruntime beim Lesen von Daten aus dem ursprünglichen Datenspeicher berücksichtigt und verwendet. <br/>

- **2. Option:** Wenn Sie mit dem Schema und der domänenspezifischen Sprache der Quelldaten vertraut sind, können Sie das Datenflussquellskript manuell aktualisieren, um zusätzliche/fehlende Spalten hinzuzufügen, die beim Lesen der Daten berücksichtigt werden sollen. Die folgende Abbildung zeigt ein entsprechendes Beispiel: 

    ![Screenshot: Zweite Option zum Anpassen des Quellschemas](./media/data-flow-troubleshoot-connector-format/customize-schema-option-2.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Hilfe zur Problembehandlung finden Sie in diesen Ressourcen:

*  [Problembehandlung bei Zuordnungsdatenflüssen in Azure Data Factory](data-flow-troubleshoot-guide.md)
*  [Data Factory-Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory-Funktionsanfragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-Videos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Stack Overflow-Forum für Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter-Informationen über Data Factory](https://twitter.com/hashtag/DataFactory)