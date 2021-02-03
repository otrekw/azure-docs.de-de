---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/29/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 88945738b33cb6d1d7760eec1015a006132d3c3f
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179840"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure HDInsight-Cluster müssen kundenseitig verwaltete Schlüssel zur Verschlüsselung ruhender Daten verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F64d314f6-6062-4780-a861-c23e8951bee5) |Verwenden Sie kundenseitig verwaltete Schlüssel (Customer Managed Keys, CMKs), um die Verschlüsselung ruhender Daten Ihrer Azure HDInsight-Cluster zu verwalten. Standardmäßig werden Kundendaten mit dienstseitig verwalteten Schlüsseln verschlüsselt. CMKs sind jedoch häufig zur Einhaltung gesetzlicher Bestimmungen erforderlich. Mit CMKs können die Daten mit einem Azure Key Vault-Schlüssel verschlüsselt werden, der von Ihnen erstellt wird und sich in Ihrem Besitz befindet. Sie verfügen über die volle Kontrolle über und Verantwortung für den Schlüssellebenszyklus, einschließlich Rotation und Verwaltung. Weitere Informationen zur CMK-Verschlüsselung finden Sie unter [https://aka.ms/hdi.cmk](https://aka.ms/hdi.cmk).  |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/HDInsight/HDInsight_CMK_Audit.json) |
|[Azure HDInsight-Cluster müssen Verschlüsselung auf dem Host zur Verschlüsselung ruhender Daten verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1fd32ebd-e4c3-4e13-a54a-d7422d4d95f6) |Durch das Aktivieren der Verschlüsselung auf dem Host können Sie Ihre Daten schützen, um die Sicherheits- und Konformitätsverpflichtungen Ihrer Organisation zu erfüllen. Wenn Sie die Verschlüsselung auf dem Host aktivieren, werden die auf dem VM-Host gespeicherten Daten ruhend verschlüsselt und verschlüsselt an den Speicherdienst übermittelt. |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/HDInsight/HDInsight_EncryptionAtHost_Audit.json) |
|[Azure HDInsight-Cluster müssen Verschlüsselung während der Übertragung für die Verschlüsselung der Kommunikation zwischen Azure HDInsight-Clusterknoten verwenden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd9da03a1-f3c3-412a-9709-947156872263) |Daten können während der Übertragung zwischen Azure HDInsight-Clusterknoten manipuliert werden. Zur Vermeidung von Missbrauch und Manipulation können die Daten durch Aktivieren der Verschlüsselung während der Übertragung verschlüsselt werden. |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/HDInsight/HDInsight_EncryptionInTransit_Audit.json) |
