---
title: 'Tutorial: Erstellen einer Hierarchie für IoT Edge-Geräte – Azure IoT Edge'
description: In diesem Tutorial wird veranschaulicht, wie Sie eine hierarchische Struktur für IoT Edge-Geräte mit Gateways erstellen.
author: v-tcassi
manager: philmea
ms.author: v-tcassi
ms.date: 2/26/2021
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
monikerRange: '>=iotedge-2020-11'
ms.openlocfilehash: c1b30a1eafe9af92c1ef3f81773d213ccf96555c
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/14/2021
ms.locfileid: "103462030"
---
# <a name="tutorial-create-a-hierarchy-of-iot-edge-devices-preview"></a>Tutorial: Erstellen einer Hierarchie für IoT Edge-Geräte (Vorschauversion)

[!INCLUDE [iot-edge-version-202011](../../includes/iot-edge-version-202011.md)]

Stellen Sie netzwerkübergreifend Azure IoT Edge-Knoten bereit, die in hierarchischen Ebenen angeordnet sind. Jede Ebene einer Hierarchie entspricht einem Gatewaygerät, mit dem Nachrichten und Anforderungen von den Geräten der direkt untergeordneten Ebene verarbeitet werden.

>[!NOTE]
>Für dieses Feature ist die IoT Edge-Version 1.2, die sich in der öffentlichen Vorschauphase befindet, und die Ausführung von Linux-Containern erforderlich.

Sie können eine Hierarchie mit Geräten so strukturieren, dass nur die oberste Ebene über Konnektivität mit der Cloud verfügt und die untergeordneten Ebenen nur mit der Ebene direkt darüber bzw. darunter kommunizieren können. Diese Netzwerkebenen sind die Grundlage der meisten Unternehmensnetzwerke, die auf der [ISA-95-Norm](https://en.wikipedia.org/wiki/ANSI/ISA-95) basieren.

Das Ziel dieses Tutorials ist die Erstellung einer Hierarchie mit IoT Edge-Geräten, mit der eine Produktionsumgebung simuliert wird. Zum Schluss stellen Sie das [Simulated Temperature Sensor-Modul](https://azuremarketplace.microsoft.com/marketplace/apps/azure-iot.simulated-temperature-sensor) auf einem Gerät einer untergeordneten Ebene ohne Internetzugriff bereit, indem Sie Containerimages über die Hierarchie herunterladen.

Zur Erreichung dieses Ziels werden in diesem Tutorial Schritt für Schritt die Erstellung einer Hierarchie mit IoT Edge-Geräten, die Bereitstellung von IoT Edge-Runtimecontainern auf Ihren Geräten und die lokale Konfiguration der Geräte beschrieben. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Erstellen und Definieren der Beziehungen in einer Hierarchie mit IoT Edge-Geräten
> * Konfigurieren der IoT Edge-Runtime auf den Geräten in Ihrer Hierarchie
> * Installieren von einheitlichen Zertifikaten in Ihrer gesamten Gerätehierarchie
> * Hinzufügen von Workloads zu den Geräten Ihrer Hierarchie
> * Verwenden eines [IoT Edge-API-Proxymoduls](https://azuremarketplace.microsoft.com/marketplace/apps/azure-iot.azureiotedge-api-proxy?tab=Overview) zum sicheren Weiterleiten von HTTP-Datenverkehr über einen einzelnen Port von Ihren Geräten der unteren Ebenen

In diesem Tutorial werden die folgenden Netzwerkebenen definiert:

* **Oberste Ebene**: IoT Edge-Geräte auf dieser Ebene können eine direkte Verbindung mit der Cloud herstellen.

* **Untere Ebenen**: IoT Edge-Geräte in Ebenen unterhalb der obersten Ebene können keine direkte Verbindung mit der Cloud herstellen. Zum Senden und Empfangen von Daten müssen sie den Weg über ein oder mehrere zwischengeschaltete IoT Edge-Geräte nehmen.

In diesem Tutorial wird der Einfachheit halber eine Hierarchie mit nur zwei Geräten verwendet, die weiter unten dargestellt ist. Ein Gerät (**Top Layer Device**, „Gerät der obersten Ebene“) befindet sich auf der obersten Ebene der Hierarchie und kann eine direkte Verbindung mit der Cloud herstellen. Dieses Gerät wird auch als **übergeordnetes Gerät** bezeichnet. Das andere Gerät (**Lower Layer Device**, „Gerät der unteren Ebene“) befindet sich auf der untergeordneten Ebene der Hierarchie und kann keine direkte Verbindung mit der Cloud herstellen. Sie können nach Bedarf weitere untergeordnete Geräte hinzufügen, um Ihre Produktionsumgebung darzustellen. Geräte in unteren Ebenen werden auch als **untergeordnete Geräte** bezeichnet. Die Konfiguration aller zusätzlichen untergeordneten Geräte basiert auf der Konfiguration des **Geräts der unteren Ebene**.

![Struktur der Tutorialhierarchie, die zwei Geräte enthält: das Gerät der obersten und das der unteren Ebene](./media/tutorial-nested-iot-edge/tutorial-hierarchy-diagram.png)

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um eine Hierarchie mit IoT Edge-Geräten zu erstellen:

* Einen Computer (Windows oder Linux) mit Internetverbindung.
* Ein Azure-Konto mit einem gültigen Abonnement. Wenn Sie kein [Azure-Abonnement](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing) besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/), bevor Sie beginnen.
* Eine [IoT Hub-Instanz](../iot-hub/iot-hub-create-through-portal.md) in Azure mit dem Tarif „Free“ oder „Standard“.
* Azure CLI v2.3.1 mit einer Installation der Azure IoT-Erweiterung v0.10.6 oder höher. In diesem Tutorial wird [Azure Cloud Shell](../cloud-shell/overview.md) verwendet. Falls Sie mit Azure Cloud Shell noch nicht vertraut sein sollten, helfen Ihnen die [Informationen in der Schnellstartanleitung](./quickstart-linux.md#prerequisites) weiter.
  * Wenn Sie die aktuellen Versionen der Azure CLI-Module und -Erweiterungen anzeigen möchten, führen Sie [az version](/cli/azure/reference-index?#az_version) aus.
* Ein Linux-Gerät, das für jedes Gerät in der Hierarchie als IoT Edge-Gerät konfiguriert wird. In diesem Tutorial werden zwei Geräte verwendet: Sind keine Geräte verfügbar, können Sie für jedes Gerät in Ihrer Hierarchie einen virtuellen Azure-Computer erstellen, indem Sie den Platzhaltertext im folgenden Befehl ersetzen und diesen ausführen:

   ```azurecli-interactive
   az vm create \
    --resource-group <REPLACE_WITH_RESOURCE_GROUP> \
    --name <REPLACE_WITH_UNIQUE_NAMES_FOR_EACH_VM> \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password <REPLACE_WITH_PASSWORD>
   ```

* Stellen Sie sicher, dass die folgenden Ports in eingehender Richtung geöffnet sind: 8000, 443, 5671, 8883:
  * 8000: Wird zum Pullen von Docker-Containerimages über den API-Proxy verwendet.
  * 443: Wird zwischen übergeordneten und untergeordneten Edge-Hubs für REST-API-Aufrufe verwendet.
  * 5671, 8883: Wird für AMQP und MQTT verwendet.

  Weitere Informationen finden Sie unter [Öffnen von Ports zu einem virtuellen Computer mit dem Azure-Portal](../virtual-machines/windows/nsg-quickstart-portal.md).

>[!TIP]
>Wenn Sie einen Einblick in die automatisierte Einrichtung einer Hierarchie von IoT Edge-Geräten verschaffen möchten, können Sie das Beispielskript unter [Azure IoT Edge-Beispiel für industrielles IoT](https://aka.ms/iotedge-nested-sample) ausführen. In diesem geskripteten Szenario werden Azure-VMs als vorkonfigurierte Geräte bereitgestellt, um eine Factoryumgebung zu simulieren.
>
>Wenn Sie mit der Schritt-für-Schritt-Erstellung der Beispielhierarchie fortfahren möchten, fahren Sie mit den folgenden Tutorialschritten fort.

## <a name="configure-your-iot-edge-device-hierarchy"></a>Konfigurieren Ihrer Hierarchie für IoT Edge-Geräte

### <a name="create-a-hierarchy-of-iot-edge-devices"></a>Erstellen einer Hierarchie für IoT Edge-Geräte

IoT Edge-Geräte bilden die Ebenen Ihrer Hierarchie. In diesem Tutorial wird eine Hierarchie aus zwei IoT Edge-Geräten erstellt: aus dem **Gerät der obersten** und dem der **unteren Ebene**. Sie können nach Bedarf weitere untergeordnete Geräte erstellen.

Ihre IoT Edge-Geräte können Sie über das Azure-Portal oder die Azure CLI erstellen.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Navigieren Sie im [Azure-Portal](https://ms.portal.azure.com/) zu Ihrer IoT Hub-Instanz.

1. Wählen Sie im Menü im linken Bereich unter **Automatische Geräteverwaltung** die Option **IoT Edge** aus.

1. Wählen Sie **+ IoT Edge-Gerät hinzufügen** aus. Da dies das Gerät der obersten Ebene ist, sollten Sie eine geeignete eindeutige Geräte-ID eingeben. Wählen Sie **Speichern** aus.

1. Wählen Sie erneut die Option **+ IoT Edge-Gerät hinzufügen** aus. Da dies ein untergeordnetes Gerät wird, sollten Sie eine geeignete eindeutige Geräte-ID eingeben.

1. Wählen Sie die Option **Übergeordnetes Gerät festlegen** und dann in der Liste mit den Geräten Ihr Gerät der obersten Ebene und **OK** aus. Wählen Sie **Speichern** aus.

   ![Festlegen des übergeordneten Geräts für das untergeordnete Gerät](./media/tutorial-nested-iot-edge/set-parent-device.png)

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

1. Geben Sie in [Azure Cloud Shell](https://shell.azure.com/) den folgenden Befehl ein, um auf Ihrem Hub ein IoT Edge-Gerät zu erstellen. Da dies das Gerät der obersten Ebene ist, sollten Sie eine geeignete eindeutige Geräte-ID eingeben:

   ```azurecli-interactive
   az iot hub device-identity create --device-id {top_layer_device_id} --edge-enabled --hub-name {hub_name}
   ```

   Bei erfolgreicher Geräteerstellung wird die JSON-Konfiguration des Geräts ausgegeben.

   ![JSON-Ausgabe einer erfolgreichen Geräteerstellung](./media/tutorial-nested-iot-edge/json-success-output.png)

1. Geben Sie den folgenden Befehl ein, um ein IoT Edge-Gerät der unteren Ebene zu erstellen. Sie können mehr als ein Gerät der unteren Ebene erstellen, wenn Ihre Hierarchie aus mehreren Ebenen bestehen soll. Achten Sie darauf, für jedes Gerät eine eindeutige Identität bereitzustellen.

   ```azurecli-interactive
   az iot hub device-identity create --device-id {lower_layer_device_id} --edge-enabled --hub-name {hub_name}
   ```

1. Geben Sie den folgenden Befehl ein, um alle über- und untergeordneten Beziehungen zwischen dem **Gerät der obersten Ebene** und den **Geräten der unteren Ebene** zu definieren. Achten Sie darauf, diesen Befehl für jedes Gerät der unteren Ebene in Ihrer Hierarchie auszuführen.

   ```azurecli-interactive
   az iot hub device-identity parent set --device-id {lower_layer_device_id} --parent-device-id {top_layer_device_id} --hub-name {hub_name}
   ```

   Dieser Befehl hat keine explizite Ausgabe.

---

Notieren Sie sich als Nächstes die Verbindungszeichenfolgen der einzelnen IoT Edge-Geräte. Diese werden später noch benötigt.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Navigieren Sie im [Azure-Portal](https://ms.portal.azure.com/) zum Abschnitt **IoT Edge** Ihrer IoT Hub-Instanz.

1. Klicken Sie in der Liste mit den Geräten auf die Geräte-ID eines Geräts.

1. Wählen Sie im Feld **Primäre Verbindungszeichenfolge** die Option **Kopieren** aus, und fügen Sie sie an einem Ort Ihrer Wahl ein.

1. Wiederholen Sie diesen Schritt für alle anderen Geräte.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

1. Geben Sie in [Azure Cloud Shell](https://shell.azure.com/) für jedes Gerät den folgenden Befehl ein, um die Verbindungszeichenfolge Ihres Geräts abzurufen, und fügen Sie sie an einem Ort Ihrer Wahl ein:

   ```azurecli-interactive
   az iot hub device-identity connection-string show --device-id {device_id} --hub-name {hub_name}
   ```

---

Wenn Sie die obigen Schritte richtig ausgeführt haben, können Sie wie folgt im Azure-Portal oder über die Azure CLI überprüfen, ob Ihre über- und untergeordneten Beziehungen korrekt sind.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Navigieren Sie im [Azure-Portal](https://ms.portal.azure.com/) zum Abschnitt **IoT Edge** Ihrer IoT Hub-Instanz.

1. Klicken Sie in der Geräteliste auf die Geräte-ID eines **Geräts der unteren Ebene**.

1. Auf der Detailseite des Geräts sollte die Identität Ihres **Geräts der obersten Ebene** neben dem Feld **Übergeordnetes Gerät** angezeigt werden.

   [Untergeordnetes Gerät bestätigt übergeordnetes Gerät](./media/tutorial-nested-iot-edge/lower-layer-device-parent.png)

Sie können die Hierarchiebeziehungen auch über Ihr **Gerät der obersten Ebene** aufrufen.

1. Klicken Sie in der Geräteliste auf die Geräte-ID eines **Geräts der obersten Ebene**.

1. Klicken Sie oben auf die Registerkarte **Untergeordnete Geräte verwalten**.

1. In der Liste der Geräte sollte Ihr **Gerät der unteren Ebene** angezeigt werden.

   [Übergeordnetes Gerät bestätigt untergeordnetes Gerät](./media/tutorial-nested-iot-edge/top-layer-device-child.png)

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

1. In [Azure Cloud Shell](https://shell.azure.com/) können Sie überprüfen, ob Ihre untergeordneten Geräte erfolgreich eine Beziehung mit dem übergeordneten Gerät hergestellt haben, indem Sie für die untergeordneten Geräte die Identität des jeweiligen bestätigten übergeordneten Geräts abrufen. Dies gibt, so wie oben dargestellt, die JSON-Konfiguration des übergeordneten Geräts aus:

   ```azurecli-interactive
   az iot hub device-identity parent show --device-id {lower_layer_device_id} --hub-name {hub_name}
   ```

Sie können die Hierarchiebeziehungen auch durch Abfragen Ihres **Geräts der obersten Ebene** aufrufen.

1. Listen Sie die untergeordneten Geräte auf, die dem übergeordneten Gerät bekannt sind:

    ```azurecli-interactive
    az iot hub device-identity children list --device-id {top_layer_device_id} --hub-name {hub_name}
    ```

---

Wenn Sie mit der Struktur Ihrer Hierarchie zufrieden sind, können Sie fortfahren.

### <a name="create-certificates"></a>Erstellen von Zertifikaten

Für alle Geräte eines [Gatewayszenarios](iot-edge-as-gateway.md) ist ein gemeinsames Zertifikat erforderlich, um dafür sichere Verbindungen einzurichten. Führen Sie die folgenden Schritte aus, um in diesem Szenario Demozertifikate für beide Geräte zu erstellen.

Zum Erstellen von Demozertifikaten auf einem Linux-Gerät müssen Sie die Generierungsskripts klonen und für die lokale Ausführung in Bash einrichten.

1. Klonen Sie das IoT Edge-Git-Repository, das Skripts zum Generieren von Demozertifikaten enthält.

   ```bash
   git clone https://github.com/Azure/iotedge.git
   ```

1. Navigieren Sie zu dem Verzeichnis, in dem Sie arbeiten möchten. Alle Zertifikat- und Schlüsseldateien werden in diesem Verzeichnis erstellt.

1. Kopieren Sie die Konfigurations- und Skriptdateien aus dem geklonten IoT Edge-Repository in Ihr Arbeitsverzeichnis.

   ```bash
   cp <path>/iotedge/tools/CACertificates/*.cnf .
   cp <path>/iotedge/tools/CACertificates/certGen.sh .
   ```

1. Erstellen Sie das Zertifikat der Stammzertifizierungsstelle und ein Zwischenzertifikat.

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   Mit diesem Skriptbefehl werden mehrere Zertifikat- und Schlüsseldateien erstellt, aber wir verwenden die folgende Datei als **Zertifikat der Stammzertifizierungsstelle** für die Gatewayhierarchie:

   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`  

1. Erstellen Sie mit dem folgenden Befehl zwei Gruppen mit IoT Edge-Zertifizierungsstellenzertifikaten und privaten Schlüsseln: eine für das Gerät der obersten Ebene und eine für das untergeordnete Gerät. Geben Sie für die Zertifizierungsstellenzertifikate aussagekräftige Namen an, damit sie gut voneinander unterschieden werden können.

   ```bash
   ./certGen.sh create_edge_device_ca_certificate "{top_layer_certificate_name}"
   ./certGen.sh create_edge_device_ca_certificate "{lower_layer_certificate_name}"
   ```

   Mit diesem Skriptbefehl werden mehrere Zertifikat- und Schlüsseldateien erstellt. Für die einzelnen IoT Edge-Geräte werden jedoch das folgende Zertifikat und Schlüsselpaar verwendet, auf die in der Konfigurationsdatei verwiesen wird:

   * `<WRKDIR>/certs/iot-edge-device-<CA cert name>-full-chain.cert.pem`
   * `<WRKDIR>/private/iot-edge-device-<CA cert name>.key.pem`

Für jedes Gerät werden eine Kopie des Zertifikats der Stammzertifizierungsstelle sowie Kopien des eigenen Zertifizierungsstellenzertifikats und privaten Schlüssels für das Gerät benötigt. Sie können ein USB-Laufwerk oder [Secure File Copy](https://www.ssh.com/ssh/scp/) verwenden, um die Zertifikate auf die einzelnen Geräte zu verschieben.

1. Installieren Sie nach Abschluss der Zertifikatübertragung jeweils das Zertifikat der Stammzertifizierungsstelle für die Geräte.

   ```bash
   sudo cp <path>/azure-iot-test-only.root.ca.cert.pem /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
   sudo update-ca-certificates
   ```

   Mit diesem Befehl sollte ausgegeben werden, dass unter `/etc/ssl/certs` ein Zertifikat hinzugefügt wurde.

   [Meldung der erfolgreichen Zertifikatinstallation](./media/tutorial-nested-iot-edge/certificates-success-output.png)

Wenn Sie die oben genannten Schritte richtig ausgeführt haben, können Sie die Installation Ihrer Zertifikate in `/etc/ssl/certs` überprüfen, indem Sie zu diesem Verzeichnis navigieren und nach den installierten Zertifikaten suchen.

1. Navigieren Sie zu `/etc/ssl/certs`:

   ```bash
   cd /etc/ssl/certs
   ```

1. Listen Sie die installierten Zertifikate und `grep` für `azure` auf:

   ```bash
   ll | grep azure
   ```

   Es sollte ein Zertifikathash angezeigt werden, der mit dem Zertifikat der Stammzertifizierungsstelle verknüpft ist. Außerdem sollte das Zertifikat der Stammzertifizierungsstelle angezeigt werden, das mit der Kopie in Ihrem Verzeichnis `usr/local/share` verknüpft ist.

   [Ergebnisse der Suche nach Azure-Zertifikaten](./media/tutorial-nested-iot-edge/certificate-list-results.png)

Wenn Sie überprüft haben, ob Ihre Zertifikate auf jedem Gerät installiert sind, können Sie fortfahren.

### <a name="install-iot-edge-on-the-devices"></a>Installieren von IoT Edge auf den Geräten

Wenn Sie Runtimeimages der IoT Edge-Version 1.2 installieren, erhalten Ihre Gerätezugriff auf die Features, die für das Funktionieren als Gerätehierarchie erforderlich sind.

Für die Installation von IoT Edge müssen Sie die entsprechende Repositorykonfiguration, die erforderlichen Komponenten und die erforderlichen Releaseressourcen installieren.

1. Installieren Sie die zum Betriebssystem Ihres Geräts passende Repositorykonfiguration.

   * **Ubuntu Server 18.04**:

     ```bash
     curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
     ```

   * **Raspberry Pi OS Stretch**:

     ```bash
     curl https://packages.microsoft.com/config/debian/stretch/multiarch/prod.list > ./microsoft-prod.list
     ```

1. Kopieren Sie die generierte Liste in das Verzeichnis „sources.list.d“.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

1. Installieren Sie den öffentlichen Schlüssel von Microsoft GPG.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

1. Aktualisieren Sie die Paketlisten auf Ihrem Gerät.

   ```bash
   sudo apt-get update
   ```

1. Installieren Sie die Moby-Engine.

   ```bash
   sudo apt-get install moby-engine
   ```

1. Installieren Sie IoT Edge und den IoT-Identitätsdienst.

   ```bash
   sudo apt-get install aziot-edge
   ```

Wenn Sie die vorherigen Schritte richtig ausgeführt haben, wurde Ihnen in Azure IoT Edge die Bannermeldung mit der Aufforderung angezeigt, die Azure IoT Edge-Konfigurationsdatei `/etc/aziot/config.toml` auf jedem Gerät Ihrer Hierarchie zu aktualisieren. Wenn dies der Fall ist, können Sie fortfahren.

### <a name="configure-the-iot-edge-runtime"></a>Konfigurieren der IoT Edge-Runtime

Zusätzlich zur Bereitstellung Ihrer Geräte wird durch diese Konfigurationsschritte die vertrauenswürdige Kommunikation zwischen den Geräten in der Hierarchie mithilfe Ihrer zuvor erstellten Zertifikate eingerichtet. Außerdem beginnen diese Schritte mit der Einrichtung der Netzwerkstruktur Ihrer Hierarchie. Das Gerät der obersten Ebene erhält die Internetverbindung aufrecht und ermöglicht es, Images für seine Runtime aus der Cloud zu pullen, während die Geräte der unteren Ebene über das Gerät der obersten Ebene auf diese Images zugreifen können.

Zum Konfigurieren der IoT Edge-Runtime müssen Sie mehrere Komponenten der Konfigurationsdatei ändern. Die Konfigurationen unterscheiden sich geringfügig zwischen dem **Gerät der obersten Ebene** und einem **Gerät der unteren Ebene**. Achten Sie also in den einzelnen Schritten darauf, für welches Gerät Sie die Konfigurationsdatei bearbeiten. Die Konfiguration der IoT Edge-Runtime für Ihre Geräte umfasst vier Schritte, in denen jeweils die IoT Edge-Konfigurationsdatei bearbeitet wird:

* Stellen Sie die Geräte jeweils manuell bereit, indem Sie die Verbindungszeichenfolge des Geräts der Konfigurationsdatei hinzufügen.

* Beenden Sie die Einrichtung der Zertifikate Ihres Geräts, indem Sie in der Konfigurationsdatei auf das Zertifizierungsstellenzertifikat des Geräts, den privaten Schlüssel der Zertifizierungsstelle für das Gerät und das Zertifikat der Stammzertifizierungsstelle verweisen.

* Führen Sie für das System einen Bootstrapvorgang durch, indem Sie den IoT Edge-Agent verwenden.

* Aktualisieren Sie den Parameter **hostname** für Ihr Gerät der **obersten Ebene**, und aktualisieren Sie sowohl den Parameter **hostname** als auch den Parameter **parent_hostname** für Ihre **untergeordneten** Geräte.

Führen Sie die folgenden Schritte aus, und starten Sie den IoT Edge-Dienst neu, um Ihre Geräte zu konfigurieren.

>[!TIP]
>Wenn Sie in Nano in der Konfigurationsdatei navigieren, können Sie mithilfe von **STRG+W** in der Datei nach Schlüsselwörtern suchen.

1. Erstellen Sie auf jedem Gerät anhand der enthaltenen Vorlage eine Konfigurationsdatei.

   ```bash
   sudo cp /etc/aziot/config.toml.edge.template /etc/aziot/config.toml

1. On each device, open the IoT Edge configuration file.

   ```bash
   sudo nano /etc/aziot/config.toml
   ```

1. Suchen Sie für Ihr Gerät der **obersten Ebene** nach dem Abschnitt **Hostname**. Heben Sie die Auskommentierung der Zeile mit dem `hostname`-Parameter auf, und aktualisieren Sie den Wert auf den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) oder die IP-Adresse des IoT Edge-Geräts. Verwenden Sie den von Ihnen gewählten Wert einheitlich auf allen Geräten Ihrer Hierarchie.

   ```toml
   hostname = "<device fqdn or IP>"
   ```

   >[!TIP]
   >Verwenden Sie zum Einfügen des Inhalts der Zwischenablage in Nano `Shift+Right Click`, oder drücken Sie `Shift+Insert`.

1. Suchen Sie für ein beliebiges IoT Edge-Gerät in den **unteren Ebenen** den Abschnitt **Parent hostname** (Hostname des übergeordneten Geräts). Heben Sie die Auskommentierung der Zeile mit dem `parent_hostname`-Parameter auf, und aktualisieren Sie den Wert so, dass er auf den FQDN oder die IP-Adresse des übergeordneten Geräts verweist. Verwenden Sie den exakten Wert, den Sie in das Feld **Hostname** des übergeordneten Geräts eingegeben haben. Lassen Sie diesen Parameter für das IoT Edge-Gerät der **obersten Ebene** auskommentiert.

   ```toml
   parent_hostname = "<parent device fqdn or IP>"
   ```

   > [!NOTE]
   > Aktualisieren Sie bei Hierarchien mit mehr als einer untergeordneten Ebene das Feld *parent_hostname* mit dem FQDN des Geräts auf der direkt übergeordneten Ebene.

1. Suchen Sie den Abschnitt **Trust bundle cert** (Vertrauensstellungszertifikatbündel) der Datei. Heben Sie die Auskommentierung der Zeile mit dem `trust_bundle_cert`-Parameter auf, und aktualisieren Sie den Wert mit dem Datei-URI-Pfad auf das Zertifikat der Stammzertifizierungsstelle, das von allen Geräten in der Gatewayhierarchie verwendet wird.

   ```toml
   trust_bundle_cert = "<root CA certificate>"
   ```

1. Suchen Sie den Abschnitt **Provisioning** (Bereitstellung) der Datei. Heben Sie die Auskommentierung der Zeilen unter **Manual provisioning with connection string** (Manuelle Bereitstellung mit Verbindungszeichenfolge) auf. Aktualisieren Sie für jedes Gerät in Ihrer Hierarchie den Wert von **connection_string** mit der Verbindungszeichenfolge Ihres IoT Edge-Geräts.

   ```toml
   # Manual provisioning with connection string
   [provisioning]
   source = "manual"
   connection_string: "<Device connection string>"
   ```

1. Suchen Sie den Abschnitt **Default Edge Agent** (Standard-Edge-Agent).

   * Aktualisieren Sie den Wert des Images „edgeAgent“ für Ihr Gerät der **obersten Ebene** auf die Public Preview-Version des Moduls in Azure Container Registry.
   
     ```toml
     [agent.config]
     image = "mcr.microsoft.com/azureiotedge-agent:1.2.0-rc4"
     ```

   * Aktualisieren Sie das Image „edgeAgent“ für jedes IoT Edge-Gerät in den **unteren Ebenen** so, dass es auf das übergeordnete Gerät verweist, gefolgt von dem Port, an dem der API-Proxy lauscht. Im nächsten Abschnitt stellen Sie das API-Proxymodul auf dem übergeordneten Gerät bereit.
   
     ```toml
     [agent.config]
     image = "<parent hostname value>:8000/azureiotedge-agent:1.2.0-rc4"
     ```

1. Suchen Sie den Abschnitt **Edge CA certificate** (Edgezertifizierungsstellenzertifikat). Heben Sie die Auskommentierung der ersten drei Zeilen dieses Abschnitts auf. Aktualisieren Sie dann die beiden Parameter so, dass sie auf das Zertifikat und die privaten Schlüsseldateien der Gerätezertifizierungsstelle verweisen, die Sie im vorherigen Abschnitt erstellt und auf das IoT Edge-Gerät verschoben haben. Geben Sie die Datei-URI-Pfade im Format `file:///<path>/<filename>` an, z. B. `file:///certs/iot-edge-device-ca-top-layer-device.key.pem`.

   ```yml
   [edge_ca]
   cert = "<File URI path to the device full chain CA certificate unique to this device.>"
   pk = "<File URI path to the device CA private key unique to this device.>"
   ```

   >[!NOTE]
   >Achten Sie darauf, im Feld `device_ca_cert` den Pfad und den Dateinamen der **vollständigen Kette** des Zertifizierungsstellenzertifikats anzugeben.

1. Speichern und schließen Sie die Datei.

   `CTRL + X`, `Y`, `Enter`

1. Übernehmen Sie die Änderungen, nachdem Sie die Bereitstellungsinformationen in der Konfigurationsdatei angegeben haben:

   ```bash
   sudo iotedge config apply
   ```

Bevor Sie fortfahren, sollten Sie sicherstellen, dass Sie die Konfigurationsdateien aller Geräte in der Hierarchie aktualisiert haben. Je nach der Struktur Ihrer Hierarchie haben Sie ein **Gerät der obersten Ebene** und mindestens ein **Gerät der unteren Ebene** konfiguriert.

Wenn Sie die obigen Schritte richtig ausgeführt haben, können Sie überprüfen, ob Ihre Geräte ordnungsgemäß konfiguriert sind.

1. Führen Sie die Konfigurations- und Konnektivitätsprüfungen auf Ihren Geräten aus:

   ```bash
   sudo iotedge check
   ```

Auf Ihrem **Gerät der obersten Ebene** sollte eine Ausgabe mit mehreren bestandenen Auswertungen und mindestens einer Warnung angezeigt werden. Die Überprüfung auf den neuesten Sicherheitsdaemon (`latest security daemon`) warnt Sie, dass eine andere IoT Edge-Version die neueste stabile Version ist, da sich IoT Edge-Version 1.2 in der Public Preview befindet. Möglicherweise werden weitere Warnungen zu Protokollrichtlinien und, in Abhängigkeit von Ihrem Netzwerk, DNS-Richtlinien angezeigt.

Auf einem **Gerät der unteren Ebene** sollte eine Ausgabe angezeigt werden, die der auf dem Gerät der obersten Ebene ähnelt. Sie enthält jedoch eine zusätzliche Warnung, dass das Modul „EdgeAgent“ nicht aus der oberen Ebene gepullt werden kann. Dies ist hinnehmbar, da das IoT Edge-API-Proxymodul und das Modul von Docker Container Registry, über das Geräte der unteren Ebene Images pullen, noch nicht auf dem **Gerät der obersten Ebene** bereitgestellt wurden.

Nachfolgend sehen Sie eine Beispielausgabe von `iotedge check`:

[Beispielkonfiguration und Konnektivitätsergebnisse](./media/tutorial-nested-iot-edge/configuration-and-connectivity-check-results.png)

Wenn Sie überzeugt sind, dass die Konfigurationen auf den einzelnen Geräten richtig sind, können Sie fortfahren.

## <a name="deploy-modules-to-the-top-layer-device"></a>Bereitstellen von Modulen auf dem Gerät der obersten Ebene

Module dienen dazu, die Bereitstellung und die IoT Edge-Runtime auf Ihren Geräten abzuschließen und die Struktur Ihrer Hierarchie genauer zu definieren. Das IoT Edge-API-Proxymodul leitet HTTP-Datenverkehr von Ihren Geräten der unteren Ebenen sicher über einen einzelnen Port weiter. Das Modul von Docker Registry bietet ein Repository mit Docker-Images, auf die Ihre Geräte der unteren Ebene zugreifen können, indem Imagepulls an das Gerät der obersten Ebene weitergeleitet werden.

Zum Bereitstellen von Modulen auf dem Gerät der obersten Ebene können Sie das Azure-Portal oder die Azure CLI verwenden.

>[!NOTE]
>Die übrigen Schritte zum Abschließen der Konfiguration der IoT Edge-Runtime und Bereitstellen von Workloads erfolgen nicht auf Ihren IoT Edge-Geräten.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Im [Azure-Portal](https://ms.portal.azure.com/):

1. Navigieren Sie zu Ihrer IoT Hub-Instanz.

1. Wählen Sie im Menü im linken Bereich unter **Automatische Geräteverwaltung** die Option **IoT Edge** aus.

1. Klicken Sie in der Liste mit den Geräten auf die Geräte-ID Ihres Edgegeräts der **obersten Ebene**.

1. Wählen Sie in der oberen Leiste **Module festlegen** aus.

1. Wählen Sie neben dem Zahnradsymbol die Option **Runtimeeinstellungen** aus.

1. Geben Sie unter **Edge Hub** im Imagefeld die Zeichenfolge `mcr.microsoft.com/azureiotedge-hub:1.2.0-rc4` ein.

   ![Bearbeiten des Edge-Hub-Images](./media/tutorial-nested-iot-edge/edge-hub-image.png)

1. Fügen Sie Ihrem Edge-Hubmodul die folgenden Umgebungsvariablen hinzu:

    | Name | Wert |
    | - | - |
    | `experimentalFeatures__enabled` | `true` |
    | `experimentalFeatures__nestedEdgeEnabled` | `true` |

   ![Bearbeiten der Umgebungsvariablen für den Edge-Hub](./media/tutorial-nested-iot-edge/edge-hub-environment-variables.png)

1. Geben Sie unter **Edge-Agent** im Imagefeld die Zeichenfolge `mcr.microsoft.com/azureiotedge-agent:1.2.0-rc4` ein. Wählen Sie **Speichern** aus.

1. Fügen Sie das Docker-Registrierungsmodul Ihrem Gerät der obersten Ebene hinzu. Wählen Sie **+ Hinzufügen** und in der Dropdownliste die Option **IoT Edge-Modul** aus. Geben Sie den Namen `registry` für Ihr Docker-Registrierungsmodul und `registry:latest` für den Image-URI ein. Fügen Sie als Nächstes Umgebungsvariablen hinzu, und erstellen Sie Optionen, um für Ihr lokales Registrierungsmodul auf die Microsoft-Containerregistrierung zu verweisen, damit hierüber Containerimages heruntergeladen und unter registry:5000 bereitgestellt werden können.

1. Geben Sie unter der Registerkarte mit den Umgebungsvariablen das folgende Name/Wert-Paar als Umgebungsvariable ein:

    | Name | Wert |
    | - | - |
    | `REGISTRY_PROXY_REMOTEURL` | `https://mcr.microsoft.com` |

1. Geben Sie auf der Registerkarte mit den Optionen für die Containererstellung den folgenden JSON-Code ein:

   ```json
   {
    "HostConfig": {
        "PortBindings": {
            "5000/tcp": [
                {
                    "HostPort": "5000"
                }
            ]
         }
      }
   }
   ```

1. Fügen Sie als Nächstes das API-Proxymodul Ihrem Gerät der obersten Ebene hinzu. Wählen Sie **+ Hinzufügen** und in der Dropdownliste die Option **Marketplace-Modul** aus. Suchen Sie nach `IoT Edge API Proxy`, und wählen Sie das Modul aus. Vom IoT Edge-API-Proxy wird Port 8000 verwendet, und er ist standardmäßig für die Nutzung Ihres Registrierungsmoduls mit dem Namen `registry` an Port 5000 konfiguriert.

1. Wählen Sie die Option **Überprüfen und erstellen** und dann **Erstellen** aus, um die Bereitstellung durchzuführen. Von der IoT Edge-Runtime Ihres Geräts der obersten Ebene, die über Internetzugriff verfügt, werden die **öffentlichen Vorschauversionen** der Konfigurationen für IoT Edge-Hub und den IoT Edge-Agent gepullt und ausgeführt.

   ![Vollständige Bereitstellung mit Edge-Hub, Edge-Agent, Registrierungsmodul und API-Proxymodul](./media/tutorial-nested-iot-edge/complete-top-layer-deployment.png)

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

1. Geben Sie in [Azure Cloud Shell](https://shell.azure.com/) den folgenden Befehl ein, um die Datei „deployment.json“ zu erstellen:

   ```azurecli-interactive
   code deploymentTopLayer.json
   ```

1. Kopieren Sie den unten angegebenen JSON-Code in die Datei „deployment.json“, und speichern und schließen Sie sie.

   ```json
   {
       "modulesContent": {
           "$edgeAgent": {
               "properties.desired": {
                   "modules": {
                       "registry": {
                           "settings": {
                               "image": "registry:latest",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5000/tcp\":[{\"HostPort\":\"5000\"}]}}}"
                           },
                           "type": "docker",
                           "version": "1.0",
                           "env": {
                               "REGISTRY_PROXY_REMOTEURL": {
                                   "value": "https://mcr.microsoft.com"
                               } 
                           },
                           "status": "running",
                           "restartPolicy": "always"
                       },
                       "IoTEdgeAPIProxy": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-api-proxy",
                               "createOptions": "{\"HostConfig\": {\"PortBindings\": {\"8000/tcp\": [{\"HostPort\":\"8000\"}]}}}"
                           },
                           "type": "docker",
                           "env": {
                               "NGINX_DEFAULT_PORT": {
                                   "value": "8000"
                               },
                               "DOCKER_REQUEST_ROUTE_ADDRESS": {
                                   "value": "registry:5000"
                               },
                               "BLOB_UPLOAD_ROUTE_ADDRESS": {
                                   "value": "AzureBlobStorageonIoTEdge:11002"
                               }
                           },
                           "status": "running",
                           "restartPolicy": "always",
                           "version": "1.0"
                       }
                   },
                   "runtime": {
                       "settings": {
                           "minDockerVersion": "v1.25"
                       },
                       "type": "docker"
                   },
                   "schemaVersion": "1.1",
                   "systemModules": {
                       "edgeAgent": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-agent:1.2.0-rc4",
                               "createOptions": ""
                           },
                           "type": "docker"
                       },
                       "edgeHub": {
                           "settings": {
                               "image": "mcr.microsoft.com/azureiotedge-hub:1.2.0-rc4",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}]}}}"
                           },
                           "type": "docker",
                           "env": {
                               "experimentalFeatures__enabled": {
                                   "value": "true"
                               },
                               "experimentalFeatures__nestedEdgeEnabled": {
                                   "value": "true"
                               }
                           },
                           "status": "running",
                           "restartPolicy": "always"
                       }
                   }
               }
           },
           "$edgeHub": {
               "properties.desired": {
                   "routes": {
                       "route": "FROM /messages/* INTO $upstream"
                   },
                   "schemaVersion": "1.1",
                   "storeAndForwardConfiguration": {
                       "timeToLiveSecs": 7200
                   }
               }
           }
       }
   }
   ```

1. Geben Sie den folgenden Befehl ein, um eine Bereitstellung auf Ihrem Edgegerät der obersten Ebene zu erstellen:

   ```azurecli-interactive
   az iot edge set-modules --device-id <top_layer_device_id> --hub-name <iot_hub_name> --content ./deploymentTopLayer.json
   ```

---

Wenn Sie die oben genannten Schritte richtig durchgeführt haben, sollte Ihr **Gerät der obersten Ebene** die folgenden vier Module melden: das IoT Edge-API-Proxymodul, das Docker Container Registry-Modul und die Systemmodule, wie **in der Bereitstellung angegeben**. Es kann einige Minuten dauern, bis das Gerät seine neue Bereitstellung erhält und die Module startet. Aktualisieren Sie die Seite, bis das Temperatursensormodul als **Vom Gerät gemeldet** angezeigt wird. Nachdem die Module vom Gerät gemeldet wurden, können Sie fortfahren.

## <a name="deploy-modules-to-the-lower-layer-device"></a>Bereitstellen von Modulen auf dem Gerät der untergeordneten Ebene

Module dienen außerdem als Workloads Ihrer Geräte der unteren Ebene. Das Modul „Simulated Temperature Sensor“ (Simulierter Temperatursensor) erstellt Beispieltelemetriedaten, um einen funktionalen Datenfluss in Ihrer Gerätehierarchie bereitzustellen.

Für die Bereitstellung von Modulen auf Ihren Geräten der unteren Ebene können Sie das Azure-Portal oder die Azure CLI verwenden.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Im [Azure-Portal](https://ms.portal.azure.com/):

1. Navigieren Sie zu Ihrer IoT Hub-Instanz.

1. Wählen Sie im Menü im linken Bereich unter **Automatische Geräteverwaltung** die Option **IoT Edge** aus.

1. Klicken Sie in der Liste mit den IoT Edge-Geräten auf die Geräte-ID Ihres Geräts auf der untergeordneten Ebene.

1. Wählen Sie in der oberen Leiste **Module festlegen** aus.

1. Wählen Sie neben dem Zahnradsymbol die Option **Runtimeeinstellungen** aus.

1. Geben Sie unter **Edge Hub** im Imagefeld die Zeichenfolge `$upstream:8000/azureiotedge-hub:1.2.0-rc4` ein.

1. Fügen Sie Ihrem Edge-Hubmodul die folgenden Umgebungsvariablen hinzu:

    | Name | Wert |
    | - | - |
    | `experimentalFeatures__enabled` | `true` |
    | `experimentalFeatures__nestedEdgeEnabled` | `true` |

1. Geben Sie unter **Edge-Agent** im Imagefeld die Zeichenfolge `$upstream:8000/azureiotedge-agent:1.2.0-rc4` ein. Wählen Sie **Speichern** aus.

1. Fügen Sie das Temperatursensormodul hinzu. Wählen Sie **+ Hinzufügen** und in der Dropdownliste die Option **Marketplace-Modul** aus. Suchen Sie nach `Simulated Temperature Sensor`, und wählen Sie das Modul aus.

1. Wählen Sie unter **IoT Edge-Module** das eben hinzugefügte Modul `Simulated Temperature Sensor` aus, und aktualisieren Sie den Image-URI so, dass er auf `$upstream:8000/azureiotedge-simulated-temperature-sensor:1.0` verweist.

1. Wählen Sie **Speichern**, **Überprüfen und erstellen** und **Erstellen** aus, um die Bereitstellung durchzuführen.

   ![Vollständige Bereitstellung mit Edge-Hub, Edge-Agent und Simulated Temperature Sensor](./media/tutorial-nested-iot-edge/complete-lower-layer-deployment.png)

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

1. Geben Sie in [Azure Cloud Shell](https://shell.azure.com/) den folgenden Befehl ein, um die Datei „deployment.json“ zu erstellen:

   ```azurecli-interactive
   code deploymentLowerLayer.json
   ```

1. Kopieren Sie den unten angegebenen JSON-Code in die Datei „deployment.json“, und speichern und schließen Sie sie.

   ```json
   {
       "modulesContent": {
           "$edgeAgent": {
               "properties.desired": {
                   "modules": {
                       "simulatedTemperatureSensor": {
                           "settings": {
                               "image": "$upstream:8000/azureiotedge-simulated-temperature-sensor:1.0",
                               "createOptions": ""
                           },
                           "type": "docker",
                           "status": "running",
                           "restartPolicy": "always",
                           "version": "1.0"
                       }
                   },
                   "runtime": {
                       "settings": {
                           "minDockerVersion": "v1.25"
                       },
                       "type": "docker"
                   },
                   "schemaVersion": "1.1",
                   "systemModules": {
                       "edgeAgent": {
                           "settings": {
                               "image": "$upstream:8000/azureiotedge-agent:1.2.0-rc4",
                               "createOptions": ""
                           },
                           "type": "docker"
                       },
                       "edgeHub": {
                           "settings": {
                               "image": "$upstream:8000/azureiotedge-hub:1.2.0-rc4",
                               "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"443/tcp\":[{\"HostPort\":\"443\"}],\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}]}}}"
                           },
                           "type": "docker",
                           "env": {
                               "experimentalFeatures__enabled": {
                                   "value": "true"
                               },
                               "experimentalFeatures__nestedEdgeEnabled": {
                                   "value": "true"
                               }
                           },
                           "status": "running",
                           "restartPolicy": "always"
                       }
                   }
               }
           },
           "$edgeHub": {
               "properties.desired": {
                   "routes": {
                       "route": "FROM /messages/* INTO $upstream"
                   },
                   "schemaVersion": "1.1",
                   "storeAndForwardConfiguration": {
                       "timeToLiveSecs": 7200
                   }
               }
           }
       }
   }
   ```

1. Geben Sie den folgenden Befehl ein, um eine „set-modules“-Bereitstellung auf Ihrem Edgegerät der untergeordneten Ebene zu erstellen:

   ```azurecli-interactive
   az iot edge set-modules --device-id <lower_layer_device_id> --hub-name <iot_hub_name> --content ./deploymentLowerLayer.json

---

Notice that the image URI that we used for the simulated temperature sensor module pointed to `$upstream:8000` instead of to a container registry. We configured this device to not have direct connections to the cloud, because it's in a lower layer. To pull container images, this device requests the image from its parent device instead. At the top layer, the API proxy module routes this container request to the registry module, which handles the image pull.

If you completed the above steps correctly, your **lower layer device** should report three modules: the temperature sensor module and the system modules, as **Specified in Deployment**. It may take a few minutes for the device to receive its new deployment, request the container image, and start the module. Refresh the page until you see the temperature sensor module listed as **Reported by Device**. Once the modules are reported by the device, you are ready to continue.

## Troubleshooting

Run the `iotedge check` command to verify the configuration and to troubleshoot errors.

You can run `iotedge check` in a nested hierarchy, even if the child machines don't have direct internet access.

When you run `iotedge check` from the lower layer, the program tries to pull the image from the parent through port 443.

In this tutorial, we use port 8000, so we need to specify it:

```bash
sudo iotedge check --diagnostics-image-name <parent_device_fqdn_or_ip>:8000/azureiotedge-diagnostics:1.2.0-rc4
```

Der Wert `azureiotedge-diagnostics` wird aus der Containerregistrierung gepullt, die mit dem Registrierungsmodul verknüpft ist. In diesem Tutorial ist er standardmäßig auf https://mcr.microsoft.com: festgelegt.

| Name | Wert |
| - | - |
| `REGISTRY_PROXY_REMOTEURL` | `https://mcr.microsoft.com` |

Stellen Sie bei Verwendung einer privaten Containerregistrierung sicher, dass alle Images (etwa IoTEdgeAPIProxy, edgeAgent, edgeHub und diagnostics) in der Containerregistrierung vorhanden sind.

## <a name="view-generated-data"></a>Anzeigen generierter Daten

Mit dem von Ihnen gepushten **simulierten Temperatursensormodul** werden Beispielumgebungsdaten generiert. Es sendet Nachrichten mit Umgebungstemperatur und Luftfeuchtigkeit, Computertemperatur und Druck sowie dem Zeitstempel.

Sie können die bei Ihrem IoT-Hub eingehenden Nachrichten mit der [Azure IoT Hub-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) anzeigen.

Sie können diese Nachrichten auch über [Azure Cloud Shell](https://shell.azure.com/) anzeigen:

   ```azurecli-interactive
   az iot hub monitor-events -n <iothub_name> -d <lower-layer-device-name>
   ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die in diesem Artikel erstellten lokalen Konfigurationen und Azure-Ressourcen löschen, um Kosten zu vermeiden.

So löschen Sie die Ressourcen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie auf **Ressourcengruppen**.

2. Wählen Sie den Namen der Ressourcengruppe aus, die Ihre IoT Edge-Testressourcen enthält. 

3. Überprüfen Sie die Liste der Ressourcen in Ihrer Ressourcengruppe. Wenn Sie alle löschen möchten, klicken Sie auf **Ressourcengruppe löschen**. Wenn Sie nur einige Ressourcen löschen möchten, klicken Sie auf die jeweiligen Ressourcen, um sie einzeln zu löschen. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie zwei IoT Edge-Geräte als Gateways konfiguriert und eins als übergeordnetes Gerät des anderen festgelegt. Anschließend haben Sie gezeigt, wie Sie ein Containerimage über ein Gateway auf das untergeordnete Gerät pullen.

Sie können nun die anderen Tutorials bearbeiten, um noch mehr darüber zu erfahren, wie sich mit Azure IoT Edge weitere Lösungen für Ihr Unternehmen erstellen lassen.

> [!div class="nextstepaction"]
> [Bereitstellen eines Azure Machine Learning-Modells als Modul](tutorial-deploy-machine-learning.md)
