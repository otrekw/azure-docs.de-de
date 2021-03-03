---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 02/09/2021
ms.author: alkohli
ms.openlocfilehash: 71f18cf8448060385ea38be9b2719b1ed545c5d2
ms.sourcegitcommit: 5a999764e98bd71653ad12918c09def7ecd92cf6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2021
ms.locfileid: "100545298"
---
> [!NOTE] 
> Sie können die Verschlüsselungseinstellungen und die Anmeldeinformationen des Speicherkontos, die einem Volumecontainer zugeordnet sind, nach der Erstellung nicht mehr ändern.

#### <a name="to-modify-a-volume-container"></a>So ändern Sie einen Volumecontainer

1. Wechseln Sie zu Ihrem StorSimple-Geräte-Manager-Dienst und anschließend zu **Verwaltung > Volumecontainer**.

2. Wählen Sie in der tabellarischen Volumecontainerliste den Volumecontainer aus, den Sie ändern möchten. Wählen Sie auf der Seite **Geräte** das Gerät aus, doppelklicken Sie darauf, und klicken Sie dann auf die Registerkarte **Volumecontainer**.

3. Wählen Sie in der tabellarischen Volumecontainerliste den Volumecontainer aus, den Sie ändern möchten. Klicken Sie im Blatt, dass sich anschließend öffnet, in der Befehlsleiste auf **Ändern**.

    ![Ändern eines Volumecontainers](./media/storsimple-8000-modify-volume-container/modify-volume-container-01.png)

4. Führen Sie auf dem Blatt **Volumecontainer ändern** die folgenden Schritte aus:
   
   1. Der Name, der Verschlüsselungsschlüssel und das Speicherkonto, die dem Volumecontainer zugeordnet wurden, können nicht geändert werden, nachdem sie einmal angegeben wurden. Ändern Sie die Einstellung für die zugeordnete Bandbreite.<!--STEPS NEED WORK. Updated screen doesn't show alternative to Unlimited or subsequent steps if they customize bandwidth. Can we talk them through this (briefly)?-->
      
       ![Ändern der Einstellung für die Bandbreite](./media/storsimple-8000-modify-volume-container/modify-volume-container-02.png)<!--New graphic based on: modify-volume-container-bw-setting.png-->

   1.  Klicken Sie auf **OK**.<!--If they choose Custom, do they still click OK, or are there more steps?-->

5. Auf der nächsten Seite des Dialogfelds **Volumecontainer ändern** :<!--This step happens only if they choose Custom bandwidth? Are the steps similar to those in "Add volume container," step 3f, above?"-->
   
   1. Wählen Sie in der Dropdownliste eine vorhandene Bandbreitenvorlage aus.
   1. Überprüfen Sie die Zeitplaneinstellungen für die angegebene Bandbreitenvorlage.
   1. Klicken Sie auf **Speichern**, und bestätigen Sie die Änderungen.
      
       ![Bestätigen der Änderungen](./media/storsimple-8000-modify-volume-container/modify-volume-container-03.png)

      Das Blatt **Volumecontainer** wird den Änderungen entsprechend aktualisiert.
