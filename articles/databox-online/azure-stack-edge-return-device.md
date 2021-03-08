---
title: Zurückgeben Ihres Azure Stack Edge Pro-Geräts | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Daten löschen und Ihr Azure Stack Edge Pro-Gerät zurückgeben und dann die dem Gerät zugeordnete Ressource löschen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/26/2021
ms.author: alkohli
ms.openlocfilehash: 6c9e8938feb3c3444d01f9d37476589776707425
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101703183"
---
# <a name="return-your-azure-stack-edge-pro-device"></a>Zurückgeben Ihres Azure Stack Edge Pro-Geräts

In diesem Artikel wird beschrieben, wie Sie die Daten löschen und dann Ihr Azure Stack Edge Pro-Gerät zurückgeben. Nachdem Sie das Gerät zurückgegeben haben, können Sie auch die mit dem Gerät verknüpfte Ressource löschen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
>
> * Löschen der Daten von den Datenträgern des Geräts
> * Initiieren der Geräterückgabe im Azure-Portal
> * Packen des Geräts und Planen einer Abholung
> * Löschen der Ressourcen im Azure-Portal

## <a name="erase-data-from-the-device"></a>Löschen der Daten vom Gerät

Um die Daten von den Datenträgern Ihres Geräts zu löschen, müssen Sie Ihr Gerät zurücksetzen.

Erstellen Sie bei Bedarf vor dem Zurücksetzen eine Kopie der lokalen Daten auf dem Gerät. Sie können die Daten vom Gerät in einen Azure Storage-Container kopieren. 

Die Geräterückgabe kann bereits vor dem Zurücksetzen des Geräts initiiert werden.

Sie können Ihr Gerät über die lokale Webbenutzeroberfläche oder über PowerShell zurücksetzen. PowerShell-Anweisungen finden Sie unter [Zurücksetzen Ihres Geräts](./azure-stack-edge-connect-powershell-interface.md#reset-your-device).


[!INCLUDE] [Zurücksetzen von Daten auf dem Gerät](../../includes/azure-stack-edge-device-reset.md)

> [!NOTE]
> - Bei einem Geräteaustausch oder Upgrade auf ein neues Gerät sollten Sie Ihr Gerät erst zurücksetzen, nachdem Sie das neue Gerät erhalten haben.
> - Beim Zurücksetzen werden nur alle lokalen Daten vom Gerät gelöscht. Die Daten in der Cloud werden nicht gelöscht, und für sie fallen [Kosten](https://azure.microsoft.com/pricing/details/storage/) an. Diese Daten müssen mithilfe eines Cloudspeicher-Verwaltungstools wie [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) separat gelöscht werden.

## <a name="initiate-device-return"></a>Initiieren der Geräterückgabe

Beginnen Sie den Rückgabeprozess mit den folgenden Schritten.

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Stack Edge Pro-/Data Box Gateway-Ressource. Wählen Sie in der **Übersicht** über die Befehlsleiste im rechten Bereich die Option **Gerät zurückgeben** aus. 

    ![Zurückgeben des Geräts 1](media/azure-stack-edge-return-device/return-device-1.png)  

2. Gehen Sie auf dem Blatt **Gerät zurückgeben** unter **Grundlegende Details** wie folgt vor:

    1. Geben Sie die Seriennummer des Geräts an. Die Seriennummer des Geräts ist auf der lokalen Webbenutzeroberfläche des Geräts in der **Übersicht** angegeben.  
    
       ![Seriennummer des Geräts 1](media/azure-stack-edge-return-device/device-serial-number-1.png) 

    2. Geben Sie die Nummer des Serviceetiketts ein. Die Nummer des Serviceetiketts ist ein Bezeichner mit fünf oder mehr Zeichen, der für Ihr Gerät eindeutig ist. Das Serviceetikett befindet sich in der rechten unteren Ecke des Geräts (wenn Sie auf das Gerät blicken). Ziehen Sie das Informationsetikett heraus. Der Auszug enthält Systeminformationen wie Serviceetikett, NIC, MAC-Adresse und Ähnliches. 
    
       ![Serviceetikettnummer 1](media/azure-stack-edge-return-device/service-tag-number-1.png)

    3. Wählen Sie in der Dropdownliste einen Grund für die Rückgabe aus.

       ![Zurückgeben des Geräts 2](media/azure-stack-edge-return-device/return-device-2.png) 

3. Gehen Sie unter **Versandinformationen** wie folgt vor:

    1. Geben Sie Ihren Namen, den Namen des Unternehmens sowie die vollständige Unternehmensadresse an. Geben Sie eine geschäftliche Telefonnummer einschließlich Vorwahl sowie eine E-Mail-ID für Benachrichtigungen ein.
    2. Wenn Sie einen Karton für die Rückgabe benötigen, können Sie ihn anfordern. Antworten Sie mit **Ja** auf die Frage **Benötigen Sie einen leeren Karton für die Rückgabe?** .

    ![Zurückgeben des Geräts 3](media/azure-stack-edge-return-device/return-device-3.png)

4. Lesen Sie die **Datenschutzbestimmungen**, und aktivieren Sie das Kontrollkästchen, um zu bestätigen, dass Sie die Datenschutzbestimmungen gelesen haben und ihnen zustimmen.

5. Wählen Sie **Rückgabe initiieren** aus.

    ![Zurückgeben des Geräts 4](media/azure-stack-edge-return-device/return-device-4.png) 

6. Nach der Erfassung der Angaben für die Geräterückgabe können Sie das Azure Stack Edge Pro-Betriebsteam per E-Mail benachrichtigen. Sie können Ihre E-Mail-Anwendung verwenden, sofern diese installiert und konfiguriert ist. Sie können die Daten auch kopieren, um eine E-Mail zu erstellen und zu senden.

    ![Zurückgeben des Geräts 5](media/azure-stack-edge-return-device/return-device-5.png) 

7. Nachdem das Azure Stack Edge Pro-Betriebsteam die E-Mail erhalten hat, sendet es Ihnen ein Etikett für die Rücksendung. Nach Erhalt dieses Etiketts können Sie die Geräteabholung mit dem Versandunternehmen planen. 

## <a name="schedule-a-pickup"></a>Planen der Abholung

Führen Sie die folgenden Schritte aus, um eine Abholung zu planen:

1. Fahren Sie das Gerät herunter. Wechseln Sie auf der lokalen Webbenutzeroberfläche zu **Wartung > Power settings** (Energieeinstellungen).
2. Wählen Sie **Herunterfahren** aus. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja**, um fortzufahren. Weitere Informationen finden Sie unter [Verwalten der Energieeinstellungen](../databox-gateway/data-box-gateway-manage-access-power-connectivity-mode.md#manage-power).
3. Trennen Sie die Netzkabel, und entfernen Sie alle Netzwerkkabel vom Gerät.
4. Bereiten Sie das Versandpaket vor, indem Sie Ihren eigenen Karton verwenden oder den leeren, den Sie von Azure erhalten haben. Bringen Sie das Gerät und die mitgelieferten Netzkabel im Karton unter.
5. Bringen Sie das Adressetikett, das Sie von Azure erhalten haben, auf dem Paket an.
6. Planen Sie eine Abholung mit Ihrem regionalen Paketdienst. Bei Rückgabe des Geräts in den USA ist dies UPS oder FedEx. So planen Sie eine Abholung durch UPS

    1. Rufen Sie Ihre lokale UPS-Versandstelle an (landes-/regionsspezifische gebührenfreie Telefonnummer).
    2. Geben Sie bei dem Telefonat die Nachverfolgungsnummer für die Rücksendung an, die Sie auf dem gedruckten Etikett finden.
    3. Wenn Sie keine Nachverfolgungsnummer angeben, fordert UPS eine Zusatzgebühr, die Sie bei der Abholung entrichten müssen.

    Wenn Sie keine Abholung planen möchten, können Sie das Azure Stack Edge Pro-Gerät auch an der nächstgelegenen Versandstelle abgeben.

## <a name="delete-the-resource"></a>Löschen der Ressource

Nachdem das Gerät im Azure-Rechenzentrum eingegangen ist, wird das Gerät auf Beschädigungen oder Anzeichen von Manipulation untersucht.

- Wenn das Gerät intakt und in gutem Zustand eintrifft, endet die Abrechnung für diese Ressource. Das Azure Stack Edge Pro-Betriebsteam setzt sich mit Ihnen in Verbindung, um die Rückgabe des Geräts zu bestätigen. Anschließend können Sie die Ressource löschen, die im Azure-Portal mit dem Gerät verknüpft ist.
- Wenn das Gerät erheblich beschädigt eintrifft, fallen möglicherweise Geldbußen an. Weitere Informationen finden Sie unter [häufig gestellten Fragen zu verloren gegangenen oder beschädigten Geräten](https://azure.microsoft.com/pricing/details/databox/edge/) und [Produktbestimmungen](https://www.microsoft.com/licensing/product-licensing/products).  


Sie können das Gerät im Azure-Portal löschen:

- Nachdem Sie eine Bestellung aufgegeben haben und bevor das Gerät von Microsoft vorbereitet wird.
- Nachdem Sie ein Gerät an Microsoft zurückgegeben haben und das Azure Stack Edge Pro-Betriebsteam Ihnen die Rückgabe des Geräts telefonisch bestätigt hat. Das Betriebsteam meldet sich erst, wenn das zurückgegebene Gerät die physische Überprüfung im Azure-Rechenzentrum bestanden hat.

Wenn Sie das Gerät mit einem anderen Abonnement oder Standort aktiviert haben, verschiebt Microsoft Ihre Bestellung innerhalb eines Geschäftstags an das neue Abonnement oder den neuen Standort. Nach dem Verschieben der Bestellung können Sie diese Ressource löschen.


Löschen Sie mit folgenden Schritten das Gerät und die Ressource im Azure-Portal.

1. Navigieren Sie im Azure-Portal zu Ihrer Ressource und anschließend zu **Übersicht**. Wählen Sie in der Befehlsleiste **Löschen** aus.

    ![Auswählen von „Löschen“](media/azure-stack-edge-return-device/delete-resource-1.png)

2. Geben Sie auf dem Blatt **Gerät löschen** den Namen des zu löschenden Geräts ein, und wählen Sie **Löschen** aus.

    ![Bestätigen des Löschens](media/azure-stack-edge-return-device/delete-resource-2.png)

Sie werden nach erfolgreichem Löschen von Gerät und zugeordneter Ressource benachrichtigt.


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [ein Azure Stack Edge Pro-Ersatzgerät erhalten](azure-stack-edge-replace-device.md).