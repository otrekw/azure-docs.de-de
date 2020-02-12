---
title: Erstellen von Webhooks für Regeln in Azure IoT Central | Microsoft-Dokumentation
description: Erstellen Sie Webhooks in Azure IoT Central, um andere Anwendungen automatisch über ausgelöste Regeln zu informieren.
author: viv-liu
ms.author: viviali
ms.date: 12/02/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: corywink
ms.openlocfilehash: db4e48a7bff9127810b051a9ab63bbe9d78cf6da
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77022425"
---
# <a name="create-webhook-actions-on-rules-in-azure-iot-central"></a>Erstellen von Webhookaktionen für Regeln in Azure IoT Central

*Dieses Thema gilt für Generatoren und Administratoren.*

Webhooks ermöglichen es Ihnen, Ihre IoT Central-App mit anderen Anwendungen und Diensten für Remoteüberwachung und Benachrichtigung zu verbinden. Durch Webhooks werden automatisch andere Anwendungen und Dienste benachrichtigt, die Sie verbinden, wenn eine Regel in Ihrer IoT Central-App ausgelöst wird. Ihre IoT Central-App sendet eine POST-Anforderung an den HTTP-Endpunkt der anderen Anwendung, sobald eine Regel ausgelöst wird. Die Nutzlast enthält Details zu Gerät und Regeltriggern.

## <a name="set-up-the-webhook"></a>Einrichten des Webhooks

In diesem Beispiel stellen Sie mithilfe von Webhooks eine Verbindung mit RequestBin her, damit Sie Benachrichtigungen erhalten, sobald Regeln ausgelöst werden.

1. Öffnen Sie [RequestBin](https://requestbin.net/).

1. Erstellen Sie ein neues RequestBin-Element, und kopieren Sie die **Bin-URL**.

1. Erstellen Sie eine [Telemetrieregel](tutorial-create-telemetry-rules.md). Speichern Sie die Regel, und fügen Sie eine neue Aktion hinzu.

    ![Bildschirm für die Webhookerstellung](media/howto-create-webhooks/webhookcreate.png)

1. Wählen Sie die Webhookaktion, geben Sie einen Anzeigenamen an, und fügen Sie die Bin-URL als Rückruf-URL ein.

1. Speichern Sie die Regel.

Wenn jetzt die Regel ausgelöst wird, wird eine neue Anforderung in RequestBin angezeigt.

## <a name="payload"></a>Nutzlast

Wenn eine Regel ausgelöst wird, erfolgt eine HTTP-POST-Anforderung an die Rückruf-URL, die eine JSON-Nutzlast mit den Telemetriedaten, dem Gerät, der Regel und den Anwendungsdetails enthält. Die Nutzlast könnte so ähnlich wie hier aussehen:

```json
{
    "id": "<id>",
    "displayName": "Webhook 1",
    "timestamp": "2019-10-24T18:27:13.538Z",
    "rule": {
        "id": "<id>",
        "displayName": "High temp alert",
        "enabled": true
    },
    "device": {
        "id": "mx1",
        "displayName": "MXChip IoT DevKit - mx1",
        "instanceOf": "<device-template-id>",
        "simulated": true,
        "provisioned": true,
        "approved": true
    },
    "data": [{
        "@id": "<id>",
        "@type": ["Telemetry"],
        "name": "temperature",
        "displayName": "Temperature",
        "value": 66.27310467496761,
        "interfaceInstanceName": "sensors"
    }],
    "application": {
        "id": "<id>",
        "displayName": "x - Store Analytics Checkout---PnP",
        "subdomain": "<subdomain>",
        "host": "<host>"
    }
}
```

## <a name="known-limitations"></a>Bekannte Einschränkungen

Derzeit gibt es keine programmatische Möglichkeit, sich über eine API von diesen Webhooks an- oder abzumelden.

Wenn Sie Ideen haben, wie diese Funktion verbessert werden können, veröffentlichen Sie Ihre Vorschläge in unserem [UserVoice-Forum](https://feedback.azure.com/forums/911455-azure-iot-central).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie Webhooks einrichten und verwenden, empfiehlt es sich, als nächsten Schritt das [Konfigurieren von Azure Monitor-Aktionsgruppen](howto-use-action-groups.md) zu erkunden.
