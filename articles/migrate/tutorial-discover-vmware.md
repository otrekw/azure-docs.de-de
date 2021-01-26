---
title: Ermitteln von VMware-VMs mit der Azure Migrate-Serverbewertung
description: Hier erfahren Sie, wie Sie lokale virtuelle VMware-Computer mit dem Azure Migrate-Serverbewertungstool ermitteln.
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: tutorial
ms.date: 9/14/2020
ms.custom: mvc
ms.openlocfilehash: 0e06d82c30743a4084cfc5ff856b4a9c8d548146
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2021
ms.locfileid: "98566941"
---
# <a name="tutorial-discover-vmware-vms-with-server-assessment"></a>Tutorial: Ermitteln virtueller VMware-Computer mit der Serverbewertung

Im Rahmen der Migration zu Azure ermitteln Sie den lokalen Bestand und die lokalen Workloads.

In diesem Tutorial wird gezeigt, wie Sie lokale virtuelle VMware-Computer mit dem Azure Migrate- Serverbewertungstool und einer einfachen Azure Migrate-Appliance ermitteln. Sie stellen die Appliance als virtuellen VMware-Computer bereit, um kontinuierlich virtuelle Computer und ihre Leistungsmetadaten, auf virtuellen Computern ausgeführte Anwendungen und VM-Abhängigkeiten zu ermitteln.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten eines Azure-Kontos
> * Vorbereiten der VMware-Umgebung für die Ermittlung
> * Erstellen eines Azure Migrate-Projekts
> * Einrichten der Azure Migrate-Appliance
> * Starten der kontinuierlichen Ermittlung

> [!NOTE]
> In Tutorials wird der schnellste Weg zum Ausprobieren eines Szenarios beschrieben, und nach Möglichkeit werden dabei die Standardoptionen verwendet.  

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen, überprüfen Sie, ob die folgenden Voraussetzungen erfüllt sind:


**Anforderung** | **Details**
--- | ---
**vCenter Server-/ESXi-Host** | Sie benötigen eine vCenter Server-Instanz mit Version 5.5, 6.0, 6.5 oder 6.7.<br/><br/> Die virtuellen Computer müssen auf einem ESXi-Host mit Version 5.5 oder höher gehostet werden.<br/><br/> Lassen Sie in der vCenter Server-Instanz eingehende Verbindungen über den TCP-Port 443 zu, damit die Appliance Konfigurations- und Leistungsmetadaten sammeln kann.<br/><br/> Die Appliance stellt standardmäßig über Port 443 eine Verbindung mit vCenter her. Wenn die vCenter Server-Instanz an einem anderen Port lauscht, können Sie den Port beim Angeben der vCenter Server-Details im Appliance-Konfigurations-Manager ändern.<br/><br/> Vergewissern Sie sich, dass auf dem ESXi-Server, der die virtuellen Computer hostet, der eingehende Zugriff an TCP-Port 443 zugelassen wurde, um die auf den virtuellen Computern installierten Anwendungen und VM-Abhängigkeiten zu ermitteln.
**Appliance** | vCenter Server benötigt Ressourcen, um einen virtuellen Computer für die Azure Migrate-Appliance zuzuordnen:<br/><br/> - 32 GB RAM, acht vCPUs und ca. 80 GB Speicherplatz auf dem Datenträger<br/><br/> - Einen externen virtuellen Switch und Internetzugriff für die Appliance-VM (direkt oder über einen Proxy)
**VMs** | Für die Ermittlung von Konfigurations- und Leistungsmetadaten sowie die Ermittlung von auf virtuellen Computern installierten Anwendungen werden alle Windows- und Linux-Betriebssystemversionen unterstützt. <br/><br/> [Hier](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) finden Sie Informationen zu den Betriebssystemversionen, die für die Abhängigkeitsanalyse ohne Agent unterstützt werden.<br/><br/> Zum Ermitteln installierter Anwendungen und VM-Abhängigkeiten müssen VMware-Tools (höher als 10.2.0) auf virtuellen Computern installiert sein und ausgeführt werden, und auf virtuellen Windows-Computern muss mindestens PowerShell-Version 2.0 installiert sein.


## <a name="prepare-an-azure-user-account"></a>Vorbereiten eines Azure-Benutzerkontos

Zum Erstellen eines Azure Migrate-Projekts und Registrieren der Azure Migrate-Appliance benötigen Sie ein Konto mit den folgenden Berechtigungen:
- Berechtigungen vom Typ „Mitwirkender“ oder „Besitzer“ für das Azure-Abonnement
- Berechtigungen zum Registrieren von AAD-Apps (Azure Active Directory)
- Berechtigungen vom Typ „Besitzer“ oder „Mitwirkender“ sowie Berechtigungen vom Typ „Benutzerzugriffsadministrator“ für das Azure-Abonnement, um einen Schlüsseltresor zu erstellen, der bei der VMware-Migration ohne Agent verwendet wird

Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements. Wenn Sie nicht der Besitzer des Abonnements sind, müssen Sie mit dem Besitzer zusammenarbeiten, um die Berechtigungen wie folgt zuzuweisen:

1. Suchen Sie im Azure-Portal nach „Abonnements“, und wählen Sie unter **Dienste** die Option **Abonnements** aus.

    ![Suchfeld, um nach dem Azure-Abonnement zu suchen](./media/tutorial-discover-vmware/search-subscription.png)

2. Wählen Sie auf der Seite **Abonnements** das Abonnement aus, in dem Sie ein Azure Migrate-Projekt erstellen möchten. 
3. Wählen Sie im Abonnement die Option **Zugriffssteuerung (IAM)**  > **Zugriff überprüfen** aus.
4. Suchen Sie unter **Zugriff überprüfen** nach dem entsprechenden Benutzerkonto.
5. Klicken Sie unter **Rollenzuweisung hinzufügen** auf **Hinzufügen**.

    ![Suchen nach einem Benutzerkonto, um den Zugriff zu überprüfen und eine Rolle zuzuweisen](./media/tutorial-discover-vmware/azure-account-access.png)

6. Wählen Sie unter **Rollenzuweisung hinzufügen** die Rolle „Mitwirkender“ oder „Besitzer“ und das Konto aus (in unserem Beispiel „azmigrateuser“). Klicken Sie anschließend auf **Speichern**.

    ![Die Seite „Rollenzuweisung“ wird geöffnet, auf der Sie dem Konto eine Rolle zuweisen können.](./media/tutorial-discover-vmware/assign-role.png)

1. Ihr Azure-Konto benötigt **Berechtigungen zum Registrieren von AAD-Apps**, um die Appliance registrieren zu können.
1. Navigieren Sie im Azure-Portal zu **Azure Active Directory** > **Benutzer** > **Benutzereinstellungen**.
1. Vergewissern Sie sich unter **Benutzereinstellungen**, dass Azure AD-Benutzer Anwendungen registrieren können (standardmäßig auf **Ja** festgelegt).

    ![Überprüfen unter „Benutzereinstellungen“, ob Benutzer Active Directory-Apps registrieren können](./media/tutorial-discover-vmware/register-apps.png)

9. Wenn die Einstellungen für „App-Registrierungen“ auf „Nein“ festgelegt sind, fordern Sie den Mandantenadministrator/globalen Administrator auf, die erforderliche Berechtigung zuzuweisen. Alternativ kann der Mandantenadministrator/globale Administrator einem Konto die Rolle **Anwendungsentwickler** zuweisen, um die Registrierung von AAD-Apps zuzulassen. [Weitere Informationen](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md)

## <a name="prepare-vmware"></a>Vorbereiten von VMware

Vergewissern Sie sich in vCenter Server, dass Ihr Konto über Berechtigungen zum Erstellen eines virtuellen Computers mit einer OVA-Datei verfügt. Dies ist erforderlich, wenn Sie die Azure Migrate-Appliance unter Verwendung einer OVA-Datei als virtuellen VMware-Computer bereitstellen.

Die Serverbewertung erfordert ein schreibgeschütztes vCenter Server-Konto für die Ermittlung und Bewertung virtueller VMware-Computer. Wenn Sie außerdem installierte Anwendungen und VM-Abhängigkeiten ermitteln möchten, müssen für das Konto unter **Virtual Machines > Gastvorgänge** entsprechende Berechtigungen aktiviert werden.

### <a name="create-an-account-to-access-vcenter"></a>Erstellen eines Kontos für den Zugriff auf vCenter

Richten Sie im vSphere-Webclient wie folgt ein Konto ein:

1. Wählen Sie unter Verwendung eines Kontos mit Administratorrechten im vSphere-Webclient **Verwaltung** aus.
2. Wählen Sie für den Zugriff unter **Einmaliges Anmelden** die Option **Benutzer und Gruppen** aus.
3. Fügen Sie unter **Benutzer** einen neuen Benutzer hinzu.
4. Geben Sie unter **Neuer Benutzer** die Kontodetails ein. Klicken Sie dann auf **OK**.
5. Wählen Sie unter **Globale Berechtigungen** das Benutzerkonto aus, und weisen Sie ihm die Rolle **Schreibgeschützt** zu. Klicken Sie dann auf **OK**.
6. Sollen darüber hinaus installierte Anwendungen und VM-Abhängigkeiten ermittelt werden, navigieren Sie zu **Rollen**, und wählen Sie die Rolle **Schreibgeschützt** und anschließend unter **Berechtigungen** die Option **Gastvorgänge** aus. Sie können die Berechtigungen für alle Objekte unter vCenter Server weitergeben, indem Sie das Kontrollkästchen „Propagate to children“ (An untergeordnete Objekte weitergeben) aktivieren.
 
    ![Kontrollkästchen zum Zulassen von Gastvorgängen für die Rolle „Schreibgeschützt“](./media/tutorial-discover-vmware/guest-operations.png)


### <a name="create-an-account-to-access-vms"></a>Erstellen eines Kontos für den Zugriff auf virtuelle Computer

Sie benötigen ein Benutzerkonto mit den erforderlichen Berechtigungen auf den virtuellen Computern, um installierte Anwendungen und VM-Abhängigkeiten zu ermitteln. Sie können das Benutzerkonto im Appliance-Konfigurations-Manager angeben. Die Appliance installiert keine Agents auf den virtuellen Computern.

1. Erstellen Sie für virtuelle Windows-Computer ein Konto (lokales Konto oder Domänenkonto) mit Administratorberechtigungen auf den virtuellen Computern.
2. Erstellen Sie für virtuelle Linux-Computer ein Konto mit Root-Berechtigungen. Alternativ können Sie ein Konto mit den folgenden Berechtigungen in den Dateien „/bin/netstat“ und „/bin/ls“ erstellen: CAP_DAC_READ_SEARCH und CAP_SYS_PTRACE.

> [!NOTE]
> Derzeit unterstützt Azure Migrate ein Benutzerkonto für virtuelle Windows-Computer und ein Benutzerkonto für virtuelle Linux-Computer, die auf der Appliance zur Ermittlung von installierten Anwendungen und VM-Abhängigkeiten angegeben werden können.


## <a name="set-up-a-project"></a>Einrichten eines Projekts

Richten Sie ein neues Azure Migrate-Projekt ein.

1. Wählen Sie im Azure-Portal **Alle Dienste** aus, und suchen Sie nach **Azure Migrate**.
2. Wählen Sie unter **Dienste** die Option **Azure Migrate** aus.
3. Wählen Sie unter **Übersicht** die Option **Create project** (Projekt erstellen) aus.
5. Wählen Sie unter **Create project** (Projekt erstellen) Ihr Azure-Abonnement und die Ressourcengruppe aus. Falls noch keine vorhanden ist, erstellen Sie eine Ressourcengruppe.
6. Geben Sie unter **Projektdetails** den Projektnamen und die geografische Region an, in der Sie das Projekt erstellen möchten. Beachten Sie die unterstützten geografischen Regionen für [öffentliche Clouds](migrate-support-matrix.md#supported-geographies-public-cloud) und [Azure Government-Clouds](migrate-support-matrix.md#supported-geographies-azure-government).

   ![Felder für Projektname und Region](./media/tutorial-discover-vmware/new-project.png)

7. Klicken Sie auf **Erstellen**.
8. Warten Sie einige Minuten, bis das Azure Migrate-Projekt bereitgestellt wurde. Das Tool **Azure Migrate: Serverbewertung** wird dem neuen Projekt standardmäßig hinzugefügt.

![Seite mit dem standardmäßig hinzugefügten Serverbewertungstool](./media/tutorial-discover-vmware/added-tool.png)

> [!NOTE]
> Wenn Sie bereits ein Projekt erstellt haben, können Sie dieses Projekt zum Registrieren zusätzlicher Appliances verwenden, um mehr virtuelle Computer zu ermitteln und zu bewerten. [Weitere Informationen](create-manage-projects.md#find-a-project)

## <a name="set-up-the-appliance"></a>Einrichten der Appliance

Von der Azure Migrate-Serverbewertung wird eine einfache Azure Migrate-Appliance verwendet. Die Appliance ermittelt virtuelle Computer und sendet Konfigurations- und Leistungsmetadaten zu virtuellen Computern an Azure Migrate. Die Appliance kann durch Bereitstellen einer OVA-Vorlage eingerichtet werden, die aus dem Azure Migrate-Projekt heruntergeladen werden kann.

> [!NOTE]
> Wenn Sie die Appliance aus irgendeinem Grund nicht mithilfe der Vorlage einrichten können, können Sie für die Einrichtung ein PowerShell-Skript auf einem vorhandenen Windows Server 2016-Server verwenden. [Weitere Informationen](deploy-appliance-script.md#set-up-the-appliance-for-vmware)


### <a name="deploy-with-ova"></a>Bereitstellen mit OVA

Gehen Sie zum Einrichten der Appliance mithilfe einer OVA-Vorlage wie folgt vor:
1. Geben Sie einen Appliancenamen ein, und generieren Sie einen Azure Migrate-Projektschlüssel im Portal.
1. Herunterladen einer OVA-Vorlagendatei und Importieren der Datei in vCenter Server Vergewissern Sie sich, dass die OVA sicher ist.
1. Erstellen der Appliance und Überprüfen der Verbindungsherstellung mit der Azure Migrate-Serverbewertung
1. Führen Sie eine Erstkonfiguration für die Appliance aus, und registrieren Sie die Appliance beim Azure Migrate-Projekt unter Verwendung des Azure Migrate-Projektschlüssels.

### <a name="1-generate-the-azure-migrate-project-key"></a>1. Generieren des Azure Migrate-Projektschlüssels

1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Ermitteln**.
2. Wählen Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere Hypervisor** aus.
3. Geben Sie in **1: Generieren eines Azure Migrate-Projektschlüssels** einen Namen für die Azure Migrate-Appliance an, die Sie für die Ermittlung von VMware-VMs einrichten möchten. Der Name muss alphanumerisch sein und darf höchstens 14 Zeichen enthalten.
1. Klicken Sie auf **Schlüssel generieren**, um mit der Erstellung der erforderlichen Azure-Ressourcen zu beginnen. Schließen Sie die Seite „Computer ermitteln“ nicht, während die Ressourcen erstellt werden.
1. Nach der erfolgreichen Erstellung der Azure-Ressourcen wird ein **Azure Migrate-Projektschlüssel** generiert.
1. Kopieren Sie den Schlüssel, da Sie ihn benötigen, um die Registrierung der Appliance während der Konfiguration abzuschließen.

### <a name="2-download-the-ova-template"></a>2. Herunterladen der OVA-Vorlage

Wählen Sie in **2: Azure Migrate-Appliance herunterladen** die OVA-Datei aus, und klicken Sie auf **Herunterladen**.

### <a name="verify-security"></a>Überprüfen der Sicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die OVA-Datei sicher ist:

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:
  
   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
   
   Beispielverwendung: ```C:\>CertUtil -HashFile C:\Users\Administrator\Desktop\MicrosoftAzureMigration.ova SHA256```

3. Überprüfen Sie die aktuellen Applianceversionen und Hashwerte:

    - Öffentliche Azure-Cloud:
    
        **Algorithmus** | **Download** | **SHA256**
        --- | --- | ---
        VMware (11,9 GB) | [Aktuelle Version](https://go.microsoft.com/fwlink/?linkid=2140333) | e9c9a1fe4f3ebae81008328e8f3a7933d78ff835ecd871d1b17f367621ce3c74

    - Azure Government:
    
        **Algorithmus** | **Download** | **SHA256**
        --- | --- | ---
        VMware (85,8 MB) | [Aktuelle Version](https://go.microsoft.com/fwlink/?linkid=2140337) | 2daaa2a59302bf911e8ef195f8add7d7c8352de77a9af0b860e2a627979085ca

### <a name="3-create-the-appliance-vm"></a>3. Erstellen der Appliance-VM

Importieren Sie die heruntergeladene Datei, und erstellen Sie eine VM.

1. Klicken Sie in der vSphere-Clientkonsole auf **Datei** > **Deploy OVF Template** (OVF-Vorlage bereitstellen).
2. Geben Sie im Assistenten zum Bereitstellen der OVF-Vorlage unter **Quelle** den Speicherort der OVA-Datei an.
3. Geben Sie unter **Name** und **Standort** einen Anzeigenamen für die VM an. Wählen Sie das Inventarobjekt aus, in dem die VM gehostet wird.
5. Geben Sie unter **Host/Cluster** den Host oder Cluster an, auf bzw. in dem die VM ausgeführt wird.
6. Geben Sie unter **Speicher** das Speicherziel für die VM an.
7. Geben Sie unter **Datenträgerformat** den Typ und die Größe des Datenträgers an.
8. Geben Sie unter **Netzwerkzuordnung** das Netzwerk an, mit dem die VM eine Verbindung herstellt. Das Netzwerk muss über eine Internetverbindung verfügen, um Metadaten an die Azure Migrate-Serverbewertung senden zu können.
9. Überprüfen Sie die Einstellungen, und klicken Sie dann auf **Fertig stellen**.


### <a name="verify-appliance-access-to-azure"></a>Überprüfen des Appliancezugriffs auf Azure

Stellen Sie sicher, dass die Appliance-VM eine Verbindung mit Azure-URLs für [öffentliche](migrate-appliance.md#public-cloud-urls) und [Government](migrate-appliance.md#government-cloud-urls)-Clouds herstellen kann.


### <a name="4-configure-the-appliance"></a>4. Konfigurieren der Appliance

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
1. Für die Authentifizierung bei Azure benötigen Sie einen Gerätecode. Wenn Sie auf **Anmelden** klicken, wird ein modales Dialogfeld mit dem Gerätecode angezeigt. Dies ist in der folgenden Abbildung dargestellt.

    ![Modales Dialogfeld mit Gerätecode](./media/tutorial-discover-vmware/device-code.png)

1. Klicken Sie auf **Copy code & Login** (Code kopieren und anmelden), um den Gerätecode zu kopieren und eine Azure-Anmeldeaufforderung in einer neuen Browserregisterkarte zu öffnen. Sollte keine Anmeldung angezeigt werden, vergewissern Sie sich, dass Sie den Popupblocker im Browser deaktiviert haben.
1. Fügen Sie auf der neuen Registerkarte den Gerätecode ein, und melden Sie sich mit Ihrem Azure-Benutzernamen und dem zugehörigen Kennwort an.
   
   Die Anmeldung mit einer PIN wird nicht unterstützt.
3. Falls Sie die Registerkarte für die Anmeldung versehentlich schließen, ohne die Anmeldung durchzuführen, müssen Sie die Browserregisterkarte des Appliance-Konfigurations-Managers aktualisieren, um die Schaltfläche „Anmelden“ wieder zu aktivieren.
1. Wechseln Sie nach der erfolgreichen Anmeldung wieder zur vorherigen Registerkarte mit dem Appliance-Konfigurations-Manager.
1. Wenn das für die Protokollierung verwendete Azure-Benutzerkonto über die richtigen Berechtigungen für die während der Schlüsselgenerierung erstellten Azure-Ressourcen verfügt, wird die Registrierung der Appliance initiiert.
1. Nachdem die Appliance erfolgreich registriert wurde, können Sie die Registrierungsdetails anzeigen, indem Sie auf **Details anzeigen** klicken.



## <a name="start-continuous-discovery"></a>Starten der kontinuierlichen Ermittlung

Die Appliance muss eine Verbindung mit der vCenter Server-Instanz herstellen, um die Konfigurations- und Leistungsdaten der VMs zu ermitteln.

1. Wählen Sie in **Schritt 1: Geben Sie die vCenter Server-Anmeldeinformationen** ein, klicken Sie auf **Anmeldeinformationen hinzufügen**, um einen Anzeigenamen für die Anmeldeinformationen anzugeben, fügen Sie **Benutzername** und **Kennwort** für das vCenter Server-Konto hinzu, das die Appliance verwendet, um VMs auf der vCenter Server-Instanz zu ermitteln.
    - Sie sollten im vorherigen Tutorial ein Konto mit den erforderlichen Berechtigungen eingerichtet haben.
    - Wenn Sie die Ermittlung auf bestimmte VMware-Objekte (vCenter Server-Rechenzentren, Cluster, einen Ordner mit Clustern, Hosts, einen Ordner mit Hosts oder einzelne VMs) begrenzen möchten, hilft Ihnen die Anleitung in [diesem Artikel](set-discovery-scope.md) beim Einschränken des von Azure Migrate genutzten Kontos weiter.
1. Geben Sie in **Step 2: Geben Sie vCenter Server-Details** ein, klicken Sie auf **Ermittlungsquelle hinzufügen**, um den Anzeigenamen für die Anmeldeinformationen aus der Dropdownliste auszuwählen, geben Sie den **IP-Adresse/FQDN**-Wert der vCenter Server-Instanz an. Sie können für **Port** den Standardwert (443) beibehalten oder einen benutzerdefinierten Port angeben, den vCenter Server überwacht, und dann auf **Speichern** klicken.
1. Wenn Sie auf „Speichern“ klicken, versucht die Appliance, die Verbindung zum vCenter Server mit den angegebenen Anmeldeinformationen zu überprüfen, und zeigt den **Überprüfungszustand** in der Tabelle für die vCenter Server-IP-Adresse/FQDN an.
1. Sie können die Verbindung zu vCenter Server jederzeit vor Beginn der Ermittlung **erneut überprüfen**.
1. Wählen Sie in **Schritt 3: Geben Sie VM-Anmeldeinformationen an, um installierte Anwendungen zu ermitteln und eine Abhängigkeitszuordnung ohne Agent** durchzuführen, klicken Sie auf **Anmeldeinformationen hinzufügen**, und geben Sie das Betriebssystem, für das die Anmeldeinformationen bereitgestellt werden, einen Anzeigenamen für die Anmeldeinformationen sowie den **Benutzernamen** und das **Kennwort** an. Klicken Sie anschließend auf **Speichern**.

    - Hier können Sie optional Anmeldeinformationen hinzufügen, wenn Sie ein Konto erstellt haben, das für [Anwendungsermittlung](how-to-discover-applications.md) oder [Abhängigkeitsanalyse ohne Agent](how-to-create-group-machine-dependencies-agentless.md) verwendet werden soll.
    - Wenn Sie diese Features nicht verwenden möchten, können Sie auf den Schieberegler klicken, um den Schritt zu überspringen. Sie können die Absicht jederzeit später rückgängig machen.
    - Sehen Sie sich die Berechtigungen an, die für das Konto für die [Anwendungsermittlung](migrate-support-matrix-vmware.md#application-discovery-requirements) oder für die [Abhängigkeitsanalyse ohne Agent](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) erforderlich sind.

5. Klicken Sie auf **Ermittlung starten**, um die VM-Ermittlung zu starten. Nachdem die Ermittlung erfolgreich gestartet wurde, können Sie den Ermittlungsstatus für die vCenter Server-IP-Adresse/FQDN in der Tabelle überprüfen.

Die Ermittlung funktioniert wie folgt:
- Es dauert ca. 15 Minuten, bis die ermittelten VM-Metadaten im Portal angezeigt werden.
- Die Ermittlung installierter Anwendungen, Rollen und Features dauert etwas. Die Dauer hängt davon ab, wie viele virtuelle Computer ermittelt werden. Bei 500 VMs dauert es ungefähr ein Stunde, bis der Anwendungsbestand im Azure Migrate-Portal angezeigt wird.
- Wenn die Ermittlung virtueller Computer abgeschlossen ist, können Sie die Abhängigkeitsanalyse ohne Agent auf den gewünschten virtuellen Computern über das Portal aktivieren.


## <a name="next-steps"></a>Nächste Schritte

- [Bewerten Sie virtuelle VMware-Computer](./tutorial-assess-vmware-azure-vm.md) für die Migration zu virtuellen Azure-Computern.
- [Informieren Sie sich über die Daten](migrate-appliance.md#collected-data---vmware), die von der Appliance im Zuge der Ermittlung gesammelt werden.