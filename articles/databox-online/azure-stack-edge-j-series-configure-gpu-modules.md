---
title: Ausführen eines GPU-Moduls auf einem Microsoft Azure Stack Edge Pro-GPU-Gerät | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal ein Modul auf dem Grafikprozessor (Graphics Processing Unit, GPU) Ihres Azure Stack Edge Pro-Geräts konfigurieren und ausführen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: dfae1a9b02db7e7b9577acdb47a1ba089f1609e8
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102439050"
---
# <a name="configure-and-run-a-module-on-gpu-on-azure-stack-edge-pro-device"></a>Konfigurieren und Ausführen eines Moduls auf dem GPU Ihres Azure Stack Edge Pro-Geräts

[!INCLUDE [applies-to-GPU-and-pro-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-sku.md)]

Ihr Azure Stack Edge Pro-Gerät enthält einen oder mehrere Grafikprozessoren (GPUs). Da GPUs eine Parallelverarbeitung ermöglichen und ein schnelleres Bildrendering bieten als CPUs (Central Processing Unit, Zentralprozessor), werden GPUs häufig für KI-Berechnungen verwendet. Weitere Informationen zur GPU Ihres Azure Stack Edge Pro-Geräts finden Sie in den [technischen Spezifikationen für Ihr Azure Stack Edge Pro-Gerät](azure-stack-edge-gpu-technical-specifications-compliance.md).

In diesem Artikel wird beschrieben, wie Sie ein Modul auf dem Grafikprozessor (GPU) Ihres Azure Stack Edge Pro-Geräts konfigurieren und ausführen. In diesem Artikel wird das öffentlich verfügbare Containermodul **Digits** verwendet, das für Nvidia T4-GPUs geschrieben wurde. Diese Vorgehensweise kann zur Konfiguration beliebiger Module verwendet werden, die von Nvidia für diese GPUs veröffentlicht wurden.


## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

1. Sie haben Zugriff auf ein GPU-fähiges Azure Stack Edge Pro-Gerät mit einem Knoten. Dieses Gerät wurde mit einer Ressource in Azure aktiviert.  

## <a name="configure-module-to-use-gpu"></a>Konfigurieren eines Moduls für die Verwendung einer GPU

Wenn Sie ein Modul so konfigurieren möchten, dass es den GPU auf Ihrem Azure Stack Edge Pro-Gerät zur Ausführung verwendet,<!--Can it be simplified? "To configure a module to be run by the GPU on your Azure Stack Edge Pro device,"?--> führen Sie die folgenden Schritte aus.

1. Navigieren Sie im Azure-Portal zur Ressource, die mit Ihrem Gerät verknüpft ist.

2. Wählen Sie in **Übersicht** die Option **IoT Edge** aus.

    ![Konfigurieren eines Moduls für die Verwendung einer GPU 1](media/azure-stack-edge-j-series-configure-gpu-modules/configure-compute-1.png)

3. Wählen Sie unter **IoT Edge-Dienst aktivieren** die Option **Hinzufügen** aus.

   ![Konfigurieren eines Moduls für die Verwendung einer GPU 2](media/azure-stack-edge-j-series-configure-gpu-modules/configure-compute-2.png)

4. Geben Sie unter **Create IoT Edge service** (IoT Edge-Dienst erstellen) Einstellungen für Ihre IoT Hub-Ressource ein:

   |Feld   |Wert    |
   |--------|---------|
   |Abonnement      | Von der Azure Stack Edge-Ressource verwendetes Abonnement |
   |Ressourcengruppe    | Von der Azure Stack Edge-Ressource verwendete Ressource |
   |IoT Hub           | Wählen Sie **Neu erstellen** oder **Vorhandene verwenden** aus. <br> Standardmäßig wird ein Standard-Tarif (S1) verwendet, um eine IoT-Ressource zu erstellen. Wenn Sie eine IoT-Ressource im Free-Tarif verwenden möchten, können Sie diese erstellen und dann als vorhandene Ressource auswählen. <br> Die IoT Hub-Ressource nutzt in jedem Fall dasselbe Abonnement und dieselbe Ressourcengruppe wie die Azure Stack Edge-Ressource.     |
   |Name              | Wenn Sie nicht den für eine neue IoT Hub-Ressource bereitgestellten Standardnamen verwenden möchten, geben Sie einen anderen Namen ein. |

   Wenn Sie die Einstellungen abgeschlossen haben, wählen Sie **Überprüfen + erstellen** aus. Überprüfen Sie die Einstellungen für Ihre IoT Hub-Ressource, und wählen Sie **Erstellen** aus.

   ![Erste Schritte mit Compute 2](./media/azure-stack-edge-j-series-deploy-configure-compute/configure-compute-3.png)

   Die Ressourcenerstellung für eine IoT Hub-Ressource dauert einige Minuten. Nach der Erstellung der Ressource ist in der **Übersicht** angegeben, dass der IoT Edge-Dienst jetzt ausgeführt wird.

   ![Erste Schritte mit Compute 3](./media/azure-stack-edge-j-series-deploy-configure-compute/configure-compute-4.png)

5. Wenn Sie überprüfen möchten, ob die Edge-Computerolle konfiguriert wurde, wählen Sie **Eigenschaften** aus.

   ![Erste Schritte mit Compute 4](./media/azure-stack-edge-j-series-deploy-configure-compute/configure-compute-5.png)

6. Wählen Sie in **Eigenschaften** den Link für **IoT-Edgegerät** aus.

   ![Konfigurieren eines Moduls für die Verwendung einer GPU 6](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-2.png)

   Auf der rechten Seite sehen Sie das IoT Edge-Gerät, das mit Ihrem Azure Stack Edge Pro-Gerät verknüpft ist. Dieses Gerät entspricht dem IoT Edge-Gerät, das Sie beim Erstellen der IoT Hub-Ressource erstellt haben.
 
7. Wählen Sie dieses IoT Edge-Gerät aus.

   ![Konfigurieren eines Moduls für die Verwendung einer GPU 7](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-3.png)

8. Wählen Sie **Module festlegen** aus.

   ![Konfigurieren eines Moduls für die Verwendung einer GPU 8](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-4.png)

9. Wählen Sie **+ Hinzufügen** und dann **+ IoT Edge-Modul** aus. 

    ![Konfigurieren eines Moduls für die Verwendung einer GPU 9](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-5.png)

10. Führen Sie auf der Registerkarte **IoT Edge-Modul hinzufügen** folgende Schritte aus:

    1. Geben Sie den **Bild-URI** an. In diesem Beispiel wird das öffentlich verfügbare Nvidia-Modul **Digits** verwendet. 
    
    2. Legen Sie für **Neustartrichtlinie** die Option **Immer** fest.
    
    3. Legen Sie für **Gewünschter Zustand** die Option **Wird ausgeführt** fest.
    
    ![Konfigurieren eines Moduls für die Verwendung einer GPU 10](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-6.png)

11. Geben Sie auf der Registerkarte **Umgebungsvariablen** den Variablennamen und den entsprechenden Wert an. 

    1. Damit das aktuelle Modul eine GPU auf diesem Gerät verwendet, verwenden Sie NVIDIA_VISIBLE_DEVICES. 

    2. Legen Sie den Wert auf 0 oder 1 fest. Mit einem Wert von 0 oder 1 wird sichergestellt, dass das Gerät mindestens einen GPU für dieses Modul verwendet. Wenn Sie den Wert auf 0, 1 festlegen, bedeutet dies, dass beide GPUs auf Ihrem Gerät von diesem Modul verwendet werden.

       ![Konfigurieren eines Moduls für die Verwendung einer GPU 11](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-7.png)

       Weitere Informationen zu Umgebungsvariablen, die Sie mit der Nvidia-GPU verwenden können, finden Sie unter [nvidia-container-runtime](https://github.com/NVIDIA/nvidia-container-runtime#environment-variables-oci-spec).

    > [!NOTE]
    > Eine GPU kann jeweils nur einem Modul zugeordnet werden. Ein Modul kann jedoch eine, beide oder keine GPUs verwenden.

12. Geben Sie einen Namen für Ihr Modul ein. An dieser Stelle können Sie bei Bedarf eine Option zur Containererstellung angeben und die Modulzwillingseinstellungen ändern. Wenn Sie fertig sind, wählen Sie **Hinzufügen** aus. 

    ![Konfigurieren eines Moduls für die Verwendung einer GPU 12](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-8.png)

13. Stellen Sie sicher, dass das Modul ausgeführt wird, und wählen Sie **Überprüfen + Erstellen** aus.

    ![Konfigurieren eines Moduls für die Verwendung einer GPU 13](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-9.png)

14. Auf der Registerkarte **Überprüfen + Erstellen** werden die Bereitstellungsoptionen angezeigt, die Sie ausgewählt haben. Überprüfen Sie die Optionen, und wählen Sie **Erstellen** aus.
    
    ![Konfigurieren eines Moduls für die Verwendung einer GPU 14](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-10.png)

15. Achten Sie auf den **Laufzeitstatus** des Moduls.
    
    ![Konfigurieren eines Moduls für die Verwendung einer GPU 15](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-11.png)

    Der Vorgang zur Bereitstellung des Moduls dauert einige Minuten. Wählen Sie **Aktualisieren** aus. Der **Laufzeitstatus** sollte in **Wird ausgeführt** geändert werden.

    ![Konfigurieren eines Moduls für die Verwendung einer GPU 16](media/azure-stack-edge-j-series-configure-gpu-modules/configure-gpu-12.png)


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Umgebungsvariablen, die mit der Nvidia-GPU verwendet werden können](https://github.com/NVIDIA/nvidia-container-runtime#environment-variables-oci-spec).
