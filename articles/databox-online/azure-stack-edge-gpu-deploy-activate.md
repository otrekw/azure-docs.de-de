---
title: Tutorial zum Aktivieren eines Azure Stack Edge Pro-Geräts mit GPU im Azure-Portal | Microsoft-Dokumentation
description: Im Tutorial zur Bereitstellung von Azure Stack Edge Pro mit GPU erfahren Sie, wie Sie Ihr physisches Gerät aktivieren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 09/10/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to activate Azure Stack Edge Pro so I can use it to transfer data to Azure.
ms.openlocfilehash: 15680a4f8228af95e6643478c9262653171912ca
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90903451"
---
# <a name="tutorial-activate-azure-stack-edge-pro-with-gpu"></a>Tutorial: Aktivieren von Azure Stack Edge Pro mit GPU

In diesem Tutorial wird erläutert, wie Sie Ihr Azure Stack Edge Pro-Gerät mit eingebauter GPU über die lokale Webbenutzeroberfläche aktivieren.

Der Aktivierungsvorgang dauert ungefähr 5 Minuten.

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Voraussetzungen
> * Aktivieren des physischen Geräts

## <a name="prerequisites"></a>Voraussetzungen

Überprüfen Sie Folgendes, bevor Sie ein Azure Stack Edge Pro-Gerät mit GPU konfigurieren und einrichten:

* Für Ihr physisches Gerät: 
    
    - Sie haben Ihr physisches Gerät gemäß der Anleitung unter [Installieren von Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-install.md) installiert.
    - Sie haben die Netzwerk- und Computenetzwerkeinstellungen gemäß [Konfigurieren von Netzwerk, Computenetzwerk und Webproxy](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md) konfiguriert.
    - Sie haben Ihre eigenen Gerätezertifikate hochgeladen oder auf Ihrem Gerät generiert, sofern Sie den Gerätenamen oder die DNS-Domäne auf der Seite **Gerät** geändert haben. Falls Sie diesen Schritt nicht durchgeführt haben, wird während der Aktivierung des Geräts ein Fehler angezeigt, und die Aktivierung wird blockiert. Weitere Informationen finden Sie unter [Konfigurieren von Zertifikaten](azure-stack-edge-gpu-deploy-configure-certificates.md).
    
* Sie verfügen über den Aktivierungsschlüssel aus dem Azure Stack Edge-Dienst, den Sie zum Verwalten des Azure Stack Edge Pro-Geräts erstellt haben. Weitere Informationen finden Sie unter [Vorbereiten der Bereitstellung von Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-prep.md).


## <a name="activate-the-device"></a>Aktivieren des Geräts

1. Navigieren Sie auf der lokalen Webbenutzeroberfläche des Geräts zur Seite **Erste Schritte**.
2. Wählen Sie auf der Kachel **Aktivierung** die Option **Aktivieren** aus. 

    ![Seite „Clouddetails“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-activate/activate-1.png)
    
3. Geben Sie im Bereich **Aktivieren** den **Aktivierungsschlüssel** ein, den Sie unter [Aktivierungsschlüssel für Azure Stack Edge Pro abrufen](azure-stack-edge-gpu-deploy-prep.md#get-the-activation-key) abgerufen haben.

4. Wählen Sie **Übernehmen**.

    ![Seite „Clouddetails“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-activate/activate-2.png)


5. Zunächst wird das Gerät aktiviert. Sie werden dann aufgefordert, die Schlüsseldatei herunterzuladen.
    
    ![Seite „Clouddetails“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-activate/activate-3.png)
    
    Wählen Sie **Herunterladen und fortfahren** aus, und speichern Sie die Datei *device-serial-no.json* an einem sicheren Speicherort außerhalb des Geräts. **Diese Schlüsseldatei enthält die Wiederherstellungsschlüssel für den Betriebssystem-Datenträger und die anderen Datenträger auf Ihrem Gerät**. Diese Schlüssel werden möglicherweise für eine zukünftige Systemwiederherstellung benötigt.

    Nachfolgend ist der Inhalt der *json*-Datei gezeigt:

        
    ```json
    {
      "Id": "<Device ID>",
      "DataVolumeBitLockerExternalKeys": {
        "hcsinternal": "<BitLocker key for data disk>",
        "hcsdata": "<BitLocker key for data disk>"
      },
      "SystemVolumeBitLockerRecoveryKey": "<BitLocker key for system volume>",
      "ServiceEncryptionKey": "<Azure service encryption key>"
    }
    ```
        
 
    In der folgenden Tabelle sind die verschiedenen Schlüssel erläutert:
    
    |Feld  |BESCHREIBUNG  |
    |---------|---------|
    |`Id`    | Die ID des Geräts.        |
    |`DataVolumeBitLockerExternalKeys`|Die BitLocker-Schlüssel für die Datenträger, die zur Wiederherstellung der lokalen Daten auf Ihrem Gerät verwendet werden.|
    |`SystemVolumeBitLockerRecoveryKey`| Der BitLocker-Schlüssel für das Systemvolume. Dieser Schlüssel hilft bei der Wiederherstellung der Systemkonfiguration und der Systemdaten für Ihr Gerät. |
    |`ServiceEncryptionKey`| Dieser Schlüssel schützt die durch den Azure-Dienst übertragenen Daten. Dieser Schlüssel stellt sicher, dass eine Kompromittierung des Azure-Diensts nicht zu einer Kompromittierung gespeicherter Informationen führt. |

6. Wechseln Sie zur Seite **Übersicht**. Der Gerätestatus muss als **Aktiviert** angezeigt werden.

    ![Seite „Clouddetails“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-activate/activate-4.png)
 
Die Aktivierung des Geräts ist abgeschlossen. Sie können nun Freigaben auf Ihrem Gerät hinzufügen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Voraussetzungen
> * Aktivieren des physischen Geräts

Informationen zum Übertragen von Daten mit Ihrem Azure Stack Edge Pro-Gerät finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Übertragen von Daten mit Azure Stack Edge Pro](./azure-stack-edge-j-series-deploy-add-shares.md)
