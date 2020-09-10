---
title: Bewerten von VMware-VMs mit der Azure Migrate-Serverbewertung
description: Hier erfahren Sie, wie Sie lokale VMware-VMs mit der Azure Migrate-Serverbewertung für die Migration zu Azure bewerten.
ms.topic: tutorial
ms.date: 06/03/2020
ms.custom: mvc
ms.openlocfilehash: abd8aafebe9cc02b8ee88cce88616389c82af83f
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89514100"
---
# <a name="assess-vmware-vms-with-server-assessment"></a>Bewerten virtueller VMware-Computer mit der Serverbewertung

In diesem Artikel erfahren Sie, wie Sie lokale virtuelle VMware-Computer (Virtual Machines, VMs) mit dem Tool [Azure Migrate-Serverbewertung](migrate-services-overview.md#azure-migrate-server-assessment-tool) bewerten.


Dieses Tutorial ist das zweite in einer Reihe zur Bewertung und Migration von VMware-VMs zu Azure. In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Einrichten eines Azure Migrate-Projekts
> * Einrichten einer lokal ausgeführten Azure Migrate-Appliance zur Bewertung von VMs
> * Starten der kontinuierlichen Ermittlung lokaler VMs Die Appliance sendet Konfigurations- und Leistungsdaten für erkannte VMs an Azure.
> * Gruppieren erkannter VMs und Bewerten der VM-Gruppe
> * Überprüfen der Bewertung

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell einen Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in den Anleitungsartikeln.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- [Absolvieren Sie das erste Tutorial](tutorial-prepare-vmware.md) dieser Reihe. Andernfalls funktionieren die Anweisungen in diesem Tutorial nicht.
- Im ersten Tutorial müssen folgende Schritte ausgeführt werden:
    - [Vorbereiten von Azure](tutorial-prepare-vmware.md#prepare-azure) auf die Arbeit mit Azure Migrate
    - [Vorbereiten von VMware](tutorial-prepare-vmware.md#prepare-for-assessment) auf die Bewertung. Dies umfasst das Überprüfen der VMware-Einstellungen und das Einrichten eines Kontos, über das Azure Migrate auf vCenter Server zugreifen kann.
    - [Überprüfen](tutorial-prepare-vmware.md#verify-appliance-settings-for-assessment), welche Komponenten für die Bereitstellung der Azure Migrate-Appliance für die VMware-Bewertung erforderlich sind

## <a name="set-up-an-azure-migrate-project"></a>Einrichten eines Azure Migrate-Projekts

Richten Sie wie folgt ein neues Azure Migrate-Projekt ein:

1. Wählen Sie im Azure-Portal **Alle Dienste** aus, und suchen Sie nach **Azure Migrate**.
2. Wählen Sie unter **Dienste** die Option **Azure Migrate** aus.
3. Wählen Sie in der **Übersicht** unter **Server ermitteln, bewerten und migrieren** die Option **Server bewerten und migrieren** aus.

   ![Schaltfläche zum Bewerten und Migrieren von Servern](./media/tutorial-assess-vmware/assess-migrate.png)

4. Wählen Sie unter **Erste Schritte** die Option **Tools hinzufügen** aus.
5. Wählen Sie unter **Projekt migrieren** Ihr Azure-Abonnement aus, und erstellen Sie bei Bedarf eine Ressourcengruppe.     
6. Geben Sie unter **Projektdetails** den Projektnamen und die geografische Region an, in der Sie das Projekt erstellen möchten. Beachten Sie die unterstützten geografischen Regionen für [öffentliche Clouds](migrate-support-matrix.md#supported-geographies-public-cloud) und [Azure Government-Clouds](migrate-support-matrix.md#supported-geographies-azure-government).

   ![Felder für Projektname und Region](./media/tutorial-assess-vmware/migrate-project.png)

7. Wählen Sie **Weiter** aus.
8. Wählen Sie unter **Bewertungstool auswählen** Folgendes aus: **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) > **Weiter**.

   ![Auswahl für das Serverbewertungstool](./media/tutorial-assess-vmware/assessment-tool.png)

9. Wählen Sie unter **Migrationstool auswählen** die Option **Hinzufügen eines Migrationstools vorerst überspringen** >  und anschließend **Weiter** aus.
10. Überprüfen Sie die Einstellungen unter **Überprüfen + Tools hinzufügen**, und wählen Sie **Tools hinzufügen** aus.
11. Warten Sie einige Minuten, bis das Azure Migrate-Projekt bereitgestellt wurde. Sie werden zur Projektseite weitergeleitet. Sollte das Projekt nicht angezeigt werden, können Sie auf dem Azure Migrate-Dashboard unter **Server** darauf zugreifen.

## <a name="set-up-the-azure-migrate-appliance"></a>Einrichten der Azure Migrate-Appliance

Bei der Azure Migrate-Serverbewertung wird eine einfache Azure Migrate-Appliance verwendet. Die Appliance ermittelt VMs und sendet Meta- und Leistungsdaten zu VMs an Azure Migrate. Es gibt verschiedene Möglichkeiten zur Einrichtung der Appliance.

- Einrichten auf einer VMware-VM mit einer heruntergeladenen OVA-Vorlage. **Diese Methode wird in diesem Tutorial verwendet.**
- Einrichten als VMware-VM oder physischer Computer mit einem PowerShell-Installationsskript. [Diese Methode](deploy-appliance-script.md) sollte verwendet werden, wenn Sie eine VM nicht mithilfe einer OVA-Vorlage einrichten können oder wenn Sie in Azure Government arbeiten.

Überprüfen Sie nach der Erstellung der Appliance, ob diese eine Verbindung mit der Azure Migrate-Serverbewertung herstellen kann. Führen Sie dann die erstmalige Konfiguration durch, und registrieren Sie sie für das Azure Migrate-Projekt.


### <a name="generate-the-azure-migrate-project-key"></a>Generieren des Azure Migrate-Projektschlüssels

1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Ermitteln**.
2. Wählen Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere Hypervisor** aus.
3. Geben Sie in **1: Generieren eines Azure Migrate-Projektschlüssels** einen Namen für die Azure Migrate-Appliance an, die Sie für die Ermittlung von VMware-VMs einrichten möchten. Der Name muss alphanumerisch sein und darf höchstens 14 Zeichen enthalten.
1. Klicken Sie auf **Schlüssel generieren**, um mit der Erstellung der erforderlichen Azure-Ressourcen zu beginnen. Schließen Sie die Seite „Computer ermitteln“ nicht, während die Ressourcen erstellt werden.
1. Nach der erfolgreichen Erstellung der Azure-Ressourcen wird ein **Azure Migrate-Projektschlüssel** generiert.
1. Kopieren Sie den Schlüssel, da Sie ihn benötigen, um die Registrierung der Appliance während der Konfiguration abzuschließen.

### <a name="download-the-ova-template"></a>Herunterladen der OVA-Vorlage
Wählen Sie in **2: Azure Migrate-Appliance herunterladen** die OVA-Datei aus, und klicken Sie auf **Herunterladen**. 


   ![Auswahloptionen für „Computer ermitteln“](./media/tutorial-assess-vmware/servers-discover.png)


   ![Auswahloptionen für „Schlüssel generieren“](./media/tutorial-assess-vmware/generate-key-vmware.png)


### <a name="verify-security"></a>Überprüfen der Sicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die OVA-Datei sicher ist:

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:
  
   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
   
   Beispielverwendung: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

3. Überprüfen Sie die aktuellen Applianceversionen und Hashwerte:

    - Öffentliche Azure-Cloud:
    
        **Algorithmus** | **Download** | **SHA256**
        --- | --- | ---
        VMware (11,6 GB) | [Aktuelle Version](https://go.microsoft.com/fwlink/?linkid=2140333) | e9c9a1fe4f3ebae81008328e8f3a7933d78ff835ecd871d1b17f367621ce3c74

    - Azure Government:
    
        **Algorithmus** | **Download** | **SHA256**
        --- | --- | ---
        VMware (85 MB) | [Aktuelle Version](https://go.microsoft.com/fwlink/?linkid=2140337) | 47179f47eba2842337bbe533c424dd1da56baccdcf68b1d87b71a5a4280108c2


### <a name="create-the-appliance-vm"></a>Erstellen der Appliance-VM

Importieren Sie die heruntergeladene Datei, und erstellen Sie einen virtuellen Computer:

1. Klicken Sie in der vSphere-Clientkonsole auf **Datei** > **OVF-Vorlage bereitstellen**.

   ![Menübefehl zum Bereitstellen einer OVF-Vorlage](./media/tutorial-assess-vmware/deploy-ovf.png)

2. Geben Sie im Assistenten zum Bereitstellen der OVF-Vorlage unter **Quelle** den Speicherort der OVA-Datei an.
3. Geben Sie unter **Name** und **Standort** einen Anzeigenamen für die VM an. Wählen Sie das Inventarobjekt aus, in dem die VM gehostet wird.
4. Geben Sie unter **Host/Cluster** den Host oder Cluster an, auf bzw. in dem die VM ausgeführt wird.
5. Geben Sie unter **Speicher** das Speicherziel für die VM an.
6. Geben Sie unter **Datenträgerformat** den Typ und die Größe des Datenträgers an.
7. Geben Sie unter **Netzwerkzuordnung** das Netzwerk an, mit dem der virtuelle Computer eine Verbindung herstellt. Das Netzwerk muss über eine Internetverbindung verfügen, um Metadaten an die Azure Migrate-Serverbewertung senden zu können.
8. Überprüfen und bestätigen Sie die Einstellungen, und klicken Sie auf **Fertig stellen**.

## <a name="verify-appliance-access-to-azure"></a>Überprüfen des Appliancezugriffs auf Azure

Stellen Sie sicher, dass die Appliance-VM eine Verbindung mit Azure-URLs für [öffentliche](migrate-appliance.md#public-cloud-urls) und [Government](migrate-appliance.md#government-cloud-urls)-Clouds herstellen kann.

### <a name="configure-the-appliance"></a>Konfigurieren der Appliance

Führen Sie die Ersteinrichtung der Appliance durch.

> [!NOTE]
> Wenn Sie die Appliance mithilfe eines [PowerShell-Skripts](deploy-appliance-script.md) und nicht mit der heruntergeladenen OVA einrichten, sind die ersten beiden Schritte in diesem Verfahren nicht relevant.

1. Klicken Sie in der vSphere-Clientkonsole mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Konsole öffnen** aus.
2. Geben Sie die Sprache, die Zeitzone und das Kennwort für die Appliance an.
3. Öffnen Sie in einem Browser auf einem beliebigen Computer, der eine Verbindung mit der VM herstellen kann, und öffnen Sie die URL der Appliance-Web-App: **https://*Appliancename oder IP-Adresse*: 44368**.

   Alternativ können Sie auch auf dem Appliancedesktop die App-Verknüpfung auswählen, um die App zu öffnen.
1. Akzeptieren Sie die **Lizenzbedingungen**, und lesen Sie die Drittanbieterinformationen.
1. Gehen Sie in der Web-App unter **Erforderliche Komponenten einrichten** wie folgt vor:
   - **Konnektivität**: Die App überprüft, ob die VM über Internetzugriff verfügt. Falls die VM einen Proxy verwendet, gehen Sie wie folgt vor:
     - Klicken Sie auf **Proxy einrichten**, um die Proxyadresse im Format http://ProxyIPAddress oder http://ProxyFQDN) und den überwachenden Port anzugeben.
     - Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert.
     - Es werden nur HTTP-Proxys unterstützt.
     - Wenn Sie Proxydetails hinzugefügt oder den Proxy und/oder die Authentifizierung deaktiviert haben, klicken Sie auf **Speichern**, um die Konnektivitätsprüfung erneut auszulösen.
   - **Uhrzeitsynchronisierung**: Die Uhrzeit der Appliance muss mit der Internetzeit synchronisiert werden, damit die Ermittlung ordnungsgemäß funktioniert.
   - **Updates installieren**: Die Appliance stellt sicher, dass die neuesten Updates installiert sind. Nachdem die Prüfung abgeschlossen ist, können Sie auf **Appliancedienste anzeigen** klicken, um den Status und die Versionen der auf der Appliance ausgeführten Komponenten anzuzeigen.
   - **Install VDDK** (VDDK installieren): Die Appliance überprüft, ob das VMware vSphere-VDDK (Virtual Disk Development Kit) installiert ist. Laden Sie bei Bedarf VDDK 6.7 von VMware herunter, und extrahieren Sie den Inhalt der heruntergeladenen ZIP-Datei am angegebenen Ort auf der Appliance, wie unter **Installationsanweisungen** beschrieben.

     Das VDDK wird von der Azure Migrate-Servermigration verwendet, um Computer bei der Migration zu Azure zu replizieren. 
1. Wenn Sie möchten, können Sie während der Konfiguration der Appliance jederzeit **Voraussetzungen erneut ausführen**, um zu prüfen, ob die Appliance alle Voraussetzungen erfüllt.

### <a name="register-the-appliance-with-azure-migrate"></a>Registrieren der Appliance bei Azure Migrate

1. Fügen Sie den aus dem Portal kopierten **Azure Migrate-Projektschlüssel** ein. Wenn Sie den Schlüssel nicht haben, wechseln Sie zu **Serverbewertung > Ermitteln > Vorhandene Appliances verwalten**, wählen Sie den Appliancenamen aus, den Sie bei der Generierung des Schlüssels angegeben haben, und kopieren Sie den entsprechenden Schlüssel.
1. Klicken Sie auf **Anmelden**. Auf einer neuen Browserregisterkarte wird eine Azure-Anmeldeaufforderung geöffnet. Sollte keine Anmeldung angezeigt werden, vergewissern Sie sich, dass Sie den Popupblocker im Browser deaktiviert haben.
1. Melden Sie sich auf dem neuen Tab mit Ihrem Azure-Benutzernamen und -Kennwort an.
   
   Die Anmeldung mit einer PIN wird nicht unterstützt.
3. Kehren Sie nach erfolgreicher Anmeldung zur Web-App zurück. 
4. Wenn das für die Protokollierung verwendete Azure-Benutzerkonto über die richtigen [Berechtigungen](tutorial-prepare-vmware.md#prepare-azure) für die während der Schlüsselgenerierung erstellten Azure-Ressourcen verfügt, wird die Registrierung der Appliance initiiert.
1. Nachdem die Appliance erfolgreich registriert wurde, können Sie die Registrierungsdetails anzeigen, indem Sie auf **Details anzeigen** klicken.


## <a name="start-continuous-discovery"></a>Starten der kontinuierlichen Ermittlung

Die Appliance muss eine Verbindung mit der vCenter Server-Instanz herstellen, um die Konfigurations- und Leistungsdaten der VMs zu ermitteln.

1. Wählen Sie in **Schritt 1: Geben Sie die vCenter Server-Anmeldeinformationen** ein, klicken Sie auf **Anmeldeinformationen hinzufügen**, um einen Anzeigenamen für die Anmeldeinformationen anzugeben, fügen Sie **Benutzername** und **Kennwort** für das vCenter Server-Konto hinzu, das die Appliance verwendet, um VMs auf der vCenter Server-Instanz zu ermitteln.
    - Sie sollten im [vorherigen Tutorial](tutorial-prepare-vmware.md#set-up-permissions-for-assessment) ein Konto mit den erforderlichen Berechtigungen eingerichtet haben.
    - Wenn Sie die Ermittlung auf bestimmte VMware-Objekte (vCenter Server-Rechenzentren, Cluster, einen Ordner mit Clustern, Hosts, einen Ordner mit Hosts oder einzelne VMs) begrenzen möchten, hilft Ihnen die Anleitung in [diesem Artikel](set-discovery-scope.md) beim Einschränken des von Azure Migrate genutzten Kontos weiter.
1. Geben Sie in **Step 2: Geben Sie vCenter Server-Details** ein, klicken Sie auf **Ermittlungsquelle hinzufügen**, um den Anzeigenamen für die Anmeldeinformationen aus der Dropdownliste auszuwählen, geben Sie den **IP-Adresse/FQDN**-Wert der vCenter Server-Instanz an. Sie können für **Port** den Standardwert (443) beibehalten oder einen benutzerdefinierten Port angeben, den vCenter Server überwacht, und dann auf **Speichern** klicken.
1. Wenn Sie auf „Speichern“ klicken, versucht die Appliance, die Verbindung zum vCenter Server mit den angegebenen Anmeldeinformationen zu überprüfen, und zeigt den **Überprüfungszustand** in der Tabelle für die vCenter Server-IP-Adresse/FQDN an.
1. Sie können die Verbindung zu vCenter Server jederzeit vor Beginn der Ermittlung **erneut überprüfen**.
1. Wählen Sie in **Schritt 3: Geben Sie VM-Anmeldeinformationen an, um installierte Anwendungen zu ermitteln und eine Abhängigkeitszuordnung ohne Agent** durchzuführen, klicken Sie auf **Anmeldeinformationen hinzufügen**, und geben Sie das Betriebssystem, für das die Anmeldeinformationen bereitgestellt werden, einen Anzeigenamen für die Anmeldeinformationen sowie den **Benutzernamen** und das **Kennwort** an. Klicken Sie anschließend auf **Speichern**.

    - Hier können Sie optional Anmeldeinformationen hinzufügen, wenn Sie ein Konto erstellt haben, das für das [Feature „Anwendungsermittlung“](how-to-discover-applications.md) oder das [Feature „Abhängigkeitsanalyse ohne Agent“](how-to-create-group-machine-dependencies-agentless.md) verwendet werden soll.
    - Wenn Sie diese Features nicht verwenden möchten, können Sie auf den Schieberegler klicken, um den Schritt zu überspringen. Sie können die Absicht jederzeit später rückgängig machen.
    - Sehen Sie sich die Anmeldeinformationen für [Anwendungsermittlung](migrate-support-matrix-vmware.md#application-discovery-requirements) oder für die [Abhängigkeitsanalyse ohne Agent](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) an.

5. Klicken Sie auf **Ermittlung starten**, um die VM-Ermittlung zu starten. Nachdem die Ermittlung erfolgreich gestartet wurde, können Sie den Ermittlungsstatus für die vCenter Server-IP-Adresse/FQDN in der Tabelle überprüfen.

Die Ermittlung funktioniert wie folgt:
- Es dauert ca. 15 Minuten, bis die ermittelten VM-Metadaten im Portal angezeigt werden.
- Die Ermittlung installierter Anwendungen, Rollen und Features dauert etwas. Die Dauer hängt davon ab, wie viele virtuelle Computer ermittelt werden. Bei 500 VMs dauert es ungefähr ein Stunde, bis der Anwendungsbestand im Azure Migrate-Portal angezeigt wird.

### <a name="verify-vms-in-the-portal"></a>Überprüfen virtueller Computer im Portal

Nach der Ermittlung können Sie überprüfen, ob die virtuellen Computer im Azure-Portal angezeigt werden:

1. Öffnen Sie das Azure Migrate-Dashboard.
2. Klicken Sie unter **Azure Migrate – Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf das Symbol mit der Anzahl für **Ermittelte Server**.

## <a name="set-up-an-assessment"></a>Einrichten einer Bewertung

Mit der Azure Migrate-Serverbewertung können zwei Arten von Bewertungen erstellt werden:

**Bewertungstyp** | **Details**
--- | --- 
**Azure-VM** | Bewertungen zum Migrieren Ihrer lokalen Server zu virtuellen Azure-Computern. <br/><br/> Sie können Ihre lokalen [VMware-VMs](how-to-set-up-appliance-vmware.md), [Hyper-V-VMs](how-to-set-up-appliance-hyper-v.md) und [physischen Server](how-to-set-up-appliance-physical.md) für die Migration zu Azure bewerten, indem Sie diesen Bewertungstyp verwenden. [Weitere Informationen](concepts-assessment-calculation.md)
**Azure VMware Solution (AVS)** | Bewertungen zum Migrieren Ihrer lokalen Server zu [Azure VMware Solution (AVS)](../azure-vmware/introduction.md). <br/><br/> Sie können Ihre lokalen [VMware-VMs](how-to-set-up-appliance-vmware.md) für die Migration zu Azure VMware Solution (AVS) bewerten, indem Sie diesen Bewertungstyp verwenden. [Weitere Informationen](concepts-azure-vmware-solution-assessment-calculation.md)

Die Serverbewertung verfügt über zwei Optionen für Größenkriterien:

**Größenkriterien** | **Details** | **Daten**
--- | --- | ---
**Leistungsbasiert** | Bewertungen, die Empfehlungen auf der Grundlage der erfassten Leistungsdaten aussprechen | **Bewertung vom Typ „Virtueller Azure-Computer“** : Die Empfehlung zur VM-Größe basiert auf CPU- und Speicherauslastungsdaten.<br/><br/> Die Empfehlung zum Datenträgertyp (HDD/SSD Standard oder Premium) basiert auf dem IOPS und dem Durchsatz der lokalen Datenträger.<br/><br/> **Bewertung vom Typ „Azure VMware Solution (AVS)“** : Die Empfehlung zu AVS-Knoten basiert auf CPU- und Speicherauslastungsdaten.
**Aktuelle lokale Umgebung** | Bewertungen, die keine Leistungsdaten verwenden, um Empfehlungen auszusprechen. | **Bewertung vom Typ „Virtueller Azure-Computer“** : Die Empfehlung zur VM-Größe basiert auf der lokalen VM-Größe<br/><br> Der empfohlene Datenträgertyp basiert auf dem, was Sie in der Speichertypeinstellung für die Bewertung auswählen.<br/><br/> **Bewertung vom Typ „Azure VMware Solution (AVS)“** : Die Empfehlung zu AVS-Knoten basiert auf der lokalen VM-Größe.

## <a name="run-an-assessment"></a>Durchführen einer Bewertung

Führen Sie wie folgt eine *Azure-VM-Bewertung* aus:

1. Machen Sie sich mit den [bewährten Methoden](best-practices-assessment.md) für die Bewertungserstellung vertraut.
2. Wählen Sie auf der Registerkarte **Server** auf der Kachel **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) die Option **Bewerten** aus.

   ![Position der Schaltfläche „Bewerten“](./media/tutorial-assess-vmware/assess.png)

3. Wählen Sie unter **Server bewerten** die Option „Azure-VM“ als Bewertungstyp und dann die Ermittlungsquelle aus, und geben Sie den Bewertungsnamen an.

    ![Grundlagen der Bewertung](./media/tutorial-assess-vmware/assess-servers-azurevm.png)
 
4. Wählen Sie **Alle anzeigen** aus, und überprüfen Sie anschließend die Bewertungseigenschaften.

   ![Bewertungseigenschaften](./media/tutorial-assess-vmware/view-all.png)

5. Klicken Sie auf **Weiter**, um zu **Computer für die Bewertung auswählen** zu gelangen. Wählen Sie unter **Gruppe auswählen oder erstellen** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Gruppe ein. Eine Gruppe enthält mindestens eine zu bewertende VM.
6. Wählen Sie unter **Computer zur Gruppe hinzufügen** die VMs aus, die der Gruppe hinzugefügt werden sollen.
7. Klicken Sie auf **Weiter**, um unter **Überprüfen + Bewertung erstellen** die Bewertungsdetails zu überprüfen.
8. Wählen Sie **Bewertung erstellen** aus, um die Gruppe zu erstellen und die Bewertung auszuführen.

   ![Bewerten von Servern](./media/tutorial-assess-vmware/assessment-create.png)

8. Zeigen Sie die erstellte Bewertung unter **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) > **Bewertungen** an.
9. Wählen Sie **Bewertung exportieren** aus, um sie als Excel-Datei herunterzuladen.

Führen Sie die [hier](how-to-create-azure-vmware-solution-assessment.md) beschriebenen Schritte aus, wenn Sie eine **AVS-Bewertung (Azure VMware Solution)** ausführen möchten.


## <a name="review-an-assessment"></a>Überprüfen einer Bewertung

Eine Bewertung beschreibt Folgendes:

- **Azure-Bereitschaft**: Gibt an, ob VMs für die Migration zu Azure geeignet sind.
- **Geschätzte monatliche Kosten**: Die geschätzten monatlichen Compute- und Speicherkosten für die Ausführung der VMs in Azure.
- **Geschätzte monatliche Speicherkosten**: Die geschätzten Kosten für den Datenträgerspeicher nach der Migration.

So zeigen Sie eine Bewertung an:

1. Wählen Sie unter **Migrationsziele** > **Server** die Option **Bewertungen** unter **Azure Migrate: Serverbewertung** aus.
2. Wählen Sie unter **Bewertungen** eine Bewertung aus, um sie zu öffnen.

   ![Zusammenfassung der Bewertung](./media/tutorial-assess-vmware/assessment-summary.png)

### <a name="review-azure-readiness"></a>Überprüfen der Azure-Bereitschaft

1. Überprüfen Sie unter **Azure-Bereitschaft**, ob VMs für die Migration zu Azure bereit sind.
2. Überprüfen Sie den VM-Status:
    - **Bereit für Azure**: Wird verwendet, wenn Azure Migrate in der Bewertung eine VM-Größe empfiehlt und Kostenschätzungen für virtuelle Computer angibt.
    - **Bereit mit Bedingungen**: Zeigt Probleme und eine vorgeschlagene Abhilfe an.
    - **Nicht bereit für Azure**: Zeigt Probleme und eine vorgeschlagene Abhilfe an.
    - **Bereitschaft unbekannt**: Wird verwendet, wenn Azure Migrate die Bereitschaft aufgrund von Problemen mit der Datenverfügbarkeit nicht bewerten kann.

3. Wählen Sie unter **Azure-Bereitschaft** einen Status aus. Sie können Details zur VM-Bereitschaft anzeigen. Darüber hinaus können Sie VM-Details wie Compute-, Speicher- und Netzwerkeinstellungen anzeigen.

### <a name="review-cost-details"></a>Überprüfen der Kostendetails

Die Zusammenfassung der Bewertung enthält die geschätzten Compute- und Speicherkosten für die VM-Ausführung in Azure. Die Kosten für alle VMs in der bewerteten Gruppe werden aggregiert. Sie können einen Drilldown ausführen, um Kostendetails für bestimmte virtuelle Computer anzuzeigen.

> [!NOTE]
> Kostenschätzungen basieren auf den Größenempfehlungen für einen Computer sowie auf seinen Datenträgern und Eigenschaften. Schätzungen gelten für die Ausführung der lokalen virtuellen Computer als virtuelle IaaS-Computer. PaaS- oder SaaS-Kosten werden von der Azure Migrate-Serverbewertung nicht berücksichtigt.

Die aggregierten Speicherkosten für die bewertete Gruppe verteilen sich auf verschiedene Arten von Speicherdatenträgern. 

### <a name="review-confidence-rating"></a>Prüfen der Zuverlässigkeitsstufe

Die Azure Migrate-Serverbewertung weist einer leistungsbasierten Bewertung eine Zuverlässigkeitsstufe zwischen einem Stern und fünf Sternen zu.

![Zuverlässigkeitsstufe](./media/tutorial-assess-vmware/confidence-rating.png)

Anhand der Zuverlässigkeitsstufe können Sie die Zuverlässigkeit der von der Bewertung abgegebenen Größenempfehlungen besser einschätzen. Die Stufe basiert auf der Verfügbarkeit von Datenpunkten, die zur Berechnung der Bewertung erforderlich sind:

**Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
--- | ---
0 % bis 20 % | 1 Stern
21 % bis 40 % | 2 Sterne
41 % bis 60 % | 3 Sterne
61 % bis 80 % | 4 Sterne
81 % bis 100 % | 5 Sterne

Informationen zu Best Practices für Zuverlässigkeitsstufen finden Sie [hier](best-practices-assessment.md#best-practices-for-confidence-ratings).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure Migrate-Appliance eingerichtet. Außerdem haben Sie eine Bewertung erstellt und überprüft.

Im dritten Tutorial der Reihe erfahren Sie, wie Sie virtuelle VMware-Computer mithilfe der Azure Migrate-Servermigration zu Azure migrieren.

> [!div class="nextstepaction"]
> [Migrieren von VMware-VMs zu Azure (ohne Agent)](./tutorial-migrate-vmware.md)
