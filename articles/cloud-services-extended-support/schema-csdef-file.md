---
title: Azure Cloud Services (erweiterter Support) – Definitionsschema (CSDEF-Datei) | Microsoft-Dokumentation
description: Informationen zum Definitionsschema (CSDEF) für Cloud Services (erweiterter Support)
ms.topic: article
ms.service: cloud-services-extended-support
ms.date: 10/14/2020
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: d9bf1b54f1bfeebacbb406a50c8496817857204c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102507567"
---
# <a name="azure-cloud-services-extended-support-definition-schema-csdef-file"></a>Azure Cloud Services (erweiterter Support) – Definitionsschema (CSDEF-Datei)

Die Dienstdefinitionsdatei definiert das Dienstmodell für eine Anwendung. Die Datei enthält die Definitionen für die Rollen, die für einen Clouddienst verfügbar sind, gibt die Anbieterendpunkte an und legt Konfigurationseinstellungen für den Dienst fest. Konfigurationseinstellungswerte werden in der Dienstkonfigurationsdatei festgelegt, wie im [Clouddienst-Konfigurationsschema (erweiterter Support)](schema-cscfg-file.md) beschrieben.

Standardmäßig wird die Konfigurationsschemadatei der Azure-Diagnose im `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas`-Verzeichnis installiert. Ersetzen Sie `<version>` durch die installierte Version des [Azure SDK](https://www.windowsazure.com/develop/downloads/).

Die Standarderweiterung für die Dienstdefinitionsdatei lautet CSDEF.

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
| topologyChangeDiscovery | Optional. Gibt die Änderungsbenachrichtigung zur Art der Topologie an. Dabei sind folgende Werte möglich:<br /><br /> -   `Blast`: Sendet das Update so schnell wie möglich an alle Rolleninstanzen. Wenn Sie die Option auswählen, sollte die Rolle das Topologieupdate verarbeiten können, ohne neu gestartet werden zu müssen.<br />-   `UpgradeDomainWalk`: Sendet das Update sequenziell an alle Rolleninstanzen, nachdem die vorherige Instanz das Update erfolgreich akzeptiert hat.|
| schemaVersion           | Optional. Gibt die Version des Dienstdefinitionsschemas an. Die Schemaversion ermöglicht Visual Studio, die richtigen SDK-Tools für die Schemaüberprüfung auszuwählen, wenn mehrere Versionen des SDK nebeneinander installiert sind.|
| upgradeDomainCount      | Optional. Gibt die Anzahl von Upgradedomänen an, über die die Rollen in diesem Dienst zugeordnet werden. Rolleninstanzen werden bei der Bereitstellung des Diensts einer Upgradedomäne zugeordnet. Weitere Informationen finden Sie unter [Aktualisieren einer Clouddienstrolle oder -bereitstellung](sample-update-cloud-service.md) und [Verwalten der Verfügbarkeit virtueller Windows-Computer in Azure](../virtual-machines/availability.md). Sie können bis zu 20 Upgradedomänen angeben. Bei fehlender Angabe wird der Standardwert in Höhe von 5 Upgradedomänen verwendet.|

## <a name="see-also"></a>Siehe auch

[Azure Cloud Services (erweiterter Support) – Konfigurationsschema (CSCFG-Datei)](schema-cscfg-file.md).