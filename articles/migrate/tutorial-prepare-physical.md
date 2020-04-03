---
title: Vorbereiten physischer Server auf die Bewertung/Migration mit Azure Migrate
description: Es wird beschrieben, wie Sie die Bewertung/Migration von physischen Servern mit Azure Migrate vorbereiten.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 11/19/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 5f9048b08b3e77a0c8d5ae9a9d10c614a4e0af61
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80336685"
---
# <a name="prepare-for-assessment-and-migration-of-physical-servers-to-azure"></a>Vorbereiten auf die Bewertung und Migration physischer Server zu Azure

In diesem Artikel wird beschrieben, wie Sie sich mit [Azure Migrate](migrate-services-overview.md) auf die Bewertung und Migration physischer zu Azure vorbereiten.

[Azure Migrate](migrate-overview.md) stellt einen Hub mit Tools bereit, die Ihnen dabei helfen, Apps, Infrastrukturen und Workloads zu ermitteln, zu bewerten und zu Microsoft Azure zu migrieren. Der Hub umfasst Azure Migrate-Tools sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs). 

Dieses Tutorial ist das erste in einer Reihe und zeigt Ihnen, wie Sie physische Server mit Azure Migrate bewerten. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereiten Sie Azure vor. Richten sie Berechtigungen für Ihr Azure-Konto und Ihre Ressourcen ein, um Azure Migrate verwenden zu können.
> * Bereiten Sie lokale physischer Server auf die Serverbewertung vor.


> [!NOTE]
> In den Tutorials wird der einfachste Bereitstellungspfad für ein Szenario erläutert, damit Sie schnell einen Proof of Concept einrichten können. Die Tutorials verwenden nach Möglichkeit Standardoptionen und zeigen nicht alle möglichen Einstellungen und Pfade. Ausführliche Anweisungen finden Sie Anleitungen zur Bewertung physischer Server.


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


## <a name="prepare-for-physical-server-assessment"></a>Vorbereiten auf die Bewertung physischer Server

Zur Vorbereitung auf die Bewertung physischer Server müssen Sie Einstellungen des physischen Servers sowie Einstellungen für die Appliancebereitstellung überprüfen:

### <a name="verify-physical-server-settings"></a>Überprüfen der Einstellungen des physischen Servers

1. Überprüfen Sie für die Serverbewertung die [Anforderungen an physische Server](migrate-support-matrix-physical.md#physical-server-requirements).
2. Stellen Sie sicher, dass auf physischen Servern die [erforderlichen Ports](migrate-support-matrix-physical.md#port-access) geöffnet sind.


### <a name="verify-appliance-settings"></a>Überprüfen von Appliance-Einstellungen

Bevor Sie im nächsten Tutorial die Azure Migrate-Appliance einrichten und mit der Bewertung beginnen, müssen Sie die Appliance-Bereitstellung vorbereiten.

1. [Lesen](migrate-appliance.md#appliance---physical) Sie die Applianceanforderungen für physische Server.
2. [Überprüfen](migrate-appliance.md#url-access) Sie die Azure-URLs, auf die die Appliance zugreifen muss.
3. [Überprüfen](migrate-appliance.md#collected-data---vmware) Sie die Daten, die die Appliance während der Ermittlung und Bewertung sammelt.
4. [Beachten](migrate-support-matrix-physical.md#port-access) Sie die Portzugriffsanforderungen für die Bewertung physischer Server.


### <a name="set-up-an-account-for-physical-server-discovery"></a>Einrichten eines Kontos für die Ermittlung physischer Server

Azure Migrate benötigt Berechtigungen zum Ermitteln lokaler Server.

- **Windows:** Richten Sie auf allen Windows-Servern, die Sie in die Ermittlung einschließen möchten, ein lokales Benutzerkonto ein. Das Benutzerkonto muss den folgenden Gruppen hinzugefügt werden: Remoteverwaltungsbenutzer, Leistungsüberwachungsbenutzer und Leistungsprotokollbenutzer.
- **Linux:** Sie benötigen ein root-Konto auf den Linux-Servern, die Sie ermitteln möchten.

## <a name="prepare-for-physical-server-migration"></a>Vorbereiten auf die Migration physischer Server

Lesen Sie die Anforderungen für die Migration physischer Server.

- [Lesen](migrate-support-matrix-physical-migration.md#physical-server-requirements) Sie die Anforderungen an physische Server für die Migration.
- Von der Azure Bei der Servermigration wird ein Replikationsserver für die Migration physischer Server verwendet:
    - [Lesen](migrate-replication-appliance.md#appliance-requirements) Sie die Bereitstellungsanforderungen für die Replikationsappliance, und informieren Sie sich über die [Optionen](migrate-replication-appliance.md#mysql-installation) für die Installation von MySQL auf der Appliance.
    - Überprüfen Sie die [URL-](migrate-replication-appliance.md#url-access) und [Portzugriffsanforderungen] (migrate-replication-appliance.md#port-access) für die Replikationsappliance.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Einrichten von Azure-Kontoberechtigungen.
> * Physische Server auf die Bewertung vorbereitet.

Fahren Sie mit dem nächsten Tutorial fort, um ein Azure Migrate-Projekt zu erstellen und physische Server für die Migration zu Azure zu bewerten.

> [!div class="nextstepaction"]
> [Bewerten physischer Server](./tutorial-assess-physical.md)
