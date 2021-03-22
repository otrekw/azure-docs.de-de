---
title: 'Tutorial: Konfigurieren eines Azure IoT Edge-Geräts: Machine Learning in IoT Edge'
description: In diesem Tutorial konfigurieren Sie einen virtuellen Azure-Computer unter Linux als Azure IoT Edge-Gerät, das als transparentes Gateway fungiert.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 2/5/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: amqp, devx-track-azurecli
ms.openlocfilehash: 0ed085a161ced22efb1e5022e34b6f9b0344f942
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101721428"
---
# <a name="tutorial-configure-an-azure-iot-edge-device"></a>Tutorial: Konfigurieren eines Azure IoT Edge-Geräts

In diesem Artikel konfigurieren wir einen virtuellen Azure-Computer unter Linux als Azure IoT Edge-Gerät, das als transparentes Gateway fungiert. Die Konfiguration eines transparenten Gateways ermöglicht es Geräten, eine Verbindung mit Azure IoT Hub über das Gateway herzustellen, ohne zu wissen, dass das Gateway vorhanden ist. Gleichzeitig ist auch ein Benutzer, der mit den Geräten in IoT Hub interagiert, nicht über das zwischengeschaltete Gatewaygerät informiert. Am Ende fügen wir unserem System eine Edgeanalyse hinzu, indem wir dem transparenten Gateway IoT Edge-Module hinzufügen.

Die Schritte in diesem Artikel werden normalerweise von einem Cloudentwickler ausgeführt.

In diesem Abschnitt des Tutorials lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Erstellen von Zertifikaten, um Ihrem Gatewaygerät das Herstellen einer sicheren Verbindung mit den nachgeschalteten Geräten zu ermöglichen
> * Erstellen eines IoT Edge-Geräts
> * Erstellen eines virtuellen Azure-Computers zum Simulieren des IoT Edge-Geräts

## <a name="prerequisites"></a>Voraussetzungen

Dieser Artikel ist Teil einer Tutorialreihe zur Verwendung von Azure Machine Learning für IoT Edge. Jeder Artikel in der Reihe baut auf der Arbeit im vorherigen Artikel auf. Wenn Sie diesen Artikel direkt aufgerufen haben, wechseln Sie zum [ersten Artikel](tutorial-machine-learning-edge-01-intro.md) in der Reihe.

## <a name="create-certificates"></a>Erstellen von Zertifikaten

Damit ein Gerät als Gateway fungieren kann, muss es eine sichere Verbindung mit nachgeschalteten Geräten herstellen können. Mit IoT Edge können Sie mithilfe der Public Key-Infrastruktur (PKI) sichere Verbindungen zwischen Geräten einrichten. In diesem Fall lassen Sie zu, dass ein nachgeschaltetes IoT-Gerät eine Verbindung mit einem IoT Edge-Gerät herstellt, das als transparentes Gateway fungiert. Um eine angemessene Sicherheit zu gewährleisten, sollte das nachgeschaltete Gerät die Identität des IoT Edge-Geräts bestätigen. Weitere Informationen dazu, wie IoT Edge-Geräte Zertifikate verwenden, finden Sie unter [Details zur Verwendung von Azure IoT Edge-Zertifikaten](iot-edge-certs.md).

In diesem Abschnitt erstellen wir die selbstsignierten Zertifikate mithilfe eines Docker-Images, das wir anschließend erstellen und ausführen. Wir haben uns bei diesem Schritt für ein Docker-Image entschieden, weil sich hierdurch der Aufwand zum Erstellen der Zertifikate auf dem Windows-Entwicklungscomputer verringert. Informationen zu den mit dem Docker-Image automatisierten Komponenten finden Sie unter [Erstellen von Demozertifikaten zum Testen der Features von IoT Edge-Geräten](how-to-create-test-certificates.md).

1. Melden Sie sich bei Ihrem virtuellen Entwicklungscomputer an.
1. Erstellen Sie folgenden neuen Ordner: **c:\edgeCertificates**.

1. Wenn **Docker für Windows** noch nicht ausgeführt wird, starten Sie die Anwendung über das Windows-Startmenü.

1. Öffnen Sie Visual Studio Code.

1. Klicken Sie auf **Datei** > **Ordner öffnen**, und wählen Sie **C:\\source\\IoTEdgeAndMlSample\\CreateCertificates** aus.

1. Klicken Sie im **Explorer**-Bereich mit der rechten Maustaste auf **dockerfile**, und wählen Sie **Image erstellen** aus.

1. Übernehmen Sie im Dialogfeld den Standardwert für Imagename und Tag: **createcertificates: latest**.

    ![Screenshot: Erstellen von Zertifikaten in Visual Studio Code](media/tutorial-machine-learning-edge-05-configure-edge-device/create-certificates.png)

1. Warten Sie, bis der Buildvorgang abgeschlossen wird.

    > [!NOTE]
    > Möglicherweise wird eine Warnung zu einem fehlenden öffentlichen Schlüssel angezeigt. Sie können diese Warnung ignorieren. Sie sehen wahrscheinlich auch eine Sicherheitswarnung mit dem Hinweis, dass Sie die Berechtigungen auf Ihrem Image überprüfen bzw. zurücksetzen sollten. Sie können diesen Hinweis für dieses Image ignorieren.

1. Führen Sie im Visual Studio Code-Terminalfenster den Container „createcertificates“ aus.

    ```cmd
    docker run --name createcertificates --rm -v c:\edgeCertificates:/edgeCertificates createcertificates /edgeCertificates
    ```

1. Docker fordert Sie auf, Zugriff auf das Laufwerk **c:\\** zu gewähren. Wählen Sie **Teilen**.

1. Geben Sie Ihre Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden.

1. Überprüfen Sie nach Abschluss der Containerausführung, ob die folgenden Dateien unter **c:\\edgeCertificates** vorhanden sind:

    * c:\\edgeCertificates\\certs\\azure-iot-test-only.root.ca.cert.pem
    * c:\\edgeCertificates\\certs\\new-edge-device-full-chain.cert.pem
    * c:\\edgeCertificates\\certs\\new-edge-device.cert.pem
    * c:\\edgeCertificates\\certs\\new-edge-device.cert.pfx
    * c:\\edgeCertificates\\private\\new-edge-device.key.pem

## <a name="upload-certificates-to-azure-key-vault"></a>Hochladen von Zertifikaten in Azure Key Vault

Wir laden die Zertifikate in Azure Key Vault hoch, um sie sicher zu speichern und den Zugriff darauf von mehreren Geräten aus zu ermöglichen. Wie Sie in der Liste oben sehen, gibt es zwei Arten von Zertifikatdateien: PFX und PEM. Wir behandeln die PFX-Dateien als Key Vault-Zertifikate, die in Key Vault hochgeladen werden sollen. Die PEM-Dateien sind Nur-Text-Dateien und werden als Key Vault-Geheimnisse behandelt. Wir verwenden die Key Vault-Instanz, die dem Azure Machine Learning-Arbeitsbereich zugeordnet ist, den wir durch Ausführen der [Jupyter-Notebooks](tutorial-machine-learning-edge-04-train-model.md#run-the-jupyter-notebooks) erstellt haben.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Azure Machine Learning-Arbeitsbereich.

1. Suchen Sie auf der Übersichtsseite des Azure Machine Learning-Arbeitsbereichs nach dem Namen der **Key Vault-Instanz**.

    ![Screenshot: Kopieren des Namens der Key Vault-Instanz](media/tutorial-machine-learning-edge-05-configure-edge-device/find-key-vault-name.png)

1. Laden Sie auf Ihrem Entwicklungscomputer die Zertifikate in Key Vault hoch. Ersetzen Sie **\<subscriptionId\>** und **\<keyvaultname\>** durch Ihre Ressourceninformationen.

    ```powershell
    c:\source\IoTEdgeAndMlSample\CreateCertificates\upload-keyvaultcerts.ps1 -SubscriptionId <subscriptionId> -KeyVaultName <keyvaultname>
    ```

1. Melden Sie sich bei entsprechender Aufforderung bei Azure an.

1. Das Skript wird einige Minuten lang ausgeführt, und in der Ausgabe sind die neuen Key Vault-Einträge aufgelistet.

    ![Screenshot: Ausgabe des Key Vault-Skripts](media/tutorial-machine-learning-edge-05-configure-edge-device/key-vault-entries-output.png)

## <a name="create-an-iot-edge-device"></a>Erstellen eines IoT Edge-Geräts

Für die Herstellung einer Verbindung für ein Azure IoT Edge-Gerät mit einem IoT-Hub erstellen wir zuerst im Hub eine Identität für das Gerät. Wir verwenden die Verbindungszeichenfolge aus der Geräteidentität in der Cloud und nutzen sie zum Konfigurieren der Runtime auf unserem IoT Edge-Gerät. Nachdem ein konfiguriertes Gerät eine Verbindung mit dem Hub hergestellt hat, können wir Module bereitstellen und Nachrichten senden. Wir können auch die Konfiguration des physischen IoT Edge-Geräts ändern, indem wir die entsprechende Geräteidentität in IoT Hub ändern.

In diesem Tutorial erstellen wir die neue Geräteidentität mit Visual Studio Code. Sie können diese Schritte auch im Azure-Portal oder in der Azure CLI ausführen.

1. Öffnen Sie Visual Studio Code auf Ihrem Entwicklungscomputer.

1. Erweitern Sie in der **Explorer**-Ansicht von Visual Studio Code den Bereich **Azure IoT Hub**.

1. Klicken Sie auf die Auslassungspunkte, und wählen Sie **IoT Edge-Gerät erstellen** aus.

1. Geben Sie dem Gerät einen Namen. Der Einfachheit halber verwenden wir den Namen **aaTurbofanEdgeDevice**, da diesen an den Anfang der Geräteliste sortiert wird.

1. Das neue Gerät wird in der Geräteliste angezeigt.

    ![Screenshot: Ansicht des Geräts im Visual Studio Code-Explorer](media/tutorial-machine-learning-edge-05-configure-edge-device/iot-hub-devices-list.png)

## <a name="deploy-an-azure-virtual-machine"></a>Bereitstellen eines virtuellen Azure-Computers

Wir verwenden das Image [Azure IoT Edge on Ubuntu](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview) vom Azure Marketplace, um unser IoT Edge-Gerät für dieses Tutorial zu erstellen. Das Image „Azure IoT Edge on Ubuntu“ installiert beim Starten die aktuelle IoT Edge-Runtime und die zugehörigen Abhängigkeiten. Wir stellen die VM mit Folgendem bereit:

- Ein PowerShell-Skript: `Create-EdgeVM.ps1`
- Eine Azure Resource Manager-Vorlage: `IoTEdgeVMTemplate.json`
- Ein Shellskript: `install packages.sh`

### <a name="enable-programmatic-deployment"></a>Aktivieren der programmgesteuerten Bereitstellung

Um das Image aus dem Azure Marketplace für eine skriptgesteuerte Bereitstellung verwenden zu können, müssen wir die programmgesteuerte Bereitstellung für das Image aktivieren.

1. Melden Sie sich beim Azure-Portal an.

1. Wählen Sie **Alle Dienste** aus.

1. Geben Sie in der Suchleiste **Marketplace** ein, und wählen Sie den entsprechenden Eintrag aus.

1. Geben Sie auf der Marketplace-Suchleiste **Azure IoT Edge unter Ubuntu** ein, und wählen Sie den entsprechenden Eintrag aus.

1. Wählen Sie den Link **Erste Schritte** aus, um die programmgesteuerte Bereitstellung durchzuführen.

1. Klicken Sie auf die Schaltfläche **Aktivieren** und dann auf **Speichern**.

    ![Screenshot: Aktivieren der programmgesteuerten Bereitstellung für einen virtuellen Computer](media/tutorial-machine-learning-edge-05-configure-edge-device/deploy-ubuntu-vm.png)

1. Eine Erfolgsmeldung wird angezeigt.

### <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Führen Sie als Nächstes das Skript zum Erstellen des virtuellen Computers für Ihr IoT Edge-Gerät aus.

1. Öffnen Sie ein PowerShell-Fenster, und wechseln Sie zum Verzeichnis **EdgeVM**.

    ```powershell
    cd c:\source\IoTEdgeAndMlSample\EdgeVM
    ```

1. Führen Sie das Skript aus, um den virtuellen Computer zu erstellen.

    ```powershell
    .\Create-EdgeVm.ps1
    ```

1. Geben Sie einen Wert für jeden Parameter an, wenn Sie dazu aufgefordert werden. Es empfiehlt sich, für das Abonnement, die Ressourcengruppe und den Standort die gleichen Werte anzugeben wie für alle anderen Ressourcen im Rahmen dieses Tutorials.

    * **Azure-Abonnement-ID**: Über das Azure-Portal ermittelbar.
    * **Ressourcengruppenname**: Einprägsamer Name zur Gruppierung der Ressourcen für dieses Tutorial.
    * **Standort**: Azure-Standort, an dem der virtuelle Computer erstellt wird. Beispiel: „westus2“ oder „northeurope“. Weitere Standorte finden Sie unter [Azure-Standorte](https://azure.microsoft.com/global-infrastructure/locations/).
    * **AdminUsername**: Der Name für das Administratorkonto, das Sie zum Anmelden beim virtuellen Computer verwenden.
    * **AdminPassword**: Das Kennwort, das für den AdminUsername auf dem virtuellen Computer festgelegt ist.

1. Damit die VM mit dem Skript eingerichtet werden kann, melden Sie sich bei Azure mit den Anmeldeinformationen an, die dem von Ihnen genutzten Azure-Abonnement zugeordnet sind.

1. Das Skript bestätigt die Informationen für die Erstellung Ihres virtuellen Computers. Drücken Sie **y** oder die **EINGABETASTE**, um fortzufahren.

1. Die Ausführung des Skripts dauert mehrere Minuten, und es werden die folgenden Schritte ausgeführt:

    * Die Ressourcengruppe wird erstellt, sofern sie noch nicht vorhanden ist.
    * Der virtuelle Computer wird erstellt.
    * NSG-Ausnahmen für die VM für die Ports 22 (SSH), 5671 (AMQP), 5672 (AMPQ) und 443 (TLS) werden hinzugefügt.
    * Die [Azure CLI](/cli/azure/install-azure-cli-apt) wird installiert.

1. Das Skript gibt die SSH-Verbindungszeichenfolge für die Herstellung der Verbindung mit der VM aus. Kopieren Sie die Verbindungszeichenfolge für den nächsten Schritt.

    ![Screenshot: Kopieren der SSH-Verbindungszeichenfolge für einen virtuellen Computer](media/tutorial-machine-learning-edge-05-configure-edge-device/vm-ssh-connection-string.png)

## <a name="connect-to-your-iot-edge-device"></a>Herstellen einer Verbindung mit Ihrem IoT Edge-Gerät

In den nächsten Abschnitten wird der von uns erstellte virtuelle Azure-Computer konfiguriert. Der erste Schritt ist die Herstellung der Verbindung mit dem virtuellen Computer.

1. Öffnen Sie eine Eingabeaufforderung, und fügen Sie die SSH-Verbindungszeichenfolge ein, die Sie aus der Ausgabe des Skripts kopiert haben. Geben Sie Ihre eigenen Informationen für Benutzername, Suffix und Region gemäß den Werten ein, die Sie im vorherigen Abschnitt für das PowerShell-Skript angegeben haben.

    ```cmd
    ssh -l <username> iotedge-<suffix>.<region>.cloudapp.azure.com
    ```

1. Geben Sie **yes** ein, und drücken Sie die **EINGABETASTE**, wenn Sie zur Bestätigung der Echtheit des Hosts aufgefordert werden.

1. Geben Sie bei entsprechender Aufforderung Ihr Kennwort an.

1. Ubuntu zeigt eine Begrüßungsnachricht und dann eine Aufforderung wie diese `<username>@<machinename>:~$` an.

## <a name="download-key-vault-certificates"></a>Herunterladen von Key Vault-Zertifikaten

Weiter oben in diesem Artikel haben wir Zertifikate in Key Vault hochgeladen, um sie für unser IoT Edge-Gerät und für unser Blattgerät verfügbar zu machen. Das Blattgerät ist ein nachgeschaltetes Gerät, das das IoT Edge-Gerät als Gateway für die Kommunikation mit IoT Hub verwendet.

Um das Blattgerät kümmern wir uns später in diesem Tutorial. In diesem Abschnitt laden Sie die Zertifikate auf das IoT Edge-Gerät herunter.

1. Melden Sie sich über die SSH-Sitzung auf dem virtuellen Linux-Computer mit der Azure CLI an Azure an.

    ```azurecli
    az login
    ```

1. Sie werden aufgefordert, die Seite einer [Microsoft-Geräteanmeldung](https://microsoft.com/devicelogin) in einem Browser zu öffnen und einen eindeutigen Code anzugeben. Sie können diese Schritte auf Ihrem lokalen Computer ausführen. Schließen Sie das Browserfenster, wenn Sie die Authentifizierung abgeschlossen haben.

1. Wenn die Authentifizierung erfolgreich war, wird der virtuelle Linux-Computer angemeldet, und Ihre Azure-Abonnements werden aufgelistet.

1. Legen Sie das Azure-Abonnement fest, das Sie für Azure CLI-Befehle verwenden möchten.

    ```azurecli
    az account set --subscription <subscriptionId>
    ```

1. Erstellen Sie auf der VM ein Verzeichnis für die Zertifikate.

    ```bash
    sudo mkdir /edgeMlCertificates
    ```

1. Laden Sie die Zertifikate herunter, die Sie im Schlüsseltresor gespeichert haben: „new-edge-device-full-chain.cert.pem“, „new-edge-device.key.pem“ und „azure-iot-test-only.root.ca.cert.pem“.

    ```azurecli
    key_vault_name="<key vault name>"
    sudo az keyvault secret download --vault-name $key_vault_name --name new-edge-device-full-chain-cert-pem -f /edgeMlCertificates/new-edge-device-full-chain.cert.pem
    sudo az keyvault secret download --vault-name $key_vault_name --name new-edge-device-key-pem -f /edgeMlCertificates/new-edge-device.key.pem
    sudo az keyvault secret download --vault-name $key_vault_name --name azure-iot-test-only-root-ca-cert-pem -f /edgeMlCertificates/azure-iot-test-only.root.ca.cert.pem
    ```

## <a name="update-the-iot-edge-device-configuration"></a>Aktualisieren der Konfiguration für das IoT Edge-Gerät

Für die IoT Edge-Runtime wird die Datei „/etc/iotedge/config.yaml“ genutzt, um die Konfiguration zu speichern. Wir müssen in dieser Datei drei Arten von Informationen aktualisieren:

* **Geräteverbindungszeichenfolge**: die Verbindungszeichenfolge der Identität dieses Geräts in IoT Hub
* **Zertifikate**: die Zertifikate, die für die Verbindungsherstellung mit nachgeschalteten Geräten verwendet werden
* **Hostname**: der vollqualifizierte Domänenname (FQDN) des IoT Edge-Geräts der VM

Das Image „Azure IoT Edge on Ubuntu“, das wir zum Erstellen der IoT Edge-VM verwendet haben, enthält ein Shellskript, das die Datei „config.yaml“ mit der Verbindungszeichenfolge aktualisiert.

1. Klicken Sie in Visual Studio Code mit der rechten Maustaste auf das IoT Edge-Gerät, und wählen Sie die Option **Verbindungszeichenfolge des Geräts kopieren** aus.

    ![Screenshot: Kopieren der Verbindungszeichenfolge aus Visual Studio Code](media/tutorial-machine-learning-edge-05-configure-edge-device/copy-device-connection-string-command.png)

1. Führen Sie in Ihrer SSH-Sitzung den Befehl zum Aktualisieren der Datei „config.yaml“ mit Ihrer Geräteverbindungszeichenfolge aus.

    ```bash
    sudo /etc/iotedge/configedge.sh "<your_iothub_edge_device_connection_string>"
    ```

Als Nächstes aktualisieren wir die Zertifikate und den Hostnamen, indem wir die Datei „config.yaml“ direkt bearbeiten.

1. Öffnen Sie die Datei „config.yaml“.

    ```bash
    sudo nano /etc/iotedge/config.yaml
    ```

1. Aktualisieren Sie den Abschnitt „certificates“ der Datei „config.yaml“, indem Sie das vorangestellte Zeichen **#** entfernen und den Pfad festlegen, sodass die Datei wie im folgenden Beispiel aussieht:

    ```yaml
    certificates:
      device_ca_cert: "/edgeMlCertificates/new-edge-device-full-chain.cert.pem"
      device_ca_pk: "/edgeMlCertificates/new-edge-device.key.pem"
      trusted_ca_certs: "/edgeMlCertificates/azure-iot-test-only.root.ca.cert.pem"
    ```

    Stellen Sie sicher, dass der Zeile **certificates:** kein Leerzeichen vorangestellt ist und dass die geschachtelten Zertifikate jeweils um zwei Leerstellen eingerückt sind.

    Wenn Sie in Nano mit der rechten Maustaste klicken, wird der Inhalt Ihrer Zwischenablage an der aktuellen Cursorposition eingefügt. Verwenden Sie die Pfeiltasten auf der Tastatur, um zu der Zeichenfolge zu navigieren, die ersetzt werden soll. Löschen Sie die Zeichenfolge, und klicken Sie dann mit der rechten Maustaste, um den Inhalt aus dem Puffer einzufügen.

1. Navigieren Sie im Azure-Portal zu Ihrem virtuellen Computer. Kopieren Sie den DNS-Namen (FQDN des Computers) aus dem Abschnitt **Übersicht**.

1. Fügen Sie den FQDN in den Abschnitt „hostname“ der Datei „config.yaml“ ein. Stellen Sie sicher, dass der Name ausschließlich Kleinbuchstaben enthält.

    ```yaml
    hostname: '<machinename>.<region>.cloudapp.azure.com'
    ```

1. Speichern und schließen Sie die Datei mit **STRG+X**, **Y** und **EINGABETASTE**.

1. Starten Sie den IoT Edge-Daemon neu.

    ```bash
    sudo systemctl restart iotedge
    ```

1. Überprüfen Sie den Status des IoT Edge-Daemons. Geben Sie nach Ausführung des Befehls zum Beenden **:q** ein.

    ```bash
    systemctl status iotedge
    ```

1. Falls im Statusbereich Fehler angezeigt werden (farbiger Text mit dem Präfix „\[ERROR\]“), untersuchen Sie die Daemonprotokolle, um ausführliche Fehlerinformationen zu erhalten.

    ```bash
    journalctl -u iotedge --no-pager --no-full
    ```
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Dieses Tutorial ist Teil einer Reihe, in der jeder Artikel auf den Schritten aufbaut, die jeweils im vorherigen Artikel ausgeführt wurden. Warten Sie mit dem Bereinigen von Ressourcen, bis Sie das letzte Tutorial abgeschlossen haben.

## <a name="next-steps"></a>Nächste Schritte

Wir haben die Konfiguration einer Azure-VM als transparentes IoT Edge-Gateway abgeschlossen. Zunächst haben wir Testzertifikate generiert, die wir in Key Vault hochgeladen haben. Anschließend haben wir ein Skript und eine Resource Manager-Vorlage verwendet, um die VM mit dem Image „Ubuntu Server 16.04 LTS + Azure IoT Edge-Runtime“ aus dem Azure Marketplace bereitzustellen. Als die VM eingerichtet und betriebsbereit war, haben wir über SSH eine Verbindung hergestellt. Danach haben wir uns bei Azure angemeldet und Zertifikate aus Key Vault heruntergeladen. Wir haben die Datei „config.yaml“ aktualisiert, um verschiedene Aspekte der IoT Edge-Runtimekonfiguration zu aktualisieren.

Fahren Sie mit dem nächsten Artikel fort, um IoT Edge-Module zu erstellen.

> [!div class="nextstepaction"]
> [Erstellen und Bereitstellen von benutzerdefinierten IoT Edge-Modulen](tutorial-machine-learning-edge-06-custom-modules.md)
