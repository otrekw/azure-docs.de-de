---
title: 'Azure Cloud Services (klassisch): Definition NetworkTrafficRules-Schema | Microsoft-Dokumentation'
description: Erfahren Sie mehr über NetworkTrafficRules, das die Rollen, die auf die internen Endpunkte einer Rolle zugreifen können, einschränkt. Es wird mit Rollen in einer Dienstdefinitionsdatei kombiniert.
ms.topic: article
ms.service: cloud-services
ms.date: 10/14/2020
ms.author: tagore
author: tanmaygore
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: 2c8ab53068b71652d03d03bf79a224fe5e34dff3
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98739767"
---
# <a name="azure-cloud-services-classic-definition-networktrafficrules-schema"></a>Azure Cloud Services (klassisch): Definition des NetworkTrafficRules-Schemas

> [!IMPORTANT]
> [Azure Cloud Services (erweiterter Support)](../cloud-services-extended-support/overview.md) ist ein neues auf Azure Resource Manager basierendes Bereitstellungsmodell für Azure Cloud Services. Im Zuge dieser Änderung wurden Azure Cloud Services-Instanzen, die unter dem Azure Service Manager-basierten Bereitstellungsmodell ausgeführt werden, in „Cloud Services (klassisch)“ umbenannt. Für alle neuen Bereitstellungen wird [Azure Cloud Services (erweiterter Support)](../cloud-services-extended-support/overview.md) verwendet.

Der `NetworkTrafficRules`-Knoten ist ein optionales Element in der Dienstdefinitionsdatei, das angibt, wie Rollen miteinander kommunizieren. Er begrenzt, welche Rollen auf die internen Endpunkte der bestimmten Rolle zugreifen können. Der `NetworkTrafficRules`-Knoten ist kein eigenständiges Element; er wird mit mindestens zwei Rollen in einer Dienstdefinitionsdatei kombiniert.

Die Standarderweiterung für die Dienstdefinitionsdatei lautet „.csdef“.

> [!NOTE]
>  Der `NetworkTrafficRules`-Knoten ist nur mit Azure SDK Version 1.3 und höher verfügbar.

## <a name="basic-service-definition-schema-for-the-network-traffic-rules"></a>Basisdienstdefinitions-Schema für die Netzwerkverkehrs-Regeln
Das Standardformat einer Dienstdefinitionsdatei, die Netzwerkverkehrs-Definitionen enthält, lautet wie folgt.

```xml
<ServiceDefinition …>
   <NetworkTrafficRules>
      <OnlyAllowTrafficTo>
         <Destinations>
            <RoleEndpoint endpointName="<name-of-the-endpoint>" roleName="<name-of-the-role-containing-the-endpoint>"/>
         </Destinations>
         <AllowAllTraffic/>
         <WhenSource matches="[AnyRule]">
            <FromRole roleName="<name-of-the-role-to-allow-traffic-from>"/>
         </WhenSource>
      </OnlyAllowTrafficTo>
   </NetworkTrafficRules>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Schema-Elemente
Der `NetworkTrafficRules`-Knoten der Dienstdefinitionsdatei enthält die folgenden Elemente, die in den folgenden Abschnitten in diesem Thema ausführlich beschrieben werden:

[NetworkTrafficRules-Element](#NetworkTrafficRules)

[OnlyAllowTrafficTo-Element](#OnlyAllowTrafficTo)

[Destinations-Element](#Destinations)

[RoleEndpoint-Element](#RoleEndpoint)

AllowAllTraffic-Element

[WhenSource-Element](#WhenSource)

[FromRole-Element](#FromRole)

##  <a name="networktrafficrules-element"></a><a name="NetworkTrafficRules"></a>NetworkTrafficRules-Element
Das `NetworkTrafficRules`-Element gibt an, welche Rollen mit welchem Endpunkt einer anderen Rolle kommunizieren können. Ein Dienst kann eine einzelne `NetworkTrafficRules`-Definition enthalten.

##  <a name="onlyallowtrafficto-element"></a><a name="OnlyAllowTrafficTo"></a>OnlyAllowTrafficTo-Element
Das `OnlyAllowTrafficTo`-Element beschreibt eine Sammlung der Zielendpunkte und der Rollen, die mit ihnen kommunizieren können. Sie können mehrere Feldzuordnungen `OnlyAllowTrafficTo`-Knoten angeben.

##  <a name="destinations-element"></a><a name="Destinations"></a>Destinations-Element
Das `Destinations`-Element beschreibt eine Sammlung von RoleEndpoints, mit denen kommuniziert werden kann.

##  <a name="roleendpoint-element"></a><a name="RoleEndpoint"></a> RoleEndpoint-Element
Das `RoleEndpoint`-Element beschreibt einen Endpunkt einer Rolle, um die Kommunikation mit ihm zuzulassen. Sie können mehrere `RoleEndpoint`-Elemente angeben, wenn die Rolle mehrere Endpunkte aufweist.

| Attribut      | type     | Beschreibung |
| -------------- | -------- | ----------- |
| `endpointName` | `string` | Erforderlich. Der Name des Endpunkts, mit dem Datenverkehr zugelassen ist.|
| `roleName`     | `string` | Erforderlich. Der Name der Webrolle, mit der Kommunikation zugelassen ist.|

## <a name="allowalltraffic-element"></a>AllowAllTraffic-Element
Das `AllowAllTraffic`-Element ist eine Regel, die allen Rollen die Kommunikation mit den im `Destinations`-Knoten definierten Endpunkten ermöglicht.

##  <a name="whensource-element"></a><a name="WhenSource"></a> WhenSource-Element
Das `WhenSource`-Element beschreibt eine Sammlung von Rollen, die mit den im `Destinations`-Knoten definierten Endpunkten kommunizieren können.

| Attribut | type     | Beschreibung |
| --------- | -------- | ----------- |
| `matches` | `string` | Erforderlich. Gibt die Regel an, die beim Zulassen von Kommunikation angewendet werden soll. Derzeit ist `AnyRule` der einzige gültige Wert.|
  
##  <a name="fromrole-element"></a><a name="FromRole"></a> FromRole-Element
Das `FromRole`-Element gibt die Rollen an, die mit den im `Destinations`-Knoten definierten Endpunkten kommunizieren können. Sie können mehrere `FromRole`-Elemente angeben, wenn mehrere Rollen mit den Endpunkten kommunizieren können.

| Attribut  | type     | Beschreibung |
| ---------- | -------- | ----------- |
| `roleName` | `string` | Erforderlich. Der Name für die Rolle, von der aus Kommunikation zugelassen ist.|

## <a name="see-also"></a>Weitere Informationen
[Clouddienst-Definitionsschema (klassisch)](schema-csdef-file.md)




