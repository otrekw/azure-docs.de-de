---
title: Sichern von SQL Server-Datenbanken auf virtuellen Azure-Computern
description: In diesem Artikel erfahren Sie, wie Sie SQL Server-Datenbanken auf virtuellen Azure-Computern mit Azure Backup sichern können.
ms.reviewer: vijayts
ms.topic: conceptual
ms.date: 09/11/2019
ms.openlocfilehash: 887f15deed74330cf132e0574d166c074d2c7cad
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81685712"
---
# <a name="back-up-sql-server-databases-in-azure-vms"></a>Sichern von SQL Server-Datenbanken auf virtuellen Azure-Computern

SQL Server-Datenbanken sind kritische Workloads, die eine niedrige Recovery Point Objective (RPO) und lange Aufbewahrungszeit erfordern. Sie können auf virtuellen Azure-Computern (VMs) ausgeführte SQL Server-Datenbanken mithilfe von [Azure Backup](backup-overview.md) sichern.

In diesem Artikel wird veranschaulicht, wie eine SQL Server-Datenbank, die auf einer Azure-VM ausgeführt wird, in einem Azure Backup Recovery Services-Tresor gesichert wird.

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Erstellen und Konfigurieren eines Tresors
> * Ermitteln von Datenbanken und Einrichten von Sicherungen
> * Einrichten des automatischen Schutzes für Datenbanken

>[!NOTE]
>**Das vorläufige Löschen für SQL Server auf Azure-VMs und das vorläufige Löschen für SAP HANA in Azure-VM-Workloads ist jetzt in der Vorschau verfügbar.**<br>
>Um sich für die Vorschauversion zu registrieren, schreiben Sie an AskAzureBackupTeam@microsoft.com.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie eine SQL Server-Datenbank sichern können, müssen folgende Kriterien erfüllt sein:

1. Bestimmen oder erstellen Sie einen [Recovery Services-Tresor](backup-sql-server-database-azure-vms.md#create-a-recovery-services-vault) in derselben Region und demselben Abonnement wie die VM, die die SQL Server-Instanz hostet.
2. Stellen Sie sicher, dass der virtuelle Computer über [Netzwerkkonnektivität](backup-sql-server-database-azure-vms.md#establish-network-connectivity) verfügt.
3. Vergewissern Sie sich, dass die SQL Server-Datenbanken die [Benennungsrichtlinien für Datenbanken für Azure Backup](#database-naming-guidelines-for-azure-backup) befolgen.
4. Stellen Sie sicher, dass keine anderen Sicherungslösungen für die Datenbank aktiviert sind. Deaktivieren Sie alle anderen SQL Server-Sicherungen, bevor Sie die Datenbank sichern.

> [!NOTE]
> Sie können Azure Backup für eine Azure-VM und auch für eine auf der VM ausgeführte SQL Server-Datenbank ohne Konflikte aktivieren.

### <a name="establish-network-connectivity"></a>Herstellen der Netzwerkverbindung

Eine SQL Server-VM benötigt für alle Vorgänge eine Verbindung mit öffentlichen IP-Adressen von Azure. VM-Vorgänge (Ermitteln von Datenbanken, Konfigurieren von Sicherungen, Planen von Sicherungen, Zurücksetzen von Wiederherstellungspunkten usw.) schlagen ohne Verbindung mit öffentlichen IP-Adressen von Azure fehl.

Stellen Sie die Verbindung mithilfe einer der folgenden Optionen her:

#### <a name="allow-the-azure-datacenter-ip-ranges"></a>IP-Adressbereiche des Azure-Rechenzentrums zulassen

Diese Option lässt die [IP-Adressbereiche](https://www.microsoft.com/download/details.aspx?id=41653) in der heruntergeladenen Datei zu. Verwenden Sie für den Zugriff auf eine Netzwerksicherheitsgruppe (NSG) das Cmdlet Set-AzureNetworkSecurityRule. Wenn Ihre Liste sicherer Empfänger nur regionsspezifische IP-Adressen enthält, müssen Sie auch die Liste sicherer Empfänger im Diensttag „Azure Active Directory (Azure AD)“ aktualisieren, um die Authentifizierung zu ermöglichen.

#### <a name="allow-access-using-nsg-tags"></a>Zugriff mithilfe von NSG-Tags zulassen

Wenn Sie die Konnektivität mit NSG einschränken, sollten Sie den ausgehenden Zugriff auf Azure Backup mit dem Diensttag AzureBackup zulassen. Zusätzlich sollten Sie die Konnektivität für Authentifizierung und Datenübertragung mithilfe von [Regeln](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) für Azure AD und Azure Storage ermöglichen. Dies kann über das Azure-Portal oder mithilfe von PowerShell erfolgen.

So erstellen Sie eine Regel über das Portal

  1. Navigieren Sie unter **Alle Dienste** zu **Netzwerksicherheitsgruppen**, und wählen Sie die Netzwerksicherheitsgruppe aus.
  2. Wählen Sie unter **Einstellungen** die Option **Ausgangssicherheitsregeln** aus.
  3. Wählen Sie **Hinzufügen**. Geben Sie die erforderlichen Informationen zum Erstellen einer neuen Regel ein, wie unter [Einstellungen zu Sicherheitsregeln](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group#security-rule-settings) beschrieben. Stellen Sie sicher, dass die Option **Ziel** auf **Diensttag** und **Zieldiensttag** auf **AzureBackup** festgelegt wurde.
  4. Klicken Sie auf **Hinzufügen**, um die neu erstellte Ausgangssicherheitsregel zu speichern.

So erstellen Sie eine Regel mit PowerShell:

 1. Hinzufügen von Azure-Anmeldeinformationen und Aktualisieren der nationalen Clouds<br/>
      `Add-AzureRmAccount`<br/>

 2. Auswählen des NSG-Abonnements<br/>
      `Select-AzureRmSubscription "<Subscription Id>"`

 3. Auswählen der NSG<br/>
    `$nsg = Get-AzureRmNetworkSecurityGroup -Name "<NSG name>" -ResourceGroupName "<NSG resource group name>"`

 4. Hinzufügen der Ausgangsregel zum Zulassen des Diensttags „Azure Backup“<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "AzureBackupAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "AzureBackup" -DestinationPortRange 443 -Description "Allow outbound traffic to Azure Backup service"`

 5. Hinzufügen der Ausgangsregel zum Zulassen des Diensttags „Storage“<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "StorageAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "Storage" -DestinationPortRange 443 -Description "Allow outbound traffic to Azure Backup service"`

 6. Hinzufügen der Ausgangsregel zum Zulassen des Diensttags „AzureActiveDirectory“<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "AzureActiveDirectoryAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "AzureActiveDirectory" -DestinationPortRange 443 -Description "Allow outbound traffic to AzureActiveDirectory service"`

 7. Speichern der NSG<br/>
    `Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg`

**Zulassen des Zugriffs mithilfe von Azure Firewall-Tags**. Wenn Sie Azure Firewall verwenden, erstellen Sie eine Anwendungsregel mithilfe des [FQDN-Tags](https://docs.microsoft.com/azure/firewall/fqdn-tags) „AzureBackup“. Es erlaubt den ausgehenden Zugriff auf Azure Backup.

**Bereitstellen eines HTTP-Proxyservers für das Weiterleiten von Datenverkehr**. Wenn Sie eine SQL Server-Datenbank auf einem virtuellen Azure-Computer sichern, verwendet die Sicherungserweiterung auf dem virtuellen Computer die HTTPS-APIs, um Verwaltungsbefehle an Azure Backup und Daten an Azure Storage zu senden. Die Sicherungserweiterung verwendet auch Azure AD zur Authentifizierung. Leiten Sie den Datenverkehr der Sicherungserweiterung für diese drei Dienste über den HTTP-Proxy weiter. Für Azure Backup werden keine Platzhalterdomänen verwendet, die der Zulassungsliste für Ihre Proxyregeln hinzugefügt werden können. Sie müssen die Bereiche mit den öffentlichen IP-Adressen für diese Dienste verwenden, die von Azure bereitgestellt werden. Die Erweiterungen sind die einzigen Komponenten, die für den Zugriff auf das öffentliche Internet konfiguriert sind.

Die Konnektivitätsoptionen haben u.a. die folgenden Vor- und Nachteile:

**Option** | **Vorteile** | **Nachteile**
--- | --- | ---
Zulassen von IP-Adressbereichen | Keine zusätzlichen Kosten | Komplexe Verwaltung, da sich die IP-Adressbereiche im Laufe der Zeit ändern <br/><br/> Zugriff auf alle Azure-Dienste, nicht nur auf Azure Storage
Verwenden von NSG-Diensttags | Einfachere Verwaltung, da Bereichsänderungen automatisch zusammengeführt werden <br/><br/> Keine zusätzlichen Kosten <br/><br/> | Nur mit NSGs möglich <br/><br/> Zugriff auf den gesamten Dienst
Verwenden von FQDN-Tags von Azure Firewall | Einfachere Verwaltung, da die erforderlichen FQDNs automatisch verwaltet werden | Nur mit Azure Firewall möglich
Verwenden eines HTTP-Proxys | Zentraler Internetzugriffspunkt für VMs <br/> | Zusätzliche Kosten für das Ausführen einer VM mit der Proxysoftware <br/> Keine veröffentlichten FQDN-Adressen, Zulassungsregeln unterliegen Änderungen von Azure-IP-Adressen

#### <a name="private-endpoints"></a>Private Endpunkte

[!INCLUDE [Private Endpoints](../../includes/backup-private-endpoints.md)]

### <a name="database-naming-guidelines-for-azure-backup"></a>Benennungsrichtlinien für Datenbanken in Azure Backup

Verwenden Sie in Datenbanknamen nicht die folgenden Elemente:

* Nachstehende und führende Leerzeichen
* Nachstehende Ausrufezeichen (!)
* Schließende eckige Klammern (])
* Semikolon ';'
* Schrägstrich '/'

Obwohl Aliase für nicht unterstützte Zeichen möglich ist, wird empfohlen, sie zu vermeiden. Weitere Informationen finden Sie unter [Grundlegendes zum Tabellendienst-Datenmodell](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model).

>[!NOTE]
>Der Vorgang **Schutz konfigurieren** wird für Datenbanken mit Sonderzeichen wie „+“ oder „&“ im Namen nicht unterstützt. Sie können entweder den Datenbanknamen ändern oder **Automatischen Schutz** aktivieren, sodass diese Datenbanken erfolgreich geschützt werden können.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>Ermitteln von SQL Server-Datenbanken

Ermitteln von auf einer VM ausgeführten Datenbanken:

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) den Recovery Services-Tresor, mit dem Sie die Datenbank sichern.

2. Wählen Sie **Sichern** im Dashboard von **Recovery Services-Tresor** aus.

   ![Auswählen von „Sichern“, um Menü „Sicherungsziel“ zu öffnen](./media/backup-azure-sql-database/open-backup-menu.png)

3. Legen Sie unter **Sicherungsziel** die Einstellung **Wo wird die Workload ausgeführt?** auf **Azure** fest.

4. Wählen Sie unter **Was möchten Sie sichern?** die Option **SQL Server in Azure VM** aus.

    ![Auswählen von „SQL Server in Azure-VM“ für die Sicherung](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

5. Wählen Sie in **Sicherungsziel** > **DBs in VMs ermitteln** die Option **Ermittlung starten** aus, um nach nicht geschützten virtuellen Computern im Abonnement zu suchen. Diese Suche kann je nach Anzahl ungeschützter VMs im Abonnement eine Weile dauern.

   * Nicht geschützte virtuelle Computer sollten nach der Ermittlung sortiert nach Name und Ressourcengruppe in der Liste angezeigt werden.
   * Wenn eine VM nicht wie erwartet aufgeführt wird, prüfen Sie, ob sie bereits in einem Tresor gesichert wird.
   * Mehrere VMs können den gleichen Namen aufweisen, gehören dann aber verschiedenen Ressourcengruppen an.

     ![Ausstehende Sicherung bei der Suche nach Datenbanken in VMs](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. Wählen Sie in der Liste der virtuellen Computer den virtuellen Computer mit der SQL Server-Datenbank und dann **Datenbankermittlung** aus.

7. Verfolgen Sie die Datenbankermittlung im Bereich **Benachrichtigungen**. Der Zeitaufwand für diese Aktion hängt von der Anzahl von Datenbanken auf VMs ab. Wenn die ausgewählten Datenbanken ermittelt wurden, wird eine Erfolgsmeldung angezeigt.

    ![Meldung über erfolgreiche Bereitstellung](./media/backup-azure-sql-database/notifications-db-discovered.png)

8. Azure Backup ermittelt alle SQL Server-Datenbanken auf dem virtuellen Computer. Während der Ermittlung geschieht im Hintergrund Folgendes:

    * Azure Backup registriert die VM beim Tresor für die Sicherung der Workload. Alle Datenbanken auf der registrierten VM können nur in diesem Tresor gesichert werden.
    * Azure Backup installiert die Erweiterung AzureBackupWindowsWorkload auf der VM. In einer SQL Server-Datenbank ist kein Agent installiert.
    * Azure Backup erstellt das Dienstkonto „NT Service\AzureWLBackupPluginSvc“ auf der VM.
      * Für alle Sicherungs- und Wiederherstellungsvorgänge wird das Dienstkonto verwendet.
      * „NT Service\AzureWLBackupPluginSvc erfordert“ Systemadministratorberechtigungen für SQL Server. Die Erweiterung SqlIaaSExtension ist auf allen in Azure Marketplace erstellten SQL Server-VMs vorinstalliert. Die Erweiterung AzureBackupWindowsWorkload verwendet SQLIaaSExtension, um erforderliche Berechtigungen automatisch abzurufen.
    * Wenn Sie die VM nicht im Marketplace erstellt haben oder SQL Server 2008 und 2008 R2 verwenden, ist die Erweiterung „SqlIaaSExtension“ möglicherweise nicht auf der VM installiert, und der Ermittlungsvorgang schlägt mit der Fehlermeldung „UserErrorSQLNoSysAdminMembership“ fehl. Führen Sie die Schritte unter [Set VM permissions (Einrichten von VM-Berechtigungen)](backup-azure-sql-database.md#set-vm-permissions) aus, um dieses Problem zu beheben.

        ![Auswählen der VM und der Datenbank](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup"></a>Konfigurieren der Sicherung  

1. In **Sicherungsziel** > **Schritt 2: Sicherung konfigurieren** wählen Sie **Sicherung konfigurieren** aus.

   ![Auswählen von „Sicherung konfigurieren“](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

2. Unter **Elemente für die Sicherung auswählen** sehen Sie alle registrierten Verfügbarkeitsgruppen und eigenständigen SQL Server-Instanzen. Klicken Sie auf den Pfeil links neben einer Zeile, um die Liste aller ungeschützten Datenbanken in dieser Instanz oder Always On-Verfügbarkeitsgruppe zu erweitern.  

    ![Anzeigen aller SQL Server-Instanzen mit eigenständigen Datenbanken](./media/backup-azure-sql-database/list-of-sql-databases.png)

3. Wählen Sie alle Datenbanken, die Sie schützen möchten, und klicken Sie dann auf **OK**.

   ![Schützen der Datenbank](./media/backup-azure-sql-database/select-database-to-protect.png)

   Zum Optimieren von Sicherungslasten legt Azure Backup die maximale Anzahl von Datenbanken in einem Sicherungsauftrag auf 50 fest.

     * Zum Schützen von mehr als 50 Datenbanken konfigurieren Sie mehrere Sicherungen.
     * Zum [Aktivieren](#enable-auto-protection) der gesamten Instanz oder Always On-Verfügbarkeitsgruppe wählen Sie in der Dropdownliste **AUTOMATISCHER SCHUTZ** die Option **EIN** aus und klicken dann auf **OK**.

    > [!NOTE]
    > Das Feature [Automatischer Schutz](#enable-auto-protection) ermöglicht nicht nur den gleichzeitigen Schutz aller vorhandenen Datenbanken, sondern schützt auch automatisch alle neuen Datenbanken, die dieser Instanz oder Verfügbarkeitsgruppe hinzugefügt werden.  

4. Klicken Sie auf **OK**, um **Sicherungsrichtlinie** zu öffnen.

    ![Aktivieren des automatischen Schutzes für die Always On-Verfügbarkeitsgruppe](./media/backup-azure-sql-database/enable-auto-protection.png)

5. Wählen Sie unter **Sicherungsrichtlinie**eine Richtlinie und anschließend **OK** aus.

   * Wählen Sie als Standardrichtlinie HourlyLogBackup aus.
   * Auswählen einer vorhandenen, zuvor für SQL erstellten Sicherungsrichtlinie
   * Definieren einer neuen Richtlinie basierend auf Ihrer RPO und Ihrer Beibehaltungsdauer

     ![Auswählen der Sicherungsrichtlinie](./media/backup-azure-sql-database/select-backup-policy.png)

6. Klicken Sie unter **Sicherung** auf **Sicherung aktivieren**.

    ![Aktivieren der ausgewählten Sicherungsrichtlinie](./media/backup-azure-sql-database/enable-backup-button.png)

7. Den Konfigurationsprozess können Sie im Portalbereich **Benachrichtigungen** nachverfolgen.

    ![Benachrichtigungsbereich](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>Erstellen einer Sicherungsrichtlinie

Eine Sicherungsrichtlinie legt fest, wann Sicherungen erstellt und wie lange sie aufbewahrt werden.

* Eine Richtlinie wird auf Tresorebene erstellt.
* Mehrere Tresore können die gleiche Sicherungsrichtlinie verwenden. Allerdings müssen Sie die Sicherungsrichtlinie auf jeden Tresor anwenden.
* Wenn Sie eine Sicherungsrichtlinie erstellen, entspricht eine tägliche vollständige Sicherung der Standardeinstellung.
* Sie können eine differenzielle Sicherung nur hinzufügen, wenn Sie für die vollständige Sicherung festlegen, dass diese wöchentlich erfolgt.
* Erfahren Sie mehr über [verschiedene Arten von Sicherungsrichtlinien](backup-architecture.md#sql-server-backup-types).

So erstellen Sie eine Sicherungsrichtlinie

1. Klicken Sie im Tresor auf **Sicherungsrichtlinien** > **Hinzufügen**.
2. Wählen Sie in **Hinzufügen** zum Definieren des Richtlinientyps **SQL Server in Azure-VM**  aus.

   ![Auswählen eines Richtlinientyps für die neue Sicherungsrichtlinie](./media/backup-azure-sql-database/policy-type-details.png)

3. Geben Sie unter **Richtlinienname** einen Namen für die neue Richtlinie ein.
4. Wählen Sie in **Richtlinie für vollständige Sicherung** eine **Sicherungshäufigkeit** aus. Wählen Sie entweder **Täglich** oder **Wöchentlich**.

   * Wählen Sie für **Täglich** die Uhrzeit und die Zeitzone für den Beginn des Sicherungsauftrags aus.
   * Wählen Sie für **Wöchentlich** den Wochentag, die Uhrzeit und die Zeitzone für den Beginn des Sicherungsauftrags aus.
   * Sie müssen eine vollständige Sicherung ausführen, da die Option **Vollständige Sicherung** nicht deaktiviert werden kann.
   * Klicken Sie auf **Vollständige Sicherung**, um die Richtlinie anzuzeigen.
   * Sie können keine differenziellen Sicherungen für tägliche vollständige Sicherungen erstellen.

     ![Neue Felder für Sicherungsrichtlinien](./media/backup-azure-sql-database/full-backup-policy.png)  

5. In **BEIBEHALTUNGSDAUER** sind standardmäßig alle Optionen aktiviert. Deaktivieren Sie alle Optionen für die Beibehaltungsdauer, die Sie nicht wünschen, und legen Sie dann die zu verwendenden Intervalle fest.

    * Die Mindestbeibehaltungsdauer beträgt für alle Sicherungstypen (vollständig/differenziell/Protokoll) sieben Tage.
    * Wiederherstellungspunkte werden unter Berücksichtigung ihrer Beibehaltungsdauer mit einer Markierung versehen. Wenn Sie beispielsweise eine tägliche vollständige Sicherung wählen, wird pro Tag nur eine vollständige Sicherung ausgelöst.
    * Die Sicherung für einen bestimmten Tag wird auf Grundlage der wöchentlichen Beibehaltungsdauer und der wöchentlichen Beibehaltungseinstellung markiert und beibehalten.
    * Mit der monatlichen und jährlichen Beibehaltungsdauer verhält es sich ähnlich.

       ![Intervalleinstellungen für Beibehaltungsdauer](./media/backup-azure-sql-database/retention-range-interval.png)

6. Wählen Sie im Menü **Richtlinie für vollständige Sicherung** **OK** aus, um die Einstellungen zu übernehmen.
7. Um eine Richtlinie für eine differenzielle Sicherung hinzuzufügen, wählen Sie **Differenzielle Sicherung** aus.

   ![Intervalleinstellungen für Beibehaltungsdauer](./media/backup-azure-sql-database/retention-range-interval.png)
   ![Öffnen des Menüs „Richtlinie für differenzielle Sicherung“](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

8. Wählen Sie in **Richtlinie für differenzielle Sicherung** die Option **Aktivieren** aus, um die Einstellungen für Häufigkeit und Beibehaltung vorzunehmen.

    * Pro Tag können Sie nur eine differenzielle Sicherung auslösen.
    * Differenzielle Sicherungen können maximal 180 Tage aufbewahrt werden. Verwenden Sie für eine längere Aufbewahrung vollständige Sicherungen.

9. Wählen Sie **OK** aus, um die Richtlinie zu speichern und zum Hauptmenü **Sicherungsrichtlinie** zurückzukehren.

10. Um eine Richtlinie für eine Transaktionsprotokollsicherung hinzuzufügen, wählen Sie **Protokollsicherung** aus.
11. Wählen Sie in **Protokollsicherung** die Option **Aktivieren** aus, und legen Sie die Einstellungen für Häufigkeit und Aufbewahrung fest. Transaktionsprotokollsicherungen können alle 15 Minuten erfolgen und bis zu 35 Tage aufbewahrt werden.
12. Wählen Sie **OK** aus, um die Richtlinie zu speichern und zum Hauptmenü **Sicherungsrichtlinie** zurückzukehren.

    ![Bearbeiten der Richtlinie für die Transaktionsprotokollsicherung](./media/backup-azure-sql-database/log-backup-policy-editor.png)

13. Legen Sie im Menü **Sicherungsrichtlinie** fest, ob die **SQL-Sicherungskomprimierung** aktiviert wird. Diese Option ist standardmäßig deaktiviert. Wenn sie aktiviert ist, sendet SQL Server einen komprimierten Sicherungsdatenstrom an die VDI.  Beachten Sie, dass Azure Backup je nach Wert dieses Steuerelements die Standardwerte für Instanzebenen mit der Klausel „COMPRESSION / NO_COMPRESSION“ überschreibt.

14. Nachdem Sie die Sicherungsrichtlinie bearbeitet haben, wählen Sie **OK** aus.

> [!NOTE]
> Jede Protokollsicherung wird mit der vorherigen vollständigen Sicherung zu einer Wiederherstellungskette verkettet. Diese vollständige Sicherung wird aufbewahrt, bis die Aufbewahrungsdauer der letzten Protokollsicherung abgelaufen ist. Das kann bedeuten, dass die vollständige Sicherung für einen zusätzlichen Zeitraum aufbewahrt wird, um sicherzustellen, dass alle Protokolle wiederhergestellt werden können. Ein Beispiel: Ein Benutzer hat eine wöchentliche vollständige Sicherung, tägliche differenzielle Sicherungen und alle zwei Stunden ausgeführte Protokolle eingerichtet. Alle werden 30 Tage lang aufbewahrt. Die wöchentliche vollständige Sicherung kann aber erst vollständig bereinigt/gelöscht werden, wenn die nächste vollständige Sicherung verfügbar ist, also nach 30 + 7 Tagen. Angenommen, am 16. November erfolgt eine wöchentliche vollständige Sicherung. Gemäß der Aufbewahrungsrichtlinie wird diese bis zum 16. Dezember aufbewahrt. Die letzte Protokollsicherung hierfür erfolgt vor der nächsten geplanten vollständigen Sicherung, und zwar am 22. November. Solange dieses Protokoll bis zum 22. Dezember verfügbar ist, kann die vollständige Sicherung vom 16. November nicht gelöscht werden. Die vollständige Sicherung vom 16. November wird also bis zum 22. Dezember aufbewahrt.

## <a name="enable-auto-protection"></a>Aktivieren des automatischen Schutzes  

Sie können den automatischen Schutz aktivieren, um automatisch alle vorhandenen und künftigen Datenbanken in einer eigenständigen SQL Server-Instanz oder Always On-Verfügbarkeitsgruppe zu sichern.

* Die Anzahl der Datenbanken, die Sie für den automatischen Schutz in einer Aktion auswählen können, ist nicht begrenzt.
* Es ist nicht möglich, beim Aktivieren des automatischen Schutzes Datenbanken selektiv zu schützen oder vom Schutz in einer Instanz auszuschließen.
* Falls Ihre Instanz bereits einige geschützte Datenbanken enthält, sind sie unter den jeweiligen Richtlinien auch dann weiterhin geschützt, wenn Sie den automatischen Schutz deaktivieren. Für alle nicht geschützten Datenbanken, die später hinzugefügt werden, gilt aber nur eine einzelne Richtlinie, die Sie zum Zeitpunkt der Aktivierung des automatischen Schutzes unter **Sicherung konfigurieren** definieren. Sie können die Richtlinie, die einer automatisch geschützten Datenbank zugeordnet ist, später aber ändern.  

So aktivieren Sie den automatischen Schutz

  1. Wählen Sie in **Elemente für Sicherung** die Instanz aus, für die Sie den automatischen Schutz aktivieren möchten.
  2. Wählen Sie in der Dropdownliste unter **AUTOMATISCHER SCHUTZ** die Option **EIN** aus, und klicken Sie dann auf **OK**.

      ![Aktivieren des automatischen Schutzes für die Verfügbarkeitsgruppe](./media/backup-azure-sql-database/enable-auto-protection.png)

  3. Die Sicherung wird für alle Datenbanken gemeinsam konfiguriert und kann unter **Sicherungsaufträge** nachverfolgt werden.

Wenn Sie den automatischen Schutz deaktivieren müssen, klicken Sie unter **Sicherung konfigurieren** auf den Namen der Instanz, und wählen Sie dann **Automatischen Schutz deaktivieren** für die Instanz aus. Alle Datenbanken werden weiterhin gesichert, aber zukünftige Datenbanken sind nicht automatisch geschützt.

![Deaktivieren des automatischen Schutzes für diese Instanz](./media/backup-azure-sql-database/disable-auto-protection.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel werden folgende Themen erläutert:

* [Wiederherstellen von gesicherten SQL Server-Datenbanken](restore-sql-database-azure-vm.md)
* [Verwalten von gesicherten SQL Server-Datenbanken](manage-monitor-sql-database-backup.md)
