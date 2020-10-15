---
title: Ausführen eines GPU-Moduls auf einem Microsoft Azure Stack Edge Pro-GPU-Gerät | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal ein Modul auf dem Grafikprozessor (Graphics Processing Unit, GPU) Ihres Azure Stack Edge Pro-Geräts konfigurieren und ausführen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/25/2020
ms.author: alkohli
ms.openlocfilehash: 8b9f1180639f638e72fdea2f87958628a2e9e86b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90891468"
---
# <a name="configure-and-run-a-module-on-gpu-on-azure-stack-edge-pro-device"></a>Konfigurieren und Ausführen eines Moduls auf dem GPU Ihres Azure Stack Edge Pro-Geräts

Ihr Azure Stack Edge Pro-Gerät enthält einen oder mehrere Grafikprozessoren (GPUs). Da GPUs eine Parallelverarbeitung ermöglichen und ein schnelleres Bildrendering bieten als CPUs (Central Processing Unit, Zentralprozessor), werden GPUs häufig für KI-Berechnungen verwendet. Weitere Informationen zur GPU Ihres Azure Stack Edge Pro-Geräts finden Sie in den [technischen Spezifikationen für Ihr Azure Stack Edge Pro-Gerät](azure-stack-edge-gpu-technical-specifications-compliance.md).

In diesem Artikel wird beschrieben, wie Sie ein Modul auf dem Grafikprozessor (GPU) Ihres Azure Stack Edge Pro-Geräts konfigurieren und ausführen. In diesem Artikel wird das öffentlich verfügbare Containermodul **Digits** verwendet, das für Nvidia T4-GPUs geschrieben wurde. Diese Vorgehensweise kann zur Konfiguration beliebiger Module verwendet werden, die von Nvidia für diese GPUs veröffentlicht wurden.


## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

1. Sie haben Zugriff auf ein GPU-fähiges Azure Stack Edge Pro-Gerät mit einem Knoten. Dieses Gerät wurde mit einer Ressource in Azure aktiviert.  

## <a name="configure-module-to-use-gpu"></a>Konfigurieren eines Moduls für die Verwendung einer GPU

Führen Sie die folgenden Schritte aus, um ein Modul für die Verwendung des GPU auf Ihrem Azure Stack Edge Pro-Gerät zu konfigurieren.

1. Navigieren Sie im Azure-Portal zur Ressource, die mit Ihrem Gerät verknüpft ist. 

2. Navigieren Sie zu **Edgecomputing > Erste Schritte**. Wählen Sie auf der Kachel **Edgecomputing konfigurieren** die Option „Konfigurieren“ aus.

    ![Konfigurieren eines Moduls für die Verwendung einer GPU 1](media/azure-stack-edge-j-series-configure-gpu-modules/configure-compute-1.png)

3. Führen Sie auf dem Blatt **Edgecomputing konfigurieren** folgende Schritte aus:

    1. Wählen Sie für **IoT Hub** die Option **Neu erstellen** aus.
    2. Geben Sie einen Namen für die IoT Hub-Ressource ein, die für Ihr Gerät erstellt werden soll. Um einen Free-Tarif zu verwenden, wählen Sie eine vorhandene Ressource aus. 
    3. Notieren Sie sich das IoT Edge-Gerät und das IoT Gateway-Gerät, das mit der IoT Hub-Ressource erstellt wird. Sie benötigen diese Informationen in späteren Schritten.

    ![Konfigurieren eines Moduls für die Verwendung einer GPU 2](media/azure-stack-edge-j-series-configure-gpu-modules/configure-compute-2.png)

4. Der Vorgang zum Erstellen der IoT Hub-Ressource dauert einige Minuten. Nachdem die Ressource erstellt wurde, wählen Sie auf der Kachel **Edgecomputing konfigurieren** die Option **Konfiguration anzeigen** aus, um die Details der IoT Hub-Ressource anzuzeigen.

    ![Konfigurieren eines Moduls für die Verwendung einer GPU 4](media/azure-stack-edge-j-series-configure-gpu-modules/configure-compute-4.png)

5. Navigieren Sie zu **Automatische Geräteverwaltung > IoT Edge**.

    ![Konfigurieren eines Moduls für die Verwendung einer GPU 6](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-2.png)

    Auf der rechten Seite sehen Sie das IoT Edge-Gerät, das mit Ihrem Azure Stack Edge Pro-Gerät verknüpft ist. Dies ist das IoT Edge-Gerät, das im vorherigen Schritt beim Erstellen der IoT Hub-Ressource erstellt wurde. 
    
6. Wählen Sie dieses IoT Edge-Gerät aus.

   ![Konfigurieren eines Moduls für die Verwendung einer GPU 7](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-3.png)

7.  Wählen Sie **Module festlegen** aus.

    ![Konfigurieren eines Moduls für die Verwendung einer GPU 8](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-4.png)

8. Wählen Sie **+ Hinzufügen** und dann **+ IoT Edge-Modul** aus. 

    ![Konfigurieren eines Moduls für die Verwendung einer GPU 9](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-5.png)

9. Führen Sie auf der Registerkarte **IoT Edge-Modul hinzufügen** folgende Schritte aus:

    1. Geben Sie den **Bild-URI** an. In diesem Beispiel wird das öffentlich verfügbare Nvidia-Modul **Digits** verwendet. 
    
    2. Legen Sie für **Neustartrichtlinie** die Option **Immer** fest.
    
    3. Legen Sie für **Gewünschter Zustand** die Option **Wird ausgeführt** fest.
    
    ![Konfigurieren eines Moduls für die Verwendung einer GPU 10](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-6.png)

10. Geben Sie auf der Registerkarte **Umgebungsvariablen** den Variablennamen und den entsprechenden Wert an. 

    1. Damit das aktuelle Modul eine GPU auf diesem Gerät verwendet, verwenden Sie NVIDIA_VISIBLE_DEVICES. 

    2. Legen Sie den Wert auf 0 oder 1 fest. Dadurch wird sichergestellt, dass mindestens eine GPU des Geräts für dieses Modul verwendet wird. Wenn Sie den Wert auf 0, 1 festlegen, bedeutet dies, dass beide GPUs auf Ihrem Gerät von diesem Modul verwendet werden.

        ![Konfigurieren eines Moduls für die Verwendung einer GPU 11](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-7.png)

        Weitere Informationen zu Umgebungsvariablen, die Sie mit der Nvidia-GPU verwenden können, finden Sie unter [nvidia-container-runtime](https://github.com/NVIDIA/nvidia-container-runtime#environment-variables-oci-spec).

    > [!NOTE]
    > Eine GPU kann jeweils nur einem Modul zugeordnet werden. Ein Modul kann jedoch eine, beide oder keine GPUs verwenden. 

11. Geben Sie einen Namen für Ihr Modul ein. An dieser Stelle können Sie bei Bedarf eine Option zur Containererstellung angeben und die Modulzwillingseinstellungen ändern. Wenn Sie fertig sind, wählen Sie **Hinzufügen** aus. 

    ![Konfigurieren eines Moduls für die Verwendung einer GPU 12](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-8.png)

12. Stellen Sie sicher, dass das Modul ausgeführt wird, und wählen Sie **Überprüfen + Erstellen** aus.    

    ![Konfigurieren eines Moduls für die Verwendung einer GPU 13](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-9.png)

13. Auf der Registerkarte **Überprüfen + Erstellen** werden die Bereitstellungsoptionen angezeigt, die Sie ausgewählt haben. Überprüfen Sie die Optionen, und wählen Sie **Erstellen** aus.
    
    ![Konfigurieren eines Moduls für die Verwendung einer GPU 14](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-10.png)

14. Achten Sie auf den **Laufzeitstatus** des Moduls. 
    
    ![Konfigurieren eines Moduls für die Verwendung einer GPU 15](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-11.png)

    Der Vorgang zur Bereitstellung des Moduls dauert einige Minuten. Wählen Sie **Aktualisieren** aus. Der **Laufzeitstatus** sollte in **Wird ausgeführt** geändert werden.

    ![Konfigurieren eines Moduls für die Verwendung einer GPU 16](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-12.png)


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Umgebungsvariablen, die mit der Nvidia-GPU verwendet werden können](https://github.com/NVIDIA/nvidia-container-runtime#environment-variables-oci-spec).
