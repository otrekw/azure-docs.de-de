---
title: 'Konfigurieren von Azure Key Vault-Firewalls und virtuellen Netzwerken: Azure Key Vault'
description: Informationen zu Netzwerkeinstellungen für Schlüsseltresore
services: key-vault
author: msmbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 10/01/2020
ms.author: mbaldwin
ms.custom: devx-track-azurecli
ms.openlocfilehash: b3e9332706c3bcc9d4f4cabd20d10fa099271d69
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114285329"
---
# <a name="configure-azure-key-vault-firewalls-and-virtual-networks"></a>Konfigurieren von Azure Key Vault-Firewalls und virtuellen Netzwerken

Dieses Dokument enthält detaillierte Informationen zu den verschiedenen Konfigurationen der Key Vault-Firewall. Eine ausführliche Anleitung zum Konfigurieren dieser Einstellungen finden Sie [hier](how-to-azure-key-vault-network-security.md). 

Weitere Informationen finden Sie unter [VNET-Dienstendpunkte für Azure Key Vault](overview-vnet-service-endpoints.md).

## <a name="firewall-settings"></a>Firewalleinstellungen

In diesem Abschnitt werden die verschiedenen Konfigurationsmöglichkeiten der Azure Key Vault-Firewall beschrieben.

### <a name="key-vault-firewall-disabled-default"></a>Key Vault-Firewall deaktiviert (Standardeinstellung)

Wenn Sie einen neuen Schlüsseltresor erstellen, ist die Azure Key Vault-Firewall standardmäßig deaktiviert. Alle Anwendungen und Azure-Dienste können auf den Schlüsseltresor zugreifen und Anforderungen an den Schlüsseltresor senden. Diese Konfiguration bedeutet allerdings nicht, dass jeder beliebige Benutzer Vorgänge für Ihren Schlüsseltresor ausführen kann. Für den Zugriff auf im Schlüsseltresor gespeicherte Geheimnisse, Schlüssel und Zertifikate sind trotzdem eine Azure Active Directory-Authentifizierung und Zugriffsrichtlinienberechtigungen erforderlich. Ausführlichere Informationen zur Schlüsseltresorauthentifizierung finden Sie unter [Grundlagen der Key Vault-Authentifizierung](./authentication.md). Weitere Informationen finden Sie unter [Zugreifen auf Azure Key Vault hinter einer Firewall](./access-behind-firewall.md).

### <a name="key-vault-firewall-enabled-trusted-services-only"></a>Key Vault-Firewall aktiviert (nur vertrauenswürdige Dienste)

Wenn Sie die Key Vault-Firewall aktivieren, können Sie festlegen, dass vertrauenswürdigen Microsoft-Diensten die Umgehung dieser Firewall erlaubt werden soll. Die Liste der vertrauenswürdigen Dienste deckt nicht alle Azure-Dienste ab. Azure DevOps ist beispielsweise nicht in der Liste der vertrauenswürdigen Dienste enthalten. **Das bedeutet nicht, dass Dienste, die nicht in der Liste der vertrauenswürdigen Dienste enthalten sind, nicht vertrauenswürdig oder unsicher sind.** Die Liste der vertrauenswürdigen Dienste umfasst Dienste, bei denen der gesamte im Dienst ausgeführte Code von Microsoft kontrolliert wird. Da Benutzer in Azure-Diensten wie Azure DevOps benutzerdefinierten Code schreiben können, bietet Microsoft keine Option zur Erstellung einer Pauschalgenehmigung für den Dienst. Außerdem gilt: Die Tatsache, dass ein Dienst in der Liste vertrauenswürdiger Dienste enthalten ist, bedeutet nicht, dass er für alle Szenarien zulässig ist. 

Wie Sie ermitteln, ob ein Dienst, den Sie verwenden möchten, in der Liste der vertrauenswürdigen Dienste enthalten ist, erfahren Sie [hier](./overview-vnet-service-endpoints.md#trusted-services).
Eine Schrittanleitung für das Portal, die Azure CLI und PowerShell finden Sie [hier](how-to-azure-key-vault-network-security.md).

### <a name="key-vault-firewall-enabled-ipv4-addresses-and-ranges---static-ips"></a>Key Vault-Firewall aktiviert (IPv4-Adressen und -Bereiche – statische IP-Adressen)

Wenn Sie für einen bestimmten Dienst den Zugriff auf den Schlüsseltresor durch die Key Vault-Firewall autorisieren möchten, können Sie seine IP-Adresse der Positivliste der Key Vault-Firewall hinzufügen. Diese Konfiguration eignet sich am besten für Dienste mit statischen IP-Adressen oder bekannten Bereichen. Für diesen Fall gilt ein Grenzwert von 1.000 CIDR-Bereichen.

Gehen Sie wie folgt vor, wenn Sie eine IP-Adresse oder einen Bereich einer Azure-Ressource (beispielsweise eine Web- oder Logik-App) zulassen möchten:

1. Anmelden beim Azure-Portal
1. Wählen Sie die Ressource (spezifische Instanz des Diensts) aus.
1. Klicken Sie unter „Einstellungen“ auf das Blatt Eigenschaften“.
1. Suchen Sie nach dem Feld „IP-Adresse“.
1. Kopieren Sie den Wert oder Bereich, und fügen Sie ihn in die Positivliste der Schlüsseltresorfirewall ein.

Wenn Sie einem gesamten Azure-Dienst den Zugriff durch die Key Vault-Firewall gestatten möchten, verwenden Sie die [Liste der öffentlich dokumentierten IP-Adressen von Azure-Rechenzentren](https://www.microsoft.com/download/details.aspx?id=41653). Suchen Sie die IP-Adressen, die dem gewünschten Dienst in der gewünschten Region zugeordnet sind, und fügen Sie sie wie oben beschrieben der Key Vault-Firewall hinzu.

### <a name="key-vault-firewall-enabled-virtual-networks---dynamic-ips"></a>Key Vault-Firewall aktiviert (virtuelle Netzwerke – dynamische IP-Adressen)

Wenn Sie versuchen, eine Azure-Ressource (beispielsweise einen virtuellen Computer) über den Schlüsseltresor zuzulassen, kann es vorkommen, dass Sie keine statischen IP-Adressen verwenden können und ggf. nicht für alle IP-Adressen für Azure Virtual Machines den Zugriff auf Ihren Schlüsseltresor zulassen möchten.

Erstellen Sie in diesem Fall die Ressource in einem virtuellen Netzwerk, und lassen Sie dann für Datenverkehr aus dem spezifischen virtuellen Netzwerk und Subnetz den Zugriff auf Ihren Schlüsseltresor zu. Führen Sie dazu die folgenden Schritte aus.

1. Anmelden beim Azure-Portal
1. Wählen Sie den Schlüsseltresor aus, den Sie konfigurieren möchten.
1. Wählen Sie das Blatt „Netzwerk“ aus.
1. Wählen Sie „+ Vorhandenes virtuelles Netzwerk hinzufügen“ aus.
1. Wählen Sie das virtuelle Netzwerk und das Subnetz aus, für die Sie den Zugriff durch die Schlüsseltresorfirewall zulassen möchten.

### <a name="key-vault-firewall-enabled-private-link"></a>Key Vault-Firewall aktiviert (Private Link)

Informationen zum Konfigurieren einer Private Link-Verbindung für Ihren Schlüsseltresor finden Sie [hier](./private-link-service.md).

> [!IMPORTANT]
> Nachdem Firewallregeln in Kraft sind, können Benutzer nur Vorgänge auf Key Vault-[Datenebene](security-features.md#privileged-access) ausführen, wenn ihre Anforderungen aus zulässigen virtuellen Netzwerken oder IPv4-Adressbereichen stammen. Dies gilt auch für den Zugriff auf den Schlüsseltresor aus dem Azure-Portal. Obwohl Benutzer im Azure-Portal zu einem Schlüsseltresor navigieren können, können sie möglicherweise keine Schlüssel, Geheimnisse oder Zertifikate auflisten, wenn ihr Clientcomputer nicht in der Zulassungsliste enthalten ist. Dies wirkt sich auch auf die Key Vault-Auswahl anderer Azure-Dienste aus. Benutzer können möglicherweise eine Liste von Schlüsseltresoren einsehen, aber keine Schlüssel auflisten, wenn Firewallregeln ihren Clientcomputer blockieren.

> [!NOTE]
> Bedenken Sie dabei folgende Konfigurationseinschränkungen:
> * Maximal 200 VNET-Regeln und 1.000 IPv4-Regeln sind zulässig. 
> * IP-Netzwerkregeln sind nur für öffentliche IP-Adressen zulässig. Für private Netzwerke reservierte IP-Adressbereiche (gemäß RFC 1918) sind in IP-Adressregeln nicht zulässig. Private Netzwerke enthalten Adressen, die mit **10.** , **172.16-31** und **192.168.** beginnen. 
> * Derzeit werden nur IPv4-Adressen unterstützt.

## <a name="references"></a>References
* ARM-Vorlagenreferenz: [Azure Key Vault: ARM-Vorlagenreferenz](/azure/templates/Microsoft.KeyVault/vaults)
* Azure CLI-Befehle: [az keyvault network-rule](/cli/azure/keyvault/network-rule)
* Azure PowerShell-Cmdlets: [Get-AzKeyVault](/powershell/module/az.keyvault/get-azkeyvault), [Add-AzKeyVaultNetworkRule](/powershell/module/az.KeyVault/Add-azKeyVaultNetworkRule), [Remove-AzKeyVaultNetworkRule](/powershell/module/az.KeyVault/Remove-azKeyVaultNetworkRule), [Update-AzKeyVaultNetworkRuleSet](/powershell/module/az.KeyVault/Update-azKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Nächste Schritte

* [VNET-Dienstendpunkte für Key Vault](overview-vnet-service-endpoints.md)
* [Azure Key Vault-Sicherheitsübersicht](security-features.md)