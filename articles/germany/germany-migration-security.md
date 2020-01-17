---
title: Migrieren von Azure-Sicherheitsressourcen – Azure Deutschland zu Azure weltweit
description: Dieser Artikel enthält Informationen zum Migrieren von Azure-Sicherheitsressourcen von Azure Deutschland zu Azure weltweit.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 12/12/2019
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: a3321c1e1dcce642aec4a1ee1da5aa583fa747c1
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75436654"
---
# <a name="migrate-security-resources-to-global-azure"></a>Migrieren von Sicherheitsressourcen zu Azure weltweit

> [!IMPORTANT]
> Seit [August 2018](https://news.microsoft.com/europe/2018/08/31/microsoft-to-deliver-cloud-services-from-new-datacentres-in-germany-in-2019-to-meet-evolving-customer-needs/) haben wir keine neuen Kunden akzeptiert und keine neuen Features und Dienste an den ursprünglichen Microsoft Cloud Deutschland-Standorten bereitgestellt.
>
> Aufgrund der Weiterentwicklung der Kundenbedürfnisse haben wir vor Kurzem zwei neue Rechenzentrumsregionen in Deutschland [gestartet](https://azure.microsoft.com/blog/microsoft-azure-available-from-new-cloud-regions-in-germany/), die Datenresidenz für Kundendaten, umfassende Konnektivität mit dem globalen Cloudnetzwerk von Microsoft sowie wettbewerbsfähige Preise bieten. 
>
> Profitieren Sie von der Vielfalt der Funktionen, Sicherheit auf Unternehmensniveau und den umfangreichen Features, die in unseren neuen deutschen Rechenzentrumsregionen zur Verfügung stehen, und [migrieren](germany-migration-main.md) Sie noch heute.

Dieser Artikel enthält Informationen dazu, wie Sie Azure-Sicherheitsressourcen von Azure Deutschland zu Azure weltweit migrieren können.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-active-directory"></a>Azure Active Directory

Weitere Informationen zum Migrieren von Azure Active Directory finden Sie unter [Migration von Identity-Ressourcen von Azure Deutschland zu Azure weltweit](./germany-migration-identity.md#azure-active-directory).

## <a name="key-vault"></a>Key Vault

Einige Features von Azure Key Vault können nicht von Azure Deutschland zu Azure weltweit migriert werden.

### <a name="encryption-keys"></a>Verschlüsselungsschlüssel

Verschlüsselungsschlüssel können nicht migriert werden. Erstellen Sie neue Schlüssel in der Zielregion zu, und verwenden Sie dann diese Schlüssel, um die Zielressource (z. B. Azure Storage oder Azure SQL-Datenbank) zu schützen. Migrieren Sie die Daten sicher aus der alten Region in die neue Region.

### <a name="application-secrets"></a>Anwendungsgeheimnisse

Anwendungsgeheimnisse sind Zertifikate, Speicherkontoschlüssel und andere anwendungsbezogene Geheimnisse. Während einer Migration erstellen Sie zunächst einen neuen Schlüsseltresor in Azure weltweit. Danach führen Sie eine der folgenden Aktionen aus:

- Erstellen Sie neue Anwendungsgeheimnisse.
- Lesen Sie die aktuellen Geheimnisse in Azure Deutschland, und geben Sie den Wert dann in den neuen Tresor ein.

```powershell
Get-AzKeyVaultSecret -vaultname mysecrets -name Deploydefaultpw
```

Weitere Informationen finden Sie unter:

- Frischen Sie Ihre Kenntnisse auf, indem Sie die [Tutorials zu Key Vault](https://docs.microsoft.com/azure/key-vault/) durcharbeiten.
- Lesen Sie die [Übersicht über Key Vault](../key-vault/key-vault-overview.md).
- Lesen Sie die Referenz zu den [PowerShell-Cmdlets für Key Vault](/powershell/module/az.keyvault/).

## <a name="vpn-gateway"></a>VPN Gateway

Das Migrieren einer Azure VPN Gateway-Instanz von Azure Deutschland zu Azure weltweit wird derzeit nicht unterstützt. Es wird empfohlen, in Azure weltweit eine neue VPN Gateway-Instanz zu erstellen und zu konfigurieren.

Informationen zu Ihrer aktuellen VPN Gateway-Konfiguration können Sie über das Portal oder mit PowerShell abrufen. Verwenden Sie in PowerShell einen Satz von Cmdlets, die mit `Get-AzVirtualNetworkGateway*` beginnen.

Stellen Sie sicher, dass Sie Ihre lokale Konfiguration aktualisieren. Löschen Sie auch alle vorhandenen Regeln für die alten IP-Adressbereiche, nachdem Sie Ihre Azure-Netzwerkumgebung aktualisiert haben.

Weitere Informationen finden Sie unter:

- Frischen Sie Ihre Kenntnisse auf, indem Sie die [Tutorials zu VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway) durcharbeiten.
- Im Artikel [Herstellen einer Site-to-Site-Verbindung](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) finden Sie weitere Informationen zu diesem Thema.
- Lesen Sie auch den Artikel über die PowerShell-Cmdlets [Get-AzVirtualNetworkGateway](/powershell/module/az.network/get-azvirtualnetworkgateway).
- Lesen Sie den Blogbeitrag [Herstellen einer Site-to-Site-Verbindung](https://blogs.technet.microsoft.com/ralfwi/2017/02/02/connecting-clouds/).
  
## <a name="application-gateway"></a>Application Gateway

Das Migrieren einer Azure Application Gateway-Instanz von Azure Deutschland zu Azure weltweit wird derzeit nicht unterstützt. Es wird empfohlen, in Azure weltweit ein neues Gateway zu erstellen und zu konfigurieren.

Informationen zu Ihrer aktuellen Gatewaykonfiguration können Sie über das Portal oder mit PowerShell abrufen. Verwenden Sie in PowerShell einen Satz von Cmdlets, die mit `Get-AzApplicationGateway*` beginnen.

Weitere Informationen finden Sie unter:

- Frischen Sie Ihre Kenntnisse auf, indem Sie die [Tutorials zu Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) durcharbeiten.
- Im Artikel [Erstellen eines Application Gateways](../application-gateway/quick-create-portal.md) finden Sie weitere Informationen zu diesem Thema.
- Lesen Sie auch den Artikel über die PowerShell-Cmdlets [Get-AzApplicationGateway](/powershell/module/az.network/get-azapplicationgateway).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Tools, Techniken und Empfehlungen zum Migrieren von Ressourcen in den folgenden Dienstkategorien:

- [Compute](./germany-migration-compute.md)
- [Netzwerk](./germany-migration-networking.md)
- [Storage](./germany-migration-storage.md)
- [Web](./germany-migration-web.md)
- [Datenbanken](./germany-migration-databases.md)
- [Analyse](./germany-migration-analytics.md)
- [IoT](./germany-migration-iot.md)
- [Integration](./germany-migration-integration.md)
- [Identität](./germany-migration-identity.md)
- [Verwaltungstools](./germany-migration-management-tools.md)
- [Medien](./germany-migration-media.md)

