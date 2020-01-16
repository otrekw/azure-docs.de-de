---
title: Tutorial – Verwenden von Azure IoT Hub-Nachrichtenanreicherungen
description: Ein Tutorial, in dem gezeigt wird, wie Nachrichtenanreicherungen für Azure IoT Hub-Nachrichten verwendet werden
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 12/20/2019
ms.author: robinsh
ms.openlocfilehash: 323730fff4659c87058669016b69808a880994cf
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453894"
---
# <a name="tutorial-using-azure-iot-hub-message-enrichments"></a>Tutorial: Verwenden von Azure IoT Hub-Nachrichtenanreicherungen

*Nachrichtenanreicherungen* ist die Fähigkeit des IoT-Hubs, Nachrichten mit zusätzlichen Informationen zu *stempeln*, bevor sie an den angegebenen Endpunkt gesendet werden. Ein Grund für die Verwendung von Nachrichtenanreicherungen: Auf diese Weise lassen sich Daten einbeziehen, die zur Vereinfachung der Downstreamverarbeitung verwendet werden können. So kann beispielsweise durch die Anreicherung von Gerätetelemetrienachrichten mit einem Gerätezwillingstag die Last bei Kunden reduziert werden, um Gerätezwillings-API-Aufrufe für diese Informationen durchzuführen. Weitere Informationen finden Sie in der [Übersicht über Nachrichtenanreicherungen](iot-hub-message-enrichments-overview.md).

In diesem Tutorial lernen Sie zwei Möglichkeiten zum Erstellen und Konfigurieren der Ressourcen kennen, die zum Testen der Nachrichtenanreicherungen für einen IoT Hub benötigt werden. Die Ressourcen umfassen ein Speicherkonto mit zwei Speichercontainern – einen zum Speichern der angereicherten Nachrichten und einen zum Speichern der ursprünglichen Nachrichten. Außerdem gehört dazu ein IoT Hub, der die Nachrichten empfängt und an den entsprechenden Speichercontainer weiterleitet – je nachdem, ob sie angereichert sind oder nicht. 

* Bei der ersten Methode verwenden Sie die Azure CLI (Azure-Befehlszeilenschnittstelle) zum Erstellen von Ressourcen und Konfigurieren des Nachrichtenroutings. Anschließend definieren Sie die Anreicherungen manuell über das [Azure-Portal](https://portal.azure.com). 

* Bei der zweiten Methode verwenden Sie eine Azure Resource Manager-Vorlage zum Erstellen der Ressourcen *und* der Konfigurationen für das Nachrichtenrouting und die Nachrichtenanreicherungen. 

Nach Abschluss der Konfigurationen für das Nachrichtenrouting und die Nachrichtenanreicherungen verwenden Sie eine Anwendung zum Senden von Nachrichten an den IoT Hub, der sie dann an beide Speichercontainer weiterleitet. Nur die an den Endpunkt für den Speichercontainer **enriched** gesendeten Nachrichten werden angereichert.

Hier sind die Aufgaben, die Sie beim Durcharbeiten dieses Tutorials ausführen:

**Verwenden von IoT Hub-Nachrichtenanreicherungen**
> [!div class="checklist"]
> * Erste Methode: Manuelle Nachrichtenanreicherungen
>   - Erstellen Sie Ressourcen, und konfigurieren Sie das Nachrichtenrouting über die Azure CLI.
>   - Konfigurieren Sie die Nachrichtenanreicherungen manuell über das [Azure-Portal](https://portal.azure.com).
> * Zweite Methode: Verwendung einer RM-Vorlage
>   - Erstellen Sie Ressourcen, und konfigurieren Sie Nachrichtenrouting und Nachrichtenanreicherungen mithilfe einer Azure Resource Manager-Vorlage. 
> * Führen Sie eine App aus, die ein IoT-Gerät simuliert, das Nachrichten an den Hub sendet.
> * Zeigen Sie die Ergebnisse an, und überprüfen Sie, ob die Nachrichtenanreicherungen wie erwartet funktionieren.

## <a name="prerequisites"></a>Voraussetzungen

* Sie benötigen ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

* Installieren Sie [Visual Studio](https://www.visualstudio.com/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="retrieve-the-iot-c-samples-repository"></a>Abrufen des Repositorys „IoT C#-Beispiele“

Laden Sie die [IoT C#-Beispiele](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) von GitHub herunter, und entzippen Sie sie. Dieses Repository enthält mehrere Anwendungen, Skripts und Resource Manager-Vorlagen. Für dieses Tutorial müssen folgende Vorlagen verwendet werden:

* Für die manuelle Methode gibt es ein CLI-Skript zum Erstellen der Ressourcen. Dieses Skript ist in **/azure-iot-samples-csharp/iot-hub/Tutorials/Routing/SimulatedDevice/resources/iothub_msgenrichment_cli.azcli** gespeichert. Mit diesem Skript werden die Ressourcen erstellt und wird das Nachrichtenrouting konfiguriert. Nachdem Sie es ausgeführt haben, erstellen Sie die Nachrichtenanreicherungen manuell über das [Azure-Portal](https://portal.azure.com) und führen dann die Gerätesimulations-App aus, um zu sehen, ob die Anreicherungen funktionieren.

* Für die automatisierte Methode gibt es eine Azure Resource Manager-Vorlage. Diese Vorlage ist in **/azure-iot-samples-csharp/iot-hub/Tutorials/Routing/SimulatedDevice/resources/template_msgenrichments.json** gespeichert. Mit dieser Vorlage werden die Ressourcen erstellt, wird das Nachrichtenrouting konfiguriert und werden schließlich die Nachrichtenanreicherungen konfiguriert. Nachdem Sie diese Vorlage geladen haben, führen Sie die Gerätesimulations-App aus, um zu sehen, ob die Anreicherungen funktionieren.

* Die dritte verwendete Anwendung ist die Gerätesimulations-App, die Sie zum Senden von Nachrichten an den IoT Hub und zum Testen der Nachrichtenanreicherungen verwenden.

## <a name="manual-set-up-and-configuration-using-azure-cli"></a>Manuelles Einrichten und Konfiguration über die Azure CLI

Zusätzlich zum Erstellen der erforderlichen Ressourcen konfiguriert das Azure CLI-Skript auch die beiden Routen zu den Endpunkten, die separate Speichercontainer sind. Weitere Informationen zum Konfigurieren des Nachrichtenroutings finden Sie im [Routing-Tutorial](tutorial-routing.md). Nachdem die Ressourcen eingerichtet wurden, konfigurieren Sie über das [Azure-Portal](https://portal.azure.com) Nachrichtenanreicherungen für jeden Endpunkt und gehen dann über zum Schritt „Testen“.

> [!NOTE]
> Alle Nachrichten werden an beide Endpunkte weitergeleitet, aber nur die Nachrichten, deren Ziel der Endpunkt mit konfigurierten Nachrichtenanreicherungen ist, werden angereichert.
>

Sie können das nachstehende Skript verwenden oder das Skript im Ordner „/resources“ des heruntergeladenen Repositorys öffnen. Hier sind die Schritte, die das Skript ausführen wird:

* Erstellen Sie einen IoT Hub.
* Erstellen Sie ein Speicherkonto.
* Zwei Container im Speicherkonto erstellen – einen für die angereicherten Nachrichten und einen für Nachrichten, die nicht angereichert sind.
* Das Routing für die beiden verschiedenen Speicherkonten einrichten.
    * Einen Endpunkt für jeden Container des Speicherkontos erstellen.
    * Eine Route zu jedem Containerendpunkt des Speicherkontos erstellen.

Einige Ressourcennamen müssen global eindeutig sein. Hierzu zählen beispielsweise der IoT Hub-Name und der Name des Speicherkontos. Um die Ausführung des Skripts zu vereinfachen, wird an diese Ressourcennamen der alphanumerische Zufallswert *randomValue* angefügt. Der Zufallswert wird einmalig zu Beginn des Skripts generiert und innerhalb des gesamten Skripts nach Bedarf an die Ressourcennamen angefügt. Falls Sie keinen Zufallswert verwenden möchten, können Sie den Wert auf eine leere Zeichenfolge oder auf einen bestimmten Wert festlegen.

Falls Sie dies noch nicht getan haben, öffnen Sie ein [Cloud Shell-Fenster](https://shell.azure.com), und sorgen Sie dafür, dass es auf „Bash“ festgelegt ist. Öffnen Sie das Skript im entpackten Repository, wählen Sie es mit STRG+A vollständig aus, und kopieren Sie es mit STRG+C. Alternativ können Sie das folgende CLI-Skript kopieren oder es direkt in Cloud Shell öffnen. Fügen Sie das Skript im Cloud Shell-Fenster ein, indem Sie mit der rechten Maustaste auf die Befehlszeile klicken und **Einfügen** auswählen. Das Skript führt jeweils eine Anweisung aus. Nachdem das Skript die Ausführung beendet hat, drücken Sie die**EINGABETASTE**, um sicherzustellen, dass der letzte Befehl ausgeführt wird. Der nachstehende Codeblock zeigt das verwendete Skript und Kommentare mit einer Erläuterung der jeweiligen Funktion.

Hier sind die vom Skript erstellten Ressourcen. **enriched** bedeutet, dass die Ressource für Nachrichten mit Anreicherungen vorgesehen ist. **original** bedeutet, dass die Ressource für nicht angereicherte Nachrichten vorgesehen ist.

| Name | value |
|-----|-----|
| resourceGroup | ContosoResourcesMsgEn |
| Containername | original  |
| Containername | enriched  |
| IoT-Gerätename | Contoso-Test-Device |
| IoT Hub-Name | ContosoTestHubMsgEn |
| Speicherkontoname | contosostorage |
| Endpunktname 1 | ContosoStorageEndpointOriginal |
| Endpunktname 2 | ContosoStorageEndpointEnriched|
| Routenname 1 | ContosoStorageRouteOriginal |
| Routenname 2 | ContosoStorageRouteEnriched |

```azurecli-interactive
# This command retrieves the subscription id of the current Azure account.
# This field is used when setting up the routing queries.
subscriptionID=$(az account show --query id -o tsv)

# Concatenate this number onto the resources that have to be globally unique.
# You can set this to "" or to a specific value if you don't want it to be random.
# This retrieves a random value.
randomValue=$RANDOM

# This command installs the IOT Extension for Azure CLI.
# You only need to install this the first time.
# You need it to create the device identity.
az extension add --name azure-cli-iot-ext

# Set the values for the resource names that
#   don't have to be globally unique.
location=westus2
resourceGroup=ContosoResourcesMsgEn
containerName1=original
containerName2=enriched
iotDeviceName=Contoso-Test-Device

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique,
#   so add a random value to the end.
iotHubName=ContosoTestHubMsgEn$randomValue
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# You need a storage account that will have two containers
#   -- one for the original messages and
#   one for the enriched messages.
# The storage account name must be globally unique,
#   so add a random value to the end.
storageAccountName=contosostorage$randomValue
echo "Storage account name = " $storageAccountName

# Create the storage account to be used as a routing destination.
az storage account create --name $storageAccountName \
    --resource-group $resourceGroup \
    --location $location \
    --sku Standard_LRS

# Get the primary storage account key.
#    You need this to create the containers.
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroup \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')

# See the value of the storage account key.
echo "storage account key = " $storageAccountKey

# Create the containers in the storage account.
az storage container create --name $containerName1 \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

az storage container create --name $containerName2 \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
# If you are using Cloud Shell, you can scroll the window back up to retrieve this value.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName

##### ROUTING FOR STORAGE #####

# You're going to have two routes and two endpoints.
# One route points to the first container ("original") in the storage account
#   and includes the original messages.
# The other points to the second container ("enriched") in the same storage account
#   and includes the enriched versions of the messages.

endpointType="azurestoragecontainer"
endpointName1="ContosoStorageEndpointOriginal"
endpointName2="ContosoStorageEndpointEnriched"
routeName1="ContosoStorageRouteOriginal"
routeName2="ContosoStorageRouteEnriched"

# for both endpoints, retrieve the messages going to storage
condition='level="storage"'

# Get the connection string for the storage account.
# Adding the "-o tsv" makes it be returned without the default double quotes around it.
storageConnectionString=$(az storage account show-connection-string \
  --name $storageAccountName --query connectionString -o tsv)

# Create the routing endpoints and routes.
# Set the encoding format to either avro or json.

# This is the endpoint for the first container, for endpoint messages that are not enriched.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName1 \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName1 \
  --resource-group $resourceGroup \
  --encoding json

# This is the endpoint for the second container, for endpoint messages that are enriched.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName2 \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName2 \
  --resource-group $resourceGroup \
  --encoding json

# This is the route for messages that are not enriched.
# Create the route for the first storage endpoint.
az iot hub route create \
  --name $routeName1 \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName1 \
  --enabled \
  --condition $condition

# This is the route for messages that are enriched.
# Create the route for the second storage endpoint.
az iot hub route create \
  --name $routeName2 \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName2 \
  --enabled \
  --condition $condition
```

Jetzt sind alle Ressourcen eingerichtet, und das Nachrichtenrouting ist konfiguriert. Sie können die Konfiguration des Nachrichtenroutings im Portal anzeigen und die Nachrichtenanreicherungen für Nachrichten einrichten, die an den Speichercontainer **enriched** gesendet werden.

### <a name="manually-configure-the-message-enrichments-using-the-azure-portal"></a>Manuelles Konfigurieren der Nachrichtenanreicherungen über das Azure-Portal

1. Wechseln Sie zu Ihrem IoT-Hub, indem Sie **Ressourcengruppen** und dann die für dieses Tutorial eingerichtete Ressourcengruppe, (**ContosoResourcesMsgEn**), auswählen. Suchen Sie den IoT-Hub in der Liste, und wählen Sie ihn aus. Wählen Sie **Nachrichtenrouting** für den Iot-Hub aus.

   ![Auswählen von „Nachrichtenrouting“](./media/tutorial-message-enrichments/select-iot-hub.png)

   Im Bereich „Nachrichtenrouting“ gibt es drei Registerkarten: **Routen**, **Benutzerdefinierte Endpunkte** und **Enrich messages** (Nachrichten anreichern). Sie können die ersten beiden Registerkarten durchsuchen, um die vom Skript eingerichtete Konfiguration anzuzeigen. Auf der dritten Registerkarte können Sie Nachrichtenanreicherungen hinzufügen. Reichern Sie jetzt Nachrichten an, indem Sie zum Endpunkt für den Speichercontainer **enriched** wechseln. Geben Sie den Namen und Wert ein, und wählen Sie den Endpunkt **ContosoStorageEndpointEnriched** aus der Dropdownliste aus. Hier ist ein Beispiel für das Einrichten einer Anreicherung, die den Namen des IoT-Hubs zur Nachricht hinzufügt:

   ![Hinzufügen der ersten Anreicherung](./media/tutorial-message-enrichments/add-message-enrichments.png)

2. Fügen Sie diese Werte zur Liste für den Endpunkt „ContosoStorageEndpointEnriched“ hinzu.

   | Key | value | Endpunkt (Dropdownliste) |
   | ---- | ----- | -------------------------|
   | myIotHub | $iothubname | AzureStorageContainers > ContosoStorageEndpointEnriched |
   | DeviceLocation | $twin.tags.location | AzureStorageContainers > ContosoStorageEndpointEnriched |
   |customerID | 6ce345b8-1e4a-411e-9398-d34587459a3a | AzureStorageContainers > ContosoStorageEndpointEnriched |

   > [!NOTE]
   > Falls Ihr Gerät keinen Zwilling hat, wird der hier eingegebene Wert als Zeichenfolge für den Wert in den Nachrichtenanreicherungen gestempelt. Wenn Sie die Informationen zum Gerätezwilling anzeigen möchten, wechseln Sie zu Ihrem Hub im Portal, und wählen Sie **IoT-Geräte**, Ihr Gerät und dann oben auf der Seite **Gerätezwilling** aus.
   >
   > Sie können die Zwillingsinformationen bearbeiten, um Tags (z.B. einen Standort) hinzuzufügen und bei Bedarf einen bestimmten Wert dafür festzulegen. Weitere Informationen finden Sie unter [Verstehen und Verwenden von Gerätezwillingen in IoT Hub](iot-hub-devguide-device-twins.md).

3. Wenn Sie fertig sind, sollte Ihr Bereich ähnlich wie in dieser Abbildung aussehen:

   ![Tabelle mit allen hinzugefügten Anreicherungen](./media/tutorial-message-enrichments/all-message-enrichments.png)

4. Wählen Sie **Übernehmen** aus, um die Änderungen zu speichern. Wechseln Sie zum Abschnitt [Testen von Nachrichtenanreicherungen](#testing-message-enrichments).

## <a name="use-an-rm-template-to-create-and-configure-the-resources-message-routing-and-message-enrichments"></a>Verwenden einer RM-Vorlage zum Erstellen und Konfigurieren der Ressourcen, des Nachrichtenroutings und der Nachrichtenanreicherungen 

1. Melden Sie sich im Azure-Portal an. Klicken Sie auf **+ Ressource erstellen**. Dann wird das Suchfeld angezeigt. Suchen Sie nach **Vorlagenbereitstellung**. Wählen Sie im Ergebnisbereich **Vorlagenbereitstellung (Bereitstellen mit benutzerdefinierten Vorlagen)** aus.

   ![Vorlagenbereitstellung im Azure-Portal](./media/tutorial-message-enrichments/template-select-deployment.png)

1. Wählen Sie im Bereich „Vorlagenbereitstellung“ die Option **Erstellen** aus. 

1. Wählen Sie im Bereich „Benutzerdefinierte Bereitstellung“ die Option **Eigene Vorlage im Editor erstellen** aus.

1. Wählen Sie im Bereich „Vorlage bearbeiten“ die Option **Datei laden** aus. Dann wird der Windows-Explorer angezeigt. Suchen Sie in der entzippten Repositorydatei in **/iot-hub/Tutorials/Routing/SimulatedDevice/resources** die Datei **template_messageenrichments.json**. 

   ![Auswählen der Vorlage vom lokalen Computer](./media/tutorial-message-enrichments/template-select.png)

1. Wählen Sie **Öffnen** aus, um die Vorlagendatei vom lokalen Computer zu laden. Sie wird in den Bearbeitungsbereich geladen und Ihnen angezeigt.

   Diese Vorlage wird so eingerichtet, dass sie einen global eindeutigen IoT Hub-Namen und Speicherkontonamen verwendet, indem sie am Ende der Standardnamen einen Zufallswert hinzufügt. Dann können Sie die Vorlage verwenden, ohne Änderungen daran vornehmen zu müssen. 

   Hier sind die Ressourcen, die durch das Laden der Vorlage erstellt werden. **enriched** bedeutet, dass die Ressource für Nachrichten mit Anreicherungen vorgesehen ist. **original** bedeutet, dass die Ressource für nicht angereicherte Nachrichten vorgesehen ist. Dabei handelt es sich um dieselben Werte, die im Azure CLI-Skript verwendet werden.

   | Name | value |
   |-----|-----|
   | resourceGroup | ContosoResourcesMsgEn |
   | Containername | original  |
   | Containername | enriched  |
   | IoT-Gerätename | Contoso-Test-Device |
   | IoT Hub-Name | ContosoTestHubMsgEn |
   | Speicherkontoname | contosostorage |
   | Endpunktname 1 | ContosoStorageEndpointOriginal |
   | Endpunktname 2 | ContosoStorageEndpointEnriched|
   | Routenname 1 | ContosoStorageRouteOriginal |
   | Routenname 2 | ContosoStorageRouteEnriched |

1. Wählen Sie **Speichern** aus. Dann wird der Bereich „Benutzerdefinierte Bereitstellung“ mit allen von der Vorlage verwendeten Parametern angezeigt. Als einziges Feld müssen Sie **Ressourcengruppe** verwenden. Erstellen Sie entweder eine neue Gruppe, oder wählen Sie eine aus der Dropdownliste aus.

   Hier ist die obere Hälfte des Bereichs „Benutzerdefinierte Bereitstellung“. Sie können sehen, wo Sie die Ressourcengruppe ausfüllen müssen.

   ![Obere Hälfte des Bereichs „Benutzerdefinierte Bereitstellung“](./media/tutorial-message-enrichments/template-deployment-top.png)

1. Hier ist die untere Hälfte des Bereichs „Benutzerdefinierte Bereitstellung“. Sie können die restlichen Parameter und die Nutzungsbedingungen sehen. 

   ![Untere Hälfte des Bereichs „Benutzerdefinierte Bereitstellung“](./media/tutorial-message-enrichments/template-deployment-bottom.png)

1. Aktivieren Sie das Kontrollkästchen mit der Angabe, dass Sie den Nutzungsbedingungen zustimmen, und wählen Sie **Kauf** aus, um mit der Vorlagenbereitstellung fortzufahren.

1. Warten Sie, bis die Vorlage vollständig bereitgestellt wurde. Sie können das Glockensymbol oben auf dem Bildschirm auswählen, um den Fortschritt zu überprüfen. Wenn die Bereitstellung abgeschlossen ist, können Sie mit [Testen von Nachrichtenanreicherungen](#testing-message-enrichments) fortfahren.

## <a name="testing-message-enrichments"></a>Testen von Nachrichtenanreicherungen

Sie können die Nachrichtenanreicherungen anzeigen, indem Sie **Ressourcengruppen** und dann die Ressourcengruppe auswählen, die Sie für dieses Tutorial verwenden. Wählen Sie in der Liste der Ressourcen den IoT-Hub aus, und wechseln Sie zu **Messaging**. Dann werden die Konfiguration des Nachrichtenroutings und die konfigurierten Anreicherungen angezeigt.

Nachdem Sie die Nachrichtenanreicherungen für den Endpunkt konfiguriert haben, führen Sie die App „Simuliertes Gerät“ aus, um Nachrichten an den IoT-Hub zu senden. Der Hub wurde mit Einstellungen eingerichtet, die folgende Aktionen ausführen:

* Nachrichten, die an den Speicherendpunkt „ContosoStorageEndpointOriginal“ weitergeleitet wurden, werden nicht angereichert und im Speichercontainer `original` gespeichert.

* Nachrichten, die an den Speicherendpunkt „ContosoStorageEndpointEnriched“ weitergeleitet wurden, werden angereichert und im Speichercontainer `enriched` gespeichert.

Die App „Simuliertes Gerät“ ist eine der Apps im entzippten Download. Die App sendet Nachrichten für jede der verschiedenen Nachrichtenroutingmethoden, die im [Routing-Tutorial](tutorial-routing.md) erläutert werden. Dazu gehört auch Azure Storage.

Doppelklicken Sie auf die Lösungsdatei (IoT_SimulatedDevice.sln), um den Code in Visual Studio zu öffnen. Öffnen Sie anschließend „Program.cs“. Ersetzen Sie den IoT Hub-Namen durch den Marker `{your hub name}`. Das Format des IoT Hub-Hostnamens ist **{Ihr Hub-Name}.azure-devices.net**. Für dieses Tutorial lautet der Hub-Hostname **ContosoTestHubMsgEn.azure-devices.net**. Ersetzen Sie als Nächstes den Geräteschlüssel, den Sie vorher bei der Ausführung des Skripts zum Erstellen der Ressourcen gespeichert haben, durch den Marker `{your device key}`.

Wenn Sie den Geräteschlüssel nicht haben, können Sie ihn über das Portal abrufen. Nachdem Sie sich angemeldet haben, wechseln Sie zu **Ressourcengruppen**, wählen Sie Ihre Ressourcengruppe und dann Ihren IoT-Hub aus. Suchen Sie unter **IoT-Geräte** nach Ihrem Testgerät, und wählen Sie es aus. Wählen Sie das Kopiersymbol neben **Primärschlüssel** aus, um ihn in die Zwischenablage zu kopieren.

   ```csharp
        private readonly static string s_myDeviceId = "Contoso-Test-Device";
        private readonly static string s_iotHubUri = "ContosoTestHubMsgEn.azure-devices.net";
        // This is the primary key for the device. This is in the portal.
        // Find your IoT hub in the portal > IoT devices > select your device > copy the key.
        private readonly static string s_deviceKey = "{your device key}";
   ```

### <a name="run-and-test"></a>Ausführen und Testen

Führen Sie die Konsolenanwendung ein paar Minuten lang aus. Die gesendeten Nachrichten werden auf dem Konsolenbildschirm der App angezeigt.

Die App sendet jede Sekunde eine neue Gerät-zu-Cloud-Nachricht an die IoT Hub-Instanz. Die Nachricht enthält ein JSON-serialisiertes Objekt mit Geräte-ID, Temperatur und Luftfeuchtigkeit sowie der Nachrichtenebene, die standardmäßig `normal` lautet. Die App weist die Ebene `critical` oder `storage` nach dem Zufallsprinzip zu. Dies bewirkt, dass die Nachricht an das Speicherkonto oder den Standardendpunkt weitergeleitet wird. Die an den Container **enriched** im Speicherkonto gesendeten Nachrichten werden angereichert.

Nachdem mehrere Speichernachrichten gesendet wurden, zeigen Sie die Daten an.

1. Wählen Sie **Ressourcengruppen** aus, suchen Sie nach Ihrer Ressourcengruppe, („ContosoResourcesMsgEn“), und wählen Sie sie aus.

2. Wählen Sie Ihr Speicherkonto, („contosostorage“), aus. Wählen Sie dann im Auswahlbereich links **Speicher-Explorer (Vorschauversion)** aus.

   ![Auswählen des Speicher-Explorers](./media/tutorial-message-enrichments/select-storage-explorer.png)

   Wählen Sie **BLOB-Container** aus, um die beiden Container anzuzeigen, die verwendet werden können.

   ![Anzeigen der Container im Speicherkonto](./media/tutorial-message-enrichments/show-blob-containers.png)

Den Nachrichten im Container **enriched** wurden die Nachrichtenanreicherungen hinzugefügt. Die Nachrichten im Container **original** wurden nicht angereichert. Führen Sie in einem der Container einen Drilldown bis ganz nach unten aus, und öffnen Sie die neueste Nachrichtendatei. Wiederholen Sie diesen Schritt dann bei dem anderen Container, um sich zu vergewissern, dass den Nachrichten in diesem Container keine Anreicherungen hinzugefügt wurden.

Bei angereicherten Nachrichten sollte die Bezeichnung „my IoT Hub“ (mein IoT-Hub) mit dem Hubnamen sowie dem Gerätestandort und der Kunden-ID angezeigt werden, wie hier:

```json
{"EnqueuedTimeUtc":"2019-05-10T06:06:32.7220000Z","Properties":{"level":"storage","my IoT Hub":"contosotesthubmsgen3276","devicelocation":"$twin.tags.location","customerID":"6ce345b8-1e4a-411e-9398-d34587459a3a"},"SystemProperties":{"connectionDeviceId":"Contoso-Test-Device","connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}","connectionDeviceGenerationId":"636930642531278483","enqueuedTime":"2019-05-10T06:06:32.7220000Z"},"Body":"eyJkZXZpY2VJZCI6IkNvbnRvc28tVGVzdC1EZXZpY2UiLCJ0ZW1wZXJhdHVyZSI6MjkuMjMyMDE2ODQ4MDQyNjE1LCJodW1pZGl0eSI6NjQuMzA1MzQ5NjkyODQ0NDg3LCJwb2ludEluZm8iOiJUaGlzIGlzIGEgc3RvcmFnZSBtZXNzYWdlLiJ9"}
```

Hier ist eine nicht angereicherte Nachricht. „my IoT Hub“, „devicelocation“ und „customerID“ werden hier nicht angezeigt, weil es sich hierbei um die Felder handelt, die von den Anreicherungen hinzugefügt würden, und es für diesen Endpunkt keine Anreicherungen gibt.

```json
{"EnqueuedTimeUtc":"2019-05-10T06:06:32.7220000Z","Properties":{"level":"storage"},"SystemProperties":{"connectionDeviceId":"Contoso-Test-Device","connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}","connectionDeviceGenerationId":"636930642531278483","enqueuedTime":"2019-05-10T06:06:32.7220000Z"},"Body":"eyJkZXZpY2VJZCI6IkNvbnRvc28tVGVzdC1EZXZpY2UiLCJ0ZW1wZXJhdHVyZSI6MjkuMjMyMDE2ODQ4MDQyNjE1LCJodW1pZGl0eSI6NjQuMzA1MzQ5NjkyODQ0NDg3LCJwb2ludEluZm8iOiJUaGlzIGlzIGEgc3RvcmFnZSBtZXNzYWdlLiJ9"}
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie alle für dieses Tutorial erstellten Ressourcen entfernen möchten, löschen Sie die Ressourcengruppe. Diese Aktion löscht alle in der Gruppe enthaltenen Ressourcen. In diesem Fall werden der IoT-Hub, das Speicherkonto und die Ressourcengruppe selbst entfernt.

### <a name="use-the-azure-cli-to-clean-up-resources"></a>Verwenden der Azure CLI zum Bereinigen von Ressourcen

Um die Ressourcengruppe zu entfernen, verwenden Sie den Befehl [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete). `$resourceGroup` wurde am Anfang dieses Tutorials auf **ContosoResourcesMsgEn** festgelegt.

```azurecli-interactive
az group delete --name $resourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie mit folgenden Schritten das Hinzufügen von Nachrichtenanreicherungen zu IoT Hub-Nachrichten konfiguriert und getestet:

**Verwenden von IoT Hub-Nachrichtenanreicherungen**
> [!div class="checklist"]
> * Erste Methode
>   * Erstellen Sie Ressourcen, und konfigurieren Sie das Nachrichtenrouting über die Azure CLI.
>   * Konfigurieren Sie die Nachrichtenanreicherungen manuell über das [Azure-Portal](https://portal.azure.com).
> * Zweite Methode
>   * Erstellen Sie Ressourcen, und konfigurieren Sie Nachrichtenrouting und Nachrichtenanreicherungen mithilfe einer Azure Resource Manager-Vorlage. 
> * Führen Sie eine App aus, die ein IoT-Gerät simuliert, das Nachrichten an den Hub sendet.
> * Zeigen Sie die Ergebnisse an, und überprüfen Sie, ob die Nachrichtenanreicherungen wie erwartet funktionieren.

Weitere Informationen zu Nachrichtenanreicherungen finden Sie in der [Übersicht über Nachrichtenanreicherungen](iot-hub-message-enrichments-overview.md).

Weitere Informationen zum Nachrichtenrouting finden Sie in diesen Artikeln:

* [Verwenden des IoT Hub-Nachrichtenroutings zum Senden von Gerät-zu-Cloud-Nachrichten an verschiedene Endpunkte](iot-hub-devguide-messages-d2c.md)

* [Tutorial: IoT Hub-Routing](tutorial-routing.md)