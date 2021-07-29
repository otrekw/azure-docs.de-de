---
title: Grundlegendes zu Device Update for Azure IoT Hub-Gerätegruppen | Microsoft-Dokumentation
description: Erfahren Sie, wie Gerätegruppen verwendet werden.
author: aysancag
ms.author: aysancag
ms.date: 2/09/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: aa3ee7e5b92044c35ac1856309f7265ad06923a1
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110083528"
---
# <a name="device-groups"></a>Gerätegruppen

Bei einer Gerätegruppe handelt es sich um eine Sammlung verwandter Geräte. Gerätegruppen bieten eine Möglichkeit zum Skalieren von Bereitstellungen auf viele Geräten. Jedes Gerät gehört jeweils zu genau einer Gerätegruppe.
Sie können mehrere Gerätegruppen erstellen, um Ihre Geräte zu organisieren. Contoso könnte z. B. die Gerätegruppe „Flighting“ für die Geräte im Testlabor und die Gerätegruppe „Evaluation“ für die Geräte verwenden, die das Außendienstteam in der Einsatzzentrale verwendet. Darüber hinaus kann Contoso die Produktionsgeräte nach geografischen Regionen gruppieren, sodass die Geräte nach einem Zeitplan aktualisiert werden können, der mit den regionalen Zeitzonen übereinstimmt. 


## <a name="using-device-or-module-twin-tag-for-device-group-creation"></a>Verwenden des Geräte- oder Modulzwillingtags für die Erstellung von Gerätegruppen

Tags ermöglichen Benutzern das Gruppieren von Geräten. Geräte müssen einen ADUGroup-Schlüssel und einen Wert in Ihrem Geräte- oder Modulzwilling haben, damit sie gruppiert werden können.

### <a name="device-or-module-twin-tag-format"></a>Format des Geräte- oder Modulzwillingtags

```markdown
"tags": {
  "ADUGroup": "<CustomTagValue>"
}
```


## <a name="uncategorized-device-group"></a>Nicht kategorisierte Gerätegruppe

Nicht kategorisiert („uncategorized“) ist ein reserviertes Wort, das zum Gruppieren von Geräten verwendet wird, die:
- Kein ADUGroup-Geräte- oder Modulzwillingtag haben.
- Ein ADUGroup-Geräte- oder Modulzwillingtag haben, aber keine Gruppe mit diesem Gruppennamen erstellt wird.

Betrachten Sie zum Beispiel die unten aufgeführten Geräte mit ihren Gerätezwillingtags:

```markdown
"deviceId": "Device1",
"tags": {
  "ADUGroup": "Group1"
}
```

```markdown
"deviceId": "Device2",
"tags": {
  "ADUGroup": "Group1"
}
```

```markdown
"deviceId": "Device3",
"tags": {
  "ADUGroup": "Group2"
}
```

```markdown
"deviceId": "Device4",
```

Im Folgenden finden Sie die Geräte und die möglichen Gruppen, die für sie erstellt werden können.

|Sicherungsmedium |Group  |
|-----------|--------------|
|Device1    |Gruppe1|
|Device2    |Gruppe1|
|Device3    |Gruppe2|
|Device4    |Nicht kategorisiert|



## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Gerätegruppe](./create-update-group.md)
