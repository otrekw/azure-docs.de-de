---
title: Azure Cloud Services-Definitionsschema (CSDEF-Datei) | Microsoft-Dokumentation
description: In einer Dienstdefinitionsdatei (.csdef) wird ein Dienstmodell für eine Anwendung definiert, das verfügbare Rollen, Endpunkte und Konfigurationswerte für den Dienst enthält.
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 42
author: tgore03
ms.author: tagore
ms.openlocfilehash: dadb50bd0663f47e6a1bf3d58b5187c8b466964d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79528369"
---
# <a name="azure-cloud-services-definition-schema-csdef-file"></a>Azure Cloud Services-Definitionsschema (.CSDEF-Datei)
Die Dienstdefinitionsdatei definiert das Dienstmodell für eine Anwendung. Die Datei enthält die Definitionen für die Rollen, die für einen Clouddienst verfügbar sind, gibt die Anbieterendpunkte an und legt Konfigurationseinstellungen für den Dienst fest. Konfigurationseinstellungswerte werden in der Dienstkonfigurationsdatei festgelegt, wie im [Clouddienst-Konfigurationsschema (klassisch)](/previous-versions/azure/reference/ee758710(v=azure.100)) beschrieben.

Standardmäßig wird die Konfigurationsschemadatei der Azure-Diagnose im `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas`-Verzeichnis installiert. Ersetzen Sie `<version>` durch die installierte Version des [Azure SDK](https://www.windowsazure.com/develop/downloads/).

Die Standarderweiterung für die Dienstdefinitionsdatei lautet „.csdef“.

## <a name="basic-service-definition-schema"></a>Basisdienstdefinitionsschema
Die Dienstdefinitionsdatei muss ein `ServiceDefinition`-Element enthalten. Die Dienstdefinition muss mindestens ein Rollenelement enthalten (`WebRole` oder `WorkerRole`). Sie kann bis zu 25 Rollen enthalten, die in einer Definition definiert sind. Rollentypen können miteinander kombiniert werden. Zudem enthält die Dienstdefinition das optionale Element `NetworkTrafficRules`, mit dem eingeschränkt werden kann, welche Rollen mit angegebenen internen Endpunkten kommunizieren können. Die Dienstdefinition enthält auch das optionale Element `LoadBalancerProbes`, das vom Kunden definierte Integritätstests von Endpunkten enthält.

Das Standardformat der Dienstdefinitionsdatei sieht wie folgt aus.

```xml
<ServiceDefinition name="<service-name>" topologyChangeDiscovery="<change-type>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" upgradeDomainCount="<number-of-upgrade-domains>" schemaVersion="<version>">
  
  <LoadBalancerProbes>
         …
  </LoadBalancerProbes>
  
  <WebRole …>
         …
  </WebRole>
  
  <WorkerRole …>
         …
  </WorkerRole>
  
  <NetworkTrafficRules>
         …
  </NetworkTrafficRules>

</ServiceDefinition>
```

## <a name="schema-definitions"></a>Schemadefinitionen
In den folgenden Themen wird das Schema beschrieben:

- [LoadBalancerProbe-Schema](schema-csdef-loadbalancerprobe.md)
- [WebRole-Schema](schema-csdef-webrole.md)
- [WorkerRole-Schema](schema-csdef-workerrole.md)
- [NetworkTrafficRules-Schema](schema-csdef-networktrafficrules.md)

##  <a name="servicedefinition-element"></a><a name="ServiceDefinition"></a> Element ServiceDefinition
Das Element `ServiceDefinition` ist das Element der obersten Ebene der Dienstdefinitionsdatei.

In der folgenden Tabelle werden die Attribute des Elements `ServiceDefinition` beschrieben.

| attribute               | BESCHREIBUNG |
| ----------------------- | ----------- |
| name                    |Erforderlich. Der Name des Diensts. Der Name muss innerhalb des Dienstkontos eindeutig sein.|
| topologyChangeDiscovery | Optional. Gibt die Änderungsbenachrichtigung zur Art der Topologie an. Mögliche Werte:<br /><br /> -   `Blast`: Sendet das Update so schnell wie möglich an alle Rolleninstanzen. Wenn Sie die Option auswählen, sollte die Rolle das Topologieupdate verarbeiten können, ohne neu gestartet werden zu müssen.<br />-   `UpgradeDomainWalk`: Sendet das Update sequenziell an alle Rolleninstanzen, nachdem die vorherige Instanz das Update erfolgreich akzeptiert hat.|
| schemaVersion           | Optional. Gibt die Version des Dienstdefinitionsschemas an. Die Schemaversion ermöglicht Visual Studio, die richtigen SDK-Tools für die Schemaüberprüfung auszuwählen, wenn mehrere Versionen des SDK nebeneinander installiert sind.|
| upgradeDomainCount      | Optional. Gibt die Anzahl von Upgradedomänen an, über die die Rollen in diesem Dienst zugeordnet werden. Rolleninstanzen werden bei der Bereitstellung des Diensts einer Upgradedomäne zugeordnet. Weitere Informationen finden Sie unter [Aktualisieren einer Clouddienstrolle oder -bereitstellung](cloud-services-how-to-manage-portal.md#update-a-cloud-service-role-or-deployment), [Verwalten der Verfügbarkeit virtueller Windows-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) und [Was ist das Clouddienstmodell, und wie kann es gepackt werden?](https://docs.microsoft.com/azure/cloud-services/cloud-services-model-and-package).<br /><br /> Sie können bis zu 20 Upgradedomänen angeben. Bei fehlender Angabe wird der Standardwert in Höhe von 5 Upgradedomänen verwendet.|
