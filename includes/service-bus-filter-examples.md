---
title: include file
description: include file
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 01/22/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7ddc0234accd9f434aad850d2404e2f3001c4bae
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98742681"
---
## <a name="examples"></a>Beispiele

### <a name="filter-on-system-properties"></a>Filter für Systemeigenschaften
Verwenden Sie das folgende Format, um in einem Filter auf eine Systemeigenschaft zu verweisen: `sys.<system-property-name>`. 

```csharp
sys.Label LIKE '%bus%'`
sys.messageid = 'xxxx'
sys.correlationid like 'abc-%'
```

### <a name="filter-on-message-properties"></a>Filter für Nachrichteneigenschaften
Hier sind die Beispiele für die Verwendung von Nachrichteneigenschaften in einem Filter angegeben. Sie können auf Nachrichteneigenschaften zugreifen, indem Sie `user.property-name` oder einfach `property-name` verwenden.

```csharp
MessageProperty = 'A'
SuperHero like 'SuperMan%'
```

### <a name="filter-on-message-properties-with-special-characters"></a>Filter für Nachrichteneigenschaften mit Sonderzeichen
Wenn der Name der Nachrichteneigenschaft Sonderzeichen enthält, sollten Sie den Namen in doppelte Anführungszeichen (`"`) setzen. Verwenden Sie im Filter beispielsweise die folgende Syntax, wenn der Eigenschaftsname `"http://schemas.microsoft.com/xrm/2011/Claims/EntityLogicalName"` lautet. 

```csharp
"http://schemas.microsoft.com/xrm/2011/Claims/EntityLogicalName" = 'account'
```

### <a name="filter-on-message-properties-with-numeric-values"></a>Filter für Nachrichteneigenschaften mit numerischen Werten
In den folgenden Beispielen wird veranschaulicht, wie Sie Eigenschaften mit numerischen Werten in Filtern verwenden können. 

```csharp
MessageProperty = 1
MessageProperty > 1
MessageProperty > 2.08
MessageProperty = 1 AND MessageProperty2 = 3
MessageProperty = 1 OR MessageProperty2 = 3
```

### <a name="parameter-based-filters"></a>Parameterbasierte Filter
Hier sind einige Beispiele für die Verwendung von parameterbasierten Filtern angegeben. In diesen Beispielen ist `DataTimeMp` eine Nachrichteneigenschaft vom Typ `DateTime` und `@dtParam` ein Parameter, der als `DateTime`-Objekt an den Filter übergeben wird.

```csharp
DateTimeMp < @dtParam
DateTimeMp > @dtParam

(DateTimeMp2-DateTimeMp1) <= @timespan //@timespan is a parameter of type TimeSpan
DateTimeMp2-DateTimeMp1 <= @timespan
```

### <a name="using-in-and-not-in"></a>Verwenden von IN und NOT IN

```csharp
StoreId IN('Store1', 'Store2', 'Store3')"

sys.To IN ('Store5','Store6','Store7') OR StoreId = 'Store8'

sys.To NOT IN ('Store1','Store2','Store3','Store4','Store5','Store6','Store7','Store8') OR StoreId NOT IN ('Store1','Store2','Store3','Store4','Store5','Store6','Store7','Store8')
```

Ein C# Beispiel finden Sie im [Beispiel für Themenfilter auf GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Azure.Messaging.ServiceBus/BasicSendReceiveTutorialwithFilters).


### <a name="set-rule-action-for-a-sql-filter"></a>Festlegen der Regelaktion für einen SQL-Filter

```csharp
// instantiate the ManagementClient
this.mgmtClient = new ManagementClient(connectionString);

// create the SQL filter
var sqlFilter = new SqlFilter("source = @stringParam");

// assign value for the parameter
sqlFilter.Parameters.Add("@stringParam", "orders");

// instantiate the Rule = Filter + Action
var filterActionRule = new RuleDescription
{
    Name = "filterActionRule",
    Filter = sqlFilter,
    Action = new SqlRuleAction("SET source='routedOrders'")
};

// create the rule on Service Bus
await this.mgmtClient.CreateRuleAsync(topicName, subscriptionName, filterActionRule);
```

### <a name="correlation-filter-using-correlationid"></a>Korrelationsfilter mithilfe der CorrelationID

```csharp
new CorrelationFilter("Contoso");
```

Filtert Nachrichten, bei denen `CorrelationID` auf `Contoso` festgelegt ist 

### <a name="correlation-filter-using-system-and-user-properties"></a>Korrelationsfilter mithilfe von System- und Benutzereigenschaften

```csharp
var filter = new CorrelationFilter();
filter.Label = "Important";
filter.ReplyTo = "johndoe@contoso.com";
filter.Properties["color"] = "Red";
```

Entspricht `sys.ReplyTo = 'johndoe@contoso.com' AND sys.Label = 'Important' AND color = 'Red'`

