---
title: Tutorial zum Verbinden, Konfigurieren und Aktivieren eines Azure Stack Edge-Geräts mit GPU im Azure-Portal | Microsoft-Dokumentation
description: Im Tutorial zur Bereitstellung eines Azure Stack Edge-Geräts mit GPU erfahren Sie, wie Sie Ihr physisches Gerät verbinden, einrichten und aktivieren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 08/29/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Azure Stack Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 41055fbd455d3f7b9da63ee8f7420f008ea75a00
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89254507"
---
# <a name="tutorial-configure-device-settings-for-azure-stack-edge-with-gpu"></a>Tutorial: Konfigurieren von Geräteeinstellungen für Azure Stack Edge-Gerät mit GPU

In diesem Tutorial wird erläutert, wie Sie Geräteeinstellungen für Ihr Azure Stack Edge-Gerät mit eingebauter GPU konfigurieren. Sie können auf der lokalen Webbenutzeroberfläche den Gerätenamen, Updateserver und Zeitserver einrichten.

Dieser Geräteeinstellungsvorgang kann ca. 5 bis 7 Minuten dauern.

In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]
>
> * Voraussetzungen
> * Konfigurieren von Geräteeinstellungen
> * Konfigurieren des Updates 
> * Konfigurieren der Uhrzeit

## <a name="prerequisites"></a>Voraussetzungen

Überprüfen Sie Folgendes, bevor Sie Geräteeinstellungen auf Ihrem Azure Stack Edge-Gerät mit GPU konfigurieren:

* Für Ihr physisches Gerät:

    - Sie haben Ihr physisches Gerät gemäß der Anleitung unter [Tutorial: Installieren von Azure Stack Edge](azure-stack-edge-gpu-deploy-install.md) installiert.
    - Sie haben das Netzwerk konfiguriert und das Computenetzwerk auf Ihrem Gerät aktiviert und konfiguriert, wie in [ Konfigurieren des Netzwerks für Azure Stack Edge-Gerät mit GPU](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md) beschrieben.


## <a name="configure-device-settings"></a>Konfigurieren von Geräteeinstellungen

Führen Sie zum Konfigurieren von Geräteeinstellungen die folgenden Schritte aus.
 
1. Wählen Sie auf der Kachel **Geräteeinrichtung** für **Gerät** die Option **Konfigurieren** aus.

    ![Seite „Gerät“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/device-1.png)

    Führen Sie auf der Seite **Gerät** die folgenden Schritte aus:

    1. Geben Sie einen Anzeigenamen für Ihr Gerät ein. Der Anzeigename muss aus 1 bis 13 Zeichen bestehen und kann Buchstaben, Zahlen und Bindestriche enthalten.

    2. Geben Sie eine **DNS-Domäne** für Ihr Gerät an. Diese Domäne wird zum Einrichten des Geräts als Dateiserver verwendet.

    3. Klicken Sie auf **Übernehmen**, um die konfigurierten Geräteeinstellungen zu überprüfen und zu übernehmen.

        ![Seite „Gerät“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/device-2.png)

        Wenn Sie den Gerätenamen und die DNS-Domäne geändert haben, funktionieren die automatisch generierten selbstsignierten Zertifikate auf dem Gerät nicht. Bei der Konfiguration von Zertifikaten müssen Sie eine der folgenden Optionen wählen: 
        
        - Generieren Sie die Gerätezertifikate, und laden Sie sie herunter. 
        - Stellen Sie Ihre eigenen Zertifikate für das Gerät einschließlich Signaturkette bereit.
    

        ![Seite „Gerät“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/device-3.png)

    4. Bei Änderung des Gerätenamens und der DNS-Domäne werden die SMB- und NFS-Endpunkte erstellt.  

    5. Nachdem die Einstellungen übernommen wurden, wechseln Sie zurück zu **Erste Schritte**.

## <a name="configure-update"></a>Konfigurieren des Updates

1. Wählen Sie auf der Kachel **Geräteeinrichtung** für **Update** die Option **Konfigurieren** aus. Sie können nun den Speicherort konfigurieren, aus dem die Updates für Ihr Gerät heruntergeladen werden sollen.  

    ![Seite „Updateserver“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/update-1.png)

    - Sie können die Updates direkt über den **Microsoft Update-Server** beziehen.

        ![Seite „Updateserver“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/update-2.png)

        Sie können Updates auch über **Windows Server Update Services** (WSUS) bereitstellen. Geben Sie den Pfad zum WSUS-Server an.
        
        ![Seite „Updateserver“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/update-3.png)

        > [!NOTE] 
        > Wenn ein separater Windows Update-Server konfiguriert ist und Sie eine Verbindung über *https* (statt *http*) wählen, werden für die Verbindung mit dem Updateserver Signaturkettenzertifikate benötigt. Informationen zum Erstellen und Hochladen von Zertifikaten finden Sie unter [Verwalten von Zertifikaten](azure-stack-edge-j-series-manage-certificates.md). 

2. Wählen Sie **Übernehmen**.
3. Wechseln Sie nach der Konfiguration des Updateservers zurück zu **Erste Schritte**.
    

## <a name="configure-time"></a>Konfigurieren der Uhrzeit

Führen Sie die folgenden Schritte aus, um die Zeiteinstellungen auf Ihrem Gerät zu konfigurieren. 

1. Wählen Sie auf der Kachel **Geräteeinrichtung** die Option **Zeit** aus. Sie können die Zeitzone sowie den primären und sekundären NTP-Server für Ihr Gerät auswählen.  

    > [!IMPORTANT]
    > Obwohl die Zeiteinstellungen optional sind, empfehlen wir dringend, dass Sie für Ihr Gerät einen primären und sekundären NTP-Server im lokalen Netzwerk konfigurieren. Wenn der lokale Server nicht verfügbar ist, können öffentliche NTP-Server konfiguriert werden.
    
    NTP-Server sind für die Zeitsynchronisierung erforderlich, damit Ihr Gerät bei den Clouddienstanbietern authentifiziert werden kann.

    ![Seite „Zeit“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/time-1.png)

2. Gehen Sie auf der Seite **Zeit** wie folgt vor:
    
    1. Wählen Sie aus der Dropdownliste die **Zeitzone** aus, die dem geografischen Standort entspricht, an dem das Gerät bereitgestellt wird.
        Die Standardzeitzone für Ihr Gerät ist „PST“. Ihr Gerät verwendet diese Zeitzone für alle geplanten Vorgänge.

    2. Geben Sie im Feld **Primärer NTP-Server** den primären Server für das Gerät ein, oder übernehmen Sie den Standardwert „time.windows.com“.  
        Stellen Sie sicher, dass Ihr Netzwerk NTP-Datenverkehr vom Rechenzentrum ins Internet zulässt.

    3. Geben Sie optional im Feld **Sekundärer NTP-Server** einen sekundären Server für Ihr Gerät ein.

    4. Klicken Sie auf **Übernehmen**, um die konfigurierten Uhrzeiteinstellungen zu überprüfen und anzuwenden.

        ![Seite „Zeit“ auf der lokalen Webbenutzeroberfläche](./media/azure-stack-edge-gpu-deploy-set-up-device-update-time/time-2.png)

3. Nachdem die Einstellungen übernommen wurden, wechseln Sie zurück zu **Erste Schritte**.



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]
>
> * Voraussetzungen
> * Konfigurieren von Geräteeinstellungen
> * Konfigurieren des Updates 
> * Konfigurieren der Uhrzeit

Informationen zum Konfigurieren von Zertifikaten für Ihr Azure Stack Edge-Gerät finden Sie unter:

> [!div class="nextstepaction"]
> [Konfigurieren von Zertifikaten](./azure-stack-edge-gpu-deploy-configure-certificates.md)
