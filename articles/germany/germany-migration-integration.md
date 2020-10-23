---
title: Migrieren von Azure-Integrationsressourcen – Azure Deutschland zu Azure weltweit
description: Dieser Artikel enthält Informationen zum Migrieren von Azure-Integrationsressourcen von Azure Deutschland zu Azure weltweit.
ms.topic: article
ms.date: 10/16/2020
author: gitralf
ms.author: ralfwi
ms.service: germany
ms.custom: bfmigrate
ms.openlocfilehash: 764eeaf203e58bff0737b0d32ddbf0b6f06bcd73
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92126747"
---
# <a name="migrate-integration-resources-to-global-azure"></a>Migrieren von Integrationsressourcen zu Azure weltweit

[!INCLUDE [closureinfo](../../includes/germany-closure-info.md)]

Dieser Artikel enthält Informationen dazu, wie Sie Azure-Integrationsressourcen von Azure Deutschland zu Azure weltweit migrieren können.

## <a name="service-bus"></a>Service Bus

Azure Service Bus-Dienste haben keine Datenexport- und -importfunktionen. Um Service Bus-Ressourcen von Azure Deutschland zu Azure weltweit zu migrieren, können Sie die Ressourcen [als eine Azure Resource Manager-Vorlage](../azure-resource-manager/templates/export-template-portal.md) exportieren. Passen Sie dann die exportierte Vorlage für Azure weltweit an, und erstellen Sie die Ressourcen erneut.

> [!NOTE]
> Bei einem Exportieren einer Resource Manager-Vorlage werden die Daten (z. B. Nachrichten) nicht kopiert. Bei einem Exportieren einer Vorlage werden nur die Metadaten neu erstellt.

> [!IMPORTANT]
> Ändern Sie den Standort, Azure Key Vault-Geheimnisse, Zertifikate und andere GUIDs, damit diese konsistent mit der neuen Region sind.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="service-bus-metadata"></a>Service Bus-Metadaten

Die folgenden Service Bus-Metadatenelemente werden neu erstellt, wenn Sie eine Resource Manager-Vorlage exportieren:

- Namespaces
- Warteschlangen
- Themen
- Abonnements
- Regeln
- Autorisierungsregeln

### <a name="keys"></a>Schlüssel

In den vorherigen Schritten zum Exportieren und erneuten Erstellen werden die Shared Access Signature-Schlüssel, die Autorisierungsregeln zugeordnet sind, nicht kopiert. Wenn Sie die Shared Access Signature-Schlüssel beibehalten müssen, verwenden Sie das Cmdlet `New-AzServiceBuskey` mit dem optionalen Parameter `-Keyvalue`, um den Schlüssel als Zeichenfolge zu übernehmen. Das aktualisierte Cmdlet ist im [Az-Modul von Azure PowerShell](/powershell/azure/install-az-ps) verfügbar.

### <a name="usage-example"></a>Verwendungsbeispiel

```powershell
New-AzServiceBuskey -ResourceGroupName <resourcegroupname> -Namespace <namespace> -Name <name of Authorization rule> -RegenerateKey <PrimaryKey/SecondaryKey> -KeyValue <string - key value>
```

```powershell
New-AzServiceBuskey -ResourceGroupName <resourcegroupname> -Namespace <namespace> -Queue <queuename> -Name <name of Authorization rule> -RegenerateKey <PrimaryKey/SecondaryKey> -KeyValue <string - key value>
```

```powershell
New-AzServiceBuskey -ResourceGroupName <resourcegroupname> -Namespace <namespace> -Topic <topicname> -Name <name of Authorization rule> -RegenerateKey <PrimaryKey/SecondaryKey> -KeyValue <string - key value>
```

> [!NOTE]
> Sie müssen Ihre Anwendungen so aktualisieren, dass sie eine neue Verbindungszeichenfolge verwenden, und zwar selbst dann, wenn Sie die Schlüssel beibehalten. DNS-Hostnamen sind in Azure Deutschland und Azure weltweit unterschiedlich.

### <a name="sample-connection-strings"></a>Beispielverbindungszeichenfolgen

**Azure Deutschland**

```cmd
Endpoint=sb://myBFProdnamespaceName.**servicebus.cloudapi.de**/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXx=
```

**Globale Azure-Umgebung**

```cmd
Endpoint=sb://myProdnamespaceName.**servicebus.windows.net**/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXx=
```

Weitere Informationen finden Sie unter:

- Frischen Sie Ihre Kenntnisse auf, indem Sie die [Tutorials zu Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/) durcharbeiten.
- Machen Sie sich damit vertraut, wie Sie [Resource Manager-Vorlagen exportieren](../azure-resource-manager/templates/export-template-portal.md), oder lesen Sie die Übersicht zu [Azure Resource Manager](../azure-resource-manager/management/overview.md).
- Lesen Sie die [Übersicht über Service Bus](../service-bus-messaging/service-bus-messaging-overview.md).

## <a name="logic-apps"></a>Logic Apps

Azure Logic Apps ist in Azure Deutschland nicht verfügbar. Sie können aber Planungsaufträge stattdessen mithilfe von Logic Apps im globalen Azure erstellen. Azure Scheduler war zwar in Azure Deutschland verfügbar, wird jedoch eingestellt.

Weitere Informationen finden Sie unter:

- Erfahren Sie mehr, indem Sie die [Tutorials zu Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/tutorial-build-schedule-recurring-logic-app-workflow) durcharbeiten.
- Lesen Sie [Was ist Azure Logic Apps?](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Tools, Techniken und Empfehlungen zum Migrieren von Ressourcen in den folgenden Dienstkategorien:

- [Compute](./germany-migration-compute.md)
- [Netzwerk](./germany-migration-networking.md)
- [Storage](./germany-migration-storage.md)
- [Web](./germany-migration-web.md)
- [Datenbanken](./germany-migration-databases.md)
- [Analyse](./germany-migration-analytics.md)
- [IoT](./germany-migration-iot.md)
- [Identität](./germany-migration-identity.md)
- [Security](./germany-migration-security.md)
- [Verwaltungstools](./germany-migration-management-tools.md)
- [Medien](./germany-migration-media.md)
