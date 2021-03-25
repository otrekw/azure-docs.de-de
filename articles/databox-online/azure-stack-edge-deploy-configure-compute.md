---
title: Tutorial zum Filtern und Analysieren von Daten mit Compute in Azure Stack Edge Pro | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie die Computerolle für ein Azure Stack Edge Pro-Gerät konfigurieren und verwenden, um Daten vor dem Senden an Azure zu transformieren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 01/06/2021
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure compute on Azure Stack Edge Pro so I can use it to transform the data before sending it to Azure.
ms.openlocfilehash: 10741fbf2deb61d63f444ff9e2247bc59f41af38
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97968730"
---
# <a name="tutorial-transform-the-data-with-azure-stack-edge-pro"></a>Tutorial: Transformieren von Daten mit Azure Stack Edge Pro

In diesem Tutorial wird beschrieben, wie Sie eine Computerolle auf Ihrem Azure Stack Edge Pro-Gerät konfigurieren. Nach dem Konfigurieren der Computerolle kann Azure Stack Edge Pro Daten vor dem Senden an Azure transformieren.

Dieser Vorgang kann ca. 10 bis 15 Minuten dauern.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren der Computeumgebung
> * Hinzufügen von Freigaben
> * Hinzufügen eines Computemoduls
> * Überprüfen der Datentransformation und -übertragung

 
## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie eine Computerolle auf Ihrem Azure Stack Edge Pro-Gerät einrichten:

- Das Azure Stack Edge Pro-Gerät wurde aktiviert, wie unter [Verbinden, Einrichten und Aktivieren von Azure Stack Edge Pro](azure-stack-edge-deploy-connect-setup-activate.md) beschrieben.


## <a name="configure-compute"></a>Konfigurieren der Computeumgebung

Sie erstellen eine IoT Hub-Ressource, um die Computeumgebung in Ihrer Azure Stack Edge Pro-Instanz zu konfigurieren.

1. Navigieren Sie im Azure-Portal für Ihre Azure Stack Edge-Ressource zu **Übersicht**. Wählen Sie im rechten Bereich **IoT Edge** aus.

    ![Erste Schritte mit Compute](./media/azure-stack-edge-deploy-configure-compute/configure-compute-1.png)

1. Wählen Sie auf der Kachel **IoT Edge-Dienst aktivieren** die Option **Hinzufügen** aus. Dadurch wird der IoT Edge-Dienst aktiviert, der Ihnen das lokale Bereitstellen von IoT Edge-Modulen auf Ihrem Gerät ermöglicht.

    ![Erste Schritte mit Compute 2](./media/azure-stack-edge-deploy-configure-compute/configure-compute-2.png)

1. Geben Sie auf dem Blatt **IoT Edge-Dienst erstellen** Folgendes ein:

   
    |Feld  |Wert  |
    |---------|---------|
    |Subscription     |Wählen Sie ein Abonnement für Ihre IoT Hub-Ressource aus. Sie können dasselbe Abonnement verwenden, das auch von der Azure Stack Edge-Ressource verwendet wird.         |
    |Resource group     |Wählen Sie eine Ressourcengruppe für Ihre IoT Hub-Ressource aus. Sie können dieselbe Ressourcengruppe verwenden, die auch von der Azure Stack Edge-Ressource verwendet wird.         |
    |IoT Hub     | Wählen Sie zwischen **Neu** und **Vorhanden**. <br> Standardmäßig wird ein Standard-Tarif (S1) verwendet, um eine IoT-Ressource zu erstellen. Wenn Sie eine IoT-Ressource im Free-Tarif verwenden möchten, können Sie diese erstellen und dann als vorhandene Ressource auswählen. <br> Die IoT Hub-Ressource nutzt in jedem Fall dasselbe Abonnement und dieselbe Ressourcengruppe wie die Azure Stack Edge-Ressource.     |
    |Name     |Geben Sie einen Namen für Ihre IoT Hub-Ressource ein.         |

    ![Erste Schritte mit Compute 3](./media/azure-stack-edge-deploy-configure-compute/configure-compute-3.png)

4. Klicken Sie auf **Überprüfen + erstellen**. Die Erstellung der IoT Hub-Ressource dauert einige Minuten. Nach der Erstellung der IoT Hub-Ressource wird die **Übersicht** aktualisiert, um anzugeben, dass der IoT Edge-Dienst ausgeführt wird. 

    ![Erste Schritte mit Compute 4](./media/azure-stack-edge-deploy-configure-compute/configure-compute-4.png)    
    
    Wenn der IoT Edge-Dienst auf dem Edge-Gerät konfiguriert ist, werden zwei Geräte erstellt: ein IoT-Gerät und ein IoT Edge-Gerät. Beide Geräte können in der IoT Hub-Ressource angezeigt werden. Auf diesem IoT Edge-Gerät wird auch eine IoT Edge-Runtime ausgeführt. Derzeit ist für Ihr IoT Edge-Gerät nur die Linux-Plattform verfügbar.

    Um zu bestätigen, dass die Edgecomputingrolle konfiguriert wurde, wählen Sie **IoT Edge-Dienst > Eigenschaften aus**, und zeigen Sie das IoT-Gerät und das IoT Edge-Gerät an.

    ![Erste Schritte mit Compute 5](./media/azure-stack-edge-deploy-configure-compute/configure-compute-5.png) 

## <a name="add-shares"></a>Hinzufügen von Freigaben

Für die einfache Bereitstellung in diesem Tutorial benötigen Sie zwei Freigaben: eine Edgefreigabe und eine weitere lokale Edgefreigabe.

1. Fügen Sie auf dem Gerät eine Edgefreigabe hinzu, indem Sie die folgenden Schritte ausführen:

    1. Navigieren Sie in Ihrer Azure Stack Edge-Ressource zu **IoT Edge > Freigaben**.
    2. Wählen Sie auf der Befehlsleiste **+ Freigabe hinzufügen** aus.
    3. Geben Sie auf dem Blatt **Freigabe hinzufügen** den Freigabenamen an, und wählen Sie den Freigabetyp aus.
    4. Aktivieren Sie zum Bereitstellen der Edgefreigabe das Kontrollkästchen für **Freigabe mit Edgecomputing verwenden**.
    5. Wählen Sie das **Speicherkonto**, den **Speicherdienst**, einen vorhandenen Benutzer und dann die Option **Erstellen** aus.

        ![Hinzufügen einer Edgefreigabe](./media/azure-stack-edge-deploy-configure-compute/add-edge-share-1.png) 

    Wenn Sie eine lokale NFS-Freigabe erstellt haben, können Sie die folgende rsync-Befehlsoption verwenden, um Dateien auf die Freigabe zu kopieren:

    `rsync <source file path> < destination file path>`

    Weitere Informationen zum rsync-Befehl finden Sie in der [Dokumentation zu rsync](https://www.computerhope.com/unix/rsync.htm).

    Die Edgefreigabe wird erstellt, und Sie erhalten eine Benachrichtigung zur erfolgreichen Erstellung. Die Freigabeliste wird unter Umständen aktualisiert. Sie müssen jedoch warten, bis die Erstellung der Freigabe abgeschlossen ist.

2. Fügen Sie auf dem Edgegerät eine lokale Edgefreigabe hinzu, indem Sie alle Schritte des vorherigen Schritts wiederholen und das Kontrollkästchen **Als lokale Edge-Freigabe konfigurieren** aktivieren. Die Daten auf der lokalen Dateifreigabe verbleiben auf dem Gerät.

    ![Hinzufügen einer lokalen Edgefreigabe](./media/azure-stack-edge-deploy-configure-compute/add-edge-share-2.png)

  
3. Navigieren Sie zu **IoT Edge > Freigaben**, um die aktualisierte Liste der Freigaben anzuzeigen.

    ![Aktualisierte Liste der Freigaben](./media/azure-stack-edge-deploy-configure-compute/add-edge-share-3.png) 
 

## <a name="add-a-module"></a>Hinzufügen eines Moduls

Sie können ein benutzerdefiniertes oder ein vorgefertigtes Modul hinzufügen. Auf diesem Edgegerät sind keine benutzerdefinierten Module vorhanden. Informationen zum Erstellen eines benutzerdefinierten Moduls finden Sie unter [Entwickeln eines C#-Moduls für Ihr Azure Stack Edge Pro-Gerät](azure-stack-edge-create-iot-edge-module.md).

In diesem Abschnitt fügen Sie dem IoT Edge-Gerät, das Sie unter [Entwickeln eines C#-Moduls für Ihr Azure Stack Edge Pro-Gerät](azure-stack-edge-create-iot-edge-module.md) erstellt haben, ein benutzerdefiniertes Modul hinzu. Dieses benutzerdefinierte Modul verwendet Dateien von einer lokalen Edgefreigabe auf dem Edgegerät und verschiebt sie auf eine Edgefreigabe (Cloudfreigabe) auf dem Gerät. Die Cloudfreigabe überträgt die Dateien dann per Pushvorgang an das Azure-Speicherkonto, das der Cloudfreigabe zugeordnet ist.

1. Navigieren Sie zu **IoT Edge > Module**. Wählen Sie auf der Gerätebefehlsleiste die Option **+ Modul hinzufügen** aus.
2. Geben Sie auf dem Blatt **Modul konfigurieren und hinzufügen** die folgenden Werte ein:

    
    |Feld  |Wert  |
    |---------|---------|
    |Name     | Ein eindeutiger Name für das Modul. Dieses Modul ist ein Docker-Container, den Sie auf dem IoT Edge-Gerät bereitstellen können, das Ihrer Azure Stack Edge Pro-Instanz zugeordnet ist.        |
    |Image-URI     | Der Image-URI für das entsprechende Containerimage für das Modul.        |
    |Anmeldeinformationen erforderlich     | Wenn dieses Kontrollkästchen aktiviert ist, werden ein Benutzername und ein Kennwort verwendet, um Module mit einer übereinstimmenden URL abzurufen.        |
    |Eingabefreigabe     | Wählen Sie eine Eingabefreigabe aus. Die lokale Edgefreigabe ist in diesem Fall die Eingabefreigabe. Mit dem hier verwendeten Modul werden Dateien von der lokalen Edgefreigabe auf eine Edgefreigabe verschoben, von der sie in die Cloud hochgeladen werden.        |
    |Ausgabefreigabe     | Wählen Sie eine Ausgabefreigabe aus. Die Edgefreigabe ist in diesem Fall die Ausgabefreigabe.        |
    |Triggertyp     | Wählen Sie zwischen den Optionen **Datei** und **Zeitplan**. Es wird jeweils ein Dateitrigger ausgelöst, wenn ein Dateiereignis eintritt, z. B. das Schreiben einer Datei auf die Eingabefreigabe. Ein geplanter Trigger wird nach einem von Ihnen festgelegten Zeitplan ausgelöst.         |
    |Triggername     | Ein eindeutiger Name für Ihren Trigger.         |
    |Umgebungsvariablen| Optionale Informationen zum Definieren der Umgebung, in der Ihr Modul ausgeführt wird.   |

    ![Hinzufügen und Konfigurieren des Moduls](./media/azure-stack-edge-deploy-configure-compute/add-module-1.png)

3. Wählen Sie **Hinzufügen**. Das Modul wird hinzugefügt. Die Seite **IoT Edge > Übersicht** wird aktualisiert, um anzugeben, dass das Modul bereitgestellt wurde. 

    ![Modul bereitgestellt](./media/azure-stack-edge-deploy-configure-compute/add-module-2.png)

### <a name="verify-data-transform-and-transfer"></a>Überprüfen der Datentransformation und -übertragung

Im letzten Schritt wird sichergestellt, dass das Modul verbunden ist und wie erwartet ausgeführt wird. Der Laufzeitstatus des Moduls sollte für Ihr IoT Edge-Gerät auf der IoT Hub-Ressource „Wird ausgeführt“ lauten.

Führen Sie die folgenden Schritte aus, um zu überprüfen, ob das Modul ausgeführt wird:

1. Wählen Sie die Kachel **Modul hinzufügen** aus. Das Blatt **Module** wird geöffnet. Identifizieren Sie in der Liste mit den Modulen das von Ihnen bereitgestellte Modul. Der Laufzeitstatus des hinzugefügten Moduls sollte *Wird ausgeführt* lauten.

    ![Überprüfen der Datentransformation](./media/azure-stack-edge-deploy-configure-compute/verify-data-1.png)
 
1. Stellen Sie im Datei-Explorer eine Verbindung mit den lokalen und anderen Edgefreigaben her, die Sie zuvor erstellt haben.

    ![Überprüfen der Datentransformation 2](./media/azure-stack-edge-deploy-configure-compute/verify-data-2.png) 
 
1. Fügen Sie der lokalen Freigabe Daten hinzu.

    ![Überprüfen der Datentransformation 3](./media/azure-stack-edge-deploy-configure-compute/verify-data-3.png) 
 
    Die Daten werden auf die Cloudfreigabe verschoben.

    ![Überprüfen der Datentransformation 4](./media/azure-stack-edge-deploy-configure-compute/verify-data-4.png)  

    Anschließend werden die Daten per Pushvorgang von der Cloudfreigabe in das Speicherkonto übertragen. Wechseln Sie zum Speicher-Explorer, um die Daten anzuzeigen.

    ![Überprüfen der Datentransformation 5](./media/azure-stack-edge-deploy-configure-compute/verify-data-5.png) 
 
Sie haben den Überprüfungsvorgang abgeschlossen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Konfigurieren der Computeumgebung
> * Hinzufügen von Freigaben
> * Hinzufügen eines Computemoduls
> * Überprüfen der Datentransformation und -übertragung

Weitere Informationen zum Verwalten Ihres Azure Stack Edge Pro-Geräts finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Verwenden der lokalen Webbenutzeroberfläche zur Verwaltung eines Azure Stack Edge Pro-Geräts](azure-stack-edge-manage-access-power-connectivity-mode.md)
