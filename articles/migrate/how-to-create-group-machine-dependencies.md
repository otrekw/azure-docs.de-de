---
title: Einrichten der Abhängigkeitsvisualisierung in Azure Migrate
description: Beschreibt das Einrichten der Abhängigkeitsvisualisierung in der Serverbewertung von Azure Migrate
ms.topic: article
ms.date: 2/24/2020
ms.openlocfilehash: f5b0682adf345681c03bb6dbf14e9a0d9ba1fd58
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77589048"
---
# <a name="set-up-dependency-visualization"></a>Einrichten der Abhängigkeitsvisualisierung

In diesem Artikel wird das Einrichten der Abhängigkeitsvisualisierung in der Serverbewertung von Azure Migrate beschrieben. Mit der [Abhängigkeitsvisualisierung](concepts-dependency-visualization.md#what-is-dependency-visualization) können Sie Abhängigkeiten zwischen Computern, die Sie bewerten und zu Azure migrieren möchten, besser identifizieren und verstehen.

## <a name="before-you-start"></a>Vorbereitung

- [Überprüfen](concepts-dependency-visualization.md) Sie die Anforderungen und die verbundenen Kosten für die Abhängigkeitsvisualisierung.
- Stellen Sie sicher, dass Sie ein Azure Migrate-Projekt [erstellt](how-to-add-tool-first-time.md) haben.
- Wenn Sie bereits ein Projekt erstellt haben, vergewissern Sie sich, dass Sie das Serverbewertungstool von Azure Migrate [hinzugefügt](how-to-assess.md) haben.
- Stellen Sie sicher, dass Sie eine [Azure Migrate-Appliance](migrate-appliance.md) eingerichtet haben, mit der Ihre lokalen Computer ermittelt werden können. Erfahren Sie, wie Sie eine Appliance für [VMware](how-to-set-up-appliance-vmware.md) oder [Hyper-V](how-to-set-up-appliance-hyper-v.md) einrichten. Die Appliance ermittelt lokale Computer und sendet Metadaten und Leistungsdaten an die Serverbewertung von Azure Migrate.
- Sie müssen einem Azure Migrate-Projekt einen [Log Analytics-Arbeitsbereich](../azure-monitor/platform/manage-access.md) zuordnen, um die Abhängigkeitsvisualisierung nutzen zu können:
    - Stellen Sie sicher, dass Sie über einen Arbeitsbereich in dem Abonnement mit dem Azure Migrate-Projekt verfügen.
    - Der Arbeitsbereich muss sich in einer der Regionen „USA, Osten“, „Asien, Südosten“ oder „Europa, Westen“ befinden. Arbeitsbereiche in anderen Regionen können keinem Projekt zugeordnet werden.
    - Der Arbeitsbereich muss sich in einer Region befinden, in der die [Dienstzuordnung unterstützt wird](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites).
    - Sie können einem Azure Migrate-Projekt einen neuen oder vorhandenen Log Analytics-Arbeitsbereich zuordnen.
    - Sie hängen den Arbeitsbereich bei der ersten Einrichtung der Visualisierung von Abhängigkeiten für einen Computer an. Der Arbeitsbereich für ein Azure Migrate-Projekt kann nach dem Hinzufügen nicht mehr geändert werden.
    - In Log Analytics wird der Arbeitsbereich, der Azure Migrate zugeordnet ist, mit dem Schlüssel des Migrationsprojekts und dem Projektnamen gekennzeichnet.

- Sie können einen Arbeitsbereich erst dann anhängen, wenn Sie im Projekt Azure Migrate Computer ermittelt haben. Sie können dies tun, indem Sie eine Azure Migrate-Appliance für [VMware](how-to-set-up-appliance-vmware.md) oder [Hyper-V](how-to-set-up-appliance-hyper-v.md) einrichten. Die Appliance ermittelt lokale Computer und sendet Metadaten und Leistungsdaten an Azure Migrate: Server Assessment“ (Azure Migrate-Serverbewertung) erstellen. [Weitere Informationen](migrate-appliance.md)

## <a name="associate-a-workspace"></a>Zuordnen eines Arbeitsbereichs

1. Nachdem Sie die Computer für die Bewertung ermittelt haben, klicken Sie unter **Server** > **Azure Migrate: Serverbewertung** auf **Übersicht**.  
2. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Zusammenfassung**.
3. Klicken Sie im **OMS-Arbeitsbereich** auf **Konfiguration erforderlich**.

     ![Konfigurieren des Log Analytics-Arbeitsbereichs](./media/how-to-create-group-machine-dependencies/oms-workspace-select.png)   

4. Geben Sie unter **OMS-Arbeitsbereich konfigurieren** an, ob Sie einen neuen Arbeitsbereich erstellen oder einen vorhandenen verwenden möchten.
    - Sie können zwischen allen Arbeitsbereichen im Migrate-Projektabonnement einen vorhandenen Arbeitsbereich auswählen.
    - Sie benötigen Lesezugriff auf den Arbeitsbereich, um ihn zuordnen zu können.
5. Wenn Sie einen neuen Arbeitsbereich erstellen, wählen Sie einen Speicherort für ihn aus.

    ![Hinzufügen eines neuen Arbeitsbereichs](./media/how-to-create-group-machine-dependencies/workspace.png)


## <a name="download-and-install-the-vm-agents"></a>Herunterladen und Installieren der VM-Agents

Installieren Sie die Agents auf jedem Computer, den Sie analysieren möchten.

> [!NOTE]
    > Bei Computern, die von System Center Operations Manager 2012 R2 oder höher überwacht werden, müssen Sie den MMA-Agent nicht installieren. Die Dienstzuordnung ist in den Operations Manager integriert. [Befolgen Sie diese](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-scom#prerequisites) Anleitung zur Integration.

1. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Ermittelte Server**.
2. Klicken Sie für jeden Computer, den Sie mit der Abhängigkeitsvisualisierung analysieren möchten, in der Spalte **Abhängigkeiten** auf **Agent-Installation erforderlich**.
3. Laden Sie auf der Seite **Abhängigkeiten** MMA und Dependency-Agent für Windows oder Linux herunter.
4. Kopieren Sie unter „**MMA-Agent konfigurieren“** die Arbeitsbereichs-ID und den Schlüssel. Sie benötigen diese Angaben für die Installation des MMA-Agents.

    ![Installieren der Agents](./media/how-to-create-group-machine-dependencies/dependencies-install.png)


## <a name="install-the-mma"></a>Installieren des MMA

Installieren Sie MMA auf jedem Windows- oder Linux-Computer, den Sie analysieren möchten.

### <a name="install-mma-on-a-windows-machine"></a>Installieren von MMA auf einem Windows-Computer

Gehen Sie wie folgt vor, um den Agent auf einem Windows-Computer zu installieren:

1. Doppelklicken Sie auf den heruntergeladenen Agent.
2. Klicken Sie auf der Seite **Willkommen**auf **Weiter**. Klicken Sie auf der Seite **Lizenzbedingungen** auf **Ich stimme zu**, um die Lizenzbedingungen zu akzeptieren.
3. Behalten Sie unter **Zielordner** den Standardinstallationsordner bei, oder ändern Sie ihn, und klicken Sie anschließend auf **Weiter**.
4. Wählen Sie unter **Agent-Setupoptionen** die Optionen **Azure Log Analytics** > **Weiter**.
5. Klicken Sie auf **Hinzufügen**, um einen neuen Log Analytics-Arbeitsbereich hinzuzufügen. Fügen Sie die Arbeitsbereichs-ID und den dazugehörigen Schlüssel ein, die bzw. den Sie im Portal kopiert haben. Klicken Sie auf **Weiter**.

Sie können den Agent über die Befehlszeile oder mithilfe einer automatisierten Methode wie Configuration Manager oder [Intigua](https://go.microsoft.com/fwlink/?linkid=2104196) installieren.
- [Weitere Informationen](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) zur Verwendung dieser Methoden zum Installieren des MMA-Agent.
- Der MMA-Agent kann auch mit diesem [Skript](https://go.microsoft.com/fwlink/?linkid=2104394) installiert werden.
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) über die durch den MMA unterstützten Windows-Betriebssysteme.

### <a name="install-mma-on-a-linux-machine"></a>Installieren von MMA auf einem Linux-Computer

So installieren Sie MMA auf einem Linux-Computer

1. Übertragen Sie das entsprechende Paket (x86 oder x64) mithilfe von scp/sftp auf Ihren Linux-Computer.
2. Installieren Sie das Bundle mit dem Argument „--install“.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```

[Erfahren Sie mehr](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-linux-operating-systems) über die Liste der Unterstützungen durch den MMA für Linux-Betriebssysteme. 

## <a name="install-the-dependency-agent"></a>Installieren des Abhängigkeits-Agents

1. Doppelklicken Sie zum Installieren des Abhängigkeits-Agents auf einem Windows-Computer auf die Setupdatei, und befolgen Sie die Schritte im Assistenten.
2. Installieren Sie den Abhängigkeits-Agent auf einem Linux-Computer als „root“, indem Sie den folgenden Befehl verwenden:

    ```sh InstallDependencyAgent-Linux64.bin```

- [Erfahren Sie mehr](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-hybrid-cloud#installation-script-examples) darüber, wie Sie den Dependency-Agent mithilfe von Skripts installieren können.
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-overview#supported-operating-systems) über die vom Abhängigkeits-Agent unterstützten Betriebssysteme.


## <a name="create-a-group-using-dependency-visualization"></a>Erstellen von Gruppen mit Abhängigkeitsvisualisierung

Erstellen Sie nun eine Gruppe für die Bewertung. 


> [!NOTE]
> Gruppen, für die Sie Gruppenabhängigkeiten visualisieren möchten, sollten höchstens zehn Computer enthalten. Wenn Sie über mehr als 10 Computer verfügen, teilen Sie sie in kleinere Gruppen auf.

1. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Ermittelte Server**.
2. Klicken Sie in der Spalte **„Abhängigkeiten“** auf **„Abhängigkeiten anzeigen“** für jeden Computer, den Sie überprüfen möchten.
3. Auf dem Abhängigkeitsdiagramm sehen Sie Folgendes:
    - Eingehende (Clients) und ausgehende (Server) TCP-Verbindungen vom und zum Computer.
    - Abhängige Computer, auf denen die Dependency-Agents installiert sind, werden nach Portnummer gruppiert.
    - Abhängige Computer mit installierten Dependency-Agents werden als separate Felder angezeigt.
    - Prozesse, die auf dem Computer ausgeführt werden. Sie können die einzelnen Computerfelder erweitern, um die Prozesse anzuzeigen.
    - Computereigenschaften (einschließlich FQDN, Betriebssystem, MAC-Adresse). Klicken Sie jeweils auf ein Computerfeld, um die Details anzuzeigen.

4. Sie können Abhängigkeiten für verschiedene Zeiträume anzeigen, indem Sie auf im Zeitbereich auf die Zeitdauer klicken.
    - Standardmäßig ist ein Bereich von einer Stunde ausgewählt. 
    - Sie können den Zeitraum ändern oder das Start- und Enddatum und die Dauer angeben.
    - Der Zeitbereich kann maximal eine Stunde lang sein. Falls Sie einen längeren Bereich benötigen, können Sie Azure Monitor nutzen, um abhängige Daten für einen längeren Zeitraum abzufragen.

5. Nachdem Sie die abhängigen Computer identifiziert haben, die Sie gruppieren möchten, können Sie die gewünschten Computer mit Strg+Klick auswählen und auf **„Computer gruppieren“** klicken.
6. Geben Sie einen Gruppennamen an.
7. Stellen Sie sicher, dass die abhängigen Computer von Azure Migrate ermittelt werden.

    - Wenn ein abhängiger Computer nicht von Azure Migrate: Serverbewertung ermittelt wird, dann können Sie ihn der Gruppe nicht hinzufügen.
    - Um einen Computer hinzuzufügen, führen Sie die Ermittlung erneut durch, und vergewissern Sie sich, dass der Computer erkannt wird.

8. Wenn Sie eine Bewertung für diese Gruppe erstellen möchten, aktivieren Sie das Kontrollkästchen, um eine neue Bewertung für die Gruppe zu erstellen.
8. Klicken Sie auf **OK**, um die Gruppe zu speichern.

Nachdem Sie die Gruppe erstellt haben, empfehlen wir Ihnen, Agents auf allen Computern der Gruppe zu installieren und dann Abhängigkeiten für die gesamte Gruppe zu visualisieren.

## <a name="query-dependency-data-in-azure-monitor"></a>Abfragen von Abhängigkeitsdaten in Azure Monitor

Sie können von der Dienstzuordnung erfasste Abhängigkeitsdaten in dem Log Analytics-Arbeitsbereich abfragen, der dem Azure Migrate-Projekt zugeordnet ist. Log Analytics wird zum Schreiben und Ausführen von Azure Monitor-Protokollabfragen verwendet.

- [Erfahren Sie](../azure-monitor/insights/service-map.md#log-analytics-records), wie Sie in Log Analytics nach Dienstzuordnungsdaten suchen.
- [Verschaffen Sie sich einen Überblick](../azure-monitor/log-query/get-started-queries.md) über das Schreiben von Protokollabfragen in [Log Analytics](../azure-monitor/log-query/get-started-portal.md).

Führen Sie eine Abfrage nach Abhängigkeitsdaten wie folgt durch:

1. Navigieren Sie nach der Installation des Agents zum Portal, und klicken Sie auf **Übersicht**.
2. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Übersicht**. Klicken Sie auf den Abwärtspfeil, um **Essentials** zu erweitern.
3. Klicken Sie im **OMS-Arbeitsbereich** auf den Namen des Arbeitsbereichs.
3. Klicken Sie auf der Seite Log Analytics-Arbeitsbereich > **„Allgemein“** auf **„Protokolle“** .
4. Schreiben Sie Ihre Anfrage und klicken Sie auf **„Ausführen“** .

### <a name="sample-queries"></a>Beispielabfragen

Hier finden Sie einige Beispielabfragen, mit denen Sie Abhängigkeitsdaten extrahieren können.

- Sie können die Abfragen ändern, um Ihre bevorzugten Datenpunkte zu extrahieren.
- [Überprüfen](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records) Sie eine vollständige Liste der Datensätze der Abhängigkeiten.
- [Überprüfen](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#sample-log-searches) Sie zusätzliche Beispielabfragen.

#### <a name="sample-review-inbound-connections"></a>Beispiel: Überprüfen eingehender Verbindungen

Überprüfen Sie eingehende Verbindungen für eine Gruppe von VMs.

- Die Datensätze in der Tabelle für Verbindungsmetriken (VMConnection) stellen keine einzelnen physischen Netzwerkverbindungen dar.
- Mehrere physische Netzwerkverbindungen werden in einer logischen Verbindung gruppiert.
- [Erfahren Sie mehr darüber](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#connections), wie Daten von physischen Netzwerkverbindungen in VMConnection aggregiert werden.

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z);
VMConnection
| where Direction == 'inbound'
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(LinksEstablished) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```

#### <a name="sample-summarize-sent-and-received-data"></a>Beispiel: Zusammenfassen von gesendeten und empfangenen Daten

Dieses Beispiel zeigt eine Zusammenfassung des gesendeten und empfangenen Datenvolumens in eingehenden Verbindungen zwischen einer Gruppe von Computern an.

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z);
VMConnection
| where Direction == 'inbound'
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(BytesSent), sum(BytesReceived) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Bewertung](how-to-create-assessment.md) für eine Gruppe.


