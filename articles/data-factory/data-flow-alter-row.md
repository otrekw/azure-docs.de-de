---
title: Zeilenänderungstransformation im Zuordnungsdatenfluss
description: Hier erfahren Sie, wie Sie das Datenbankziel mithilfe der Zeilenänderungstransformation im Zuordnungsdatenfluss aktualisieren.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/06/2020
ms.openlocfilehash: c3858756a0140481c0ab249e29c95f76c4b90da5
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "82982648"
---
# <a name="alter-row-transformation-in-mapping-data-flow"></a>Zeilenänderungstransformation im Zuordnungsdatenfluss

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Verwenden Sie die Zeilenänderungstransformation, um Einfüge-, Lösch-, Aktualisierungs- und Upsertrichtlinien für Zeilen festzulegen. Sie können 1: n-Bedingungen als Ausdrücke hinzufügen. Diese Bedingungen müssen in der Reihenfolge ihrer Priorität angegeben werden, da jede Zeile mit der Richtlinie gekennzeichnet wird, die dem ersten übereinstimmenden Ausdruck entspricht. Jede dieser Bedingungen kann dazu führen, dass für eine Zeile (oder für mehrere Zeilen) ein Einfüge-, Aktualisierungs-, Lösch- oder Upsertvorgang ausgeführt wird. Die Zeilenänderung kann sowohl DDL- als auch DML-Aktionen für Ihre Datenbank generieren.

![Zeilenänderungseinstellungen](media/data-flow/alter-row1.png "Zeilenänderungseinstellungen")

Zeilenänderungstransformationen können nur für Datenbanksenken oder Cosmos DB-Senken in Ihrem Datenfluss verwendet werden. Die Aktionen, die Sie Zeilen zuweisen (Einfügen, Aktualisieren, Löschen, Upsert), werden während Debugsitzungen nicht ausgeführt. Führen Sie eine Aktivität des Typs „Datenfluss ausführen“ in einer Pipeline aus, um die Richtlinien für Zeilenänderungen auf Ihre Datenbanktabellen anzuwenden.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4vJYc]

## <a name="specify-a-default-row-policy"></a>Angeben einer Standardzeilenrichtlinie

Erstellen Sie eine Zeilenänderungstransformation, und geben Sie eine Zeilenrichtlinie mit der Bedingung `true()` an. Jede Zeile, die mit keinem der zuvor definierten Ausdrücke übereinstimmt, wird für die angegebene Zeilenrichtlinie gekennzeichnet. Standardmäßig wird jede Zeile, die mit keinem bedingten Ausdruck übereinstimmt, für `Insert` gekennzeichnet.

![Richtlinie für Zeilenänderungen](media/data-flow/alter-row4.png "Richtlinie für Zeilenänderungen")

> [!NOTE]
> Wenn Sie alle Zeilen mit einer Richtlinie kennzeichnen möchten, können Sie eine Bedingung für diese Richtlinie erstellen und `true()` als Bedingung angeben.

## <a name="view-policies-in-data-preview"></a>Anzeigen von Richtlinien in der Datenvorschau

Verwenden Sie den [Debugmodus](concepts-data-flow-debug-mode.md), um die Ergebnisse Ihrer Richtlinien für Zeilenänderungen im Datenvorschaubereich anzuzeigen. Eine Datenvorschau einer Zeilenänderungstransformation erzeugt keine DDL- oder DML-Aktionen für Ihr Ziel.

![Richtlinien für Zeilenänderungen](media/data-flow/alter-row3.png "Richtlinien für Zeilenänderungen")

Jede Richtlinie für Zeilenänderungen wird durch ein Symbol dargestellt, das angibt, ob eine Aktion des Typs „Einfügen“, „Aktualisieren“, „Upsert“ oder „Löschen“ ausgeführt wird. Am Anfang des Kopfteils wird angezeigt, auf wie viele Zeilen sich die jeweilige Richtlinie in der Vorschau auswirkt.

## <a name="allow-alter-row-policies-in-sink"></a>Zulassen von Richtlinien für Zeilenänderungen in der Senke

Damit die Richtlinien für Zeilenänderungen funktionieren, muss der Datenstrom in eine Datenbank oder eine Cosmos-Senke schreiben. Aktivieren Sie auf der Registerkarte **Einstellungen** in Ihrer Senke, welche Richtlinien für Zeilenänderungen dafür zulässig sein sollen.

![Senke für Zeilenänderung](media/data-flow/alter-row2.png "Senke für Zeilenänderung")

Das Standardverhalten besteht darin, dass nur Einfügevorgänge zugelassen werden. Sollen Aktualisierungs-, Upsert- oder Löschvorgänge zulässig sein, aktivieren Sie das Kontrollkästchen in der Senke, das dieser Bedingung entspricht. Wenn Aktualisierungs-, Upsert- oder Löschvorgänge aktiviert sind, müssen Sie angeben, welche Schlüsselspalten in der Senke abgeglichen werden sollen.

> [!NOTE]
> Falls das Schema der Zieltabelle in der Senke durch Ihre Einfüge-, Aktualisierungs- oder Upsertvorgänge geändert wurde, schlägt der Datenfluss fehl. Um das Zielschema in Ihrer Datenbank zu ändern, wählen Sie **Tabelle neu erstellen** als Tabellenaktion aus. Dadurch wird Ihre Tabelle verworfen und mit der neuen Schemadefinition neu erstellt.

Für die Senkentransformation ist entweder ein einzelner Schlüssel oder eine Reihe von Schlüsseln zur eindeutigen Zeilenidentifizierung in Ihrer Zieldatenbank erforderlich. Legen Sie für SQL-Senken die Schlüssel auf der Registerkarte „Senkeneinstellungen“ fest. Legen Sie für CosmosDB den Partitionsschlüssel in den Einstellungen und außerdem das CosmosDB-Systemfeld „ID“ in Ihrer Senkenzuordnung fest. Bei CosmosDB ist es zwingend erforderlich, die Systemspalte „ID“ für Update-, Upsert- und Löschvorgänge mit einzubeziehen.

## <a name="merges-and-upserts-with-azure-sql-database-and-synapse"></a>Merge- und Upsertvorgänge mit Azure SQL-Datenbank und Synapse

ADF-Datenflüsse unterstützen Mergevorgänge für Azure SQL-Datenbank und Synapse-Datenbankpool (Data Warehouse) mit der Option „upsert“.

Es kann jedoch zu Szenarien kommen, in denen Ihr Schema der Zieldatenbank die IDENTITY-Eigenschaft von Schlüsselspalten verwendet hat. ADF erfordert es, dass Sie die Schlüssel identifizieren, mit denen Sie die Zeilenwerte für Update- und Upsertvorgänge abgleichen werden. Wenn aber für die Zielspalte die IDENTITY-Eigenschaft festgelegt wurde und Sie die Upsertrichtlinie verwenden, können Sie in der Zieldatenbank nicht in die Spalte schreiben. Möglicherweise treten auch Fehler auf, wenn Sie versuchen, einen Upsert-Vorgang für die Verteilungsspalte einer verteilten Tabelle durchzuführen.

Hier sind die Möglichkeiten zur Behebung dieses Problems:

1. Wechseln Sie zu den Einstellungen für die Senkentransformation, und legen Sie „Skip writing key columns“ (Schreiben von Schlüsselspalten überspringen) fest. Dadurch wird ADF angewiesen, nicht in die Spalte zu schreiben, die Sie als Schlüsselwert für Ihre Zuordnung ausgewählt haben.

2. Wenn diese Schlüsselspalte nicht diejenige Spalte ist, die das Problem bei Identitätsspalten verursacht, können Sie die SQL-Option für die Vorbearbeitung von Senkentransformationen verwenden: ```SET IDENTITY_INSERT tbl_content ON```. Deaktivieren Sie die Option dann mit der SQL-Eigenschaft für Nachverarbeitung: ```SET IDENTITY_INSERT tbl_content OFF```.

3. Sowohl bei der Identitäts- als auch der Verteilungsspalte können Sie Ihre Logik vom Upsert auf die Verwendung einer separaten Updatebedingung und einer separaten Einfügebedingung mithilfe einer Transformation für bedingtes Teilen umstellen. Auf diese Weise können Sie die Zuordnung für den Updatepfad so festlegen, dass die Schlüsselspaltenzuordnung ignoriert wird.

## <a name="data-flow-script"></a>Datenflussskript

### <a name="syntax"></a>Syntax

```
<incomingStream>
    alterRow(
           insertIf(<condition>?),
           updateIf(<condition>?),
           deleteIf(<condition>?),
           upsertIf(<condition>?),
        ) ~> <alterRowTransformationName>
```

### <a name="example"></a>Beispiel

Das nachstehende Beispiel ist die Zeilenänderungstransformation `CleanData`, die aus dem eingehenden Stream `SpecifyUpsertConditions` drei Zeilenänderungsbedingungen erstellt. In der vorherigen Transformation wird die Spalte `alterRowCondition` berechnet, die bestimmt, ob eine Zeile in der Datenbank eingefügt, aktualisiert oder gelöscht werden soll oder nicht. Wenn der Wert der Spalte einen Zeichenfolgenwert enthält, der mit der Zeilenänderungsregel übereinstimmt, wird ihr diese Richtlinie zugewiesen.

Auf der Data Factory-Benutzeroberfläche sieht diese Transformation wie folgt aus:

![Beispiel für Zeilenänderung](media/data-flow/alter-row4.png "Beispiel für Zeilenänderung")

Das Datenflussskript für diese Transformation befindet sich im folgenden Codeausschnitt:

```
SpecifyUpsertConditions alterRow(insertIf(alterRowCondition == 'insert'),
    updateIf(alterRowCondition == 'update'),
    deleteIf(alterRowCondition == 'delete')) ~> AlterRow
```

## <a name="next-steps"></a>Nächste Schritte

Nach der Zeilenänderungstransformation können Sie [Ihre Daten in einen Zieldatenspeicher senken](data-flow-sink.md).
