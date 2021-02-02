---
title: 'Schnellstart: Erstellen eines Azure IoT Edge-Geräts unter Windows | Microsoft-Dokumentation'
description: In diesem Schnellstart erfahren Sie, wie ein IoT Edge-Geräte erstellen und anschließend vordefinierten Code remote über das Azure-Portal bereitstellen.
author: rsameser
manager: kgremban
ms.author: riameser
ms.date: 01/20/2021
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc, devx-track-azurecli
monikerRange: =iotedge-2018-06
ms.openlocfilehash: 71e38059aceb7da63f3545610b9acfe48c5d3150
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2021
ms.locfileid: "98663212"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-windows-device-preview"></a>Schnellstart: Bereitstellen Ihres ersten IoT Edge-Moduls auf einem Windows-Gerät (Vorschauversion)

In dieser Schnellstartanleitung können Sie Azure IoT Edge ausprobieren, indem Sie Code in Containern auf einem IoT Edge-Gerät für Linux unter Windows bereitstellen. IoT Edge ermöglicht Ihnen die Remoteverwaltung von Code auf Ihren Geräten, damit Sie noch mehr Workloads an den Edgebereich senden können. In dieser Schnellstartanleitung empfiehlt es sich, ein eigenes Gerät zu verwenden, um zu sehen, wie einfach es ist, Azure IoT Edge für Linux unter Windows zu verwenden.

In dieser Schnellstartanleitung wird Folgendes vermittelt:

* Erstellen Sie einen IoT Hub.
* Registrieren eines IoT Edge-Geräts für Ihren IoT Hub
* Installieren und Starten der Runtime von IoT Edge für Linux unter Windows auf Ihrem Gerät
* Durchführen der Remotebereitstellung eines Moduls für ein IoT Edge-Gerät und Senden von Telemetriedaten

![Diagramm: Schnellstart-Architektur für Geräte und Cloud](./media/quickstart/install-edge-full.png)

In dieser Schnellstartanleitung erfahren Sie Schritt für Schritt, wie Sie Ihr Gerät mit Azure IoT Edge für Linux unter Windows einrichten. Anschließend stellen Sie ein Modul über das Azure-Portal auf Ihrem Gerät bereit. Das in dieser Schnellstartanleitung verwendete Modul ist ein simulierter Sensor, mit dem Daten zu Temperatur, Luftfeuchtigkeit und Luftdruck generiert werden. Die anderen Tutorials zu Azure IoT Edge bauen hierauf auf und erläutern die Bereitstellung von zusätzlichen Modulen, mit denen die simulierten Daten analysiert werden, um geschäftliche Erkenntnisse zu gewinnen.

Wenn Sie über kein aktives Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen, bevor Sie beginnen.

>[!NOTE]
>IoT Edge für Linux unter Windows befindet sich in der [Public Preview-Phase](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Bereiten Sie die Umgebung für die Azure CLI vor.

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

Cloudressourcen:

* Eine Ressourcengruppe zum Verwalten aller Ressourcen, die Sie in dieser Schnellstartanleitung verwenden.

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

IoT Edge-Gerät:

* Windows-PC oder -Server mit Version 1809 oder höher
* Mindestens 4 GB Arbeitsspeicher (empfohlen: 8 GB)
* 10 GB freier Festplattenspeicher

## <a name="create-an-iot-hub"></a>Erstellen eines IoT-Hubs

Beginnen Sie den Schnellstart, indem Sie an der Azure-Befehlszeilenschnittstelle einen IoT-Hub erstellen.

![Diagramm: Erstellen einer IoT Hub-Instanz in der Cloud](./media/quickstart/create-iot-hub.png)

Der kostenlose IoT Hub kann für diesen Schnellstart verwendet werden. Wenn Sie IoT Hub schon einmal genutzt und bereits einen Hub erstellt haben, können Sie diesen IoT-Hub verwenden.

Mit dem folgenden Code wird ein kostenloser **F1**-Hub in der Ressourcengruppe `IoTEdgeResources` erstellt. Ersetzen Sie `{hub_name}` durch einen eindeutigen Namen für Ihren IoT-Hub. Die Erstellung einer IoT Hub-Instanz kann einige Minuten dauern.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 --partition-count 2
   ```

   Wenn Sie eine Fehlermeldung erhalten, da bereits ein kostenloser Hub in Ihrem Abonnement vorhanden ist, ändern Sie die SKU auf **S1**. Sollten Sie eine Fehlermeldung mit dem Hinweis erhalten, dass der IoT Hub-Name nicht verfügbar ist, ist bereits ein Hub mit diesem Namen vorhanden. Probieren Sie einen neuen Namen aus.

## <a name="register-an-iot-edge-device"></a>Registrieren eines IoT Edge-Geräts

Registrieren Sie ein IoT Edge-Gerät bei Ihrem neu erstellten IoT Hub.

![Diagramm – Registrieren eines Geräts mit einer IoT Hub-Identität](./media/quickstart/register-device.png)

Erstellen Sie eine Geräteidentität für das simulierte Gerät, sodass es mit dem IoT Hub kommunizieren kann. Die Geräteidentität befindet sich in der Cloud, und Sie verwenden eine eindeutige Geräte-Verbindungszeichenfolge, um einem physischen Gerät eine Geräteidentität zuzuordnen.

Da IoT Edge-Geräte sich von typischen IoT-Geräten unterscheiden und auf unterschiedliche Weise verwaltet werden können, deklarieren Sie diese Identität mit dem Flag `--edge-enabled` für ein IoT Edge-Gerät.

1. Geben Sie in Azure Cloud Shell den folgenden Befehl ein, um in Ihrem Hub ein Gerät mit dem Namen **myEdgeDevice** zu erstellen.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --edge-enabled --hub-name {hub_name}
   ```

   Falls Sie eine Fehlermeldung zu iothubowner-Richtlinienschlüsseln erhalten, überprüfen Sie, ob in Ihrer Cloud Shell-Instanz die aktuelle Version der Erweiterung „azure-iot“ ausgeführt wird.

2. Zeigen Sie die Verbindungszeichenfolge für Ihr Gerät an, über die Ihr physisches Gerät mit seiner Identität in IoT Hub verknüpft wird. Sie enthält den Namen Ihres IoT-Hubs, den Namen Ihres Geräts und einen gemeinsam verwendeten Schlüssel, mit dem die Verbindungen zwischen diesen Komponenten authentifiziert werden.

   ```azurecli-interactive
   az iot hub device-identity connection-string show --device-id myEdgeDevice --hub-name {hub_name}
   ```

3. Kopieren Sie den Wert des Schlüssels `connectionString` aus der JSON-Ausgabe, und speichern Sie ihn. Dieser Wert ist die Verbindungszeichenfolge des Geräts. Sie verwenden ihn im nächsten Abschnitt zum Konfigurieren der IoT Edge-Runtime.

   ![Abrufen der Verbindungszeichenfolge aus der CLI-Ausgabe](./media/quickstart/retrieve-connection-string.png)

## <a name="install-and-start-the-iot-edge-runtime"></a>Installieren und Starten der IoT Edge-Runtime

Installieren Sie IoT Edge für Linux unter Windows auf Ihrem Gerät, und konfigurieren Sie es mit einer Geräte-Verbindungszeichenfolge.

![Diagramm: Starten der IoT Edge-Runtime auf einem Gerät](./media/quickstart/start-runtime.png)

1. [Laden Sie Windows Admin Center herunter.](https://aka.ms/WACDownloadEFLOW)

1. Führen Sie die Schritte des Installations-Assistenten aus, um Windows Admin Center auf Ihrem Gerät einzurichten.

1. Wählen Sie in Windows Admin Center rechts oben auf dem Bildschirm die Option **Einstellungen** (Zahnradsymbol) aus.

1. Wählen Sie im Einstellungsmenü unter „Gateway“ die Option **Erweiterungen** aus.

1. Wählen Sie die Registerkarte **Feeds** und dann **Hinzufügen** aus.

1. Geben Sie https://aka.ms/wac-insiders-feed in das Textfeld ein, und wählen Sie **Hinzufügen** aus.

1. Nachdem der Feed hinzugefügt wurde, navigieren Sie zur Registerkarte **Verfügbare Erweiterungen**. Es kann einen Moment dauern, bis die Liste der Erweiterungen aktualisiert wird.

1. Wählen Sie in der Liste **Verfügbare Erweiterungen** die Option **Azure IoT Edge** aus.

1. **Installieren** Sie die Erweiterung.

1. Navigieren Sie nach Abschluss der Erweiterungsinstallation zur Dashboard-Hauptseite. Wählen Sie hierzu links oben auf dem Bildschirm die Option **Windows Admin Center** aus.

1. Daraufhin wird die Verbindung mit dem lokalen Host angezeigt, die den PC darstellt, auf dem Windows Admin Center ausgeführt wird.

   :::image type="content" source="media/quickstart/windows-admin-center-start-page.png" alt-text="Screenshot: Startseite für Windows Admin Center":::

1. Wählen Sie **Hinzufügen**.

   :::image type="content" source="media/quickstart/windows-admin-center-start-page-add.png" alt-text="Screenshot: Schaltfläche „Hinzufügen“ auf der Startseite für Windows Admin Center":::

1. Navigieren Sie zur Kachel „Azure IoT Edge“, und wählen Sie **Neu erstellen** aus. Dadurch wird der Installations-Assistent gestartet.

    :::image type="content" source="media/quickstart/select-tile-screen.png" alt-text="Screenshot: Kachel für Azure IoT Edge für Linux unter Windows":::

1. Durchlaufen Sie den Installations-Assistenten, um die Lizenzbedingungen zu akzeptieren, und wählen Sie **Weiter** aus.

    :::image type="content" source="media/quickstart/wizard-welcome-screen.png" alt-text="Screenshot: Willkommensseite des Assistenten":::

1. Wählen Sie **Optionale Diagnosedaten** aus, um erweiterte Diagnosedaten bereitzustellen, die Microsoft bei der Überwachung und Gewährleistung der Dienstqualität unterstützen, und klicken Sie anschließend auf **Weiter: Bereitstellen**.

    :::image type="content" source="media/quickstart/diagnostic-data-screen.png" alt-text="Screenshot: Diagnosedaten":::

1. Wählen Sie auf dem Bildschirm **Zielgerät auswählen** das gewünschte Zielgerät aus, um sich zu vergewissern, dass es die Mindestanforderungen erfüllt. Im Rahmen dieser Schnellstartanleitung wird IoT Edge auf dem lokalen Gerät installiert. Wählen Sie daher die Localhost-Verbindung aus. Wählen Sie nach der Bestätigung **Weiter** aus, um fortzufahren.

    :::image type="content" source="media/quickstart/wizard-select-target-device-screen.png" alt-text="Screenshot: Auswählen des Zielgeräts":::

1. Wählen Sie **Weiter** aus, um die Standardeinstellungen zu akzeptieren.

1. Auf dem Bereitstellungsbildschirm werden das Herunterladen des Pakets, das Installieren des Pakets, das Konfigurieren des Hosts und das abschließende Einrichten des virtuellen Linux-Computers angezeigt.  Eine erfolgreiche Bereitstellung sieht wie folgt aus:

    :::image type="content" source="media/quickstart/wizard-deploy-success-screen.png" alt-text="Screenshot: Assistent mit erfolgreicher Bereitstellung":::

1. Klicken Sie auf **Weiter: Verbinden**, um zum letzten Schritt zu gelangen und Ihr Azure IoT Edge-Gerät mit seiner Geräte-ID aus Ihrer IoT Hub-Instanz bereitzustellen.

1. Kopieren Sie die Verbindungszeichenfolge von Ihrem Gerät in Azure IoT Hub, und fügen Sie sie in das Feld für die Geräte-Verbindungszeichenfolge ein. Wählen Sie dann **Provisioning with the selected method** (Mit der ausgewählten Methode bereitstellen) aus.

    > [!NOTE]
    > Informationen zum Abrufen Ihrer Verbindungszeichenfolge finden Sie im vorherigen Abschnitt ([Registrieren eines IoT Edge-Geräts](#register-an-iot-edge-device)) in Schritt 3.

    :::image type="content" source="media/quickstart/wizard-provision.png" alt-text="Screenshot: Bereitstellung mithilfe des Assistenten":::

1. Wählen Sie nach Abschluss der Bereitstellung die Option **Fertig stellen** aus, um den Vorgang abzuschließen und zum Startbildschirm von Windows Admin Center zurückzukehren. Ihr Gerät sollte nun als IoT Edge-Gerät aufgeführt werden.

    :::image type="content" source="media/quickstart/windows-admin-center-device-screen.png" alt-text="Screenshot: Windows Admin Center mit Azure IoT Edge-Gerät":::

1. Wählen Sie Ihr Azure IoT Edge-Gerät aus, um das zugehörige Dashboard anzuzeigen. Sie sollten sehen, dass die Workloads von Ihrem Gerätezwilling in Azure IoT Hub bereitgestellt wurden. In der **Liste der IoT Edge-Module** sollte ein einzelnes ausgeführtes Modul (**edgeAgent**) angezeigt werden, und der **IoT Edge-Status** sollte **Aktiv (wird ausgeführt)** lauten.

Ihr IoT Edge-Gerät ist jetzt konfiguriert. Es kann nun zum Ausführen von in der Cloud bereitgestellten Modulen verwendet werden.

## <a name="deploy-a-module"></a>Bereitstellen eines Moduls

Verwalten Sie Ihr Azure IoT Edge-Gerät über die Cloud, um ein Modul bereitzustellen, das Telemetriedaten an die IoT Hub-Instanz sendet.

![Diagramm: Bereitstellen eines Moduls aus der Cloud für das Gerät](./media/quickstart/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Anzeigen generierter Daten

In diesem Schnellstart haben Sie ein neues IoT Edge-Gerät erstellt und die IoT Edge-Runtime darauf installiert. Anschließend haben Sie das Azure-Portal verwendet, um ein IoT Edge-Modul zur Ausführung auf dem Gerät bereitzustellen, ohne Änderungen am Gerät selbst vornehmen zu müssen.

In diesem Fall werden von dem Modul, das Sie gepusht haben, Daten für die Beispielumgebung generiert, die Sie später zum Testen verwenden können. Der simulierte Sensor überwacht sowohl einen Computer als auch seine Umgebung. Beispielsweise kann sich dieser Sensor in einem Serverraum, in einer Fabrik oder in einer Windturbine befinden. Er meldet die Umgebungstemperatur und Luftfeuchtigkeit, die Computertemperatur und den Druck sowie einen Zeitstempel. In den IoT Edge-Tutorials werden diese vom Modul erstellten Daten als Testdaten für die Analyse verwendet.

Vergewissern Sie sich, dass das über die Cloud bereitgestellte Modul auf Ihrem IoT Edge-Gerät ausgeführt wird, indem Sie zur Befehlsshell in Windows Admin Center navigieren.

1. Herstellen einer Verbindung mit Ihrem neu erstellten IoT Edge-Gerät

   :::image type="content" source="media/quickstart/connect-edge-screen.png" alt-text="Screenshot: Herstellen einer Verbindung mit einem Gerät":::

2. Auf der Seite **Übersicht** werden die **Liste der IoT Edge-Module** und der **IoT Edge-Status** angezeigt. Dort finden Sie die verschiedenen bereitgestellten Module sowie den Gerätestatus.  

3. Wählen Sie unter **Tools** die Option **Befehlsshell** aus. Die Befehlsshell ist ein PowerShell-Terminal, das automatisch SSH (Secure Shell) verwendet, um eine Verbindung mit dem virtuellen Linux-Computer Ihres Azure IoT Edge-Geräts auf Ihrem Windows-PC herzustellen.

   :::image type="content" source="media/quickstart/command-shell-screen.png" alt-text="Screenshot: Befehlsshell":::

4. Führen Sie den folgenden **Bash-Befehl** aus, um die drei Module auf Ihrem Gerät zu überprüfen:

   ```bash
   sudo iotedge list
   ```

   :::image type="content" source="media/quickstart/iotedge-list-screen.png" alt-text="Screenshot: Befehlsshell-Liste":::

5. Zeigen Sie die vom Temperatursensormodul an die Cloud gesendeten Nachrichten an.

   ```bash
   iotedge logs SimulatedTemperatureSensor -f
   ```

   >[!TIP]
   >Bei IoT Edge-Befehlen werden beim Verweis auf Modulnamen Groß- und Kleinschreibung unterschieden.

   :::image type="content" source="media/quickstart/temperature-sensor-screen.png" alt-text="Screenshot: Temperatursensor":::

Sie können die bei Ihrem IoT-Hub eingehenden Nachrichten auch mit der [Azure IoT Hub-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) anzeigen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit den IoT Edge-Tutorials fortfahren möchten, können Sie das Gerät verwenden, das Sie in dieser Schnellstartanleitung registriert und eingerichtet haben. Andernfalls können Sie die erstellten Azure-Ressourcen löschen, um Kosten zu vermeiden.

Wenn Sie Ihren virtuellen Computer und Azure IoT Hub in einer neuen Ressourcengruppe erstellt haben, können Sie diese Gruppe und alle zugehörigen Ressourcen löschen. Überprüfen Sie den Inhalt der Ressourcengruppe, um sicherzustellen, dass sie keine Elemente enthält, die Sie behalten möchten. Wenn Sie nicht die gesamte Gruppe löschen möchten, können Sie stattdessen einzelne Ressourcen löschen.

> [!IMPORTANT]
> Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden.

Entfernen Sie die Gruppe **IoTEdgeResources**. Das Löschen einer Ressourcengruppe kann einige Minuten dauern.

```azurecli-interactive
az group delete --name IoTEdgeResources
```

Sie können sich vergewissern, dass die Ressourcengruppe entfernt wurde, indem Sie die Liste der Ressourcengruppen anzeigen.

```azurecli-interactive
az group list
```

### <a name="clean-removal-of-azure-iot-edge-for-linux-on-windows"></a>Gründliches Entfernen von Azure IoT Edge für Linux unter Windows

Sie können Azure IoT Edge für Linux unter Windows über die Dashboarderweiterung in Windows Admin Center auf Ihrem IoT Edge-Gerät deinstallieren.

1. Stellen Sie in Windows Admin Center eine Verbindung mit dem Gerät mit Azure IoT Edge für Linux unter Windows her. Die Azure-Dashboard-Toolerweiterung wird geladen.
2. Wählen Sie **Deinstallieren** aus. Nach dem Entfernen von Azure IoT Edge für Linux unter Windows wird von Windows Admin Center die Startseite angezeigt und der Eintrag für die Azure IoT Edge-Geräteverbindung aus der Liste entfernt.

Alternativ können Sie auch auf Ihrem IoT Edge-Gerät zu **Start** > **Einstellungen** > **Apps** > **Azure IoT Edge** > **Deinstallieren** navigieren, um Azure IoT Edge aus Ihrem Windows-System zu entfernen. Dadurch wird Azure IoT Edge zwar von Ihrem IoT Edge-Gerät entfernt, die Verbindung ist jedoch weiterhin in Windows Admin Center vorhanden. Windows Admin Center kann ebenfalls über das Menü „Einstellungen“ deinstalliert werden.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein IoT Edge-Gerät erstellt und die Azure IoT Edge-Cloudschnittstelle zum Bereitstellen von Code auf dem Gerät verwendet. Sie verfügen nun über ein Testgerät, das Rohdaten zu seiner Umgebung generiert.

Im nächsten Schritt richten Sie Ihre lokale Entwicklungsumgebung ein, damit Sie mit der Entwicklung von IoT Edge-Modulen für Ihre Geschäftslogik beginnen können.

> [!div class="nextstepaction"]
> [Tutorial: Entwickeln von IoT Edge-Modulen für Linux-Geräte](tutorial-develop-for-linux.md)
