---
title: Einrichten der Azure-VM-Notfallwiederherstellung mit Azure Site Recovery
description: Erfahren Sie, wie Sie die Notfallwiederherstellung für Azure-VMs in eine andere Region mit dem Dienst „Azure Site Recovery“ durchführen.
ms.topic: tutorial
ms.date: 1/24/2020
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 50bf1ec7f21ccbc3a3fa8feaea02e45bd08a158a
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87421415"
---
# <a name="set-up-disaster-recovery-for-azure-vms"></a>Einrichten der Notfallwiederherstellung für Azure-VMs

Der Dienst [Azure Site Recovery](site-recovery-overview.md) verwaltet und orchestriert die Replikation, das Failover und das Failback von lokalen Computern sowie Azure-VMs und unterstützt dadurch Ihre Strategie zur Notfallwiederherstellung.

In diesem Tutorial wird erläutert, wie Sie die Notfallwiederherstellung für Azure-VMs einrichten, indem Sie eine Replikation von einer Azure-Region in einer anderen ausführen. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Recovery Services-Tresors
> * Überprüfen der Zielressourceneinstellungen
> * Einrichten der ausgehenden Netzwerkkonnektivität für VMs
> * Aktivieren der Replikation für eine VM

> [!NOTE]
> Dieser Artikel enthält Anweisungen für die Bereitstellung der Notfallwiederherstellung mit den einfachsten Einstellungen. Wenn Sie sich über benutzerdefinierte Einstellungen informieren möchten, lesen Sie die Artikel im Abschnitt mit den [Anleitungen](azure-to-azure-how-to-enable-replication.md).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Überprüfen Sie [Architektur und Komponenten des Szenarios](./azure-to-azure-architecture.md).
- Sehen Sie sich die [Supportanforderungen](./azure-to-azure-support-matrix.md) an, bevor Sie beginnen.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

Erstellen Sie den Tresor in einer beliebigen Region außer der Quellregion.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Ressource erstellen** aus. Wählen Sie anschließend **IT- und Verwaltungstools** > **Backup und Site Recovery** aus.
1. Geben Sie unter **Name**einen Anzeigenamen an, mit dem der Tresor identifiziert wird. Wenn Sie mehrere Abonnements haben, wählen Sie das gewünschte aus.
1. Erstellen Sie eine Ressourcengruppe, oder wählen Sie eine vorhandene aus. Geben Sie eine Azure-Region an. Eine Liste mit den unterstützten Regionen finden Sie in den [Preisdetails zu Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/)unter „Geografische Verfügbarkeit“.
1. Wählen Sie **An Dashboard anheften** und dann **Erstellen** aus, um über das Dashboard auf den Tresor zuzugreifen.

   ![Neuer Tresor](./media/azure-to-azure-tutorial-enable-replication/new-vault-settings.png)

Der neue Tresor wird dem **Dashboard** unter **Alle Ressourcen** und der Hauptseite **Recovery Services-Tresore** hinzugefügt.

## <a name="verify-target-resource-settings"></a>Überprüfen der Zielressourceneinstellungen

Überprüfen Sie die Zielregion Ihres Azure-Abonnements.

- Vergewissern Sie sich, dass Ihr Azure-Abonnement das Erstellen von VMs in der Zielregion zulässt. Wenden Sie sich an den Support, um das erforderliche Kontingent zu aktivieren.
- Stellen Sie sicher, dass Ihr Abonnement über ausreichend Ressourcen verfügt, um die entsprechenden VM-Größen für Ihre Quell-VMs zu unterstützen. Site Recovery wählt dieselbe oder eine möglichst ähnliche Größe für die Ziel-VM aus.

## <a name="set-up-outbound-network-connectivity-for-vms"></a>Einrichten der ausgehenden Netzwerkkonnektivität für VMs

Damit Site Recovery erwartungsgemäß funktioniert, müssen Sie die ausgehende Netzwerkkonnektivität der VMs ändern, die Sie replizieren möchten.

> [!NOTE]
> Site Recovery unterstützt die Verwendung eines Authentifizierungsproxys zur Steuerung der Netzwerkkonnektivität nicht.

### <a name="outbound-connectivity-for-urls"></a>Ausgehende Konnektivität für URLs

Lassen Sie den Zugriff auf die folgenden URLs zu, wenn Sie einen URL-basierten Firewallproxy zum Steuern der ausgehenden Konnektivität verwenden:

| **Name**                  | **Kommerziell**                               | **Behörden**                                 | **Beschreibung** |
| ------------------------- | -------------------------------------------- | ---------------------------------------------- | ----------- |
| Storage                   | `*.blob.core.windows.net`                  | `*.blob.core.usgovcloudapi.net`              | Ermöglicht das Schreiben von Daten aus der VM in das Cachespeicherkonto in der Quellregion |
| Azure Active Directory    | `login.microsoftonline.com`                | `login.microsoftonline.us`                   | Stellt die Autorisierung und Authentifizierung für Site Recovery-Dienst-URLs bereit. |
| Replikation               | `*.hypervrecoverymanager.windowsazure.com` | `*.hypervrecoverymanager.windowsazure.com`   | Ermöglicht die Kommunikation der VM mit Site Recovery |
| Service Bus               | `*.servicebus.windows.net`                 | `*.servicebus.usgovcloudapi.net`             | Ermöglicht es der VM, die Site Recovery-Überwachung und -Diagnosedaten zu schreiben |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>Ausgehende Konnektivität für IP-Adressbereiche

Wenn Sie eine Netzwerksicherheitsgruppe (NSG) verwenden, erstellen Sie auf Diensttags basierende NSG-Regeln für den Zugriff auf Azure Storage, Azure Active Directory, den Site Recovery-Dienst und die Site Recovery-Überwachung. [Weitere Informationen](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags)

## <a name="verify-azure-vm-certificates"></a>Überprüfen der Azure-VM-Zertifikate

Überprüfen Sie, ob die VMs, die Sie replizieren möchten, über die aktuellen Stammzertifikate verfügen. Andernfalls kann die VM aufgrund von Sicherheitseinschränkungen nicht bei Site Recovery registriert werden.

- Installieren Sie alle aktuellen Windows-Updates auf Windows-VMs, damit alle vertrauenswürdigen Stammzertifikate auf den Computern vorhanden sind. Führen Sie in einer nicht verbundenen Umgebung den Standardprozess für Windows Update und Zertifikatupdates in Ihrer Organisation durch.
- Befolgen Sie bei Linux-VMs die Anleitung Ihres Linux-Händlers, um die aktuellen vertrauenswürdigen Stammzertifikate und die Zertifikatssperrliste auf der VM abzurufen.

## <a name="set-permissions-on-the-account"></a>Festlegen von Berechtigungen für das Konto

Azure Site Recovery bietet drei integrierte Rollen zum Steuern von Site Recovery-Verwaltungsvorgängen.

- **Site-Recovery Contributor**: Diese Rolle verfügt über alle erforderlichen Berechtigungen zum Verwalten von Azure Site Recovery-Vorgängen in einem Tresor von Recovery Services. Ein Benutzer mit dieser Rolle kann jedoch keinen Tresor von Recovery Services erstellen oder löschen oder anderen Benutzern Zugriffsrechte zuweisen. Diese Rolle eignet sich optimal für Administratoren der Notfallwiederherstellung, die die Notfallwiederherstellung für Anwendungen oder gesamte Unternehmen aktivieren und verwalten können.

- **Site Recovery-Operator**: Diese Rolle verfügt über Berechtigungen zum Ausführen und Verwalten von Failover- und Failbackvorgängen. Ein Benutzer, der über diese Rolle verfügt, kann die Replikation nicht aktivieren oder deaktivieren, keine Tresore erstellen oder löschen, keine neue Infrastruktur registrieren oder anderen Benutzern keine Zugriffsrechte zuweisen. Diese Rolle eignet sich optimal für einen Notfallwiederherstellungsexperten, der ein Failover für einen virtuellen Computer oder Anwendungen durchführen kann, wenn er dazu vom Besitzer der Anwendung oder IT-Administratoren angewiesen wird. Nachdem der Notfall behandelt wurde, kann der Notfallwiederherstellungsexperte die virtuellen Computer erneut schützen und ein Failback durchführen.

- **Site Recovery-Leser**: Diese Rolle verfügt über Berechtigungen zum Anzeigen aller Site Recovery-Verwaltungsvorgänge. Diese Rolle eignet sich optimal für eine IT-Überwachungsführungskraft, die den aktuellen Schutzstatus überwachen und Supporttickets ausstellen kann.

Erfahren Sie mehr über [integrierte Rollen in Azure](../role-based-access-control/built-in-roles.md).

## <a name="enable-replication-for-a-vm"></a>Aktivieren der Replikation für eine VM

In den folgenden Abschnitten wird beschrieben, wie Sie die Replikation aktivieren.

### <a name="select-the-source"></a>Auswählen der Quelle

Wählen Sie zum Starten der Replikationseinrichtung die Quelle aus, in der Ihre virtuellen Azure-Computer ausgeführt werden.

1. Navigieren Sie zu **Recovery Services-Tresore**, und wählen Sie den Tresornamen und dann **+Replizieren** aus.
1. Wählen Sie unter **Quelle** die Option **Azure** aus.
1. Wählen Sie unter **Quellstandort** die Azure-Quellregion aus, in der Ihre VMs derzeit ausgeführt werden.
1. Wählen Sie das **Quellabonnement** aus, in dem die virtuellen Computer ausgeführt werden. Dies kann ein beliebiges Abonnement im gleichen Azure Active Directory-Mandanten sein, in dem sich auch Ihr Recovery Services-Tresor befindet.
1. Wählen Sie die **Quellressourcengruppe** und dann **OK** aus, um die Einstellungen zu speichern.

   ![Quelle einrichten](./media/azure-to-azure-tutorial-enable-replication/source.png)

### <a name="select-the-vms"></a>Auswählen der virtuellen Computer

Site Recovery ruft eine Liste der virtuellen Computer ab, die dem Abonnement und der Ressourcengruppe bzw. dem Clouddienst zugeordnet sind.

1. Wählen Sie unter **Virtuelle Computer** die VMs aus, die Sie replizieren möchten.
1. Klicken Sie auf **OK**.

### <a name="configure-replication-settings"></a>Konfigurieren der Replikationseinstellungen

Site Recovery erstellt Standardeinstellungen und Replikationsrichtlinien für die Zielregion. Sie können diese Einstellungen nach Bedarf ändern.

1. Wählen Sie **Einstellungen** aus, um die Ziel- und Replikationseinstellungen anzuzeigen.

1. Um die Standardzieleinstellungen zu überschreiben, wählen Sie neben **Ressourcengruppe, Netzwerk, Speicher und Verfügbarkeitsgruppen** die Option **Anpassen** aus.

   ![Konfigurieren von Einstellungen](./media/azure-to-azure-tutorial-enable-replication/settings.png)

1. Ändern Sie die Zieleinstellungen wie in der Tabelle zusammengefasst.

   | **Einstellung** | **Details** |
   | --- | --- |
   | **Zielabonnement** | Zielabonnement und Quellabonnement sind standardmäßig identisch. Wählen Sie **Anpassen** aus, um ein anderes Zielabonnement im gleichen Azure Active Directory-Mandanten auszuwählen. |
   | **Zielstandort** | Die Zielregion, die zur Notfallwiederherstellung verwendet wird.<br/><br/> Der Zielspeicherort sollte mit dem Speicherort des Site Recovery-Tresors übereinstimmen. |
   | **Zielressourcengruppe** | Die Ressourcengruppe in der Zielregion, zu der Azure-VMs nach einem Failover gehören.<br/><br/> Site Recovery erstellt standardmäßig in der Zielregion eine neue Ressourcengruppe mit dem Suffix `asr`. Der Speicherort der Zielressourcengruppe kann eine beliebige Region sein, mit Ausnahme der Region, in der Ihre virtuellen Quellcomputer gehostet werden. |
   | **Virtuelles Zielnetzwerk** | Das Netzwerk in der Zielregion, in dem sich Azure-VMs nach einem Failover befinden.<br/><br/> Site Recovery erstellt standardmäßig in der Zielregion ein neues virtuelles Netzwerk (und Subnetze) mit dem Suffix `asr`. |
   | **Cachespeicherkonten** | Site Recovery verwendet ein Speicherkonto in der Quellregion. Änderungen an Quell-VMs werden vor der Replikation am Zielspeicherort an dieses Konto gesendet.<br/><br/> Wenn Sie ein Cachespeicherkonto mit aktivierter Firewall verwenden, müssen Sie **vertrauenswürdige Microsoft-Dienste zulassen**, indem Sie die entsprechende Option auswählen. [Weitere Informationen](../storage/common/storage-network-security.md#exceptions) Stellen Sie außerdem sicher, dass Sie den Zugriff auf mindestens ein Subnetz des Quell-VNET zulassen. |
   | **Zielspeicherkonten (wenn die Quell-VM keine verwalteten Datenträger verwendet)** | Standardmäßig erstellt Site Recovery ein neues Speicherkonto in der Zielregion, um das Quell-VM-Speicherkonto zu spiegeln.<br/><br/> Aktivieren Sie die Option **Vertrauenswürdige Microsoft-Dienste zulassen**, wenn Sie ein Cachespeicherkonto mit aktivierter Firewall verwenden. |
   | **Verwaltete Replikatdatenträger (wenn die Quell-VM verwaltete Datenträger verwendet)** | Site Recovery erstellt standardmäßig verwaltete Replikatdatenträger in der Zielregion, um die verwalteten Datenträger der Quell-VM zu spiegeln. Dabei wird der gleiche Speichertyp (Standard oder Premium) wie für die verwalteten Datenträger der Quell-VM verwendet. Sie können nur den Datenträgertyp anpassen. |
   | **Zielverfügbarkeitsgruppen** | Standardmäßig erstellt Azure Site Recovery in der Zielregion eine neue Verfügbarkeitsgruppe und verwendet das Namenssuffix `asr` für die virtuellen Computer, die zu einer Verfügbarkeitsgruppe in der Quellregion gehören. Falls bereits eine von Azure Site Recovery erstellte Verfügbarkeitsgruppe vorhanden ist, wird diese wiederverwendet. |
   | **Zielverfügbarkeitszonen** | Site Recovery weist in der Zielregion standardmäßig die gleiche Anzahl von Zonen zu wie in der Quellregion, sofern die Zielregion Verfügbarkeitszonen unterstützt.<br/><br/> Wenn die Zielregion keine Verfügbarkeitszonen unterstützt, werden die virtuellen Zielcomputer standardmäßig als einzelne Instanzen konfiguriert.<br/><br/> Wählen Sie **Anpassen** aus, um virtuelle Computer als Teil einer Verfügbarkeitsgruppe in der Zielregion zu konfigurieren.<br/><br/> Nach der Aktivierung der Replikation können Sie den Verfügbarkeitstyp (einzelne Instanz, Verfügbarkeitsgruppe oder Verfügbarkeitszone) nicht mehr ändern. Wenn Sie den Verfügbarkeitstyp ändern möchten, müssen Sie die Replikation deaktivieren und wieder aktivieren. |

1. Um die Einstellungen für die Replikationsrichtlinie anzupassen, wählen Sie neben **Replikationsrichtlinie** die Option **Anpassen** aus, und ändern Sie die Einstellungen nach Bedarf.

   | **Einstellung** | **Details** |
   | --- | --- |
   | **Name der Replikationsrichtlinie** | Der Richtlinienname. |
   | **Aufbewahrungszeitraum des Wiederherstellungspunkts** | Standardmäßig behält Site Recovery Wiederherstellungspunkte 24 Stunden lang bei. Sie können einen Wert zwischen 1 und 72 Stunden konfigurieren. |
   | **App-konsistente Momentaufnahmenhäufigkeit** | Standardmäßig erstellt Site Recovery alle 4 Stunden eine App-konsistente Momentaufnahme. Sie können einen Wert zwischen 1 und 12 Stunden konfigurieren.<br/><br/> Eine App-konsistente Momentaufnahme ist eine Zeitpunkt-Momentaufnahme der Anwendungsdaten innerhalb des virtuellen Computers. VSS (Volume Shadow Copy Service, Volumeschattenkopie-Dienst) stellt sicher, dass Apps zum Zeitpunkt der Momentaufnahme konsistent sind. |
   | **Replikationsgruppe** | Wenn für Ihre Anwendung VM-übergreifende Konsistenz mehrerer virtueller Computer erforderlich ist, können Sie eine Replikationsgruppe für diese VMs erstellen. Standardmäßig sind die ausgewählten VMs nicht Teil einer Replikationsgruppe. |

1. Wenn Sie VMs einer neuen oder vorhandenen Replikationsgruppe hinzufügen möchten, wählen Sie unter **Anpassen** die Option **Ja** für Multi-VM-Konsistenz aus, Klicken Sie anschließend auf **OK**.

   > [!NOTE]
   > - Alle Computer in einer Replikationsgruppe verfügen beim Failover über absturz- und anwendungskonsistente Wiederherstellungspunkte.
   > - Das Aktivieren von Multi-VM-Konsistenz kann die Workloadleistung beeinträchtigen (sie ist CPU-intensiv). Sie sollte nur verwendet werden, wenn Computer dieselbe Workload ausführen und Konsistenz für mehrere Computer erforderlich ist.
   > - Eine Replikationsgruppe kann maximal 16 virtuelle Computer enthalten.
   > - Wenn Sie die Multi-VM-Konsistenz aktivieren, kommunizieren Computer in der Replikationsgruppe über den Port 20004 miteinander. Stellen Sie sicher, dass die interne Kommunikation zwischen den VMs über diesen Port nicht durch eine Firewall blockiert wird.
   > - Stellen Sie bei Linux-VMs in einer Replikationsgruppe sicher, dass der ausgehende Datenverkehr auf Port 20004 gemäß den Anweisungen für die jeweilige Linux-Version manuell geöffnet wird.

### <a name="configure-encryption-settings"></a>Konfigurieren der Verschlüsselungseinstellungen

Wenn Azure Disk Encryption (ADE) für die Quell-VM aktiviert ist, überprüfen Sie die Einstellungen.

1. Überprüfen Sie die Einstellungen:
   1. **Schlüsseltresore für Datenträgerverschlüsselung**: Standardmäßig erstellt Site Recovery einen neuen Schlüsseltresor für die Datenträger-Verschlüsselungsschlüssel der Quell-VM mit dem Suffix `asr`. Wenn der Schlüsseltresor bereits vorhanden ist, wird er wiederverwendet.
   1. **Schlüsseltresore für Schlüsselverschlüsselung**: Site Recovery erstellt standardmäßig einen neuen Schlüsseltresor in der Zielregion. Der Name verfügt über das Suffix `asr` und basiert auf den Schlüsselverschlüsselungsschlüsseln der Quell-VM. Falls bereits ein von Site Recovery erstellter Schlüsseltresor vorhanden ist, wird er wiederverwendet.
1. Wählen Sie **Anpassen** aus, um benutzerdefinierte Schlüsseltresore auszuwählen.

>[!NOTE]
> Site Recovery unterstützt derzeit ADE mit und ohne Azure Active Directory (AAD) für virtuelle Computer, auf denen Windows-Betriebssysteme ausgeführt werden. Für Linux-Betriebssysteme wird nur ADE ohne AAD unterstützt. Bei Computern mit ADE 1.1 (ohne AAD) müssen die virtuellen Computer außerdem verwaltete Datenträger verwenden. Virtuelle Computer mit nicht verwalteten Datenträgern werden nicht unterstützt. Wenn Sie von ADE 0.1 (mit AAD) zu 1.1 wechseln, müssen Sie die Replikation deaktivieren und die Replikation für einen virtuellen Computer aktivieren, nachdem Sie 1.1 aktiviert haben.

### <a name="track-replication-status"></a>Nachverfolgen des Replikationsstatus

Nach dem Aktivieren der Replikation können Sie den Auftragsstatus nachverfolgen.

1. Wählen Sie unter **Einstellungen** die Option **Aktualisieren** aus, um den aktuellen Status abzurufen.
1. Verfolgen Sie den Fortschritt und Status wie folgt nach:
   1. Verfolgen Sie den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge**.
   1. Unter **Einstellungen** > **Replizierte Elemente** können Sie den Status der VMs und den der ersten Replikation einsehen. Wählen Sie die VM aus, um ein Drilldown auf die zugehörigen Einstellungen auszuführen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die Notfallwiederherstellung für eine Azure-VM konfiguriert. Jetzt können Sie ein Notfallwiederherstellungsverfahren ausführen, um zu überprüfen, ob das Failover wie erwartet funktioniert.

> [!div class="nextstepaction"]
> [Durchführen eines Notfallwiederherstellungsverfahrens](azure-to-azure-tutorial-dr-drill.md)
