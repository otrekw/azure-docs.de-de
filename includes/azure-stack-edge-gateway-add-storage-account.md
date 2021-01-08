---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 01/04/2021
ms.author: alkohli
ms.openlocfilehash: 5f39f727deaf3a53db5e2928e5af23779c298318
ms.sourcegitcommit: d7d5f0da1dda786bda0260cf43bd4716e5bda08b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2021
ms.locfileid: "97911630"
---
1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihre Azure Stack Edge-Ressource aus, und navigieren Sie anschließend zu **Übersicht**. Ihr Gerät sollte online sein. Navigieren Sie zu **Cloudspeichergateway > Speicherkonten**.

2. Wählen Sie auf der Gerätebefehlsleiste die Option **+ Speicherkonto hinzufügen** aus. 

   ![Hinzufügen von Speicherkonten](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-1.png)

3. Legen Sie im Bereich **Edge-Speicherkonto hinzufügen** die folgenden Einstellungen fest:

    a. Ein eindeutiger Name für das Edge-Speicherkonto auf Ihrem Gerät. Speicherkontonamen dürfen nur Kleinbuchstaben und Ziffern enthalten. Sonderzeichen sind nicht zulässig. Der Name des Speicherkontos muss auf dem Gerät (nicht jedoch geräteübergreifend) eindeutig sein.

    b. Eine optionale Beschreibung der Daten, die im Speicherkonto gespeichert werden.  
    
    c. Standardmäßig ist das Edge-Speicherkonto einem Azure Storage-Konto in der Cloud zugeordnet. Die Daten aus dem Speicherkonto werden automatisch per Push in die Cloud übertragen. Geben Sie das Azure-Speicherkonto an, dem Ihr Edge-Speicherkonto zugeordnet ist.  

    d. Erstellen Sie einen neuen Container, oder wählen Sie einen vorhandenen Container im Azure Storage-Konto aus. Alle Daten des Geräts, die in das Edge-Speicherkonto geschrieben werden, werden automatisch in den ausgewählten Speichercontainer im zugeordneten Azure Storage-Konto hochgeladen.

    <!--![Add a storage account](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-2.png)-->

    e. Nachdem alle Speicherkontooptionen angegeben wurden, klicken Sie auf **Hinzufügen**, um das Edge-Speicherkonto zu erstellen. Sie werden benachrichtigt, wenn das Edge-Speicherkonto erfolgreich erstellt wurde. Das neue Edge-Speicherkonto wird im Azure-Portal in der Liste der Speicherkonten angezeigt. 

    
4. Wenn Sie dieses neue Speicherkonto auswählen und zu **Zugriffsschlüssel** wechseln, finden Sie dort den Blobdienstendpunkt und den Namen des entsprechenden Speicherkontos. Kopieren Sie diese Informationen. Mit diesen Werten und den Zugriffsschlüsseln können Sie eine Verbindung mit dem Edge-Speicherkonto herstellen.

    ![Hinzufügen eines Speicherkontos 2](media/azure-stack-edge-gateway-add-storage-account/add-storage-account-4.png)

    Sie erhalten die Zugriffsschlüssel durch [Herstellen einer Verbindung mit den lokalen Geräte-APIs über Azure Resource Manager](../articles/databox-online/azure-stack-edge-j-series-connect-resource-manager.md). 
