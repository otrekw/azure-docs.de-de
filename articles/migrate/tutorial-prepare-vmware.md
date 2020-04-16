---
title: Vorbereiten von VMware-VMs für die Bewertung/Migration mit Azure Migrate
description: Hier erfahren Sie, wie Sie virtuelle VMware-Computer auf die Bewertung/Migration mit Azure Migrate vorbereiten.
ms.topic: tutorial
ms.date: 11/19/2019
ms.custom: mvc
ms.openlocfilehash: 2e8aa72300c840832168138015e0a01ab054f954
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619430"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>Vorbereiten von VMware-VMs für die Bewertung und die Migration zu Azure

In diesem Artikel wird erläutert, wie Sie VMware-VMs mit [Azure Migrate](migrate-services-overview.md) für die Bewertung und die Migration zu Azure vorbereiten.



Dieses Tutorial ist das erste in einer Reihe zur Bewertung und Migration von VMware-VMs. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorbereiten von Azure auf die Arbeit mit Azure Migrate
> * Vorbereiten von VMware auf die VM-Bewertung mit dem Azure Migrate-Bewertungstool
> * Vorbereiten von VMware auf die VM-Migration mit dem Tool für die Azure Migrate-Servermigration 

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert. Sie eignen sich als nützliche Einführung in die Einrichtung einer Bereitstellung und als schnelles Proof of Concept. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prepare-azure"></a>Vorbereiten von Azure

Sie benötigen diese Berechtigungen für diese Aufgaben in Azure, bevor Sie VMware-VMs bewerten oder migrieren können.

**Aufgabe** | **Details** 
--- | --- 
**Erstellen eines Azure Migrate-Projekts** | Ihr Azure-Konto benötigt zum Erstellen eines Projekts Berechtigungen vom Typ „Mitwirkender“ oder „Besitzer“. 
**Registrieren von Ressourcenanbietern** | Azure Migrate verwendet eine schlanke Azure Migrate-Appliance, um virtuelle VMware-Computer zu ermitteln und zu bewerten und mit der Azure Migrate-Serverbewertung zu Azure zu migrieren.<br/><br/> Bei der Applianceregistrierung werden Ressourcenanbieter bei dem Abonnement registriert, das in der Appliance ausgewählt wurde. [Weitere Informationen](migrate-appliance-architecture.md#appliance-registration)<br/><br/> Sie müssen über die Rolle „Mitwirkender“ oder „Besitzer“ für das Abonnement verfügen, um die Ressourcenanbieter zu registrieren.
**Erstellen von Azure AD-Apps** | Bei der Registrierung der Appliance erstellt Azure Migrate Azure Active Directory-Apps (Azure AD-Apps). <br/><br/> – Die erste App wird für die Kommunikation zwischen den auf der Appliance ausgeführten Agents und den entsprechenden Diensten in Azure verwendet.<br/><br/> – Die zweite App wird ausschließlich für den Zugriff auf die im Abonnement des Benutzers erstellte Key Vault-Instanz für die Migration virtueller VMware-Computer ohne Agent verwendet. [Weitere Informationen](migrate-appliance-architecture.md#appliance-registration)<br/><br/> Sie benötigen Berechtigungen zum Erstellen von Azure AD-Apps (in der Rolle „Anwendungsentwickler“ verfügbar).
**Erstellen einer Key Vault-Instanz** | Zum Migrieren von VMware-VMs mit der Migration ohne Agents erstellt Azure Migrate eine Key Vault-Instanz, um Zugriffsschlüssel für das Replikationsspeicherkonto in Ihrem Abonnement zu verwalten.<br/><br/> Für die Tresorerstellung benötigen Sie Rollenzuweisungsberechtigungen für die Ressourcengruppe, in der sich das Azure Migrate-Projekt befindet.




### <a name="assign-permissions-to-create-project"></a>Zuweisen von Berechtigungen für die Projekterstellung

1. Öffnen Sie im Azure-Portal das Abonnement, und wählen Sie **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** nach dem relevanten Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.
3. Sie sollten über die Berechtigung **Mitwirkender** oder **Besitzer** verfügen.
    - Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements.
    - Wenn Sie nicht der Besitzer des Abonnements sind, müssen Sie mit dem Besitzer zusammenarbeiten, um die Rolle zuzuweisen.

### <a name="assign-permissions-to-register-the-appliance"></a>Zuweisen von Berechtigungen zum Registrieren der Appliance

Um die Appliance zu registrieren, weisen Sie Berechtigungen für Azure Migrate zu, um die Azure AD-Apps während der Applianceregistrierung zu erstellen. Die Berechtigungen können über eine der folgenden Methoden zugewiesen werden:

- **Erteilen von Berechtigungen**: Ein Mandantenadministrator/globaler Administrator kann Benutzern unter dem Mandanten Berechtigungen zum Erstellen und Registrieren von Azure AD-Apps erteilen.
- **Zuweisen der Rolle „Anwendungsentwickler“** : Ein Mandantenadministrator/globaler Administrator kann dem Konto die Rolle „Anwendungsentwickler“ (die über die Berechtigungen verfügt) zuweisen.

> [!NOTE]
> - Die Apps verfügen nur über die oben beschriebenen Zugriffsberechtigungen für das Abonnement.
> - Sie benötigen diese Berechtigungen nur, wenn Sie eine neue Appliance registrieren. Nach Einrichtung der Appliance können die Berechtigungen wieder entfernt werden.


#### <a name="grant-account-permissions"></a>Erteilen von Kontoberechtigungen

Wenn Sie möchten, dass der Mandantenadministrator/globale Administrator Berechtigungen erteilt, gehen Sie wie folgt vor:

1. In Azure AD muss der globale oder der Mandantenadministrator zu **Azure Active Directory** > **Benutzer** > **Benutzereinstellungen** navigieren.
2. Der Administrator muss **App-Registrierungen** auf **Ja** festlegen. Dies ist eine Standardeinstellung, die nicht vertraulich ist. [Weitere Informationen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance)

    ![Azure AD-Berechtigungen](./media/tutorial-prepare-vmware/aad.png)



#### <a name="assign-application-developer-role"></a>Zuweisen der Rolle „Anwendungsentwickler“

Als Alternative kann der Mandantenadministrator/globale Administrator einem Konto die Rolle „Anwendungsentwickler“ zuweisen. [Hier finden Sie weitere Informationen](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal) zum Zuweisen von Rollen.

### <a name="assign-permissions-to-create-a-key-vault"></a>Zuweisen von Berechtigungen für die Key Vault-Erstellung

Um das Erstellen einer Key Vault-Instanz durch Azure Migrate zu ermöglichen, weisen Sie die folgenden Berechtigungen zu:

1. Wählen Sie im Azure-Portal unter der Ressourcengruppe die Option **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** nach dem relevanten Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.

    - Zum Ausführen der Serverbewertung ist die Berechtigung **Mitwirkender** ausreichend.
    - Wenn Sie eine Servermigration ohne Agent ausführen möchten, müssen Sie über die Berechtigung **Besitzer** (oder über die Berechtigungen **Mitwirkender** und **Benutzerzugriffsadministrator**) verfügen.

3. Sollten Sie nicht über die erforderlichen Berechtigungen verfügen, müssen Sie sie beim Besitzer der Ressourcengruppe anfordern.



## <a name="prepare-for-vmware-vm-assessment"></a>Vorbereiten der Bewertung von VMware-VMs

Führen Sie die folgenden Schritte aus, um die Bewertung von VMware-VMs vorzubereiten:

- **Überprüfen Sie die VMware-Einstellungen**. Stellen Sie sicher, dass die vCenter Server-Instanz und die zu migrierenden VMs die geltenden Anforderungen erfüllen.
- **Richten Sie ein Konto für die Bewertung ein**. Azure Migrate nutzt dieses Konto für den Zugriff auf die vCenter Server-Instanz, um VMs für die Bewertung zu ermitteln.
- **Überprüfen Sie die Appliance-Anforderungen**. Überprüfen Sie die Bereitstellungsanforderungen für die Azure Migrate-Appliance, bevor Sie sie bereitstellen.

### <a name="verify-vmware-settings"></a>Überprüfen der VMware-Einstellungen

1. [Überprüfen](migrate-support-matrix-vmware.md#vmware-requirements) Sie die VMware-Serveranforderungen für die Bewertung.
2. [Vergewissern Sie sich](migrate-support-matrix-vmware.md#port-access), dass auf der vCenter Server-Instanz die benötigten Ports geöffnet sind.
3. Vergewissern Sie sich in vCenter Server, dass Ihr Konto über Berechtigungen zum Erstellen einer VM mit einer OVA-Datei verfügt. Die Azure Migrate-Appliance wird unter Verwendung einer OVA-Datei als VMware-VM bereitgestellt.


### <a name="set-up-an-account-for-assessment"></a>Einrichten eines Kontos für die Bewertung

Azure Migrate muss auf die vCenter Server-Instanz zugreifen, um VMs für die Bewertung und die Migration ohne Agent zu ermitteln.

- Wenn Sie Anwendungen ermitteln oder Abhängigkeiten ohne Agent visualisieren möchten, erstellen Sie ein vCenter Server-Konto mit Lesezugriff sowie den Berechtigungen **Virtuelle Computer** > **Gastvorgänge**.

  ![Berechtigungen des vCenter Server-Kontos](./media/tutorial-prepare-vmware/vcenter-server-permissions.png)

- Falls Sie keine Anwendungsermittlung und keine Abhängigkeitsvisualisierung ohne Agent durchführen möchten, richten Sie ein schreibgeschütztes Konto für die vCenter Server-Instanz ein.

### <a name="verify-appliance-settings-for-assessment"></a>Überprüfen der Applianceeinstellungen für die Bewertung

Bevor Sie im nächsten Tutorial die Azure Migrate-Appliance einrichten und mit der Bewertung beginnen, müssen Sie die Appliance-Bereitstellung vorbereiten.

1. [Überprüfen](migrate-appliance.md#appliance---vmware) Sie die Anforderungen für die Azure Migrate-Appliance.
2. [Überprüfen](migrate-appliance.md#url-access) Sie die Azure-URLs, auf die die Appliance zugreifen muss. Sollten Sie eine URL-basierte Firewall oder einen Proxy verwenden, lassen Sie unbedingt den Zugriff auf die erforderlichen URLs zu.
3. [Überprüfen Sie die Daten](migrate-appliance.md#collected-data---vmware), die die Appliance während der Ermittlung und Bewertung sammelt.
4. [Beachten](migrate-support-matrix-vmware.md#port-access) Sie die Portzugriffsanforderungen für die Appliance.




## <a name="prepare-for-agentless-vmware-migration"></a>Vorbereiten der VMware-Migration ohne Agent

Überprüfen Sie die Anforderungen für die [Migration von VMware-VMs ohne Agent](server-migrate-overview.md).

1. [Überprüfen](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) Sie die Anforderungen für VMware-Server.
2. [Überprüfen Sie die Berechtigungen](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers), die Azure Migrate für den Zugriff auf vCenter Server benötigt.
3. [Überprüfen](migrate-support-matrix-vmware-migration.md#agentless-vmware-vms) Sie die Anforderungen für VMware-VMs.
4. [Überprüfen](migrate-support-matrix-vmware-migration.md#agentless-azure-migrate-appliance) Sie die Anforderungen für die Azure Migrate-Appliance.
5. Beachten Sie die Anforderungen für den [URL-Zugriff](migrate-appliance.md#url-access) und [Portzugriff](migrate-support-matrix-vmware-migration.md#agentless-ports).

## <a name="prepare-for-agent-based-vmware-migration"></a>Vorbereiten der Agent-basierten VMware-Migration

Überprüfen Sie die Anforderungen für die [Agent-basierte Migration](server-migrate-overview.md) von VMware-VMs.

1. [Überprüfen](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers) Sie die Anforderungen für VMware-Server.
2. [Überprüfen Sie die Berechtigungen](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers), die Azure Migrate für den Zugriff auf vCenter Server benötigt.
2. [Überprüfen Sie](migrate-support-matrix-vmware-migration.md#agent-based-vmware-vms) die Anforderungen für VMware-VMs, einschließlich der Installation des Mobilitätsdiensts auf jedem virtuellen Computer, den Sie migrieren möchten.
3. Bei der Migration mit Agent wird eine Replikationsappliance verwendet:
    - [Überprüfen](migrate-replication-appliance.md#appliance-requirements) Sie die Bereitstellungsanforderungen für die Replikationsappliance.
    - [Überprüfen Sie die Optionen](migrate-replication-appliance.md#mysql-installation) für die Installation von MySQL auf der Appliance.
    - Überprüfen Sie die [URL-](migrate-replication-appliance.md#url-access) und [Portzugriffsanforderungen](migrate-replication-appliance.md#port-access) für die Replikationsappliance.
    
## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Einrichten von Azure-Berechtigungen
> * Vorbereiten von VMware für die Bewertung und Migration


Im nächsten Tutorial erfahren Sie, wie Sie ein Azure Migrate-Projekt einrichten und VMware-VMs bewerten, die zu Azure migriert werden sollen.

> [!div class="nextstepaction"]
> [Migrieren von VMware-VMs zu Azure (ohne Agent)](./tutorial-assess-vmware.md)
