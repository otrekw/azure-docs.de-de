---
title: Funktionen in Azure Monitor-Protokollabfragen
description: In diesem Artikel wird die Verwendung von Funktionen beschrieben, um in Azure Monitor aus einer Abfrage eine andere Protokollabfrage aufzurufen.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/19/2021
ms.openlocfilehash: fecede1d582232ebc75878a687a24818031f0d80
ms.sourcegitcommit: 6686a3d8d8b7c8a582d6c40b60232a33798067be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107752851"
---
# <a name="functions-in-azure-monitor-log-queries"></a>Funktionen in Azure Monitor-Protokollabfragen
Eine Funktion ist eine Protokollabfrage in Azure Monitor, die in anderen Protokollabfragen verwendet werden kann, als wäre sie ein Befehl. Funktionen ermöglichen es Entwicklern, Lösungen für verschiedene Kunden bereitzustellen und Abfragelogik in Ihrer eigenen Umgebung wiederzuverwenden. Dieser Artikel enthält Details zur Verwendung von Funktionen und zum Erstellen eigener Funktionen.

## <a name="types-of-functions"></a>Funktionstypen
In Azure Monitor gibt es zwei Typen von Funktionen:

- **Lösungsfunktion:** Vordefinierte Funktionen, die in Azure Monitor enthalten sind. Diese sind in allen Log Analytics-Arbeitsbereichen verfügbar und können nicht geändert werden.
- **Arbeitsbereichsfunktionen:** Funktionen, die in einem bestimmten Log Analytics-Arbeitsbereich installiert sind und vom Benutzer geändert und gesteuert werden können.

## <a name="viewing-functions"></a>Anzeigen von Funktionen
Sie können Lösungsfunktionen und Arbeitsbereichsfunktionen im aktuellen Arbeitsbereich auf der Registerkarte **Funktionen** im linken Bereich eines Log Analytics-Arbeitsbereichs anzeigen. Verwenden Sie die Schaltfläche **Filter**, um die in der Liste enthaltenen Funktionen zu filtern, und **Gruppieren nach**, um deren Gruppierung zu ändern. Geben Sie eine Zeichenfolge in das **Suchfeld** ein, um nach einer bestimmten Funktion zu suchen. Zeigen Sie auf eine Funktion, um Details dazu anzuzeigen, einschließlich einer Beschreibung und den Parameter.

:::image type="content" source="media/functions/view-functions.png" alt-text="Anzeigefunktion" lightbox="media/functions/view-functions.png":::

## <a name="use-a-function"></a>Verwenden einer Funktion
Verwenden Sie eine Funktion in einer Abfrage, indem Sie ihren Namen mit Werten für alle Parameter eingeben, so wie Sie einen Befehl eingeben würden. Die Ausgabe der Funktion kann entweder als Ergebnisse zurückgegeben oder an einen anderen Befehl weitergesendet werden.

Fügen Sie der aktuellen Abfrage eine Funktion hinzu, indem Sie auf ihren Namen doppelklicken oder darauf zeigen und **Im Editor verwenden** auswählen. Funktionen im Arbeitsbereich werden auch in IntelliSense einbezogen, wenn Sie eine Abfrage eingeben. 

Wenn eine Abfrage Parameter erfordert, geben Sie sie mit der Syntax an: `function_name(param1,param2,...)`.

:::image type="content" source="media/functions/function-use.png" alt-text="Verwenden einer Funktion" lightbox="media/functions/function-use.png":::

## <a name="create-a-function"></a>Erstellen einer Funktion
Um eine Funktion aus der aktuellen Abfrage im Editor zu erstellen, wählen Sie **Speichern** und dann **Als Funktion speichern** aus. 

:::image type="content" source="media/functions/function-save.png" alt-text="Erstellen einer Funktion" lightbox="media/functions/function-save.png":::

Erstellen Sie eine Funktion mit Log Analytics, indem Sie im Azure-Portal auf **Speichern** klicken und dann die Informationen in der folgenden Tabelle angeben.

| Einstellung | Beschreibung |
|:---|:---|
| Funktionsname  | Name für die Funktion. Er darf kein Leerzeichen oder Sonderzeichen enthalten. Er darf auch nicht mit einem Unterstrich (_) beginnen, da dieses Zeichen für Projektmappenfunktionen reserviert ist. |
| Legacykategorie | Benutzerdefinierte Kategorie, um Filter- und Gruppierungsfunktionen zu unterstützen.   |
| Als Computergruppe speichern | Diese Abfrage als [Computergruppe](computer-groups.md) speichern.  |
| Parameter | Fügen Sie einen Parameter für jede Variable in der Funktion hinzu, die einen Wert erfordert, wenn sie verwendet wird. Weitere Informationen finden Sie unter [Funktionsparameter](#function-parameters). |

:::image type="content" source="media/functions/function-details.png" alt-text="Funktionsdetails" lightbox="media/functions/function-details.png":::

## <a name="function-parameters"></a>Funktionsparameter 
Sie können einer Funktion Parameter hinzufügen, sodass Sie beim Aufruf Werte für bestimmte Variablen bereitstellen können. Dadurch kann dieselbe Funktion in verschiedenen Abfragen verwendet werden, die jeweils unterschiedliche Werte für die Parameter bereitstellen. Die Parameter werden durch folgende Eigenschaften definiert.

| Einstellung | Beschreibung |
|:---|:---|
| type  | Datentyp für den Wert. |
| Name  | Namen für den Parameter. Dies ist der Name, der in der Abfrage verwendet werden muss, um durch den Parameterwert zu ersetzen.  |
| Standardwert | Der Wert, der für den Parameter verwendet werden soll, wenn kein Wert angegeben wird. |

Parameter werden so geordnet, wie sie mit allen Parametern erstellt werden, deren Standardwert nicht vor den Parametern mit einem Standardwert positioniert ist.

## <a name="working-with-function-code"></a>Arbeiten mit dem Funktionscode
Sie können den Code einer Funktion anzeigen, um einen Einblick in die Funktionsweise zu erhalten oder den Code für eine Arbeitsbereichsfunktion zu ändern. Wählen Sie **Funktionscode laden** aus, um den Funktionscode der aktuellen Abfrage im Editor hinzuzufügen. Wenn Sie ihn einer leeren Abfrage oder der ersten Zeile einer vorhandenen Abfrage hinzufügen, wird der Funktionsname der Registerkarte hinzugefügt. Wenn es eine Arbeitsbereichsfunktion ist, ermöglicht dies die Option zum Bearbeiten der Funktionsdetails.

:::image type="content" source="media/functions/function-code.png" alt-text="Funktionscode laden" lightbox="media/functions/function-code.png":::

## <a name="edit-a-function"></a>Bearbeiten einer Funktion
Bearbeiten Sie die Eigenschaften oder den Code einer Funktion, indem Sie eine neue Abfrage erstellen, den Cursor dann auf den Namen der Funktion bewegen und **Funktionscode laden** auswählen. Nehmen Sie alle Änderungen am Code vor, und wählen Sie **Speichern** und dann **Funktionsdetails bearbeiten** aus. Nehmen Sie alle Änderungen an den Eigenschaften und Parametern der Funktion vor, bevor Sie auf **Speichern** klicken.

:::image type="content" source="media/functions/function-edit.png" alt-text="Funktion bearbeiten" lightbox="media/functions/function-edit.png":::
## <a name="example"></a>Beispiel
Die folgende Beispielfunktion gibt alle Ereignisse im Azure-Aktivitätsprotokoll seit einem bestimmten Datum zurück, die einer bestimmten Kategorie entsprechen. 

Beginnen Sie mit der folgenden Abfrage mit hartcodierten Werten. Dadurch wird überprüft, ob die Abfrage wie erwartet funktioniert.

```Kusto
AzureActivity
| where CategoryValue == "Administrative"
| where TimeGenerated > todatetime("2021/04/05 5:40:01.032 PM")
```

:::image type="content" source="media/functions/example-query.png" alt-text="Beispielfunktion – anfängliche Abfrage" lightbox="media/functions/example-query.png":::

Ersetzen Sie als Nächstes die hartcodierten Werte durch Parameternamen, und speichern Sie dann die Funktion, indem Sie **Speichern** und dann **Als Funktion speichern** auswählen.

```Kusto
AzureActivity
| where CategoryValue == CategoryParam
| where TimeGenerated > DateParam
```

:::image type="content" source="media/functions/example-save-function.png" alt-text="Beispielfunktion - Funktion speichern" lightbox="media/functions/example-save-function.png":::

 Geben Sie die folgenden Werte für die Funktionseigenschaften ein.

| Eigenschaft | Wert |
|:---|:---|
| Funktionsname | AzureActivityByCategory |
| Legacykategorie | Demofunktionen |

Definieren Sie die folgenden Parameter, bevor Sie die Funktion speichern.

| type | Name | Standardwert |
|:---|:---|:---|
| Zeichenfolge   | CategoryParam | „Administrative“ |
| datetime | DateParam     | |

:::image type="content" source="media/functions/example-function-properties.png" alt-text="Beispielfunktion – Funktionseigenschaften" lightbox="media/functions/example-function-properties.png":::

Erstellen Sie eine neue Abfrage, und zeigen Sie die neue Funktion an, indem Sie darauf zeigen. Beachten Sie die Reihenfolge der Parameter, da dies die Reihenfolge ist, in der sie angegeben werden müssen, wenn Sie die Funktion verwenden.

:::image type="content" source="media/functions/example-view-details.png" alt-text="Beispielfunktion – Anzeigen von Details" lightbox="media/functions/example-view-details.png":::

Wählen Sie **Im Editor verwenden** aus, um einer Abfrage die neue Funktion hinzuzufügen, und fügen Sie dann Werte für die Parameter hinzu. Beachten Sie, dass Sie keinen Wert für „CategoryParam“ angeben müssen, da er über einen Standardwert verfügt.

:::image type="content" source="media/functions/example-use-function.png" alt-text="Beispielfunktion – Funktion verwenden" lightbox="media/functions/example-use-function.png":::



## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Schreiben von Azure Monitor-Protokollabfragen finden Sie in folgenden weiteren Lektionen:

- [Zeichenfolgenvorgänge](/azure/data-explorer/kusto/query/samples?&pivots=azuremonitor#string-operations)

