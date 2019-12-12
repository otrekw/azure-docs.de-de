---
title: Hinzufügen eines Azure IoT Edge-Geräts zu Azure IoT Central | Microsoft-Dokumentation
description: Fügen Sie Ihrer Azure IoT Central-Anwendung als Bediener ein Azure IoT Edge-Gerät hinzu.
author: rangv
ms.author: rangv
ms.date: 12/09/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: e5d60c77e9bdc0733c12bca891eb6c3e33a1fceb
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74979069"
---
# <a name="tutorial-add-an-azure-iot-edge-device-to-your-azure-iot-central-application"></a>Tutorial: Hinzufügen eines Azure IoT Edge-Geräts zu Ihrer Azure IoT Central-Anwendung

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

In diesem Tutorial erfahren Sie, wie Sie Ihrer Azure IoT Central-Anwendung ein Azure IoT Edge-Gerät hinzufügen und dieses konfigurieren. In diesem Tutorial wird eine IoT Edge-fähige Linux-VM aus dem Azure Marketplace verwendet.

Das Tutorial besteht aus zwei Teilen:

* Im ersten Teil erfahren Sie, wie Sie als Bediener eine IoT Edge-Gerätebereitstellung mit Cloudschwerpunkt durchführen.
* Anschließend wird erläutert, wie Sie eine IoT Edge-Gerätebereitstellung mit Geräteschwerpunkt durchführen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen eines neuen IoT Edge-Geräts
> * Konfigurieren des IoT Edge-Geräts für die Bereitstellung mithilfe eines SAS-Schlüssels (Shared Access Signature)
> * Anzeigen von Dashboards und der Modulintegrität in IoT Central
> * Senden von Befehlen an ein Modul, das auf dem IoT Edge-Gerät ausgeführt wird
> * Festlegen von Eigenschaften für ein Modul, das auf dem IoT Edge-Gerät ausgeführt wird

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine Azure IoT Central-Anwendung. Befolgen Sie [diesen Schnellstart zum Erstellen einer Azure IoT Central-Anwendung](./quick-deploy-iot-central.md).

## <a name="enable-azure-iot-edge-enrollment-group"></a>Aktivieren einer Azure IoT Edge-Registrierungsgruppe
Aktivieren Sie auf der Seite **Verwaltung** SAS-Schlüssel für die Azure IoT Edge-Registrierungsgruppe.

![Screenshot der Verwaltungsseite mit hervorgehobener Geräteverbindung](./media/tutorial-add-edge-as-leaf-device/groupenrollment.png)

## <a name="provision-a-cloud-first-azure-iot-edge-device"></a>Bereitstellen eines Azure IoT Edge-Geräts mit Cloudschwerpunkt  
In diesem Abschnitt erstellen Sie mithilfe der Umgebungssensorvorlage ein neues IoT Edge-Gerät, und Sie stellen ein Gerät bereit. Wählen Sie **Geräte** > **Environment Sensor Template** (Umgebungssensorvorlage) aus. 

![Screenshot der Seite „Geräte“ mit hervorgehobener Umgebungssensorvorlage](./media/tutorial-add-edge-as-leaf-device/deviceexplorer.png)

Wählen Sie **+ Neu** aus, und geben Sie eine Geräte-ID und einen Namen Ihrer Wahl ein. Klicken Sie auf **Erstellen**.

![Screenshot des Dialogfelds „Neues Gerät erstellen“ mit hervorgehobener Geräte-ID und Option „Erstellen“](./media/tutorial-add-edge-as-leaf-device/cfdevicecredentials.png)

Das Gerät wird in den Modus **Registriert** versetzt.

![Screenshot der Seite „Environment Sensor Template“ (Umgebungssensorvorlage) mit hervorgehobener Option „Gerätestatus“](./media/tutorial-add-edge-as-leaf-device/cfregistered.png)

## <a name="deploy-an-iot-edge-enabled-linux-vm"></a>Bereitstellen einer IoT Edge-fähigen Linux-VM

> [!NOTE]
> Sie können einen beliebigen Computer bzw. ein beliebiges Gerät verwenden. Als Betriebssystem sind Windows oder Linux möglich.

In diesem Tutorial wird eine Azure IoT-fähige Linux-VM verwendet, die in Azure erstellt wird. Wählen Sie im [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview) die Option **JETZT ABRUFEN** aus. 

![Screenshot des Azure Marketplace mit hervorgehobener Option „JETZT ABRUFEN“](./media/tutorial-add-edge-as-leaf-device/cfmarketplace.png)

Wählen Sie **Weiter**.

![Screenshot des Dialogfelds „Diese App in Azure erstellen“ mit hervorgehobener Option „Weiter“](./media/tutorial-add-edge-as-leaf-device/cfmarketplacecontinue.png)


Sie gelangen zum Azure-Portal. Klicken Sie auf **Erstellen**.

![Screenshot des Azure-Portals mit hervorgehobener Option „Erstellen“](./media/tutorial-add-edge-as-leaf-device/cfubuntu.png)

Wählen Sie **Abonnement** aus, erstellen Sie eine neue Ressourcengruppe, und wählen Sie für die VM-Verfügbarkeit die Option **(USA) USA, Westen 2** aus. Geben Sie dann Benutzer- und Kennwortinformationen ein. Diese Angaben werden in späteren Schritten benötigt, daher dürfen Sie sie nicht vergessen. Klicken Sie auf **Überprüfen + erstellen**.

![Screenshot der Seite mit den Details zu „Virtuellen Computer erstellen“ mit verschiedenen markierten Optionen](./media/tutorial-add-edge-as-leaf-device/cfvm.png)

Wählen Sie nach der Überprüfung die Option **Erstellen** aus.

![Screenshot der Seite „Virtuellen Computer erstellen“ mit erfolgreicher Validierung und hervorgehobener Option „Erstellen“](./media/tutorial-add-edge-as-leaf-device/cfvmvalidated.png)

Die Erstellung der Ressourcen dauert einige Minuten. Wählen Sie **Zu Ressource wechseln** aus.

![Screenshot der Seite der abgeschlossenen Bereitstellung mit hervorgehobener Option „Zu Ressource wechseln“](./media/tutorial-add-edge-as-leaf-device/cfvmdeploymentcomplete.png)

### <a name="provision-vm-as-an-iot-edge-device"></a>Bereitstellen eines virtuellen Computers als IoT Edge-Gerät 

Wählen Sie im Abschnitt **Support + Problembehandlung** die Option **Serielle Konsole** aus.

![Screenshot der Optionen für Support + Problembehandlung mit hervorgehobener Option „Serielle Konsole“](./media/tutorial-add-edge-as-leaf-device/cfserialconsole.png)

Es wird ein Bildschirm ähnlich dem folgenden angezeigt:

![Screenshot der Konsole](./media/tutorial-add-edge-as-leaf-device/cfconsole.png)

Drücken Sie die EINGABETASTE, geben Sie den Benutzernamen und das Kennwort ein, wenn Sie dazu aufgefordert werden, und drücken Sie erneut die EINGABETASTE. 

![Screenshot der Konsole](./media/tutorial-add-edge-as-leaf-device/cfconsolelogin.png)

Geben Sie den folgenden Befehl ein, um einen Befehl als Administrator (Root-Benutzer) auszuführen: **sudo su –** .

![Screenshot der Konsole](./media/tutorial-add-edge-as-leaf-device/cfsudo.png)

Überprüfen Sie die Version der IoT Edge-Runtime. Zum Zeitpunkt der Erstellung dieses Artikels ist 1.0.8 die aktuelle allgemein verfügbare Version.

![Screenshot der Konsole](./media/tutorial-add-edge-as-leaf-device/cfconsoleversion.png)

Installieren Sie den Editor Vim, oder verwenden Sie nano, wenn Sie dies bevorzugen. 

![Screenshot der Konsole](./media/tutorial-add-edge-as-leaf-device/cfconsolevim.png)

![Screenshot der Konsole](./media/tutorial-add-edge-as-leaf-device/cfvim.png)

Bearbeiten Sie die IoT Edge-Datei „config.yaml“.

![Screenshot der Konsole](./media/tutorial-add-edge-as-leaf-device/cfconsoleconfig.png)

Scrollen Sie in der YAML-Datei nach unten, und kommentieren Sie den Teil mit der Verbindungszeichenfolge aus. 

**Vorher**

![Screenshot der Konsole](./media/tutorial-add-edge-as-leaf-device/cfmanualprovisioning.png)

**Nachher** (Drücken Sie ESC, und geben Sie ein kleines „a“ ein, um mit der Bearbeitung zu beginnen.)

![Screenshot der Konsole](./media/tutorial-add-edge-as-leaf-device/cfmanualprovisioningcomments.png)

Kommentieren Sie in der YAML-Datei den Teil mit dem symmetrischen Schlüssel aus. 

**Vorher**

![Screenshot der Konsole](./media/tutorial-add-edge-as-leaf-device/cfconsolesymmcomments.png)

**Nachher**

![Screenshot der Konsole](./media/tutorial-add-edge-as-leaf-device/cfconsolesymmuncomments.png)

Wechseln Sie zu IoT Central. Rufen Sie die Bereichs-ID, die Geräte-ID und den symmetrischen Schlüssel des IoT Edge Geräts ab.
![Screenshot von IoT Central mit verschiedenen hervorgehobenen Optionen für Geräteverbindungen](./media/tutorial-add-edge-as-leaf-device/cfdeviceconnect.png)

Navigieren Sie zum Linux-Computer, und ersetzen Sie die Bereichs-ID und die Registrierungs-ID durch die Geräte-ID und den symmetrischen Schlüssel.

Drücken Sie ESC, und geben Sie **:wq!** ein. Drücken Sie die EINGABETASTE, um Ihre Änderungen zu speichern.

Starten Sie IoT Edge neu, um Ihre Änderungen zu verarbeiten, und drücken Sie die EINGABETASTE.

![Screenshot der Konsole](./media/tutorial-add-edge-as-leaf-device/cfrestart.png)

Geben Sie **iotedge list** ein. Nach einigen Minuten sehen Sie, dass drei Module bereitgestellt wurden.

![Screenshot der Konsole](./media/tutorial-add-edge-as-leaf-device/cfconsolemodulelist.png)


## <a name="iot-central-device-explorer"></a>Geräte-Explorer in IoT Central 

In IoT Central wird Ihr Gerät in den Zustand „Bereitgestellt“ versetzt.

![Screenshot von „Geräteoptionen“ in IoT Central mit hervorgehobener Option „Gerätestatus“](./media/tutorial-add-edge-as-leaf-device/cfprovisioned.png)

Auf der Registerkarte **Module** wird der Status des Geräts und des Moduls in IoT Central angezeigt. 

![Screenshot der Registerkarte „Module“ in IoT Central](./media/tutorial-add-edge-as-leaf-device/cfiotcmodulestatus.png)


Sie sehen in einem Formular angezeigte Cloudeigenschaften anhand der Gerätevorlage, die Sie in den vorherigen Schritten erstellt haben. Geben Sie Werte ein, und wählen Sie **Speichern** aus. 

![Screenshot des Formulars „My Linux Edge Device“](./media/tutorial-add-edge-as-leaf-device/deviceinfo.png)

Hier sehen Sie eine Ansicht, die in Form einer Dashboardkachel angezeigt wird.

![Screenshot der Dashboardkacheln für „My Linux Edge Device“](./media/tutorial-add-edge-as-leaf-device/dashboard.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun wissen, wie Sie mit IoT Edge-Geräten in IoT Central arbeiten, wird als Nächstes der folgende Schritt empfohlen:

<!-- Next how-tos in the sequence -->

> [!div class="nextstepaction"]
> [Konfigurieren eines transparenten Gateways](../../iot-edge/how-to-create-transparent-gateway.md)
