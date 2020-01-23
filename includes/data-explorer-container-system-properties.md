---
author: orspod
ms.service: data-explorer
ms.topic: include
ms.date: 01/08/2020
ms.author: orspodek
ms.openlocfilehash: f9788e4623ce60ad55d79558d1d77a17eb2a9f26
ms.sourcegitcommit: 5b073caafebaf80dc1774b66483136ac342f7808
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75779948"
---
### <a name="event-system-properties-mapping"></a>Zuordnung von Ereignissystemeigenschaften

Wenn Sie **Ereignissystemeigenschaften** im Abschnitt **Datenquelle** der obigen Tabelle ausgewählt haben, wechseln Sie zur [Webbenutzeroberfläche](https://dataexplorer.azure.com/), um den entsprechenden KQL-Befehl für die ordnungsgemäße Zuordnungserstellung auszuführen.

   **Für CSV-Zuordnung:**

    ```kusto
    .create table MyTable ingestion csv mapping "CsvMapping1"
    '['
    '   { "column" : "messageid", "DataType":"string", "Properties":{"Ordinal":"0"}},'
    '   { "column" : "userid", "DataType":"string", "Properties":{"Ordinal":"1"}},'
    '   { "column" : "other", "DataType":"int", "Properties":{"Ordinal":"2"}}'
    ']'
    ```
 
   **Für JSON-Zuordnung:**

    ```kusto
    .create table MyTable ingestion json mapping "JsonMapping1"
    '['
    '    { "column" : "messageid", "datatype" : "string", "Properties":{"Path":"$.message-id"}},'
    '    { "column" : "userid", "Properties":{"Path":"$.user-id"}},'
    '    { "column" : "other", "Properties":{"Path":"$.other"}}'
    ']'
    ```

   > [!TIP]
   > * Sie müssen alle ausgewählten Eigenschaften in die Zuordnung einschließen. 
   > * Die Reihenfolge der Eigenschaften ist bei der CSV-Zuordnung wichtig. Die Systemeigenschaften müssen vor allen anderen Eigenschaften und in derselben Reihenfolge aufgelistet werden, in der sie in der **Ereignissystemeigenschaften**-Dropdownliste angezeigt werden.