---
title: Einrichten einer Azure Migrate-Appliance für VMware
description: In diesem Artikel erfahren Sie, wie Sie eine Azure Migrate-Appliance für die Bewertung und Migration von VMware-VMs einrichten.
ms.topic: article
ms.date: 04/16/2020
ms.openlocfilehash: 24ba978d776da375b417fb67823651727836cb22
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87386740"
---
# <a name="set-up-an-appliance-for-vmware-vms"></a>Einrichten einer Appliance für VMware-VMs

In diesem Artikel erfahren Sie, wie Sie eine Azure Migrate-Appliance für die Bewertung mithilfe des Tools [Azure Migrate-Serverbewertung](migrate-services-overview.md#azure-migrate-server-assessment-tool) und für die Migration ohne Agent mithilfe des Tools [Azure Migrate-Servermigration](migrate-services-overview.md#azure-migrate-server-migration-tool) einrichten.

Die [Azure Migrate-Appliance](migrate-appliance.md) ist eine einfache Appliance, die von Azure Migrate-Serverbewertung und Azure Migrate-Servermigration verwendet wird, um lokale VMware-VMS zu ermitteln, VM-Metadaten/Leistungsdaten an Azure zu senden und für die Replikation von VMware-VMs während der Migration ohne Agent.

Sie können die Appliance mithilfe verschiedener Methoden bereitstellen:

- Einrichten auf einer VMware-VM mit einer heruntergeladenen OVA-Vorlage. Dies ist die in diesem Artikel beschriebene Methode.
- Einrichten auf einer VMware-VM oder einem physischen Computer mit einem PowerShell-Installationsskript. [Diese Methode](deploy-appliance-script.md) sollte verwendet werden, wenn Sie keine VM mithilfe einer OVA-Vorlage einrichten können oder wenn Sie in Azure Government arbeiten.

Überprüfen Sie nach der Erstellung der Appliance, ob diese eine Verbindung mit der Azure Migrate-Serverbewertung herstellen kann. Führen Sie dann die erstmalige Konfiguration durch, und registrieren Sie sie für das Azure Migrate-Projekt.


## <a name="appliance-deployment-ova"></a>Bereitstellung der Appliance (OVA)

Gehen Sie zum Einrichten der Appliance mithilfe einer OVA-Vorlage wie folgt vor:
- Herunterladen einer OVA-Vorlagendatei und Importieren der Datei in vCenter Server
- Erstellen der Appliance und Überprüfen der Verbindungsherstellung mit der Azure Migrate-Serverbewertung
- Durchführen der Erstkonfiguration für die Appliance und Registrieren der Appliance beim Azure Migrate-Projekt

## <a name="download-the-ova-template"></a>Herunterladen der OVA-Vorlage

1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Ermitteln**.
2. Wählen Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** die Option **Ja, mit VMware vSphere Hypervisor** aus.
3. Klicken Sie auf **Herunterladen**, um die OVA-Vorlagendatei herunterzuladen.

  ![Schritte zum Herunterladen einer OVA-Datei](./media/tutorial-assess-vmware/download-ova.png)

### <a name="verify-security"></a>Überprüfen der Sicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die OVA-Datei sicher ist.

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Für die aktuellste Applianceversion muss der generierte Hash den unten angegebenen [Einstellungen](./tutorial-assess-vmware.md#verify-security) entsprechen.



## <a name="create-the-appliance-vm"></a>Erstellen der Appliance-VM

Importieren Sie die heruntergeladene Datei, und erstellen Sie eine VM.

1. Klicken Sie in der vSphere-Clientkonsole auf **Datei** > **Deploy OVF Template** (OVF-Vorlage bereitstellen).
![Menübefehl zum Bereitstellen einer OVF-Vorlage](./media/tutorial-assess-vmware/deploy-ovf.png)

2. Geben Sie im Assistenten zum Bereitstellen der OVF-Vorlage unter **Quelle** den Speicherort der OVA-Datei an.
3. Geben Sie unter **Name** und **Standort** einen Anzeigenamen für die VM an. Wählen Sie das Inventarobjekt aus, in dem die VM gehostet wird.
5. Geben Sie unter **Host/Cluster** den Host oder Cluster an, auf bzw. in dem die VM ausgeführt wird.
6. Geben Sie unter **Speicher** das Speicherziel für die VM an.
7. Geben Sie unter **Datenträgerformat** den Typ und die Größe des Datenträgers an.
8. Geben Sie unter **Netzwerkzuordnung** das Netzwerk an, mit dem die VM eine Verbindung herstellt. Das Netzwerk muss über eine Internetverbindung verfügen, um Metadaten an die Azure Migrate-Serverbewertung senden zu können.
9. Überprüfen Sie die Einstellungen, und klicken Sie dann auf **Fertig stellen**.


## <a name="verify-appliance-access-to-azure"></a>Überprüfen des Appliancezugriffs auf Azure

Stellen Sie sicher, dass die Appliance-VM eine Verbindung mit Azure-URLs für [öffentliche](migrate-appliance.md#public-cloud-urls) und [Government](migrate-appliance.md#government-cloud-urls)-Clouds herstellen kann.


## <a name="configure-the-appliance"></a>Konfigurieren der Appliance

Führen Sie die Ersteinrichtung der Appliance durch. Wenn Sie die Appliance mithilfe eines Skripts anstelle einer OVA-Vorlage bereitstellen, gelten die ersten beiden Schritte im Verfahren nicht.

1. Klicken Sie in der vSphere-Clientkonsole mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Konsole öffnen** aus.
2. Geben Sie die Sprache, die Zeitzone und das Kennwort für die Appliance an.
3. Öffnen Sie in einem Browser auf einem beliebigen Computer, der eine Verbindung mit der VM herstellen kann, und öffnen Sie die URL der Appliance-Web-App: **https://*Appliancename oder IP-Adresse*: 44368**.

   Alternativ können Sie auch auf dem Appliancedesktop auf die App-Verknüpfung klicken, um die App zu öffnen.
4. Gehen Sie in der Web-App unter **Erforderliche Komponenten einrichten** wie folgt vor:
    - **Lizenz**: Akzeptieren Sie die Lizenzbedingungen, und lesen Sie die Drittanbieterinformationen.
    - **Konnektivität**: Die App überprüft, ob die VM über Internetzugriff verfügt. Falls die VM einen Proxy verwendet, gehen Sie wie folgt vor:
        - Klicken Sie auf **Proxyeinstellungen**, und geben Sie die Proxyadresse und den Lauschport an (im Format http://ProxyIPAddress oder http://ProxyFQDN ).
        - Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert.
        - Es werden nur HTTP-Proxys unterstützt.
    - **Uhrzeitsynchronisierung**: Die Uhrzeit wird überprüft. Die Uhrzeit der Appliance muss mit der Internetzeit synchronisiert werden, damit die Ermittlung ordnungsgemäß funktioniert.
    - **Updates installieren**: Azure Migrate überprüft, ob die neusten Applianceupdates installiert sind.
    - **Install VDDK** (VDDK installieren): Azure Migrate überprüft, ob das VMware vSphere-VDDK (Virtual Disk Development Kit) installiert ist.
        - Azure Migrate verwendet das VDDK, um Computer bei der Migration zu Azure zu replizieren.
        - Laden Sie VDDK 6.7 von VMware herunter, und extrahieren Sie den Inhalt der heruntergeladenen ZIP-Datei am angegebenen Ort auf der Appliance.

## <a name="register-the-appliance-with-azure-migrate"></a>Registrieren der Appliance bei Azure Migrate

1. Klicken Sie auf **Anmelden**. Sollte keine Anmeldung angezeigt werden, vergewissern Sie sich, dass Sie den Popupblocker im Browser deaktiviert haben.
2. Melden Sie sich auf der neuen Registerkarte mit Ihren Azure-Anmeldeinformationen an.
    - Melden Sie sich mit Ihrem Benutzernamen und Ihrem Kennwort an.
    - Die Anmeldung mit einer PIN wird nicht unterstützt.
3. Kehren Sie nach erfolgreicher Anmeldung zur Web-App zurück.
2. Wählen Sie das Abonnement aus, in dem das Azure Migrate-Projekt erstellt wurde. Wählen Sie anschließend das Projekt aus.
3. Geben Sie einen Namen für die Appliance an. Für den Namen können bis zu 14 alphanumerische Zeichen angegeben werden.
4. Klicken Sie auf **Registrieren**.


## <a name="start-continuous-discovery-by-providing-vcenter-server-and-vm-credential"></a>Starten der kontinuierlichen Ermittlung durch Bereitstellen von vCenter Server- und VM-Anmeldeinformationen

Die Appliance muss eine Verbindung mit der vCenter Server-Instanz herstellen, um die Konfigurations- und Leistungsdaten der VMs zu ermitteln.

### <a name="specify-vcenter-server-details"></a>vCenter Server-Details angeben
1. Geben Sie unter **vCenter Server-Details angeben** den Namen (FQDN) oder die IP-Adresse der vCenter Server-Instanz an. Sie können den Standardport beibehalten oder einen benutzerdefinierten Port angeben, an dem Ihre vCenter Server-Instanz lauscht.
2. Geben Sie unter **Benutzername** und **Kennwort** die Anmeldeinformationen für das schreibgeschützte Konto an, über das die Appliance VMs in der vCenter Server-Instanz ermittelt. Sie können den Ermittlungsbereich festlegen, indem Sie den Zugriff auf das vCenter-Konto einschränken. [Weitere Informationen](set-discovery-scope.md)
3. Vergewissern Sie sich durch Klicken auf **Verbindung überprüfen**, dass die Appliance eine Verbindung mit vCenter Server herstellen kann.

### <a name="specify-vm-credentials"></a>Angeben der VM-Anmeldeinformationen
Zum Ermitteln von Anwendungen, Rollen und Features sowie zum Visualisieren von Abhängigkeiten der virtuellen Computer können Sie VM-Anmeldeinformationen bereitstellen, die Zugriff auf die virtuellen VMware-Computer ermöglichen. Sie können Anmeldeinformationen für virtuelle Windows-Computer und Anmeldeinformationen für virtuelle Linux-Computer hinzufügen. [Weitere Informationen](./migrate-support-matrix-vmware.md) zu erforderlichen Zugriffsberechtigungen.

> [!NOTE]
> Diese Eingabe ist optional und wird benötigt, um die Anwendungsermittlung und die Visualisierung von Abhängigkeiten ohne Agent zu aktivieren.

1. Klicken Sie unter **Anwendungen und Abhängigkeiten auf VMs ermitteln** auf **Anmeldeinformationen hinzufügen**.
2. Wählen Sie das **Betriebssystem** aus.
3. Geben Sie einen Anzeigenamen für die Anmeldeinformationen an.
4. Geben Sie unter **Benutzername** und **Kennwort** ein Konto an, das mindestens über den Gastzugriff auf die virtuellen Computer verfügt.
5. Klicken Sie auf **Hinzufügen**.

Nachdem Sie die vCenter Server- und VM-Anmeldeinformationen angegeben haben (optional), klicken Sie auf **Speichern und Ermittlung starten**, um die Ermittlung der lokalen Umgebung zu starten.

Es dauert etwa 15 Minuten, bis Metadaten von ermittelten VMs im Portal angezeigt werden. Die Ermittlung von installierten Anwendungen, Rollen und Features dauert einige Zeit. Die Dauer hängt von der Anzahl der ermittelten VMS ab. Bei 500 VMs dauert es ungefähr 1 Stunde, bis der Anwendungsbestand im Azure Migrate-Portal angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die Tutorials zur [VMware-Bewertung](tutorial-assess-vmware.md) und zur [Migration ohne Agent](tutorial-migrate-vmware.md).
