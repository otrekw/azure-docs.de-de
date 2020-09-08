---
title: Tutorial zum Aktivieren eines Azure Stack Edge-Geräts mit GPU im Azure-Portal | Microsoft-Dokumentation
description: Im Tutorial zur Bereitstellung von Azure Stack Edge mit GPU erfahren Sie, wie Sie Ihr physisches Gerät aktivieren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 08/31/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to activate Azure Stack Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 88be4d9753e48f70dae5666e800a54209ed6ba3f
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89267937"
---
# <a name="tutorial-activate-azure-stack-edge-with-gpu"></a>Tutorial: Aktivieren von Azure Stack Edge mit GPU

In diesem Tutorial wird erläutert, wie Sie Ihr Azure Stack Edge-Gerät mit eingebauter GPU auf der lokalen Webbenutzeroberfläche aktivieren können.

Der Aktivierungsvorgang dauert ungefähr 5 Minuten.

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Voraussetzungen
> * Aktivieren des physischen Geräts

## <a name="prerequisites"></a>Voraussetzungen

Überprüfen Sie Folgendes, bevor Sie ein Azure Stack Edge-Gerät mit GPU konfigurieren und einrichten:

* Für Ihr physisches Gerät: 
    
    - Sie haben Ihr physisches Gerät gemäß der Anleitung unter [Tutorial: Installieren von Azure Stack Edge](azure-stack-edge-gpu-deploy-install.md) installiert.
    - Sie haben die Netzwerk- und Computenetzwerkeinstellungen gemäß [Konfigurieren von Netzwerk, Computenetzwerk und Webproxy](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md) konfiguriert.
    - Sie haben Ihre eigenen Gerätezertifikate hochgeladen oder auf Ihrem Gerät generiert, sofern Sie den Gerätenamen oder die DNS-Domäne auf der Seite **Gerät** geändert haben. Falls Sie diesen Schritt nicht durchgeführt haben, wird während der Aktivierung des Geräts ein Fehler angezeigt, und die Aktivierung wird blockiert. Weitere Informationen finden Sie unter [Konfigurieren von Zertifikaten](azure-stack-edge-gpu-deploy-configure-certificates.md).
    
* Sie verfügen über den Aktivierungsschlüssel vom Azure Stack Edge-Dienst, den Sie zum Verwalten von Azure Stack Edge-Geräten erstellt haben. Weitere Informationen finden Sie unter [Vorbereiten der Bereitstellung von Azure Stack Edge](azure-stack-edge-gpu-deploy-prep.md).


## <a name="activate-the-device"></a>Aktivieren des Geräts

1. Navigieren Sie auf der lokalen Webbenutzeroberfläche des Geräts zur Seite **Erste Schritte**.
2. Wählen Sie auf der Kachel **Aktivierung** die Option **Aktivieren** aus. 

    ![Seite „Clouddetails“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-activate/activate-1.png)
    
3. Geben Sie im Bereich **Aktivieren** den **Aktivierungsschlüssel** ein, den Sie unter [Aktivierungsschlüssel für Azure Stack Edge abrufen](azure-stack-edge-gpu-deploy-prep.md#get-the-activation-key) abgerufen haben.

4. Wählen Sie **Übernehmen**.

    ![Seite „Clouddetails“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-activate/activate-2.png)


5. Zunächst wird das Gerät aktiviert. Sie werden dann aufgefordert, die Schlüsseldatei herunterzuladen.
    
    ![Seite „Clouddetails“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-activate/activate-3.png)
    
    Wählen Sie **Schlüsseldatei herunterladen** aus, und speichern Sie die Datei *keys.json* an einem sicheren Speicherort außerhalb des Geräts. **Diese Schlüsseldatei enthält die Wiederherstellungsschlüssel für den Betriebssystem-Datenträger und die anderen Datenträger auf Ihrem Gerät**. Es folgt der Inhalt der Datei *keys.json*:

        
    ```json
    {
      "Id": "1ab3fe39-26e6-4984-bb22-2e02d3fb147e",
      "DataVolumeBitLockerExternalKeys": {
        "hcsinternal": "C086yg1DrPo0DuZB/a7hUh+kBWj804coJfBA9LDzZqw=",
        "hcsdata": "8ohX9bG3YSZl9DZmZLkYl//L9dXi1XiQrqza+iSd64Q="
      },
      "SystemVolumeBitLockerRecoveryKey": "105347-156739-594473-151107-005082-252604-471955-439395",
      "ServiceEncryptionKey": "oEwxNJeULzGRFt6DsLgcLw=="
    }
    ```
        
 
    In der folgenden Tabelle werden die einzelnen Schlüssel näher erläutert:
    
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

Weitere Informationen zum Übertragen von Daten mit Azure Stack Edge finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Übertragen von Daten mit Azure Stack Edge](./azure-stack-edge-j-series-deploy-add-shares.md)
