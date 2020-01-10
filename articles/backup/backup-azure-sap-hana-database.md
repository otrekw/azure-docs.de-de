---
title: Sichern einer SAP HANA-Datenbank mit Azure Backup in Azure
description: In diesem Artikel erfahren Sie, wie Sie eine SAP HANA-Datenbanken mit dem Azure Backup-Dienst auf virtuellen Azure-Computern sichern können.
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: 3246f6cf8046e0a0c5795059ad3448b70130e7e1
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/26/2019
ms.locfileid: "75496961"
---
# <a name="back-up-sap-hana-databases-in-azure-vms"></a>Sichern von SAP HANA-Datenbanken auf virtuellen Azure-Computern

SAP HANA-Datenbanken sind kritische Workloads, die eine niedrige Recovery Point Objective (RPO) und lange Aufbewahrungszeit erfordern. Sie können auf virtuellen Azure-Computern (VMs) ausgeführte SAP HANA-Datenbanken mithilfe von [Azure Backup](backup-overview.md) sichern.

In diesem Artikel wird veranschaulicht, wie SAP HANA-Datenbanken, die auf virtuellen Azure-Computern ausgeführt werden, in einem Azure Backup Recovery Services-Tresor gesichert werden.

In diesem Artikel wird Folgendes behandelt:
> [!div class="checklist"]
>
> * Erstellen und Konfigurieren eines Tresors
> * Ermitteln von Datenbanken
> * Konfigurieren von Sicherungen
> * Ausführen eines bedarfsgesteuerten Sicherungsauftrag

## <a name="prerequisites"></a>Voraussetzungen

Informationen zum Einrichten der Datenbank für die Sicherung finden Sie in den Abschnitten zu den [Voraussetzungen](tutorial-backup-sap-hana-db.md#prerequisites) und zum [Einrichten von Berechtigungen](tutorial-backup-sap-hana-db.md#setting-up-permissions).

### <a name="set-up-network-connectivity"></a>Einrichten der Netzwerkkonnektivität

Der virtuelle SAP HANA-Computer benötigt für alle Vorgänge eine Verbindung, um auf die öffentlichen Azure-IP-Adressen zugreifen zu können. VM-Vorgänge (Ermitteln von Datenbanken, Konfigurieren von Sicherungen, Planen von Sicherungen, Zurücksetzen von Wiederherstellungspunkten usw.) können ohne Konnektivität nicht ausgeführt werden. Richten Sie Konnektivität ein, indem Sie Zugriff auf die IP-Adressbereiche für Azure-Rechenzentren gewähren:

* Sie können die [IP-Adressbereiche](https://www.microsoft.com/download/details.aspx?id=41653) für Azure-Rechenzentren herunterladen und dann Zugriff auf diese IP-Adressen gewähren.
* Wenn Sie Netzwerksicherheitsgruppen (Network Security Groups, NSGs) verwenden, können Sie mit dem [Diensttag](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) „AzureCloud“ alle öffentlichen Azure-IP-Adressen zulassen. Mit dem Cmdlet [Set-AzureNetworkSecurityRule](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurenetworksecurityrule?view=azuresmps-4.0.0) können Sie NSG-Regeln ändern.
* Port 443 sollte der Zulassungsliste hinzugefügt werden, da der Transport über HTTPS erfolgt.

## <a name="onboard-to-the-public-preview"></a>Onboarding in die öffentliche Vorschau

Führen Sie das Onboarding in die öffentliche Vorschau wie folgt durch:

* Registrieren Sie im Portal Ihre Abonnement-ID bei dem Recovery Services-Dienstanbieter [gemäß der Beschreibung in diesem Artikel](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal).
* Führen Sie für das Modul „Az“ in PowerShell das folgende Cmdlet aus. Der Vorgang sollte mit „Registered“ (Registriert) abgeschlossen werden.

    ```powershell
    Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
    ```
* Wenn Sie das Modul „AzureRM“ in PowerShell verwenden, führen Sie das folgende Cmdlet aus. Der Vorgang sollte mit „Registered“ (Registriert) abgeschlossen werden.

    ```powershell
    Register-AzureRmProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
    ```
    

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-the-databases"></a>Ermitteln der Datenbanken

1. Klicken Sie im Tresor unter **Erste Schritte** auf **Sicherung**. Wählen Sie unter **Wo wird Ihre Workload ausgeführt?** den Eintrag **SAP HANA in Azure-VM** aus.
2. Klicken Sie auf **Ermittlung starten**. Dadurch wird die Ermittlung nicht geschützter Linux-VMs in der Tresorregion initiiert.

   * Nach der Ermittlung werden nicht geschützte VMs im Portal nach Name und Ressourcengruppe sortiert angezeigt.
   * Wenn ein virtueller Computer nicht wie erwartet aufgeführt wird, überprüfen Sie, ob er bereits in einem Tresor gesichert wird.
   * Mehrere virtuelle Computer können den gleichen Namen aufweisen, gehören dann aber verschiedenen Ressourcengruppen an.

3. Klicken Sie im Bereich **Virtuelle Computer auswählen** auf den Link zum Herunterladen des Skripts, das dem Azure Backup-Dienst die Berechtigungen zum Zugreifen auf die SAP HANA-VMs für die Ermittlung von Datenbanken erteilt.
4. Führen Sie das Skript auf jedem virtuellen Computer aus, der die zu sichernden SAP HANA-Datenbanken hostet.
5. Wählen Sie nach dem Ausführen des Skripts auf den VMs im Bereich **Virtuelle Computer auswählen** die VMs aus. Klicken Sie dann auf **DBs ermitteln**.
6. Azure Backup ermittelt alle SAP HANA-Datenbanken auf dem virtuellen Computer. Während der Ermittlung registriert Azure Backup den virtuellen Computer beim Tresor und installiert eine Erweiterung auf dem VM. Für die Datenbank wird kein Agent installiert.

    ![Ermitteln von SAP HANA-Datenbanken](./media/backup-azure-sap-hana-database/hana-discover.png)

## <a name="configure-backup"></a>Konfigurieren der Sicherung  

Aktivieren Sie jetzt die Sicherung.

1. Klicken Sie unter „Schritt 2“ auf **Sicherung konfigurieren**.

    ![Konfiguration der Sicherung](./media/backup-azure-sap-hana-database/configure-backup.png)
2. Wählen Sie unter **Zu sichernde Elemente auswählen** alle Datenbanken aus, die Sie schützen möchten, und wählen Sie dann **OK** aus.

    ![Auswählen von Elementen für die Sicherung](./media/backup-azure-sap-hana-database/select-items.png)
3. Wählen Sie **Sicherungsrichtlinie** > **Sicherungsrichtlinie auswählen** aus, und erstellen Sie eine neue Sicherungsrichtlinie für die Datenbanken gemäß den unten stehenden Anweisungen.

    ![Auswählen der Sicherungsrichtlinie](./media/backup-azure-sap-hana-database/backup-policy.png)
4. Klicken Sie nach dem Erstellen der Richtlinie im Menü **Sicherung** auf **Sicherung aktivieren**.

    ![Aktivieren der Sicherung](./media/backup-azure-sap-hana-database/enable-backup.png)
5. Verfolgen Sie den Fortschritt der Sicherungskonfiguration im Bereich **Benachrichtigungen** des Portals.

### <a name="create-a-backup-policy"></a>Erstellen einer Sicherungsrichtlinie

Eine Sicherungsrichtlinie legt fest, wann Sicherungen erstellt und wie lange sie aufbewahrt werden.

* Eine Richtlinie wird auf Tresorebene erstellt.
* Mehrere Tresore können die gleiche Sicherungsrichtlinie verwenden. Allerdings müssen Sie die Sicherungsrichtlinie auf jeden Tresor anwenden.

Legen Sie die Richtlinieneinstellungen wie folgt fest:

1. Geben Sie unter **Richtlinienname** einen Namen für die neue Richtlinie ein.

   ![Eingeben des Richtliniennamens](./media/backup-azure-sap-hana-database/policy-name.png)
2. Wählen Sie in **Richtlinie für vollständige Sicherung** für **Sicherungshäufigkeit** **Täglich** oder **Wöchentlich** aus.
   * **Daily** (Täglich): Wählen Sie die Uhrzeit und die Zeitzone für den Beginn des Sicherungsauftrags aus.
       * Sie müssen eine vollständige Sicherung ausführen. Diese Option kann nicht deaktiviert werden.
       * Klicken Sie auf **Vollständige Sicherung**, um die Richtlinie anzuzeigen.
       * Sie können keine differenziellen Sicherungen für tägliche vollständige Sicherungen erstellen.
   * **Wöchentlich**: Wählen Sie den Wochentag, die Uhrzeit und die Zeitzone für die Ausführung des Sicherungsauftrags aus.

   ![Auswählen der Sicherungshäufigkeit](./media/backup-azure-sap-hana-database/backup-frequency.png)

3. Konfigurieren Sie unter **Beibehaltungsdauer** die Aufbewahrungseinstellungen für die vollständige Sicherung.
    * Standardmäßig sind alle Optionen ausgewählt. Deaktivieren Sie alle Optionen für die Beibehaltungsdauer, die Sie nicht verwenden möchten, und legen Sie die gewünschten Grenzwerte fest.
    * Die Mindestaufbewahrungsdauer beträgt für alle Sicherungstypen (vollständig/differenziell/Protokoll) sieben Tage.
    * Wiederherstellungspunkte werden unter Berücksichtigung ihrer Beibehaltungsdauer mit einer Markierung versehen. Wenn Sie beispielsweise eine tägliche vollständige Sicherung wählen, wird pro Tag nur eine vollständige Sicherung ausgelöst.
    * Die Sicherung für einen bestimmten Tag wird auf Grundlage der wöchentlichen Beibehaltungsdauer und der entsprechenden Einstellung markiert und beibehalten.
    * Mit der monatlichen und jährlichen Beibehaltungsdauer verhält es sich ähnlich.

4. Klicken Sie im Menü **Vollständige Sicherung** auf **OK**, um die Einstellungen zu übernehmen.
5. Wählen Sie **Differenzielle Sicherung** aus, um eine Richtlinie für eine differenzielle Sicherung hinzuzufügen.
6. Wählen Sie in **Richtlinie für differenzielle Sicherung** die Option **Aktivieren** aus, um die Einstellungen für Häufigkeit und Beibehaltung vorzunehmen.
    * Pro Tag kann höchstens eine differenzielle Sicherung ausgelöst werden.
    * Differenzielle Sicherungen können maximal 180 Tage aufbewahrt werden. Wenn Sie eine längere Aufbewahrung wünschen, müssen Sie vollständige Sicherungen verwenden.

    ![Richtlinie für differenzielle Sicherung](./media/backup-azure-sap-hana-database/differential-backup-policy.png)

    > [!NOTE]
    > Inkrementelle Sicherungen werden derzeit nicht unterstützt.

7. Klicken Sie auf **OK**, um die Richtlinie zu speichern und zum Hauptmenü **Sicherungsrichtlinie** zurückzukehren.
8. Wählen Sie **Protokollsicherung** aus, um eine Richtlinie für eine Transaktionsprotokollsicherung hinzuzufügen.
    * Wählen Sie unter **Protokollsicherung** die Option **Aktivieren** aus.  Diese Option kann nicht deaktiviert werden, da SAP HANA alle Protokollsicherungen verwaltet.
    * Legen Sie die Häufigkeits- und Aufbewahrungssteuerelemente fest.

    > [!NOTE]
    > Protokollsicherungen werden erst nach dem erfolgreichen Abschluss einer vollständigen Sicherung übertragen.

9. Klicken Sie auf **OK**, um die Richtlinie zu speichern und zum Hauptmenü **Sicherungsrichtlinie** zurückzukehren.
10. Klicken Sie nach dem Definieren der Sicherungsrichtlinie auf **OK**.

> [!NOTE]
> Jede Protokollsicherung wird mit der vorherigen vollständigen Sicherung zu einer Wiederherstellungskette verkettet. Diese vollständige Sicherung wird aufbewahrt, bis die Aufbewahrungsdauer der letzten Protokollsicherung abgelaufen ist. Das kann bedeuten, dass die vollständige Sicherung für einen zusätzlichen Zeitraum aufbewahrt wird, um sicherzustellen, dass alle Protokolle wiederhergestellt werden können. Angenommen, der Benutzer verfügt über eine wöchentliche vollständige Sicherung, tägliche differenzielle Sicherungen und zweistündige Protokolle. Alle werden 30 Tage lang aufbewahrt. Allerdings kann die wöchentliche vollständige Sicherung erst vollständig bereinigt/gelöscht werden, wenn die nächste vollständige Sicherung verfügbar ist, also nach 30 + 7 Tagen. Angenommen, eine wöchentliche vollständige Sicherung erfolgt am 16. November. Gemäß der Aufbewahrungsrichtlinie sollte diese bis zum 16. Dezember aufbewahrt werden. Die letzte Protokollsicherung hierfür erfolgt vor der nächsten geplanten vollständigen Sicherung, und zwar am 22. November. Solange dieses Protokoll bis zum 22. Dezember verfügbar ist, kann die vollständige Sicherung vom 16. November nicht gelöscht werden. Die vollständige Sicherung vom 16. November wird also bis zum 22. Dezember aufbewahrt.

## <a name="run-an-on-demand-backup"></a>Ausführen einer On-Demand-Sicherung

Sicherungen werden gemäß dem Richtlinienzeitplan ausgeführt. Eine bedarfsgesteuerte Sicherung können Sie wie folgt ausführen:

1. Klicken Sie im Tresormenü auf **Sicherungselemente**.
2. Wählen Sie unter **Sicherungselemente** den virtuellen Computer aus, auf dem die SAP HANA-Datenbank ausgeführt wird, und klicken Sie dann auf **Jetzt sichern**.
3. Verwenden Sie unter **Jetzt sichern** den Kalender, um den letzten Tag zur Beibehaltung des Wiederherstellungspunkts auszuwählen. Klicken Sie dann auf **OK**.
4. Überwachen Sie die Portalbenachrichtigungen. Sie können den Auftragsstatus im Dashboard des Tresors unter **Sicherungsaufträge** > **In Bearbeitung** überwachen. Je nach Größe Ihrer Datenbank kann das Erstellen der ersten Sicherung einige Zeit dauern.

## <a name="run-sap-hana-studio-backup-on-a-database-with-azure-backup-enabled"></a>Ausführen einer SAP HANA Studio-Sicherung einer Datenbank, für die Azure Backup aktiviert ist

Führen Sie die folgenden Schritte aus, wenn Sie (mit HANA Studio) eine lokale Sicherung einer Datenbank ausführen möchten, die mit Azure Backup gesichert wird:

1. Warten Sie, bis alle vollständigen Sicherungen oder Protokollsicherungen für die Datenbank abgeschlossen sind. Überprüfen Sie den Status in SAP HANA Studio/Cockpit.
2. Deaktivieren Sie Protokollsicherungen, und legen Sie den Sicherungskatalog auf das Dateisystem für die entsprechende Datenbank fest.
3. Doppelklicken Sie hierzu auf **systemdb** > **Konfiguration** > **Datenbank auswählen** > **Filter (Protokoll)** .
4. Legen Sie **enable_auto_log_backup** auf **No** fest.
5. Legen Sie **log_backup_using_backint** auf **False** fest.
6. Erstellen Sie eine bedarfsgesteuerte vollständige Sicherung der Datenbank.
7. Warten Sie, bis die vollständige Sicherung und die Katalogsicherung abgeschlossen sind.
8. Stellen Sie die vorherigen Einstellungen für Azure wieder her:
    * Legen Sie **enable_auto_log_backup** auf **Yes** fest.
    * Legen Sie **log_backup_using_backint** auf **True** fest.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [nur auf virtuellen Azure-Computern ausgeführte SAP HANA-Datenbanken wiederherstellen können](https://docs.microsoft.com/azure/backup/sap-hana-db-restore).
* Erfahren Sie, wie Sie [mit Azure Backup gesicherte SAP HANA-Datenbanken verwalten können](https://docs.microsoft.com/azure/backup/sap-hana-db-manage).
