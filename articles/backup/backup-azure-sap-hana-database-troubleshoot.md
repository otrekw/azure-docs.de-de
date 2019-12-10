---
title: Problembehandlung bei Sicherungsfehlern in SAP HANA-Datenbanken
description: Beschreibt, wie häufige Fehler behoben werden, die auftreten können, wenn Sie SAP HANA-Datenbanken mithilfe von Azure Backup sichern.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: b4c39c631963a358dcdc9d1eafe954a85a9499ad
ms.sourcegitcommit: 428fded8754fa58f20908487a81e2f278f75b5d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2019
ms.locfileid: "74554858"
---
# <a name="troubleshoot-backup-of-sap-hana-databases-on-azure"></a>Behandeln von Problemen beim Sichern von SAP HANA-Datenbanken in Azure

Dieser Artikel enthält Informationen zur Problembehandlung beim Sichern von SAP HANA-Datenbanken in Azure Virtual Machines. Weitere Informationen zu den derzeit unterstützten SAP HANA-Sicherungsszenarien finden Sie unter [Unterstützung von Szenarien](sap-hana-backup-support-matrix.md#scenario-support).

## <a name="prerequisites-and-permissions"></a>Voraussetzungen und Berechtigungen

Sehen Sie vor dem Konfigurieren von Sicherungen die Abschnitte [Voraussetzungen](tutorial-backup-sap-hana-db.md#prerequisites) und [Einrichten von Berechtigungen](tutorial-backup-sap-hana-db.md#setting-up-permissions) ein.

## <a name="common-user-errors"></a>Häufige Benutzerfehler

###  <a name="usererrorinopeninghanaodbcconnection"></a>UserErrorInOpeningHanaOdbcConnection 

| Fehlermeldung      | Fehler beim Herstellen der Verbindung mit dem HANA-System                             |
| ------------------ | ------------------------------------------------------------ |
| Mögliche Ursachen    | Die SAP HANA-Instanz ist möglicherweise ausgefallen.<br/>Die erforderlichen Berechtigungen für die Interaktion von Azure Backup mit der Hana-Datenbank sind nicht festgelegt. |
| Empfohlene Maßnahme | Überprüfen Sie, ob die SAP HANA-Datenbank aktiv ist. Wenn die Datenbank ausgeführt wird, überprüfen Sie, ob alle erforderlichen Berechtigungen festgelegt sind. Wenn Berechtigungen nicht vorhanden sind, führen Sie das [Vorregistrierungsskript](https://aka.ms/scriptforpermsonhana) aus, um die fehlenden Berechtigungen hinzuzufügen. |

###  <a name="usererrorhanainstancenameinvalid"></a>UserErrorHanaInstanceNameInvalid 

| Fehlermeldung      | Die angegebene SAP HANA-Instanz ist entweder ungültig oder wurde nicht gefunden. |
| ------------------ | ------------------------------------------------------------ |
| Mögliche Ursachen    | Es können nicht mehrere SAP HANA-Instanzen auf einem einzelnen virtuellen Azure-Computer gesichert werden. |
| Empfohlene Maßnahme | Führen Sie das [Vorregistrierungsskript](https://aka.ms/scriptforpermsonhana) auf der SAP HANA-Instanz aus, die gesichert werden soll. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support. |

###  <a name="usererrorhanaunsupportedoperation"></a>UserErrorHanaUnsupportedOperation 

| Fehlermeldung      | Der angegebene SAP HANA-Vorgang wird nicht unterstützt.             |
| ------------------ | ------------------------------------------------------------ |
| Mögliche Ursachen    | Azure Backup für SAP HANA unterstützt keine inkrementellen Sicherungen und keine Aktionen, die für SAP HANA-native Clients (Studio/Cockpit/DBA Cockpit) ausgeführt werden. |
| Empfohlene Maßnahme | Weitere Informationen finden Sie [hier](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support). |

###  <a name="usererrorhanapodoesnotsupportbackuptype"></a>UserErrorHANAPODoesNotSupportBackupType 

| Fehlermeldung      | Diese SAP HANA-Datenbank unterstützt nicht den angeforderten Sicherungstyp. |
| ------------------ | ------------------------------------------------------------ |
| Mögliche Ursachen    | Azure Backup unterstützt keine inkrementellen Sicherungen und keine Sicherungen mit Momentaufnahmen. |
| Empfohlene Maßnahme | Weitere Informationen finden Sie [hier](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support). |

###  <a name="usererrorhanalsnvalidationfailure"></a>UserErrorHANALSNValidationFailure 

| Fehlermeldung      | Die Sicherungsprotokollkette ist unterbrochen.                                   |
| ------------------ | ------------------------------------------------------------ |
| Mögliche Ursachen    | Das Protokollsicherungsziel wurde möglicherweise von backint in das Dateisystem geändert, oder die ausführbare backint-Datei wurde geändert. |
| Empfohlene Maßnahme | Lösen Sie eine vollständige Sicherung aus, um das Problem zu beheben.                   |

###  <a name="usererrorincomaptiblesrctargetsystsemsforrestore"></a>UserErrorIncomaptibleSrcTargetSystsemsForRestore 

| Fehlermeldung      | Das Quell- und das Zielsystem für die Wiederherstellung sind nicht kompatibel.   |
| ------------------ | ------------------------------------------------------------ |
| Mögliche Ursachen    | Das Zielsystem für die Wiederherstellung ist nicht mit dem Quellsystem kompatibel. |
| Empfohlene Maßnahme | Informationen zu den derzeit unterstützten Wiederherstellungstypen finden Sie im SAP-Hinweis [1642148](https://launchpad.support.sap.com/#/notes/1642148). |

###  <a name="usererrorsdctomdcupgradedetected"></a>UserErrorSDCtoMDCUpgradeDetected 

| Fehlermeldung      | Upgrade von SDC auf MDC erkannt.                                  |
| ------------------ | ------------------------------------------------------------ |
| Mögliche Ursachen    | Es wurde ein Upgrade der SAP HANA-Instanz von SDC auf MDC ausgeführt. Sicherungen können nach dem Upgrade nicht mehr ausgeführt werden. |
| Empfohlene Maßnahme | Führen Sie die Schritte im Abschnitt [Upgrade von SAP HANA 1.0 auf 2.0](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot#upgrading-from-sap-hana-10-to-20) aus, um das Problem zu beheben. |

###  <a name="usererrorinvalidbackintconfiguration"></a>UserErrorInvalidBackintConfiguration 

| Fehlermeldung      | Ungültige backint-Konfiguration erkannt.                       |
| ------------------ | ------------------------------------------------------------ |
| Mögliche Ursachen    | Die backint-Parameter sind nicht ordnungsgemäß für Azure Backup angegeben. |
| Empfohlene Maßnahme | Überprüfen Sie, ob die folgenden backint-Parameter festgelegt sind:<br/>\* [catalog_backup_using_backint:true]<br/>\* [enable_accumulated_catalog_backup:false]<br/>\* [parallel_data_backup_backint_channels:1]<br/>\* [log_backup_timeout_s:900)]<br/>\* [backint_response_timeout:7200]<br/>Wenn in HOST backint-Parameter vorhanden sind, entfernen Sie sie. Wenn Parameter auf HOST-Ebene nicht vorhanden sind, aber auf Datenbankebene manuell geändert wurden, setzen Sie sie wie oben beschrieben auf die entsprechenden Werte zurück. Oder führen Sie im Azure-Portal [Schutz beenden und Sicherungsdaten beibehalten](https://docs.microsoft.com/azure/backup/sap-hana-db-manage#stop-protection-for-an-sap-hana-database) aus, und wählen Sie dann **Sicherung fortsetzen** aus. |

## <a name="restore-checks"></a>Wiederherstellungsprüfungen

### <a name="single-container-database-sdc-restore"></a>Wiederherstellung einer Datenbank mit einem einzelnen Container (SDC)

Achten Sie auf Eingaben während der Wiederherstellung einer Datenbank mit einem einzelnen Container (SDC) für HANA auf einen anderen SDC-Computer. Der Datenbankname sollte in Kleinbuchstaben angegeben werden, wobei „sdc“ in Klammern angefügt wird. Die HANA-Instanz wird in Großbuchstaben angezeigt.

Angenommen, eine SDC HANA-Instanz „H21“ wird gesichert. Auf der Seite mit den Sicherungselementen wird der Sicherungselementname **„h21(sdc)“** angezeigt. Wenn Sie versuchen, diese Datenbank auf einem anderen Ziel-SDC wiederherzustellen (z. B. „H11“), müssen Sie die folgenden Eingaben bereitstellen:

![Eingaben für die SDC-Wiederherstellung](media/backup-azure-sap-hana-database/hana-sdc-restore.png)

Beachten Sie folgende Punkte:

- Standardmäßig wird der Name der wiederhergestellten Datenbank mit dem Namen des Sicherungselements aufgefüllt, d.h. „h21(sdc)“.
- Wenn Sie als Ziel „H11“ auswählen, wird der Name der wiederhergestellten Datenbank NICHT automatisch geändert. **Er sollte in „h11(sdc)“ geändert werden**. Im Fall von SDC ist der Name der wiederhergestellten Datenbank die ID der Zielinstanz in Kleinbuchstaben, an die „sdc“ in Klammern angefügt wird.
- Da SDC nur über eine einzelne Datenbank verfügen kann, müssen Sie auch das Kontrollkästchen aktivieren, um das Überschreiben der vorhandenen Datenbankdaten mit den Daten des Wiederherstellungspunkts zuzulassen.
- Bei Linux muss die Groß-/Kleinschreibung beachtet werden. Sie müssen daher unbedingt die Groß-/Kleinschreibung beibehalten.

### <a name="multiple-container-database-mdc-restore"></a>Wiederherstellung einer Datenbank mit mehreren Containern (MDC)

In Datenbanken mit mehreren Containern (MDC) für HANA ist die Standardkonfiguration „SYSTEMDB + 1 oder mehr Mandantendatenbanken“. Das Wiederherstellen einer vollständigen SAP HANA-Instanz bedeutet, dass sowohl SYSTEMDB als auch die Mandantendatenbanken wiederhergestellt werden. Zuerst wird SYSTEMDB wiederhergestellt, und dann wird die Wiederherstellung für die Mandantendatenbank fortgesetzt. Systemdatenbank bedeutet im Wesentlichen, dass die Systeminformationen für das ausgewählte Ziel überschrieben werden. Bei dieser Wiederherstellung werden auch die BackInt-bezogenen Informationen in der Zielinstanz überschrieben. Daher muss das Vorregistrierungsskript erneut ausgeführt werden, nachdem die Systemdatenbank auf einer Zielinstanz wiederhergestellt wurde. Nur dann ist die nachfolgende Wiederherstellung der Mandantendatenbanken erfolgreich.

## <a name="upgrading-from-sap-hana-10-to-20"></a>Upgrade von SAP HANA 1.0 auf 2.0

Wenn Sie SAP HANA 1.0-Datenbanken schützen und ein Upgrade auf 2.0 durchführen möchten, führen Sie die unten beschriebenen Schritte aus:

- [Beenden Sie den Schutz](sap-hana-db-manage.md#stop-protection-for-an-sap-hana-database) mit Beibehaltung der Daten für die alte SDC-Datenbank.
- Führen Sie das [Vorregistrierungsskript](https://aka.ms/scriptforpermsonhana) mit den richtigen Details (SID und MDC) erneut aus.
- Registrieren Sie die Erweiterung erneut (Sicherung > Details anzeigen > relevante Azure-VM auswählen > erneut registrieren).
- Klicken Sie auf „Datenbanken neu ermitteln“ für denselben virtuellen Computer. Durch diese Aktion sollten die neuen Datenbanken in Schritt 2 mit den richtigen Details (Systemdatenbank und Mandantendatenbank, nicht SDC) angezeigt werden.
- Schützen Sie diese neuen Datenbanken.

## <a name="upgrading-without-an-sid-change"></a>Upgrade ohne SID-Änderung

Upgrades auf OS oder SAP HANA, die keine SID-Änderung bewirken, können wie unten beschrieben behandelt werden:

- [Beenden Sie den Schutz](sap-hana-db-manage.md#stop-protection-for-an-sap-hana-database) mit Beibehaltung der Daten für die Datenbank.
- Führen Sie das [Vorregistrierungsskript](https://aka.ms/scriptforpermsonhana) erneut aus.
- [Setzen Sie den Schutz](sap-hana-db-manage.md#resume-protection-for-an-sap-hana-database) für die Datenbank fort.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie die [häufig gestellten Fragen](https://docs.microsoft.com/azure/backup/sap-hana-faq-backup-azure-vm) zum Sichern von SAP HANA-Datenbanken auf Azure-VMs ein.
