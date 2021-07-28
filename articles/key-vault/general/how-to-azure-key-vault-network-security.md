---
title: Konfigurieren der Azure Key Vault-Netzwerkkonfiguration
description: Schrittweise Anweisungen zum Konfigurieren von Key Vault-Firewalls und virtuellen Netzwerken
services: key-vault
author: sebansal
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 5/11/2021
ms.author: mbaldwin
ms.custom: devx-track-azurecli
ms.openlocfilehash: d6035436a005012cf67a46302213f79b2dce1f59
ms.sourcegitcommit: 42ac9d148cc3e9a1c0d771bc5eea632d8c70b92a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/13/2021
ms.locfileid: "109850190"
---
# <a name="configure-azure-key-vault-networking-settings"></a>Konfigurieren von Azure Key Vault-Netzwerkeinstellungen

In diesem Artikel erfahren Sie, wie Sie die Azure Key Vault-Netzwerkeinstellungen konfigurieren, um mit anderen Anwendungen und Azure-Diensten zu arbeiten. Ausführlichere Informationen zu verschiedenen Netzwerksicherheitskonfigurationen finden Sie [hier](network-security.md).

Hier finden Sie schrittweise Anweisungen zum Konfigurieren der Key Vault-Firewall und der virtuellen Netzwerke über das Azure-Portal, die Azure CLI und Azure PowerShell.

# <a name="portal"></a>[Portal](#tab/azure-portal)


1. Navigieren Sie zum Schlüsseltresor, den Sie absichern möchten.
2. Wählen Sie **Netzwerk** und anschließend die Registerkarte **Firewalls und virtuelle Netzwerke** aus.
3. Wählen Sie unter **Zugriff erlauben von** den Eintrag **Ausgewählte Netzwerke** aus.
4. Wenn Sie vorhandene virtuelle Netzwerke zu Firewalls und VNET-Regeln hinzufügen möchten, klicken Sie auf **+ Vorhandene virtuelle Netzwerke hinzufügen**.
5. Wählen Sie auf dem angezeigten neuen Blatt das Abonnement, die virtuellen Netzwerke und Subnetze aus, denen Sie Zugriff auf diesen Schlüsseltresor gewähren möchten. Wenn für die von Ihnen ausgewählten virtuellen Netzwerke und Subnetze keine Dienstendpunkte aktiviert sind, bestätigen Sie, dass Sie Dienstendpunkte aktivieren möchten, und wählen Sie **Aktivieren** aus. Es kann bis zu 15 Minuten dauern, bis diese Änderung wirksam wird.
6. Fügen Sie unter **IP-Netzwerke** IPv4-Adressbereiche hinzu, indem Sie IPv4-Adressbereiche in [CIDR-Notation (Classless Inter-Domain Routing)](https://tools.ietf.org/html/rfc4632) oder einzelne IP-Adressen eingeben.
7. Wenn Sie zulassen möchten, dass vertrauenswürdige Microsoft-Dienste die Key Vault-Firewall umgehen, wählen Sie „Ja“ aus. Eine vollständige Liste der aktuellen vertrauenswürdigen Key Vault-Dienste finden Sie unter folgendem Link: [Azure Key Vault – Vertrauenswürdige Dienste](./overview-vnet-service-endpoints.md#trusted-services).
7. Wählen Sie **Speichern** aus.

Sie können auch neue virtuelle Netzwerke und Subnetze hinzufügen und dann Dienstendpunkte für die neu erstellten virtuellen Netzwerke und Subnetze aktivieren, indem Sie **+ Neues virtuelles Netzwerk hinzufügen** auswählen. Folgen Sie dann den Anweisungen.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Im Folgenden finden Sie Anweisungen zum Konfigurieren von Key Vault-Firewalls und virtuellen Netzwerken mithilfe der Azure-Befehlszeilenschnittstelle.

1. [Installieren Sie die Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli), und [melden Sie sich an](/cli/azure/authenticate-azure-cli).

2. Listen Sie die verfügbaren VNET-Regeln auf. Wenn Sie keine Regeln für diesen Schlüsselspeicher festgelegt haben, ist die Liste leer.
   ```azurecli
   az keyvault network-rule list --resource-group myresourcegroup --name mykeyvault
   ```

3. Aktivieren Sie den Dienstendpunkt für Key Vault in einem vorhandenen virtuellen Netzwerk und Subnetz.
   ```azurecli
   az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.KeyVault"
   ```

4. Fügen Sie eine Netzwerkregel für ein virtuelles Netzwerk und Subnetz hinzu.
   ```azurecli
   subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
   az keyvault network-rule add --resource-group "demo9311" --name "demo9311premium" --subnet $subnetid
   ```

5. Fügen Sie einen IP-Adressbereich hinzu, aus dem Datenverkehr zugelassen werden soll.
   ```azurecli
   az keyvault network-rule add --resource-group "myresourcegroup" --name "mykeyvault" --ip-address "191.10.18.0/24"
   ```

6. Wenn dieser Schlüsselspeicher für vertrauenswürdige Dienste zugänglich sein sollte, legen Sie `bypass` auf `AzureServices` fest.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mykeyvault" --bypass AzureServices
   ```

7. Aktivieren Sie die Netzwerkregeln, indem Sie die Standardaktion auf `Deny` festlegen.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mekeyvault" --default-action Deny
   ```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Im Folgenden finden Sie Anweisungen zum Konfigurieren von Key Vault-Firewalls und virtuellen Netzwerken mithilfe der PowerShell:

1. Installieren Sie die neueste Version von [Azure PowerShell](/powershell/azure/install-az-ps), und [melden Sie sich an](/powershell/azure/authenticate-azureps).

2. Listen Sie die verfügbaren VNET-Regeln auf. Wenn Sie keine Regeln für diesen Schlüsselspeicher festgelegt haben, ist die Liste leer.
   ```powershell
   (Get-AzKeyVault -VaultName "mykeyvault").NetworkAcls
   ```

3. Aktivieren Sie den Dienstendpunkt für Key Vault in einem vorhandenen virtuellen Netzwerk und Subnetz.
   ```powershell
   Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.KeyVault" | Set-AzVirtualNetwork
   ```

4. Fügen Sie eine Netzwerkregel für ein virtuelles Netzwerk und Subnetz hinzu.
   ```powershell
   $subnet = Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"
   Add-AzKeyVaultNetworkRule -VaultName "mykeyvault" -VirtualNetworkResourceId $subnet.Id
   ```

5. Fügen Sie einen IP-Adressbereich hinzu, aus dem Datenverkehr zugelassen werden soll.
   ```powershell
   Add-AzKeyVaultNetworkRule -VaultName "mykeyvault" -IpAddressRange "16.17.18.0/24"
   ```

6. Wenn dieser Schlüsselspeicher für vertrauenswürdige Dienste zugänglich sein sollte, legen Sie `bypass` auf `AzureServices` fest.
   ```powershell
   Update-AzKeyVaultNetworkRuleSet -VaultName "mykeyvault" -Bypass AzureServices
   ```

7. Aktivieren Sie die Netzwerkregeln, indem Sie die Standardaktion auf `Deny` festlegen.
   ```powershell
   Update-AzKeyVaultNetworkRuleSet -VaultName "mykeyvault" -DefaultAction Deny
   ```
---
## <a name="references"></a>References
* ARM-Vorlagenreferenz: [Azure Key Vault: ARM-Vorlagenreferenz](/azure/templates/Microsoft.KeyVault/vaults)
* Azure CLI-Befehle: [az keyvault network-rule](/cli/azure/keyvault/network-rule)
* Azure PowerShell-Cmdlets: [Get-AzKeyVault](/powershell/module/az.keyvault/get-azkeyvault), [Add-AzKeyVaultNetworkRule](/powershell/module/az.KeyVault/Add-azKeyVaultNetworkRule), [Remove-AzKeyVaultNetworkRule](/powershell/module/az.KeyVault/Remove-azKeyVaultNetworkRule), [Update-AzKeyVaultNetworkRuleSet](/powershell/module/az.KeyVault/Update-azKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Nächste Schritte

* [VNET-Dienstendpunkte für Key Vault](overview-vnet-service-endpoints.md)
* [Azure Key Vault-Sicherheitsübersicht](security-features.md)
