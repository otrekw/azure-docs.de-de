---
title: Sichern einer Azure-VM über die VM-Einstellungen
description: In diesem Artikel erfahren Sie, wie Sie entweder einen einzelnen virtuellen Azure-Computer oder mehrere virtuelle Azure-Computer mit dem Azure Backup-Dienst sichern können.
ms.topic: conceptual
ms.date: 06/13/2019
ms.openlocfilehash: 55b71d2a2901cdde984df3ebfd68a2a643b78b74
ms.sourcegitcommit: 0194a29a960e3615f96a2d9d8a7e681cf3e8f9ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89667523"
---
# <a name="back-up-an-azure-vm-from-the-vm-settings"></a>Sichern einer Azure-VM über die VM-Einstellungen

Dieser Artikel beschreibt, wie Sie Azure-VMs mit dem [Azure Backup](backup-overview.md)-Dienst sichern. Sie können Azure-VMs mit verschiedenen Methoden sichern:

- Einzelne Azure-VM: Die Anweisungen in diesem Artikel beschreiben, wie Sie eine Azure-VM direkt aus den VM-Einstellungen sichern können.
- Mehrere Azure-VMs: Sie können einen Recovery Services-Tresor einrichten und die Sicherung für mehrere Azure-VMs konfigurieren. Folgen Sie für dieses Szenario den Anweisungen in [diesem Artikel](backup-azure-arm-vms-prepare.md).

## <a name="before-you-start"></a>Vorbereitung

1. [Erfahren Sie](backup-architecture.md#how-does-azure-backup-work), wie die Sicherung funktioniert, und [überprüfen](backup-support-matrix.md#azure-vm-backup-support) Sie die Supportanforderungen.
2. [Verschaffen Sie sich einen Überblick](backup-azure-vms-introduction.md) über die Sicherung von Azure-VMs.

### <a name="azure-vm-agent-installation"></a>Azure-VM-Agent-Installation

Zum Sichern von Azure-VMs installiert Azure Backup eine Erweiterung auf dem VM-Agent, der auf dem Computer ausgeführt wird. Wenn Ihre VM aus einem Azure Marketplace-Image erstellt wurde, wird der Agent ausgeführt. In manchen Fällen müssen Sie den Agent möglicherweise manuell installieren, wenn Sie beispielsweise eine benutzerdefinierte VM erstellen oder einen lokalen Computer migrieren.

- Wenn Sie den VM-Agent manuell installieren müssen, folgen Sie den Anweisungen für [Windows](../virtual-machines/extensions/agent-windows.md)- oder [Linux](../virtual-machines/extensions/agent-linux.md)-VMs.
- Nach der Installation des Agent installiert Azure Backup die Sicherungserweiterung auf den Agent, wenn Sie die Sicherung aktivieren. Die Erweiterung wird ohne Eingreifen des Benutzers aktualisiert und gepatcht.

## <a name="back-up-from-azure-vm-settings"></a>Sichern über Azure-VM-Einstellungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie auf **Alle Dienste** und geben Sie im Filter **Virtuelle Computer** ein, und wählen Sie dann **Virtuelle Computer** aus.
3. Wählen Sie aus der Liste der VMs die VM aus, die Sie sichern möchten.
4. Klicken Sie im VM-Menü auf **Sichern**.
5. Gehen Sie im **Recovery Services-Tresor** wie folgt vor:
   - Wenn Sie bereits über einen Tresor verfügen, klicken Sie auf **Vorhandenen auswählen**, und wählen Sie einen Tresor aus.
   - Wenn Sie über keinen Tresor verfügen, klicken Sie auf **Neu erstellen**. Geben Sie einen Namen für den Tresor an. Es wird in der gleichen Region und Ressourcengruppe erstellt wie die VM. Sie können diese Einstellungen nicht ändern, wenn Sie die Sicherung direkt aus den VM-Einstellungen heraus aktivieren.

        ![Aktivieren des Sicherungs-Assistenten](./media/backup-azure-vms-first-look-arm/vm-menu-enable-backup-small.png)

6. Gehen Sie unter **Sicherungsrichtlinie auswählen** wie folgt vor:

   - Übernehmen Sie die Standardrichtlinie. Dadurch wird die VM einmal täglich zur angegebenen Zeit gesichert und die Sicherungen werden 30 Tage lang im Tresor aufbewahrt.
   - Wählen Sie eine bestehende Sicherungsrichtlinie aus, sofern vorhanden.
   - Erstellen Sie eine neue Richtlinie, und definieren Sie die Richtlinieneinstellungen.  

       ![Sicherungsrichtlinie auswählen](./media/backup-azure-vms-first-look-arm/set-backup-policy.png)

7. Wählen Sie **Sicherung aktivieren** aus. Dadurch wird die Sicherungsrichtlinie mit der VM verknüpft.

    ![Schaltfläche „Sicherung aktivieren“](./media/backup-azure-vms-first-look-arm/vm-management-menu-enable-backup-button.png)

8. Den Konfigurationsprozess können Sie über die Benachrichtigungen im Portal nachverfolgen.
9. Klicken Sie nach Abschluss des Auftrags im VM-Menü auf **Sicherung**. Die Seite zeigt den Sicherungsstatus für die VM, Informationen über Wiederherstellungspunkte, laufende Aufträge und ausgegebene Warnmeldungen an.

   ![Sicherungsstatus](./media/backup-azure-vms-first-look-arm/backup-item-view-update.png)

10. Nachdem Sie die Sicherung aktiviert haben, wird eine erste Sicherung ausgeführt. Sie können die erste Sicherung sofort starten oder warten, bis sie gemäß dem Sicherungszeitplan beginnt.
    - Bis zum Abschluss des ersten Sicherungsvorgangs wird **Status der letzten Sicherung** als **Warnung (erste Sicherung steht aus)** angezeigt.
    - Klicken Sie auf den Namen der Sicherungsrichtlinie, um zu sehen, wann die nächste geplante Sicherung durchgeführt wird.

## <a name="run-a-backup-immediately"></a>Sofortige Ausführung einer Sicherung

1. Klicken Sie im VM-Menü auf **Sicherung** > **Jetzt sichern**, um sofort eine Sicherung durchzuführen.

    ![Ausführen der Sicherung](./media/backup-azure-vms-first-look-arm/backup-now-update.png)

2. Verwenden Sie den Kalender unter **Jetzt sichern**, um auszuwählen, bis wann der Wiederherstellungspunkt beibehalten wird, und klicken Sie dann auf **OK**.

    ![Tage für Aufbewahrung von Sicherungen](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

3. Sie werden über Portalbenachrichtigungen darüber informiert, dass der Sicherungsauftrag ausgelöst wurde. Klicken Sie auf **Alle Aufträge anzeigen**, um den Sicherungsfortschritt zu überwachen.

## <a name="back-up-from-the-recovery-services-vault"></a>Sichern über den Recovery Services-Tresor

Befolgen Sie die Anweisungen in [diesem Artikel](backup-azure-arm-vms-prepare.md), um die Sicherung für Azure-VMs zu aktivieren, indem Sie einen Azure Backup Recovery Services-Tresor einrichten und die Sicherung im Tresor aktivieren.

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie Schwierigkeiten mit einem der Verfahren in diesem Artikel haben, lesen Sie den [Leitfaden zur Problembehandlung](backup-azure-vms-troubleshoot.md).
- [Erfahren Sie mehr](backup-azure-manage-vms.md) über das Verwalten Ihrer Sicherungen.
