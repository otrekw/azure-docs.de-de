---
title: Bewerten von Servern mithilfe der Azure Migrate-Serverbewertung anhand von importierten Serverdaten
description: Es wird beschrieben, wie Sie lokale Server für die Migration zu Azure bewerten, indem Sie die Azure Migrate-Serverbewertung und importierte Daten verwenden.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 10/23/2019
ms.author: raynew
ms.openlocfilehash: 40dd81b9eb7ff2b20abb1bf3ea4337b63fb2a831
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2020
ms.locfileid: "87447072"
---
# <a name="assess-servers-by-using-imported-data"></a>Bewerten von Servern anhand von importierten Daten

In diesem Artikel erfahren Sie, wie Sie lokale Server mit der [Azure Migrate-Serverbewertung](migrate-services-overview.md#azure-migrate-server-assessment-tool) bewerten, indem Sie Servermetadaten im CSV-Format importieren. Bei dieser Bewertungsmethode muss keine Azure Migrate-Appliance für die Bewertungserstellung eingerichtet werden. Sie ist in folgenden Szenarien hilfreich:

- Sie möchten schnell eine anfängliche Bewertung erstellen, bevor Sie die Appliance bereitstellen.
- Sie können die Azure Migrate-Appliance nicht in Ihrer Organisation bereitstellen.
- Sie können keine Anmeldeinformationen freigeben, die den Zugriff auf lokale Server ermöglichen.
- Aufgrund von Sicherheitsbeschränkungen wird verhindert, dass Sie von der Appliance erfasste Daten sammeln und an Azure senden. Sie können steuern, welche Daten in einer importierten Datei weitergegeben werden. Darüber hinaus ist ein Großteil der Daten optional (beispielsweise die Angabe von IP-Adressen).

## <a name="before-you-start"></a>Vorbereitung

Beachten Sie die folgenden Punkte:

- In einer einzelnen CSV-Datei können bis zu 20.000 Server hinzugefügt werden.
- In einem Azure Migrate-Projekt können per CSV-Datei bis zu 20.000 Server hinzugefügt werden.
- Serverinformationen können per CSV-Datei mehrmals in die Serverbewertung hochgeladen werden.
- Die Erfassung von App-Informationen ist nützlich, wenn Sie Ihre lokale Umgebung für die Migration bewerten. Mit der Serverbewertung wird derzeit aber keine Bewertung auf Anwendungsebene durchgeführt, und Anwendungen werden bei der Bewertungserstellung nicht berücksichtigt.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Einrichten eines Azure Migrate-Projekts
> * Fügen Sie die Serverinformationen in eine CSV-Datei ein.
> * Importieren der Datei, um der Serverbewertung Serverinformationen hinzuzufügen
> * Erstellen und überprüfen Sie eine Bewertung.

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell einen Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in den Schrittanleitungen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.

## <a name="set-azure-permissions-for-azure-migrate"></a>Festlegen von Azure-Berechtigungen für Azure Migrate

Ihr Azure-Konto benötigt Berechtigungen zum Erstellen eines Azure Migrate-Projekts.

1. Öffnen Sie im Azure-Portal das Abonnement, und wählen Sie **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** nach dem relevanten Konto, und wählen Sie es aus, um Berechtigungen anzuzeigen.
3. Vergewissern Sie sich, dass Sie über die Berechtigung **Mitwirkender** oder **Besitzer** verfügen.
    - Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements.
    - Wenn Sie nicht der Besitzer des Abonnements sind, müssen Sie mit dem Besitzer zusammenarbeiten, um die Rolle zuzuweisen.

## <a name="set-up-an-azure-migrate-project"></a>Einrichten eines Azure Migrate-Projekts

So richten Sie ein neues Azure Migrate-Projekt ein:

1. Suchen Sie im Azure-Portal unter **Alle Dienste** nach **Azure Migrate**.
2. Wählen Sie unter **Dienste** die Option **Azure Migrate** aus.
3. Wählen Sie in der **Übersicht** unter **Server ermitteln, bewerten und migrieren** die Option **Server bewerten und migrieren** aus.

    ![Ermitteln und Bewerten von Servern](./media/tutorial-assess-import/assess-migrate.png)

4. Wählen Sie unter **Erste Schritte** die Option **Tools hinzufügen** aus.
5. Wählen Sie unter **Projekt migrieren** Ihr Azure-Abonnement aus, und erstellen Sie bei Bedarf eine Ressourcengruppe.
6. Geben Sie unter **Projektdetails** den Projektnamen und die geografische Region an, in der Sie das Projekt erstellen möchten. Weitere Informationen finden Sie unter:

    - Beachten Sie die unterstützten geografischen Regionen für [öffentliche Clouds](migrate-support-matrix.md#supported-geographies-public-cloud) und [Azure Government-Clouds](migrate-support-matrix.md#supported-geographies-azure-government).
    - Beim Ausführen einer Migration kann eine beliebige Zielregion ausgewählt werden.

    ![Erstellen eines Azure Migrate-Projekts](./media/tutorial-assess-import/migrate-project.png)

7. Wählen Sie **Weiter** aus.
8. Wählen Sie unter **Bewertungstool auswählen** Folgendes aus: **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) > **Weiter**.

    ![Erstellen einer Azure Migrate-Bewertung](./media/tutorial-assess-import/assessment-tool.png)

9. Wählen Sie unter **Migrationstool auswählen** die Option **Hinzufügen eines Migrationstools vorerst überspringen** >  und anschließend **Weiter** aus.
10. Überprüfen Sie die Einstellungen unter **Überprüfen + Tools hinzufügen**, und wählen Sie **Tools hinzufügen** aus.
11. Warten Sie einige Minuten, bis das Azure Migrate-Projekt bereitgestellt wurde. Anschließend werden Sie zur Projektseite weitergeleitet. Sollte das Projekt nicht angezeigt werden, können Sie auf dem Azure Migrate-Dashboard unter **Server** darauf zugreifen.

## <a name="prepare-the-csv"></a>Vorbereiten der CSV-Datei

Laden Sie die CSV-Vorlage herunter, und fügen Sie ihr Serverinformationen hinzu.

### <a name="download-the-template"></a>Herunterladen der Vorlage

1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Ermitteln**.
2. Wählen Sie unter **Computer ermitteln** die Option **Über CSV importieren** aus.
3. Wählen Sie **Herunterladen** aus, um die CSV-Vorlage herunterzuladen. Alternativ können Sie den [Download direkt durchführen](https://go.microsoft.com/fwlink/?linkid=2109031).

    ![Herunterladen der CSV-Vorlage](./media/tutorial-assess-import/download-template.png)

### <a name="add-server-information"></a>Hinzufügen von Serverinformationen

Sammeln Sie Serverdaten, und fügen Sie sie der CSV-Datei hinzu.

- Zum Sammeln von Daten können Sie diese aus Tools exportieren, die Sie für die lokale Serververwaltung nutzen. Beispiele wären etwa VMware vSphere oder Ihre Konfigurationsverwaltungsdatenbank (CMDB).
- Laden Sie unsere [Beispieldatei](https://go.microsoft.com/fwlink/?linkid=2108405) herunter, um sich die Beispieldaten anzusehen.

In der folgenden Tabelle sind die auszufüllenden Felder zusammengefasst:

**Feldname** | **Obligatorisch** | **Details**
--- | --- | ---
**Servername** | Ja | Wir empfehlen, den vollqualifizierten Domänennamen anzugeben.
**IP-Adresse** | Nein | Serveradresse
**Kerne** | Ja | Anzahl von Prozessorkernen, die dem Server zugeordnet sind
**Memory** | Ja | Gesamter Arbeitsspeicher, der dem Server zugeordnet ist (in MB)
**OS name** (Betriebssystemname) | Ja | Serverbetriebssystem <br/> Betriebssystemnamen, die den Namen in [dieser Liste](#supported-operating-system-names) entsprechen oder diese Namen enthalten, werden von der Bewertung erkannt.
**Betriebssystemversion** | Nein | Betriebssystemversion des Servers
**Betriebssystemarchitektur** | Nein | Architektur des Serverbetriebssystems <br/> Gültige Werte: x64, x86, amd64, 32-Bit oder 64-Bit
**Anzahl der Datenträger** | Nein | Nicht erforderlich, wenn Details zu den einzelnen Datenträgern angegeben werden.
**Disk 1 size** (Größe von Datenträger 1)  | Nein | Die maximale Größe des Datenträgers (in GB).<br/>Sie können Details zu weiteren Datenträgern hinzufügen, indem Sie in der Vorlage [Spalten hinzufügen](#add-multiple-disks). Es ist möglich, bis zu acht Datenträger hinzuzufügen.
**Disk 1 read ops** (Lesevorgänge für Datenträger 1) | Nein | Datenträger-Lesevorgänge pro Sekunde
**Disk 1 write ops** (Schreibvorgänge für Datenträger 1) | Nein | Datenträger-Schreibvorgänge pro Sekunde
**Disk 1 read throughput** (Lesedurchsatz für Datenträger 1) | Nein | Vom Datenträger gelesenen Daten pro Sekunde (in MB pro Sekunde)
**Disk 1 write throughput** (Schreibdurchsatz für Datenträger 1) | Nein | Daten, die pro Sekunde auf den Datenträger geschrieben werden (in MB pro Sekunde)
**Prozentsatz der CPU-Auslastung** | Nein | CPU-Auslastung in Prozent
**Memory utilization percentage** (Prozentuale Auslastung des Arbeitsspeichers) | Nein | Auslastung des Arbeitsspeichers in Prozent
**Total disks read ops** (Gesamte Lesevorgänge für Datenträger) | Nein | Datenträger-Lesevorgänge pro Sekunde
**Total disks write ops** (Gesamte Schreibvorgänge für Datenträger) | Nein | Datenträger-Schreibvorgänge pro Sekunde
**Total disks read throughput** (Gesamter Lesedurchsatz für Datenträger) | Nein | Vom Datenträger gelesene Daten (in MB pro Sekunde)
**Total disks write throughput** (Gesamter Schreibdurchsatz für Datenträger) | Nein | Auf den Datenträger geschriebene Daten (in MB pro Sekunden)
**Network In throughput** (Eingehender Netzwerkdurchsatz) | Nein | Vom Server empfangene Daten (in MB pro Sekunde)
**Network Out throughput** (Ausgehender Netzwerkdurchsatz) | Nein | Vom Server übertragene Daten (in MB pro Sekunde)
**Firmware type** (Firmwaretyp) | Nein | Serverfirmware. Mögliche Werte: „BIOS“ und „UEFI“.
**MAC address** (MAC-Adresse)| Nein | MAC-Adresse des Servers


### <a name="add-operating-systems"></a>Hinzufügen von Betriebssystemen

Bei der Bewertung werden bestimmte Betriebssystemnamen erkannt. Alle von Ihnen angegebenen Namen müssen exakt einer der Zeichenfolgen aus der [Liste mit den unterstützten Namen](#supported-operating-system-names) entsprechen.

### <a name="add-multiple-disks"></a>Hinzufügen mehrerer Datenträger

Die Vorlage enthält Standardfelder für den ersten Datenträger. Sie können ähnliche Spalten für bis zu acht Datenträger hinzufügen.

Fügen Sie beispielsweise die folgenden Spalten hinzu, um alle Felder für einen zweiten Datenträger anzugeben:

- „Disk 2 size“ (Größe von Datenträger 2)
- „Disk 2 read ops“ (Lesevorgänge für Datenträger 2)
- „Disk 2 write ops“ (Schreibvorgänge für Datenträger 2)
- „Disk 2 read throughput“ (Lesedurchsatz für Datenträger 2)
- „Disk 2 write throughput“ (Schreibdurchsatz für Datenträger 2)


## <a name="import-the-server-information"></a>Importieren der Serverinformationen

Importieren Sie die Server in die Serverbewertung, nachdem Sie der CSV-Vorlage Informationen hinzugefügt haben.

1. Navigieren Sie in Azure Migrate unter **Computer ermitteln** zu der fertigen Vorlage.
2. Wählen Sie **Importieren** aus.
3. Der Importstatus wird angezeigt.
    - Sollten Warnungen vorliegen, können Sie diese entweder beheben oder ignorieren.
    - Optimieren Sie die Serverinformationen gemäß den Vorschlägen in den Warnungen, um die Genauigkeit von Bewertungen zu verbessern.
    - Wählen Sie zum Anzeigen und Beheben von Warnungen die Option **Download warning details .CSV** (CSV mit Warnungsdetails herunterladen) aus. Dadurch wird die CSV-Datei mit Warnungen heruntergeladen. Überprüfen Sie die Warnungen, und beheben Sie sie bei Bedarf.
    - Sollten Fehler vorliegen (Importstatus: **Fehler**), müssen Sie diese beheben, um den Importvorgang fortsetzen zu können.
        1. Laden Sie die CSV-Datei herunter. Diese enthält nun Fehlerdetails.
        1. Überprüfen und beheben Sie die Fehler nach Bedarf. 
        1. Laden Sie die geänderte Datei erneut hoch.
4. Wenn der Importstatus **Abgeschlossen** lautet, wurden die Serverinformationen importiert.

## <a name="update-server-information"></a>Aktualisieren von Serverinformationen

Sie können die Informationen für einen Server aktualisieren, indem Sie die Daten für den Server mit dem gleichen Wert für **Servername** erneut hochladen. Das Feld **Servername** kann nicht geändert werden. Das Löschen von Servern wird derzeit nicht unterstützt.

## <a name="verify-servers-in-the-portal"></a>Überprüfen von Servern im Portal

Gehen Sie wie folgt vor, um sich zu vergewissern, dass die Server nach der Ermittlung im Azure-Portal angezeigt werden:

1. Öffnen Sie das Azure Migrate-Dashboard.
2. Wählen Sie auf der Seite **Azure Migrate – Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) das Symbol mit der Anzahl für **Ermittelte Server** aus.
3. Wählen Sie die Registerkarte **Importbasiert** aus.

## <a name="set-up-and-run-an-assessment"></a>Einrichten und Ausführen einer Bewertung

Mit der Serverbewertung können zwei Arten von Bewertungen erstellt werden.


**Bewertungstyp** | **Details**
--- | --- 
**Azure-VM** | Bewertungen zum Migrieren Ihrer lokalen Server zu virtuellen Azure-Computern. <br/><br/> Sie können Ihre lokalen [VMware-VMs](how-to-set-up-appliance-vmware.md), [Hyper-V-VMs](how-to-set-up-appliance-hyper-v.md) und [physischen Server](how-to-set-up-appliance-physical.md) für die Migration zu Azure bewerten, indem Sie diesen Bewertungstyp verwenden.
**Azure VMware Solution (AVS)** | Bewertungen zum Migrieren Ihrer lokalen Server zu [Azure VMware Solution (AVS)](../azure-vmware/introduction.md). <br/><br/> Sie können Ihre lokalen [VMware-VMs](how-to-set-up-appliance-vmware.md) für die Migration zu Azure VMware Solution (AVS) bewerten, indem Sie diesen Bewertungstyp verwenden. [Weitere Informationen](concepts-azure-vmware-solution-assessment-calculation.md)

### <a name="sizing-criteria"></a>Größenkriterien

Die Serverbewertung verfügt über zwei Optionen für Größenkriterien:

**Größenkriterien** | **Details** | **Daten**
--- | --- | ---
**Leistungsbasiert** | Bewertungen, die Empfehlungen auf der Grundlage der erfassten Leistungsdaten aussprechen | **Bewertung vom Typ „Virtueller Azure-Computer“** : Die Empfehlung zur VM-Größe basiert auf CPU- und Speicherauslastungsdaten.<br/><br/> Die Empfehlung zum Datenträgertyp (HDD/SSD Standard oder Premium) basiert auf dem IOPS und dem Durchsatz der lokalen Datenträger.<br/><br/> **Bewertung vom Typ „Azure VMware Solution (AVS)“** : Die Empfehlung zu AVS-Knoten basiert auf CPU- und Speicherauslastungsdaten.
**Aktuelle lokale Umgebung** | Bewertungen, die keine Leistungsdaten verwenden, um Empfehlungen auszusprechen. | **Bewertung vom Typ „Virtueller Azure-Computer“** : Die Empfehlung zur VM-Größe basiert auf der lokalen VM-Größe<br/><br> Der empfohlene Datenträgertyp basiert auf dem, was Sie in der Speichertypeinstellung für die Bewertung auswählen.<br/><br/> **Bewertung vom Typ „Azure VMware Solution (AVS)“** : Die Empfehlung zu AVS-Knoten basiert auf der lokalen VM-Größe.


So führen Sie eine Bewertung aus:

1. Machen Sie sich mit den [bewährten Methoden](best-practices-assessment.md) für die Bewertungserstellung vertraut.
2. Wählen Sie auf der Registerkarte **Server** auf der Kachel **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) die Option **Bewerten** aus.

    ![Bewerten](./media/tutorial-assess-physical/assess.png)

3. Geben Sie unter **Server bewerten** den Bewertungsnamen an, und wählen Sie als Typ der **Bewertung** die Option *Azure-VM* aus, wenn Sie Bewertungen von Azure-VMs durchführen möchten. Wählen Sie *Azure VMware Solution (AVS)* aus, falls Sie AVS-Bewertungen durchführen möchten.

    ![Grundlagen der Bewertung](./media/how-to-create-assessment/assess-servers-azurevm.png)

4. Wählen Sie unter **Ermittlungsquelle** die Option **Machines added via import to Azure Migrate** (Per Azure Migrate-Import hinzugefügte Computer) aus.

5. Wählen Sie **Alle anzeigen** aus, um die Eigenschaften der Bewertung zu überprüfen.

    ![Bewertungseigenschaften](./media/tutorial-assess-physical/view-all.png)

6. Klicken Sie auf **Weiter**, um zu **Computer für die Bewertung auswählen** zu gelangen. Wählen Sie unter **Gruppe auswählen oder erstellen** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Gruppe ein. Eine Gruppe enthält mindestens eine zu bewertende VM.
7. Wählen Sie unter **Computer zur Gruppe hinzufügen** die Server aus, die der Gruppe hinzugefügt werden sollen.
8. Klicken Sie auf **Weiter**, um unter **Überprüfen + Bewertung erstellen** die Bewertungsdetails zu überprüfen.
9. Klicken Sie auf **Bewertung erstellen**, um die Gruppe zu erstellen, und führen Sie anschließend die Bewertung aus.

    ![Erstellen einer Bewertung](./media/tutorial-assess-physical/assessment-create.png)

9. Zeigen Sie die erstellte Bewertung unter **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) > **Bewertungen** an.
10. Wählen Sie **Bewertung exportieren** aus, um sie als Microsoft Excel-Datei herunterzuladen.

Weitere Informationen zur Bewertung vom Typ **Azure VMware Solution (AVS)** finden Sie [hier](how-to-create-azure-vmware-solution-assessment.md). 

## <a name="review-an-azure-vm-assessment"></a>Überprüfen einer Azure-VM-Bewertung

Bei einer Azure-VM-Bewertung wird Folgendes beschrieben:

- **Azure-Bereitschaft**: Gibt an, ob Server für die Migration zu Azure geeignet sind.
- **Geschätzte monatliche Kosten**: Die geschätzten monatlichen Compute- und Speicherkosten für die Ausführung der Server in Azure.
- **Geschätzte monatliche Speicherkosten**: Die geschätzten Kosten für den Datenträgerspeicher nach der Migration.

### <a name="view-an-assessment"></a>Anzeigen einer Bewertung

1. Wählen Sie unter **Migrationsziele** > **Server** die Option **Bewertungen** unter **Azure Migrate: Serverbewertung** aus.
2. Wählen Sie unter **Bewertungen** eine Bewertung aus, um sie zu öffnen.

    ![Zusammenfassung der Bewertung](./media/tutorial-assess-physical/assessment-summary.png)

### <a name="review-azure-readiness"></a>Überprüfen der Azure-Bereitschaft

1. Ermitteln Sie unter **Azure-Bereitschaft**, ob die Server für die Migration zu Azure bereit sind.
2. Überprüfen Sie den Status:
    - **Bereit für Azure**: Azure Migrate empfiehlt in der Bewertung eine VM-Größe und gibt Kostenschätzungen für VMs ab.
    - **Bereit mit Bedingungen**: Zeigt Probleme und Lösungsvorschläge an.
    - **Nicht bereit für Azure**: Zeigt Probleme und Lösungsvorschläge an.
    - **Bereitschaft unbekannt**: Die Bereitschaft kann von Azure Migrate aufgrund von Problemen mit der Datenverfügbarkeit nicht bewertet werden.

3. Wählen Sie unter **Azure-Bereitschaft** einen Status aus. Sie können Details zur Serverbereitschaft sowie Serverdetails wie Compute-, Speicher- und Netzwerkeinstellungen anzeigen.

### <a name="review-cost-details"></a>Überprüfen der Kostendetails

In dieser Ansicht werden die geschätzten Compute- und Speicherkosten für die Ausführung der VMs in Azure angezeigt. Ihre Möglichkeiten:

- Überprüfen Sie die monatlichen Compute- und Speicherkosten. Die Kosten für alle Server in der bewerteten Gruppe werden aggregiert.

    - Kostenschätzungen basieren auf den Größenempfehlungen für einen Computer sowie auf seinen Datenträgern und Eigenschaften.
    - Die geschätzten monatlichen Kosten für Compute und Speicher werden angezeigt.
    - Die Kostenschätzung bezieht sich auf die Ausführung lokaler Server als virtuelle IaaS-Computer (Infrastructure-as-a-Service). Von der Serverbewertung werden keine PaaS- (Platform-as-a-Service) oder SaaS-Kosten (Software-as-a-Service) berücksichtigt.

- Überprüfen Sie die geschätzten monatlichen Speicherkosten. Diese Ansicht enthält aggregierte Speicherkosten für die bewertete Gruppe, aufgeschlüsselt nach verschiedenen Arten von Speicherdatenträgern.
- Führen Sie einen Drilldown aus, um Details für bestimmte virtuelle Computer anzuzeigen.

> [!NOTE]
> Zuverlässigkeitsstufen werden keinen Bewertungen von Servern zugewiesen, die per CSV-Datei in die Serverbewertung importiert wurden.

## <a name="supported-operating-system-names"></a>Namen von unterstützten Betriebssystemen

Die in der CSV-Datei angegebenen Betriebssystemnamen müssen mit den Namen in dieser Liste identisch sein oder diese enthalten. Dies ist erforderlich, damit die angegebenen Namen bei der Bewertung als gültig erkannt werden.

<!-- BEGIN A - H -->

:::row:::
   :::column span="2":::
      **A - H**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Apple Mac OS X 10
   :::column-end:::
   :::column span="":::
      Asianux 3<br/>
      Asianux 4<br/>
      Asianux 5
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      CentOS<br/>
      CentOS 4/5
   :::column-end:::
   :::column span="":::
      CoreOS Linux
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Debian GNU/Linux 4<br/>
      Debian GNU/Linux 5<br/>
      Debian GNU/Linux 6<br/>
      Debian GNU/Linux 7<br/>
      Debian GNU/Linux 8
   :::column-end:::
   :::column span="":::
      FreeBSD
   :::column-end:::
:::row-end:::

<!-- BEGIN I - R -->

:::row:::
   :::column span="2":::
      **I - R**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      IBM OS/2
   :::column-end:::
   :::column span="":::
      MS-DOS
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Novell NetWare 5<br/>
      Novell NetWare 6
   :::column-end:::
   :::column span="":::
      Oracle Linux<br/>
       Oracle Linux 4/5<br/>
      Oracle Solaris 10<br/>
       Oracle Solaris 11
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Red Hat Enterprise Linux 2<br/>
      Red Hat Enterprise Linux 3<br/>
      Red Hat Enterprise Linux 4<br/>
      Red Hat Enterprise Linux 5<br/>
      Red Hat Enterprise Linux 6<br/>
      Red Hat Enterprise Linux 7<br/>
      Red Hat Fedora
   :::column-end:::
:::row-end:::

<!-- BEGIN S - T -->

:::row:::
   :::column span="2":::
      **S–T**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      SCO OpenServer 5<br/>
      SCO OpenServer 6<br/>
      SCO UnixWare 7
   :::column-end:::
   :::column span="":::
      Serenity Systems eComStation 1<br/>
      Serenity Systems eComStation 2
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Sun Microsystems Solaris 8<br/>
      Sun Microsystems Solaris 9
   :::column-end:::
   :::column span="":::
      SUSE Linux Enterprise 10<br/>
      SUSE Linux Enterprise 11<br/>
      SUSE Linux Enterprise 12<br/>
      SUSE Linux Enterprise 8/9<br/>
      SUSE Linux Enterprise 11<br/>
      SUSE openSUSE
   :::column-end:::
:::row-end:::

<!-- BEGIN U - Z -->
:::row:::
   :::column span="2":::
      **U–Z**
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Ubuntu Linux
   :::column-end:::
   :::column span="":::
      VMware ESXi 4<br/>
      VMware ESXi 5<br/>
      VMware ESXi 6
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Windows 10<br/>
      Windows 2000<br/>
      Windows 3<br/>
      Windows 7<br/>
      Windows 8<br/>
      Windows 95<br/>
      Windows 98<br/>
      Windows NT<br/>
      Windows Server (R) 2008<br/>
      Windows Server 2003
   :::column-end:::
   :::column span="":::
      Windows Server 2008<br/>
      Windows Server 2008 R2<br/>
      Windows Server 2012<br/>
      Windows Server 2012 R2<br/>
      Windows Server 2016<br/>
      Windows Server 2019<br/>
      Windows Server Threshold<br/>
      Windows Vista<br/>
      Windows Web Server 2008 R2<br/>
      Windows XP Professional
   :::column-end:::
:::row-end:::

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Importieren von Servern in die Azure Migrate-Serverbewertung per CSV-Datei
> * Erstellen und Überprüfen einer Bewertung

Als Nächstes können Sie [eine Appliance bereitstellen](./migrate-appliance.md), um genauere Bewertungen zu erhalten, und Server in Gruppen zusammenfassen, um mittels [Abhängigkeitsanalyse](./concepts-dependency-visualization.md) eingehendere Bewertungen vorzunehmen.
