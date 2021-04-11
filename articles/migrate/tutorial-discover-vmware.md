---
title: Ermitteln von in einer VMware-Umgebung ausgeführten Servern mit der Ermittlung und Bewertung von Azure Migrate
description: Hier erfahren Sie, wie Sie in einer VMware-Umgebung ausgeführte lokale Server mit dem Azure Migrate-Tool zur Ermittlung und Bewertung ermitteln.
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: tutorial
ms.date: 03/17/2021
ms.custom: mvc
ms.openlocfilehash: f394fd4b1b4124c259489580fb5dc320fedf73fa
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104863648"
---
# <a name="tutorial-discover-servers-running-in-vmware-environment-with-azure-migrate-discovery-and-assessment"></a>Tutorial: Ermitteln von in einer VMware-Umgebung ausgeführten Servern mit der Ermittlung und Bewertung von Azure Migrate

Im Rahmen der Migration zu Azure ermitteln Sie den lokalen Bestand und die lokalen Workloads.

In diesem Tutorial wird gezeigt, wie Sie in einer VMware-Umgebung ausgeführte Server mit dem Azure Migrate-Tool zur Ermittlung und Bewertung und einer schlanken Azure Migrate-Appliance ermitteln. Sie stellen die Appliance als Server bereit, der in Ihrer vCenter Server-Instanz läuft, um Server und deren Leistungsmetadaten, auf Servern laufende Anwendungen, Serverabhängigkeiten und SQL Server-Instanzen und -Datenbanken kontinuierlich zu ermitteln.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten eines Azure-Kontos
> * Vorbereiten der VMware-Umgebung für die Ermittlung
> * Erstellt ein Projekt.
> * Einrichten der Azure Migrate-Appliance
> * Starten der kontinuierlichen Ermittlung

> [!NOTE]
> In Tutorials wird der schnellste Weg zum Ausprobieren eines Szenarios beschrieben, und nach Möglichkeit werden dabei die Standardoptionen verwendet.  

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen, überprüfen Sie, ob die folgenden Voraussetzungen erfüllt sind:


**Anforderung** | **Details**
--- | ---
**vCenter Server-/ESXi-Host** | Sie benötigen eine vCenter Server-Instanz mit Version 5.5, 6.0, 6.5 oder 6.7.<br/><br/> Server müssen auf einem ESXi-Host mit einer Version ab 5.5 gehostet werden.<br/><br/> Lassen Sie in der vCenter Server-Instanz eingehende Verbindungen über den TCP-Port 443 zu, damit die Appliance Konfigurations- und Leistungsmetadaten sammeln kann.<br/><br/> Die Appliance stellt standardmäßig an Port 443 eine Verbindung mit vCenter Server her. Wenn die vCenter Server-Instanz an einem anderen Port lauscht, können Sie den Port beim Angeben der vCenter Server-Details im Appliance-Konfigurations-Manager ändern.<br/><br/> Stellen Sie auf den ESXi-Hosts sicher, dass der eingehende Zugriff an TCP-Port 443 erlaubt ist, um die Ermittlung installierter Anwendungen und die Abhängigkeitsanalyse ohne Agent auf Servern durchzuführen.
**Appliance** | vCenter Server benötigt Ressourcen, um einen Server für die Azure Migrate-Appliance zuzuteilen:<br/><br/> 32 GB RAM, acht vCPUs und ca. 80 GB Speicherplatz auf dem Datenträger<br/><br/> Einen externen virtuellen Switch und Internetzugriff für den Server auf der Appliance (direkt oder über einen Proxy)
**Server** | Für die Ermittlung von Konfigurations- und Leistungsmetadaten werden alle Windows- und Linux-Betriebssystemversionen unterstützt. <br/><br/> Zur Anwendungsermittlung auf Servern werden alle Windows- und Linux-Betriebssystemversionen unterstützt. [Hier](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) finden Sie Informationen zu den Betriebssystemversionen, die für die Abhängigkeitsanalyse ohne Agent unterstützt werden.<br/><br/> Für die Ermittlung installierter Anwendungen und Abhängigkeitsanalyse ohne Agent müssen VMware Tools (ab 10.2.0) auf den Servern installiert sein und ausgeführt werden. Auf Windows-Servern muss PowerShell ab Version 2.0 installiert sein.<br/><br/> Um SQL Server-Instanzen und -Datenbanken zu ermitteln, finden Sie [hier](migrate-support-matrix-vmware.md#requirements-for-discovery-of-sql-server-instances-and-databases) die unterstützten SQL Server-Versionen und -Editionen, die unterstützten Windows-Betriebssystemversionen und Authentifizierungsmechanismen.

## <a name="prepare-an-azure-user-account"></a>Vorbereiten eines Azure-Benutzerkontos

Zum Erstellen eines Projekts und Registrieren der Azure Migrate-Appliance benötigen Sie ein Konto mit den folgenden Berechtigungen:

- Berechtigungen vom Typ „Mitwirkender“ oder „Besitzer“ für das Azure-Abonnement
- Berechtigungen zum Registrieren von AAD-Apps (Azure Active Directory)
- Berechtigungen des Typs „Besitzer“ oder „Mitwirkender“ sowie Berechtigungen des Typs „Benutzerzugriffsadministrator“ für das Azure-Abonnement, um einen Schlüsseltresor zu erstellen, der zur Servermigration ohne Agent verwendet wird

Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements. Wenn Sie nicht der Besitzer des Abonnements sind, müssen Sie mit dem Besitzer zusammenarbeiten, um die Berechtigungen wie folgt zuzuweisen:

1. Suchen Sie im Azure-Portal nach „Abonnements“, und wählen Sie unter **Dienste** die Option **Abonnements** aus.

    :::image type="content" source="./media/tutorial-discover-vmware/search-subscription.png" alt-text="Suchfeld, um nach dem Azure-Abonnement zu suchen":::


2. Wählen Sie auf der Seite **Abonnements** das Abonnement aus, in dem Sie ein Projekt erstellen möchten.
3. Wählen Sie im Abonnement die Option **Zugriffssteuerung (IAM)**  > **Zugriff überprüfen** aus.
4. Suchen Sie unter **Zugriff überprüfen** nach dem entsprechenden Benutzerkonto.
5. Klicken Sie unter **Rollenzuweisung hinzufügen** auf **Hinzufügen**.
:::image type="content" source="./media/tutorial-discover-vmware/azure-account-access.png" alt-text="Suchen nach einem Benutzerkonto, um den Zugriff zu überprüfen und eine Rolle zuzuweisen":::
    
6. Wählen Sie unter **Rollenzuweisung hinzufügen** die Rolle „Mitwirkender“ oder „Besitzer“ und das Konto aus (in unserem Beispiel „azmigrateuser“). Klicken Sie anschließend auf **Speichern**.

    :::image type="content" source="./media/tutorial-discover-vmware/assign-role.png" alt-text="Die Seite „Rollenzuweisung“ wird geöffnet, auf der Sie dem Konto eine Rolle zuweisen können.":::

1. Ihr Azure-Konto benötigt **Berechtigungen zum Registrieren von AAD-Apps**, um die Appliance registrieren zu können.
1. Navigieren Sie im Azure-Portal zu **Azure Active Directory** > **Benutzer** > **Benutzereinstellungen**.
1. Vergewissern Sie sich unter **Benutzereinstellungen**, dass Azure AD-Benutzer Anwendungen registrieren können (standardmäßig auf **Ja** festgelegt).

    :::image type="content" source="./media/tutorial-discover-vmware/register-apps.png" alt-text="Überprüfen unter „Benutzereinstellungen“, ob Benutzer Active Directory-Apps registrieren können":::

9. Wenn die Einstellungen für „App-Registrierungen“ auf „Nein“ festgelegt sind, fordern Sie den Mandantenadministrator/globalen Administrator auf, die erforderliche Berechtigung zuzuweisen. Alternativ kann der Mandantenadministrator/globale Administrator einem Konto die Rolle **Anwendungsentwickler** zuweisen, um die Registrierung von AAD-Apps zuzulassen. [Weitere Informationen](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md)

## <a name="prepare-vmware"></a>Vorbereiten von VMware

Vergewissern Sie sich in vCenter Server, dass Ihr Konto über Berechtigungen zum Erstellen eines virtuellen Computers mit einer OVA-Datei verfügt. Dies ist erforderlich, wenn Sie die Azure Migrate-Appliance unter Verwendung einer OVA-Datei als virtuellen VMware-Computer bereitstellen.

Azure Migrate benötigt ein vCenter Server-Konto mit Lesezugriff für die Ermittlung und Bewertung von Servern, die in Ihrer VMware-Umgebung laufen. Wenn Sie auch die Ermittlung installierter Anwendungen und die Abhängigkeitsanalyse ohne Agent durchführen möchten, benötigt das Konto für **Virtuelle Maschinen > Gastvorgänge** aktivierte Berechtigungen.

### <a name="create-an-account-to-access-vcenter"></a>Erstellen eines Kontos für den Zugriff auf vCenter

Richten Sie im vSphere-Webclient wie folgt ein Konto ein:

1. Wählen Sie unter Verwendung eines Kontos mit Administratorrechten im vSphere-Webclient **Verwaltung** aus.
2. Wählen Sie für den Zugriff unter **Einmaliges Anmelden** die Option **Benutzer und Gruppen** aus.
3. Fügen Sie unter **Benutzer** einen neuen Benutzer hinzu.
4. Geben Sie unter **Neuer Benutzer** die Kontodetails ein. Klicken Sie dann auf **OK**.
5. Wählen Sie unter **Globale Berechtigungen** das Benutzerkonto aus, und weisen Sie ihm die Rolle **Schreibgeschützt** zu. Klicken Sie dann auf **OK**.
6. Wenn Sie auch die Ermittlung installierter Anwendungen und Abhängigkeitsanalyse ohne Agent durchführen möchten, wechseln Sie zu **Rollen**. Wählen Sie die Rolle **Schreibgeschützt** und in **Berechtigungen** die Option **Gastvorgänge** aus. Sie können die Berechtigungen für alle Objekte unter vCenter Server weitergeben, indem Sie das Kontrollkästchen „Propagate to children“ (An untergeordnete Objekte weitergeben) aktivieren.

    :::image type="content" source="./media/tutorial-discover-vmware/guest-operations.png" alt-text="Kontrollkästchen zum Zulassen von Gastvorgängen für die Rolle „Schreibgeschützt“":::


> [!NOTE]
> Sie können die Ermittlung auf bestimmte vCenter Server-Rechenzentren, Cluster, einen Ordner mit Clustern, Hosts, einen Ordner mit Hosts oder einzelne Server beschränken, indem Sie das vCenter Server-Konto einschränken. [**Erfahren Sie mehr**](set-discovery-scope.md) zum Einschränken des vCenter Server-Benutzerkontos.

### <a name="create-an-account-to-access-servers"></a>Erstellen eines Kontos für den Zugriff auf Server

Sie benötigen ein Benutzerkonto mit den erforderlichen Berechtigungen für die Server, um die Ermittlung installierter Anwendungen, Abhängigkeitsanalyse ohne Agent und Ermittlung von SQL Server-Instanzen und -Datenbanken durchzuführen. Sie können das Benutzerkonto im Appliance-Konfigurations-Manager angeben. Die Appliance installiert keine Agents auf den Servern.

1. Erstellen Sie für Server unter Windows ein Konto (lokales Konto oder Domänenkonto) mit Administratorberechtigungen für die Server. Um SQL Server Instanzen und -Datenbanken zu ermitteln, muss das Windows- oder SQL Server-Konto Mitglied der Serverrolle sysadmin sein. [Erfahren Sie mehr](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/server-level-roles) darüber, wie Sie dem Benutzerkonto die erforderliche Rolle zuweisen.
2. Erstellen Sie für Server unter Linux ein Konto mit Root-Berechtigungen. Alternativ können Sie ein Konto mit den folgenden Berechtigungen in den Dateien „/bin/netstat“ und „/bin/ls“ erstellen: CAP_DAC_READ_SEARCH und CAP_SYS_PTRACE.

> [!NOTE]
> Sie können jetzt dem Konfigurations-Manager mehrere Anmeldeinformationen hinzufügen, um die Ermittlung installierter Anwendungen, Abhängigkeitsanalyse ohne Agent und Ermittlung von SQL Server-Instanzen und -Datenbanken durchzuführen.Sie können Anmeldeinformationen für die Authentifizierung für mehrere Domänen, Windows (ohne Domäne), Linux (ohne Domäne) und/oder SQL Server angeben. [**Weitere Informationen**](add-server-credentials.md)

## <a name="set-up-a-project"></a>Einrichten eines Projekts

Richten Sie ein neues Projekt ein.

1. Wählen Sie im Azure-Portal **Alle Dienste** aus, und suchen Sie nach **Azure Migrate**.
2. Wählen Sie unter **Dienste** die Option **Azure Migrate** aus.
3. Wählen Sie unter **Übersicht** die Option **Create project** (Projekt erstellen) aus.
5. Wählen Sie unter **Create project** (Projekt erstellen) Ihr Azure-Abonnement und die Ressourcengruppe aus. Falls noch keine vorhanden ist, erstellen Sie eine Ressourcengruppe.
6. Geben Sie unter **Projektdetails** den Projektnamen und die geografische Region an, in der Sie das Projekt erstellen möchten. Beachten Sie die unterstützten geografischen Regionen für [öffentliche Clouds](migrate-support-matrix.md#supported-geographies-public-cloud) und [Azure Government-Clouds](migrate-support-matrix.md#supported-geographies-azure-government).

    :::image type="content" source="./media/tutorial-discover-vmware/new-project.png" alt-text="Felder für Projektname und Region":::

7. Klicken Sie auf **Erstellen**.
8. Warten Sie einige Minuten, bis das Projekt bereitgestellt wurde. Das Tool **Azure Migrate: Ermittlung und Bewertung** wird dem neuen Projekt standardmäßig hinzugefügt.

> [!NOTE]
> Wenn Sie bereits ein Projekt erstellt haben, können Sie dieses Projekt zum Registrieren zusätzlicher Appliances verwenden, um mehr Server zu ermitteln und zu bewerten. [**Weitere Informationen**](create-manage-projects.md#find-a-project)

## <a name="set-up-the-appliance"></a>Einrichten der Appliance

Für das Azure Migrate-Tool zur Ermittlung und Bewertung wird eine einfache Azure Migrate-Appliance verwendet. Die Appliance führt die Serverermittlung durch und sendet Metadaten zur Serverkonfiguration und -leistung an Azure Migrate. Die Appliance kann durch Bereitstellen einer OVA-Vorlage eingerichtet werden, die aus dem Projekt heruntergeladen werden kann.

> [!NOTE]
> Wenn Sie die Appliance aus irgendeinem Grund nicht mithilfe der Vorlage einrichten können, können Sie für die Einrichtung ein PowerShell-Skript auf einem vorhandenen Windows Server 2016-Server verwenden. [**Weitere Informationen**](deploy-appliance-script.md#set-up-the-appliance-for-vmware)

### <a name="deploy-with-ova"></a>Bereitstellen mit OVA

Gehen Sie zum Einrichten der Appliance mithilfe einer OVA-Vorlage wie folgt vor:

1. Geben Sie einen Appliancenamen ein, und generieren Sie einen Projektschlüssel im Portal.
1. Herunterladen einer OVA-Vorlagendatei und Importieren der Datei in vCenter Server Vergewissern Sie sich, dass die OVA sicher ist.
1. Erstellen Sie die Appliance-VM anhand der OVA-Datei, und prüfen Sie, ob sie sich mit Azure Migrate verbinden kann.
1. Führen Sie die Erstkonfiguration der Appliance aus, und registrieren Sie sie unter Verwendung des Projektschlüssels im Projekt.

### <a name="1-generate-the-project-key"></a>1. Generieren des Projektschlüssels

1. Wählen Sie unter **Migrationsziele** > **Windows, Linux and SQL Servers (Windows-/Linux-Server und SQL Server-Instanzen)**  > **Azure Migrate: Ermittlung und Bewertung** die Option **Ermitteln** aus.
2. Wählen Sie unter **Discover servers (Server ermitteln)**  > **Sind Ihre Server virtualisiert?** die Option **Ja, mit VMware vSphere-Hypervisor** aus.
3. Geben Sie unter **1: Projektschlüssel generieren** einen Namen für die Azure Migrate-Appliance ein, die Sie für die Ermittlung von Servern in der VMware-Umgebung einrichten. Für den Namen können bis zu 14 alphanumerische Zeichen angegeben werden.
1. Klicken Sie auf **Schlüssel generieren**, um mit der Erstellung der erforderlichen Azure-Ressourcen zu beginnen. Schließen Sie die Ermittlungsseite nicht, während die Ressourcen erstellt werden.
1. Nach der erfolgreichen Erstellung der Azure-Ressourcen wird ein **Projektschlüssel** generiert.
1. Kopieren Sie den Schlüssel, da Sie ihn benötigen, um die Registrierung der Appliance während der Konfiguration abzuschließen.

### <a name="2-download-the-ova-template"></a>2. Herunterladen der OVA-Vorlage

Wählen Sie in **2: Azure Migrate-Appliance herunterladen** die OVA-Datei aus, und klicken Sie auf **Herunterladen**.

### <a name="verify-security"></a>Überprüfen der Sicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die OVA-Datei sicher ist:

1. Öffnen Sie auf dem Server, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
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

### <a name="3-create-the-appliance-server"></a>3. Erstellen des Servers auf der Appliance

Importieren Sie die heruntergeladene Datei, und erstellen Sie in der VMware-Umgebung einen Server.

1. Klicken Sie in der vSphere-Clientkonsole auf **Datei** > **Deploy OVF Template** (OVF-Vorlage bereitstellen).
2. Geben Sie im Assistenten zum Bereitstellen der OVF-Vorlage unter **Quelle** den Speicherort der OVA-Datei an.
3. Geben Sie unter **Name** und **Standort** einen Anzeigenamen für den Server an. Wählen Sie das Bestandsobjekt aus, in dem der Server gehostet wird.
5. Geben Sie unter **Host/Cluster** den Host oder Cluster an, auf bzw. in dem der Server ausgeführt wird.
6. Geben Sie unter **Speicher** das Speicherziel des Servers an.
7. Geben Sie unter **Datenträgerformat** den Typ und die Größe des Datenträgers an.
8. Geben Sie unter **Netzwerkzuordnung** das Netzwerk an, mit dem der Server eine Verbindung herstellt. Das Netzwerk benötigt Internetkonnektivität, um Metadaten an Azure Migrate zu senden.
9. Überprüfen Sie die Einstellungen, und klicken Sie dann auf **Fertig stellen**.


### <a name="verify-appliance-access-to-azure"></a>Überprüfen des Appliancezugriffs auf Azure

Stellen Sie sicher, dass der Server auf der Appliance eine Verbindung mit Azure-URLs für [öffentliche](migrate-appliance.md#public-cloud-urls) und [Government](migrate-appliance.md#government-cloud-urls)-Clouds herstellen kann.


### <a name="4-configure-the-appliance"></a>4. Konfigurieren der Appliance

Führen Sie die Ersteinrichtung der Appliance durch.

> [!NOTE]
> Wenn Sie die Appliance mithilfe eines [**PowerShell-Skripts**](deploy-appliance-script.md) und nicht mithilfe der heruntergeladenen OVA-Datei einrichten, sind die ersten beiden Schritte in diesem Verfahren nicht relevant.

1. Klicken Sie in der vSphere-Clientkonsole mit der rechten Maustaste auf den Server, und wählen Sie **Konsole öffnen** aus.
2. Geben Sie die Sprache, die Zeitzone und das Kennwort für die Appliance an.
3. Öffnen Sie in einem Browser auf einem beliebigen Computer, der eine Verbindung mit der Appliance herstellen kann, die URL des Konfigurations-Managers der Appliance: `https://appliance name or IP address: 44368`.

   Alternativ können Sie den Konfigurations-Manager auch auf dem Desktop des Servers auf der Appliance öffnen, indem Sie die Verknüpfung für den Konfigurations-Manager auswählen.
1. Akzeptieren Sie die **Lizenzbedingungen**, und lesen Sie die Drittanbieterinformationen.
1. Navigieren Sie im Konfigurations-Manager zu **Erforderliche Komponenten einrichten**, und führen Sie die folgenden Schritte aus:
   - **Konnektivität**: Die Appliance überprüft, ob der Server über Internetzugriff verfügt. Wenn der Server einen Proxy verwendet:
     - Klicken Sie auf **Proxy einrichten**, um die Proxyadresse `http://ProxyIPAddress` oder `http://ProxyFQDN` und den lauschenden Port anzugeben.
     - Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert.
     - Es werden nur HTTP-Proxys unterstützt.
     - Wenn Sie Proxydetails hinzugefügt oder den Proxy und/oder die Authentifizierung deaktiviert haben, klicken Sie auf **Speichern**, um die Konnektivitätsprüfung erneut auszulösen.
   - **Uhrzeitsynchronisierung**: Die Uhrzeit der Appliance muss mit der Internetzeit synchronisiert werden, damit die Ermittlung ordnungsgemäß funktioniert.
   - **Updates installieren**: Die Appliance stellt sicher, dass die neuesten Updates installiert sind. Nachdem die Überprüfung abgeschlossen ist, können Sie auf **View appliance services** (Appliancedienste anzeigen) klicken, um den Status und die Versionen der auf dem Applianceserver ausgeführten Dienste anzuzeigen.
   - **Install VDDK** (VDDK installieren): Die Appliance überprüft, ob das VMware vSphere-VDDK (Virtual Disk Development Kit) installiert ist. Laden Sie bei Bedarf VDDK 6.7 von VMware herunter, und extrahieren Sie den Inhalt der heruntergeladenen ZIP-Datei am angegebenen Ort auf der Appliance, wie unter **Installationsanweisungen** beschrieben.

     Bei der Servermigration mit Azure Migrate wird das VDDK zur Replikation von Servern während der Migration zu Azure verwendet. 
1. Wenn Sie möchten, können Sie während der Konfiguration der Appliance jederzeit **Voraussetzungen erneut ausführen**, um zu prüfen, ob die Appliance alle Voraussetzungen erfüllt.

    :::image type="content" source="./media/tutorial-discover-vmware/appliance-prerequisites.png" alt-text="Panel 1 im Konfigurations-Manager der Appliance":::

### <a name="register-the-appliance-with-azure-migrate"></a>Registrieren der Appliance bei Azure Migrate

1. Fügen Sie den aus dem Portal kopierten **Projektschlüssel** ein. Wenn Sie den Schlüssel nicht haben, wechseln Sie zu **Azure Migrate: Ermittlung und Bewertung > Ermitteln > Vorhandene Appliances verwalten**, wählen Sie den Appliancenamen aus, den Sie bei der Generierung des Schlüssels angegeben haben, und kopieren Sie den entsprechenden Schlüssel.
1. Für die Authentifizierung bei Azure benötigen Sie einen Gerätecode. Wenn Sie auf **Anmelden** klicken, wird ein modales Dialogfeld mit dem Gerätecode angezeigt. Dies ist in der folgenden Abbildung dargestellt.

    :::image type="content" source="./media/tutorial-discover-vmware/device-code.png" alt-text="Modales Dialogfeld mit Gerätecode":::

1. Klicken Sie auf **Copy code & Login** (Code kopieren und anmelden), um den Gerätecode zu kopieren und eine Azure-Anmeldeaufforderung in einer neuen Browserregisterkarte zu öffnen. Sollte keine Anmeldung angezeigt werden, vergewissern Sie sich, dass Sie den Popupblocker im Browser deaktiviert haben.
1. Fügen Sie auf der neuen Registerkarte den Gerätecode ein, und melden Sie sich mit Ihrem Azure-Benutzernamen und dem zugehörigen Kennwort an.
   
   Die Anmeldung mit einer PIN wird nicht unterstützt.
3. Falls Sie die Registerkarte für die Anmeldung versehentlich schließen, ohne die Anmeldung durchzuführen, müssen Sie die Browserregisterkarte des Appliance-Konfigurations-Managers aktualisieren, um die Schaltfläche „Anmelden“ wieder zu aktivieren.
1. Wechseln Sie nach der erfolgreichen Anmeldung wieder zur vorherigen Registerkarte mit dem Appliance-Konfigurations-Manager.
1. Wenn das für die Protokollierung verwendete Azure-Benutzerkonto über die richtigen Berechtigungen für die während der Schlüsselgenerierung erstellten Azure-Ressourcen verfügt, wird die Registrierung der Appliance initiiert.
1. Nachdem die Appliance erfolgreich registriert wurde, können Sie die Registrierungsdetails anzeigen, indem Sie auf **Details anzeigen** klicken.

    :::image type="content" source="./media/tutorial-discover-vmware/appliance-registration.png" alt-text="Panel 2 im Appliance-Konfigurations-Manager":::

## <a name="start-continuous-discovery"></a>Starten der kontinuierlichen Ermittlung

### <a name="provide-vcenter-server-details"></a>Angeben von vCenter Server-Details

Die Appliance muss eine Verbindung mit der vCenter Server-Instanz herstellen, um die Konfigurations- und Leistungsdaten der Server zu ermitteln.

1. Klicken Sie in **Schritt 1: Geben Sie die vCenter Server-Anmeldeinformationen ein** auf **Anmeldeinformationen hinzufügen**, um einen Anzeigenamen für die Anmeldeinformationen anzugeben. Fügen Sie **Benutzername** und **Kennwort** für das vCenter Server-Konto hinzu, das die Appliance verwendet, um Server in der vCenter Server-Instanz zu ermitteln.
    - Sie sollten ein Konto mit den erforderlichen Berechtigungen festgelegt haben, wie weiter oben in diesem Artikel beschrieben.
    - Wenn Sie die Ermittlung auf bestimmte VMware-Objekte (vCenter Server-Rechenzentren, Cluster, einen Ordner mit Clustern, Hosts, einen Ordner mit Hosts oder einzelne Server) begrenzen möchten, hilft Ihnen die Anleitung in [diesem Artikel](set-discovery-scope.md) beim Einschränken des von Azure Migrate genutzten Kontos weiter.
1. Klicken Sie in **Schritt 2: Geben Sie vCenter Server-Details ein** auf **Ermittlungsquelle hinzufügen**, um den Anzeigenamen für die Anmeldeinformationen in der Dropdownliste auszuwählen, und geben Sie den Wert für **IP-Adresse/FQDN** der vCenter Server-Instanz an. Sie können für **Port** den Standardwert (443) beibehalten oder einen benutzerdefinierten Port angeben, den vCenter Server überwacht, und dann auf **Speichern** klicken.
1. Wenn Sie auf **Speichern** klicken, versucht die Appliance, die Verbindung mit der vCenter Server-Instanz mit den angegebenen Anmeldeinformationen zu überprüfen, und zeigt den **Überprüfungszustand** in der Tabelle für die vCenter Server-IP-Adresse/FQDN an.
1. Sie können die Verbindung zu vCenter Server jederzeit vor Beginn der Ermittlung **erneut überprüfen**.

    :::image type="content" source="./media/tutorial-discover-vmware/appliance-manage-sources.png" alt-text="Panel 3 im Konfigurations-Manager auf der Appliance mit vCenter Server-Details":::

### <a name="provide-server-credentials"></a>Angeben von Serveranmeldeinformation

In **Schritt 3: Geben Sie Serveranmeldeinformationen an, um eine Software-Inventur, Abhängigkeitsanalyse ohne Agent und Ermittlung von SQL Server-Instanzen und -Datenbanken durchzuführen** können Sie entweder die Anmeldeinformationen für mehrere Server angeben oder, wenn Sie diese Features nicht nutzen möchten, den Schritt überspringen und mit der vCenter Server-Ermittlung fortfahren. Sie können Ihre Absicht später jederzeit ändern.

:::image type="content" source="./media/tutorial-discover-vmware/appliance-server-credentials-mapping.png" alt-text="Panel 3 im Konfigurations-Manager auf der Appliance mit Serverdetails":::

Wenn Sie diese Features nutzen möchten, können Sie Serveranmeldeinformationen bereitstellen, indem Sie die folgenden Schritte ausführen. Die Appliance versucht, die Anmeldeinformationen automatisch den Servern zuzuordnen, um die Ermittlungsfeatures auszuführen.

- Sie können Anmeldeinformationen für den Server hinzufügen, indem Sie auf die Schaltfläche **Anmeldeinformationen hinzufügen** klicken. Dadurch wird ein modales Dialogfeld geöffnet, in dem Sie den **Anmeldeinformationstyp** im Dropdownmenü auswählen können.
- Sie können Anmeldeinformationen für die Authentifizierung für eine Domäne, Windows (ohne Domäne), Linux (ohne Domäne) oder SQL Server angeben. [Erfahren Sie mehr](add-server-credentials.md) zur Bereitstellung von Anmeldeinformationen und zu deren Handhabung.
- Für jeden Typ von Anmeldeinformationen müssen Sie einen Anzeigenamen für Anmeldeinformationen angeben, **Benutzername** und **Kennwort** hinzufügen und auf **Speichern** klicken.
- Wenn Sie Domänenanmeldeinformationen wählen, müssen Sie auch den FQDN für die Domäne angeben. Der FQDN wird benötigt, um die Echtheit der Anmeldeinformationen mithilfe der Active Directory-Instanz der jeweiligen Domäne zu bestätigen.
- Überprüfen Sie die [erforderlichen Berechtigungen](add-server-credentials.md#required-permissions) des Kontos für die Ermittlung installierter Anwendungen, Abhängigkeitsanalyse ohne Agent oder für Ermittlung von SQL Server-Instanzen und -Datenbanken.
- Wenn Sie mehrere Anmeldeinformationen gleichzeitig hinzufügen möchten, klicken Sie auf **Weitere hinzufügen**, um die Angeben zu speichern und weitere Anmeldeinformationen hinzuzufügen.
- Wenn Sie auf **Speichern** oder **Weitere hinzufügen** klicken, überprüft die Appliance die Domänenanmeldeinformationen mit dem Active Directory der Domäne auf Echtheit. Dies geschieht, um Kontosperrungen zu vermeiden, wenn die Appliance mehrere Iterationen durchführt, um Anmeldeinformationen den jeweiligen Servern zuzuordnen.
- Sie können den **Überprüfungsstatus** aller Domänenanmeldeinformationen in der Tabelle mit den Anmeldeinformationen einsehen. Nur Domänenanmeldeinformationen werden überprüft.
- Wenn die Überprüfung fehlschlägt, können Sie auf den Status **Fehlgeschlagen** klicken, um den aufgetretenen Fehler einzusehen, und nach Behebung des Problems auf **Anmeldeinformationen erneut überprüfen** klicken, um die fehlgeschlagenen Domänenanmeldeinformationen erneut zu überprüfen.

### <a name="start-discovery"></a>Ermittlung starten

1. Klicken Sie auf **Ermittlung starten**, um die vCenter Server-Ermittlung zu starten. Nachdem die Ermittlung erfolgreich gestartet wurde, können Sie den Ermittlungsstatus anhand von IP-Adresse/FQDN von vCenter Server in der Tabelle mit den Quellen überprüfen.
1. Wenn Sie Serveranmeldeinformationen angegeben haben, wird die Software-Inventur (Ermittlung installierter Anwendungen) automatisch eingeleitet, nachdem die vCenter Server-Ermittlung abgeschlossen ist. Die Software-Inventur erfolgt einmal alle 12 Stunden.
1. Bei der [Software-Inventur](how-to-discover-applications.md) werden die auf den Servern laufenden SQL Server-Instanzen ermittelt. Anhand der Informationen versucht die Appliance, sich mit den auf der Appliance bereitgestellten Anmeldeinformationen für die Windows- oder SQL Server-Authentifizierung mit den Instanzen zu verbinden und Daten zu SQL Server-Datenbanken und deren Eigenschaften zu sammeln. Die SQL Server-Ermittlung erfolgt einmal alle 24 Stunden.
1. Bei der Softwareinventur werden die hinzugefügten Serveranmeldeinformationen mit Servern abgeglichen und für die Abhängigkeitsanalyse ohne Agent überprüft. Sie können die Abhängigkeitsanalyse ohne Agent für Server im Portal aktivieren. Nur die Server mit erfolgreicher Überprüfung können ausgewählt werden, um die Abhängigkeitsanalyse ohne Agent zu aktivieren.

> [!Note]
>Azure Migrate verschlüsselt die Kommunikation zwischen der Azure Migrate-Appliance und den SQL Server-Quellinstanzen (durch auf TRUE festgelegte Eigenschaft „Verbindung verschlüsseln“). Diese Verbindungen sind mit (auf TRUE festgelegter Option) [**TrustServerCertificate**](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.trustservercertificate) verschlüsselt. Die Transportschicht verwendet SSL, um den Kanal zu verschlüsseln und die Zertifikatskette zum Überprüfen der Vertrauenswürdigkeit zu umgehen. Der Server der Appliance muss so eingerichtet sein, dass er die [**Stammzertifizierungsstelle des Zertifikats als vertrauenswürdig einstuft**](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine).<br/>
Wenn auf dem Server beim Start kein Zertifikat bereitgestellt wird, erstellt SQL Server ein selbstsigniertes Zertifikat, mit dem Anmeldungspakete verschlüsselt werden. [**Weitere Informationen**](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine)

Die Ermittlung funktioniert wie folgt:

- Es dauert etwa 15 Minuten, bis der Bestand ermittelter Server im Portal sichtbar ist.
- Die Ermittlung installierter Anwendungen kann einige Zeit in Anspruch nehmen. Die Dauer hängt von der Anzahl der ermittelten Server ab. Bei 500 Servern dauert es ungefähr ein Stunde, bis der ermittelte Bestand im Azure Migrate-Portal angezeigt wird.
- Nach der Ermittlung von Servern können Sie im Portal die Abhängigkeitsanalyse ohne Agent auf den Servern aktivieren.
- SQL Server-Instanzen und -Datenbankdaten werden im Portal innerhalb von 24 Stunden nach Einleiten der Ermittlung angezeigt.

## <a name="next-steps"></a>Nächste Schritte

- [Bewerten von Servern](./tutorial-assess-vmware-azure-vm.md) für die Migration zu Azure-VMs
- [Bewerten von SQL Server-Instanzen](./tutorial-assess-sql.md) für die Migration zu Azure SQL
- [Informieren Sie sich über die Daten](migrate-appliance.md#collected-data---vmware), die von der Appliance im Zuge der Ermittlung gesammelt werden.