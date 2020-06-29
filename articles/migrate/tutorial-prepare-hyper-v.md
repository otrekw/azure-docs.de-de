---
title: Vorbereiten von Hyper-V-VMs für die Bewertung/Migration mit Azure Migrate
description: Es wird beschrieben, wie Sie die Bewertung/Migration von Hyper-V-VMs mit Azure Migrate vorbereiten.
ms.topic: tutorial
ms.date: 04/15/2020
ms.custom: mvc
ms.openlocfilehash: ca9020a9c306eea39d75c15c96b5f9fe9bcc11fe
ms.sourcegitcommit: 99d016949595c818fdee920754618d22ffa1cd49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/15/2020
ms.locfileid: "84770542"
---
# <a name="prepare-for-assessment-and-migration-of-hyper-v-vms-to-azure"></a>Vorbereiten auf die Bewertung und Migration von virtuellen Hyper-V-Computern zu Azure

Dieser Artikel hilft Ihnen beim Vorbereiten der Bewertung lokaler Hyper-V-VMs (mit der [Azure Migrate-Serverbewertung](migrate-services-overview.md#azure-migrate-server-assessment-tool)) und deren Migration zu Azure mit der [Azure Migrate-Servermigration](migrate-services-overview.md#azure-migrate-server-migration-tool).


Dieses Tutorial ist das erste in einer Reihe, die Ihnen zeigt, wie Sie virtuelle Hyper-V-Computer bewerten und zu Azure migrieren. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorbereiten von Azure auf die Arbeit mit Azure Migrate
> * Bereiten Sie die Bewertung von Hyper-V-VMs vor.
> * Vorbereiten der Migration von Hyper-V-VMS 

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell einen Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prepare-azure"></a>Vorbereiten von Azure

In der Tabelle werden die Aufgaben zusammengefasst, die Sie in Azure ausführen müssen. Anleitungen dazu sind im Anschluss an die Tabelle zu finden.

**Aufgabe** | **Details** | **Berechtigungen**
--- | --- | ---
**Erstellen eines Azure Migrate-Projekts** | Ein Azure Migrate-Projekt stellt einen zentralen Ort zum Orchestrieren und Verwalten von Bewertungen und Migrationen mit Azure Migrate Tools, Microsoft-Tools und Angeboten von Drittanbietern bereit. | Ihr Azure-Konto benötigt die Berechtigung „Mitwirkender“ oder „Besitzer“ in der Ressourcengruppe, in der sich das Projekt befindet.
**Appliance registrieren** | Azure Migrate verwendet eine einfache Azure Migrate-Appliance zum Ermitteln und Bewerten von Hyper-V VMs. [Weitere Informationen](migrate-appliance-architecture.md#appliance-registration) | Ihr Azure-Konto benötigt zum Registrieren der Appliance die Berechtigung „Mitwirkender“ oder „Besitzer“ für das Azure-Abonnement.
**Erstellen der Azure AD-App** | Bei der Registrierung der Appliance erstellt Azure Migrate eine Azure AD-App (Azure Active Directory) für die Kommunikation zwischen den auf der Appliance ausgeführten Agents und Azure Migrate. | Für Ihr Azure-Konto sind Berechtigungen zum Erstellen von Azure AD-Apps erforderlich.
**Erstellen eines virtuellen Computers** | Sie benötigen Berechtigungen zum Erstellen einer VM in der Ressourcengruppe und im virtuellen Netzwerk sowie zum Schreiben auf einen verwalteten Azure-Datenträger. | Ihr Azure-Konto benötigt die Rolle „Mitwirkender für virtuelle Computer“.


### <a name="assign-permissions-to-create-project"></a>Zuweisen von Berechtigungen für die Projekterstellung

Überprüfen Sie, ob Sie die Berechtigung zum Erstellen eines Azure Migrate-Projekts besitzen.

1. Öffnen Sie im Azure-Portal das Abonnement, und wählen Sie **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** nach dem relevanten Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.
3. Sie sollten über die Berechtigung **Mitwirkender** oder **Besitzer** verfügen.
    - Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements.
    - Wenn Sie nicht der Besitzer des Abonnements sind, müssen Sie mit dem Besitzer zusammenarbeiten, um die Rolle zuzuweisen.


### <a name="assign-permissions-to-create-azure-ad-apps"></a>Zuweisen von Berechtigungen zum Erstellen von Azure AD-Apps

Sie können wie folgt Berechtigungen für Azure Migrate zuweisen, um während der Applianceregistrierung die Azure AD-App zu erstellen:

- Ein Mandantenadministrator/globaler Administrator kann Benutzern unter dem Mandanten Berechtigungen zum Erstellen und Registrieren von Azure AD-Apps erteilen.
- Ein Mandantenadministrator/globaler Administrator kann dem Konto die Rolle „Anwendungsentwickler“ (die über die Berechtigungen verfügt) zuweisen.

> [!NOTE]
> - Die App verfügt nur über die oben beschriebenen Zugriffsberechtigungen für das Abonnement.
> - Sie benötigen diese Berechtigungen nur, wenn Sie eine neue Appliance registrieren. Nach Einrichtung der Appliance können die Berechtigungen wieder entfernt werden.


#### <a name="grant-account-permissions"></a>Erteilen von Kontoberechtigungen

Der Mandanten-/globale Administrator kann wie folgt Berechtigungen erteilen:

1. In Azure AD muss der globale oder der Mandantenadministrator zu **Azure Active Directory** > **Benutzer** > **Benutzereinstellungen** navigieren.
2. Der Administrator muss **App-Registrierungen** auf **Ja** festlegen.

    ![Azure AD-Berechtigungen](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Dies ist eine Standardeinstellung, die nicht vertraulich ist. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance)



#### <a name="assign-application-developer-role"></a>Zuweisen der Rolle „Anwendungsentwickler“

Der Mandantenadministrator/globale Administrator kann einem Konto die Rolle „Anwendungsentwickler“ zuweisen. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)

### <a name="assign-azure-account-permissions"></a>Zuweisen der Azure-Kontoberechtigungen

Weisen Sie dem Konto die Rolle „Mitwirkender für virtuelle Computer“ zu, damit Sie über die folgenden Berechtigungen verfügen:

- Erstellen einer VM in der ausgewählten Ressourcengruppe
- Erstellen einer VM im ausgewählten virtuellen Netzwerk
- Schreiben auf einen verwalteten Azure-Datenträger 


### <a name="set-up-an-azure-network"></a>Richten Sie ein Azure-Netzwerk ein

[Richten Sie ein Azure-Netzwerk ein](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Lokale Computer werden auf verwalteten Azure-Datenträgern repliziert. Wenn Sie für die Migration ein Failover auf Azure durchführen, werden aus diesen verwalteten Datenträgern Azure-VMs erstellt und mit dem von Ihnen eingerichteten Azure-Netzwerk verknüpft.


## <a name="prepare-for-assessment"></a>Vorbereiten für die Bewertung

Sie können Hyper-V für die VM-Bewertung manuell oder mithilfe eines Konfigurationsskripts vorbereiten. Dies sind die erforderlichen Vorbereitungsschritte. Wenn Sie für die Vorbereitung ein Skript verwenden, werden sie automatisch konfiguriert.

**Schritt** | **Skript** | **Manuell**
--- | --- | ---
**Überprüfen der Anforderungen für Hyper-V-Hosts** | Das Skript überprüft, ob auf dem Host eine unterstützte Hyper-V-Version und die Hyper-V-Rolle ausgeführt werden.<br/><br/> Es aktiviert den WinRM-Dienst und öffnet die Ports 5985 (HTTP) und 5986 (HTTPS) auf dem Host (erforderlich für die Metadatensammlung). | Überprüfen Sie die [Hyper-V-Hostanforderungen](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements) für die Serverbewertung.<br/><br/> Stellen Sie sicher, dass die [erforderlichen Ports](migrate-support-matrix-hyper-v.md#port-access) auf den Hyper-V-Hosts geöffnet sind.
**Überprüfen der PowerShell-Version** | Es wird überprüft, ob das Skript unter einer unterstützten PowerShell-Version ausgeführt wird. | Überprüfen Sie, ob Sie PowerShell, Version 4.0 oder höher, auf dem Hyper-V-Host ausführen.
**Erstellen eines Kontos** | Es wird überprüft, ob Sie (der Benutzer, der das Skript ausführt) über Administratorberechtigungen auf dem Hyper-V-Host verfügen.<br/><br/>  Das Skript ermöglicht die Erstellung eines lokalen Benutzerkontos (kein Administratorkonto), das vom Azure Migrate-Dienst für die Kommunikation mit dem Hyper-V-Host verwendet wird. Dieses Benutzerkonto wird den folgenden Gruppen auf dem Host hinzugefügt:<br/><br/> – Remoteverwaltungsbenutzer<br/><br/> – Hyper-V-Administratoren<br/><br/>– Systemmonitorbenutzer | Richten Sie ein Domänen- oder lokales Benutzerkonto mit Administratorberechtigungen auf dem Hyper-V-Host/-Cluster ein.<br/><br/> – Sie benötigen ein einziges Konto für alle Hosts und Cluster, die Sie in die Ermittlung einschließen möchten.<br/><br/> – Das Konto kann ein lokales oder ein Domänenkonto sein. Es wird empfohlen, dass es über Administratorberechtigungen für die Hyper-V-Hosts oder -Cluster verfügt.<br/><br/> Wenn Sie keine Administratorberechtigungen zuweisen möchten, sind alternativ die folgenden Berechtigungen erforderlich: „Remoteverwaltungsbenutzer“; „Hyper-V-Administratoren“; „Systemmonitorbenutzer“.
**Aktivieren von PowerShell-Remoting** | Aktivieren Sie auf dem Host PowerShell-Remoting, sodass die Azure Migrate-Appliance über eine WinRM-Verbindung darauf PowerShell-Befehle ausführen kann.| Zum Einrichten auf jedem Host öffnen Sie eine PowerShell-Konsole als Administrator, und führen Sie den folgenden Befehl aus:<br/><br/>``` Enable-PSRemoting -force ```
**Einrichten von Hyper-V-Integrationsdiensten** | Es überprüft, ob die Hyper-V-Integrationsdienste auf allen vom Host verwalteten virtuellen Computern aktiviert sind. |  [Aktivieren Sie die Hyper-V Integrationsdienste](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) (Hyper-V Integration Services) auf den einzelnen virtuellen Computern.<br/><br/> Wenn Sie Windows Server 2003 ausführen, [folgen Sie diesen Anleitungen](prepare-windows-server-2003-migration.md).
**Delegieren von Anmeldeinformationen, wenn sich VM-Datenträger auf SMB-Remotefreigaben befinden** | Das Skript delegiert Anmeldeinformationen. | [Aktivieren Sie „CredSSP“](#enable-credssp-to-delegate-credentials) zum Delegieren von Anmeldeinformationen.

### <a name="run-the-script"></a>Führen Sie das Skript aus.

Führen Sie das Skript wie folgt aus:

1. Vergewissern Sie sich, dass mindestens PowerShell-Version 4.0 auf dem Hyper-V-Host installiert ist.
2. Laden Sie das Skript aus dem [Microsoft Download Center](https://aka.ms/migrate/script/hyperv) herunter. Das Skript wird von Microsoft kryptografisch signiert.
3. Überprüfen Sie die Skriptintegrität mithilfe von MD5- oder SHA256-Hashdateien. Die Hashtagwerte sind unten aufgeführt. Führen Sie den folgenden Befehl aus, um den Hash für das Skript zu generieren:
    ```
    C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]
    ```
    Beispielverwendung:
    ```
    C:\>CertUtil -HashFile C:\Users\Administrators\Desktop\ MicrosoftAzureMigrate-Hyper-V.ps1 SHA256
    ```

4. Führen Sie nach der Überprüfung der Skriptintegrität das Skript mit dem folgenden PowerShell-Befehl auf jedem Hyper-V-Host aus:
    ```
    PS C:\Users\Administrators\Desktop> MicrosoftAzureMigrate-Hyper-V.ps1
    ```

#### <a name="hashtag-values"></a>Hashtagwerte

Hashwerte:

| **Hash** | **Wert** |
| --- | --- |
| **MD5** | 0ef418f31915d01f896ac42a80dc414e |
| **SHA256** | 0ad60e7299925eff4d1ae9f1c7db485dc9316ef45b0964148a3c07c80761ade2 |



### <a name="enable-credssp-to-delegate-credentials"></a>Aktivieren von „CredSSP“ zum Delegieren von Anmeldeinformationen

Wenn sich auf dem Host VM-Datenträger auf SMB-Freigaben befinden, führen Sie diesen Schritt auf dem Host aus.

- Sie können diesen Befehl remote auf allen Hyper-V-Hosts ausführen.
- Wenn Sie in einem Cluster neue Hostknoten hinzufügen, werden diese automatisch zur Ermittlung hinzugefügt, aber Sie müssen CredSSP jedoch bei Bedarf manuell auf den neuen Knoten aktivieren.

Gehen Sie zur Aktivierung wie folgt vor:

1. Identifizieren Sie Hyper-V-Hosts, auf denen Hyper-V-VMs mit Datenträgern auf SMB-Freigaben ausgeführt werden.
2. Führen Sie den folgenden Befehl auf jedem identifizierten Hyper-V-Host aus:

    ```
    Enable-WSManCredSSP -Role Server -Force
    ```

Beim Einrichten der Appliance schließen Sie die Einrichtung von CredSSP durch [die Aktivierung für die Appliance](tutorial-assess-hyper-v.md#delegate-credentials-for-smb-vhds) ab. Dieser Vorgang wird im nächsten Tutorial dieser Reihe beschrieben.


## <a name="prepare-for-appliance-deployment"></a>Vorbereiten der Bereitstellung der Appliance

Bevor Sie im nächsten Tutorial die Azure Migrate-Appliance einrichten und mit der Bewertung beginnen, müssen Sie die Appliance-Bereitstellung vorbereiten.

1. [Überprüfen](migrate-appliance.md#appliance---hyper-v) Sie Appliance-Anforderungen.
2. Überprüfen Sie die Azure-URLs, die die Appliance für den Zugriff in [öffentlichen Clouds](migrate-appliance.md#public-cloud-urls) und [Azure Government-Clouds](migrate-appliance.md#government-cloud-urls) benötigt. Sollten Sie eine URL-basierte Firewall oder einen Proxy verwenden, lassen Sie unbedingt den Zugriff auf die erforderlichen URLs zu.
3. Überprüfen Sie die Daten, die die Appliance während der Ermittlung und Bewertung sammelt.
4. [Überprüfen](migrate-appliance.md#collected-data---hyper-v) Sie die Portzugriffsanforderungen für die Appliance.


## <a name="prepare-for-hyper-v-migration"></a>Vorbereiten der Hyper-V-Migration

1. [Beachten](migrate-support-matrix-hyper-v-migration.md#hyper-v-host-requirements) Sie die Anforderungen der Hyper-V-Hosts für die Migration sowie die Azure-URLs, auf die Hyper-V-Hosts und -Cluster bei der VM-Migration zugreifen müssen.
2. [Überprüfen](migrate-support-matrix-hyper-v-migration.md#hyper-v-vms) Sie die Anforderungen für Hyper-V-VMs, die Sie zu Azure migrieren möchten.
3. Auf VMs sind einige Änderungen erforderlich, bevor Sie sie zu Azure migrieren.
    - Es ist wichtig, diese Änderungen vorzunehmen, bevor Sie mit der Migration beginnen. Wenn Sie den virtuellen Computer migrieren, bevor Sie die Änderung vorgenommen haben, wird der virtuelle Computer in Azure unter Umständen nicht gestartet.
    - Überprüfen Sie die [Windows](prepare-for-migration.md#windows-machines)- und [Linux](prepare-for-migration.md#linux-machines)-Änderungen, die Sie vornehmen müssen.



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Einrichten von Azure-Kontoberechtigungen.
> * Vorbereiten von virtuellen Hyper-V-Hosts für die Bewertung und Migration.
> * Vorbereiten der Bereitstellung der Azure Migrate-Appliance

Fahren Sie mit dem nächsten Tutorial fort, um ein Azure Migrate-Projekt zu erstellen, die Appliance bereitzustellen und Hyper-V-VMs für die Migration zu Azure zu ermitteln und zu bewerten.

> [!div class="nextstepaction"]
> [Bewerten von Hyper-V-VMs](./tutorial-assess-hyper-v.md)
