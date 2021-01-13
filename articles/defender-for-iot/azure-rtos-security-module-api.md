---
title: 'Sicherheitsmodul für Azure RTOS: API'
description: Referenz-API des Sicherheitsmoduls für Azure RTOS.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2020
ms.author: mlottner
ms.openlocfilehash: 8191d68f701a3be6a37d8623d2d900f6aa6c1851
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90931468"
---
# <a name="security-module-for-azure-rtos-api-preview"></a>Sicherheitsmodul für Azure RTOS: API (Vorschau)

Diese API ist ausschließlich für die Verwendung mit dem Sicherheitsmodul für Azure RTOS gedacht. Weitere Ressourcen finden Sie in der [GitHub-Ressource zum Sicherheitsmodul für Azure RTOS](https://github.com/azure-rtos/azure-iot-preview/releases). 

## <a name="enable-security-module-for-azure-rtos"></a>Aktivieren des Sicherheitsmoduls für Azure RTOS

**nx_azure_iot_security_module_enable**

### <a name="prototype"></a>Prototyp

```c
UINT nx_azure_iot_security_module_enable(NX_AZURE_IOT *nx_azure_iot_ptr);
```

### <a name="description"></a>Beschreibung

Diese Routine aktiviert das Subsystem des Azure IoT-Sicherheitsmoduls. Ein interner Zustandsautomat verwaltet die Sammlung von Sicherheitsereignissen und sendet sie an Azure IoT Hub. Nur eine NX_AZURE_IOT_SECURITY_MODULE-Instanz ist erforderlich, um die Datensammlung zu verwalten.

### <a name="parameters"></a>Parameter

| Name | Beschreibung |
|---------|---------|
| nx_azure_iot_ptr  [in]    | Ein Zeiger auf ein `NX_AZURE_IOT`.  |

### <a name="return-values"></a>Rückgabewerte

|Rückgabewerte  |Beschreibung |
|---------|---------|
|NX_AZURE_IOT_SUCCESS|   Azure IoT-Sicherheitsmodul erfolgreich aktiviert.     |
|NX_AZURE_IOT_FAILURE   |  Fehler beim Aktivieren des Azure IoT-Sicherheitsmoduls wegen eines internen Fehlers.    |
|NX_AZURE_IOT_INVALID_PARAMETER   |  Das Sicherheitsmodul erfordert eine gültige #NX_AZURE_IOT-Instanz.      |

### <a name="allowed-from"></a>Zulässig von

Threads

## <a name="disable-azure-iot-security-module"></a>Azure IoT-Sicherheitsmodul deaktivieren

**nx_azure_iot_security_module_disable**


### <a name="prototype"></a>Prototyp

```c
UINT nx_azure_iot_security_module_disable(NX_AZURE_IOT *nx_azure_iot_ptr);
```

### <a name="description"></a>Beschreibung

Diese Routine deaktiviert das Subsystem des Azure IoT-Sicherheitsmoduls.

### <a name="parameters"></a>Parameter

| Name | Beschreibung |
|---------|---------|
| nx_azure_iot_ptr  [in]    | Ein Zeiger auf `NX_AZURE_IOT`. Wenn NULL, wird die Singletoninstanz deaktiviert. |

### <a name="return-values"></a>Rückgabewerte

|Rückgabewerte  |Beschreibung |
|---------|---------|
|NX_AZURE_IOT_SUCCESS     |   Erfolgreich, wenn das Azure IoT-Sicherheitsmodul erfolgreich deaktiviert wurde.      |
|NX_AZURE_IOT_INVALID_PARAMETER   |  Azure IoT Hub-Instanz unterscheidet sich von der zusammengesetzten Singletoninstanz.       |
|NX_AZURE_IOT_FAILURE    |  Fehler beim Deaktivieren des Azure IoT-Sicherheitsmoduls wegen eines internen Fehlers.       |

### <a name="allowed-from"></a>Zulässig von

Threads


## <a name="next-steps"></a>Nächste Schritte

Um mehr über die ersten Schritte mit dem Sicherheitsmodul für Azure RTOS zu erfahren, lesen Sie die folgenden Artikel:

- Lesen Sie die [Übersicht](iot-security-azure-rtos.md) über das RTOS-Sicherheitsmodul für Defender für IoT.