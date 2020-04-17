---
title: Vorlagenfunktionen – Datumsangaben
description: Hier werden die Funktionen beschrieben, die in einer Azure Resource Manager-Vorlage zum Arbeiten mit Datumsangaben verwendet werden können.
ms.topic: conceptual
ms.date: 04/06/2020
ms.openlocfilehash: 75234a142b9f2fbe61c337bfeb378b47534bac79
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80986207"
---
# <a name="date-functions-for-arm-templates"></a>Datumsfunktionen für ARM-Vorlagen

Resource Manager stellt die folgenden Funktionen für das Arbeiten mit Datumsangaben in Ihren ARM-Vorlagen (Azure Resource Manager) bereit:

* [dateTimeAdd](#datetimeadd)
* [utcNow](#utcnow)

## <a name="datetimeadd"></a>dateTimeAdd

`dateTimeAdd(base, duration, [format])`

Hiermit wird einem datetime-Basiswert eine Dauer hinzugefügt.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | type | BESCHREIBUNG |
|:--- |:--- |:--- |:--- |
| base | Ja | Zeichenfolge | Der datetime-Startwert für die Hinzufügung. Verwenden Sie das [Zeitstempelformat nach ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). |
| duration | Ja | Zeichenfolge | Der Zeitwert, der der Basis hinzugefügt werden soll. Dieser darf kein negativer Wert sein. Verwenden Sie das [Format der Zeitspanne nach ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Durations). |
| format | Nein | Zeichenfolge | Das Ausgabeformat für das datetime-Ergebnis. Ist dieser Wert nicht angegeben, wird das Format des Basiswerts verwendet. Verwenden Sie entweder [Standardformatzeichenfolgen](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) oder [benutzerdefinierte Formatzeichenfolgen](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). |

### <a name="return-value"></a>Rückgabewert

Der datetime-Wert, der durch Hinzufügen der Dauer zum Basiswert erhalten wird.

### <a name="examples"></a>Beispiele

In der folgenden Beispielvorlage werden verschiedene Möglichkeiten zum Hinzufügen von time-Werten veranschaulicht.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "baseTime":{
            "type":"string",
            "defaultValue": "[utcNow('u')]"
        }
    },
    "variables": {
        "add3Years": "[dateTimeAdd(parameters('baseTime'), 'P3Y')]",
        "subtract9Days": "[dateTimeAdd(parameters('baseTime'), '-P9D')]",
        "add1Hour": "[dateTimeAdd(parameters('baseTime'), 'PT1H')]"
    },
    "resources": [],
    "outputs": {
        "add3Years": {
            "value": "[variables('add3Years')]",
            "type": "string"
        },
        "subtract9Days": {
            "value": "[variables('subtract9Days')]",
            "type": "string"
        },
        "add1Hour": {
            "value": "[variables('add1Hour')]",
            "type": "string"
        },
    }
}
```

Wenn die obige Vorlage mit dem time-Basiswert `2020-04-07 14:53:14Z` bereitgestellt wird, erhalten wir folgende Ausgabe:

| Name | type | Wert |
| ---- | ---- | ----- |
| add3Years | String | 07.04.2023 14:53:14 |
| subtract9Days | String | 29.03.2020 14:53:14 |
| add1Hour | String | 07.04.2020 15:53:14 |

Mit der nächsten Beispielvorlage wird veranschaulicht, wie die Startzeit für einen Automation-Zeitplan festgelegt wird.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "omsAutomationAccountName": {
            "type": "string",
            "defaultValue": "demoAutomation",
            "metadata": {
                "description": "Use an existing Automation account."
            }
        },
        "scheduleName": {
            "type": "string",
            "defaultValue": "demoSchedule1",
            "metadata": {
                "description": "Name of the new schedule."
            }
        },
        "baseTime":{
            "type":"string",
            "defaultValue": "[utcNow('u')]",
            "metadata": {
                "description": "Schedule will start one hour from this time."
            }
        }
    },
    "variables": {
        "startTime": "[dateTimeAdd(parameters('baseTime'), 'PT1H')]"
    },
    "resources": [
        {
            "name": "[concat(parameters('omsAutomationAccountName'), '/', parameters('scheduleName'))]",
            "type": "microsoft.automation/automationAccounts/schedules",
            "apiVersion": "2015-10-31",
            "location": "eastus2",
            "tags": {
            },
            "properties": {
                "description": "Demo Scheduler",
                "startTime": "[variables('startTime')]",
                "isEnabled": "true",
                "interval": 1,
                "frequency": "hour"
            }
        }
    ],
    "outputs": {
    }
}
```

## <a name="utcnow"></a>utcNow

`utcNow(format)`

Gibt den aktuellen datetime-Wert (UTC) im festgelegten Format zurück. Wenn kein Format angegeben wird, wird das ISO 8601-Format (JJJJMMTTThhmmssZ) verwendet. **Diese Funktion kann nur für den Standardwert eines Parameters verwendet werden.**

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | type | BESCHREIBUNG |
|:--- |:--- |:--- |:--- |
| format |Nein |Zeichenfolge |Der als URI codierte Wert, der in eine Zeichenfolge konvertiert werden soll. Verwenden Sie entweder [Standardformatzeichenfolgen](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) oder [benutzerdefinierte Formatzeichenfolgen](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). |

### <a name="remarks"></a>Bemerkungen

Sie können diese Funktion nur in einem Ausdruck für den Standardwert eines Parameters verwenden. Wenn diese Funktion an einer anderen Stelle in einer Vorlage verwendet wird, wird ein Fehler zurückgegeben. Die Funktion ist in anderen Teilen der Vorlage nicht zulässig, da bei jedem Aufruf ein anderer Wert zurückgegeben wird. Das Bereitstellen der gleichen Vorlage mit den gleichen Parametern würde nicht zuverlässig zu den gleichen Ergebnissen führen.

Wenn Sie die [Option zum erneuten Bereitstellen einer vorherigen erfolgreichen Bereitstellung](rollback-on-error.md) verwenden und die vorherige Bereitstellung einen Parameter enthält, der die Funktion „utcNow“ nutzt, wird dieser Parameter nicht erneut ausgewertet. Stattdessen wird der Parameterwert der vorherigen Bereitstellung in der Rollbackbereitstellung automatisch wiederverwendet.

Seien Sie vorsichtig, wenn Sie eine Vorlage erneut bereitstellen, die die utcNow-Funktion für einen Standardwert nutzt. Wenn Sie die erneute Bereitstellung durchführen und keinen Wert für den Parameter bereitstellen, wird die Funktion erneut ausgewertet. Wenn Sie eine vorhandene Ressource aktualisieren möchten, anstatt eine neue zu erstellen, übergeben Sie den Parameterwert aus der früheren Bereitstellung.

### <a name="return-value"></a>Rückgabewert

Der aktuelle UTC-datetime-Wert

### <a name="examples"></a>Beispiele

In der folgenden Beispielvorlage werden verschiedene Formate für den datetime-Wert veranschaulicht.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "utcValue": {
            "type": "string",
            "defaultValue": "[utcNow()]"
        },
        "utcShortValue": {
            "type": "string",
            "defaultValue": "[utcNow('d')]"
        },
        "utcCustomValue": {
            "type": "string",
            "defaultValue": "[utcNow('M d')]"
        }
    },
    "resources": [
    ],
    "outputs": {
        "utcOutput": {
            "type": "string",
            "value": "[parameters('utcValue')]"
        },
        "utcShortOutput": {
            "type": "string",
            "value": "[parameters('utcShortValue')]"
        },
        "utcCustomOutput": {
            "type": "string",
            "value": "[parameters('utcCustomValue')]"
        }
    }
}
```

Die Ausgabe des vorherigen Beispiels variiert bei jeder Bereitstellung. Sie sollte jedoch folgender ähneln:

| Name | type | Wert |
| ---- | ---- | ----- |
| utcOutput | Zeichenfolge | 20190305T175318Z |
| utcShortOutput | Zeichenfolge | 03/05/2019 |
| utcCustomOutput | Zeichenfolge | 3 5 |

Im folgenden Beispiel wird gezeigt, wie ein Wert der Funktion beim Festlegen eines Tagwerts verwendet wird.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "utcShort": {
            "type": "string",
            "defaultValue": "[utcNow('d')]"
        },
        "rgName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "name": "[parameters('rgName')]",
            "location": "westeurope",
            "tags":{
                "createdDate": "[parameters('utcShort')]"
            },
            "properties":{}
        }
    ],
    "outputs": {
        "utcShort": {
            "type": "string",
            "value": "[parameters('utcShort')]"
        }
    }
}
```