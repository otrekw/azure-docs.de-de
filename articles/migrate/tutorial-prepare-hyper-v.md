---
title: Vorbereiten von Hyper-V-VMs für die Bewertung/Migration mit Azure Migrate
description: Es wird beschrieben, wie Sie die Bewertung/Migration von Hyper-V-VMs mit Azure Migrate vorbereiten.
ms.topic: tutorial
ms.date: 03/31/2020
ms.custom: mvc
ms.openlocfilehash: d14ae4282afb610d025d08419a69c6d10c2f1d08
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2020
ms.locfileid: "80436224"
---
# <a name="prepare-for-assessment-and-migration-of-hyper-v-vms-to-azure"></a>Vorbereiten auf die Bewertung und Migration von virtuellen Hyper-V-Computern zu Azure

In diesem Artikel wird beschrieben, wie Sie die Bewertung lokaler Hyper-V-VMs mit der Azure Migrate-Serverbewertung(migrate-services-overview.md#azure-migrate-server-assessment-tool) sowie die Migration von Hyper-V-VMs mit der [Azure Migrate-Servermigration](migrate-services-overview.md#azure-migrate-server-migration-tool) vorbereiten.


Dieses Tutorial ist das erste in einer Reihe, die Ihnen zeigt, wie Sie virtuelle Hyper-V-Computer bewerten und zu Azure migrieren. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereiten Sie Azure vor. Richten sie Berechtigungen für Ihr Azure-Konto und Ihre Ressourcen ein, um Azure Migrate verwenden zu können.
> * Vorbereiten lokaler Hyper-V-Hosts und VMs für die Serverbewertung. Sie können die Vorbereitung mithilfe eines Konfigurationsskripts oder manuell durchführen.
> * Bereiten Sie die Bereitstellung der Azure Migrate-Appliance vor. Die Appliance wird zum Ermitteln und Bewerten lokaler virtueller Computer verwendet.
> * Vorbereiten lokaler Hyper-V-Hosts und VMs für die Servermigration.


> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell einen Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie in den Vorgehensweisen für die Hyper-V-Bewertung und -Migration.


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prepare-azure"></a>Vorbereiten von Azure

### <a name="azure-permissions"></a>Azure-Berechtigungen

Sie müssen Berechtigungen für die Azure Migrate-Bereitstellung einrichten:

**Aufgabe** | **Details** 
--- | --- 
**Erstellen eines Azure Migrate-Projekts** | Ihr Azure-Konto benötigt zum Erstellen eines Projekts Berechtigungen vom Typ „Mitwirkender“ oder „Besitzer“. | 
**Registrieren von Ressourcenanbietern** | Azure Migrate verwendet eine schlanke Azure Migrate-Appliance, um virtuelle Hyper-V-Computer mit der Azure Migrate-Serverbewertung zu ermitteln und zu bewerten.<br/><br/> Bei der Applianceregistrierung werden Ressourcenanbieter bei dem Abonnement registriert, das in der Appliance ausgewählt wurde. [Weitere Informationen](migrate-appliance-architecture.md#appliance-registration)<br/><br/> Sie müssen über die Rolle „Mitwirkender“ oder „Besitzer“ für das Abonnement verfügen, um die Ressourcenanbieter zu registrieren.
**Erstellen der Azure AD-App** | Bei der Registrierung erstellt Azure Migrate eine Azure AD-App (Azure Active Directory) für die Kommunikation zwischen den auf der Appliance ausgeführten Agents und den entsprechenden Diensten in Azure. [Weitere Informationen](migrate-appliance-architecture.md#appliance-registration)<br/><br/> Sie benötigen Berechtigungen zum Erstellen von Azure AD-Apps (in der Rolle „Anwendungsentwickler“ verfügbar).



### <a name="assign-permissions-to-create-project"></a>Zuweisen von Berechtigungen für die Projekterstellung

Überprüfen Sie, ob Sie die Berechtigung zum Erstellen eines Azure Migrate-Projekts besitzen.

1. Öffnen Sie im Azure-Portal das Abonnement, und wählen Sie **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** nach dem relevanten Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.
3. Sie sollten über die Berechtigung **Mitwirkender** oder **Besitzer** verfügen.
    - Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements.
    - Wenn Sie nicht der Besitzer des Abonnements sind, müssen Sie mit dem Besitzer zusammenarbeiten, um die Rolle zuzuweisen.


### <a name="assign-permissions-to-register-the-appliance"></a>Zuweisen von Berechtigungen zum Registrieren der Appliance

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


## <a name="prepare-hyper-v-for-assessment"></a>Vorbereiten von Hyper-V auf die Bewertung

Sie können Hyper-V für die VM-Bewertung manuell oder mithilfe eines Konfigurationsskripts vorbereiten. Vorbereitungsschritte:
- [Überprüfen](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements) Sie die Einstellungen des Hyper-V-Hosts, und stellen Sie sicher, dass die [erforderlichen Ports](migrate-support-matrix-hyper-v.md#port-access) auf Hyper-V-Hosts geöffnet sind.
- Richten Sie auf jedem Host PowerShell-Remoting ein, sodass die Azure Migrate-Appliance über eine WinRM-Verbindung PowerShell-Befehle auf dem Host ausführen kann.
- Delegieren Sie Anmeldeinformationen, wenn sich VM-Datenträger auf SMB-Remotefreigaben befinden.
- Richten Sie ein Konto ein, das von der Appliance zum Ermitteln virtueller Computer auf Hyper-V-Hosts verwendet wird.
- Richten Sie auf jedem virtuellen Computer, den Sie ermitteln und bewerten möchten, Hyper-V-Integrationsdienste ein. Die Standardeinstellungen beim Aktivieren von Integration Services sind für Azure Migrate ausreichend.

    ![Aktivieren von Integration Services](./media/tutorial-prepare-hyper-v/integrated-services.png)


## <a name="prepare-with-a-script"></a>Vorbereiten mit einem Skript

Das Skript führt Folgendes aus:

- Es wird überprüft, ob das Skript unter einer unterstützten PowerShell-Version ausgeführt wird.
- Es wird überprüft, ob Sie (der Benutzer, der das Skript ausführt) über Administratorberechtigungen auf dem Hyper-V-Host verfügen.
- Das Skript ermöglicht die Erstellung eines lokalen Benutzerkontos (kein Administratorkonto), das vom Azure Migrate-Dienst für die Kommunikation mit dem Hyper-V-Host verwendet wird. Dieses Benutzerkonto wird den folgenden Gruppen auf dem Host hinzugefügt:
    - Remoteverwaltungsbenutzer
    - Hyper-V-Administratoren
    - Systemmonitorbenutzer
- Das Skript überprüft, ob auf dem Host eine unterstützte Hyper-V-Version und die Hyper-V-Rolle ausgeführt werden.
- Es aktiviert den WinRM-Dienst und öffnet die Ports 5985 (HTTP) und 5986 (HTTPS) auf dem Host (erforderlich für die Metadatensammlung).
- Das Skript aktiviert PowerShell-Remoting auf dem Host.
- Es überprüft, ob die Hyper-V-Integrationsdienste auf allen vom Host verwalteten virtuellen Computern aktiviert sind.
- Es aktiviert bei Bedarf CredSSP auf dem Host.

Führen Sie das Skript wie folgt aus:

1. Vergewissern Sie sich, dass mindestens PowerShell-Version 4.0 auf dem Hyper-V-Host installiert ist.
2. Laden Sie das Skript aus dem [Microsoft Download Center](https://aka.ms/migrate/script/hyperv) herunter. Das Skript wird von Microsoft kryptografisch signiert.
3. Überprüfen Sie die Skriptintegrität mithilfe von MD5- oder SHA256-Hashdateien. Die Hashtagwerte sind unten aufgeführt. Führen Sie den folgenden Befehl aus, um den Hash für das Skript zu generieren:
    ```
    C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]
    ```
    Beispielverwendung:
    ```
    C:\>CertUtil -HashFile C:\Users\Administrators\Desktop\ MicrosoftAzureMigrate-Hyper-V.ps1
    SHA256
    ```

4.    Führen Sie nach der Überprüfung der Skriptintegrität das Skript mit dem folgenden PowerShell-Befehl auf jedem Hyper-V-Host aus:
    ```
    PS C:\Users\Administrators\Desktop> MicrosoftAzureMigrate-Hyper-V.ps1
    ```

### <a name="hashtag-values"></a>Hashtagwerte

Hashwerte:

| **Hash** | **Wert** |
| --- | --- |
| **MD5** | 0ef418f31915d01f896ac42a80dc414e |
| **SHA256** | 0ad60e7299925eff4d1ae9f1c7db485dc9316ef45b0964148a3c07c80761ade2 |


## <a name="prepare-manually"></a>Manuelle Vorbereitung

Führen Sie das Verfahren in diesem Abschnitt aus, um Hyper-V manuell und nicht mithilfe des Skripts vorzubereiten.

### <a name="verify-powershell-version"></a>Überprüfen der PowerShell-Version

Vergewissern Sie sich, dass mindestens PowerShell-Version 4.0 auf dem Hyper-V-Host installiert ist.



### <a name="set-up-an-account-for-vm-discovery"></a>Einrichten eines Kontos für die VM-Ermittlung

Azure Migrate benötigt Berechtigungen zum Ermitteln lokaler VMs.

- Richten Sie ein Domänen- oder lokales Benutzerkonto mit Administratorberechtigungen auf dem Hyper-V-Host/-Cluster ein.

    - Sie benötigen ein einziges Konto für alle Hosts und Cluster, die Sie in die Ermittlung einschließen möchten.
    - Das Konto kann ein lokales oder ein Domänenkonto sein. Es wird empfohlen, dass es über Administratorberechtigungen für die Hyper-V-Hosts oder -Cluster verfügt.
    - Wenn Sie keine Administratorberechtigungen zuweisen möchten, sind alternativ die folgenden Berechtigungen erforderlich:
        - Remoteverwaltungsbenutzer
        - Hyper-V-Administratoren
        - Systemmonitorbenutzer

### <a name="verify-hyper-v-host-settings"></a>Überprüfen der Hyper-V-Hosteinstellungen

1. Überprüfen Sie die [Hyper-V-Hostanforderungen](migrate-support-matrix-hyper-v.md#hyper-v-host-requirements) für die Serverbewertung.
2. Stellen Sie sicher, dass die [erforderlichen Ports](migrate-support-matrix-hyper-v.md#port-access) auf den Hyper-V-Hosts geöffnet sind.

### <a name="enable-powershell-remoting-on-hosts"></a>Aktivieren von PowerShell-Remoting auf Hosts

Richten Sie auf jedem Host PowerShell-Remoting wie folgt ein:

1. Öffnen Sie auf jedem Host eine PowerShell-Konsole als Administrator.
2. Führen Sie den folgenden Befehl aus:

    ```
    Enable-PSRemoting -force
    ```
### <a name="enable-integration-services-on-vms"></a>Aktivieren von Integrationsdiensten auf VMs

Integrationsdienste sollten auf jedem virtuellen Computer aktiviert werden, damit Azure Migrate Betriebssysteminformationen auf dem virtuellen Computer erfassen kann.

Aktivieren Sie auf virtuellen Computern, die Sie ermitteln und bewerten möchten, [Hyper-V-Integrationsdienste](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) auf jedem virtuellen Computer.


### <a name="enable-credssp-on-hosts"></a>Aktivieren von „CredSSP“ auf Hosts

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
2. [Überprüfen](migrate-appliance.md#url-access) Sie die Azure-URLs, auf die die Appliance zugreifen muss.
3. Überprüfen Sie die Daten, die die Appliance während der Ermittlung und Bewertung sammelt.
4. [Beachten](migrate-appliance.md#collected-data---hyper-v) Sie die Portzugriffsanforderungen für die Appliance.


## <a name="prepare-for-hyper-v-migration"></a>Vorbereiten der Hyper-V-Migration

1. [Beachten](migrate-support-matrix-hyper-v-migration.md#hyper-v-hosts) Sie die Anforderungen der Hyper-V-Hosts für die Migration sowie die Azure-URLs, auf die Hyper-V-Hosts und -Cluster bei der VM-Migration zugreifen müssen.
2. [Überprüfen](migrate-support-matrix-hyper-v-migration.md#hyper-v-vms) Sie die Anforderungen für Hyper-V-VMs, die Sie zu Azure migrieren möchten.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Einrichten von Azure-Kontoberechtigungen.
> * Vorbereiten von virtuellen Hyper-V-Hosts für die Bewertung und Migration.
> * Vorbereiten der Bereitstellung der Azure Migrate-Appliance

Fahren Sie mit dem nächsten Tutorial fort, um ein Azure Migrate-Projekt zu erstellen, die Appliance bereitzustellen und Hyper-V-VMs für die Migration zu Azure zu ermitteln und zu bewerten.

> [!div class="nextstepaction"]
> [Bewerten von Hyper-V-VMs](./tutorial-assess-hyper-v.md)
