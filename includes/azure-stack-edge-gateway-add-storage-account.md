---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/12/2021
ms.author: alkohli
ms.openlocfilehash: cf67ea58c4c13756fdc6e437883f12124ca47eb9
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105105946"
---
1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihre Azure Stack Edge-Ressource aus, und navigieren Sie anschließend zu **Übersicht**. Ihr Gerät sollte online sein. Navigieren Sie zu **Cloudspeichergateway > Speicherkonten**.

2. Wählen Sie auf der Gerätebefehlsleiste die Option **+ Speicherkonto hinzufügen** aus. 

   ![Hinzufügen von Speicherkonten](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-1.png)

3. Legen Sie im Bereich **Edge-Speicherkonto hinzufügen** die folgenden Einstellungen fest:

    1. Geben Sie einen eindeutigen Namen für das Edge-Speicherkonto auf Ihrem Gerät an. Speicherkontonamen dürfen nur Kleinbuchstaben und Ziffern enthalten. Sonderzeichen sind nicht zulässig. Der Name des Speicherkontos muss auf dem Gerät (nicht jedoch geräteübergreifend) eindeutig sein.

    2. Geben Sie eine optionale Beschreibung der Daten an, die im Speicherkonto gespeichert werden.  
    
    3. Standardmäßig ist das Edge-Speicherkonto einem Azure Storage-Konto in der Cloud zugeordnet. Die Daten aus dem Speicherkonto werden automatisch per Push in die Cloud übertragen. Geben Sie das Azure-Speicherkonto an, dem Ihr Edge-Speicherkonto zugeordnet ist.

    4. Erstellen Sie als Nächstes einen neuen Container, oder wählen Sie einen vorhandenen Container im Azure Storage-Konto aus. Alle Daten des Geräts, die in das Edge-Speicherkonto geschrieben werden, werden automatisch in den ausgewählten Speichercontainer im zugeordneten Azure Storage-Konto hochgeladen.

    5. Nachdem alle Speicherkontooptionen angegeben wurden, klicken Sie auf **Hinzufügen**, um das Edge-Speicherkonto zu erstellen. Sie werden benachrichtigt, wenn das Edge-Speicherkonto erfolgreich erstellt wurde. Das neue Edge-Speicherkonto wird im Azure-Portal in der Liste der Speicherkonten angezeigt.

    <!--[Add a storage account](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-2.png)-->
    
4. Wenn Sie dieses neue Speicherkonto auswählen und zu **Zugriffsschlüssel** wechseln, finden Sie dort den Blobdienstendpunkt und den Namen des entsprechenden Speicherkontos. Kopieren Sie diese Informationen. Mit diesen Werten und den Zugriffsschlüsseln können Sie eine Verbindung mit dem Edge-Speicherkonto herstellen.

    ![Hinzufügen eines Speicherkontos 2](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-4.png)

    Sie erhalten die Zugriffsschlüssel durch [Herstellen einer Verbindung mit den lokalen Geräte-APIs über Azure Resource Manager](../articles/databox-online/azure-stack-edge-gpu-connect-resource-manager.md).