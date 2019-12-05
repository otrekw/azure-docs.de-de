---
title: 'Tutorial: Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern'
description: In diesem Tutorial wird beschrieben, wie Sie SAP HANA-Datenbanken, die auf einem virtuellen Azure-Computer ausgeführt werden, in einem Azure Backup Recovery Services-Tresor sichern.
ms.topic: tutorial
ms.date: 11/12/2019
ms.openlocfilehash: a622370fca3144aeb6a5d7c071c227b3c21cf135
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2019
ms.locfileid: "74287187"
---
# <a name="tutorial-back-up-sap-hana-databases-in-an-azure-vm"></a>Tutorial: Sichern von SAP HANA-Datenbanken auf einem virtuellen Azure-Computer

In diesem Tutorial wird veranschaulicht, wie Sie SAP HANA-Datenbanken, die auf virtuellen Azure-Computern ausgeführt werden, in einem Azure Backup Recovery Services-Tresor sichern. In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Erstellen und Konfigurieren eines Tresors
> * Ermitteln von Datenbanken
> * Konfigurieren von Sicherungen

[Hier](sap-hana-backup-support-matrix.md#scenario-support) finden Sie alle Szenarien, die von uns derzeit unterstützt werden.

## <a name="onboard-to-the-public-preview"></a>Onboarding in die öffentliche Vorschau

Führen Sie das Onboarding in die öffentliche Vorschau wie folgt durch:

* Registrieren Sie im Portal Ihre Abonnement-ID bei dem Recovery Services-Dienstanbieter [gemäß der Beschreibung in diesem Artikel](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal).

* Führen Sie bei Verwendung von PowerShell das folgende Cmdlet aus. Der Vorgang sollte mit „Registered“ (Registriert) abgeschlossen werden.

    ```powershell
    Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie vor dem Konfigurieren von Sicherungen unbedingt die folgenden Schritte aus:

1. Installieren und aktivieren Sie auf dem virtuellen Computer, auf dem die SAP HANA-Datenbank ausgeführt wird, die im offiziellen SLES-Paket/auf den offiziellen SLES-Medien enthaltenen ODBC-Treiberpakete mithilfe von zypper. Gehen Sie dabei wie folgt vor:

```bash
sudo zypper update
sudo zypper install unixODBC
```

>[!NOTE]
> Wenn Sie die Repositorys nicht aktualisieren, sollten Sie sicherstellen, dass mindestens Version 2.3.4 von unixODBC verwendet wird. Die Version von unixODBC zu ermitteln, führen Sie `odbcinst -j` als Stamm aus.
>

2. Lassen Sie eine Verbindung des virtuellen Computers mit dem Internet zu, damit er Azure erreichen kann. Dies ist [weiter unten](#set-up-network-connectivity) beschrieben.

3. Führen Sie das Vorregistrierungsskript auf dem virtuellen Computer aus, auf dem HANA als Root-Benutzer installiert ist. [Mit diesem Skript](https://aka.ms/scriptforpermsonhana) werden die [richtigen Berechtigungen](#setting-up-permissions) festgelegt.

## <a name="set-up-network-connectivity"></a>Einrichten der Netzwerkkonnektivität

Der virtuelle SAP HANA-Computer benötigt für alle Vorgänge eine Verbindung, um auf die öffentlichen Azure-IP-Adressen zugreifen zu können. VM-Vorgänge (Ermitteln von Datenbanken, Konfigurieren von Sicherungen, Planen von Sicherungen, Zurücksetzen von Wiederherstellungspunkten usw.) können ohne Konnektivität nicht ausgeführt werden. Richten Sie Konnektivität ein, indem Sie Zugriff auf die IP-Adressbereiche für Azure-Rechenzentren gewähren:

* Sie können die [IP-Adressbereiche](https://www.microsoft.com/download/details.aspx?id=41653) für Azure-Rechenzentren herunterladen und dann Zugriff auf diese IP-Adressen gewähren.
* Wenn Sie Netzwerksicherheitsgruppen (Network Security Groups, NSGs) verwenden, können Sie mit dem [Diensttag](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) „AzureCloud“ alle öffentlichen Azure-IP-Adressen zulassen. Mit dem Cmdlet [Set-AzureNetworkSecurityRule](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurenetworksecurityrule?view=azuresmps-4.0.0) können Sie NSG-Regeln ändern.
* Port 443 sollte der Zulassungsliste hinzugefügt werden, da der Transport per HTTPS erfolgt.

## <a name="setting-up-permissions"></a>Einrichten von Berechtigungen

Mit dem Skript für die Vorregistrierung werden die folgenden Aktionen durchgeführt:

1. Erstellen von AZUREWLBACKUPHANAUSER im HANA-System und Hinzufügen der erforderlichen Rollen und Berechtigungen:
   * DATABASE ADMIN: Erstellen neuer Datenbanken während der Wiederherstellung
   * CATALOG READ: Lesen des Sicherungskatalogs
   * SAP_INTERNAL_HANA_SUPPORT: Zugreifen auf einige private Tabellen
2. Hinzufügen eines Schlüssels zum Hdbuserstore für das HANA-Plug-In, um alle Vorgänge (Datenbankabfragen, Wiederherstellungsvorgänge, Konfigurieren und Ausführen von Sicherungen) zu behandeln.

Um die Schlüsselerstellung zu bestätigen, führen Sie auf dem HANA-Computer den Befehl HDBSQL mit SIDADM-Anmeldeinformationen aus:

```hdbsql
hdbuserstore list
```

Die Ausgabe des Befehls sollte den Schlüssel {SID} {DBNAME} und den Benutzer AZUREWLBACKUPHANAUSER enthalten.

>[!NOTE]
> Stellen Sie sicher, dass sich unter „/usr/sap/{SID}/home/.hdb/“ eine eindeutige Gruppe von SSFS-Dateien befindet. In diesem Pfad darf nur ein Ordner vorhanden sein.
>

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
   * Die Mindestbeibehaltungsdauer beträgt für alle Sicherungstypen (vollständig/differenziell/Protokoll) sieben Tage.
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
   * **Protokollsicherung** ist standardmäßig auf **Aktivieren** festgelegt. Diese Option kann nicht deaktiviert werden, da SAP HANA alle Protokollsicherungen verwaltet.
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
* Informieren Sie sich über das [Wiederherstellen von SAP HANA-Datenbanken, die auf Azure-VMs ausgeführt werden](sap-hana-db-restore.md).
* Informieren Sie sich über das [Verwalten von SAP HANA-Datenbanken, die mit Azure Backup gesichert werden](sap-hana-db-manage.md).
* Informieren Sie sich über das [Behandeln von Problemen beim Sichern von SAP HANA-Datenbanken in Azure](backup-azure-sap-hana-database-troubleshoot.md).
