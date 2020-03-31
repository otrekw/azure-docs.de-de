---
title: 'Tutorial: Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern'
description: In diesem Tutorial wird beschrieben, wie Sie SAP HANA-Datenbanken, die auf einem virtuellen Azure-Computer ausgeführt werden, in einem Azure Backup Recovery Services-Tresor sichern.
ms.topic: tutorial
ms.date: 02/24/2020
ms.openlocfilehash: f64dd74ad0e038c5cad152e20ae2255de03114e3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "79501456"
---
# <a name="tutorial-back-up-sap-hana-databases-in-an-azure-vm"></a>Tutorial: Sichern von SAP HANA-Datenbanken auf einem virtuellen Azure-Computer

In diesem Tutorial wird veranschaulicht, wie Sie SAP HANA-Datenbanken, die auf virtuellen Azure-Computern ausgeführt werden, in einem Azure Backup Recovery Services-Tresor sichern. In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Erstellen und Konfigurieren eines Tresors
> * Ermitteln von Datenbanken
> * Konfigurieren von Sicherungen

[Hier](sap-hana-backup-support-matrix.md#scenario-support) finden Sie alle Szenarien, die von uns derzeit unterstützt werden.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie vor dem Konfigurieren von Sicherungen unbedingt die folgenden Schritte aus:

* Lassen Sie eine Verbindung des virtuellen Computers mit dem Internet zu, damit er Azure erreichen kann. Dies ist unten im Abschnitt [Einrichten der Netzwerkkonnektivität](#set-up-network-connectivity) beschrieben.
* In **hdbuserstore** sollte ein Schlüssel enthalten sein, der die folgenden Kriterien erfüllt:
  * Er sollte sich im Standardspeicher **hdbuserstore** befinden.
  * Für MDC sollte der Schlüssel auf den SQL-Port von **NAMESERVER** verweisen. Für SDC sollte er auf den SQL-Port von **INDEXSERVER** verweisen.
  * Es sollten Anmeldeinformationen zum Hinzufügen und Löschen von Benutzern vorhanden sein.
* Führen Sie das Skript für die SAP HANA-Sicherungskonfiguration (Vorregistrierungsskript) auf dem virtuellen Computer, auf dem HANA installiert ist, als Stammbenutzer aus. Mit [diesem Skript](https://aka.ms/scriptforpermsonhana) wird das HANA-System für die Sicherung vorbereitet. Weitere Informationen zum Vorregistrierungsskript finden Sie unter [Aufgaben des Vorregistrierungsskripts](#what-the-pre-registration-script-does).

## <a name="set-up-network-connectivity"></a>Einrichten der Netzwerkkonnektivität

Der virtuelle SAP HANA-Computer benötigt für alle Vorgänge eine Verbindung mit den öffentlichen Azure-IP-Adressen. VM-Vorgänge (Ermitteln von Datenbanken, Konfigurieren von Sicherungen, Planen von Sicherungen, Zurücksetzen von Wiederherstellungspunkten usw.) schlagen ohne Verbindung mit öffentlichen IP-Adressen von Azure fehl.

Stellen Sie die Verbindung mithilfe einer der folgenden Optionen her:

### <a name="allow-the-azure-datacenter-ip-ranges"></a>IP-Adressbereiche des Azure-Rechenzentrums zulassen

Diese Option lässt die [IP-Adressbereiche](https://www.microsoft.com/download/details.aspx?id=41653) in der heruntergeladenen Datei zu. Verwenden Sie für den Zugriff auf eine Netzwerksicherheitsgruppe (NSG) das Cmdlet Set-AzureNetworkSecurityRule. Wenn Ihre Liste sicherer Empfänger nur regionsspezifische IP-Adressen enthält, müssen Sie auch die Liste sicherer Empfänger im Diensttag „Azure Active Directory (Azure AD)“ aktualisieren, um die Authentifizierung zu ermöglichen.

### <a name="allow-access-using-nsg-tags"></a>Zugriff mithilfe von NSG-Tags zulassen

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

**Bereitstellen eines HTTP-Proxyservers für das Weiterleiten von Datenverkehr**. Wenn Sie eine SAP HANA-Datenbank auf einem virtuellen Azure-Computer sichern, verwendet die Sicherungserweiterung auf dem virtuellen Computer die HTTPS-APIs, um Verwaltungsbefehle an Azure Backup und Daten an Azure Storage zu senden. Die Sicherungserweiterung verwendet auch Azure AD zur Authentifizierung. Leiten Sie den Datenverkehr der Sicherungserweiterung für diese drei Dienste über den HTTP-Proxy weiter. Die Erweiterungen sind die einzigen Komponenten, die für den Zugriff auf das öffentliche Internet konfiguriert sind.

Die Konnektivitätsoptionen haben u.a. die folgenden Vor- und Nachteile:

**Option** | **Vorteile** | **Nachteile**
--- | --- | ---
Zulassen von IP-Adressbereichen | Keine zusätzlichen Kosten | Komplexe Verwaltung, da sich die IP-Adressbereiche im Laufe der Zeit ändern <br/><br/> Zugriff auf alle Azure-Dienste, nicht nur auf Azure Storage
Verwenden von NSG-Diensttags | Einfachere Verwaltung, da Bereichsänderungen automatisch zusammengeführt werden <br/><br/> Keine zusätzlichen Kosten <br/><br/> | Nur mit NSGs möglich <br/><br/> Zugriff auf den gesamten Dienst
Verwenden von FQDN-Tags von Azure Firewall | Einfachere Verwaltung, da die erforderlichen FQDNs automatisch verwaltet werden | Nur mit Azure Firewall möglich
Verwenden eines HTTP-Proxys | Feinsteuerung im Proxy über die Speicher-URLs zulässig <br/><br/> Zentraler Internetzugriffspunkt für VMs <br/><br/> Unterliegt keinen Azure-IP-Adressänderungen | Zusätzliche Kosten für das Ausführen einer VM mit der Proxysoftware

## <a name="what-the-pre-registration-script-does"></a>Aufgaben des Vorregistrierungsskripts

Beim Ausführen des Skripts für die Vorregistrierung wird Folgendes durchgeführt:

* Es werden alle Pakete installiert bzw. aktualisiert, die vom Azure Backup-Agent Ihrer Distribution benötigt werden.
* Es werden Konnektivitätsprüfungen in ausgehender Richtung mit Azure Backup-Servern und abhängigen Diensten wie Azure Active Directory und Azure Storage durchgeführt.
* Die Anmeldung bei Ihrem HANA-System erfolgt mit dem Benutzerschlüssel, der Teil der [Voraussetzungen](#prerequisites) ist. Der Benutzerschlüssel wird zum Erstellen eines Sicherungsbenutzers (AZUREWLBACKUPHANAUSER) im HANA-System verwendet und kann gelöscht werden, nachdem das Vorregistrierungsskript erfolgreich ausgeführt wurde.
* AZUREWLBACKUPHANAUSER werden die folgenden erforderlichen Rollen und Berechtigungen zugewiesen:
  * DATABASE ADMIN (DATENBANKADMINISTRATOR, im Fall von MDC) und BACKUP ADMIN (SICHERUNGSADMINISTRATOR, im Fall von SDC): zum Erstellen neuer Datenbanken während der Wiederherstellung.
  * CATALOG READ: Lesen des Sicherungskatalogs
  * SAP_INTERNAL_HANA_SUPPORT: Zugreifen auf einige private Tabellen
* Das Skript fügt einen Schlüssel zu **hdbuserstore** für AZUREWLBACKUPHANAUSER für das HANA-Sicherungs-Plug-In hinzu, um alle Vorgänge (Datenbankabfragen, Wiederherstellungsvorgänge, Konfigurieren und Ausführen von Sicherungen) zu behandeln.

>[!NOTE]
> Sie können den unter [Voraussetzungen](#prerequisites) aufgeführten Benutzerschlüssel explizit als Parameter an das Vorregistrierungsskript übergeben: `-sk SYSTEM_KEY_NAME, --system-key SYSTEM_KEY_NAME`. <br><br>
>Führen Sie den Befehl `bash msawb-plugin-config-com-sap-hana.sh --help` aus, um zu erfahren, welche anderen Parameter vom Skript akzeptiert werden.

Um die Schlüsselerstellung zu bestätigen, führen Sie auf dem HANA-Computer den Befehl HDBSQL mit SIDADM-Anmeldeinformationen aus:

```hdbsql
hdbuserstore list
```

Die Ausgabe des Befehls sollte den Schlüssel {SID} {DBNAME} und den Benutzer AZUREWLBACKUPHANAUSER enthalten.

>[!NOTE]
> Stellen Sie sicher, dass sich unter `/usr/sap/{SID}/home/.hdb/` eine eindeutige Gruppe von SSFS-Dateien befindet. In diesem Pfad darf nur ein Ordner vorhanden sein.

## <a name="create-a-recovery-service-vault"></a>Erstellen eines Recovery Services-Tresors

Bei einem Recovery Services-Tresor handelt es sich um eine Entität, in der alle im Laufe der Zeit erstellten Sicherungen und Wiederherstellungspunkte gespeichert werden. Der Recovery Services-Tresor enthält auch die Sicherungsrichtlinien, die den geschützten virtuellen Computern zugeordnet sind.

So erstellen Sie einen Recovery Services-Tresor

1. Melden Sie sich bei Ihrem Abonnement im [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie im Menü links die Option **Alle Dienste** aus.

   ![Auswählen von „Alle Dienste“](./media/tutorial-backup-sap-hana-db/all-services.png)

3. Geben Sie im Dialogfeld **Alle Dienste** **Recovery Services** ein. Die Liste der Ressourcen wird Ihrer Eingabe entsprechend gefiltert. Wählen Sie in der Liste der Ressourcen **Recovery Services-Tresore** aus.

   ![Auswählen von Recovery Services-Tresoren](./media/tutorial-backup-sap-hana-db/recovery-services-vaults.png)

4. Wählen Sie im Dashboard **Recovery Services-Tresore** die Option **Hinzufügen** aus.

   ![Hinzufügen des Recovery Services-Tresors](./media/tutorial-backup-sap-hana-db/add-vault.png)

   Das Dialogfeld **Recovery Services-Tresor** wird geöffnet. Geben Sie Werte für **Name/Abonnement/Ressourcengruppe** und **Speicherort** an.

   ![Erstellen eines Recovery Services-Tresors](./media/tutorial-backup-sap-hana-db/create-vault.png)

   * **Name**: Der Name wird zum Identifizieren des Recovery Services-Tresors verwendet und muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen ein, der mindestens zwei, aber nicht mehr als 50 Zeichen enthält. Der Name muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten. In diesem Tutorial haben wir den Namen **SAPHanaVault**verwendet.
   * **Abonnement**: Wählen Sie das zu verwendende Abonnement aus. Wenn Sie nur in einem Abonnement Mitglied sind, wird dessen Name angezeigt. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Geschäfts-, Schul- oder Unikonto mehreren Azure-Abonnements zugeordnet ist. Hier haben wir das Abonnement **SAP HANA Solution Lab** verwendet.
   * **Ressourcengruppe**: Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe. Hier haben wir **SAPHANADemo** verwendet.<br>
   Um eine Liste der verfügbaren Ressourcengruppen in Ihrem Abonnement anzuzeigen, wählen Sie **Vorhandene verwenden** und dann eine Ressource im Dropdownlistenfeld aus. Wählen Sie zum Erstellen einer neuen Ressourcengruppe **Neu erstellen** aus, und geben Sie den Namen ein. Umfassende Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
   * **Standort**: Wählen Sie die geografische Region für den Tresor aus. Die Region des Tresors muss mit der Region des virtuellen Computers übereinstimmen, auf dem SAP HANA ausgeführt wird. Wir haben **USA, Osten 2** verwendet.

5. Klicken Sie auf **Überprüfen + erstellen**.

   ![Auswählen von „Bewerten + erstellen“](./media/tutorial-backup-sap-hana-db/review-create.png)

Der Recovery Services-Tresor wird jetzt erstellt.

## <a name="discover-the-databases"></a>Ermitteln der Datenbanken

1. Klicken Sie im Tresor unter **Erste Schritte** auf **Sicherung**. Wählen Sie unter **Wo wird Ihre Workload ausgeführt?** den Eintrag **SAP HANA in Azure-VM** aus.
2. Klicken Sie auf **Ermittlung starten**. Dadurch wird die Ermittlung nicht geschützter Linux-VMs in der Tresorregion initiiert. Der zu schützende virtuelle Azure-Computer wird angezeigt.
3. Klicken Sie im Bereich **Virtuelle Computer auswählen** auf den Link zum Herunterladen des Skripts, das dem Azure Backup-Dienst die Berechtigungen zum Zugreifen auf die SAP HANA-VMs für die Ermittlung von Datenbanken erteilt.
4. Führen Sie das Skript auf jedem virtuellen Computer aus, auf dem die zu sichernden SAP HANA-Datenbanken gehostet werden.
5. Wählen Sie nach dem Ausführen des Skripts auf der VM im Bereich **Virtuelle Computer auswählen** die VM aus. Klicken Sie dann auf **DBs ermitteln**.
6. Azure Backup ermittelt alle SAP HANA-Datenbanken auf dem virtuellen Computer. Während der Ermittlung registriert Azure Backup den virtuellen Computer beim Tresor und installiert eine Erweiterung auf dem VM. Für die Datenbank wird kein Agent installiert.

   ![Ermitteln der Datenbanken](./media/tutorial-backup-sap-hana-db/database-discovery.png)

## <a name="configure-backup"></a>Konfigurieren der Sicherung

Nachdem die zu sichernden Datenbanken ermittelt wurden, aktivieren wir die Sicherung.

1. Klicken Sie auf **Sicherung konfigurieren**.

   ![Konfigurieren der Sicherung](./media/tutorial-backup-sap-hana-db/configure-backup.png)

2. Wählen Sie unter **Elemente für die Sicherung auswählen** mindestens eine Datenbank aus, die Sie schützen möchten, und klicken Sie anschließend auf **OK**.

   ![Auswählen von Elementen für die Sicherung](./media/tutorial-backup-sap-hana-db/select-items-to-backup.png)

3. Erstellen Sie unter **Sicherungsrichtlinie > Sicherungsrichtlinie auswählen** eine neue Sicherungsrichtlinie für die Datenbank(en), indem Sie die Anleitung im nächsten Abschnitt befolgen.

   ![Auswählen der Sicherungsrichtlinie](./media/tutorial-backup-sap-hana-db/backup-policy.png)

4. Klicken Sie nach dem Erstellen der Richtlinie im Menü **Sicherung** auf **Sicherung aktivieren**.

   ![Klicken auf „Sicherung aktivieren“](./media/tutorial-backup-sap-hana-db/enable-backup.png)

5. Verfolgen Sie den Fortschritt der Sicherungskonfiguration im Bereich **Benachrichtigungen** des Portals.

## <a name="creating-a-backup-policy"></a>Erstellen einer Sicherungsrichtlinie

Eine Sicherungsrichtlinie legt fest, wann Sicherungen erstellt und wie lange sie aufbewahrt werden.

* Eine Richtlinie wird auf Tresorebene erstellt.
* Mehrere Tresore können die gleiche Sicherungsrichtlinie verwenden. Allerdings müssen Sie die Sicherungsrichtlinie auf jeden Tresor anwenden.

Legen Sie die Richtlinieneinstellungen wie folgt fest:

1. Geben Sie unter **Richtlinienname** einen Namen für die neue Richtlinie ein. Geben Sie für diesen Fall **SAPHANA** ein.

   ![Eingeben eines Namens für die neue Richtlinie](./media/tutorial-backup-sap-hana-db/new-policy.png)

2. Wählen Sie in **Richtlinie für vollständige Sicherung** eine **Sicherungshäufigkeit** aus. Sie können **Täglich** oder **Wöchentlich** auswählen. Für dieses Tutorial verwenden wir für die Sicherung die Option **Täglich**.

   ![Auswählen einer Sicherungshäufigkeit](./media/tutorial-backup-sap-hana-db/backup-frequency.png)

3. Konfigurieren Sie unter **Beibehaltungsdauer** die Aufbewahrungseinstellungen für die vollständige Sicherung.
   * Standardmäßig sind alle Optionen ausgewählt. Deaktivieren Sie alle Optionen für die Beibehaltungsdauer, die Sie nicht verwenden möchten, und legen Sie die gewünschten Grenzwerte fest.
   * Die Mindestaufbewahrungsdauer beträgt für alle Sicherungstypen (vollständig/differenziell/Protokoll) sieben Tage.
   * Wiederherstellungspunkte werden unter Berücksichtigung ihrer Beibehaltungsdauer mit einer Markierung versehen. Wenn Sie beispielsweise eine tägliche vollständige Sicherung wählen, wird pro Tag nur eine vollständige Sicherung ausgelöst.
   * Die Sicherung für einen bestimmten Tag wird auf Grundlage der wöchentlichen Beibehaltungsdauer und der entsprechenden Einstellung markiert und beibehalten.
   * Mit der monatlichen und jährlichen Beibehaltungsdauer verhält es sich ähnlich.
4. Klicken Sie im Menü **Vollständige Sicherung** auf **OK**, um die Einstellungen zu übernehmen.
5. Wählen Sie anschließend **Differenzielle Sicherung** aus, um eine Richtlinie für eine differenzielle Sicherung hinzuzufügen.
6. Wählen Sie in **Richtlinie für differenzielle Sicherung** die Option **Aktivieren** aus, um die Einstellungen für Häufigkeit und Beibehaltung vorzunehmen. Wir haben eine differenzielle Sicherung mit den folgenden Angaben aktiviert: jeden **Sonntag** um **2:00 Uhr** mit einer Aufbewahrungsdauer von **30 Tagen**.

   ![Richtlinie für differenzielle Sicherung](./media/tutorial-backup-sap-hana-db/differential-backup-policy.png)

   >[!NOTE]
   >Inkrementelle Sicherungen werden derzeit nicht unterstützt.
   >

7. Klicken Sie auf **OK**, um die Richtlinie zu speichern und zum Hauptmenü **Sicherungsrichtlinie** zurückzukehren.
8. Wählen Sie **Protokollsicherung** aus, um eine Richtlinie für eine Transaktionsprotokollsicherung hinzuzufügen.
   * **Protokollsicherung** ist standardmäßig auf **Aktivieren** festgelegt. Diese Option kann nicht deaktiviert werden, da SAP HANA alle Protokollsicherungen verwaltet.
   * Wir haben **2 Stunden** als Sicherungszeitplan und **15 Tage** als Aufbewahrungszeitraum angegeben.

    ![Richtlinie für Protokollsicherung](./media/tutorial-backup-sap-hana-db/log-backup-policy.png)

   >[!NOTE]
   > Protokollsicherungen werden erst nach erfolgreichem Abschluss einer vollständigen Sicherung übertragen.
   >

9. Klicken Sie auf **OK**, um die Richtlinie zu speichern und zum Hauptmenü **Sicherungsrichtlinie** zurückzukehren.
10. Klicken Sie nach dem Definieren der Sicherungsrichtlinie auf **OK**.

Sie haben nun erfolgreich eine Sicherung für Ihre SAP HANA-Datenbank(en) konfiguriert.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das [Ausführen einer On-Demand-Sicherung für SAP HANA-Datenbanken auf Azure-VMs](backup-azure-sap-hana-database.md#run-an-on-demand-backup).
* Erfahren Sie, wie Sie [nur auf virtuellen Azure-Computern ausgeführte SAP HANA-Datenbanken wiederherstellen können](sap-hana-db-restore.md).
* [Verwalten und Überwachen gesicherter SAP HANA-Datenbanken](sap-hana-db-manage.md)
* [Behandeln von Problemen beim Sichern von SAP HANA-Datenbanken in Azure](backup-azure-sap-hana-database-troubleshoot.md)
