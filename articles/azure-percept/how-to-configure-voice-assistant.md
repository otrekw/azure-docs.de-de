---
title: Konfigurieren der Sprach-Assistenten-Anwendung mithilfe von Azure IoT Hub
description: Konfigurieren der Sprach-Assistenten-Anwendung mithilfe von Azure IoT Hub
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/15/2021
ms.custom: template-how-to
ms.openlocfilehash: ec3e06b2d161785b5e6978cdf4cc6415fc0eb592
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660401"
---
# <a name="configure-voice-assistant-application-using-azure-iot-hub"></a>Konfigurieren der Sprach-Assistenten-Anwendung mithilfe von Azure IoT Hub

In diesem Artikel wird beschrieben, wie Sie Ihre Sprach-Assistenten-Anwendung mithilfe von IoT Hub konfigurieren. Ein schrittweises Tutorial, das Sie durch den Prozess der Erstellung eines Sprach-Assistenten mithilfe von Demovorlagen führt, finden Sie unter [Erstellen eines Sprach-Assistenten ohne Code mit Azure Percept Studio und Azure Percept-Audio](./tutorial-no-code-speech.md).

## <a name="update-your-voice-assistant-configuration"></a>Aktualisieren Ihrer Sprach-Assistenten-Konfiguration

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_Azure_Iothub=aduprod&microsoft_azure_marketplace_ItemHideKey=Microsoft_Azure_ADUHidden#home), und geben Sie **IoT Hub** in die Suchleiste ein. Klicken Sie auf das Symbol, um die Seite „IoT Hub“ zu öffnen.

1. Wählen Sie auf der Seite „IoT Hub“ den IOT Hub aus, in dem Ihr Gerät bereitgestellt wurde.

1. Wählen Sie im linken Navigationsmenü unter **Automatische Geräteverwaltung** den Eintrag **IoT Edge** aus, um alle mit Ihrem IoT Hub verbundenen Geräte anzuzeigen.

1. Wählen Sie das Gerät aus, auf dem Ihre Sprach-Assistenten-Anwendung bereitgestellt wurde.

1. Klicken Sie auf **Module festlegen**.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/set-modules.png" alt-text="Abbildung.":::

1. Vergewissern Sie sich, dass der folgende Eintrag im Abschnitt **Anmeldeinformationen für Container Registry** vorhanden ist. Fügen Sie Anmeldeinformationen hinzu, falls erforderlich.

    |Name|Adresse|Username|Kennwort|
    |----|-------|--------|--------|
    |azureedgedevices|azureedgedevices.azurecr.io|devkitprivatepreviewpull|

1. Wählen Sie im Abschnitt **IoT Edge-Module** das Modul **azureearspeechclientmodule** aus.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/modules.png" alt-text="Abbildung.":::

1. Klicken Sie auf die Registerkarte **Moduleinstellungen**. Überprüfen Sie die folgenden Konfiguration:

    |Image-URI|Neustartrichtlinie|Gewünschter Status|
    |---------|--------------|--------------|
    |azureedgedevices.azurecr.io/azureearspeechclientmodule:preload-devkit |immer|„Wird ausgeführt“|

    Wenn Ihre Einstellungen nicht übereinstimmen, bearbeiten Sie sie, und klicken Sie auf **Aktualisieren**.

1. Klicken Sie auf die Registerkarte **Umgebungsvariablen**. Stellen Sie sicher, dass keine Umgebungsvariablen definiert sind.

1. Klicken Sie auf die Registerkarte **Optionen für die Containererstellung**. Überprüfen Sie, ob Ihre **HostConfig**-Einstellungen den unten angezeigten entsprechen. Wenn nicht, aktualisieren Sie Ihre Einstellungen.

    ```
    {
        "HostConfig": {
            "Privileged": true,
            "Binds": [
                "/dev:/dev"
            ]
        }
    }
    ```

1. Klicken Sie auf die Registerkarte **Einstellungen für Modulzwilling**. Aktualisieren Sie den Abschnitt **speechConfigs** wie folgt:

    ```
    "speechConfigs": {
        "appId": "<Application id for custom command project>",
        "key": "<Speech Resource key for custom command project>",
        "region": "<Region for the speech service>",
        "keywordModelUrl": "https://aedspeechscenarios.blob.core.windows.net/keyword-tables/computer.table",
        "keyword": "computer"
    }
    ```

    > [!NOTE]
    > Das oben verwendete Schlüsselwort ist ein standardmäßig öffentlich verfügbares Schlüsselwort. Wenn Sie Ihr eigenes verwenden möchten, können Sie Ihr eigenes benutzerdefiniertes Schlüsselwort hinzufügen, indem Sie eine erstellte Tabellendatei in den Blob Storage hochladen. Blob Storage muss entweder mit anonymem Containerzugriff oder mit anonymem Blobzugriff konfiguriert werden.

## <a name="how-to-find-out-appid-key-and-region"></a>Herausfinden von App-ID (appId), Schlüssel (key) und Region (region)

Wechseln Sie zu [Speech Studio](https://speech.microsoft.com/), um Ihre **appID**, den **key** und die **region** herauszufinden:

1. Melden Sie sich an, und wählen Sie die entsprechende Sprachressource aus.
1. Klicken Sie auf der Startseite von **Speech Studio** unter **Sprach-Assistenten** auf **Benutzerdefinierte Befehle**.
1. Wählen Sie Ihr Zielprojekt aus.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/project.png" alt-text="Abbildung.":::

1. Klicken Sie im linken Menübereich auf **Einstellungen**.
1. **appID** und **key** befinden sich auf der Registerkarte **Allgemein** e Einstellungen.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/general-settings.png" alt-text="Abbildung.":::

1. Um Ihre **region** zu finden, öffnen Sie die Registerkarte **LUIS-Ressourcen** in den Einstellungen. Der Abschnitt **Erstellungsressource** enthält Regionsinformationen.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/luis-resources.png" alt-text="Abbildung.":::

1. Nachdem Sie Ihre **speechConfigs**-Informationen eingegeben haben, klicken Sie auf **Aktualisieren**.

1. Klicken Sie oben auf der Seite **Module festlegen** auf die Registerkarte **Routen**. Stellen Sie sicher, dass Sie eine Route mit folgendem Wert besitzen:

    ```
    FROM /messages/modules/azureearspeechclientmodule/outputs/* INTO $upstream
    ```

    Fügen Sie die Route hinzu, wenn sie nicht vorhanden ist.

1. Klicken Sie auf **Überprüfen und erstellen**.

1. Klicken Sie auf **Erstellen**.

## <a name="next-steps"></a>Nächste Schritte

Kehren Sie nach dem Aktualisieren Ihrer Sprach-Assistenten-Konfiguration zur Demo in Azure Percept Studio zurück, um mit der Anwendung zu interagieren.