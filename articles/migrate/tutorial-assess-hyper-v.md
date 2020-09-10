---
title: Bewerten von Hyper-V-VMs für die Migration zu Azure mit Azure Migrate | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie lokale Hyper-V-VMs mithilfe der Azure Migrate-Serverbewertung für die Migration zu Azure bewerten.
ms.topic: tutorial
ms.date: 06/03/2020
ms.custom: mvc
ms.openlocfilehash: 57d91f14b8f3a9f58373cbd43561a03a8546fd8f
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89514491"
---
# <a name="assess-hyper-v-vms-with-azure-migrate-server-assessment"></a>Bewerten von Hyper-V-VMs mit der Azure Migrate-Serverbewertung

In diesem Artikel erfahren Sie, wie Sie lokale Hyper-V-VMs mithilfe des [Azure Migrate-Serverbewertungstools](migrate-services-overview.md#azure-migrate-server-assessment-tool) bewerten.


Dieses Tutorial ist das zweite in einer Reihe zur Bewertung und Migration von Hyper-V-VMs zu Azure. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten eines Azure Migrate-Projekts
> * Einrichten und Registrieren einer Azure Migrate-Appliance
> * Starten der kontinuierlichen Ermittlung lokaler VMs
> * Gruppieren erkannter VMs und Bewerten der Gruppe
> * Überprüfen der Bewertung

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell einen Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in den Anleitungsartikeln.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

- [Absolvieren Sie das erste Tutorial dieser Reihe.](tutorial-prepare-hyper-v.md) Andernfalls funktionieren die Anweisungen in diesem Tutorial nicht.
- Im ersten Tutorial müssen folgende Schritte ausgeführt werden:
    - [Vorbereiten von Azure](tutorial-prepare-hyper-v.md#prepare-azure) auf die Verwendung mit Azure Migrate
    - [Vorbereiten der Bewertung von Hyper-V-Hosts](tutorial-prepare-hyper-v.md#prepare-for-assessment) und VMs
    - [Überprüfen](tutorial-prepare-hyper-v.md#prepare-for-appliance-deployment), welche Komponenten für die Bereitstellung der Azure Migrate-Appliance für die Hyper-V-Bewertung erforderlich sind

## <a name="set-up-an-azure-migrate-project"></a>Einrichten eines Azure Migrate-Projekts

1. Wählen Sie im Azure-Portal **Alle Dienste** aus, und suchen Sie nach **Azure Migrate**.
2. Wählen Sie in den Suchergebnissen die Option **Azure Migrate** aus.
3. Klicken Sie in der **Übersicht** unter **Server ermitteln, bewerten und migrieren** auf **Server bewerten und migrieren**.

    ![Ermitteln und Bewerten von Servern](./media/tutorial-assess-hyper-v/assess-migrate.png)

4. Klicken Sie unter **Erste Schritte** auf **Tools hinzufügen**.
5. Wählen Sie auf der Registerkarte **Projekt migrieren** Ihr Azure-Abonnement aus, und erstellen Sie bei Bedarf eine Ressourcengruppe.
6. Geben Sie unter **Projektdetails** den Projektnamen und die Region an, in der Sie das Projekt erstellen möchten. Beachten Sie die unterstützten geografischen Regionen für [öffentliche Clouds](migrate-support-matrix.md#supported-geographies-public-cloud) und [Azure Government-Clouds](migrate-support-matrix.md#supported-geographies-azure-government).

    - Die Projektregion dient nur zum Speichern der Metadaten, die von den lokalen VMs erfasst werden.
    - Bei der Migration der VMs kann eine andere Azure-Zielregion ausgewählt werden. Alle Azure-Regionen werden als Migrationsziel unterstützt.

    ![Erstellen eines Azure Migrate-Projekts](./media/tutorial-assess-hyper-v/migrate-project.png)

7. Klicken Sie auf **Weiter**.
8. Wählen Sie unter **Bewertungstool auswählen** Folgendes aus: **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) > **Weiter**.

    ![Erstellen eines Azure Migrate-Projekts](./media/tutorial-assess-hyper-v/assessment-tool.png)

9. Wählen Sie unter **Migrationstool auswählen** die Option **Hinzufügen eines Migrationstools vorerst überspringen** >  und anschließend **Weiter** aus.
10. Überprüfen Sie die Einstellungen unter **Review + add tools** (Überprüfen und Tools hinzufügen), und klicken Sie auf **Tools hinzufügen**.
11. Warten Sie einige Minuten, bis das Azure Migrate-Projekt bereitgestellt wurde. Sie werden zur Projektseite weitergeleitet. Sollte das Projekt nicht angezeigt werden, können Sie auf dem Azure Migrate-Dashboard unter **Server** darauf zugreifen.

## <a name="set-up-the-azure-migrate-appliance"></a>Einrichten der Azure Migrate-Appliance


Bei der Azure Migrate-Serverbewertung wird eine einfache Azure Migrate-Appliance verwendet. Die Appliance ermittelt VMs und sendet Meta- und Leistungsdaten zu VMs an Azure Migrate. Es gibt verschiedene Möglichkeiten zur Einrichtung der Appliance.

- Einrichten auf einer Hyper-V-VM mit einer heruntergeladenen Hyper-V-VHD. Diese Methode wird in diesem Tutorial verwendet.
- Einrichten als Hyper-V-VM oder physischer Computer mit einem PowerShell-Installationsskript. [Diese Methode](deploy-appliance-script.md) sollte verwendet werden, wenn Sie eine VM nicht mithilfe der VHD einrichten können oder wenn Sie in Azure Government arbeiten.

Überprüfen Sie nach der Erstellung der Appliance, ob diese eine Verbindung mit der Azure Migrate-Serverbewertung herstellen kann. Führen Sie dann die erstmalige Konfiguration durch, und registrieren Sie sie für das Azure Migrate-Projekt.

### <a name="generate-the-azure-migrate-project-key"></a>Generieren des Azure Migrate-Projektschlüssels

1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Ermitteln**.
2. Wählen Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** die Option **Ja, mit Hyper-V**.
3. Geben Sie in **1: Generieren eines Azure Migrate-Projektschlüssels** einen Namen für die Azure Migrate-Appliance an, die Sie für die Ermittlung von Hyper-V-VMs einrichten möchten. Der Name muss alphanumerisch sein und darf höchstens 14 Zeichen enthalten.
1. Klicken Sie auf **Schlüssel generieren**, um mit der Erstellung der erforderlichen Azure-Ressourcen zu beginnen. Schließen Sie die Seite „Computer ermitteln“ nicht, während die Ressourcen erstellt werden.
1. Nach der erfolgreichen Erstellung der Azure-Ressourcen wird ein **Azure Migrate-Projektschlüssel** generiert.
1. Kopieren Sie den Schlüssel, da Sie ihn benötigen, um die Registrierung der Appliance während der Konfiguration abzuschließen.

### <a name="download-the-vhd"></a>Herunterladen der VHD

Wählen Sie in **2: Azure Migrate-Appliance herunterladen** die VHD-Datei aus, und klicken Sie auf **Herunterladen**. 

   ![Auswahloptionen für „Computer ermitteln“](./media/tutorial-assess-hyper-v/servers-discover.png)


   ![Auswahloptionen für „Schlüssel generieren“](./media/tutorial-assess-hyper-v/generate-key-hyperv.png)


### <a name="verify-security"></a>Überprüfen der Sicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die gezippte Datei sicher ist.

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.

2. Führen Sie den folgenden PowerShell-Befehl aus, um den Hash für die ZIP-Datei zu generieren:
    - ```C:\>Get-FileHash -Path <file_location> -Algorithm [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>Get-FileHash -Path ./AzureMigrateAppliance_v1.19.06.27.zip -Algorithm SHA256```

3.  Überprüfen Sie die aktuellen Applianceversionen und Hashwerte:

    - Öffentliche Azure-Cloud:

        **Szenario** | **Download** | **SHA256**
        --- | --- | ---
        Hyper-V (10,4 GB) | [Aktuelle Version](https://go.microsoft.com/fwlink/?linkid=2140422) |  79c151588de049cc102f61b910d6136e02324dc8d8a14f47772da351b46d9127

    - Azure Government:

        **Szenario*** | **Download** | **SHA256**
        --- | --- | ---
        Hyper-V (85 MB) | [Aktuelle Version](https://go.microsoft.com/fwlink/?linkid=2140424) |  0769c5f8df1e8c1ce4f685296f9ee18e1ca63e4a111d9aa4e6982e069df430d7


### <a name="create-the-appliance-vm"></a>Erstellen der Appliance-VM

Importieren Sie die heruntergeladene Datei, und erstellen Sie die VM.

1. Laden Sie die gezippte VHD-Datei auf den Hyper-V-Host herunter, auf dem sich die Appliance-VM befinden wird, und extrahieren Sie sie anschließend.
    - Am Speicherort für das Extrahieren wird die Datei in einem Ordner namens **AzureMigrateAppliance_VersionNumber** entpackt.
    - Dieser Ordner enthält einen Unterordner, der ebenfalls **AzureMigrateAppliance_VersionNumber** heißt.
    - Dieser Unterordner enthält drei weitere Unterordner: **Momentaufnahmen** (Snapshots), **VHDs** (Virtual Hard Disks) und **Virtual Machines**.

2. Öffnen Sie den Hyper-V-Manager. Klicken Sie unter **Aktionen** auf **Virtuellen Computer importieren**.

    ![Bereitstellen der VHD](./media/tutorial-assess-hyper-v/deploy-vhd.png)

2. Klicken Sie im Assistenten zum Importieren virtueller Computer unter **Vorbereitung** auf **Weiter**.
3. Wählen Sie unter **Ordner suchen** den Ordner **Virtual Machines** aus. Klicken Sie dann auf **Weiter**.
1. Klicken Sie unter **Virtuellen Computer auswählen** auf **Weiter**.
2. Klicken Sie unter **Importtyp auswählen** auf **Virtuellen Computer kopieren (neue eindeutige ID erstellen)** . Klicken Sie dann auf **Weiter**.
3. Behalten Sie unter **Ziel auswählen** die Standardeinstellung bei. Klicken Sie auf **Weiter**.
4. Behalten Sie unter **Speicherordner** die Standardeinstellung bei. Klicken Sie auf **Weiter**.
5. Geben Sie unter **Netzwerk auswählen** den virtuellen Switch an, der von der VM verwendet wird. Der Switch benötigt Internetkonnektivität, um Daten an Azure senden zu können. [Hier](/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines) finden Sie Informationen zum Erstellen eines virtuellen Switches.
6. Überprüfen Sie die Einstellungen unter **Zusammenfassung**. Klicken Sie auf **Fertig stellen**.
7. Starten Sie die VM im Hyper-V-Manager unter **Virtuelle Computer**.


## <a name="verify-appliance-access-to-azure"></a>Überprüfen des Appliancezugriffs auf Azure

Stellen Sie sicher, dass die Appliance-VM eine Verbindung mit Azure-URLs für [öffentliche](migrate-appliance.md#public-cloud-urls) und [Government](migrate-appliance.md#government-cloud-urls)-Clouds herstellen kann.

### <a name="configure-the-appliance"></a>Konfigurieren der Appliance

Führen Sie die Ersteinrichtung der Appliance durch.

> [!NOTE]
> Wenn Sie die Appliance mithilfe eines [PowerShell-Skripts](deploy-appliance-script.md) und nicht mit der heruntergeladenen VHD einrichten, sind die ersten beiden Schritte in diesem Verfahren nicht relevant.

1. Klicken Sie im Hyper-V-Manager unter **Virtuelle Computer** mit der rechten Maustaste auf die VM, und klicken Sie anschließend auf **Verbinden**.
2. Geben Sie die Sprache, die Zeitzone und das Kennwort für die Appliance an.
3. Öffnen Sie in einem Browser auf einem beliebigen Computer, der eine Verbindung mit der VM herstellen kann, und öffnen Sie die URL der Appliance-Web-App: **https://*Appliancename oder IP-Adresse*: 44368**.

   Alternativ können Sie auch auf dem Appliancedesktop auf die App-Verknüpfung klicken, um die App zu öffnen.
1. Akzeptieren Sie die **Lizenzbedingungen**, und lesen Sie die Drittanbieterinformationen.
1. Gehen Sie in der Web-App unter **Erforderliche Komponenten einrichten** wie folgt vor:
    - **Konnektivität**: Die App überprüft, ob die VM über Internetzugriff verfügt. Falls die VM einen Proxy verwendet, gehen Sie wie folgt vor:
      - Klicken Sie auf **Proxy einrichten**, um die Proxyadresse im Format http://ProxyIPAddress oder http://ProxyFQDN) und den überwachenden Port anzugeben.
      - Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert.
      - Es werden nur HTTP-Proxys unterstützt.
      - Wenn Sie Proxydetails hinzugefügt oder den Proxy und/oder die Authentifizierung deaktiviert haben, klicken Sie auf **Speichern**, um die Konnektivitätsprüfung erneut auszulösen.
    - **Uhrzeitsynchronisierung**: Die Uhrzeit wird überprüft. Die Uhrzeit der Appliance muss mit der Internetzeit synchronisiert werden, damit die VM-Ermittlung ordnungsgemäß funktioniert.
    - **Updates installieren**: Azure Migrate-Serverbewertung prüft, ob die neuesten Updates für die Appliance installiert sind. Nachdem die Prüfung abgeschlossen ist, können Sie auf **Appliancedienste anzeigen** klicken, um den Status und die Versionen der auf der Appliance ausgeführten Komponenten anzuzeigen.

### <a name="register-the-appliance-with-azure-migrate"></a>Registrieren der Appliance bei Azure Migrate

1. Fügen Sie den aus dem Portal kopierten **Azure Migrate-Projektschlüssel** ein. Wenn Sie den Schlüssel nicht haben, wechseln Sie zu **Serverbewertung > Ermitteln > Vorhandene Appliances verwalten**, wählen Sie den Appliancenamen aus, den Sie bei der Generierung des Schlüssels angegeben haben, und kopieren Sie den entsprechenden Schlüssel.
1. Klicken Sie auf **Anmelden**. Auf einer neuen Browserregisterkarte wird eine Azure-Anmeldeaufforderung geöffnet. Sollte keine Anmeldung angezeigt werden, vergewissern Sie sich, dass Sie den Popupblocker im Browser deaktiviert haben.
1. Melden Sie sich auf dem neuen Tab mit Ihrem Azure-Benutzernamen und -Kennwort an.
   
   Die Anmeldung mit einer PIN wird nicht unterstützt.
3. Kehren Sie nach erfolgreicher Anmeldung zur Web-App zurück. 
4. Wenn das für die Protokollierung verwendete Azure-Benutzerkonto über die richtigen [Berechtigungen](tutorial-prepare-hyper-v.md#prepare-azure) für die während der Schlüsselgenerierung erstellten Azure-Ressourcen verfügt, wird die Registrierung der Appliance initiiert.
1. Nachdem die Appliance erfolgreich registriert wurde, können Sie die Registrierungsdetails anzeigen, indem Sie auf **Details anzeigen** klicken.



### <a name="delegate-credentials-for-smb-vhds"></a>Delegieren von Anmeldeinformationen für SMB-VHDs

Wenn Sie VHDs in SMBs ausführen, müssen Sie die Delegierung von Anmeldeinformationen von der Appliance an die Hyper-V-Hosts aktivieren. Dazu ermöglichen Sie es jedem Host, als Delegat für die Appliance zu fungieren. Wenn Sie die Tutorials der Reihe nach durchgearbeitet haben, wurden diese Schritte im vorherigen Tutorial ausgeführt, als Sie Hyper-V auf die Bewertung und Migration vorbereitet haben. Sie müssen CredSSP für die Hosts entweder [manuell](tutorial-prepare-hyper-v.md#enable-credssp-to-delegate-credentials) oder durch [Ausführung eines entsprechenden Skripts](tutorial-prepare-hyper-v.md#run-the-script) eingerichtet haben.

Gehen Sie zur Aktivierung für die Appliance wie folgt vor:

#### <a name="option-1"></a>Option 1:

Führen Sie auf der Appliance-VM den folgenden Befehl aus. „HyperVHost1“ und „HyperVHost2“ sind Beispielhostnamen.

```
Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com, HyperVHost2.contoso.com, HyperVHost1, HyperVHost2 -Force
```

Beispiel: ` Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force `

#### <a name="option-2"></a>Option 2:

Gehen Sie alternativ im Editor für lokale Gruppenrichtlinien auf der Appliance wie folgt vor:

1. Klicken Sie unter **Richtlinie für "Lokaler Computer"**  > **Computerkonfiguration** auf **Administrative Vorlagen** > **System** > **Delegierung von Anmeldeinformationen**.
2. Doppelklicken Sie auf **Delegierung von aktuellen Anmeldeinformationen zulassen**, und wählen Sie **Aktiviert** aus.
3. Klicken Sie unter **Optionen** auf **Anzeigen**, und fügen Sie jeden Hyper-V-Host, den Sie ermitteln möchten, mit dem Präfix **wsman/** der Liste hinzu.
4. Doppelklicken Sie dann unter **Delegierung von Anmeldeinformationen** auf **Delegierung von aktuellen Anmeldeinformationen mit reiner NTLM-Serverauthentifizierung zulassen**. Fügen Sie auch hier wieder jeden Hyper-V-Host, den Sie ermitteln möchten, mit dem Präfix **wsman/** der Liste hinzu.

## <a name="start-continuous-discovery"></a>Starten der kontinuierlichen Ermittlung

Stellen Sie von der Appliance aus eine Verbindung mit Hyper-V-Hosts oder -Clustern her, und starten Sie VM-Ermittlung.

1. Wählen Sie in **Schritt 1: Geben Sie die Hyper-V-Hostanmeldeinformationen** ein, klicken Sie auf **Anmeldeinformationen hinzufügen**, um einen Anzeigenamen für die Anmeldeinformationen anzugeben, und fügen Sie **Benutzername** und **Kennwort** für einen Hyper-V-Host/-Cluster hinzu, der die Appliance verwendet, um VMs zu ermitteln. Klicken Sie auf **Speichern**.
1. Wenn Sie mehrere Anmeldeinformationen gleichzeitig hinzufügen möchten, klicken Sie auf **Weitere hinzufügen**, um die Angeben zu speichern und weitere Anmeldeinformationen hinzuzufügen. Es werden mehrere Anmeldeinformationen für die Ermittlung von Hyper-V-VMs unterstützt.
1. Klicken Sie in **Schritt 2: Bereitstellen von Details zu Hyper-V-Host/-Cluster** auf **Ermittlungsquelle hinzufügen**, um **IP-Adresse/FQDN** für den Hyper-V-Host/-Cluster und den Anzeigenamen für Anmeldeinformationen zum Herstellen einer Verbindung mit dem Host/Cluster anzugeben.
1. Sie können entweder jeweils **ein einzelnes Element** oder **mehrere Elemente** in einem Schritt hinzufügen. Es besteht auch die Möglichkeit, Details zu dem Hyper-V-Host/-Cluster über **CSV importieren** bereitzustellen.

    ![Auswahloptionen zum Hinzufügen der Ermittlungsquelle](./media/tutorial-assess-hyper-v/add-discovery-source-hyperv.png)

    - Wenn Sie **Einzelnes Element hinzufügen** auswählen, müssen Sie den Anzeigenamen für die Anmeldeinformationen und **IP-Adresse/FQDN** des Hyper-V-Hosts/-Clusters hinzufügen und dann auf **Speichern** klicken.
    - Wenn Sie **Mehrere Elemente hinzufügen** auswählen _(standardmäßig ausgewählt)_ , können Sie mehrere Datensätze auf einmal hinzufügen, indem Sie **IP-Adresse/FQDN** des Hyper-V-Hosts/-Clusters mit dem Anzeigenamen für die Anmeldeinformationen im Textfeld angeben. **Überprüfen** Sie die hinzugefügten Datensätze, und klicken Sie auf **Speichern**.
    - Wenn Sie **CSV importieren** auswählen, können Sie eine CSV-Vorlagendatei herunterladen und die Datei mit **IP-Adresse/FQDN** des Hyper-V-Hosts/-Clusters und einem Anzeigenamen für die Anmeldeinformationen auffüllen. Importieren Sie die Datei dann in die Appliance, **überprüfen** Sie die Datensätze in der Datei, und klicken Sie auf **Speichern**.

1. Wenn Sie auf „Speichern“ klicken, versucht die Appliance, die Verbindung zu den hinzugefügten Hyper-V-Hosts/-Clustern zu überprüfen, und zeigt den **Überprüfungszustand** für jeden Host/Cluster in der Tabelle an.
    - Wenn die Hosts/Cluster erfolgreich überprüft werden, können Sie weitere Details anzeigen, indem Sie auf die zugehörigen IP-Adressen/FQDNs klicken.
    - Sollte bei der Überprüfung eines Hosts ein Fehler auftreten, sehen Sie sich den Fehler an, indem Sie auf **Fehler bei der Überprüfung** in der Spalte „Status“ klicken. Beheben Sie das Problem, und wiederholen Sie die Überprüfung.
    - Wenn Sie Hosts oder Cluster entfernen möchten, klicken Sie auf **Löschen**.
    - Es ist nicht möglich, einen bestimmten Host aus einem Cluster zu entfernen. Sie können nur den gesamten Cluster entfernen.
    - Ein Cluster kann auch dann hinzugefügt werden, wenn Probleme mit bestimmten, in dem Cluster enthaltenen Hosts vorliegen.
1. Sie können die Verbindung mit Hosts/Clustern jederzeit vor Beginn der Ermittlung **erneut überprüfen**.
1. Klicken Sie auf **Ermittlung starten**, um die VM-Ermittlung von den erfolgreich überprüften Hosts/Clustern aus zu starten. Nachdem die Ermittlung erfolgreich gestartet wurde, können Sie den Ermittlungsstatus für jeden Host/Cluster in der Tabelle überprüfen.

Daraufhin wird die Ermittlung gestartet. Es dauert ca. 2 Minuten pro Host, bis Metadaten des ermittelten Servers im Azure-Portal angezeigt werden.

### <a name="verify-vms-in-the-portal"></a>Überprüfen virtueller Computer im Portal

Nach Abschluss der Ermittlung können Sie überprüfen, ob die VMs im Portal angezeigt werden.

1. Öffnen Sie das Azure Migrate-Dashboard.
2. Klicken Sie unter **Azure Migrate – Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf das Symbol mit der Anzahl für **Ermittelte Server**.

## <a name="set-up-an-assessment"></a>Einrichten einer Bewertung

Mit der Azure Migrate-Serverbewertung können zwei Arten von Bewertungen ausgeführt werden.

**Bewertung** | **Details** | **Daten**
--- | --- | ---
**Leistungsbasiert** | Bewertungen basierend auf gesammelten Leistungsdaten | **Empfohlene VM-Größe**: Basierend auf CPU- und Arbeitsspeicher-Nutzungsdaten<br/><br/> **Empfohlener Datenträgertyp (Verwalteter Datenträger vom Typ Standard oder Premium)** : Basierend auf IOPS und Durchsatz der lokalen Datenträger
**Wie lokal** | Bewertungen basierend auf lokaler Größenanpassung | **Empfohlene VM-Größe**: Basierend auf der Größe der lokalen VM<br/><br> **Empfohlener Datenträgertyp**: Basierend auf der für die Bewertung ausgewählten Speichertypeinstellung



### <a name="run-an-assessment"></a>Durchführen einer Bewertung

Führen Sie eine Bewertung wie folgt aus:

1. Machen Sie sich mit den [bewährten Methoden](best-practices-assessment.md) für die Bewertungserstellung vertraut.
2. Klicken Sie unter **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Bewerten**.

    ![Bewerten](./media/tutorial-assess-hyper-v/assess.png)

3. Geben Sie unter **Server bewerten** einen Namen für die Bewertung an.
4. Klicken Sie auf **Alle anzeigen**, um die Eigenschaften für die Bewertung zu überprüfen.

    ![Bewertungseigenschaften](./media/tutorial-assess-hyper-v/assessment-properties.png)

3. Wählen Sie unter **Gruppe auswählen oder erstellen** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Gruppe ein. Eine Gruppe enthält mindestens eine zu bewertende VM.
4. Wählen Sie unter **Computer zur Gruppe hinzufügen** die VMs aus, die der Gruppe hinzugefügt werden sollen.
5. Klicken Sie auf **Bewertung erstellen**, um die Gruppe zu erstellen und die Bewertung auszuführen.

    ![Erstellen einer Bewertung](./media/tutorial-assess-hyper-v/assessment-create.png)

6. Zeigen Sie die erstellte Bewertung unter **Server** > **Azure Migrate: Serverbewertung** aus.
7. Klicken Sie auf **Bewertung exportieren**, um sie als Excel-Datei herunterzuladen.


## <a name="review-an-assessment"></a>Überprüfen einer Bewertung

Eine Bewertung beschreibt Folgendes:

- **Azure-Bereitschaft**: Gibt an, ob VMs für die Migration zu Azure geeignet sind.
- **Geschätzte monatliche Kosten**: Die geschätzten monatlichen Compute- und Speicherkosten für die Ausführung der VMs in Azure.
- **Geschätzte monatliche Speicherkosten**: Die geschätzten Kosten für den Datenträgerspeicher nach der Migration.


### <a name="view-an-assessment"></a>Anzeigen einer Bewertung

1. Klicken Sie unter **Migrationsziele** >  **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Bewertungen**.
2. Klicken Sie unter **Bewertungen** auf eine Bewertung, um sie zu öffnen.

    ![Zusammenfassung der Bewertung](./media/tutorial-assess-hyper-v/assessment-summary.png)


### <a name="review-azure-readiness"></a>Überprüfen der Azure-Bereitschaft

1. Überprüfen Sie unter **Azure-Bereitschaft**, ob VMs für die Migration zu Azure bereit sind.
2. Überprüfen Sie den VM-Status:
    - **Bereit für Azure**: Azure Migrate empfiehlt in der Bewertung eine VM-Größe und gibt Kostenschätzungen für VMs ab.
    - **Bereit mit Bedingungen**: Zeigt Probleme und eine vorgeschlagene Abhilfe an.
    - **Nicht bereit für Azure**: Zeigt Probleme und eine vorgeschlagene Abhilfe an.
    - **Bereitschaft unbekannt**: Wird verwendet, wenn Azure Migrate die Bereitschaft aufgrund von Problemen mit der Datenverfügbarkeit nicht bewerten kann.

2. Klicken Sie auf einen **Azure-Bereitschaftsstatus**. Sie können Details zur VM-Bereitschaft sowie VM-Details wie Compute-, Speicher- und Netzwerkeinstellungen anzeigen.

### <a name="review-cost-details"></a>Überprüfen der Kostendetails

In dieser Ansicht werden die geschätzten Compute- und Speicherkosten für die Ausführung der VMs in Azure angezeigt.

1. Überprüfen Sie die monatlichen Compute- und Speicherkosten. Die Kosten für alle VMs in der bewerteten Gruppe werden aggregiert.

    - Kostenschätzungen basieren auf den Größenempfehlungen für einen Computer sowie auf seinen Datenträgern und Eigenschaften.
    - Die geschätzten monatlichen Kosten für Compute und Speicher werden angezeigt.
    - Die Kostenschätzung gilt für die Ausführung der lokalen VMs als IaaS-VMs. PaaS- oder SaaS-Kosten werden von der Azure Migrate-Serverbewertung nicht berücksichtigt.

2. Sie können die geschätzten monatlichen Speicherkosten überprüfen. Diese Ansicht zeigt aggregierte Speicherkosten für die bewertete Gruppe, aufgeschlüsselt nach verschiedenen Arten von Speicherdatenträgern.
3. Sie können einen Drilldown ausführen, um die Details für bestimmte VMs anzuzeigen.


### <a name="review-confidence-rating"></a>Prüfen der Zuverlässigkeitsstufe

Wenn Sie leistungsbasierte Bewertungen ausführen, wird der Bewertung eine Zuverlässigkeitsstufe zugewiesen.

![Zuverlässigkeitsstufe](./media/tutorial-assess-hyper-v/confidence-rating.png)

- Es wird eine Bewertung zwischen einem Stern (niedrigste Stufe) und fünf Sternen (höchste Stufe) vergeben.
- Anhand der Zuverlässigkeitsstufe können Sie die Zuverlässigkeit der von der Bewertung bereitgestellten Größenempfehlungen besser einschätzen.
- Die Zuverlässigkeitsstufe basiert auf der Verfügbarkeit von Datenpunkten, die zum Berechnen der Bewertung erforderlich sind.

Die Zuverlässigkeitsstufen für eine Bewertung sehen wie folgt aus:

**Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
--- | ---
0 % bis 20 % | Ein Stern
21 % bis 40 % | Zwei Sterne
41 % bis 60 % | Drei Sterne
61 % bis 80 % | Vier Sterne
81 % bis 100 % | Fünf Sterne

Weitere Informationen zu bewährten Methoden für Zuverlässigkeitsstufen finden Sie [hier](best-practices-assessment.md#best-practices-for-confidence-ratings).





## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Einrichten einer Azure Migrate-Appliance
> * Erstellen und Überprüfen einer Bewertung

Im dritten Tutorial der Reihe erfahren Sie, wie Sie Hyper-V-VMs mithilfe der Azure Migrate-Servermigration zu Azure migrieren.

> [!div class="nextstepaction"]
> [Migrieren von virtuellen Hyper-V-Computern zu Azure](./tutorial-migrate-hyper-v.md)
