---
title: Analysieren von Livevideos mit maschinellem Sehen für die räumliche Analyse – Azure
description: In diesem Tutorial wird gezeigt, wie Sie Azure Video Analyzer zusammen mit dem KI-Feature „Maschinelles Sehen“ für die räumliche Analyse von Azure Cognitive Services verwenden, um einen Livevideofeed einer (simulierten) IP-Kamera zu analysieren.
author: Juliako
ms.author: juliako
ms.service: azure-video-analyzer
ms.topic: tutorial
ms.date: 04/01/2021
ms.openlocfilehash: d54983e25abc769a75923e59c483a4cf9495770f
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110384038"
---
# <a name="tutorial-live-video-with-computer-vision-for-spatial-analysis-preview"></a>Tutorial: Analysieren von Livevideos mit maschinellem Sehen für die räumliche Analyse (Vorschau)

In diesem Tutorial wird gezeigt, wie Sie Azure Video Analyzer zusammen mit dem [KI-Feature „Maschinelles Sehen“ für die räumliche Analyse von Azure Cognitive Services](../../cognitive-services/computer-vision/intro-to-spatial-analysis-public-preview.md) verwenden, um einen Livevideofeed einer (simulierten) IP-Kamera zu analysieren. Es wird gezeigt, wie Sie mit diesem Rückschlussserver das Streamingvideo analysieren können, um räumliche Beziehungen zwischen Personen und Bewegung im physischen Raum zu ermitteln. Eine Teilmenge der Frames im Videofeed wird an den Rückschlussserver gesendet, und die Ergebnisse werden an den IoT Edge-Hub gesendet. Wenn bestimmte Bedingungen erfüllt sind, werden Videoclips aufgezeichnet und im Video Analyzer-Konto gespeichert.

In diesem Lernprogramm führen Sie folgende Schritte aus:

> [!div class="checklist"]
>
> - Einrichten von Ressourcen
> - Untersuchen Sie den Code.
> - Ausführen des Beispielcodes
> - Überwachen von Ereignissen

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="suggested-pre-reading"></a>Empfohlene Lektüre zur Vorbereitung

Lesen Sie diese Artikel, bevor Sie beginnen:

- [Azure Video Analyzer-Übersicht](overview.md)
- [Video Analyzer-Terminologie](terminology.md)
- [Pipelinekonzepte](pipeline.md)
- [Ereignisbasierte Videoaufzeichnung](record-event-based-live-video.md)
- [Tutorial: Entwickeln eines IoT Edge-Moduls](../../iot-edge/tutorial-develop-for-linux.md)

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Voraussetzungen müssen erfüllt sein, um das Modul „spatial-analysis“ mit dem Azure Video Analyzer-Modul zu verbinden.

- Installation von [Visual Studio Code](https://code.visualstudio.com/) auf Ihrem Entwicklungscomputer. Stellen Sie sicher, dass Sie über die [Azure IoT Tools-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) verfügen.
  - Vergewissern Sie sich, dass für das Netzwerk, mit dem der Entwicklungscomputer verbunden ist, Advanced Message Queueing Protocol über Port 5671 zulässig ist. Dieses Setup ermöglicht die Kommunikation zwischen Azure IoT Tools und Azure IoT Hub.
- [Azure Cognitive Service-Container Maschinelles Sehen](../../cognitive-services/computer-vision/intro-to-spatial-analysis-public-preview.md) für räumliche Analysen.
  Um diesen Container verwenden zu können, benötigen Sie eine Ressource für maschinelles Sehen, um den zugeordneten **API-Schlüssel** und einen **Endpunkt-URI** abzurufen. Der API-Schlüssel ist im Azure-Portal auf der Übersichtsseite und Schlüsselseite für Maschinelles Sehen verfügbar. Der Schlüssel und der Endpunkt sind erforderlich, um den Container zu starten.

## <a name="set-up-azure-resources"></a>Einrichten von Azure-Ressourcen

1. Zum Ausführen des Containers für räumliche Analyse benötigen Sie ein Computegerät mit einer [NVIDIA Tesla T4-GPU](https://www.nvidia.com/data-center/tesla-t4/). Es empfiehlt sich zwar, [Azure Stack Edge](https://azure.microsoft.com/products/azure-stack/edge/) mit GPU-Beschleunigung zu verwenden, der Container kann jedoch auf jedem Desktopcomputer verwendet werden, sofern [Ubuntu Desktop 18.04 LTS](http://releases.ubuntu.com/18.04/) auf dem Hostcomputer installiert ist.

   #### <a name="azure-stack-edge-device"></a>[Azure Stack Edge-Gerät](#tab/azure-stack-edge)

   Azure Stack Edge ist eine Hardware-as-a-Service-Lösung und ein KI-fähiges Edgecomputinggerät mit Netzwerkfunktionen für die Datenübertragung. Eine ausführliche Anleitung zur Vorbereitung und Einrichtung finden Sie in der [Azure Stack Edge-Dokumentation](../../databox-online/azure-stack-edge-deploy-prep.md).

   #### <a name="desktop-machine"></a>[Desktopcomputer](#tab/desktop-machine)

   #### <a name="minimum-hardware-requirements"></a>Hardwaremindestanforderungen

   - 4 GB Systemarbeitsspeicher
   - 4 GB GPU-Arbeitsspeicher
   - CPU mit acht Kernen
   - 1 NVIDIA Tesla T4-GPU
   - 20 GB Festplattenspeicher

   #### <a name="recommended-hardware"></a>Empfohlene Hardware

   - 32 GB Systemarbeitsspeicher
   - 16 GB GPU-Arbeitsspeicher
   - CPU mit acht Kernen
   - 2 NVIDIA Tesla T4-GPUs
   - 50 GB SSD-Speicherplatz

   In diesem Artikel führen Sie den Download und die Installation der folgenden Softwarepakete durch. Auf dem Hostcomputer muss Folgendes ausgeführt werden können (siehe Anleitung unten):

   - [NVIDIA-Grafiktreiber](https://docs.nvidia.com/datacenter/tesla/tesla-installation-notes/index.html) und [NVIDIA CUDA-Toolkit](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)
   - Konfigurationen für [NVIDIA MPS](https://docs.nvidia.com/deploy/pdf/CUDA_Multi_Process_Service_Overview.pdf) (Multi-Process Service, Multiprozessdienst)
   - [Docker CE](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-engine---community-1) und [NVIDIA-Docker2](https://github.com/NVIDIA/nvidia-docker)
   - [Azure IoT Edge](../../iot-edge/how-to-install-iot-edge.md)-Runtime

   #### <a name="azure-vm-with-gpu"></a>[Azure-VM mit GPU](#tab/virtual-machine)

   Sie können einen [virtuellen Computer der NC-Serie](../../virtual-machines/nc-series.md?bc=%2fazure%2fvirtual-machines%2flinux%2fbreadcrumb%2ftoc.json&toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mit einer K80-GPU verwenden.

   1. Stellen Sie eine Verbindung mit Ihrem virtuellen Computer her, und geben Sie im Terminal den folgenden Befehl ein:

        `bash -c "$(curl -sL https://aka.ms/ava-edge/prep_device)"`
    
        Das Azure Video Analyzer-Modul wird auf dem Edgegerät mit lokalen Benutzerkonten ohne Berechtigungen ausgeführt. Darüber hinaus werden bestimmte lokale Ordner benötigt, um die Daten für die Anwendungskonfiguration zu speichern. Abschließend nutzen wir für diese Schrittanleitung einen [RTSP-Simulator](https://github.com/Azure/video-analyzer/tree/main/edge-modules/sources/rtspsim-live555), mit dem ein Videofeed in Echtzeit zur Analyse an ein AVA-Modul weitergeleitet wird. Für diesen Simulator werden als Eingabe vorab aufgezeichnete Videodateien aus einem Eingabeverzeichnis verwendet. 
    
        Mit dem obigen Skript zum Vorbereiten des Geräts werden die Aufgaben automatisiert, damit Sie nur einen Befehl ausführen müssen, um nahtlos alle relevanten Eingabe- und Konfigurationsordner, Videoeingabedateien und Benutzerkonten mit Berechtigungen zu erstellen. Nachdem der Befehl erfolgreich abgeschlossen wurde, sollten auf Ihrem Edgegerät die folgenden Ordner erstellt worden sein. 
    
        * `/home/localedgeuser/samples`
        * `/home/localedgeuser/samples/input`
        * `/var/lib/videoanalyzer`
        * `/var/media`
    
        Achten Sie auf die Videodateien (*.mkv) im Ordner „/home/localedgeuser/samples/input“, die als Eingabedateien für die Analyse dienen. 

1. [Einrichten des Edgegeräts](../../cognitive-services/computer-vision/spatial-analysis-container.md#set-up-the-host-computer)

1. Stellen Sie als Nächstes die anderen Azure-Ressourcen bereit.

   [![In Azure bereitstellen](https://aka.ms/deploytoazurebutton)](https://aka.ms/ava-click-to-deploy)

   > [!NOTE]
   > Über die obige Schaltfläche wird der virtuelle Standardcomputer erstellt und verwendet. Dieser verfügt NICHT über die NVIDIA-GPU. Verwenden Sie die Option „Vorhandenes Edgegerät verwenden“, wenn Sie in der ARM-Vorlage (Azure Resource Manager) danach gefragt werden, und verwenden Sie die IoT Hub-Instanz und die Geräteinformationen aus dem obigen Schritt.
   > :::image type="content" source="./media/spatial-analysis/use-existing-device.png" alt-text="Verwenden eines vorhandenen Geräts":::

   [!INCLUDE [resources](./includes/common-includes/azure-resources.md)]

## <a name="overview"></a>Übersicht

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/spatial-analysis/overview.png" alt-text="Übersicht über die räumliche Analyse":::

In diesem Diagramm ist der Fluss der Signale in diesem Tutorial dargestellt. Ein [Edge-Modul](https://github.com/Azure/video-analyzer/tree/main/edge-modules/sources/rtspsim-live555) simuliert eine IP-Kamera, die einen RTSP-Server (Real-Time Streaming Protocol) hostet. Der Knoten einer [RTSP-Quelle](pipeline.md#rtsp-source) ruft den Videofeed von diesem Server ab und sendet Videoframes an den Knoten des `CognitiveServicesVisionProcessor`.

Der Knoten `CognitiveServicesVisionProcessor` fungiert als Proxy. Er wandelt die Video-Einzelbilder in den angegebenen Bildtyp um. Anschließend leitet er das Bild über **gemeinsam genutzten Speicher** an ein anderes Edge-Modul weiter, das KI-Vorgänge hinter einem gRPC-Endpunkt ausführt. In diesem Beispiel ist das Edge-Modul das Modul „spatial-analysis“. Der Knoten `CognitiveServicesVisionProcessor` hat zwei Aufgaben:

- Er sammelt die Ergebnisse und veröffentlicht Ereignisse im Knoten der [IoT Hub-Senke](pipeline.md#iot-hub-message-sink). Der Knoten sendet diese Ereignisse dann an den [IoT Edge Hub-](../../iot-fundamentals/iot-glossary.md#iot-edge-hub).
- Er erfasst außerdem mit einem [Signalgateprozessor](pipeline.md#signal-gate-processor) einen Videoclip von 30 Sekunden aus der RTSP-Quelle und speichert ihn als Videodatei.

## <a name="create-the-computer-vision-resource"></a>Erstellen der Ressource für maschinelles Sehen

Sie müssen im [Azure-Portal](../../iot-edge/how-to-deploy-modules-portal.md) oder über die Azure CLI eine Azure-Ressource vom Typ Maschinelles Sehen erstellen. Sie können die Ressource erstellen, nachdem Ihre Anforderung des Zugriffs auf den Container genehmigt und Ihre Azure-Abonnement-ID registriert wurde. Wechseln Sie zu https://aka.ms/csgate, um Ihren Anwendungsfall und Ihre Azure-Abonnement-ID zu übermitteln. Sie müssen die Azure-Ressource mit dem Azure-Abonnement erstellen, das im Zugriffsanforderungsformular angegeben wurde.

### <a name="gathering-required-parameters"></a>Ermitteln erforderlicher Parameter

Es gibt drei primäre Parameter, die für alle Cognitive Services-Container (einschließlich des Containers „spatial-analysis“) erforderlich sind. Die Lizenzbedingungen müssen mit dem Wert „accept“ vorhanden sein. Außerdem werden sowohl der Endpunkt-URI als auch der API-Schlüssel benötigt.

### <a name="keys-and-endpoint-uri"></a>Schlüssel und Endpunkt-URI

Ein Schlüssel wird zum Starten des Containers „spatial-analysis“ verwendet. Er ist im Azure-Portal auf der Seite `Keys and Endpoint` der entsprechenden Cognitive Services-Ressource verfügbar. Navigieren Sie zu dieser Seite, und suchen Sie die Schlüssel und den Endpunkt-URI.  

Sie benötigen diesen Schlüssel und den Endpunkt-URI in Ihren Bereitstellungsmanifestdateien, um den Container „spatialanalysis“ bereitzustellen.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/spatial-analysis/keys-endpoint.png" alt-text="Endpunkt-URI":::

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

1. Klonen Sie das folgende Repository: [https://github.com/Azure-Samples/azure-video-analyzer-iot-edge-csharp](https://github.com/Azure-Samples/azure-video-analyzer-iot-edge-csharp).
1. Öffnen Sie in Visual Studio Code den Ordner, in den das Repository heruntergeladen wurde.
1. Navigieren Sie in Visual Studio Code zum Ordner src/cloud-to-device-console-app. Erstellen Sie darin eine Datei, und geben Sie ihr den Namen *appsettings.json*. Diese Datei enthält die Einstellungen, die zum Ausführen des Programms erforderlich sind.
1. Rufen Sie die IoT Hub-Verbindungszeichenfolge (`IotHubConnectionString`) vom Edgegerät ab:

   - Wechseln Sie zu IoT Hub im Azure-Portal, und klicken Sie im linken Navigationsbereich auf `Shared access policies`.
   - Klicken Sie auf `iothubowner`, um die Schlüssel für gemeinsamen Zugriff abzurufen.
   - Kopieren Sie den Primärschlüssel der Verbindungszeichenfolge (`Connection String – primary key`), und fügen Sie ihn in Visual Studio Code in das Eingabefeld ein.

     Die Verbindungszeichenfolge lautet: <br/>`HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx`

1. Kopieren Sie den folgenden Inhalt in die Datei. Ersetzen Sie die Variablen.

   ```json
   {
     "IoThubConnectionString": "HostName=<IoTHubName>.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=<SharedAccessKey>",
     "deviceId": "<your Edge Device name>",
     "moduleId": "avaedge"
   }
   ```

1. Navigieren Sie zum Ordner src/edge, und erstellen Sie eine Datei mit dem Namen .env.
1. Kopieren Sie den Inhalt der ENV-Datei aus dem Azure-Portal. Der Text sollte wie der folgende Code aussehen.

   ```env
   SUBSCRIPTION_ID="<Subscription ID>"
   RESOURCE_GROUP="<Resource Group>"
   AVA_PROVISIONING_TOKEN="<The provisioning token>"
   VIDEO_INPUT_FOLDER_ON_DEVICE="/home/localedgeuser/samples/input"
   VIDEO_OUTPUT_FOLDER_ON_DEVICE="/var/media"
   APPDATA_FOLDER_ON_DEVICE="/var/lib/videoanalyzer"
   CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
   CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry password>"
   ```

## <a name="set-up-deployment-template"></a>Einrichten der Bereitstellungsvorlage

Die Bereitstellungsdatei finden Sie unter /src/edge/deployment.spatialAnalysis.template.json. Die Vorlage enthält das Modul „avaedge“, das Modul „rtspsim“ und das Modul „spatialanalysis“.

Bei der Bereitstellungsvorlagendatei sind einige Aspekte zu beachten:

1. Legen Sie die Portbindung im Modul `spatialanalysis` fest.

   ```
   "PortBindings": {
       "50051/tcp": [
           {
               "HostPort": "50051"
           }
       ]
   }
   ```

1. Die Erstellungsoptionen (createOptions) von `IpcMode` in den Modulen `avaedge` und `spatialanalysis` müssen identisch und auf **host** festgelegt sein.
1. Damit der RTSP-Simulator funktioniert, müssen bei Verwendung eines Azure Stack Edge-Geräts Volumegrenzen eingerichtet worden sein.

   1. [Stellen Sie eine Verbindung mit der SMB-Freigabe her](../../databox-online/azure-stack-edge-deploy-add-shares.md#connect-to-an-smb-share), und kopieren Sie die [Beispielvideodatei „stairwell“](https://lvamedia.blob.core.windows.net/public/2018-03-05.10-27-03.10-30-01.admin.G329.mp4) in die lokale Freigabe.

      > [!VIDEO https://www.microsoft.com/videoplayer/embed/RWDRJd]

   1. Das Modul „rtspsim“ muss folgende Konfiguration aufweisen:
      ```
      "createOptions": {
                          "HostConfig": {
                            "Mounts": [
                              {
                                "Target": "/live/mediaServer/media",
                                "Source": "<your Local Docker Volume Mount name>",
                                "Type": "volume"
                              }
                            ],
                            "PortBindings": {
                              "554/tcp": [
                                {
                                  "HostPort": "554"
                                }
                              ]
                            }
                          }
                        }
      ```
## <a name="generate-and-deploy-the-deployment-manifest"></a>Generieren und Bereitstellen des Bereitstellungsmanifests

Mit dem Bereitstellungsmanifest wird definiert, welche Module auf einem Edgegerät bereitgestellt werden. Darüber hinaus werden auch die Konfigurationseinstellungen für diese Module definiert.

Führen Sie die folgenden Schritte aus, um das Manifest auf der Grundlage der Vorlagendatei zu generieren und dann auf dem Edgegerät bereitzustellen.

1. Öffnen Sie Visual Studio Code.
1. Wählen Sie neben dem Bereich `AZURE IOT HUB` das Symbol „Weitere Aktionen“ aus, um die IoT Hub-Verbindungszeichenfolge festzulegen. Sie können die Zeichenfolge aus der Datei `src/cloud-to-device-console-app/appsettings.json` kopieren.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/vscode-common-screenshots/set-connection-string.png" alt-text="Räumliche Analyse: Verbindungszeichenfolge":::

1. Klicken Sie mit der rechten Maustaste auf `src/edge/deployment.spatialAnalysis.template.json`, und wählen Sie „IoT Edge-Bereitstellungsmanifest generieren“ aus.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/spatial-analysis/generate-deployment-manifest.png" alt-text="Räumliche Analyse: Datei „deployment.amd64.json“":::

   Dadurch sollte im Ordner „src/edge/config“ eine Manifestdatei mit dem Namen `deployment.spatialAnalysis.amd64.json` erstellt werden.

1. Klicken Sie mit der rechten Maustaste auf `src/edge/config/deployment.spatialAnalysis.amd64.json`, wählen Sie **Create Deployment for Single Device** (Bereitstellung für einzelnes Gerät erstellen) und anschließend den Namen Ihres Edgegeräts aus.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/spatial-analysis/deployment-single-device.png" alt-text="Räumliche Analyse: Bereitstellen für einzelnes Gerät":::

1. Am oberen Seitenrand werden Sie aufgefordert, ein IoT Hub-Gerät auszuwählen. Wählen Sie im Dropdownmenü den Namen Ihres Edgegeräts aus.
1. Aktualisieren Sie nach ungefähr 30 Sekunden links unten im Fenster den Bereich **AZURE IOT HUB**. Das Edge-Gerät zeigt nun die folgenden bereitgestellten Module an:

   - Azure Video Analyzer (Modulname **avaedge**)
   - RTSP-Simulator (Real-Time Streaming Protocol) (Modulname **rtspsim**)
   - Räumliche Analyse (Modulname **spatialanalysis**)

Nach erfolgreicher Bereitstellung wird im Ausgabefenster eine Meldung wie die folgende angezeigt:

```
[Edge] Start deployment to device [<edge device name>]
[Edge] Deployment succeeded.
```

Anschließend finden Sie unter „Geräte/Module“ die Module `avaedge`, `spatialanalysis` und `rtspsim`, und ihr Status sollte **Wird ausgeführt** lauten.

## <a name="prepare-to-monitor-events"></a>Vorbereiten der Überwachung von Ereignissen

Führen Sie zum Anzeigen dieser Ereignisse die folgenden Schritte aus:

1. Öffnen Sie in Visual Studio Code die Registerkarte **Erweiterungen** (oder drücken Sie STRG+UMSCHALT+X), und suchen Sie nach Azure IoT Hub.
1. Klicken Sie mit der rechten Maustaste, und wählen Sie **Erweiterungseinstellungen** aus.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/vscode-common-screenshots/extension-settings.png" alt-text="Erweiterungseinstellungen":::

1. Suchen Sie nach dem Kontrollkästchen „Show Verbose Message“ (Ausführliche Meldung anzeigen), und aktivieren Sie es.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/vscode-common-screenshots/verbose-message.png" alt-text="Show Verbose Message (Ausführliche Meldung anzeigen)":::

1. Öffnen Sie den Explorer-Bereich, und suchen Sie in der linken unteren Ecke nach **AZURE IOT HUB**. Klicken Sie mit der rechten Maustaste, und wählen Sie „Überwachung des integrierten Ereignisendpunkts starten“ aus.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/vscode-common-screenshots/start-monitoring.png" alt-text="Räumliche Analyse: Starten der Überwachung":::

## <a name="run-the-program"></a>Ausführen des Programms

Durch „program.cs“ werden die direkten Methoden in `src/cloud-to-device-console-app/operations.json` aufgerufen. Wir müssen „operations.json“ einrichten und eine Pipelinetopologie für die Pipelineverwendung bereitstellen.

In „operations.json“:

- Legen Sie die Pipelinetopologie wie folgt fest:

  ```json
  {
      "opName": "pipelineTopologySet",
      "opParams": {
          "topologyUrl": "https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/spatial-analysis/person-count-operation-topology.json"
      }
  },
  ```

- Erstellen Sie wie folgt eine Livepipeline, und legen Sie dort die Parameter für die Pipelinetopologie fest:

  ```json
  {
      "opName": "livePipelineSet",
      "opParams": {
          "name": "Sample-Pipeline-1",
          "properties": {
              "topologyName": "InferencingWithCVExtension",
              "description": "Sample pipeline description",
              "parameters": [
                  {
                      "name": "rtspUrl",
                      "value": " rtsp://rtspsim:554/media/stairwell.mkv"
                  },
                  {
                      "name": "rtspUserName",
                      "value": "testuser"
                  },
                  {
                      "name": "rtspPassword",
                      "value": "testpassword"
                  }
              ]
          }
      }
  },
  ```

  > [!Note]
  > Sehen Sie sich an, wie `CognitiveServicesVisionExtension` verwendet wird, um eine Verbindung mit dem Modul „spatialanalysis“ herzustellen. Legen Sie „${grpcUrl}“ auf **tcp://spatialAnalysis:<PORTNUMMER>** fest (also beispielsweise auf „tcp://spatialAnalysis:50051“).

  ```json
  {
          "@type": "#Microsoft.VideoAnalyzer.CognitiveServicesVisionProcessor",
          "name": "computerVisionExtension",
          "endpoint": {
            "@type": "#Microsoft.VideoAnalyzer.UnsecuredEndpoint",
            "url": "${grpcUrl}",
            "credentials": {
              "@type": "#Microsoft.VideoAnalyzer.UsernamePasswordCredentials",
              "username": "${spatialanalysisusername}",
              "password": "${spatialanalysispassword}"
            }
          },
          "inputs": [
            {
              "nodeName": "rtspSource",
              "outputSelectors": [
                {
                  "property": "mediaType",
                  "operator": "is",
                  "value": "video"
                }
              ]
            }
          ],
          "operation": {
            "@type": "#Microsoft.VideoAnalyzer.SpatialAnalysisPersonCountOperation",
            "zones": [
              {
                "zone": {
                  "@type": "#Microsoft.VideoAnalyzer.NamedPolygonString",
                  "polygon": "[[0.37,0.43],[0.48,0.42],[0.53,0.56],[0.34,0.57],[0.34,0.46]]",
                  "name": "stairlanding"
                },
                "events": [
                  {
                    "trigger": "event",
                    "outputFrequency": "1",
                    "threshold": "16",
                    "focus": "bottomCenter"
                  }
                ]
              }
            ]
          }
        }
      ],
  ```

Führen Sie eine Debugsitzung aus, und befolgen Sie die Anweisungen im **TERMINAL**, um die Pipelinetopologie festzulegen, die Livepipeline festzulegen, die Livepipeline zu aktivieren und schließlich die Ressourcen zu löschen.

## <a name="interpret-results"></a>Interpretieren von Ergebnissen

Wenn ein Pipelinetopologie instanziiert wird, sollte das Ereignis „MediaSessionEstablished“ angezeigt werden. Ein Beispiel für dieses Ereignis finden Sie [hier](detect-motion-emit-events-quickstart.md#mediasessionestablished-event).

Das Modul „spatialanalysis“ sendet auch KI Insights-Ereignisse an Azure Video Analyzer und anschließend an IoT Hub. Dies wird auch im Ausgabefenster angezeigt. „ENTITY“ bezeichnet Erkennungsobjekte, und „EVENT“ bezeichnet Ereignisse der räumlichen Analyse. Diese Ausgabe wird an Azure Video Analyzer übermittelt.

Beispielausgabe für „personZoneEvent“ (aus dem Vorgang `SpatialAnalysisPersonZoneCrossingOperation`):

```
{
  "timestamp": 145666725597833,
  "inferences": [
    {
      "type": "entity",
      "inferenceId": "8724dff43d3c4716936aa6c9a808ee2e",
      "entity": {
        "tag": {
          "value": "person",
          "confidence": 0.9980469
        },
        "box": {
          "l": 0.3848941,
          "t": 0.28569314,
          "w": 0.092775516,
          "h": 0.3819766
        }
      },
      "extensions": {
        "footprintX": "inf",
        "centerGroundPointY": "0.0",
        "trackingId": "80eeb5bbb6f542b89b5b16a1e37c53c4",
        "footprintY": "inf",
        "centerGroundPointX": "0.0"
      }
    },
    {
      "type": "event",
      "inferenceId": "0b0de3d228b640488fc0624950a6c9e8",
      "relatedInferences": [
        "8724dff43d3c4716936aa6c9a808ee2e"
      ],
      "event": {
        "name": "personZoneEnterExitEvent",
        "properties": {
          "trackingId": "80eeb5bbb6f542b89b5b16a1e37c53c4",
          "status": "Enter",
          "zone": "door"
        }
      }
    }
  ]
}
```

## <a name="operations"></a>Vorgänge:

### <a name="person-zone-crossing"></a>Zonenüberquerung von Personen

#### <a name="parameters"></a>Parameter:

| Name                      | Typ    | BESCHREIBUNG                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Zonen                     | list    | Die Liste mit den Zonen.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| name                      | Zeichenfolge  | Der Anzeigename für diese Zone.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| polygon                   | Zeichenfolge  | Jedes Wertpaar stellt den x/y-Wert für Scheitelpunkte eines Polygons dar. Mit dem Polygon werden die Bereiche dargestellt, in denen Personen nachverfolgt oder gezählt werden. Die Gleitkommawerte stellen die Position des Scheitelpunkts relativ zur oberen linken Ecke dar. Zum Berechnen der absoluten x/y-Werte multiplizieren Sie diese Werte mit der Framegröße. Gleitkommaereignisse für Schwellenwerte werden ausgegeben, wenn der Personenwert diese Anzahl von Pixeln innerhalb der Zone übersteigt. Der Standardwert ist 48, wenn der Typ „zonecrossing“ und 16, wenn die Zeit DwellTime ist. Dies sind die empfohlenen Werte, um eine maximale Genauigkeit zu erzielen. |
| eventType                 | Zeichenfolge  | Für „cognitiveservices.vision.spatialanalysis-personcrossingpolygon“ sollte dies „zonecrossing“ oder „zonedwelltime“ sein.                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Trigger (trigger)                   | Zeichenfolge  | Der Typ des Triggers für das Senden eines Ereignisses. Unterstützte Werte: „event“ (Ereignis). Wird ausgelöst, wenn eine Person die Zone betritt oder verlässt.                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| focus                     | Zeichenfolge  | Dies ist die Punktposition innerhalb des Begrenzungsrahmens der Person, die zum Berechnen von Ereignissen verwendet wird. Der Wert für den Fokus kann „footprint“ (Fußabdruck der Person), „bottom_center“ (Mitte der Unterkante des Begrenzungsrahmens der Person) oder „center“ (Mitte des Begrenzungsrahmens der Person) sein. Der Standardwert ist „footprint“.                                                                                                                                                                                                                                                                                               |
| enableFaceMaskClassifier  | boolean | Mit „true“ wird die Erkennung von Personen mit Gesichtsmaske im Videostream aktiviert. Mit „false“ wird diese Option deaktiviert. Diese Einstellung ist standardmäßig deaktiviert. Für die Gesichtsmaskenerkennung muss der Parameter „width“ für das Eingabevideo auf „1920“ festgelegt sein ("INPUT_VIDEO_WIDTH": 1920). Das Gesichtsmaskenattribut wird nicht zurückgegeben.                                                                                                                                                                                                                                                                                                        |
| detectorNodeConfiguration | Zeichenfolge  | Die Parameter vom Typ „DETECTOR_NODE_CONFIG“ für alle Vorgänge der räumlichen Analyse.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

#### <a name="output"></a>Ausgabe:

```json
{
  "timestamp": 145666725597833,
  "inferences": [
    {
      "type": "entity",
      "inferenceId": "8724dff43d3c4716936aa6c9a808ee2e",
      "entity": {
        "tag": {
          "value": "person",
          "confidence": 0.9980469
        },
        "box": {
          "l": 0.3848941,
          "t": 0.28569314,
          "w": 0.092775516,
          "h": 0.3819766
        }
      },
      "extensions": {
        "footprintX": "inf",
        "centerGroundPointY": "0.0",
        "trackingId": "80eeb5bbb6f542b89b5b16a1e37c53c4",
        "footprintY": "inf",
        "centerGroundPointX": "0.0"
      }
    },
    {
      "type": "event",
      "inferenceId": "0b0de3d228b640488fc0624950a6c9e8",
      "relatedInferences": ["8724dff43d3c4716936aa6c9a808ee2e"],
      "event": {
        "name": "personZoneEnterExitEvent",
        "properties": {
          "trackingId": "80eeb5bbb6f542b89b5b16a1e37c53c4",
          "status": "Enter",
          "zone": "door"
        }
      }
    }
  ]
}
```

### <a name="more-operations"></a>Weitere Vorgänge:

<details>
  <summary>Zum Erweitern klicken</summary>

### <a name="person-line-crossing"></a>Linienüberquerung von Personen

#### <a name="parameters"></a>Parameter:

| Name                      | Typ    | BESCHREIBUNG                                                                                                                                                                                                                                                                   |
| ------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| lines                     | list    | Die Liste mit den Linien.                                                                                                                                                                                                                                                                |
| name                      | Zeichenfolge  | Der Anzeigename für diese Linie.                                                                                                                                                                                                                                                  |
| line                      | Zeichenfolge  | Die Wertpaare stellen jeweils den Start- und Endpunkt der Linie dar. Die Gleitkommawerte stellen die Position des Scheitelpunkts relativ zur oberen linken Ecke dar. Zum Berechnen der absoluten x/y-Werte multiplizieren Sie diese Werte mit der Framegröße.                            |
| outputFrequency           | INT     | Die Rate, mit der Ereignisse ausgegeben werden. Bei „output_frequency = X“ wird jedes x-te Ereignis ausgegeben. „output_frequency = 2“ bedeutet beispielsweise, dass jedes zweite Ereignis ausgegeben wird. Die Ausgabehäufigkeit (output_frequency) gilt für Ereignis und Intervall.                                                       |
| focus                     | Zeichenfolge  | Dies ist die Punktposition innerhalb des Begrenzungsrahmens der Person, die zum Berechnen von Ereignissen verwendet wird. Der Wert für den Fokus kann „footprint“ (Fußabdruck der Person), „bottom_center“ (Mitte der Unterkante des Begrenzungsrahmens der Person) oder „center“ (Mitte des Begrenzungsrahmens der Person) sein. Der Standardwert ist „footprint“. |
| threshold                 | float   | Ereignisse werden ausgegeben, wenn der Personenwert größer als diese Anzahl von Pixeln innerhalb der Zone ist. Der Standardwert ist 16. Dies ist der empfohlene Wert, um eine maximale Genauigkeit zu erzielen.                                                                                                |
| enableFaceMaskClassifier  | boolean | Mit „true“ wird die Erkennung von Personen mit Gesichtsmaske im Videostream aktiviert. Mit „false“ wird diese Option deaktiviert. Diese Einstellung ist standardmäßig deaktiviert. Für die Gesichtsmaskenerkennung muss der Parameter „width“ für das Eingabevideo auf „1920“ festgelegt sein ("INPUT_VIDEO_WIDTH": 1920). Das Gesichtsmaskenattribut wird nicht zurückgegeben.          |
| detectorNodeConfiguration | Zeichenfolge  | Die Parameter vom Typ „DETECTOR_NODE_CONFIG“ für alle Vorgänge der räumlichen Analyse.                                                                                                                                                                                                      |

#### <a name="output"></a>Ausgabe:

```json
{
  "timestamp": 145666620394490,
  "inferences": [
    {
      "type": "entity",
      "inferenceId": "2d3c7c7d6c0f4af7916eb50944523bdf",
      "entity": {
        "tag": {
          "value": "person",
          "confidence": 0.38330078
        },
        "box": {
          "l": 0.5316645,
          "t": 0.28169397,
          "w": 0.045862257,
          "h": 0.1594377
        }
      },
      "extensions": {
        "centerGroundPointX": "0.0",
        "centerGroundPointY": "0.0",
        "footprintX": "inf",
        "trackingId": "ac4a79a29a67402ba447b7da95907453",
        "footprintY": "inf"
      }
    },
    {
      "type": "event",
      "inferenceId": "2206088c80eb4990801f62c7050d142f",
      "relatedInferences": ["2d3c7c7d6c0f4af7916eb50944523bdf"],
      "event": {
        "name": "personLineEvent",
        "properties": {
          "trackingId": "ac4a79a29a67402ba447b7da95907453",
          "status": "CrossLeft",
          "zone": "door"
        }
      }
    }
  ]
}
```

### <a name="person-distance"></a>Personenabstand

#### <a name="parameters"></a>Parameter:

| Name                      | Typ    | BESCHREIBUNG                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Zonen                     | list    | Die Liste mit den Zonen.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| name                      | Zeichenfolge  | Der Anzeigename für diese Zone.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| polygon                   | Zeichenfolge  | Jedes Wertpaar stellt den x/y-Wert für Scheitelpunkte eines Polygons dar. Mit dem Polygon werden die Bereiche dargestellt, in denen Personen nachverfolgt oder gezählt werden. Die Gleitkommawerte stellen die Position des Scheitelpunkts relativ zur oberen linken Ecke dar. Zum Berechnen der absoluten x/y-Werte multiplizieren Sie diese Werte mit der Framegröße. Gleitkommaereignisse für Schwellenwerte werden ausgegeben, wenn der Personenwert diese Anzahl von Pixeln innerhalb der Zone übersteigt. Der Standardwert ist 48, wenn der Typ „zonecrossing“ und 16, wenn die Zeit DwellTime ist. Dies sind die empfohlenen Werte, um eine maximale Genauigkeit zu erzielen. |
| outputFrequency           | INT     | Die Rate, mit der Ereignisse ausgegeben werden. Bei „output_frequency = X“ wird jedes x-te Ereignis ausgegeben. „output_frequency = 2“ bedeutet beispielsweise, dass jedes zweite Ereignis ausgegeben wird. Die Ausgabehäufigkeit (output_frequency) gilt für Ereignis und Intervall.                                                                                                                                                                                                                                                                                                                                                     |
| focus                     | Zeichenfolge  | Dies ist die Punktposition innerhalb des Begrenzungsrahmens der Person, die zum Berechnen von Ereignissen verwendet wird. Der Wert für den Fokus kann „footprint“ (Fußabdruck der Person), „bottom_center“ (Mitte der Unterkante des Begrenzungsrahmens der Person) oder „center“ (Mitte des Begrenzungsrahmens der Person) sein. Der Standardwert ist „footprint“.                                                                                                                                                                                                                                                                                               |
| threshold                 | float   | Ereignisse werden ausgegeben, wenn der Personenwert größer als diese Anzahl von Pixeln innerhalb der Zone ist.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| outputFrequency           | INT     | Die Rate, mit der Ereignisse ausgegeben werden. Bei „output_frequency = X“ wird jedes x-te Ereignis ausgegeben. „output_frequency = 2“ bedeutet beispielsweise, dass jedes zweite Ereignis ausgegeben wird. Die Ausgabehäufigkeit (output_frequency) gilt für Ereignis und Intervall.                                                                                                                                                                                                                                                                                                                                                     |
| minimumDistanceThreshold  | float   | Ein Abstand in Fuß, bei dem das Ereignis „TooClose“ ausgelöst wird, wenn Personen weniger als den angegebenen Abstand voneinander aufweisen.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| maximumDistanceThreshold  | float   | Ein Abstand in Fuß, bei dem das Ereignis „TooFar“ ausgelöst wird, wenn Personen mehr als den angegebenen Abstand voneinander aufweisen.                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| aggregationMethod         | Zeichenfolge  | Die Methode für das aggregierte persondistance-Ergebnis. Die Aggregationsmethode (aggregationMethod) gilt für Modus und Durchschnitt.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| enableFaceMaskClassifier  | boolean | Mit „true“ wird die Erkennung von Personen mit Gesichtsmaske im Videostream aktiviert. Mit „false“ wird diese Option deaktiviert. Diese Einstellung ist standardmäßig deaktiviert. Für die Gesichtsmaskenerkennung muss der Parameter „width“ für das Eingabevideo auf „1920“ festgelegt sein ("INPUT_VIDEO_WIDTH": 1920). Das Gesichtsmaskenattribut wird nicht zurückgegeben.                                                                                                                                                                                                                                                                                                        |
| detectorNodeConfiguration | Zeichenfolge  | Die Parameter vom Typ „DETECTOR_NODE_CONFIG“ für alle Vorgänge der räumlichen Analyse.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

#### <a name="output"></a>Ausgabe:

```json
{
  "timestamp": 145666613610297,
  "inferences": [
    {
      "type": "event",
      "inferenceId": "85a5fc4936294a3bac90b9c43876741a",
      "event": {
        "name": "personDistanceEvent",
        "properties": {
          "maximumDistanceThreshold": "14.5",
          "personCount": "0.0",
          "eventName": "Unknown",
          "zone": "door",
          "averageDistance": "0.0",
          "minimumDistanceThreshold": "1.5",
          "distanceViolationPersonCount": "0.0"
        }
      }
    }
  ]
}
```

### <a name="person-count"></a>Personenanzahl

#### <a name="parameters"></a>Parameter:

| Name                      | Typ    | BESCHREIBUNG                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Zonen                     | list    | Die Liste mit den Zonen.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| name                      | Zeichenfolge  | Der Anzeigename für diese Zone.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| polygon                   | Zeichenfolge  | Jedes Wertpaar stellt den x/y-Wert für Scheitelpunkte eines Polygons dar. Mit dem Polygon werden die Bereiche dargestellt, in denen Personen nachverfolgt oder gezählt werden. Die Gleitkommawerte stellen die Position des Scheitelpunkts relativ zur oberen linken Ecke dar. Zum Berechnen der absoluten x/y-Werte multiplizieren Sie diese Werte mit der Framegröße. Gleitkommaereignisse für Schwellenwerte werden ausgegeben, wenn der Personenwert diese Anzahl von Pixeln innerhalb der Zone übersteigt. Der Standardwert ist 48, wenn der Typ „zonecrossing“ und 16, wenn die Zeit DwellTime ist. Dies sind die empfohlenen Werte, um eine maximale Genauigkeit zu erzielen. |
| outputFrequency           | INT     | Die Rate, mit der Ereignisse ausgegeben werden. Bei „output_frequency = X“ wird jedes x-te Ereignis ausgegeben. „output_frequency = 2“ bedeutet beispielsweise, dass jedes zweite Ereignis ausgegeben wird. Die Ausgabehäufigkeit (output_frequency) gilt für Ereignis und Intervall.                                                                                                                                                                                                                                                                                                                                                     |
| Trigger (trigger)                   | Zeichenfolge  | Der Typ des Triggers für das Senden eines Ereignisses. Unterstützte Werte sind „event“ (Ereignis) zum Senden von Ereignissen, wenn sich die Anzahl ändert, und „interval“ (Intervall) zum regelmäßigen Senden von Ereignissen unabhängig davon, ob sich die Anzahl geändert hat.                                                                                                                                                                                                                                                                                                                                                           |
| focus                     | Zeichenfolge  | Dies ist die Punktposition innerhalb des Begrenzungsrahmens der Person, die zum Berechnen von Ereignissen verwendet wird. Der Wert für den Fokus kann „footprint“ (Fußabdruck der Person), „bottom_center“ (Mitte der Unterkante des Begrenzungsrahmens der Person) oder „center“ (Mitte des Begrenzungsrahmens der Person) sein. Der Standardwert ist „footprint“.                                                                                                                                                                                                                                                                                               |
| threshold                 | float   | Ereignisse werden ausgegeben, wenn der Personenwert größer als diese Anzahl von Pixeln innerhalb der Zone ist.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| enableFaceMaskClassifier  | boolean | Mit „true“ wird die Erkennung von Personen mit Gesichtsmaske im Videostream aktiviert. Mit „false“ wird diese Option deaktiviert. Diese Einstellung ist standardmäßig deaktiviert. Für die Gesichtsmaskenerkennung muss der Parameter „width“ für das Eingabevideo auf „1920“ festgelegt sein ("INPUT_VIDEO_WIDTH": 1920). Das Gesichtsmaskenattribut wird nicht zurückgegeben.                                                                                                                                                                                                                                                                                                        |
| detectorNodeConfiguration | Zeichenfolge  | Die Parameter vom Typ „DETECTOR_NODE_CONFIG“ für alle Vorgänge der räumlichen Analyse.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

#### <a name="output"></a>Ausgabe:

```json
{
  "timestamp": 145666599533564,
  "inferences": [
    {
      "type": "entity",
      "inferenceId": "5b8076753b8c47bba8c72a7e0f7c5cc0",
      "entity": {
        "tag": {
          "value": "person",
          "confidence": 0.9458008
        },
        "box": {
          "l": 0.474487,
          "t": 0.26522297,
          "w": 0.066929355,
          "h": 0.2828749
        }
      },
      "extensions": {
        "centerGroundPointX": "0.0",
        "centerGroundPointY": "0.0",
        "footprintX": "inf",
        "footprintY": "inf"
      }
    },
    {
      "type": "event",
      "inferenceId": "fb309c9285f94f268378540b5fbbf5ad",
      "relatedInferences": ["5b8076753b8c47bba8c72a7e0f7c5cc0"],
      "event": {
        "name": "personCountEvent",
        "properties": {
          "personCount": "1.0",
          "zone": "demo"
        }
      }
    }
  ]
}
```

### <a name="custom"></a>Benutzerdefiniert

#### <a name="parameters"></a>Parameter:

| Name                   | Typ   | BESCHREIBUNG                           |
| ---------------------- | ------ | ------------------------------------- |
| extensionConfiguration | Zeichenfolge | JSON-Darstellung des Vorgangs |

#### <a name="output"></a>Ausgabe:

```json
{
  "timestamp": 145666599533564,
  "inferences": [
    {
      "type": "entity",
      "inferenceId": "5b8076753b8c47bba8c72a7e0f7c5cc0",
      "entity": {
        "tag": {
          "value": "person",
          "confidence": 0.9458008
        },
        "box": {
          "l": 0.474487,
          "t": 0.26522297,
          "w": 0.066929355,
          "h": 0.2828749
        }
      },
      "extensions": {
        "centerGroundPointX": "0.0",
        "centerGroundPointY": "0.0",
        "footprintX": "inf",
        "footprintY": "inf"
      }
    },
    {
      "type": "event",
      "inferenceId": "fb309c9285f94f268378540b5fbbf5ad",
      "relatedInferences": ["5b8076753b8c47bba8c72a7e0f7c5cc0"],
      "event": {
        "name": "personCountEvent",
        "properties": {
          "personCount": "1.0",
          "zone": "demo"
        }
      }
    }
  ]
}
```

</details>

## <a name="video-player"></a>Videoplayer

Sie können einen Videoplayer verwenden, um sich das generierte Video einschließlich der Rückschlüsse (umgebende Rechtecke) anzusehen:

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/spatial-analysis/inference.png" alt-text="Umgebende Rechtecke":::

## <a name="troubleshooting"></a>Problembehandlung

„spatialanalysis“ ist ein großer Container, dessen Startzeit bis zu 30 Sekunden betragen kann. Sobald der Container „spatialanalysis“ ausgeführt wird und verfügbar ist, werden die Rückschlussereignisse gesendet.

```JSON
[IoTHubMonitor] [3:37:28 PM] Message received from [ase03-edge/avaedge]:
{
  "sdp&quot;: &quot;SDP:\nv=0\r\no=- 1620671848135494 1 IN IP4 172.27.86.122\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/cafeteria.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.08.19\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-300.066\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/cafeteria.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=640028;sprop-parameter-sets=Z2QAKKzZQHgCHoQAAAMABAAAAwDwPGDGWA==,aOvssiw=\r\na=control:track1\r\n"
}
[IoTHubMonitor] [3:37:30 PM] Message received from [ase03-edge/avaedge]:
{
  "type": "video",
  "location": "/videos/customoperation-05102021",
  "startTime&quot;: &quot;2021-05-10T18:37:27.931Z"
}
[IoTHubMonitor] [3:37:40 PM] Message received from [ase03-edge/avaedge]:
{
  "state&quot;: &quot;initializing"
}
[IoTHubMonitor] [3:37:50 PM] Message received from [ase03-edge/avaedge]:
{
  "state&quot;: &quot;initializing"
}
[IoTHubMonitor] [3:38:18 PM] Message received from [ase03-edge/avaedge]:
{
  "type": "video",
  "location": "/videos/customoperation-05102021",
  "startTime&quot;: &quot;2021-05-10T18:37:27.931Z"
}
[IoTHubMonitor] [3:38:42 PM] Message received from [ase03-edge/avaedge]:
{
  "timestamp": 145860472980260,
  "inferences": [
    {
      "type": "entity",
      "inferenceId": "647aacf9d8bc47078a1ed31d1c459c24",
      "entity": {
        "tag": {
          "value": "person",
          "confidence": 0.7583008
        },
        "box": {
          "l": 0.48213565,
          "t": 0.21217245,
          "w": 0.056364775,
          "h": 0.29961595
        }
      },
      "extensions": {
        "centerGroundPointY": "0.0",
        "centerGroundPointX": "0.0",
        "footprintX": "0.5087100982666015",
        "footprintY&quot;: &quot;0.49634415356080924"
      }
    },
    {
      "type": "event",
      "inferenceId": "dae6c2b742634196b615c128654845dc",
      "relatedInferences": [
        "647aacf9d8bc47078a1ed31d1c459c24"
      ],
      "event": {
        "name": "personCountEvent",
        "properties": {
          "personCount": "1.0",
          "zone&quot;: &quot;stairlanding"
        }
      }
    }
  ]
}

```

> [!NOTE]
> Unter Umständen werden Initialisierungsmeldungen angezeigt. Diese Meldungen erscheinen während des Starts des Moduls „spatialAnalysis“, und es kann bis zu 60 Sekunden dauern, bis der Zustand „Wird ausgeführt“ erreicht ist. Wenn Sie ein wenig Geduld haben, sollte der Durchlauf des Rückschlussereignisses zu sehen sein.

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie verschiedene Vorgänge des Moduls `spatialAnalysis` aus. Weitere Informationen finden Sie in den folgenden Pipelinetopologien:

- [personCount](https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/spatial-analysis/person-count-operation-topology.json)
- [personDistance](https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/spatial-analysis/person-distance-pperation-topology.json)
- [personCrossingLine](https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/spatial-analysis/person-line-crossing-operation-topology.json)
- [personZoneCrossing](https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/spatial-analysis/person-zone-crossing-operation-topology.json)
- [customOperation](https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/spatial-analysis/custom-operation-topology.json)

> [!Tip]
> Verwenden Sie eine **[Beispielvideodatei](https://lvamedia.blob.core.windows.net/public/2018-03-07.16-50-00.16-55-00.school.G421.mkv)** mit mehreren Personen im Frame.
