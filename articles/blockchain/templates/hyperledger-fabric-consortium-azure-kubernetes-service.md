---
title: Hyperledger Fabric-Konsortium auf Azure Kubernetes Service (AKS)
description: Bereitstellen und Konfigurieren eines Hyperledger Fabric-Konsortiumsnetzwerks auf Azure Kubernetes Service
ms.date: 01/08/2020
ms.topic: article
ms.reviewer: v-umha
ms.openlocfilehash: 2312c002e5c2e0b813f8acbdc3e3bff597f204d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476439"
---
# <a name="hyperledger-fabric-consortium-on-azure-kubernetes-service-aks"></a>Hyperledger Fabric-Konsortium auf Azure Kubernetes Service (AKS)

Sie können die Vorlage für „Hyperledger Fabric (HLF) auf Azure Kubernetes Service (AKS)“ verwenden, um ein Hyperledger Fabric-Konsortiumsnetzwerks in Azure bereitzustellen und zu konfigurieren.

In diesem Artikel lernen Sie Folgendes:

- Erwerben Sie praktische Kenntnisse über Hyperledger Fabric und die verschiedenen Komponenten, die die Bausteine eines Hyperledger Fabric-Blockchainnetzwerks bilden.
- Erfahren Sie, wie Sie ein Hyperledger Fabric-Konsortium auf Azure Kubernetes Service für Ihre Produktionsszenarien bereitstellen und konfigurieren.

## <a name="hyperledger-fabric-consortium-architecture"></a>Architektur eines Hyperledger Fabric-Konsortiums

Um ein Hyperledger Fabric-Netzwerk in Azure zu erstellen, müssen Sie Ordering Service (Anordnungsdienst) und eine Organisation mit Peerknoten bereitstellen. Die folgenden verschiedenen grundlegenden Komponenten werden im Rahmen der Vorlagenbereitstellung erstellt:

- **Auftraggeberknoten**: Ein Knoten, der für die Transaktionsanordnung im Ledger zuständig ist. Zusammen mit anderen Knoten bilden die Anordnerknoten (Orderer-Knoten, Auftraggeberknoten) den Anordnungsdienst des Hyperledger Fabric-Netzwerks.

- **Peerknoten**: Ein Knoten, der hauptsächlich Ledger und Smart Contracts hostet, die die grundlegenden Elemente des Netzwerks darstellen.

- **Fabric CA**: Fabric CA ist die Zertifizierungsstelle (Certificate Authority, CA) für Hyperledger Fabric. Mit der Fabric CA können Sie den Serverprozess, der die Zertifizierungsstelle hostet, initialisieren und starten. Sie ermöglicht Ihnen die Verwaltung von Identitäten und Zertifikaten. Jeder AKS-Cluster, der als Teil der Vorlage bereitgestellt wird, hat standardmäßig einen Fabric CA-Pod.

- **CouchDB oder LevelDB**: Die Weltzustandsdatenbank (World state database) für die Peerknoten kann entweder in LevelDB oder CouchDB gespeichert werden. LevelDB ist die standardmäßige Zustandsdatenbank, die in den Peerknoten eingebettet ist. LevelDB speichert Chaincode-Daten als einfache Schlüssel-Wert-Paare und unterstützt nur Schlüssel- und Schlüsselbereichsabfragen sowie Abfragen für zusammengesetzte Schlüssel. CouchDB ist eine optionale alternative Zustandsdatenbank, die umfangreiche Abfragen unterstützt, wenn Chaincode-Datenwerte als JSON modelliert sind.

Die Vorlage startet bei einer Bereitstellung verschiedene Azure-Ressourcen in Ihrem Abonnement. Die folgenden verschiedenen Azure-Ressourcen werden bereitgestellt:

- **AKS-Cluster**: Ein Azure Kubernetes-Cluster, der gemäß den vom Kunden bereitgestellten Eingabeparametern konfiguriert wird. Der AKS-Cluster hat verschiedene Pods, die für das Ausführen der Hyperledger Fabric-Netzwerkkomponenten konfiguriert sind. Die folgenden unterschiedlichen Pods werden erstellt:

  - **Fabric-Tools**: Das Fabric-Tool ist für das Konfigurieren der Hyperledger Fabric-Komponenten zuständig.
  - **Anordner-/Peer-Pods**: Die Knoten des HLF-Netzwerks.
  - **Proxy**: Ein NGINX-Proxypod, über den die Clientanwendungen eine Verbindung mit dem AKS-Cluster herstellen können.
  - **Fabric CA**: Der Pod, der die Fabric CA ausführt.
- **PostgreSQL**: Eine Instanz von PostgreSQL wird bereitgestellt, um die Fabric CA-Identitäten zu verwalten.

- **Azure Key Vault**: Es wird eine Key Vault-Instanz bereitgestellt, um die Fabric CA-Anmeldeinformationen und die Stammzertifikate zu speichern, die vom Kunden bereitgestellt werden. Diese werden im Fall einer wiederholten Vorlagenbereitstellung verwendet, um die Mechanismen der Vorlage zu verarbeiten.
- **Verwalteter Azure-Datenträger**: Der verwaltete Azure-Datenträger wird für den dauerhaften Speicher für die Weltzustandsdatenbank für Ledger- und Peerknoten verwendet.
- **Öffentliche IP**: Ein öffentlicher IP-Endpunkt des AKS-Clusters, der bereitgestellt wird, um als Schnittstelle für den Cluster zu fungieren.

## <a name="hyperledger-fabric-blockchain-network-setup"></a>Einrichten eines Hyperledger Fabric Blockchain-Netzwerks

Damit Sie beginnen können, benötigen Sie ein Azure-Abonnement, das die Bereitstellung mehrerer virtueller Computer und Standardspeicherkonten unterstützen kann. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto erstellen](https://azure.microsoft.com/free/).

Führen Sie die folgenden Schritte aus, um ein Hyperledger Fabric Blockchain-Netzwerk einzurichten:

- [Bereitstellen der Anordner/Peer-Organisation](#deploy-the-ordererpeer-organization)
- [Erstellen des Konsortiums](#build-the-consortium)
- [Ausführen nativer HLF-Vorgänge](#run-native-hlf-operations)

## <a name="deploy-the-ordererpeer-organization"></a>Bereitstellen der Anordner/Peer-Organisation

Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um mit der Bereitstellung der HLF-Netzwerkkomponenten zu beginnen. Wählen Sie **Ressource erstellen > Blockchain** aus, und suchen Sie nach **Hyperledger Fabric auf Azure Kubernetes Service**.

1. Wählen Sie **Erstellen** aus, um die Vorlagenbereitstellung zu starten. Die Seite **Hyperledger Fabric auf Azure Kubernetes Service erstellen** wird angezeigt.

    ![Vorlage „Hyperledger Fabric auf Azure Kubernetes Service“](./media/hyperledger-fabric-consortium-azure-kubernetes-service/hyperledger-fabric-aks.png)

2. Geben Sie die Projektdetails auf der Seite **Grundlagen** ein.

    ![Vorlage „Hyperledger Fabric auf Azure Kubernetes Service“](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-basics.png)

3. Geben Sie die folgenden Details ein:
    - **Abonnement**: Wählen Sie den Namen des Abonnements aus, in dem Sie die HLF-Netzwerkkomponenten bereitstellen möchten.
    - **Ressourcengruppe**: Erstellen Sie entweder eine neue Ressourcengruppe, oder wählen Sie eine vorhandene leere Ressourcengruppe aus. Die Ressourcengruppe enthält alle Ressourcen, die als Teil der Vorlage bereitgestellt werden.
    - **Region**: Wählen Sie die Azure-Region aus, in der Sie den Azure Kubernetes-Cluster für die HLF-Komponenten bereitstellen möchten. Die Vorlage ist in allen Regionen verfügbar, in denen AKS verfügbar ist. Stellen Sie sicher, dass Sie eine Region auswählen, in der Ihr Abonnement nicht die Kontingentgrenze für virtuelle Computer erreicht.
    - **Ressourcenpräfix**: Das Präfix für die Benennung von Ressourcen, die bereitgestellt werden. Das Ressourcenpräfix darf höchstens fünf Zeichen lang sein, und die Kombination der Zeichen muss sowohl Zahlen als auch Buchstaben enthalten.
4. Wählen Sie die Registerkarte **Fabric-Einstellungen** aus, um die HLF-Netzwerkkomponenten zu definieren, die bereitgestellt werden.

    ![Vorlage „Hyperledger Fabric auf Azure Kubernetes Service“](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-settings.png)

5. Geben Sie die folgenden Details ein:
    - **Name der Organisation:** Der Name der Fabric-Organisation, die für verschiedene Datenebenenvorgänge erforderlich ist. Der Name der Organisation muss pro Bereitstellung eindeutig sein. 
    - **Fabric-Netzwerkkomponente**: Wählen Sie entsprechend der Blockchain-Netzwerkkomponente, die Sie einrichten möchten, entweder „Anordnungsdienst“ oder „Peerknoten“ aus.
    - **Anzahl der Knoten**: Es gibt die beiden folgenden Knotentypen:
        - Anordnungsdienst: Wählen Sie die Anzahl der Knoten aus, um Fehlertoleranz für das Netzwerk bereitzustellen. Als Anzahl für Anordnerknoten werden nur 3, 5 und 7 unterstützt.
        - Peerknoten: Sie können je nach Ihren Anforderung 1 bis 10 Knoten auswählen.
    - **Weltzustandsdatenbank für Peerknoten**: Wählen Sie zwischen „LevelDB“ und „CoucbDB“. Dieses Feld wird angezeigt, wenn der Benutzer in der Dropdownliste „Fabric-Netzwerkkomponente“ die Option „Peerknoten“ ausgewählt hat.
    - **Fabric-Benutzername**: Geben Sie den Benutzernamen ein, der für die Fabric CA-Authentifizierung verwendet wird.
    - **Fabric CA-Kennwort**: Geben Sie das Kennwort für die Fabric CA-Authentifizierung ein.
    - **Kennwort bestätigen**: Bestätigen Sie das Fabric CA-Kennwort.
    - **Zertifikate**: Wenn Sie Ihre eigenen Stammzertifikate verwenden möchten, um die Fabric CA zu initialisieren, wählen Sie die Option „Stammzertifikat für Fabric CA hochladen“ aus. Andernfalls erstellt Fabric CA standardmäßig selbstsignierte Zertifikate.
    - **Stammzertifikat**: Laden Sie das Stammzertifikat (öffentlicher Schlüssel) hoch, mit dem Fabric CA initialisiert werden muss. Zertifikate im PEM-Format werden unterstützt. Die Zertifikate müssen in der UTC-Zeitzone gültig sein.
    - **Privater Schlüssel des Stammzertifikats**: Laden Sie den privaten Schlüssel des Stammzertifikats hoch. Wenn Sie ein PEM-Zertifikat haben, in dem sowohl der öffentliche als auch der private Schlüssel kombiniert sind, laden Sie es hier ebenfalls hoch.


6. Wählen Sie die Registerkarte **AKS-Clustereinstellungen** aus, um die Azure Kubernetes-Clusterkonfiguration zu definieren, die die zugrunde liegende Infrastruktur ist, in der die Fabric-Netzwerkkomponenten eingerichtet werden.

    ![Vorlage „Hyperledger Fabric auf Azure Kubernetes Service“](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-aks-cluster-settings-1.png)

7. Geben Sie die folgenden Details ein:
    - **Name des Kubernetes-Clusters**: Der Name des AKS-Clusters, der erstellt wird. Dieses Feld ist basierend auf dem angegebenen Ressourcenpräfix bereits ausgefüllt. Sie können den Namen ggf. ändern.
    - **Kubernetes-Version**: Die Version der Kubernetes-Plattform, die im Cluster bereitgestellt wird. Entsprechend der Region, die auf der Registerkarte **Grundlagen** ausgewählt ist, können unterschiedliche unterstützte Versionen verfügbar sein.
    - **DNS-Präfix**: DNS-Namenspräfix (Domain Name System) für den AKS-Cluster. Sie verwenden DNS, um eine Verbindung mit der Kubernetes-API herzustellen, wenn Sie Container nach dem Erstellen des Clusters verwalten.
    - **Knotengröße**: Die Größe des Kubernetes-Knotens. Sie können eine Größe in der Liste der in Azure verfügbaren SKUs (Stock Keeping Units) für virtuelle Computer auswählen. Um eine optimale Leistung zu erzielen, wird Standard DS3 v2 empfohlen.
    - **Knotenanzahl**: Die Anzahl der Kubernetes-Knoten, die im Cluster bereitgestellt werden sollen. Es empfiehlt sich, als Wert für „Knotenanzahl“ mindestens die Anzahl der HLF-Knoten anzugeben, die in den Fabric-Einstellungen angegeben ist.
    - **Client-ID des Dienstprinzipals**: Geben Sie die Client-ID eines vorhandenen Dienstprinzipals ein, oder erstellen Sie einen neuen, der für die AKS-Authentifizierung erforderlich ist. Weitere Informationen finden Sie in den Schritten zum [Erstellen des Dienstprinzipals](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?view=azps-3.2.0#create-a-service-principal).
    - **Clientgeheimnis des Dienstprinzipals**: Geben Sie das Clientgeheimnis des Dienstprinzipals ein, das in der Client-ID des Dienstprinzipals bereitgestellt wird.
    - **Clientgeheimnis bestätigen**: Bestätigen Sie das Clientgeheimnis, das im Clientgeheimnis des Dienstprinzipals bereitgestellt wird.
    - **Containerüberwachung aktivieren**: Aktivieren Sie die AKS-Überwachung. Dadurch wird es ermöglicht, die AKS-Protokolle in den angegebenen Log Analytics-Arbeitsbereich zu pushen.
    - **Log Analytics-Arbeitsbereich**: Der Log Analytics-Arbeitsbereich wird mit dem Standardarbeitsbereich aufgefüllt, der erstellt wird, wenn die Überwachung aktiviert ist.

8. Nachdem Sie alle oben aufgeführten Optionen festgelegt haben, wählen Sie die Registerkarte **Überprüfen und erstellen** aus. Dadurch wird die Validierung der Werte ausgelöst, die Sie angegeben haben.
9. Sobald die Validierung erfolgreich abgeschlossen ist, können Sie **Erstellen** auswählen.
Die Bereitstellung dauert in der Regel 10 bis 12 Minuten, kann aber je nach Größe und Anzahl der angegebenen AKS-Knoten variieren.
10. Nach der erfolgreichen Bereitstellung werden Sie über Azure-Benachrichtigungen in der oberen rechten Ecke benachrichtigt.
11. Wählen Sie **Zu Ressourcengruppe wechseln**, um alle Ressourcen zu überprüfen, die im Rahmen der Vorlagenbereitstellung erstellt wurden. Alle Ressourcennamen beginnen mit dem Präfix, das in der **Grundlagen**-Einstellung angegeben ist.

## <a name="build-the-consortium"></a>Erstellen des Konsortiums

Um das Blockchain-Konsortium nach dem Bereitstellen des Anordnungsdiensts (Ordering service) und der Peerknoten zu erstellen, müssen Sie die weiter unten aufgeführten Schritte nacheinander ausführen. Sie verwenden das Skript „byn.sh“ (**Build Your Network**), das Sie beim Einrichten des Konsortiums, beim Erstellen eines Kanals und beim Installieren von Chaincode unterstützt.

> [!NOTE]
> Das „Build Your Network“-Skript (byn-Skript) ist ausschließlich für Demo-/DevTest-Szenarien geeignet. Für das Einrichten eines Produktionsumgebung sollten Sie die nativen HLF-APIs verwenden.

Alle Befehle zum Ausführen des byn-Skripts können über die Bash-Befehlszeilenschnittstelle (Command Line Interface, CLI) von Azure ausgeführt werden. Sie können sich über die Option ![Vorlage „Hyperledger Fabric auf Azure Kubernetes Service“](./media/hyperledger-fabric-consortium-azure-kubernetes-service/arrow.png) in der oberen rechten Ecke des Azure-Portals bei der Webversion der Azure-Shell anmelden. Geben Sie an der Eingabeaufforderung „bash“ ein, und drücken Sie die Eingabetaste, um zur bash-CLI zu wechseln.

Weitere Informationen finden Sie unter [Übersicht über Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

![Vorlage „Hyperledger Fabric auf Azure Kubernetes Service“](./media/hyperledger-fabric-consortium-azure-kubernetes-service/hyperledger-powershell.png)


Laden Sie die Dateien „byn.sh“ und „fabric-admin.yaml“ herunter.

```bash-interactive
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/consortiumScripts/byn.sh -o byn.sh; chmod 777 byn.sh
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/consortiumScripts/fabric-admin.yaml -o fabric-admin.yaml
```
**Legen Sie die folgenden Umgebungsvariablen für die Azure CLI-Bash-Shell** fest:

Legen Sie Kanalinformationen sowie Informationen für die Organisation des Anordners (Orderer) fest.

```bash
SWITCH_TO_AKS_CLUSTER() { az aks get-credentials --resource-group $1 --name $2 --subscription $3; }
ORDERER_AKS_SUBSCRIPTION=<ordererAKSClusterSubscriptionID>
ORDERER_AKS_RESOURCE_GROUP=<ordererAKSClusterResourceGroup>
ORDERER_AKS_NAME=<ordererAKSClusterName>
ORDERER_DNS_ZONE=$(az aks show --resource-group $ORDERER_AKS_RESOURCE_GROUP --name $ORDERER_AKS_NAME --subscription $ORDERER_AKS_SUBSCRIPTION -o json | jq .addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName | tr -d '"')
ORDERER_END_POINT="orderer1.$ORDERER_DNS_ZONE:443"
CHANNEL_NAME=<channelName>
```
Legen Sie Informationen für die Peerorganisation fest.

```bash
PEER_AKS_RESOURCE_GROUP=<peerAKSClusterResourceGroup>
PEER_AKS_NAME=<peerAKSClusterName>
PEER_AKS_SUBSCRIPTION=<peerAKSClusterSubscriptionID>
#Peer organization name is case-sensitive. Specify exactly the same name, which was provided while creating the Peer AKS Cluster.
PEER_ORG_NAME=<peerOrganizationName>
```

Erstellen Sie eine Azure-Dateifreigabe, um verschiedene öffentliche Zertifikate in Peer- und Anordnerorganisation gemeinsam zu nutzen.

```bash
STORAGE_SUBSCRIPTION=<subscriptionId>
STORAGE_RESOURCE_GROUP=<azureFileShareResourceGroup>
STORAGE_ACCOUNT=<azureStorageAccountName>
STORAGE_LOCATION=<azureStorageAccountLocation>
STORAGE_FILE_SHARE=<azureFileShareName>

az account set --subscription $STORAGE_SUBSCRIPTION
az group create -l $STORAGE_LOCATION -n $STORAGE_RESOURCE_GROUP
az storage account create -n $STORAGE_ACCOUNT -g  $STORAGE_RESOURCE_GROUP -l $STORAGE_LOCATION --sku Standard_LRS
STORAGE_KEY=$(az storage account keys list --resource-group $STORAGE_RESOURCE_GROUP  --account-name $STORAGE_ACCOUNT --query "[0].value" | tr -d '"')
az storage share create  --account-name $STORAGE_ACCOUNT  --account-key $STORAGE_KEY  --name $STORAGE_FILE_SHARE
SAS_TOKEN=$(az storage account generate-sas --account-key $STORAGE_KEY --account-name $STORAGE_ACCOUNT --expiry `date -u -d "1 day" '+%Y-%m-%dT%H:%MZ'` --https-only --permissions lruwd --resource-types sco --services f | tr -d '"')
AZURE_FILE_CONNECTION_STRING="https://$STORAGE_ACCOUNT.file.core.windows.net/$STORAGE_FILE_SHARE?$SAS_TOKEN"
```
**Befehle für die Kanalverwaltung**

Wechseln Sie zum AKS-Cluster der Anordnerorganisation (Orderer-Organisation), und führen Sie den Befehl zum Erstellen eines neuen Kanals aus.

```bash
SWITCH_TO_AKS_CLUSTER $ORDERER_AKS_RESOURCE_GROUP $ORDERER_AKS_NAME $ORDERER_AKS_SUBSCRIPTION
./byn.sh createChannel "$CHANNEL_NAME"
```

**Befehle für die Konsortiumsverwaltung**

Führen Sie die folgenden Befehle in der angegebenen Reihenfolge aus, um eine Peerorganisation in einem Kanal und einem Konsortium hinzuzufügen.

1. Wechseln Sie zum AKS-Cluster der Peerorganisation, und laden Sie deren MSP (Member Service Provider) in einen Azure-Dateispeicher hoch.

    ```bash
    SWITCH_TO_AKS_CLUSTER $PEER_AKS_RESOURCE_GROUP $PEER_AKS_NAME $PEER_AKS_SUBSCRIPTION
    ./byn.sh uploadOrgMSP "$AZURE_FILE_CONNECTION_STRING"
    ```

2. Wechseln Sie zum AKS-Cluster der Anordnerorganisation, und fügen Sie die Peerorganisation im Kanal und im Konsortium hinzu.

    ```bash
    SWITCH_TO_AKS_CLUSTER $ORDERER_AKS_RESOURCE_GROUP $ORDERER_AKS_NAME $ORDERER_AKS_SUBSCRIPTION
    #add peer in consortium
    ./byn.sh addPeerInConsortium "$PEER_ORG_NAME" "$AZURE_FILE_CONNECTION_STRING"
    #add peer in channel
    ./byn.sh addPeerInChannel "$PEER_ORG_NAME" "$CHANNEL_NAME" "$AZURE_FILE_CONNECTION_STRING"
    ```

3. Wechseln Sie zurück zur Peerorganisation, und führen Sie den Befehl aus, mit dem die Peerknoten im Kanal verknüpft werden.

    ```bash
    SWITCH_TO_AKS_CLUSTER $PEER_AKS_RESOURCE_GROUP $PEER_AKS_NAME $PEER_AKS_SUBSCRIPTION
    ./byn.sh joinNodesInChannel "$CHANNEL_NAME" "$ORDERER_END_POINT" "$AZURE_FILE_CONNECTION_STRING"
    ```

Wenn Sie weitere Peerorganisationen im Kanal hinzufügen möchten, aktualisieren Sie die Peer-AKS-Umgebungsvariablen entsprechend der erforderlichen Peerorganisation, und führen Sie die Schritte 1 bis 3 aus.

**Befehle für die Chaincode-Verwaltung**

Führen Sie den folgenden Befehl aus, um den chaincodebezogenen Vorgang durchzuführen. Dieser Befehl führt alle Vorgänge für einen Demo-Chaincode aus. Dieser Demo-Chaincode hat die beiden Variablen „a“ und „b“. Bei der Instanziierung des Chaincodes wird „a“ mit 1000 und „b“ mit 2000 initialisiert. Bei jedem Aufruf des Chaincodes werden 10 Einheiten von „a“ nach „b“ übertragen. Eine Abfrage des Chaincodes gibt den Weltzustand von Variable „a“ zurück.

Führen Sie den folgenden Befehl für den AKS-Cluster der Peerorganisation aus.

```bash
# switch to peer organization AKS cluster. Skip this command if already connected to the required Peer AKS Cluster
SWITCH_TO_AKS_CLUSTER $PEER_AKS_RESOURCE_GROUP $PEER_AKS_NAME $PEER_AKS_SUBSCRIPTION
```
**Chaincode-Vorgangsbefehle**

```bash
PEER_NODE_NAME="peer<peer#>"
./byn.sh installDemoChaincode "$PEER_NODE_NAME"
./byn.sh instantiateDemoChaincode "$PEER_NODE_NAME" "$CHANNEL_NAME" "$ORDERER_END_POINT" "$AZURE_FILE_CONNECTION_STRING"
./byn.sh invokeDemoChaincode "$PEER_NODE_NAME" "$CHANNEL_NAME" "$ORDERER_END_POINT" "$AZURE_FILE_CONNECTION_STRING"
./byn.sh queryDemoChaincode "$PEER_NODE_NAME" "$CHANNEL_NAME"
```

## <a name="run-native-hlf-operations"></a>Ausführen nativer HLF-Vorgänge

Kunden sollen dabei unterstützt werden, native Hyperledger-Befehle im HLF-Netzwerk auf AKS auszuführen. In der bereitgestellten Beispielanwendung wird das Fabric Node.js SDK verwendet, um die HLF-Vorgänge auszuführen. Mit den bereitgestellten Befehlen können Sie eine neue Benutzeridentität erstellen und Ihren eigenen Chaincode installieren.

### <a name="before-you-begin"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, um die Anfangseinrichtung der Anwendung vorzunehmen:

- Herunterladen der Anwendungsdateien
- Generieren des Verbindungsprofils und des Administratorprofils
- Importieren der Identität des Administratorbenutzers

Nach Abschluss der Anfangseinrichtung können Sie das SDK verwenden, um die folgenden Vorgänge durchzuführen:

- Generieren der Benutzeridentität
- Chaincode-Vorgänge

Die oben aufgeführten Befehle können aus Azure Cloud Shell ausgeführt werden.

### <a name="download-application-files"></a>Herunterladen der Anwendungsdateien

Der erste Schritt für eine Ausführen der Anwendung besteht darin, alle Anwendungsdateien in einen Ordner herunterzuladen.

**Erstellen Sie den Ordner „app“, und wechseln Sie in diesen Ordner**:

```bash
mkdir app
cd app
```
Führen Sie den folgenden Befehl aus, um alle erforderlichen Dateien und Pakete herunterzuladen:

```bash-interactive
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/application/setup.sh | bash
```
Dieser Befehl benötigt einige Zeit, um alle Pakete zu laden. Nach erfolgreicher Ausführung des Befehls sehen Sie den Ordner `node_modules` im aktuellen Verzeichnis. Alle erforderlichen Pakete wurden in den Ordner `node_modules` geladen.

### <a name="generate-connection-profile-and-admin-profile"></a>Generieren des Verbindungsprofils und des Administratorprofils

Erstellen Sie das Verzeichnis `profile` im Ordner `app`.

```bash
cd app
mkdir ./profile
```
Legen Sie diese Umgebungsvariablen für Azure Cloud Shell fest.

```bash
# Organization name whose connection profile is to be generated
ORGNAME=<orgname>
# Organization AKS cluster resource group
AKS_RESOURCE_GROUP=<resourceGroup>
```

Führen Sie den folgenden Befehl aus, um das Verbindungsprofil und das Administratorprofil der Organisation zu generieren.

```bash
./getConnector.sh $AKS_RESOURCE_GROUP | sed -e "s/{action}/gateway/g"| xargs curl > ./profile/$ORGNAME-ccp.json
./getConnector.sh $AKS_RESOURCE_GROUP | sed -e "s/{action}/admin/g"| xargs curl > ./profile/$ORGNAME-admin.json
```

Der Befehl erstellt das Verbindungsprofil und das Administrator`profile` der Organisation im Profilordner `<orgname>-ccp.json` bzw. `<orgname>-admin.json`.

Generieren Sie auf gleiche Weise das Verbindungsprofil und das Administratorprofil für jede Anordner- und Peerorganisation.


### <a name="import-admin-user-identity"></a>Importieren der Identität des Administratorbenutzers

Der letzte Schritt besteht darin, die Administratorbenutzeridentität der Organisation in das Wallet zu importieren.

```bash
npm run importAdmin -- -o <orgName>

```
Der obige Befehl führt „importAdmin.js“ aus, um die Administratorbenutzeridentität in das Wallet zu importieren. Das Skript liest die Administratoridentität aus dem Administratorprofil `<orgname>-admin.json` und importiert sie in das Wallet zum Ausführen von HLF-Vorgängen.

Im Skript wird das Dateisystem-Wallet verwendet, um die Identitäten zu speichern. Es erstellt ein Wallet gemäß dem Pfad, der im Verbindungsprofil im Feld „.wallet“ angegeben ist. Standardmäßig wird das Feld „.wallet“ mit `<orgname>` initialisiert. Dies bedeutet, dass ein Ordner namens `<orgname>` im aktuellen Verzeichnis erstellt wird, um die Identitäten zu speichern. Wenn Sie das Wallet in einem anderen Ordner erstellen möchten, ändern Sie das Feld „.wallet“ im Verbindungsprofil, bevor Sie das Registrieren des Administratorbenutzers und andere HLF-Vorgänge ausführen.

Importieren Sie auf gleiche Weise die Administratorbenutzeridentität für jede Organisation.

Weitere Informationen zu den Argumenten, die an den Befehl übergeben werden, finden Sie in der Hilfe zum Befehl.

```bash
npm run importAdmin -- -h

```

### <a name="user-identity-generation"></a>Generieren der Benutzeridentität

Führen Sie die folgenden Befehle in der angegebenen Reihenfolge aus, um neue Benutzeridentitäten für die HLF-Organisation zu generieren.

> [!NOTE]
> Bevor Sie mit den Schritten zur Generierung der Benutzeridentitäten beginnen, stellen Sie sicher, dass die Anfangseinrichtung der Anwendung abgeschlossen ist.

Legen Sie die folgenden Umgebungsvariablen für Azure Cloud Shell fest.

```bash
# Organization name for which user identity is to be generated
ORGNAME=<orgname>
# Name of new user identity. Identity will be registered with the Fabric-CA using this name.
USER_IDENTITY=<username>

```

Registrieren eines neuen Benutzers

Um einen neuen Benutzer zu registrieren, führen Sie den folgenden Befehl aus, mit dem „registerUser.js“ ausgeführt wird. Der Befehl speichert die generierte Benutzeridentität im Wallet.

```bash
npm run registerUser -- -o $ORGNAME -u $USER_IDENTITY

```

> [!NOTE]
> Für das Ausführen des Registrierungsbefehls wird die Identität eines Administratorbenutzers verwendet. Daher muss sich die Administratorbenutzeridentität bereits im Wallet befinden, bevor dieser Befehl ausgeführt wird. Andernfalls kann der Befehl nicht erfolgreich ausgeführt werden.

Weitere Informationen zu den Argumenten, die an den Befehl übergeben werden, finden Sie in der Hilfe zum Befehl.

```bash
npm run registerUser -- -h

```

### <a name="chaincode-operations"></a>Chaincode-Vorgänge


> [!NOTE]
> Bevor Sie mit dem Ausführen von Chaincode-Vorgängen beginnen, stellen Sie sicher, dass die Anfangseinrichtung der Anwendung abgeschlossen ist.

Legen Sie die folgenden chaincodespezifischen Umgebungsvariablen für Azure Cloud Shell fest:

```bash
# peer organization name where chaincode is to be installed
ORGNAME=<orgName>
USER_IDENTITY="admin.$ORGNAME"
CC_NAME=<chaincodeName>
CC_VERSION=<chaincodeVersion>
# Language in which chaincode is written. Supported languages are 'node', 'golang' and 'java'
# Default value is 'golang'
CC_LANG=<chaincodeLanguage>
# CC_PATH contains the path where your chaincode is place. In case of go chaincode, this path is relative to 'GOPATH'.
# For example, if your chaincode is present at path '/opt/gopath/src/chaincode/chaincode.go'.
# Then, set GOPATH to '/opt/gopath' and CC_PATH to 'chaincode'
CC_PATH=<chaincodePath>
# 'GOPATH' environment variable. This needs to be set in case of go chaincode only.
export GOPATH=<goPath>
# Channel on which chaincode is to be instantiated/invoked/queried
CHANNEL=<channelName>

````

Die folgenden Chaincode-Vorgänge können ausgeführt werden:

- Installieren von Chaincode
- Instanziieren von Chaincode
- Aufrufen von Chaincode
- Abfragen von Chaincode

### <a name="install-chaincode"></a>Installieren von Chaincode

Führen Sie den folgenden Befehl aus, um Chaincode in der Peerorganisation zu installieren.

```bash
npm run installCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -p $CC_PATH -l $CC_LANG -v $CC_VERSION

```
Dieser Befehl installiert Chaincode auf allen Peerknoten der Organisation, die in der Umgebungsvariablen `ORGNAME` festgelegt sind. Gibt es in Ihrem Kanal mehrere Peerorganisationen, und möchten Sie Chaincode auf jedem dieser Kanäle installieren, führen Sie die Befehle für jede Peerorganisation separat aus.

Führen Sie diese Schritte aus:

- Legen Sie `ORGNAME` auf `<peerOrg1Name>` fest, und führen Sie den Befehl `installCC` aus.
- Legen Sie `ORGNAME` auf `<peerOrg2Name>` fest, und führen Sie den Befehl `installCC` aus.

  Gehen Sie für jede Peerorganisation so vor.

Weitere Informationen zu den Argumenten, die an den Befehl übergeben werden, finden Sie in der Hilfe zum Befehl.

```bash
npm run installCC -- -h

```

### <a name="instantiate-chaincode"></a>Instanziieren von Chaincode

Führen Sie den folgenden Befehl aus, um Chaincode auf dem Peer zu instanziieren.

```bash
npm run instantiateCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -p $CC_PATH -v $CC_VERSION -l $CC_LANG -c $CHANNEL -f <instantiateFunc> -a <instantiateFuncArgs>

```
Übergeben Sie den Namen der Instanziierungsfunktion und die Liste der Argumente (mit Kommas als Trennzeichen) in `<instantiateFunc>` bzw. `<instantiateFuncArgs>`. Möchten Sie z. B. in [fabrcar-Chaincode](https://github.com/hyperledger/fabric-samples/blob/release/chaincode/fabcar/fabcar.go) den Chaincode instanziieren, legen Sie `<instantiateFunc>` auf `"Init"` und `<instantiateFuncArgs>` auf eine leere Zeichenfolge `""` fest.

> [!NOTE]
> Führen Sie den Befehl jeweils ein Mal aus jeder Peerorganisation im Kanal aus.
> Sobald die Transaktion erfolgreich an den Anordner (Orderer) übermittelt wurde, verteilt der Anordner diese Transaktion an alle Peerorganisationen im Kanal. Somit wird der Chaincode auf allen Peerknoten in allen Peerorganisationen im Kanal instanziiert.

Weitere Informationen zu den Argumenten, die an den Befehl übergeben werden, finden Sie in der Hilfe zum Befehl.

```bash
npm run instantiateCC -- -h

```

### <a name="invoke-chaincode"></a>Aufrufen von Chaincode

Führen Sie den folgenden Befehl aus, um die Chaincode-Funktion aufzurufen:

```bash
npm run invokeCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL -f <invokeFunc> -a <invokeFuncArgs>

```
Übergeben Sie den Namen der Aufruffunktion und die Liste der Argumente (mit Kommas als Trennzeichen) in `<invokeFunction>` bzw. `<invokeFuncArgs>`. Für das fabcar-Chaincode-Beispiel heißt das, dass Sie `<invokeFunction>` auf `"initLedger"` und `<invokeFuncArgs>` auf `""` festlegen, damit die Funktion „initLedger“ aufgerufen wird.

> [!NOTE]
> Führen Sie den Befehl jeweils ein Mal aus jeder Peerorganisation im Kanal aus.
> Sobald die Transaktion erfolgreich an den Anordner (Orderer) übermittelt wurde, verteilt der Anordner diese Transaktion an alle Peerorganisationen im Kanal. Daher wird der Weltzustand (World State) auf allen Peerknoten aller Peerorganisationen im Kanal aktualisiert.

Weitere Informationen zu den Argumenten, die an den Befehl übergeben werden, finden Sie in der Hilfe zum Befehl.

```bash
npm run invokeCC -- -h

```

### <a name="query-chaincode"></a>Abfragen von Chaincode

Führen Sie den folgenden Befehl aus, um Chaincode abzufragen:

```bash
npm run queryCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL -f <queryFunction> -a <queryFuncArgs>

```

Übergeben Sie den Namen der Abfragefunktion und die Liste der Argumente (mit Kommas als Trennzeichen) in `<queryFunction>` bzw. `<queryFuncArgs>`. Nutzen Sie den `fabcar`-Chaincode nochmals als Referenz, und legen Sie `<queryFunction>` auf `"queryAllCars"` und `<queryArgs>` auf `""` fest, um alle Autos im Weltzustand abzufragen.

Weitere Informationen zu den Argumenten, die an den Befehl übergeben werden, finden Sie in der Hilfe zum Befehl.

```bash
npm run queryCC -- -h

```
