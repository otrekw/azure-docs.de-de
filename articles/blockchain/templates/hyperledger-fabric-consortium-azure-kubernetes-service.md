---
title: Hyperledger Fabric-Konsortium auf Azure Kubernetes Service (AKS)
description: Bereitstellen und Konfigurieren eines Hyperledger Fabric-Konsortiumsnetzwerks auf Azure Kubernetes Service
ms.date: 08/06/2020
ms.topic: how-to
ms.reviewer: ravastra
ms.openlocfilehash: d23a0120aafb4dc3e6952b40959a20f9a3456614
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89226868"
---
# <a name="hyperledger-fabric-consortium-on-azure-kubernetes-service-aks"></a>Hyperledger Fabric-Konsortium auf Azure Kubernetes Service (AKS)

Sie können die Vorlage für „Hyperledger Fabric auf Azure Kubernetes Service (AKS)“ verwenden, um ein Hyperledger Fabric-Konsortiumsnetzwerk in Azure bereitzustellen und zu konfigurieren.

In diesem Artikel lernen Sie Folgendes:

- Erwerben Sie praktische Kenntnisse über Hyperledger Fabric und die Komponenten, die die Bausteine eines Hyperledger Fabric-Blockchainnetzwerks bilden.
- Erfahren Sie, wie Sie ein Hyperledger Fabric-Konsortiumnetzerk auf Azure Kubernetes Service für Ihre Produktionsszenarien bereitstellen und konfigurieren.

[!INCLUDE [Preview note](./includes/preview.md)]

## <a name="choose-an-azure-blockchain-solution"></a>Auswählen einer Azure Blockchain-Lösung

Bevor Sie eine Lösungsvorlage auswählen, sollten Sie Ihr Szenario mit den gängigen Anwendungsfällen der verfügbaren Azure Blockchain-Optionen vergleichen:

Option | Dienstmodell | Gängiger Anwendungsfall
-------|---------------|-----------------
Lösungsvorlagen | IaaS | Lösungsvorlagen sind Azure Resource Manager-Vorlagen, die Sie verwenden können, um eine vollständig konfigurierte Blockchainnetzwerktopologie bereitzustellen. Die Vorlagen stellen Microsoft Azure-Dienste für Compute-, Netzwerk- und Speicherfunktionen für einen Blockchainnetzwerktyp bereit und konfigurieren diese. Lösungsvorlagen werden ohne Vereinbarung zum Servicelevel bereitgestellt. Nutzen Sie die [Q&A-Seite von Microsoft (Fragen und Antworten)](/answers/topics/azure-blockchain-workbench.html) zur Unterstützung.
[Azure Blockchain Service](../service/overview.md) | PaaS | Mit der Vorschauversion von Azure Blockchain Service wird die Einrichtung, Verwaltung und Governance von Konsortiumblockchainnetzwerken vereinfacht. Setzen Sie Azure Blockchain Service für Lösungen ein, bei denen PaaS, Konsortiumverwaltung oder Datenschutz für Vertrags- und Transaktionsaktivitäten erforderlich ist.
[Azure Blockchain Workbench](../workbench/overview.md) | IaaS und PaaS | Azure Blockchain Workbench (Vorschauversion) ist eine Sammlung mit Azure-Diensten und -Funktionen zum Erstellen und Bereitstellen von Blockchain-Anwendungen, mit denen Geschäftsprozesse und Daten mit anderen Organisationen gemeinsam genutzt werden können. Verwenden Sie Azure Blockchain Workbench, um einen Prototyp für eine Blockchainlösung oder einen Proof of Concept für eine Blockchainanwendung zu erstellen. Azure Blockchain Workbench wird ohne Vereinbarung zum Servicelevel bereitgestellt. Nutzen Sie die [Q&A-Seite von Microsoft (Fragen und Antworten)](/answers/topics/azure-blockchain-workbench.html) zur Unterstützung.

## <a name="hyperledger-fabric-consortium-architecture"></a>Architektur eines Hyperledger Fabric-Konsortiums

Um ein Hyperledger Fabric-Netzwerk in Azure zu erstellen, müssen Sie einen Ordering Service und eine Organisation mit Peerknoten bereitstellen. Mithilfe der Lösungsvorlage „Hyperledger Fabric auf Azure Kubernetes Service“ können Sie Auftrags- oder Peerknoten erstellen. Sie müssen die Vorlage für jeden Knoten bereitstellen, den Sie erstellen möchten.

Die grundlegenden Komponenten werden im Rahmen der Vorlagenbereitstellung erstellt:

- **Auftraggeberknoten**: Ein Knoten, der für die Transaktionsanordnung im Ledger zuständig ist. Zusammen mit anderen Knoten bilden die Anordnerknoten (Orderer-Knoten, Auftraggeberknoten) den Anordnungsdienst des Hyperledger Fabric-Netzwerks.

- **Peerknoten**: Ein Knoten, der hauptsächlich Ledger und Smart Contracts hostet, die die grundlegenden Elemente des Netzwerks darstellen.

- **Fabric CA**: Die Zertifizierungsstelle (Certificate Authority, CA) für Hyperledger Fabric. Mit der Fabric CA können Sie einen Serverprozess, der die Zertifizierungsstelle hostet, initialisieren und starten. Sie ermöglicht Ihnen die Verwaltung von Identitäten und Zertifikaten. Jeder AKS-Cluster, der als Teil der Vorlage bereitgestellt wird, hat standardmäßig einen Fabric CA-Pod.

- **CouchDB oder LevelDB**: World State-Datenbanken für die Peerknoten. LevelDB ist die standardmäßige Zustandsdatenbank, die in den Peerknoten eingebettet ist. Sie speichert Chaincode-Daten als einfache Schlüssel-Wert-Paare und unterstützt nur Schlüssel- und Schlüsselbereichsabfragen sowie Abfragen für zusammengesetzte Schlüssel. CouchDB ist eine optionale alternative Zustandsdatenbank, die umfangreiche Abfragen unterstützt, wenn Chaincode-Datenwerte als JSON modelliert sind.

Die Vorlage startet bei einer Bereitstellung verschiedene Azure-Ressourcen in Ihrem Abonnement. Die bereitgestellten Azure-Ressourcen sind:

- **AKS-Cluster**: Ein Azure Kubernetes Service-Cluster, der gemäß den vom Kunden bereitgestellten Eingabeparametern konfiguriert wird. Der AKS-Cluster hat verschiedene Pods, die für das Ausführen der Hyperledger Fabric-Netzwerkkomponenten konfiguriert sind. Die erstellten Pods sind:

  - **Fabric-Tools**: Tools, die für das Konfigurieren der Hyperledger Fabric-Komponenten zuständig sind.
  - **Anordner-/Peer-Pods**: Die Knoten des Hyperledger Fabric-Netzwerks.
  - **Proxy**: Ein NGINX-Proxypod, über den die Clientanwendungen mit dem AKS-Cluster kommunizieren können.
  - **Fabric CA**: Der Pod, der die Fabric CA ausführt.
- **PostgreSQL**: Datenbankinstanz, die die Fabric CA-Identitäten beibehält.

- **Schlüsseltresor**: Instanz des Azure Key Vault-Diensts, der zum Speichern der Fabric CA-Anmeldeinformationen und der vom Kunden bereitgestellten Stammzertifikate bereitgestellt wird. Der Tresor wird bei einer Wiederholung der Vorlagenbereitstellung verwendet, um die Mechanismen der Vorlage zu verarbeiten.
- **Verwalteter Datenträger**: Eine Instanz des Azure Managed Disks-Diensts, der einen permanenten Speicher für den Ledger und die World State-Datenbank des Peerknotens bereitstellt.
- **Öffentliche IP**: Endpunkt des AKS-Clusters, der zur Kommunikation mit dem Cluster bereitgestellt wird.

## <a name="deploy-the-orderer-and-peer-organization"></a>Bereitstellen der Auftraggeber-/Peerorganisation

Damit Sie beginnen können, benötigen Sie ein Azure-Abonnement, das die Bereitstellung mehrerer virtueller Computer und Standardspeicherkonten unterstützen kann. Wenn Sie kein Azure-Abonnement haben, [können Sie ein kostenloses Azure-Konto erstellen](https://azure.microsoft.com/free/).

Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um mit der Bereitstellung der Hyperledger Fabric-Netzwerkkomponenten zu beginnen.

1. Wählen Sie **Ressource erstellen** > **Blockchain** aus, und suchen Sie nach **Hyperledger Fabric auf Azure Kubernetes Service (Vorschauversion)** .

2. Geben Sie die Projektdetails auf der Registerkarte **Grundlagen** ein.

    ![Screenshot der Registerkarte „Grundlagen“](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-basics.png)

3. Geben Sie die folgenden Details ein:
    - **Abonnement**: Wählen Sie den Namen des Abonnements aus, in dem Sie die Hyperledger Fabric-Netzwerkkomponenten bereitstellen möchten.
    - **Ressourcengruppe**: Sie können entweder eine neue Ressourcengruppe erstellen oder eine vorhandene leere Ressourcengruppe auswählen. Die Ressourcengruppe enthält alle Ressourcen, die als Teil der Vorlage bereitgestellt werden.
    - **Region**: Wählen Sie die Azure-Region aus, in der Sie den Azure Kubernetes Service-Cluster für die Hyperledger Fabric-Komponenten bereitstellen möchten. Die Vorlage ist in allen Regionen verfügbar, in denen AKS verfügbar ist. Wählen Sie eine Region aus, in der Ihr Abonnement die Kontingentgrenze für virtuelle Computer (VM) nicht erreicht.
    - **Ressourcenpräfix**: Geben Sie ein Präfix für die Benennung von Ressourcen ein, die bereitgestellt werden. Es darf höchstens fünf Zeichen lang sein, und die Kombination der Zeichen muss sowohl Zahlen als auch Buchstaben enthalten.
4. Wählen Sie die Registerkarte **Fabric-Einstellungen** aus, um die Hyperledger Fabric-Netzwerkkomponenten zu definieren, die bereitgestellt werden.

    ![Screenshot der Registerkarte „Fabric-Einstellungen“](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-settings.png)

5. Geben Sie die folgenden Details ein:
    - **Name der Organisation:** Geben Sie den Namen der Hyperledger Fabric-Organisation ein, die für verschiedene Datenebenenvorgänge erforderlich ist. Der Name der Organisation muss pro Bereitstellung eindeutig sein.
    - **Fabric-Netzwerkkomponente**: Wählen Sie entsprechend der Blockchain-Netzwerkkomponente, die Sie einrichten möchten, entweder **Ordering Service** oder **Peerknoten** aus.
    - **Anzahl von Knoten**: Es gibt die beiden folgenden Knotentypen:
        - **Ordering Service**: Wählen Sie die Anzahl der Knoten aus, um Fehlertoleranz für das Netzwerk bereitzustellen. Es werden 3, 5 und 7 Auftragsknoten unterstützt.
        - **Peerknoten**: Sie können je nach Ihren Anforderungen 1 bis 10 Knoten auswählen.
    - **Weltzustandsdatenbank für Peerknoten**: Wählen Sie zwischen LevelDB und CouchDB. Dieses Feld wird angezeigt, wenn der Benutzer in der Dropdownliste **Fabric-Netzwerkkomponente** die Option **Peerknoten** ausgewählt hat.
    - **Fabric CA-Benutzername**: Geben Sie den Benutzernamen ein, der für die Fabric CA-Authentifizierung verwendet wird.
    - **Fabric CA-Kennwort**: Geben Sie das Kennwort für die Fabric CA-Authentifizierung ein.
    - **Kennwort bestätigen**: Bestätigen Sie das Fabric CA-Kennwort.
    - **Zertifikate**: Wenn Sie Ihre eigenen Stammzertifikate verwenden möchten, um die Fabric CA zu initialisieren, wählen Sie die Option zum **Hochladen des Stammzertifikats für Fabric CA** aus. Andernfalls erstellt Fabric CA standardmäßig selbstsignierte Zertifikate.
    - **Stammzertifikat**: Laden Sie das Stammzertifikat (öffentlicher Schlüssel) hoch, mit dem Fabric CA initialisiert werden muss. Zertifikate im PEM-Format werden unterstützt. Die Zertifikate müssen gültig sein und in einer UTC-Zeitzone angegeben werden.
    - **Privater Schlüssel des Stammzertifikats**: Laden Sie den privaten Schlüssel des Stammzertifikats hoch. Wenn Sie ein PEM-Zertifikat mit kombiniertem öffentlichem und privatem Schlüssel haben, laden Sie es hier ebenfalls hoch.


6. Wählen Sie die Registerkarte **AKS-Clustereinstellungen** aus, um die Azure Kubernetes Service-Clusterkonfiguration zu definieren, die die zugrunde liegende Infrastruktur ist, in der die Hyperledger Fabric-Netzwerkkomponenten eingerichtet werden.

    ![Screenshot der Registerkarte „AKS-Clustereinstellungen“](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-aks-cluster-settings-1.png)

7. Geben Sie die folgenden Details ein:
    - **Name des Kubernetes-Clusters**: Ändern Sie ggf. den Namen des AKS-Clusters. Dieses Feld ist basierend auf dem angegebenen Ressourcenpräfix bereits ausgefüllt.
    - **Kubernetes-Version**: Wählen Sie die Version der Kubernetes-Plattform, die im Cluster bereitgestellt wird. Entsprechend der Region, die Sie auf der Registerkarte **Grundlagen** ausgewählt haben, können unterschiedliche unterstützte Versionen verfügbar sein.
    - **DNS-Präfix**: Geben Sie ein DNS-Namenspräfix (Domain Name System) für den AKS-Cluster ein. Sie verwenden DNS, um eine Verbindung mit der Kubernetes-API herzustellen, wenn Sie Container nach dem Erstellen des Clusters verwalten.
    - **Knotengröße**: Für die Größe des Kubernetes-Knotens können Sie eine Größe in der Liste der in Azure verfügbaren SKUs (Stock Keeping Units) für virtuelle Computer auswählen. Um eine optimale Leistung zu erzielen, wird Standard DS3 v2 empfohlen.
    - **Knotenanzahl**: Geben Sie die Anzahl der Kubernetes-Knoten ein, die im Cluster bereitgestellt werden sollen. Es empfiehlt sich, als Wert für „Knotenanzahl“ mindestens die Anzahl der Hyperledger Fabric-Knoten anzugeben, die auf der Registerkarte **Fabric-Einstellungen** angegeben ist.
    - **Client-ID des Dienstprinzipals**: Geben Sie die Client-ID eines vorhandenen Dienstprinzipals ein, oder erstellen Sie einen neuen Dienstprinzipal. Ein Dienstprinzipal ist für die AKS-Authentifizierung erforderlich. Informieren Sie sich über die [Schritte zum Erstellen eines Dienstprinzipals](/powershell/azure/create-azure-service-principal-azureps?view=azps-3.2.0#create-a-service-principal).
    - **Clientgeheimnis des Dienstprinzipals**: Geben Sie den geheimen Clientschlüssel des Dienstprinzipals ein, der in der Client-ID für den Dienstprinzipal bereitgestellt wird.
    - **Clientgeheimnis bestätigen**: Bestätigen Sie den geheimen Clientschlüssel des Dienstprinzipals.
    - **Containerüberwachung aktivieren**: Aktivieren Sie die AKS-Überwachung. Dies ermöglicht, die AKS-Protokolle in den angegebenen Log Analytics-Arbeitsbereich zu pushen.
    - **Log Analytics-Arbeitsbereich**: Der Log Analytics-Arbeitsbereich wird mit dem Standardarbeitsbereich aufgefüllt, der erstellt wird, wenn die Überwachung aktiviert ist.

8. Wählen Sie die Registerkarte **Überprüfen und erstellen** aus. Mit diesem Schritt wird die Validierung der Werte ausgelöst, die Sie angegeben haben.
9. Wenn die Überprüfung erfolgreich war, wählen Sie **Erstellen** aus.

    Die Bereitstellung dauert in der Regel 10 bis 12 Minuten. Die Dauer kann aber je nach Größe und Anzahl der angegebenen AKS-Knoten variieren.
10. Nach erfolgreicher Bereitstellung werden Sie in der oberen rechten Ecke über Azure-Benachrichtigungen informiert.
11. Wählen Sie **Zu Ressourcengruppe wechseln**, um alle Ressourcen zu überprüfen, die im Rahmen der Vorlagenbereitstellung erstellt wurden. Alle Ressourcennamen beginnen mit dem Präfix, das in der Registerkarte **Grundlagen** angegeben ist.

## <a name="build-the-consortium"></a>Erstellen des Konsortiums

Um das Blockchain-Konsortium nach dem Bereitstellen des Ordering Service und der Peerknoten zu erstellen, müssen Sie die folgenden Schritte nacheinander ausführen. Das Azure-Hyperledger Fabric-Skript (azhlf) unterstützt Sie beim Einrichten des Konsortiums, Erstellen des Kanals und Durchführen von Chaincodevorgängen.

> [!NOTE]
> Das Azure Hyperledger Fabric-Skript (azhlf) wurde aktualisiert, um mehr Funktionalität zu bieten. Informationen zum alten Skript finden Sie in der [Infodatei auf GitHub](https://github.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/blob/master/consortiumScripts/README.md). Dieses Skript ist mit Version 2.0.0 der Vorlage „Hyperledger Fabric auf Azure Kubernetes Service“ und höher kompatibel. Zur Überprüfung der Version der Bereitstellung befolgen Sie die Schritte unter [Problembehandlung](#troubleshoot).

> [!NOTE]
> Das Skript wird nur bereitgestellt, um Demo-, Entwicklungs- und Testszenarien zu unterstützen. Der von diesem Skript erstellte Kanal und das Konsortium verfügen über grundlegende Hyperledger Fabric-Richtlinien, um Demo-, Entwicklungs- und Testszenarien zu vereinfachen. Für die Einrichtung in der Produktion wird empfohlen, die Hyperledger Fabric-Richtlinien für Kanal/Konsortium mithilfe der nativen Hyperledger Fabric-APIs entsprechend den Complianceanforderungen Ihrer Organisation zu aktualisieren.


Alle Befehle zum Ausführen des Azure-Hyperledger Fabric-Skripts können über die Bash-Befehlszeilenschnittstelle (Command Line Interface, CLI) von Azure ausgeführt werden. Sie können sich über die Option ![Vorlage „Hyperledger Fabric auf Azure Kubernetes Service](./media/hyperledger-fabric-consortium-azure-kubernetes-service/arrow.png) in der oberen rechten Ecke des Azure-Portals bei Azure Cloud Shell anmelden. Geben Sie an der Eingabeaufforderung `bash` ein, und drücken Sie die EINGABETASTE, um zur Bash-CLI zu wechseln, oder wählen Sie **Bash** in der Cloud Shell-Symbolleiste aus.

Weitere Informationen finden Sie unter [Azure Cloud Shell](../../cloud-shell/overview.md).

![Screenshot mit Azure Cloud Shell-Befehlen](./media/hyperledger-fabric-consortium-azure-kubernetes-service/hyperledger-powershell.png)


Auf der folgenden Abbildung sehen Sie Schritt für Schritt den Prozess für das Erstellen eines Konsortiums zwischen einer Auftraggeber- und einer Peerorganisation. Die folgenden Abschnitte zeigen detaillierte Befehle zur Ausführung dieser Schritte.

![Diagramm des Prozesses zum Erstellen eines Konsortiums.](./media/hyperledger-fabric-consortium-azure-kubernetes-service/process-to-build-consortium-flow-chart.png)

Nach Abschluss der Anfangseinrichtung verwenden Sie die Clientanwendung, um die folgenden Vorgänge durchzuführen:  

- Kanalverwaltung
- Konsortiumsverwaltung
- Chaincode-Verwaltung

### <a name="download-client-application-files"></a>Herunterladen der Clientanwendungsdateien

Der erste Einrichtungsschritt besteht darin, die Clientanwendungsdateien herunterzuladen. Führen Sie die folgenden Befehle aus, um alle erforderlichen Dateien und Pakete herunterzuladen:

```bash-interactive
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/azhlfToolSetup.sh | bash
cd azhlfTool
npm install
npm run setup
```

Diese Befehle klonen Azure-Hyperledger Fabric-Clientanwendungscode aus dem öffentlichen GitHub-Repository. Anschließend werden alle abhängigen npm-Pakete geladen. Nach erfolgreicher Ausführung des Befehls sehen Sie den Ordner „node_modules“ im aktuellen Verzeichnis. Alle erforderlichen Pakete werden in den Ordner „node_modules“ geladen.

### <a name="set-up-environment-variables"></a>Einrichten von Umgebungsvariablen

Alle Umgebungsvariablen befolgen die Namenskonvention für Azure-Ressourcen.

#### <a name="set-environment-variables-for-the-orderer-organizations-client"></a>Festlegen von Umgebungsvariablen für den Client der Auftraggeberorganisation

```bash
ORDERER_ORG_SUBSCRIPTION=<ordererOrgSubscription>
ORDERER_ORG_RESOURCE_GROUP=<ordererOrgResourceGroup>
ORDERER_ORG_NAME=<ordererOrgName>
ORDERER_ADMIN_IDENTITY="admin.$ORDERER_ORG_NAME"
CHANNEL_NAME=<channelName>
```

#### <a name="set-environment-variables-for-the-peer-organizations-client"></a>Festlegen von Umgebungsvariablen für den Client der Peerorganisation

```bash
PEER_ORG_SUBSCRIPTION=<peerOrgSubscritpion>
PEER_ORG_RESOURCE_GROUP=<peerOrgResourceGroup>
PEER_ORG_NAME=<peerOrgName>
PEER_ADMIN_IDENTITY="admin.$PEER_ORG_NAME"
CHANNEL_NAME=<channelName>
```

Basierend auf der Anzahl der Peerorganisationen in Ihrem Konsortium müssen Sie möglicherweise die Peerbefehle wiederholen und die Umgebungsvariable entsprechend festlegen.

#### <a name="set-environment-variables-for-an-azure-storage-account"></a>Festlegen von Umgebungsvariablen für ein Azure-Speicherkonto

```bash
STORAGE_SUBSCRIPTION=<subscriptionId>
STORAGE_RESOURCE_GROUP=<azureFileShareResourceGroup>
STORAGE_ACCOUNT=<azureStorageAccountName>
STORAGE_LOCATION=<azureStorageAccountLocation>
STORAGE_FILE_SHARE=<azureFileShareName>
```

Erstellen Sie mit den folgenden Befehlen ein Azure-Speicherkonto. Wenn Sie bereits über ein Azure-Speicherkonto verfügen, überspringen Sie diesen Schritt.

```bash
az account set --subscription $STORAGE_SUBSCRIPTION
az group create -l $STORAGE_LOCATION -n $STORAGE_RESOURCE_GROUP
az storage account create -n $STORAGE_ACCOUNT -g  $STORAGE_RESOURCE_GROUP -l $STORAGE_LOCATION --sku Standard_LRS
```

Erstellen Sie mit den folgenden Befehlen eine Dateifreigabe im Azure-Speicherkonto. Wenn Sie bereits eine Dateifreigabe verwenden, überspringen Sie diesen Schritt.

```bash
STORAGE_KEY=$(az storage account keys list --resource-group $STORAGE_RESOURCE_GROUP  --account-name $STORAGE_ACCOUNT --query "[0].value" | tr -d '"')
az storage share create  --account-name $STORAGE_ACCOUNT  --account-key $STORAGE_KEY  --name $STORAGE_FILE_SHARE
```

Generieren Sie mit folgenden Befehlen eine Verbindungszeichenfolge für eine Azure-Dateifreigabe.

```bash
STORAGE_KEY=$(az storage account keys list --resource-group $STORAGE_RESOURCE_GROUP  --account-name $STORAGE_ACCOUNT --query "[0].value" | tr -d '"')
SAS_TOKEN=$(az storage account generate-sas --account-key $STORAGE_KEY --account-name $STORAGE_ACCOUNT --expiry `date -u -d "1 day" '+%Y-%m-%dT%H:%MZ'` --https-only --permissions lruwd --resource-types sco --services f | tr -d '"')
AZURE_FILE_CONNECTION_STRING=https://$STORAGE_ACCOUNT.file.core.windows.net/$STORAGE_FILE_SHARE?$SAS_TOKEN

```

### <a name="import-an-organization-connection-profile-admin-user-identity-and-msp"></a>Importieren eines Organisationsverbindungsprofils, einer Administratorbenutzeridentität und eines MSP

Verwenden Sie die folgenden Befehle, um das Verbindungsprofil der Organisation, die Administratorbenutzeridentität und den Managed Service Provider (MSP) aus dem Azure Kubernetes Service-Cluster abzurufen und diese Identitäten im lokalen Speicher der Clientanwendung zu speichern. Ein Beispiel für einen lokalen Speicher ist das Verzeichnis *azhlfTool/stores*.

Für die Auftraggeberorganisation:

```bash
./azhlf adminProfile import fromAzure -o $ORDERER_ORG_NAME -g $ORDERER_ORG_RESOURCE_GROUP -s $ORDERER_ORG_SUBSCRIPTION
./azhlf connectionProfile import fromAzure -g $ORDERER_ORG_RESOURCE_GROUP -s $ORDERER_ORG_SUBSCRIPTION -o $ORDERER_ORG_NAME   
./azhlf msp import fromAzure -g $ORDERER_ORG_RESOURCE_GROUP -s $ORDERER_ORG_SUBSCRIPTION -o $ORDERER_ORG_NAME
```

Für die Peerorganisation:

```bash
./azhlf adminProfile import fromAzure -g $PEER_ORG_RESOURCE_GROUP -s $PEER_ORG_SUBSCRIPTION -o $PEER_ORG_NAME
./azhlf connectionProfile import fromAzure -g $PEER_ORG_RESOURCE_GROUP -s $PEER_ORG_SUBSCRIPTION -o $PEER_ORG_NAME
./azhlf msp import fromAzure -g $PEER_ORG_RESOURCE_GROUP -s $PEER_ORG_SUBSCRIPTION -o $PEER_ORG_NAME
```

### <a name="create-a-channel"></a>Erstellen eines Kanals

Verwenden Sie den folgenden Befehl vom Client der Auftraggeberorganisation aus, um einen Kanal zu erstellen, der nur die Auftraggeberorganisation enthält.  

```bash
./azhlf channel create -c $CHANNEL_NAME -u $ORDERER_ADMIN_IDENTITY -o $ORDERER_ORG_NAME
```

### <a name="add-a-peer-organization-for-consortium-management"></a>Hinzufügen einer Peerorganisation für die Konsortiumverwaltung

>[!NOTE]
> Bevor Sie mit dem Ausführen von Konsortiumsvorgängen beginnen, stellen Sie sicher, dass Sie die Anfangseinrichtung der Clientanwendung abgeschlossen haben.  

Führen Sie die folgenden Befehle in der angegebenen Reihenfolge aus, um eine Peerorganisation in einem Kanal und einem Konsortium hinzuzufügen: 

1.  Laden Sie vom Client der Peerorganisation aus den MSP der Peerorganisation in Azure Storage hoch.

      ```bash
      ./azhlf msp export toAzureStorage -f  $AZURE_FILE_CONNECTION_STRING -o $PEER_ORG_NAME
      ```
2.  Laden Sie vom Client der Auftraggeberorganisation aus den MSP der Peerorganisation aus Azure Storage herunter. Führen Sie dann den Befehl zum Hinzufügen der Peerorganisation in Kanal und Konsortium aus.

      ```bash
      ./azhlf msp import fromAzureStorage -o $PEER_ORG_NAME -f $AZURE_FILE_CONNECTION_STRING
      ./azhlf channel join -c  $CHANNEL_NAME -o $ORDERER_ORG_NAME  -u $ORDERER_ADMIN_IDENTITY -p $PEER_ORG_NAME
      ./azhlf consortium join -o $ORDERER_ORG_NAME  -u $ORDERER_ADMIN_IDENTITY -p $PEER_ORG_NAME
      ```

3.  Laden Sie vom Client der Auftraggeberorganisation aus das Verbindungsprofil des Auftraggebers in Azure Storage hoch, sodass die Peerorganisation mithilfe dieses Verbindungsprofils eine Verbindung zu den Auftraggeberknoten herstellen kann.

      ```bash
      ./azhlf connectionProfile  export toAzureStorage -o $ORDERER_ORG_NAME -f $AZURE_FILE_CONNECTION_STRING
      ```

4.  Laden Sie vom Client der Peerorganisation aus das Verbindungsprofil des Auftraggebes aus Azure Storage herunter. Führen Sie dann den Befehl zum Hinzufügen von Peerknoten im Kanal aus.

      ```bash
      ./azhlf connectionProfile  import fromAzureStorage -o $ORDERER_ORG_NAME -f $AZURE_FILE_CONNECTION_STRING
      ./azhlf channel joinPeerNodes -o $PEER_ORG_NAME  -u $PEER_ADMIN_IDENTITY -c $CHANNEL_NAME --ordererOrg $ORDERER_ORG_NAME
      ```

Wenn Sie weitere Peerorganisationen im Kanal hinzufügen möchten, aktualisieren Sie die Peerumgebungsvariablen entsprechend der erforderlichen Peerorganisation, und führen Sie die Schritte 1 bis 4 erneut aus.

### <a name="set-anchor-peers"></a>Festlegen von Ankerpeers

Führen Sie vom Client der Peerorganisation aus den Befehl zum Festlegen der Ankerpeers für die im Kanal angegebene Peerorganisation aus.

>[!NOTE]
> Bevor Sie diesen Befehl ausführen, vergewissern Sie sich mithilfe von Befehlen für die Konsortiumsverwaltung, dass die Peerorganisation im Kanal hinzugefügt wurde.

```bash
./azhlf channel setAnchorPeers -c $CHANNEL_NAME -p <anchorPeersList> -o $PEER_ORG_NAME -u $PEER_ADMIN_IDENTITY --ordererOrg $ORDERER_ORG_NAME
```

`<anchorPeersList>` ist eine durch Leerzeichen getrennte Liste von Peerknoten, die als Ankerpeer festgelegt werden sollen. Beispiel:

  - Legen Sie `<anchorPeersList>` als `"peer1"` fest, wenn Sie nur den peer1-Knoten als Ankerpeer festlegen möchten.
  - Legen Sie `<anchorPeersList>` als `"peer1" "peer3"` fest, wenn Sie den peer1- und den peer3-Knoten als Ankerpeer festlegen möchten.

## <a name="chaincode-management-commands"></a>Befehle für die Chaincodeverwaltung

>[!NOTE]
> Bevor Sie mit dem Ausführen von Chaincodevorgängen beginnen, stellen Sie sicher, dass Sie die Anfangseinrichtung der Clientanwendung abgeschlossen haben.  

### <a name="set-the-chaincode-specific-environment-variables"></a>Festlegen der chaincodespezifischen Umgebungsvariablen

```bash
# Peer organization name where the chaincode operation will be performed
ORGNAME=<PeerOrgName>
USER_IDENTITY="admin.$ORGNAME"  
# If you are using chaincode_example02 then set CC_NAME=“chaincode_example02”
CC_NAME=<chaincodeName>  
# If you are using chaincode_example02 then set CC_VERSION=“1” for validation
CC_VERSION=<chaincodeVersion>
# Language in which chaincode is written. Supported languages are 'node', 'golang', and 'java'  
# Default value is 'golang'  
CC_LANG=<chaincodeLanguage>  
# CC_PATH contains the path where your chaincode is placed.
# If you are using chaincode_example02 to validate then CC_PATH=“/home/<username>/azhlfTool/samples/chaincode/src/chaincode_example02/go”
CC_PATH=<chaincodePath>  
# Channel on which chaincode will be instantiated/invoked/queried  
CHANNEL_NAME=<channelName>  
```

### <a name="install-chaincode"></a>Installieren von Chaincode  

Führen Sie den folgenden Befehl aus, um Chaincode in der Peerorganisation zu installieren.  

```bash
./azhlf chaincode install -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -p $CC_PATH -l $CC_LANG -v $CC_VERSION  

```
Der Befehl installiert Chaincode auf allen Peerknoten der Peerorganisation, die in der Umgebungsvariablen `ORGNAME` festgelegt sind. Gibt es in Ihrem Kanal mindestens zwei Peerorganisationen, und möchten Sie Chaincode auf jedem dieser Kanäle installieren, führen Sie diesen Befehl für jede Peerorganisation separat aus.  

Folgen Sie diesen Schritten:  

1.  Legen Sie `ORGNAME` und `USER_IDENTITY` gemäß `peerOrg1` fest, und führen Sie den Befehl `./azhlf chaincode install` aus.  
2.  Legen Sie `ORGNAME` und `USER_IDENTITY` gemäß `peerOrg2` fest, und führen Sie den Befehl `./azhlf chaincode install` aus.  

### <a name="instantiate-chaincode"></a>Instanziieren von Chaincode  

Führen Sie von der Peerclientanwendung aus den folgenden Befehl aus, um Chaincode auf dem Kanal zu instanziieren.  

```bash
./azhlf chaincode instantiate -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -v $CC_VERSION -c $CHANNEL_NAME -f <instantiateFunc> --args <instantiateFuncArgs>
```

Übergeben Sie den Namen der Instanziierungsfunktion und die Liste der Argumente mit Leerzeichen als Trennzeichen in `<instantiateFunc>` bzw. `<instantiateFuncArgs>`. Möchten Sie z. B. Chaincode „chaincode_example02.go“ instanziieren, legen Sie `<instantiateFunc>` auf `init` und `<instantiateFuncArgs>` auf `"a" "2000" "b" "1000"` fest.

Sie können die JSON-Datei für die Sammlungskonfiguration auch mit dem `--collections-config`-Flag übergeben. Alternativ legen Sie die vorübergehenden Argumente mit dem `-t`-Flag fest, während Sie einen für private Transaktionen verwendeten Chaincode instanziieren.

Beispiel:

```bash
./azhlf chaincode instantiate -c $CHANNEL_NAME -n $CC_NAME -v $CC_VERSION -o $ORGNAME -u $USER_IDENTITY --collections-config <collectionsConfigJSONFilePath>
./azhlf chaincode instantiate -c $CHANNEL_NAME -n $CC_NAME -v $CC_VERSION -o $ORGNAME -u $USER_IDENTITY --collections-config <collectionsConfigJSONFilePath> -t <transientArgs>
```

Der `<collectionConfigJSONFilePath>`-Teil ist der Pfad zu der JSON-Datei, die die für die Instanziierung eines privaten Datenchaincodes definierten Sammlungen enthält. Sie finden eine JSON-Beispieldatei für die Sammlungskonfiguration relativ zum Verzeichnis *azhlfTool* im folgenden Pfad: `./samples/chaincode/src/private_marbles/collections_config.json`.
Übergeben Sie `<transientArgs>` als gültigen JSON-Code im Zeichenfolgenformat. Versehen Sie Sonderzeichen mit Escapezeichen. Beispiel: `'{\\\"asset\":{\\\"name\\\":\\\"asset1\\\",\\\"price\\\":99}}'`

> [!NOTE]
> Führen Sie den Befehl jeweils ein Mal aus jeder Peerorganisation im Kanal aus. Sobald die Transaktion erfolgreich an den Auftraggeber übermittelt wurde, verteilt der Auftraggeber diese Transaktion an alle Peerorganisationen im Kanal. Der Chaincode wird dann auf allen Peerknoten in allen Peerorganisationen im Kanal instanziiert.  

### <a name="invoke-chaincode"></a>Aufrufen von Chaincode  

Führen Sie vom Client der Peerorganisation aus den folgenden Befehl aus, um die Chaincodefunktion aufzurufen:  

```bash
./azhlf chaincode invoke -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL_NAME -f <invokeFunc> -a <invokeFuncArgs>  
```

Übergeben Sie den Namen der Aufruffunktion und die Liste der Argumente mit Leerzeichen als Trennzeichen in `<invokeFunction>` bzw. `<invokeFuncArgs>` . Fahren Sie mit dem Chaincodebeispiel „chaincode_example02.go“ fort, und legen Sie `<invokeFunction>` auf `invoke` und `<invokeFuncArgs>` auf `"a" "b" "10"` fest, um einen Aufrufvorgang auszuführen.  

>[!NOTE]
> Führen Sie den Befehl jeweils ein Mal aus jeder Peerorganisation im Kanal aus. Sobald die Transaktion erfolgreich an den Auftraggeber übermittelt wurde, verteilt der Auftraggeber diese Transaktion an alle Peerorganisationen im Kanal. Dann wird der World State auf allen Peerknoten aller Peerorganisationen im Kanal aktualisiert.  


### <a name="query-chaincode"></a>Abfragen von Chaincode  

Führen Sie zum Abfragen von Chaincode folgenden Befehl aus:  

```bash
./azhlf chaincode query -o $ORGNAME -p <endorsingPeers> -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL_NAME -f <queryFunction> -a <queryFuncArgs> 
```
Unterstützende Peers sind Peers, bei denen Chaincode installiert ist und zur Ausführung von Transaktionen aufgerufen wird. Sie müssen festlegen, dass `<endorsingPeers>` die Namen der Peerknoten aus der aktuellen Peerorganisation enthält. Listen Sie die unterstützenden Peers für die angegebene Kombination aus Chaincode und Kanal durch Leerzeichen getrennt auf. Beispiel: `-p "peer1" "peer3"`.

Wenn Sie Chaincode mithilfe von *azhlfTool* installieren, übergeben Sie alle Peerknotennamen als Wert an das unterstützende Peerargument. Chaincode wird auf jedem Peerknoten für diese Organisation installiert. 

Übergeben Sie den Namen der Abfragefunktion und die Liste der Argumente mit Leerzeichen als Trennzeichen in `<queryFunction>` bzw. `<queryFuncArgs>` . Wenn nun wieder der Chaincode „chaincode_example02.go“ als Referenz verwendet wird, müssen Sie `<queryFunction>` auf `query` und `<queryArgs>` auf `"a"` festlegen, um den Wert von „a“ im World State abzufragen.  

## <a name="troubleshoot"></a>Problembehandlung

Führen Sie die folgenden Befehle aus, um die Version Ihrer Vorlagenbereitstellung zu ermitteln.

Legen Sie die Umgebungsvariablen entsprechend der Ressourcengruppe fest, in der die Vorlage jeweils bereitgestellt wurde.

```bash

SWITCH_TO_AKS_CLUSTER() { az aks get-credentials --resource-group $1 --name $2 --subscription $3; }
AKS_CLUSTER_SUBSCRIPTION=<AKSClusterSubscriptionID>
AKS_CLUSTER_RESOURCE_GROUP=<AKSClusterResourceGroup>
AKS_CLUSTER_NAME=<AKSClusterName>
```
Führen Sie den folgenden Befehl aus, um die Vorlagenversion zu drucken.

```bash
SWITCH_TO_AKS_CLUSTER $AKS_CLUSTER_RESOURCE_GROUP $AKS_CLUSTER_NAME $AKS_CLUSTER_SUBSCRIPTION
kubectl describe pod fabric-tools -n tools | grep "Image:" | cut -d ":" -f 3

```

## <a name="support-and-feedback"></a>Support und Feedback

Mit dem [Azure Blockchain-Blog](https://azure.microsoft.com/blog/topics/blockchain/) bleiben Sie in Bezug auf Blockchain-Dienstangebote und Informationen vom Azure Blockchain-Technikteam immer auf dem Laufenden.

Über das [Azure-Feedbackforum für Blockchain](https://aka.ms/blockchainuservoice) können Sie Produktfeedback senden, neue Features anfordern oder über Ideen abstimmen.

### <a name="community-support"></a>Communityunterstützung

Diskutieren Sie mit Microsoft-Technikern und Azure Blockchain-Communityexperten:

- [Q&A-Seite von Microsoft](/answers/topics/azure-blockchain-workbench.html) 
   
  Der technische Support für Blockchain-Vorlagen ist auf Bereitstellungsprobleme beschränkt.
- [Technische Microsoft-Community](https://techcommunity.microsoft.com/t5/Blockchain/bd-p/AzureBlockchain)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-blockchain-workbench)
