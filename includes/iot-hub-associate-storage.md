---
title: Datei einfügen
description: include file
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 01/22/2019
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: ecd7e070c353d706865b245427758c11a7bbfa5d
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111896949"
---
## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Zuweisen eines Azure Storage-Kontos zu IoT Hub

Sie müssen über ein Azure Storage-Konto verfügen, das mit Ihrem IoT-Hub verbunden ist. Informationen zum Erstellen eines Speicherkontos finden Sie unter [Erstellen eines Speicherkontos](../articles/storage/common/storage-account-create.md). Wenn Sie Azure Storage-Konto einem IoT Hub zuordnen, generiert der IoT Hub einen SAS-URI. Ein Gerät kann diesen SAS-URI für das sichere Hochladen einer Datei zu einem Blobcontainer verwenden. Der IoT Hub-Dienst und die Geräte-SDKs koordinieren den Prozess, der die SAS-URI generiert und für ein Gerät verfügbar macht, sodass er beim Hochladen einer Datei verwendet werden kann.

## <a name="create-a-container"></a>Das Erstellen eines Containers

Führen Sie die folgenden Schritte aus, um einen Blobcontainer für Ihr Speicherkonto zu erstellen:

1. Wählen Sie im linken Bereich Ihres Speicherkontos unter **Datenspeicherung** die Option **Container** aus.
1. Wählen Sie auf dem Container-Blatt die Option **+ Container** aus.
1. Geben Sie im Bereich **Neuer Container**, der geöffnet wird, einen Namen für Ihren Container ein, und wählen Sie **Erstellen** aus.

Nach der Erstellung des Containers folgen Sie den Anleitungen in [Konfigurieren des Dateiuploads im Azure-Portal](../articles/iot-hub/iot-hub-configure-file-upload.md). Stellen Sie sicher, dass Ihrem IoT-Hub ein Blobcontainer zugeordnet ist und die Dateibenachrichtigungen aktiviert sind.

![Aktivieren von Dateibenachrichtigungen im Portal](./media/iot-hub-associate-storage/file-notifications-vs2019.png)
