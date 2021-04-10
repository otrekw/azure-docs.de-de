---
title: Bereitstellen eines GPU-Moduls auf einem Azure Stack Edge Pro-GPU-Gerät | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Compute aktivieren und Ihr Azure Stack Edge Pro-Gerät über die lokale Benutzeroberfläche für Compute bereit machen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: af44912edf3ce98ceb71bd34388543f7652c2181
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105568459"
---
# <a name="deploy-a-gpu-enabled-iot-module-on-azure-stack-edge-pro-gpu-device"></a>Bereitstellen eines GPU-fähigen IoT-Moduls auf einem Azure Stack Edge Pro-GPU-Gerät

[!INCLUDE [applies-to-GPU-and-pro-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-sku.md)]

In diesem Artikel wird beschrieben, wie Sie ein GPU-fähiges IoT Edge-Modul auf Ihrem Azure Stack Edge Pro-GPU-Gerät bereitstellen. 

In diesem Artikel werden folgende Vorgehensweisen behandelt:
  - Bereiten Sie Azure Stack Edge Pro auf das Ausführen eines GPU-Moduls vor.
  - Laden Sie Beispielcode aus einem Git-Repository herunter, und installieren Sie ihn.
  - Erstellen Sie die Lösung, und generieren Sie ein Bereitstellungsmanifest.
  - Stellen Sie die Lösung auf dem Azure Stack Edge Pro-Gerät bereit.
  - Überwachen Sie die Modulausgabe.


## <a name="about-sample-module"></a>Informationen zum Beispielmodul

Das GPU-Beispielmodul in diesem Artikel enthält PyTorch und TensorFlow zum Vergleichen von Beispielcode für CPU und GPU.

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich zunächst, dass Sie über Folgendes verfügen:

- Sie haben Zugriff auf ein GPU-fähiges Azure Stack Edge Pro-Gerät mit einem Knoten. Dieses Gerät wird mit einer Ressource in Azure aktiviert. Weitere Informationen finden Sie unter [Aktivieren des Geräts](azure-stack-edge-gpu-deploy-activate.md).
- Sie haben Compute auf diesem Gerät konfiguriert. Führen Sie die Schritte im [Tutorial: Konfigurieren von Compute auf einem Azure Stack Edge Pro-Gerät](azure-stack-edge-gpu-deploy-configure-compute.md) aus.
- Eine Azure Container Registry (ACR). Wechseln Sie zum Blatt **Zugriffsschlüssel**, und notieren Sie sich ACR-Anmeldeserver, -Benutzername und -Kennwort. Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer privaten Containerregistrierung im Azure-Portal](../container-registry/container-registry-get-started-portal.md#create-a-container-registry).
- Die folgenden Entwicklungsressourcen auf einem Windows-Client:
    - [Azure CLI 2.0 oder höher](https://aka.ms/installazurecliwindows)
    - [Docker CE](https://store.docker.com/editions/community/docker-ce-desktop-windows). Möglicherweise müssen Sie ein Konto erstellen, um die Software herunterzuladen und zu installieren.
    - [Visual Studio Code](https://code.visualstudio.com/)  
    - [Azure IoT Edge-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)    
    - [Python-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python)    
    - [Python 3](https://www.python.org/)    
    - Pip zum Installieren von Python-Paketen (normalerweise in der Python-Installation enthalten)

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

1. Wechseln Sie auf GitHub zu [Azure-Samples/azure-intelligent-edge-patterns](https://github.com/azure-samples/azure-intelligent-edge-patterns). Klonen Sie unter „Code“ die ZIP-Datei, oder laden Sie sie herunter. 

    ![ZIP-Datei herunterladen](media/azure-stack-edge-gpu-deploy-sample-module/download-zip-file-1.png)

    Extrahieren Sie die Dateien aus der ZIP-Datei. Sie können auch die Beispiele klonen.

    ```json
    git clone https://github.com/Azure-Samples/azure-intelligent-edge-patterns.git
    ```

## <a name="build-and-deploy-module"></a>Erstellen und Bereitstellen des Moduls

1. Öffnen Sie in Visual Studio Code den Ordner **GpuReferenceModules**.

    ![GPUReferenceModules in VS Code öffnen](media/azure-stack-edge-gpu-deploy-sample-module/open-folder-gpu-sample-1.png)

2. Öffnen Sie die Datei *deployment.template.json*, und bestimmen Sie die Parameter, auf die sie für die Containerregistrierung verweist. In der folgenden Datei werden CONTAINER_REGISTRY_USERNAME, CONTAINER_REGISTRY_PASSWORD und CONTAINER_REGISTRY_NAME verwendet.

    ```json
        {
      "$schema-template": "2.0.0",
      "modulesContent": {
        "$edgeAgent": {
          "properties.desired": {
            "schemaVersion": "1.0",
            "runtime": {
              "type": "docker",
              "settings": {
                "minDockerVersion": "v1.25",
                "loggingOptions": "",
                "registryCredentials": {
                  "${CONTAINER_REGISTRY_NAME}":{
                  "username": "$CONTAINER_REGISTRY_USERNAME",
                  "password": "$CONTAINER_REGISTRY_PASSWORD",
                  "address": "${CONTAINER_REGISTRY_NAME}.azurecr.io"
                  }
                }
              }
            },
    ```
3. Erstellen Sie eine neue Datei. Füllen Sie die Werte der (im vorherigen Schritt bestimmten) Parameter Ihrer Containerregistrierung wie folgt aus: 

    ```json
    CONTAINER_REGISTRY_NAME=<YourContainerRegistryName>
    CONTAINER_REGISTRY_USERNAME=<YourContainerRegistryUserName>
    CONTAINER_REGISTRY_PASSWORD=<YourContainerRegistryPassword>
    ```
    Es folgt ein Beispiel einer *ENV*-Datei:
    
    ![Erstellen und Speichern einer ENV-Datei](media/azure-stack-edge-gpu-deploy-sample-module/create-save-env-file-1.png)

4. Speichern Sie die Datei mit der Erweiterung *.env* im Ordner **SampleSolution**.

5. Geben Sie zum Anmelden bei Docker im integrierten Terminal von Visual Studio Code den folgenden Befehl ein. 

    ```json
    docker login -u <CONTAINER_REGISTRY_USERNAME> -p <CONTAINER_REGISTRY_PASSWORD> <CONTAINER_REGISTRY_NAME>
    ```
    Wechseln Sie im Azure-Portal zum Abschnitt **Zugriffsschlüssel** Ihrer Containerregistrierung. Kopieren und verwenden Sie den Namen der Registrierung, das Kennwort und den Anmeldeserver.

    ![Zugriffsschlüssel in ihrer Containerregistrierung](media/azure-stack-edge-gpu-deploy-sample-module/container-registry-access-keys-1.png)

    Nach Eingabe der Anmeldeinformationen ist die Anmeldung erfolgreich.

    ![Erfolgreiche Anmeldung](media/azure-stack-edge-gpu-deploy-sample-module/successful-sign-in-1.png)

6. Pushen Sie Ihr Image in Ihre Azure Container Registry-Instanz. Klicken Sie im VS Code-Explorer mit der rechten Maustaste auf die Datei **deployment.template.json**, und wählen Sie **Build and Push IoT Edge Solution** (IoT Edge-Projektmappe erstellen und übertragen) aus. 

    ![Erstellen und Pushen der IoT Edge-Projektmappe](media/azure-stack-edge-gpu-deploy-sample-module/build-push-iot-edge-solution-1.png)   

    Wenn Python und die Python-Erweiterung nicht installiert sind, werden diese installiert, sobald Sie die Lösung erstellen und pushen. Dies führt jedoch zu längeren Buildzeiten. 

    Nach diesem Schritt wird das Modul in Ihrer Containerregistrierung angezeigt.

    ![Modul in Containerregistrierung](media/azure-stack-edge-gpu-deploy-sample-module/module-container-registry-1.png)    


7. Um ein Bereitstellungsmanifest zu erstellen, klicken Sie mit der rechten Maustaste auf die Datei **deployment.template.json** und wählen dann **Generate IoT Edge Deployment Manifest** (IoT Edge-Bereitstellungsmanifest generieren) aus. 

    ![Generieren des IoT Edge-Bereitstellungsmanifest](media/azure-stack-edge-gpu-deploy-sample-module/generate-iot-edge-deployment-manifest-1.png)  

    Die Benachrichtigung informiert Sie über den Pfad, in dem das Bereitstellungsmanifest generiert wurde. Das Manifest ist die Datei `deployment.amd64.json`, die im Ordner **config** generiert wurde. 

8. Wählen Sie die Datei **deployment.amd64.json** im Ordner **config** und dann **Create Deployment for Single Device (Bereitstellung für einzelnes Gerät erstellen)** aus. Verwenden Sie nicht die Datei **deployment.template.json**. 

    ![Erstellen einer Bereitstellung für ein einzelnes Gerät](media/azure-stack-edge-gpu-deploy-sample-module/create-deployment-single-device-1.png)  

    Im Fenster **Ausgabe** sollte die Meldung angezeigt werden, dass die Bereitstellung erfolgreich war.

    ![„Bereitstellung erfolgreich“ in „Ausgabe“](media/azure-stack-edge-gpu-deploy-sample-module/deployment-succeeded-output-1.png) 

## <a name="monitor-the-module"></a>Überwachen des Moduls  

1. Führen Sie in der Befehlspalette von VS Code den Befehl **Azure IoT Hub: Select IoT Hub** (Azure IoT Hub: IoT Hub auswählen) aus.

2. Wählen Sie das Abonnement und den IoT-Hub aus, die das zu konfigurierende IoT Edge-Gerät enthalten. Wählen Sie in diesem Fall das Abonnement aus, mit dem das Azure Stack Edge Pro-Gerät bereitgestellt wurde, und dann das für Ihr Azure Stack Edge Pro-Gerät erstellte IoT Edge-Gerät. Dies erfolgt, wenn Sie Compute über das Azure-Portal in den vorherigen Schritten konfigurieren.

3. Klappen Sie im Visual Studio Code-Explorer den Abschnitt „Azure IoT Hub“ auf. Unter **Geräte** sollte das IoT Edge-Gerät angezeigt werden, das Ihrem Azure Stack Edge Pro-Gerät entspricht. 

    1. Wählen Sie dieses Gerät aus, klicken Sie mit der rechten Maustaste, und wählen Sie **Überwachung des integrierten Ereignisendpunkts starten** aus.
  
        ![Starten der Überwachung](media/azure-stack-edge-gpu-deploy-sample-module/monitor-builtin-event-endpoint-1.png)  

    2. Wechseln Sie zu **Geräte > Module**. Hier sollte angezeigt werden, dass Ihr **GPU-Modul** ausgeführt wird.

        ![Modul in IoT Hub](media/azure-stack-edge-gpu-deploy-sample-module/module-iot-hub-1.png)  

    3. Im VS Code-Terminal sollten die IoT-Hub-Ereignisse auch als Überwachungsausgabe für Ihr Azure Stack Edge Pro-Gerät angezeigt werden.

        ![Überwachungsausgabe](media/azure-stack-edge-gpu-deploy-sample-module/monitor-events-output-1.png) 

        Wie Sie sehen, ist die Zeit, die für die Ausführung derselben Gruppe von Vorgängen (5.000 Iterationen von Formtransformationen) von der GPU benötigt wird, wesentlich kürzer als bei der CPU.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr zum [Konfigurieren der GPU für die Verwendung eines Moduls](./azure-stack-edge-gpu-configure-gpu-modules.md).