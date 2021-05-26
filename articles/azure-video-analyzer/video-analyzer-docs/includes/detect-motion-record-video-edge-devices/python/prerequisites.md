---
author: Juliako
ms.service: azure-video-analyzer
ms.topic: include
ms.date: 04/07/2021
ms.author: juliako
ms.openlocfilehash: e2cc6cabc691bf46c57dded23ca8a86ae6c77985
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385842"
---
* Ein Azure-Konto das ein aktives Abonnement beinhaltet. Sie können ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), falls Sie noch keins besitzen.

    > [!NOTE]
    > Sie benötigen ein Azure-Abonnement, in dem Sie sowohl auf die Rolle [Mitwirkender](../../../../../role-based-access-control/built-in-roles.md#contributor) als auch auf die Rolle [Benutzerzugriffsadministrator](../../../../../role-based-access-control/built-in-roles.md#user-access-administrator) zugreifen können. Wenn Sie nicht über die richtigen Berechtigungen verfügen, wenden Sie sich an Ihren Kontoadministrator, damit er Ihnen diese Berechtigungen erteilt.
    * [Visual Studio Code](https://code.visualstudio.com/) mit den folgenden Erweiterungen:
    * [Azure IoT-Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

        > [!TIP]
        > Bei der Installation von Azure IoT Tools werden Sie unter Umständen aufgefordert, Docker zu installieren. Sie können diese Aufforderung ignorieren.
    * [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
* [Python 3](https://www.python.org/downloads/) (3.6.9 oder höher), [Pip 3](https://pip.pypa.io/en/stable/installing/) und optional [venv](https://docs.python.org/3/library/venv.html)        
   
## <a name="set-up-azure-resources"></a>Einrichten von Azure-Ressourcen

[![In Azure bereitstellen](https://aka.ms/deploytoazurebutton)](https://aka.ms/ava-click-to-deploy)  
[!INCLUDE [resources](../../../includes/common-includes/azure-resources.md)]

## <a name="overview"></a>Überblick

> [!div class="mx-imgBorder"]
> :::image type="content" source="./../../../media/detect-motion-record-video-edge-devices/overview.png" alt-text="Veröffentlichen zugeordneter Rückschlussereignisse in IoT Edge-Hub":::

Im obigen Diagramm ist der Fluss der Signale in dieser Schnellstartanleitung dargestellt. Ein [Edge-Modul](https://github.com/Azure/video-analyzer/tree/main/edge-modules/sources/rtspsim-live555) simuliert eine IP-Kamera, die einen RTSP-Server (Real-Time Streaming Protocol) hostet. Der Knoten einer [RTSP-Quelle](./../../../pipeline.md#rtsp-source) ruft den Videofeed per Pullvorgang von diesem Server ab und sendet Videoframes an den Knoten des [Bewegungserkennungsprozessors](./../../../pipeline.md#motion-detection-processor). Die RTSP-Quelle sendet dieselben Videoframes an den Knoten eines [Signalgateprozessors](./../../../pipeline.md#signal-gate-processor), der geschlossen bleibt, bis er durch ein Ereignis ausgelöst wird.

Wenn der Bewegungserkennungsprozessor im Video Bewegungen erkennt, sendet er ein Ereignis an den Knoten des Signalgateprozessors und löst ihn dadurch aus. Das Gate wird für den konfigurierten Zeitraum geöffnet und sendet Videoframes an den Knoten der [Dateisenke](./../../../pipeline.md#file-sink). Von diesem Knoten der Dateisenke wird das Video als MP4-Datei im lokalen Dateisystem des Edgegeräts aufgezeichnet. Die Datei wird am konfigurierten Speicherort gespeichert.

In diesem Schnellstart führen Sie folgende Schritte aus:

1. Erstellen und Bereitstellen der Pipeline
1. Interpretieren der Ergebnisse
1. Bereinigen der Ressourcen
## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung
[!INCLUDE [setup development environment](./../../../includes/set-up-dev-environment/python/python-set-up-dev-env.md)]
