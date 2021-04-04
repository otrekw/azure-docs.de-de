---
title: Benutzeroberflächenelement „SizeSelector“
description: Hier wird das Benutzeroberflächenelement Microsoft.Compute.SizeSelector für das Azure-Portal beschrieben. Es wird zum Auswählen der Größe eines virtuellen Computers verwendet.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: d6408f8c08694ae681d302ae35f5778894091733
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "87063656"
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Benutzeroberflächenelement „Microsoft.Compute.SizeSelector“

Ein Steuerelement zum Auswählen einer Größe für VM-Instanzen.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

Der Benutzer sieht eine Auswahl mit Standardwerten aus der Elementdefinition.

![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft-compute-sizeselector.png)

Nach dem Auswählen des Steuerelements sieht der Benutzer eine erweiterte Ansicht der verfügbaren Größen.

![Microsoft.Compute.SizeSelector, erweitert](./media/managed-application-elements/microsoft-compute-sizeselector-expanded.png)

## <a name="schema"></a>Schema

```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": [],
    "numAvailabilityZonesRequired": 3,
    "zone": "3"
  },
  "options": {
    "hideDiskTypeFilter": false
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
"Standard_D1"
```

## <a name="remarks"></a>Bemerkungen

- `recommendedSizes` muss mindestens eine Größe aufweisen. Die erste empfohlene Größe wird als Standardwert verwendet. Die Liste der verfügbaren Größen ist nicht nach dem empfohlenen Status sortiert. Der Benutzer kann die Spalte auswählen, um sie nach dem empfohlenen Status zu sortieren.
- Wenn eine empfohlene Größe am ausgewählten Standort nicht verfügbar ist, wird die Größe automatisch übersprungen. Stattdessen wird die nächste empfohlene Größe verwendet.
- `constraints.allowedSizes` und `constraints.excludedSizes` sind optional, können jedoch nicht gleichzeitig angegeben werden. Die Liste der verfügbaren Größen kann mithilfe der Informationen unter [List available virtual machine sizes for a subscription](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region) (Auflisten verfügbarer VM-Größen für ein Abonnement) abgerufen werden. Alle nicht in `constraints.allowedSizes` angegebenen Größen werden ausgeblendet, und nicht in `constraints.excludedSizes` angegebene Größen werden angezeigt.
- `osPlatform` muss angegeben werden. Mögliche Optionen: **Windows** oder **Linux**. Damit werden die Hardwarekosten der virtuellen Computer ermittelt.
- `imageReference` wird für Images von Erstanbietern ausgelassen, für Images von Drittanbietern jedoch angegeben. Damit werden die Softwarekosten der virtuellen Computer ermittelt.
- `count` dient zum Festlegen des entsprechenden Multiplikators für das Element. Das Element unterstützt einen statischen Wert wie **2** oder einen dynamischen Wert aus einem anderen Element, etwa `[steps('step1').vmCount]`. Der Standardwert ist **1**.
- Für `numAvailabilityZonesRequired` kann 1, 2 oder 3 festgelegt werden.
- Standardmäßig ist `hideDiskTypeFilter`**false**. Mit dem Filter für den Datenträgertyp kann der Benutzer alle Datenträgertypen oder nur SSD anzeigen.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
