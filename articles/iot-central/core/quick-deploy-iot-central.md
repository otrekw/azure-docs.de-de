---
title: 'Schnellstart: Erstellen und Verwenden einer Azure IoT Central-Anwendung | Microsoft-Dokumentation'
description: 'Schnellstart: Erstellen einer Azure IoT Central-Anwendung und Verbinden Ihres ersten Geräts In diesem Schnellstart wird als IoT-Gerät eine Smartphone-App aus Google Play oder dem Apple App Store verwendet.'
author: dominicbetts
ms.author: dobett
ms.date: 05/27/2021
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 4e40c079c50ccb7f83a4e03d47cf3e9b419870ef
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112288527"
---
# <a name="quickstart---create-an-azure-iot-central-application-and-use-your-smartphone-to-send-telemetry"></a>Schnellstart: Erstellen einer Azure IoT Central-Anwendung und Verwenden Ihres Smartphones zum Senden von Telemetriedaten

In diesem Schnellstart erfahren Sie, wie Sie eine Azure IoT Central-Anwendung erstellen und Ihr erstes Gerät verbinden. Damit Sie schnell loslegen können, installieren Sie eine App auf Ihrem Smartphone, die als Gerät fungieren wird. Die App sendet Telemetriedaten, meldet Eigenschaften und reagiert auf Befehle:

:::image type="content" source="media/quick-deploy-iot-central/overview.png" alt-text="Übersicht über das Schnellstartszenario zum Verbinden einer Smartphone-App mit IoT Central" border="false":::

## <a name="prerequisites"></a>Voraussetzungen

Ein aktives Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

> [!TIP]
> Sie sollten in Ihrem Azure-Abonnement mindestens über Zugriff als **Mitwirkender** verfügen. Wenn Sie das Abonnement selbst erstellt haben, sind Sie automatisch Administrator mit ausreichendem Zugriff. Weitere Informationen finden Sie unter [Was ist die rollenbasierte Zugriffssteuerung in Azure?](../../role-based-access-control/overview.md).

Ein Android- oder iOS-Smartphone, auf dem Sie eine kostenlose App aus einem der offiziellen App-Stores installieren können.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Navigieren Sie zur Buildwebsite für [Azure IoT Central](https://aka.ms/iotcentral). Melden Sie sich dann mit dem persönlichen Microsoft-Konto oder einem Geschäfts-, Schul- oder Unikonto an, das Ihrem Azure-Abonnement zugeordnet ist.

IoT Central stellt verschiedene branchenspezifische Anwendungsvorlagen bereit, um Ihnen den Einstieg zu vereinfachen. In diesem Schnellstart wird die Vorlage **Benutzerdefinierte Anwendung** verwendet, um eine Anwendung von Grund auf neu zu erstellen:

1. Navigieren Sie zur Seite **Erstellen**, und wählen Sie auf der Kachel **Benutzerdefinierte App** die Option **App erstellen** aus:

    :::image type="content" source="media/quick-deploy-iot-central/iotcentralcreate-new-application.png" alt-text="Seite „Eigene IoT-Anwendung erstellen“":::

1. Vergewissern Sie sich auf der Seite **Neue Anwendung**, dass unter **Anwendungsvorlage** die Option **Benutzerdefinierte Anwendung** ausgewählt ist.

1. Azure IoT Central schlägt automatisch einen **Anwendungsnamen** entsprechend der von Ihnen ausgewählten Anwendungsvorlage vor. Geben Sie Ihren eigenen Anwendungsnamen ein, z. B. *Contoso-Schnellstart-App*.

1. Azure IoT Central generiert für Sie außerdem entsprechend dem Anwendungsnamen ein eindeutiges **URL**-Präfix. Über diese URL können Sie auf Ihre Anwendung zugreifen. Sie können dieses URL-Präfix in einen einprägsameren Wert ändern. Diese URL muss eindeutig sein.

    :::image type="content" source="media/quick-deploy-iot-central/iotcentralcreate-custom.png" alt-text="Azure IoT Central-Seite „Anwendung erstellen“":::

1. Behalten Sie für diesen Schnellstart den Tarif **Standard 2** bei.

1. Wählen Sie Ihr Abonnement in der Dropdownliste **Azure-Abonnement** aus.

1. Wählen Sie in der Dropdownliste **Standort** eine Region in Ihrer Nähe aus.

1. Lesen Sie die Geschäftsbedingungen, und wählen Sie **Erstellen** im unteren Bereich der Seite aus. Die IoT Central-Anwendung ist nach einigen Sekunden einsatzbereit:

    :::image type="content" source="media/quick-deploy-iot-central/iotcentral-application.png" alt-text="Azure IoT Central-Anwendung":::

## <a name="register-a-device"></a>Registrieren eines Geräts

Um ein Gerät mit Ihrer IoT Central-Anwendung verbinden zu können, benötigen Sie einige Verbindungsinformationen. Eine einfache Möglichkeit, diese Verbindungsinformationen zu erhalten, besteht in der Registrierung Ihres Geräts.

So registrieren Sie Ihr Gerät

1. Navigieren Sie in IoT Central zur Seite **Geräte**, und wählen Sie **Gerät erstellen** aus:

    :::image type="content" source="media/quick-deploy-iot-central/create-device.png" alt-text="Screenshot der Erstellung eines Geräts in IoT Central":::

1. Übernehmen Sie auf der Seite **Neues Gerät erstellen** die Standardeinstellungen, und wählen Sie dann **Erstellen** aus.

1. Klicken Sie in der Liste der Geräte auf den Gerätenamen:

    :::image type="content" source="media/quick-deploy-iot-central/device-name.png" alt-text="Screenshot mit hervorgehobenem Gerätenamen für die Auswahl":::

1. Wählen Sie auf der Geräteseite **Verbinden** und dann **QR-Code** aus:

    :::image type="content" source="media/quick-deploy-iot-central/device-registration.png" alt-text="Screenshot des QR-Codes zum Verbinden der Smartphone-App":::

Lassen Sie diese Seite geöffnet. Im nächsten Abschnitt scannen Sie diesen QR-Code mithilfe der Smartphone-App, um diese mit IoT Central zu verbinden.

## <a name="connect-your-device"></a>Herstellen einer Verbindung mit Ihrem Gerät

Für einen schnelleren Einsteig wird in diesem Artikel die Smartphone-App **IoT Plug and Play** als IoT-Gerät verwendet. Die App sendet Telemetriedaten, die von den Sensoren des Smartphones gesammelt wurden, reagiert auf Befehle, die von IoT Central ausgeführt werden, und meldet Eigenschaftswerte an IoT Central.

[!INCLUDE [iot-phoneapp-install](../../../includes/iot-phoneapp-install.md)]

So verbinden Sie die App **IoT Plug and Play** mit Ihrer IoT Central-Anwendung

1. Öffnen Sie die App **IoT PnP** auf Ihrem Smartphone.

1. Wählen Sie auf der Willkommensseite **Scan QR code** (QR-Code scannen) aus. Erfassen Sie mit der Smartphonekamera den QR-Code. Warten Sie dann einige Sekunden, bis die Verbindung hergestellt wurde.

1. Auf der Telemetrieseite in der App werden die Daten angezeigt, die die App an IoT Central sendet. Auf der Protokollseite wird angezeigt, dass das Gerät eine Verbindung herstellt, und es sind mehrere Initialisierungsmeldungen zu sehen.

So zeigen Sie die Telemetriedaten von der Smartphone-App in IoT Central an

1. Navigieren Sie in IoT Central zur Seite **Geräte**.

1. Klicken Sie in der Liste der Geräte auf Ihren Gerätenamen, und wählen Sie dann **Übersicht** aus:

    :::image type="content" source="media/quick-deploy-iot-central/iotcentral-telemetry.png" alt-text="Screenshot der Übersichtsseite mit Telemetriediagrammen":::

> [!TIP]
> Die Smartphone-App sendet nur Daten, wenn der Bildschirm aktiviert ist.
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [iot-central-clean-up-resources](../../../includes/iot-central-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie eine IoT Central-Anwendung erstellt und ein Gerät verbunden, das Telemetriedaten sendet. Sie haben in diesem Schnellstart eine Smartphone-App als IoT-Gerät verwendet, das eine Verbindung mit IoT Central herstellt. Wenn Sie sich weiter über IoT Central informieren möchten, sehen Sie sich die folgende Schnellstartanleitung an:

> [!div class="nextstepaction"]
> [Hinzufügen einer Regel zu Ihrer IoT Central-Anwendung](./quick-configure-rules.md)
