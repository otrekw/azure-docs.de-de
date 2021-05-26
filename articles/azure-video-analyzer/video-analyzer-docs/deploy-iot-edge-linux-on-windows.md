---
title: 'Bereitstellen für IoT Edge für Linux unter Windows: Azure'
description: Dieser Artikel enthält eine Anleitung für die Bereitstellung für IoT Edge für Linux auf einem Windows-Gerät.
ms.topic: how-to
ms.date: 05/25/2021
ms.openlocfilehash: 2907318f7d1c49c4aea247880a9880e724b46ca6
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385897"
---
# <a name="deploy-to-an-iot-edge-for-linux-on-windows-eflow-device"></a>Bereitstellen für IoT Edge für Linux auf einem Windows-Gerät (EFLOW)

In diesem Artikel wird beschrieben, wie Sie Azure Video Analyzer auf einem Edgegerät bereitstellen, das über [IoT Edge für Linux unter Windows (EFLOW)](../../iot-edge/iot-edge-for-linux-on-windows.md) verfügt. Nachdem Sie die Schritte in diesem Dokument ausgeführt haben, können Sie eine [Pipeline](pipeline.md) ausführen, mit dem Bewegung in einem Video erkannt wird und die entsprechenden Ereignisse an den IoT Hub in der Cloud ausgegeben werden. Anschließend können Sie die Pipeline für erweiterte Szenarien einsetzen und die Leistungsstärke von Azure Video Analyzer für Ihr Windows-basiertes IoT Edge-Gerät nutzen.

## <a name="prerequisites"></a>Voraussetzungen 

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), falls Sie noch keins besitzen.

* Installation von [Visual Studio Code](https://code.visualstudio.com/) auf Ihrem Entwicklungscomputer. Stellen Sie sicher, dass Sie über die [Azure IoT Tools-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) verfügen.
* Weitere Informationen finden Sie unter [Was ist Azure IoT Edge für Linux unter Windows (Vorschau)?](../../iot-edge/iot-edge-for-linux-on-windows.md).

## <a name="deployment-steps"></a>Bereitstellungsschritte

Hier ist der Inhalt und Ablauf des Dokuments dargestellt. Sie können fünf einfache Schritte ausführen, um Azure Video Analyzer auf einem Windows-Gerät mit EFLOW nutzen zu können:

![Diagramm: IoT Edge für Linux unter Windows (EFLOW)](./media/deploy-iot-edge-linux-on-windows/eflow.png)

1. [Installieren Sie EFLOW](../../iot-edge/how-to-install-iot-edge-on-windows.md) auf Ihrem Windows-Gerät. 

    1. Falls Sie Ihren Windows-PC verwenden, wird auf der Startseite von [Windows Admin Center](/windows-server/manage/windows-admin-center/overview) unter der Liste mit den Verbindungen eine Verbindung mit dem lokalen Host angezeigt. Diese stellt den PC dar, auf dem Sie Windows Admin Center ausführen. 
    1. Alle weiteren von Ihnen verwalteten Server, PCs oder Cluster werden hier ebenfalls aufgeführt.
    1. Sie können Windows Admin Center verwenden, um Azure EFLOW entweder auf Ihrem lokalen Gerät oder auf verwalteten Remotegeräten zu installieren und zu verwalten. In diesem Leitfaden fungiert die Verbindung mit dem lokalen Host als Zielgerät für die Bereitstellung von Azure IoT Edge für Linux unter Windows. Aus diesem Grund ist „Localhost“ auch als IoT Edge-Gerät aufgeführt.

    ![Bereitstellungsschritte: Windows Admin Center](./media/deploy-iot-edge-linux-on-windows/windows-admin-center.png) 
1. Klicken Sie auf das IoT Edge-Gerät, um eine Verbindung damit herzustellen. Die Registerkarten „Übersicht“ und „Befehlsshell“ sollten angezeigt werden. Auf der Registerkarte „Befehlsshell“ können Sie Befehle für Ihr Edgegerät ausführen.

    ![Bereitstellungsschritte: Azure IoT Edge-Manager](./media/deploy-iot-edge-linux-on-windows/azure-iot-edge-manager.png)
1. Navigieren Sie zur Befehlsshell, und geben Sie den folgenden Befehl ein:

    `bash -c "$(curl -sL https://aka.ms/ava-edge/prep_device)"`

    Das Azure Video Analyzer-Modul wird auf dem Edgegerät mit lokalen Benutzerkonten ohne Berechtigungen ausgeführt. Darüber hinaus werden bestimmte lokale Ordner benötigt, um die Daten für die Anwendungskonfiguration zu speichern. Abschließend nutzen wir für diese Schrittanleitung einen [RTSP-Simulator](https://github.com/Azure/video-analyzer/tree/main/edge-modules/sources/rtspsim-live555), mit dem ein Videofeed in Echtzeit zur Analyse an ein AVA-Modul weitergeleitet wird. Für diesen Simulator werden als Eingabe vorab aufgezeichnete Videodateien aus einem Eingabeverzeichnis verwendet. 

    Mit dem obigen Skript zum Vorbereiten des Geräts werden die Aufgaben automatisiert, damit Sie nur einen Befehl ausführen müssen, um nahtlos alle relevanten Eingabe- und Konfigurationsordner, Videoeingabedateien und Benutzerkonten mit Berechtigungen zu erstellen. Nachdem der Befehl erfolgreich abgeschlossen wurde, sollten auf Ihrem Edgegerät die folgenden Ordner erstellt worden sein. 

    * `/home/localedgeuser/samples`
    * `/home/localedgeuser/samples/input`
    * `/var/lib/videoanalyzer`
    * `/var/media`

    Achten Sie auf die Videodateien (*.mkv) im Ordner „/home/localedgeuser/samples/input“, die als Eingabedateien für die Analyse dienen. 
1. Nachdem Sie nun das Edgegerät eingerichtet und die Registrierung beim Hub durchgeführt haben und die Ausführung mit den richtigen erstellten Ordnerstrukturen erfolgreich ist, ist der nächste Schritt die Einrichtung der folgenden zusätzlichen Azure-Ressourcen und die Bereitstellung des AVA-Moduls. 

    * Speicherkonto
    * Azure Media Services-Konto

    [![In Azure bereitstellen](https://aka.ms/deploytoazurebutton)](https://aka.ms/ava-click-to-deploy)

    Wenn Sie bei der Vorlage gefragt werden, ob Sie ein Edgegerät benötigen, wählen Sie die Option „Vorhandenes Edgegerät verwenden“ aus, da Sie zuvor sowohl das Gerät als auch den IoT Hub erstellt haben. Außerdem werden Sie in den folgenden Schritten aufgefordert, den Namen Ihres IoT Hub und die ID des IoT Edge-Geräts einzugeben.  
    
    ![Vorhandenes Gerät verwenden](./media/deploy-iot-edge-linux-on-windows/use-existing-device.png) 

    Nach Abschluss des Vorgangs können Sie sich wieder bei der Befehlsshell des IoT Edge-Geräts anmelden und den folgenden Befehl ausführen.

    **`sudo iotedge list`**

    Auf Ihrem Edgegerät sollten die folgenden vier Module bereitgestellt und ausgeführt werden. Beachten Sie Folgendes: Mit dem Skript für die Ressourcenerstellung wird das AVA-Modul zusammen mit den IoT Edge-Modulen („edgeAgent“ und „edgeHub“) und einem RTSP-Simulatormodul bereitgestellt, um die Simulation des RTSP-Videofeeds zu ermöglichen.
    
    ![Bereitgestellte Module](./media/vscode-common-screenshots/avaedge-module.png)
1. Nachdem die Module bereitgestellt und eingerichtet wurden, können Sie Ihre erste AVA-Pipeline unter EFLOW ausführen. Sie können wie unten angegeben eine einfache Pipeline für die Bewegungserkennung ausführen und die Ergebnisse visualisieren, indem Sie die folgenden Schritte ausführen:

    ![Auf Bewegungserkennung basierter Video Analyzer](./media/get-started-detect-motion-emit-events/motion-detection.svg)

    1. [Konfigurieren](get-started-detect-motion-emit-events.md#prepare-to-monitor-the-modules) Sie die Azure IoT Tools-Erweiterung.
    1. Legen Sie die pipelineTopology fest, instanziieren Sie eine livePipeline, und aktivieren Sie diese über diese [direkten Methodenaufrufe](get-started-detect-motion-emit-events.md#use-direct-method-calls).
    1. [Verfolgen Sie die Ergebnisse](get-started-detect-motion-emit-events.md#observe-results) auf dem Hub.
    1. Rufen Sie die [Bereinigungsmethoden](get-started-detect-motion-emit-events.md#deactivate-the-live-pipeline) auf.
    1. Löschen Sie Ihre Ressourcen, falls Sie diese nicht mehr benötigen.

        > [!IMPORTANT]
        > Nicht gelöschte Ressourcen können weiterhin aktiv sein und Azure-Kosten verursachen. Stellen Sie sicher, dass Sie die Ressourcen löschen, die Sie nicht verwenden möchten.
        
## <a name="next-steps"></a>Nächste Schritte

* Probieren Sie die Bewegungserkennung mit der Aufzeichnung relevanter Videos in der Cloud aus. Führen Sie die Schritte im Schnellstart [Erkennen von Bewegung und Aufzeichnen von Videoclips](detect-motion-record-video-edge-devices.md) aus.
* Führen Sie [KI für Livevideos](analyze-live-video-use-your-model-http.md#overview) aus. (Sie können die Erfüllung der Voraussetzungen überspringen, da Sie dies oben bereits durchgeführt haben.)
* Verwenden Sie unsere [VS Code-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.live-video-analytics-edge), um weitere Pipelines anzuzeigen.
* Verwenden Sie eine [IP-Kamera](https://en.wikipedia.org/wiki/IP_camera) mit RTSP-Unterstützung anstelle des RTSP-Simulators. IP-Kameras, die RTSP unterstützen, finden Sie auf der [Seite mit den ONVIF-konformen Produkten](https://www.onvif.org/conformant-products/). Suchen Sie nach Geräten, die mit den Profilen G, S oder T konform sind.
