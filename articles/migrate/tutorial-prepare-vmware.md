---
title: Vorbereiten von VMware-VMs für die Bewertung/Migration mit Azure Migrate
description: Hier erfahren Sie, wie Sie virtuelle VMware-Computer auf die Bewertung/Migration mit Azure Migrate vorbereiten.
ms.topic: tutorial
ms.date: 06/08/2020
ms.custom: mvc
ms.openlocfilehash: 8b812924c0922d460c631baec8b0e13a9f45cd76
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2020
ms.locfileid: "86109574"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>Vorbereiten von VMware-VMs für die Bewertung und die Migration zu Azure

In diesem Artikel erfahren Sie, wie Sie virtuelle VMware-Computer mit [Azure Migrate](migrate-services-overview.md) für die Bewertung und die Migration zu Azure vorbereiten.

Dieses Tutorial ist das erste in einer Reihe zur Bewertung und Migration von VMware-VMs. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Vorbereiten von Azure auf die Arbeit mit Azure Migrate
> * Vorbereiten der Bewertung virtueller VMware-Computer mit dem Azure Migrate-Serverbewertungstool
> * Vorbereiten der Migration virtueller VMware-Computer mit dem Azure Migrate-Servermigrationstool 

> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert. Sie eignen sich gut als schnelles Proof of Concept. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.


## <a name="prepare-azure"></a>Vorbereiten von Azure

In der Tabelle werden die Aufgaben zusammengefasst, die in Azure ausgeführt werden müssen. Anleitungen für die jeweiligen Aufgaben finden Sie im Anschluss an die Tabelle.

**Aufgabe** | **Details** | **Berechtigungen**
--- | --- | ---
**Erstellen eines Azure Migrate-Projekts** | Ein Azure Migrate-Projekt stellt einen zentralen Ort zum Orchestrieren und Verwalten von Bewertungen und Migrationen mit Azure Migrate Tools, Microsoft-Tools und Angeboten von Drittanbietern bereit. | Ihr Azure-Konto benötigt die Berechtigung „Mitwirkender“ oder „Besitzer“ in der Ressourcengruppe, in der sich das Projekt befindet.
**Appliance registrieren** | Azure Migrate verwendet eine einfache Azure Migrate-Appliance, um virtuelle Computer zu ermitteln, mit dem Serverbewertungstool zu bewerten und unter Verwendung des Servermigrationstools per Migration ohne Agent zu migrieren. Weitere Informationen zur Registrierung finden Sie [hier](migrate-appliance-architecture.md#appliance-registration). | Ihr Azure-Konto benötigt zum Registrieren der Appliance die Berechtigung „Mitwirkender“ oder „Besitzer“ für das Azure-Abonnement.
**Erstellen von Azure AD-Apps** | Von Azure Migrate werden im Zuge der Applianceregistrierung Azure Active Directory-Apps (Azure AD-Apps) erstellt. <br/><br/> – Die erste App wird für die Kommunikation zwischen den auf der Appliance ausgeführten Agents und Azure Migrate verwendet. <br/><br/> – Die zweite App wird ausschließlich für den Zugriff auf die im Abonnement des Benutzers erstellte Key Vault-Instanz für die Migration virtueller VMware-Computer ohne Agent verwendet.   | Für Ihr Azure-Konto sind Berechtigungen zum Erstellen von Azure AD-Apps erforderlich.
**Erstellen einer Key Vault-Instanz** | Für die Migration von virtuellen VMware-Computer ohne Agent erstellt Azure Migrate eine Key Vault-Instanz, um Zugriffsschlüssel für das Replikationskonto in Ihrem Abonnement zu verwalten. | Um Azure Migrate die Erstellung der Key Vault-Instanz zu ermöglichen, müssen Berechtigungen („Besitzer“ oder „Mitwirkender“ und „Benutzerzugriffsadministrator“) für die Ressourcengruppe festgelegt werden, in der sich das Azure Migrate-Projekt befindet.


### <a name="assign-permissions-to-create-project"></a>Zuweisen von Berechtigungen für die Projekterstellung

1. Öffnen Sie im Azure-Portal das Abonnement, und wählen Sie **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** nach dem relevanten Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.
3. Sie sollten über die Berechtigung **Mitwirkender** oder **Besitzer** verfügen.
    - Wenn Sie gerade erst ein kostenloses Azure-Konto erstellt haben, sind Sie der Besitzer Ihres Abonnements.
    - Wenn Sie nicht der Besitzer des Abonnements sind, müssen Sie mit dem Besitzer zusammenarbeiten, um die Rolle zuzuweisen.

### <a name="assign-permissions-to-create-azure-ad-apps"></a>Zuweisen von Berechtigungen zum Erstellen von Azure AD-Apps

Ihr Azure-Konto benötigt Berechtigungen für die Erstellung von Azure AD-Apps, um die Appliance registrieren zu können. Weisen Sie die Berechtigungen mithilfe einer der folgenden Methoden zu:

- **Methode 1: Erteilen von Berechtigungen für das Konto:** Ein Mandantenadministrator/globaler Administrator kann Benutzerkonten unter dem Mandanten Berechtigungen zum Erstellen und Registrieren von Azure AD-Apps erteilen.
- **Methode 2: Zuweisen einer Rolle mit den Berechtigungen zu einem Benutzerkonto:** Ein Mandantenadministrator/globaler Administrator kann dem Benutzerkonto die Rolle „Anwendungsentwickler“ zuweisen. Diese Rolle verfügt über die erforderlichen Berechtigungen.

> [!NOTE]
> Sie benötigen diese Berechtigungen nur, wenn Sie eine neue Appliance registrieren. Nach Einrichtung der Appliance können die Berechtigungen wieder entfernt werden.


#### <a name="method-1-grant-permissions-to-the-account"></a>Methode 1: Erteilen von Berechtigungen für das Konto

Gehen Sie wie folgt vor, um dem Konto Berechtigungen zu erteilen:

1. Vergewissern Sie sich, dass Sie ein Mandantenadministrator/globaler Administrator sind. Navigieren Sie anschließend zu **Azure Active Directory** > **Benutzer** > **Benutzereinstellungen**.
2. Legen Sie **App-Registrierungen** auf **Ja** fest. Dies ist eine Standardeinstellung, die nicht vertraulich ist. [Weitere Informationen](../active-directory/develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance)

    ![Azure AD-Berechtigungen](./media/tutorial-prepare-vmware/aad.png)



#### <a name="method-2-assign-application-developer-role"></a>Methode 2: Zuweisen der Rolle „Anwendungsentwickler“

Als Alternative kann der Mandantenadministrator/globale Administrator einem Konto die Rolle „Anwendungsentwickler“ zuweisen. [Hier finden Sie weitere Informationen](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md) zum Zuweisen von Rollen.

### <a name="assign-permissions-to-create-a-key-vault"></a>Zuweisen von Berechtigungen für die Key Vault-Erstellung

Um das Erstellen einer Key Vault-Instanz durch Azure Migrate zu ermöglichen, weisen Sie die folgenden Berechtigungen zu:

1. Wählen Sie im Azure-Portal unter der Ressourcengruppe die Option **Zugriffssteuerung (IAM)** aus.
2. Suchen Sie unter **Zugriff überprüfen** nach dem relevanten Konto, und klicken Sie darauf, um Berechtigungen anzuzeigen.

    - Zum Ausführen der Serverbewertung ist die Berechtigung **Mitwirkender** ausreichend.
    - Wenn Sie eine Servermigration ohne Agent ausführen möchten, müssen Sie über die Berechtigung **Besitzer** (oder über die Berechtigungen **Mitwirkender** und **Benutzerzugriffsadministrator**) verfügen.

3. Sollten Sie nicht über die erforderlichen Berechtigungen verfügen, müssen Sie sie beim Besitzer der Ressourcengruppe anfordern.

## <a name="prepare-for-assessment"></a>Vorbereiten für die Bewertung

Führen Sie die folgenden Schritte aus, um die Bewertung von VMware-VMs vorzubereiten:

1. **Überprüfen Sie die VMware-Einstellungen**. Stellen Sie sicher, dass die vCenter Server-Instanz und die zu migrierenden VMs die geltenden Anforderungen erfüllen.
2. **Richten Sie Berechtigungen für die Bewertung ein.** Azure Migrate nutzt ein vCenter-Konto für den Zugriff auf die vCenter Server-Instanz, um virtuelle Computer zu ermitteln und zu bewerten.
3. **Überprüfen Sie die Appliance-Anforderungen**. Überprüfen Sie die Bereitstellungsanforderungen für die Azure Migrate-Appliance, bevor Sie sie im nächsten Tutorial bereitstellen.

### <a name="verify-vmware-settings"></a>Überprüfen der VMware-Einstellungen

1. [Überprüfen Sie die VMware-Anforderungen](migrate-support-matrix-vmware.md#vmware-requirements) für die Bewertung.
2. [Vergewissern Sie sich](migrate-support-matrix-vmware.md#port-access-requirements), dass auf der vCenter Server-Instanz die benötigten Ports geöffnet sind.
3. Vergewissern Sie sich in vCenter Server, dass Ihr Konto über Berechtigungen zum Erstellen eines virtuellen Computers mit einer OVA-Datei verfügt. Dies ist erforderlich, wenn Sie die Azure Migrate-Appliance unter Verwendung einer OVA-Datei als virtuellen VMware-Computer bereitstellen.
4. Für virtuelle Computer müssen vor der Migration zu Azure ein paar Änderungen vorgenommen werden.

    - Bei einigen Betriebssystemen führt Azure Migrate diese Änderungen automatisch durch. 
    - Es ist wichtig, diese Änderungen vorzunehmen, bevor Sie mit der Migration beginnen. Wenn Sie den virtuellen Computer migrieren, bevor Sie die Änderung vorgenommen haben, wird der virtuelle Computer in Azure unter Umständen nicht gestartet.
    - Überprüfen Sie die erforderlichen Änderungen für [Windows](prepare-for-migration.md#windows-machines) und [Linux](prepare-for-migration.md#linux-machines).


### <a name="set-up-permissions-for-assessment"></a>Einrichten von Berechtigungen für die Bewertung

Azure Migrate muss auf die vCenter Server-Instanz zugreifen, damit die Azure Migrate-Appliance virtuelle Computer für die Bewertung und die Migration ohne Agent ermitteln kann. Richten Sie wie folgt ein Konto ein:

1. Navigieren Sie im vSphere-Webclient zu **Administration** > **Access** > **SSO Users and Groups** („Verwaltung“ > „Zugriff“ > „SSO-Benutzer und -Gruppen“).
2. Klicken Sie unter **Users** (Benutzer) auf das Symbol **New User** (Neuer Benutzer).
3. Geben Sie die Details des neuen Benutzers ein.
4. Wählen Sie geeignete Berechtigungen aus, und orientieren Sie sich dabei an den Tabellenwerten.

    **Feature** | **Kontoberechtigungen**
    --- | ---
    [Bewerten von virtuellen Computern](tutorial-assess-vmware.md) | Für die Bewertung benötigt das Konto lediglich Lesezugriff.
    [Ermitteln von VM-Apps, -Rollen und -Features](how-to-discover-applications.md) | Wenn Sie die App-Ermittlung verwenden möchten, müssen für das Konto mit reinem Lesezugriff, das für die Bewertung verwendet wird, Berechtigungen für **Virtuelle Computer** > **Gastvorgänge** aktiviert werden.
    [Analysieren von Abhängigkeiten auf virtuellen Computern (ohne Agent)](how-to-create-group-machine-dependencies-agentless.md) | Wenn Sie Abhängigkeiten analysieren möchten, müssen für das Konto mit reinem Lesezugriff, das für die Bewertung verwendet wird, Berechtigungen für **Virtuelle Computer** > **Gastvorgänge** aktiviert werden.
    
> [!NOTE]
> Wenn Sie die VM-Ermittlung für die Bewertung auf einen bestimmten Bereich beschränken möchten, lesen Sie [diesen Artikel](set-discovery-scope.md#assign-a-role-for-assessment).

### <a name="verify-appliance-settings-for-assessment"></a>Überprüfen der Applianceeinstellungen für die Bewertung

Im [nächsten Tutorial](tutorial-assess-vmware.md) wird die Azure Migrate-Appliance eingerichtet und die Bewertung gestartet. Überprüfen Sie jedoch zuvor die Applianceanforderungen: 

1. [Überprüfen Sie die Anforderungen](migrate-appliance.md#appliance---vmware) für die Bereitstellung der Azure Migrate-Appliance.
2. Überprüfen Sie die Azure-URLs, die die Appliance für den Zugriff in [öffentlichen Clouds](migrate-appliance.md#public-cloud-urls) und [Azure Government-Clouds](migrate-appliance.md#government-cloud-urls) benötigt.
3. [Beachten Sie](migrate-support-matrix-vmware.md#port-access-requirements) die von der Appliance verwendeten Ports.
4. [Informieren Sie sich über die Daten](migrate-appliance.md#collected-data---vmware), die von der Appliance im Zuge der Ermittlung und Bewertung gesammelt werden.

## <a name="prepare-for-agentless-vmware-migration"></a>Vorbereiten der VMware-Migration ohne Agent

Virtuelle VMware-Computer können [mit oder ohne Agent](server-migrate-overview.md) migriert werden. In diesem Abschnitt werden die Anforderungen für die Migration ohne Agent zusammengefasst.

1. [Entscheiden Sie](server-migrate-overview.md#compare-migration-methods), ob Sie die Migration ohne Agent verwenden möchten.
2. [Überprüfen](migrate-support-matrix-vmware-migration.md#vmware-requirements-agentless) Sie die Hypervisor-Anforderungen für Computer, die Sie migrieren möchten.
3. [Überprüfen](migrate-support-matrix-vmware-migration.md#vm-requirements-agentless) Sie die Anforderungen für virtuelle VMware-Computer, die Sie ohne Agent migrieren möchten.
4. [Überprüfen](migrate-support-matrix-vmware-migration.md#appliance-requirements-agentless) Sie die Anforderungen für die Migration ohne Agent im Zusammenhang mit der Azure Migrate-Appliance.
5. Berücksichtigen Sie den erforderlichen URL-Zugriff für [öffentliche Clouds](migrate-appliance.md#public-cloud-urls) und [Azure Government-Clouds](migrate-appliance.md#government-cloud-urls).
6. Überprüfen Sie die [Portzugriffsanforderungen](migrate-support-matrix-vmware-migration.md#port-requirements-agentless).
7. Richten Sie Berechtigungen für die Migration ohne Agent ein, wie im Anschluss beschrieben.


### <a name="assign-permissions-to-an-account"></a>Zuweisen von Berechtigungen zu einem Konto

Von der Azure Migrate-Appliance wird eine Verbindung mit der vCenter Server-Instanz hergestellt, um virtuelle Computer zu ermitteln und per Migration ohne Agent zu migrieren. Sie können dem Benutzerkonto die von der Appliance benötigten Berechtigungen zuweisen oder eine Rolle mit den Berechtigungen erstellen und diese Rolle einem Benutzerkonto zuweisen.

1. Navigieren Sie im vSphere-Webclient zu **Administration** > **Access** > **SSO Users and Groups** („Verwaltung“ > „Zugriff“ > „SSO-Benutzer und -Gruppen“).
2. Klicken Sie unter **Users** (Benutzer) auf das Symbol **New User** (Neuer Benutzer).
3. Geben Sie die Details des neuen Benutzers ein.
4. Weisen Sie [diese Berechtigungen](migrate-support-matrix-vmware-migration.md#vmware-requirements-agentless) zu.

#### <a name="create-a-role-and-assign-to-an-account"></a>Erstellen einer Rolle und Zuweisen zu einem Konto

Alternativ können Sie ein Konto erstellen. Erstellen Sie anschließend eine Rolle, und weisen Sie sie dem Konto zu:

1. Melden Sie sich als vCenter Server-Administrator beim Sphere-Webclient an.
2. Wählen Sie die vCenter Server-Instanz und anschließend **Create role** (Rolle erstellen) aus.
3. Geben Sie einen Rollennamen an (z. B. <em>Azure_Migrate</em>), und weisen Sie der Rolle die [erforderlichen Berechtigungen](migrate-support-matrix-vmware-migration.md#vmware-requirements-agentless) zu.

    ![Berechtigungen des vCenter Server-Kontos](./media/tutorial-prepare-vmware/vcenter-server-permissions.png)

4. Erstellen Sie nun ein Konto, und weisen Sie diesem die Rolle zu.

> [!NOTE]
> Wenn Sie die VM-Ermittlung für die Migration ohne Agent auf einen bestimmten Bereich beschränken möchten, lesen Sie [diesen Artikel](set-discovery-scope.md#assign-a-role-for-agentless-migration).



## <a name="prepare-for-agent-based-vmware-migration"></a>Vorbereiten der Agent-basierten VMware-Migration

Virtuelle VMware-Computer können [mit oder ohne Agent](server-migrate-overview.md) migriert werden. In diesem Abschnitt werden die Anforderungen für die Agent-basierte Migration zusammengefasst.

1. [Entscheiden Sie](server-migrate-overview.md#compare-migration-methods), ob Sie die Agent-basierte Migration verwenden möchten.
1. [Überprüfen](migrate-support-matrix-vmware-migration.md#vmware-requirements-agent-based) Sie die Hypervisor-Anforderungen für Computer, die Sie migrieren möchten.
2. [Überprüfen Sie](migrate-support-matrix-vmware-migration.md#vm-requirements-agent-based) die Anforderungen für VMware-VMs, einschließlich der Installation des Mobilitätsdiensts auf jedem virtuellen Computer, den Sie migrieren möchten.
3. Bei der Migration mit Agent wird eine Replikationsappliance verwendet:
    - [Überprüfen](migrate-replication-appliance.md#appliance-requirements) Sie die Bereitstellungsanforderungen für die Replikationsappliance.
    - [Überprüfen Sie die Optionen](migrate-replication-appliance.md#mysql-installation) für die Installation von MySQL auf der Appliance.
    - Berücksichtigen Sie den erforderlichen URL-Zugriff für [öffentliche Clouds](migrate-replication-appliance.md#url-access) und [Azure Government-Clouds](migrate-replication-appliance.md#azure-government-url-access).
    - Überprüfen Sie die [Portzugriffsanforderungen](migrate-replication-appliance.md#port-access) für die Replikationsappliance.
4. Für virtuelle Computer müssen vor der Migration zu Azure ein paar Änderungen vorgenommen werden. Überprüfen Sie die erforderlichen Änderungen für [Windows](prepare-for-migration.md#windows-machines) und [Linux](prepare-for-migration.md#linux-machines).



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Einrichten von Azure-Berechtigungen
> * Vorbereiten von VMware für die Bewertung und Migration


Im nächsten Tutorial erfahren Sie, wie Sie ein Azure Migrate-Projekt einrichten und VMware-VMs bewerten, die zu Azure migriert werden sollen.

> [!div class="nextstepaction"]
> [Migrieren von VMware-VMs zu Azure (ohne Agent)](./tutorial-assess-vmware.md)
