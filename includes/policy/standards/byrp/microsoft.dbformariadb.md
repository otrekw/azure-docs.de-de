---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 07/22/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 89094d3adce55f907fe6cac5e2cfdd06a6206cd7
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89380319"
---
## <a name="azure-security-benchmark"></a>Vergleichstest für die Azure-Sicherheit

Der [Azure-Sicherheitsvergleichstest](../../../../articles/security/benchmarks/overview.md) enthält Empfehlungen zum Schutz Ihrer Cloudlösungen in Azure. Zum Anzeigen der vollständigen Zuordnung dieses Diensts zum Azure-Sicherheitsvergleichstest sehen Sie sich die [Zuordnungsdateien zum Azure-Sicherheitsvergleichstest](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines) an.

Weitere Informationen dazu, wie die verfügbaren Azure Policy-Build-Ins für alle Azure-Dienste diesem Konformitätsstandard entsprechen, finden Sie unter [Einhaltung gesetzlicher Bestimmungen in Azure Policy: Azure Sicherheitsvergleichstest](../../../../articles/governance/policy/samples/azure-security-benchmark.md).

|Domain |Steuerungs-ID |Steuerungstitel |Richtlinie<br /><sub>(Azure-Portal)</sub> |Version der Richtlinie<br /><sub>(GitHub)</sub>  |
|---|---|---|---|---|
|Netzwerksicherheit |1.1 |Schützen von Ressourcen mithilfe von Netzwerksicherheitsgruppen oder Azure Firewall in Virtual Network |[Privater Endpunkt muss für MariaDB-Server aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0a1302fb-a631-4106-9753-f3d494733990) |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MariaDB_EnablePrivateEndPoint_Audit.json) |
|Datenwiederherstellung |9.1 |Sicherstellen regelmäßiger automatisierter Sicherungen |[Georedundante Sicherung muss für Azure Database for MariaDB aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0ec47710-77ff-4a3d-9181-6aa50af424d0) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/GeoRedundant_DBForMariaDB_Audit.json) |
|Datenwiederherstellung |9.2 |Durchführen vollständiger Systemsicherungen und Sichern aller von Kunden verwalteten Schlüssel |[Georedundante Sicherung muss für Azure Database for MariaDB aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0ec47710-77ff-4a3d-9181-6aa50af424d0) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/GeoRedundant_DBForMariaDB_Audit.json) |

