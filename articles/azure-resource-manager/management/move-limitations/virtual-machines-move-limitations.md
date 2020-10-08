---
title: Verschieben virtueller Azure-Computer in ein neues Abonnement oder in eine neue Ressourcengruppe
description: Verwenden Sie Azure Resource Manager, um virtuelle Computer in eine neue Ressourcengruppe oder ein neues Abonnement zu verschieben.
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: 219a8b438d2715f6e97085a527b386e51759ec2c
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91317105"
---
# <a name="move-guidance-for-virtual-machines"></a>Anleitung zum Verschieben virtueller Computer

In diesem Artikel werden Szenarien beschrieben, die derzeit nicht unterstützt werden, und die Schritte zum Verschieben virtueller Computer mit Sicherungen.

## <a name="scenarios-not-supported"></a>Nicht unterstützte Szenarien

Folgende Szenarios werden noch nicht unterstützt:

* Eine Virtual Machine Scale Sets-Instanz mit Load Balancer der Standard-SKU oder einer öffentlichen IP-Adresse der Standard-SKU kann nicht verschoben werden.
* Von Marketplace-Ressourcen erstellte virtuelle Computer, an die Pläne angefügt sind, können nicht abonnementübergreifend verschoben werden. Heben Sie die Bereitstellung des virtuellen Computers im aktuellen Abonnement auf, und stellen Sie ihn im neuen Abonnement erneut bereit.
* Virtuelle Computer in einem vorhandenen virtuellen Netzwerk können nicht zu einem neuen Abonnement verschoben werden, wenn Sie nicht alle Ressourcen im virtuellen Netzwerk verschieben.
* Virtuelle Computer mit niedriger Priorität und VM-Skalierungsgruppen mit niedriger Priorität können nicht ressourcengruppen- oder abonnementübergreifend verschoben werden.
* Virtuelle Computer in einer Verfügbarkeitsgruppe können nicht einzeln verschoben werden.

## <a name="azure-disk-encryption"></a>Azure Disk Encryption

Sie können einen virtuellen Computer, der in einen Schlüsseltresor integriert ist, nicht verschieben, um [Azure Disk Encryption für virtuelle Linux-Computer](../../../virtual-machines/linux/disk-encryption-overview.md) oder [Azure Disk Encryption für virtuelle Windows-Computer](../../../virtual-machines/windows/disk-encryption-overview.md) zu implementieren. Um den virtuellen Computer zu verschieben, müssen Sie die Verschlüsselung deaktivieren.

```azurecli-interactive
az vm encryption disable --resource-group demoRG --name myVm1
```

```azurepowershell-interactive
Disable-AzVMDiskEncryption -ResourceGroupName demoRG -VMName myVm1
```

## <a name="virtual-machines-with-azure-backup"></a>Virtuelle Computer mit Azure Backup

Um mit Azure Backup konfigurierte virtuelle Computer zu verschieben, müssen Sie die Wiederherstellungspunkte im Tresor löschen.

Wenn [vorläufiges Löschen](../../../backup/backup-azure-security-feature-cloud.md) für Ihren virtuellen Computer aktiviert ist, können Sie den virtuellen Computer nicht verschieben, solange diese Wiederherstellungspunkte noch vorhanden sind. [Deaktivieren Sie entweder vorläufiges Löschen](../../../backup/backup-azure-security-feature-cloud.md#enabling-and-disabling-soft-delete), oder warten Sie 14 Tage nach dem Löschen der Wiederherstellungspunkte.

### <a name="portal"></a>Portal

1. Halten Sie die Sicherung vorübergehend an, und bewahren Sie Sicherungsdaten auf.
2. Gehen Sie folgendermaßen vor, um mit Azure Backup konfigurierte virtuelle Computer zu verschieben:

   1. Ermitteln Sie den Speicherort Ihres virtuellen Computers.
   2. Suchen Sie eine Ressourcengruppe mit dem folgenden Namensmuster: `AzureBackupRG_<VM location>_1`. Der Name weist z. B. das Format *AzureBackupRG_westus2_1* auf.
   3. Aktivieren Sie im Azure-Portal die Option **Ausgeblendete Typen anzeigen**.
   4. Suchen Sie die Ressource mit dem Typ **Microsoft.Compute/restorePointCollections** und dem Namensmuster `AzureBackup_<VM name>_###########`.
   5. Löschen Sie diese Ressource. Dieser Vorgang löscht nur die sofortigen Wiederherstellungspunkte und nicht die gesicherten Daten im Tresor.
   6. Nach Abschluss des Löschvorgangs können Sie den virtuellen Computer verschieben.

3. Verschieben Sie die VM in die Zielressourcengruppe.
4. Nehmen Sie die Sicherung wieder auf.

### <a name="powershell"></a>PowerShell

1. Ermitteln Sie den Speicherort Ihres virtuellen Computers.

1. Suchen Sie nach einer Ressourcengruppe mit diesem Namensmuster: `AzureBackupRG_<VM location>_1`. Der Name kann z. B. `AzureBackupRG_westus2_1` lauten.

1. Wenn Sie nur einen virtuellen Computer verschieben, rufen Sie die Wiederherstellungspunktsammlung für diesen virtuellen Computer ab.

   ```azurepowershell-interactive
   $restorePointCollection = Get-AzResource -ResourceGroupName AzureBackupRG_<VM location>_1 -name AzureBackup_<VM name>* -ResourceType Microsoft.Compute/restorePointCollections
   ```

   Löschen Sie diese Ressource. Dieser Vorgang löscht nur die sofortigen Wiederherstellungspunkte und nicht die gesicherten Daten im Tresor.

   ```azurepowershell-interactive
   Remove-AzResource -ResourceId $restorePointCollection.ResourceId -Force
   ```

1. Wenn Sie alle virtuellen Computer mit Sicherungen an diesem Standort verschieben, rufen Sie die Wiederherstellungspunktsammlungen für diese virtuellen Computer ab.

   ```azurepowershell-interactive
   $restorePointCollection = Get-AzResource -ResourceGroupName AzureBackupRG_<VM location>_1 -ResourceType Microsoft.Compute/restorePointCollections
   ```

   Löschen Sie jede Ressource. Dieser Vorgang löscht nur die sofortigen Wiederherstellungspunkte und nicht die gesicherten Daten im Tresor.

   ```azurepowershell-interactive
   foreach ($restorePoint in $restorePointCollection)
   {
     Remove-AzResource -ResourceId $restorePoint.ResourceId -Force
   }
   ```

### <a name="azure-cli"></a>Azure CLI

1. Ermitteln Sie den Speicherort Ihres virtuellen Computers.

1. Suchen Sie nach einer Ressourcengruppe mit diesem Namensmuster: `AzureBackupRG_<VM location>_1`. Der Name kann z. B. `AzureBackupRG_westus2_1` lauten.

1. Wenn Sie nur einen virtuellen Computer verschieben, rufen Sie die Wiederherstellungspunktsammlung für diesen virtuellen Computer ab.

   ```azurecli-interactive
   RESTOREPOINTCOL=$(az resource list -g AzureBackupRG_<VM location>_1 --resource-type Microsoft.Compute/restorePointCollections --query "[?starts_with(name, 'AzureBackup_<VM name>')].id" --output tsv)
   ```

   Löschen Sie diese Ressource. Dieser Vorgang löscht nur die sofortigen Wiederherstellungspunkte und nicht die gesicherten Daten im Tresor.

   ```azurecli-interactive
   az resource delete --ids $RESTOREPOINTCOL
   ```

1. Wenn Sie alle virtuellen Computer mit Sicherungen an diesem Standort verschieben, rufen Sie die Wiederherstellungspunktsammlungen für diese virtuellen Computer ab.

   ```azurecli-interactive
   RESTOREPOINTCOL=$(az resource list -g AzureBackupRG_<VM location>_1 --resource-type Microsoft.Compute/restorePointCollections)
   ```

   Löschen Sie jede Ressource. Dieser Vorgang löscht nur die sofortigen Wiederherstellungspunkte und nicht die gesicherten Daten im Tresor.

   ```azurecli-interactive
   az resource delete --ids $RESTOREPOINTCOL
   ```

## <a name="next-steps"></a>Nächste Schritte

* Befehle zum Verschieben von Ressourcen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../move-resource-group-and-subscription.md).

* Weitere Informationen zum Verschieben von Recovery Services-Tresoren für die Sicherung finden Sie unter [Einschränkungen von Recovery Services](../../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).
