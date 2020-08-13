---
title: Hyperledger Fabric-Konsortium auf Azure Kubernetes Service (AKS)
description: Bereitstellen und Konfigurieren eines Hyperledger Fabric-Konsortiumsnetzwerks auf Azure Kubernetes Service
ms.date: 07/27/2020
ms.topic: how-to
ms.reviewer: ravastra
ms.openlocfilehash: 4bc55090234a4ab33125ba43b8416de1eadb702f
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87533426"
---
# <a name="hyperledger-fabric-consortium-on-azure-kubernetes-service-aks"></a>Hyperledger Fabric-Konsortium auf Azure Kubernetes Service (AKS)

Sie können die Vorlage für „Hyperledger Fabric (HLF) auf Azure Kubernetes Service (AKS)“ verwenden, um ein Hyperledger Fabric-Konsortiumsnetzwerks in Azure bereitzustellen und zu konfigurieren.

In diesem Artikel lernen Sie Folgendes:

- Erwerben Sie praktische Kenntnisse über Hyperledger Fabric und die verschiedenen Komponenten, die die Bausteine eines Hyperledger Fabric-Blockchainnetzwerks bilden.
- Erfahren Sie, wie Sie ein Hyperledger Fabric-Konsortium auf Azure Kubernetes Service für Ihre Produktionsszenarien bereitstellen und konfigurieren.

[!INCLUDE [Preview note](./includes/preview.md)]

## <a name="choose-an-azure-blockchain-solution"></a>Auswählen einer Azure Blockchain-Lösung

Bevor Sie sich für eine Lösungsvorlage „entscheiden, sollten Sie Ihr Szenario mit den gängigen Anwendungsfällen der verfügbaren Azure Blockchain-Optionen vergleichen.

Option | Dienstmodell | Gängiger Anwendungsfall
-------|---------------|-----------------
Lösungsvorlagen | IaaS | Lösungsvorlagen sind Azure Resource Manager-Vorlagen, die Sie verwenden können, um eine vollständig konfigurierte Blockchainnetzwerktopologie bereitzustellen. Die Vorlagen stellen Microsoft Azure-Dienste für Compute-, Netzwerk- und Speicherfunktionen für einen bestimmten Blockchainnetzwerktyp bereit und konfigurieren diese. Lösungsvorlagen werden ohne Vereinbarung zum Servicelevel bereitgestellt. Nutzen Sie die [Frageseite von Microsoft Q&A (Fragen und Antworten)](/answers/topics/azure-blockchain-workbench.html) zur Unterstützung.
[Azure Blockchain Service](../service/overview.md) | PaaS | Mit der Vorschauversion von Azure Blockchain Service wird die Einrichtung, Verwaltung und Governance von Konsortiumblockchainnetzwerken vereinfacht. Setzen Sie Azure Blockchain Service für Lösungen ein, bei denen PaaS, Konsortiumverwaltung oder Datenschutz für Vertrags- und Transaktionsaktivitäten erforderlich ist.
[Azure Blockchain Workbench](../workbench/overview.md) | IaaS und PaaS | Azure Blockchain Workbench (Vorschauversion) ist eine Sammlung mit Azure-Diensten und -Funktionen zum Erstellen und Bereitstellen von Blockchain-Anwendungen, mit denen Geschäftsprozesse und Daten mit anderen Organisationen gemeinsam genutzt werden können. Verwenden Sie Azure Blockchain Workbench, um einen Prototyp für eine Blockchainlösung oder einen Proof of Concept für eine Blockchainanwendung zu erstellen. Azure Blockchain Workbench wird ohne Vereinbarung zum Servicelevel bereitgestellt. Nutzen Sie die [Frageseite von Microsoft Q&A (Fragen und Antworten)](/answers/topics/azure-blockchain-workbench.html) zur Unterstützung.

## <a name="hyperledger-fabric-consortium-architecture"></a>Architektur eines Hyperledger Fabric-Konsortiums

Um ein Hyperledger Fabric-Netzwerk in Azure zu erstellen, müssen Sie Ordering Service (Anordnungsdienst) und eine Organisation mit Peerknoten bereitstellen. Mithilfe der Lösungsvorlage Hyperledger Fabric on Azure Kubernetes Service können Sie Knoten vom Typ „Order“ oder „Peer“ erstellen. Sie müssen die Vorlage für jeden Knoten, den Sie erstellen möchten, bereitstellen.

Die folgenden verschiedenen grundlegenden Komponenten werden im Rahmen der Vorlagenbereitstellung erstellt:

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

## <a name="deploy-the-ordererpeer-organization"></a>Bereitstellen der Anordner/Peer-Organisation

Damit Sie beginnen können, benötigen Sie ein Azure-Abonnement, das die Bereitstellung mehrerer virtueller Computer und Standardspeicherkonten unterstützen kann. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto erstellen](https://azure.microsoft.com/free/).

Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um mit der Bereitstellung der HLF-Netzwerkkomponenten zu beginnen.

1. Wählen Sie **Ressource erstellen > Blockchain** aus, und suchen Sie nach **Hyperledger Fabric auf Azure Kubernetes Service (Vorschauversion)** .

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
    - **Zertifikate**: Wenn Sie Ihre eigenen Stammzertifikate verwenden möchten, um die Fabric CA zu initialisieren, wählen Sie die Option zum Hochladen des Stammzertifikats für Fabric CA aus. Andernfalls erstellt Fabric CA standardmäßig selbstsignierte Zertifikate.
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
    - **Client-ID des Dienstprinzipals**: Geben Sie die Client-ID eines vorhandenen Dienstprinzipals ein, oder erstellen Sie einen neuen, der für die AKS-Authentifizierung erforderlich ist. Weitere Informationen finden Sie in den Schritten zum [Erstellen des Dienstprinzipals](/powershell/azure/create-azure-service-principal-azureps?view=azps-3.2.0#create-a-service-principal).
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

Um das Blockchain-Konsortium nach dem Bereitstellen des Anordnungsdiensts (Ordering service) und der Peerknoten zu erstellen, müssen Sie die weiter unten aufgeführten Schritte nacheinander ausführen. Das Azure-HLF-Skript (azhlf) unterstützt Sie beim Einrichten des Konsortiums, beim Erstellen eines Kanals und bei Chaincodevorgängen.

> [!NOTE]
> Im Skript ist ein Update enthalten. Dieses dient dazu, weitere Funktionen zusammen mit dem Azure-HLF-Skript zur Verfügung zu stellen. Wenn Sie auf das alte Skript verweisen möchten, finden Sie [hier](https://github.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/blob/master/consortiumScripts/README.md) weitere Informationen. Dieses Skript ist mit Hyperledger Fabric in der Azure Kubernetes Service-Vorlagenversion 2.0.0 und höheren Versionen kompatibel. Zur Überprüfung der Version der Bereitstellung befolgen Sie die Schritte unter [Problembehandlung](#troubleshoot).

> [!NOTE]
> Das bereitgestellte Azure-HLF-Skript (azhlf) dient nur der Unterstützung in Demonstrations- und Dev/Test-Szenarios. Der von diesem Skript erstellte Kanal und das Konsortium verfügen über grundlegende HLF-Richtlinien zur Vereinfachung des Demonstrations- und Dev/Test-Szenarios. Für die Einrichtung in der Produktion wird empfohlen, die HLF-Richtlinien für Kanal/Konsortium mithilfe der nativen HLF-APIs entsprechend den Complianceanforderungen Ihrer Organisation zu aktualisieren.


Alle Befehle zum Ausführen des Azure-HLF-Skripts können über die Bash-Befehlszeilenschnittstelle (Command Line Interface, CLI) von Azure ausgeführt werden. Sie können sich über die Option   ![Vorlage „Hyperledger Fabric auf Azure Kubernetes Service“](./media/hyperledger-fabric-consortium-azure-kubernetes-service/arrow.png) in der oberen rechten Ecke des Azure-Portals bei der Webversion der Azure-Shell anmelden. Geben Sie an der Eingabeaufforderung „bash“ und „enter“ ein, um zur bash-Befehlszeilenschnittstelle zu wechseln, oder wählen Sie *Bash* auf der Symbolleiste der Shell aus.

Weitere Informationen finden Sie unter [Übersicht über Azure Cloud Shell](../../cloud-shell/overview.md).

![Vorlage „Hyperledger Fabric auf Azure Kubernetes Service“](./media/hyperledger-fabric-consortium-azure-kubernetes-service/hyperledger-powershell.png)


Auf der folgenden Abbildung sehen Sie Schritt für Schritt den Prozess für das Erstellen eines Konsortiums zwischen einer Auftraggeber- und einer Peerorganisation. In den folgenden Abschnitten werden Befehle zum Ausführen dieser Schritte detailliert beschrieben.

![Vorlage „Hyperledger Fabric auf Azure Kubernetes Service“](./media/hyperledger-fabric-consortium-azure-kubernetes-service/process-to-build-consortium-flow-chart.png)

Vervollständigen Sie die Abschnitte für die erste Einrichtung der Clientanwendung: 

1. Herunterladen der Clientanwendungsdateien
1. Einrichten von Umgebungsvariablen
1. Importieren des Organisationsverbindungsprofils, des Administratorbenutzers und des MSP

Nach Abschluss der Anfangseinrichtung verwenden Sie die Clientanwendung, um die folgenden Vorgänge durchzuführen:  

- Kanalverwaltung
- Konsortiumsverwaltung
- Chaincode-Verwaltung

### <a name="download-client-application-files"></a>Herunterladen der Clientanwendungsdateien

Der erste Einrichtungsschritt besteht darin, die Clientanwendungsdateien herunterzuladen. Führen Sie den folgenden Befehl aus, um alle erforderlichen Dateien und Pakete herunterzuladen:

```bash-interactive
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/azhlfToolSetup.sh | bash
cd azhlfTool
npm install
npm run setup
```

Diese Befehle klonen Azure-HLF-Clientanwendungscode aus dem öffentlichen GitHub-Repository. Anschließend werden alle abhängigen npm-Pakete geladen. Nach erfolgreicher Ausführung des Befehls sehen Sie den Ordner „node_modules“ im aktuellen Verzeichnis. Alle erforderlichen Pakete werden in den Ordner „node_modules“ geladen.

### <a name="setup-environment-variables"></a>Einrichten von Umgebungsvariablen

> [!NOTE]
> Alle Umgebungsvariablen befolgen die Namenskonvention für Azure-Ressourcen.

#### <a name="set-environment-variables-for-orderer-organization-client"></a>Festlegen von Umgebungsvariablen für den Client der Auftraggeberorganisation

```bash
ORDERER_ORG_SUBSCRIPTION=<ordererOrgSubscription>
ORDERER_ORG_RESOURCE_GROUP=<ordererOrgResourceGroup>
ORDERER_ORG_NAME=<ordererOrgName>
ORDERER_ADMIN_IDENTITY="admin.$ORDERER_ORG_NAME"
CHANNEL_NAME=<channelName>
```

#### <a name="set-the-environment-variables-for-peer-organization-client"></a>Festlegen der Umgebungsvariablen für den Client der Peerorganisation

```bash
PEER_ORG_SUBSCRIPTION=<peerOrgSubscritpion>
PEER_ORG_RESOURCE_GROUP=<peerOrgResourceGroup>
PEER_ORG_NAME=<peerOrgName>
PEER_ADMIN_IDENTITY="admin.$PEER_ORG_NAME"
CHANNEL_NAME=<channelName>
```

> [!NOTE]
> Basierend auf der Anzahl der Peerorganisationen in Ihrem Konsortium müssen Sie möglicherweise die Peerbefehle wiederholen und die Umgebungsvariable entsprechend festlegen.

#### <a name="set-the-environment-variables-for-setting-up-azure-storage-account"></a>Festlegen der Umgebungsvariablen für das Einrichten eines Azure Storage-Kontos

```bash
STORAGE_SUBSCRIPTION=<subscriptionId>
STORAGE_RESOURCE_GROUP=<azureFileShareResourceGroup>
STORAGE_ACCOUNT=<azureStorageAccountName>
STORAGE_LOCATION=<azureStorageAccountLocation>
STORAGE_FILE_SHARE=<azureFileShareName>
```

Verwenden Sie die folgenden Schritte zur Erstellung eines Azure Storage-Kontos. Wenn Sie bereits ein Azure Storage-Konto erstellt haben, können Sie diese Schritte überspringen.

```bash
az account set --subscription $STORAGE_SUBSCRIPTION
az group create -l $STORAGE_LOCATION -n $STORAGE_RESOURCE_GROUP
az storage account create -n $STORAGE_ACCOUNT -g  $STORAGE_RESOURCE_GROUP -l $STORAGE_LOCATION --sku Standard_LRS
```

Verwenden Sie die folgenden Schritte, um eine Dateifreigabe in einem Azure Storage-Konto zu erstellen. Wenn Sie bereits eine Dateifreigabe erstellt haben, können Sie diese Schritte überspringen.

```bash
STORAGE_KEY=$(az storage account keys list --resource-group $STORAGE_RESOURCE_GROUP  --account-name $STORAGE_ACCOUNT --query "[0].value" | tr -d '"')
az storage share create  --account-name $STORAGE_ACCOUNT  --account-key $STORAGE_KEY  --name $STORAGE_FILE_SHARE
```

Verwenden Sie die untenstehenden Schritte, um eine Dateifreigabeverbindungszeichenfolge in Azure zu generieren.

```bash
STORAGE_KEY=$(az storage account keys list --resource-group $STORAGE_RESOURCE_GROUP  --account-name $STORAGE_ACCOUNT --query "[0].value" | tr -d '"')
SAS_TOKEN=$(az storage account generate-sas --account-key $STORAGE_KEY --account-name $STORAGE_ACCOUNT --expiry `date -u -d "1 day" '+%Y-%m-%dT%H:%MZ'` --https-only --permissions lruwd --resource-types sco --services f | tr -d '"')
AZURE_FILE_CONNECTION_STRING=https://$STORAGE_ACCOUNT.file.core.windows.net/$STORAGE_FILE_SHARE?$SAS_TOKEN

```

### <a name="import-organization-connection-profile-admin-user-identity-and-msp"></a>Importieren des Organisationsverbindungsprofils, der Identität des Administratorbenutzers und des MSP

Führen Sie die untenstehenden Befehle aus, um das Verbindungsprofil der Organisation, die Identität des Administratorbenutzers und den MSP aus dem Azure Kubernetes-Cluster abzurufen und diese Identitäten im lokalen Speicher der Clientanwendung zu speichern, also im Verzeichnis „azhlfTool/stores“.

Befehle für Auftraggeberorganisationen:

```bash
./azhlf adminProfile import fromAzure -o $ORDERER_ORG_NAME -g $ORDERER_ORG_RESOURCE_GROUP -s $ORDERER_ORG_SUBSCRIPTION
./azhlf connectionProfile import fromAzure -g $ORDERER_ORG_RESOURCE_GROUP -s $ORDERER_ORG_SUBSCRIPTION -o $ORDERER_ORG_NAME   
./azhlf msp import fromAzure -g $ORDERER_ORG_RESOURCE_GROUP -s $ORDERER_ORG_SUBSCRIPTION -o $ORDERER_ORG_NAME
```

Befehle für Peerorganisationen:

```bash
./azhlf adminProfile import fromAzure -g $PEER_ORG_RESOURCE_GROUP -s $PEER_ORG_SUBSCRIPTION -o $PEER_ORG_NAME
./azhlf connectionProfile import fromAzure -g $PEER_ORG_RESOURCE_GROUP -s $PEER_ORG_SUBSCRIPTION -o $PEER_ORG_NAME
./azhlf msp import fromAzure -g $PEER_ORG_RESOURCE_GROUP -s $PEER_ORG_SUBSCRIPTION -o $PEER_ORG_NAME
```

### <a name="create-channel-command"></a>Befehl zum Erstellen eines Kanals

Wechseln Sie zum Client der Auftraggeberorganisation, und führen Sie den Befehl zum Erstellen eines neuen Kanals aus. Dieser Befehl erstellt einen Kanal, in dem sich nur die Auftraggeberorganisation befindet.  

```bash
./azhlf channel create -c $CHANNEL_NAME -u $ORDERER_ADMIN_IDENTITY -o $ORDERER_ORG_NAME
```

### <a name="consortium-management-commands"></a>Befehle für die Konsortiumsverwaltung

>[!NOTE]
> Bevor Sie mit dem Ausführen von Konsortiumsvorgängen beginnen, stellen Sie sicher, dass die Anfangseinrichtung der Clientanwendung abgeschlossen ist.  

Führen Sie die folgenden Befehle in der angegebenen Reihenfolge aus, um eine Peerorganisation in einem Kanal und einem Konsortium hinzuzufügen.
1.  Navigieren Sie zum Client für die Peerorganisation, und laden Sie den MSP für die Peerorganisation in den Speicher in Azure hoch.

      ```bash
      ./azhlf msp export toAzureStorage -f  $AZURE_FILE_CONNECTION_STRING -o $PEER_ORG_NAME
      ```
2.  Navigieren Sie zum Client der Auftraggeberorganisation, und laden Sie den MSP für die Peerorganisation in den Speicher in Azure hoch. Führen Sie dann den Befehl aus, um die Peerorganisation im Kanal/Konsortium hinzuzufügen.

      ```bash
      ./azhlf msp import fromAzureStorage -o $PEER_ORG_NAME -f $AZURE_FILE_CONNECTION_STRING
      ./azhlf channel join -c  $CHANNEL_NAME -o $ORDERER_ORG_NAME  -u $ORDERER_ADMIN_IDENTITY -p $PEER_ORG_NAME
      ./azhlf consortium join -o $ORDERER_ORG_NAME  -u $ORDERER_ADMIN_IDENTITY -p $PEER_ORG_NAME
      ```

3.  Laden Sie auf dem Client für die Auftraggeberorganisation die Verbindungsprofile des Auftraggebers in den Speicher in Azure hoch, sodass die Peerorganisation mithilfe dieses Verbindungsprofils eine Verbindung zu den Auftraggeberknoten herstellen kann.

      ```bash
      ./azhlf connectionProfile  export toAzureStorage -o $ORDERER_ORG_NAME -f $AZURE_FILE_CONNECTION_STRING
      ```

4.  Navigieren Sie zum Client für die Peerorganisation, laden Sie das Verbindungsprofil des Auftraggebers aus dem Speicher in Azure herunter, und führen Sie dann den Befehl aus, um Peerknoten im Kanal hinzuzufügen.

      ```bash
      ./azhlf connectionProfile  import fromAzureStorage -o $ORDERER_ORG_NAME -f $AZURE_FILE_CONNECTION_STRING
      ./azhlf channel joinPeerNodes -o $PEER_ORG_NAME  -u $PEER_ADMIN_IDENTITY -c $CHANNEL_NAME --ordererOrg $ORDERER_ORG_NAME
      ```

Wenn Sie weitere Peerorganisationen im Kanal hinzufügen möchten, aktualisieren Sie die Peerumgebungsvariablen entsprechend der erforderlichen Peerorganisation, und führen Sie die Schritte 1 bis 4 aus.

### <a name="set-anchor-peers-command"></a>Befehl zum Festlegen der Ankerpeers

Wechseln Sie zum Client der Peerorganisation, und führen Sie den Befehl zum Festlegen der Ankerpeers für die im Kanal angegebene Peerorganisation aus.

>[!NOTE]
> Bevor Sie diesen Befehl ausführen, vergewissern Sie sich mithilfe von Befehlen für die Konsortiumsverwaltung, dass die Peerorganisation im Kanal hinzugefügt wurde.

```bash
./azhlf channel setAnchorPeers -c $CHANNEL_NAME -p <anchorPeersList> -o $PEER_ORG_NAME -u $PEER_ADMIN_IDENTITY --ordererOrg $ORDERER_ORG_NAME
```

`<anchorPeersList>` ist eine durch Leerzeichen getrennte Liste von Peerknoten, die als Ankerpeer festgelegt werden sollen. Beispiel:

  - Legen Sie `<anchorPeersList>` als „peer1“ fest, wenn Sie nur den peer1-Knoten als Ankerpeer festlegen möchten.
  - Legen Sie `<anchorPeersList>` als „peer1“ „peer3“ fest, wenn Sie den peer1- und den peer3-Knoten als Ankerpeer festlegen möchten.

## <a name="chaincode-management-commands"></a>Befehle für die Chaincodeverwaltung

>[!NOTE]
> Bevor Sie mit dem Ausführen von Chaincodevorgängen beginnen, stellen Sie sicher, dass die Anfangseinrichtung der Clientanwendung abgeschlossen ist.  

### <a name="set-the-below-chaincode-specific-environment-variables"></a>Festlegen der untenstehenden für den Chaincode spezifischen Umgebungsvariablen

```bash
# peer organization name where chaincode operation is to be performed
ORGNAME=<PeerOrgName>
USER_IDENTITY="admin.$ORGNAME"  
# If you are using chaincode_example02 then set CC_NAME=“chaincode_example02”
CC_NAME=<chaincodeName>  
# If you are using chaincode_example02 then set CC_VERSION=“1” for validation
CC_VERSION=<chaincodeVersion>
# Language in which chaincode is written. Supported languages are 'node', 'golang' and 'java'  
# Default value is 'golang'  
CC_LANG=<chaincodeLanguage>  
# CC_PATH contains the path where your chaincode is place.
# If you are using chaincode_example02 to validate then CC_PATH=“/home/<username>/azhlfTool/samples/chaincode/src/chaincode_example02/go”
CC_PATH=<chaincodePath>  
# Channel on which chaincode is to be instantiated/invoked/queried  
CHANNEL_NAME=<channelName>  
```

### <a name="install-chaincode"></a>Installieren von Chaincode  

Führen Sie den folgenden Befehl aus, um Chaincode in der Peerorganisation zu installieren.  

```bash
./azhlf chaincode install -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -p $CC_PATH -l $CC_LANG -v $CC_VERSION  

```
Dieser Befehl installiert Chaincode auf allen Peerknoten der Peerorganisation, die in der Umgebungsvariablen ORGNAME festgelegt sind. Gibt es in Ihrem Kanal mindestens zwei Peerorganisationen, und möchten Sie Chaincode auf jedem dieser Kanäle installieren, führen Sie die Befehle für jede Peerorganisation separat aus.  

Führen Sie diese Schritte aus:  

1.  Legen Sie `ORGNAME` und `USER_IDENTITY` als peerOrg1 fest, und führen Sie den Befehl `./azhlf chaincode install` aus.  
2.  Legen Sie `ORGNAME` und `USER_IDENTITY` als peerOrg2 fest, und führen Sie den Befehl `./azhlf chaincode install` aus.  

### <a name="instantiate-chaincode"></a>Instanziieren von Chaincode  

Navigieren Sie zur Peerclientanwendung, und führen Sie den untenstehenden Befehl aus, um Chaincode auf dem Kanal zu instanziieren.  

```bash
./azhlf chaincode instantiate -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -v $CC_VERSION -c $CHANNEL_NAME -f <instantiateFunc> --args <instantiateFuncArgs>  
```

Übergeben Sie den Namen der Instanziierungsfunktion und die Liste der Argumente mit Leerzeichen als Trennzeichen in `<instantiateFunc>` bzw. `<instantiateFuncArgs>`. Möchten Sie z. B. im Chaincode „chaincode_example02.go“ den Chaincode instanziieren, legen Sie `<instantiateFunc>` auf `init` und `<instantiateFuncArgs>` auf „a“ „2000“ „b“ „1000“ fest.

> [!NOTE]
> Führen Sie den Befehl jeweils ein Mal aus jeder Peerorganisation im Kanal aus. Sobald die Transaktion erfolgreich an den Anordner (Orderer) übermittelt wurde, verteilt der Anordner diese Transaktion an alle Peerorganisationen im Kanal. Somit wird der Chaincode auf allen Peerknoten in allen Peerorganisationen im Kanal instanziiert.  

### <a name="invoke-chaincode"></a>Aufrufen von Chaincode  

Navigieren Sie zum Peerorganisationsclient, und führen Sie den folgenden Befehl aus, um die Chaincodefunktion aufzurufen:  

```bash
./azhlf chaincode invoke -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL_NAME -f <invokeFunc> -a <invokeFuncArgs>  
```

Übergeben Sie den Namen der Aufruffunktion und die Liste der Argumente mit Leerzeichen als Trennzeichen in  `<invokeFunction>`  bzw.  `<invokeFuncArgs>` . Fahren Sie mit dem Chaincodebeispiel „chaincode_example02.go“ fort, und legen Sie  `<invokeFunction>`  auf  `invoke`  und  `<invokeFuncArgs>`  auf „a“ „b“ „10“ fest, um einen Aufrufvorgang auszuführen.  

>[!NOTE]
> Führen Sie den Befehl jeweils ein Mal aus jeder Peerorganisation im Kanal aus. Sobald die Transaktion erfolgreich an den Anordner (Orderer) übermittelt wurde, verteilt der Anordner diese Transaktion an alle Peerorganisationen im Kanal. Daher wird der Weltzustand (World State) auf allen Peerknoten aller Peerorganisationen im Kanal aktualisiert.  


### <a name="query-chaincode"></a>Abfragen von Chaincode  

Führen Sie den folgenden Befehl aus, um Chaincode abzufragen:  

```bash
./azhlf chaincode query -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL_NAME -f <queryFunction> -a <queryFuncArgs>  
```
Übergeben Sie den Namen der Abfragefunktion und die Liste der Argumente mit Leerzeichen als Trennzeichen in  `<queryFunction>`  bzw.  `<queryFuncArgs>` . Wenn nun wieder der Chaincode „chaincode_example02.go“ als Referenz verwendet wird, müssten Sie  `<queryFunction>`  auf  `query` und  `<queryArgs>` auf „a“ festlegen, um den Wert von „a“ in World State abzufragen.  

## <a name="troubleshoot"></a>Problembehandlung

**So überprüfen Sie die Version der ausgeführten Vorlage:**

Führen Sie die untenstehenden Befehle aus, um die Version Ihrer Vorlagenbereitstellung zu ermitteln.

Legen Sie die untenstehenden Umgebungsvariablen entsprechend der Ressourcengruppe fest, in der die Vorlage jeweils bereitgestellt wurde.

```bash

SWITCH_TO_AKS_CLUSTER() { az aks get-credentials --resource-group $1 --name $2 --subscription $3; }
AKS_CLUSTER_SUBSCRIPTION=<AKSClusterSubscriptionID>
AKS_CLUSTER_RESOURCE_GROUP=<AKSClusterResourceGroup>
AKS_CLUSTER_NAME=<AKSClusterName>
```
Führen Sie den untenstehenden Befehl aus, um die Vorlagenversion zu drucken.
```bash
SWITCH_TO_AKS_CLUSTER $AKS_CLUSTER_RESOURCE_GROUP $AKS_CLUSTER_NAME $AKS_CLUSTER_SUBSCRIPTION
kubectl describe pod fabric-tools -n tools | grep "Image:" | cut -d ":" -f 3

```

## <a name="support-and-feedback"></a>Support und Feedback

Neues zu Azure Blockchain: Mit dem [Azure Blockchain-Blog](https://azure.microsoft.com/blog/topics/blockchain/) bleiben Sie in Bezug auf Blockchain-Dienstangebote und Informationen vom Azure Blockchain-Technikteam immer auf dem Laufenden.

Über das [Azure-Feedbackforum für Blockchain](https://aka.ms/blockchainuservoice) können Sie Produktfeedback senden, neue Features anfordern oder über Ideen abstimmen.

### <a name="community-support"></a>Communityunterstützung

Diskutieren Sie mit Microsoft-Technikern und Azure Blockchain-Communityexperten.

- [Frageseite von Microsoft Q&A (Fragen und Antworten)](/answers/topics/azure-blockchain-workbench.html). Der technische Support für Blockchain-Vorlagen ist auf Bereitstellungsprobleme beschränkt.
- [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Blockchain/bd-p/AzureBlockchain)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-blockchain-workbench)
