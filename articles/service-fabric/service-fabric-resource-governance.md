---
title: Ressourcenkontrolle für Container und Dienste
description: Azure Service Fabric ermöglicht Ihnen, Ressourcenanforderungen und -grenzwerte für Dienste festzulegen, die als Prozesse oder Container ausgeführt werden.
ms.topic: conceptual
ms.date: 8/9/2017
ms.openlocfilehash: d760766870c8c2be0a2d2384f6d012b75bc92fbd
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101735657"
---
# <a name="resource-governance"></a>Ressourcengovernance

Wenn Sie mehrere Dienste auf dem gleichen Knoten oder Cluster ausführen, kann ein Dienst mehr Ressourcen belegen und dadurch die Verfügbarkeit anderer Dienste im Prozess beeinträchtigen. Dieses Problem wird als „Noisy-Neighbor“-Problem bezeichnet. Azure Service Fabric ermöglicht es dem Entwickler, dieses Verhalten zu steuern, indem Anforderungen und Grenzwerte pro Dienst festgelegt werden, um die Ressourcennutzung zu begrenzen.

> Bevor Sie mit diesem Artikel fortfahren, sollten Sie sich mit dem [Service Fabric-Anwendungsmodell][application-model-link] und dem [Service Fabric-Hostingmodell][hosting-model-link] vertraut machen.
>

## <a name="resource-governance-metrics"></a>Ressourcenkontrollmetriken

Die Ressourcenkontrolle wird in Service Fabric gemäß dem [Dienstpaket][application-model-link] unterstützt. Die Ressourcen, die dem Dienstpaket zugewiesen sind, können weiter auf Codepakete aufgeteilt werden. Service Fabric unterstützt CPU- und Arbeitsspeicherkontrolle pro Dienstpaket mithilfe von zwei integrierten [Metriken](service-fabric-cluster-resource-manager-metrics.md):

* *CPU* (Metrikname `servicefabric:/_CpuCores`): Ein logischer Kern, der auf dem Hostcomputer verfügbar ist. Sämtliche Kerne in allen Knoten werden identisch gewichtet.

* *Arbeitsspeicher* (Metrikname `servicefabric:/_MemoryInMB`): Arbeitsspeicher wird in MB angegeben und entspricht dem auf dem Computer verfügbaren physischen Arbeitsspeicher.

Für diese beiden Metriken werden vom [Clusterressourcen-Manager (CRM)][cluster-resource-manager-description-link] die gesamte Clusterkapazität, die Last auf jedem Knoten im Cluster und die verbleibenden Ressourcen im Cluster nachverfolgt. Diese zwei Metriken entsprechen einem Benutzer oder einer benutzerdefinierten Metrik. Alle vorhandenen Features können mit diesen verwendet werden:

* Der Cluster kann gemäß diesen beiden Metriken [ausbalanciert](service-fabric-cluster-resource-manager-balancing.md) werden (Standardverhalten).
* Der Cluster kann gemäß diesen beiden Metriken [defragmentiert](service-fabric-cluster-resource-manager-defragmentation-metrics.md) werden.
* Beim [Beschreiben eines Clusters][cluster-resource-manager-description-link] kann für diese beiden Metriken gepufferte Kapazität festgelegt werden.

> [!NOTE]
> [Dynamische Auslastungsberichte](service-fabric-cluster-resource-manager-metrics.md) werden für diese Metriken nicht unterstützt, und Lasten für diese Metriken werden zum Zeitpunkt der Erstellung definiert.

## <a name="resource-governance-mechanism"></a>Ressourcenkontrollmechanismus

Ab Version 7.2 unterstützt Service Fabric Runtime die Angabe von Anforderungen und Grenzwerten für CPU- und Arbeitsspeicherressourcen.

> [!NOTE]
> Service Fabric-Runtimeversionen, die älter als Version 7.2 sind, unterstützen nur ein Modell, bei dem ein einzelner Wert sowohl als **Anforderung** als auch als **Grenzwert** für eine bestimmte Ressource (CPU oder Arbeitsspeicher) dient. Dies wird als **RequestsOnly**-Spezifikation in diesem Dokument beschrieben.

* *Anforderungen*: CPU- und Arbeitsspeicher-Anforderungswerte stellen die Lasten dar, die vom [Clusterressourcen-Manager (CRM)][cluster-resource-manager-description-link] für die `servicefabric:/_CpuCores`- und `servicefabric:/_MemoryInMB`-Metriken verwendet werden. Mit anderen Worten: CRM betrachtet den Ressourcenverbrauch eines Dienstanbieter so, dass er seinen Anforderungswerten entspricht, und verwendet diese Werte, wenn Platzierungsentscheidungen getroffen werden.

* *Grenzwerte:* Werte für CPU- und Arbeitsspeichergrenzwerte stellen die tatsächlichen Ressourcengrenzwerte dar, die angewendet werden, wenn ein Prozess oder ein Container auf einem Knoten aktiviert wird.

Service Fabric ermöglicht **RequestsOnly, LimitsOnly** und **RequestsAndLimits**-Spezifikationen für CPU und Arbeitsspeicher.
* Wenn die RequestsOnly-Spezifikation verwendet wird, verwendet Service Fabric die Anforderungswerte auch als Grenzwerte.
* Wenn die LimitsOnly-Spezifikation verwendet wird, nimmt Service Fabric 0 für Anforderungswerte an.
* Wenn die RequestsAndLimits-Spezifikation verwendet wird, müssen die Grenzwerte größer oder gleich den Anforderungswerten sein.

Um den Ressourcenkontrollmechanismus besser zu verstehen, betrachten wir ein Beispiel für ein Platzierungsszenario mit einer **RequestsOnly**-Spezifikation für die CPU-Ressource (der Mechanismus für Speicherkontrolle ist äquivalent). Stellen Sie sich einen Knoten mit zwei CPU-Kernen und zwei Dienstpaketen vor, die darauf platziert werden. Das erste Dienstpaket, das platziert werden soll, besteht aus nur einem Containercodepaket und gibt nur eine Anforderung eines CPU-Kerns an. Das zweite Dienstpaket, das platziert werden soll, besteht aus nur einem prozessbasierten Codepaket und gibt auch nur eine Anforderung eines CPU-Kerns an. Da beide Dienstpakete eine RequestsOnly-Spezifikation aufweisen, werden ihre Grenzwerte auf Ihre Anforderungswerte festgelegt.

1. Zuerst wird das containerbasierte Dienstpaket, das einen CPU-Kern anfordert, auf dem Knoten platziert. Die Runtime aktiviert den Container und legt den CPU-Grenzwert auf einen Kern fest. Der Container kann nicht mehr als einen Kern verwenden.

2. Dann wird das prozessbasierte Dienstpaket, das einen CPU-Kern anfordert, auf dem Knoten platziert. Die Runtime aktiviert den Dienstprozess und legt seinen CPU-Grenzwert auf einen Kern fest.

An dieser Stelle entspricht die Summe der Anforderungen der Kapazität des Knotens. Von CRM werden keine weiteren Container oder Dienstprozesse mit CPU-Anforderungen auf diesem Knoten platziert. Auf dem Knoten werden ein Prozess und ein Container mit jeweils einem Kern ausgeführt und konkurrieren nicht miteinander um die CPU.

Kehren wir nun zum Beispiel mit einer **RequestsAndLimits**-Spezifikation zurück. Dieses Mal gibt das containerbasierte Dienstpaket eine Anforderung eines CPU-Kerns und einen Grenzwert von zwei CPU-Kernen an. Das prozessbasierte Dienstpaket gibt sowohl eine Anforderung als auch einen Grenzwert von einem CPU-Kern an.
  1. Zuerst wird das containerbasierte Dienstpaket auf dem Knoten platziert. Die Runtime aktiviert den Container und legt den CPU-Grenzwert auf zwei Kerne fest. Der Container kann nicht mehr als zwei Kerne verwenden.
  2. Dann wird das prozessbasierte Dienstpaket auf dem Knoten platziert. Die Runtime aktiviert den Dienstprozess und legt seinen CPU-Grenzwert auf einen Kern fest.

  An diesem Punkt ist die Summe der CPU-Anforderungen von Dienstpaketen, die auf dem Knoten platziert werden, gleich der CPU-Kapazität des Knotens. Von CRM werden keine weiteren Container oder Dienstprozesse mit CPU-Anforderungen auf diesem Knoten platziert. Auf dem Knoten überschreitet jedoch die Summe der Grenzwerte (zwei Kerne für den Container und ein Kern für den Prozess) die Kapazität von zwei Kernen. Wenn der Container und der Prozess gleichzeitig einen Burst ausführen, besteht die Möglichkeit eines Konflikts für die CPU-Ressource. Solche Konflikte werden durch das zugrunde liegende Betriebssystem für die Plattform verwaltet. In diesem Beispiel kann der Container für bis zu zwei CPU-Kerne einen Burst ausführen, was dazu führt, dass die Anforderung eines CPU-Kerns des Prozesses nicht garantiert wird.

> [!NOTE]
> Wie im vorherigen Beispiel gezeigt, führen die Anforderungswerte für CPU und Arbeitsspeicher **nicht zur Reservierung von Ressourcen auf einem Knoten**. Diese Werte stellen den Ressourcenverbrauch dar, den der Clusterressourcen-Manager bei Platzierungsentscheidungen berücksichtigt. Grenzwerte stellen die tatsächlichen Ressourcengrenzwerte dar, die angewendet werden, wenn ein Prozess oder ein Container auf einem Knoten aktiviert wird.


Es gibt einige Situationen, in denen es zu Konflikten bei der CPU kommen kann. In diesen Fällen kann beim Prozess und Container aus unserem Beispiel das „Noisy Neighbor“-Problem auftreten:

* *Kombinieren von gesteuerten und nicht gesteuerten Diensten und Containern:* Wenn ein Benutzer einen Dienst erstellt, ohne eine Ressourcenkontrolle anzugeben, betrachtet die Laufzeit diesen so, als würde er keine Ressourcen in Anspruch nehmen, und kann ihn in unserem Beispiel auf dem Knoten platzieren. In diesem Fall verbraucht dieser neue Prozess einen Teil der CPU-Leistung, der den bereits auf dem Knoten ausgeführten Diensten dann fehlt. Für dieses Problem gibt es zwei Lösungen. Entweder können Sie vermeiden, nicht gesteuerte und gesteuerte Dienste im selben Cluster zu verwenden, oder Sie verwenden [Platzierungseinschränkungen](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md), damit diese beiden Arten von Diensten nicht im selben Knotensatz platziert werden.

* *Wenn ein anderer Prozess auf dem Knoten außerhalb von Service Fabric gestartet wird (etwa ein Betriebssystemdienst):* In diesem Fall konkurriert dieser Prozess außerhalb von Service Fabric ebenfalls mit den vorhandenen Diensten um CPU-Leistung. Zum Beheben dieses Problems können Sie Knotenkapazitäten einrichten, um den Mehraufwand des Betriebssystems abzufangen. Dieses Vorgehen wird im nächsten Abschnitt beschrieben.

* *Wenn Anforderungen nicht gleich den Grenzwerten* sind: Wie im RequestsAndLimits-Beispiel weiter oben beschrieben, führen Anforderungen nicht zu einer Reservierung von Ressourcen auf einem Knoten. Wenn ein Dienst mit Grenzwerten, die größer als Anforderungen sind, auf einem Knoten platziert werden, werden möglicherweise Ressourcen (sofern verfügbar) bis zu den Grenzwerten genutzt. In solchen Fällen sind andere Dienste auf dem Knoten möglicherweise nicht in der Lage, Ressourcen bis zu Ihren Anforderungswerten zu verbrauchen.

## <a name="cluster-setup-for-enabling-resource-governance"></a>Clustereinrichtung zum Aktivieren der Ressourcenkontrolle

Wenn ein Knoten gestartet wird und dem Cluster beitritt, erkennt Service Fabric den verfügbaren Arbeitsspeicher und die verfügbare Anzahl an Kernen und legt dann die Knotenkapazitäten für diese beiden Ressourcen fest.

Um Pufferspeicher für das Betriebssystem und andere Prozesse, die auf dem Knoten ausgeführt werden könnten, zu behalten, nutzt Service Fabric nur 80 % der verfügbaren Ressourcen auf dem Knoten. Dieser Prozentsatz ist konfigurierbar und kann im Clustermanifest geändert werden.

Mit dem folgenden Beispiel weisen Sie Service Fabric an, 50 % der verfügbaren CPU-Leistung und 70 % des verfügbaren Arbeitsspeichers zu verwenden:

```xml
<Section Name="PlacementAndLoadBalancing">
    <!-- 0.0 means 0%, and 1.0 means 100%-->
    <Parameter Name="CpuPercentageNodeCapacity" Value="0.5" />
    <Parameter Name="MemoryPercentageNodeCapacity" Value="0.7" />
</Section>
```

Für die meisten Kunden und Szenarios ist die automatische Erkennung von Knotenkapazitäten für CPU und Arbeitsspeicher die empfohlene Konfiguration (die automatische Erkennung ist standardmäßig aktiviert). Wenn jedoch eine vollständig manuelle Einrichtung der Knotenkapazitäten erforderlich ist, können Sie diese pro Knotentyp mithilfe der Mechanismen zum Beschreiben der Knoten im Cluster beschreiben. Nachfolgend finden Sie ein Beispiel für das Einrichten des Knotentyps mit vier Kernen und 2 GB Arbeitsspeicher:

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="servicefabric:/_CpuCores" Value="4"/>
        <Capacity Name="servicefabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```

Wenn die automatische Erkennung der verfügbaren Ressourcen aktiviert ist und die Knotenkapazitäten manuell im Clustermanifest definiert werden, überprüft Service Fabric, ob der Knoten über genügend Ressourcen für die vom Benutzer definierten Kapazität verfügt:

* Wenn die im Manifest definierten Knotenkapazitäten geringer sind als die verfügbaren Ressourcen auf dem Knoten oder mit diesen identisch sind, verwendet Service Fabric die im Manifest angegebenen Kapazitäten.

* Wenn die im Manifest definierten Knotenkapazitäten größer sind als die verfügbaren Ressourcen, verwendet Service Fabric die verfügbaren Ressourcen als Knotenkapazitäten.

Die automatische Erkennung der verfügbaren Ressourcen kann deaktiviert werden, sofern diese nicht erforderlich ist. Um diese Funktion zu deaktivieren, ändern Sie die folgende Einstellung:

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="AutoDetectAvailableResources" Value="false" />
</Section>
```

Für eine optimale Leistung sollte auch die folgende Einstellung im Clustermanifest aktiviert werden:

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" />
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```

> [!IMPORTANT]
> Mit Service Fabric Version 7.0 haben wir die Regel zur Berechnung von Knotenressourcenkapazitäten in den Fällen aktualisiert, in denen der Benutzer die Werte für Knotenressourcenkapazitäten manuell bereitstellt. Stellen Sie sich folgendes Szenario vor:
>
> * Es gibt insgesamt 10 CPU-Kerne auf dem Knoten.
> * SF ist konfiguriert, um 80 % der Gesamtressourcen für die Benutzerdienste zu nutzen (Standardeinstellung), was einen Puffer von 20 % für die anderen auf dem Knoten ausgeführten Dienste (einschließlich Service Fabric-Systemdienste) lässt.
> * Der Benutzer beschließt, die Kapazität der Knotenressourcen für die Metrik der CPU-Kerne manuell außer Kraft zu setzen und legt sie auf fünf Kerne fest.
>
> Wir haben die Regel, wie die verfügbare Kapazität für Service Fabric-Benutzerdienste berechnet wird, auf die folgende Weise geändert:
>
> * Vor Service Fabric 7.0 hat die Berechnung der verfügbaren Kapazität für Benutzerdienste **5 Kerne** ergeben (Kapazitätspuffer von 20 % wird ignoriert).
> * Ab Service Fabric 7.0 ergibt die Berechnung der verfügbaren Kapazität für Benutzerdienste **4 Kerne** (Kapazitätspuffer von 20 % wird nicht ignoriert).

## <a name="specify-resource-governance"></a>Festlegen der Ressourcenkontrolle

Ressourcenkontrollanforderungen und -grenzwerte werden im Anwendungsmanifest (Abschnitt „ServiceManifestImport“) festgelegt. Hier einige Beispiele:

**Beispiel 1: RequestsOnly-Spezifikation**
```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='https://www.w3.org/2001/XMLSchema-instance'>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMB="1024" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMB="1024" />
    </Policies>
  </ServiceManifestImport>
```

In diesem Beispiel wird das `CpuCores`-Attribut verwendet, um eine Anforderung von einem CPU-Kern für **ServicePackageA** anzugeben. Da der CPU-Grenzwert (`CpuCoresLimit`-Attribut) nicht angegeben wird, verwendet Service Fabric auch den angegebenen Anforderungswert von einem Kern als CPU-Grenzwert für das Dienstpaket.

**ServicePackageA** wird nur auf einem Knoten platziert, bei dem die verbleibende CPU-Kapazität nach Subtraktion der **Summe der CPU-Anforderungen aller auf diesem Knoten platzierten Servicepakete** größer oder gleich ein Kern ist. Auf dem Knoten wird das Dienstpaket auf einen Kern beschränkt. Das Dienstpaket enthält zwei Codepakete (**CodeA1** und **CodeA2**), und in beiden ist das Attribut `CpuShares` festgelegt. Das Verhältnis der CpuShares 512:256 wird zum Berechnen der CPU-Grenzwerte für die einzelnen Codepakete verwendet. Daher ist CodeA1 auf zwei Drittel eines Kerns beschränkt, und CodeA2 wird auf ein Drittel eines Kerns beschränkt. Wenn nicht für alle Codepakete CpuShares angegeben werden, teilt Service Fabric den CPU-Grenzwert gleichmäßig auf die Codepakete auf.

Während die für Codepakete angegebenen CpuShares ihren relativen Anteil am CPU-Gesamtgrenzwert des Dienstpakets darstellen, werden die Arbeitsspeicherwerte für Codepakete in absoluten Zahlen angegeben. In diesem Beispiel wird das `MemoryInMB`-Attribut verwendet, um Arbeitsspeicheranforderungen von 1.024 MB für CodeA1 und CodeA2 anzugeben. Da der Arbeitsspeichergrenzwert (`MemoryInMBLimit`-Attribut) nicht angegeben wird, verwendet Service Fabric auch die angegebenen Anforderungswerte als Grenzwerte für die Dienstpakete. Die Arbeitsspeicheranforderung (und der -grenzwert) für das Dienstpaket wird als Summe der Werte für die Arbeitsspeicheranforderung (und des -grenzwerts) der zugehörigen Codepakete berechnet. Daher wird für **ServicePackageA** die Arbeitsspeicheranforderung und der -grenzwert als 2.048 MB berechnet.

**ServicePackageA** wird nur auf einem Knoten platziert, bei dem die verbleibende Arbeitsspeicherkapazität nach Subtraktion der **Summe der Arbeitsspeicheranforderungen aller auf diesem Knoten platzierten Servicepakete** größer oder gleich 2.048 MB ist. Auf dem Knoten werden beide Codepakete auf jeweils 1.024 MB Arbeitsspeicher beschränkt. Codepakete (Container oder Prozesse) können nicht mehr Arbeitsspeicher zuweisen, als dieser Grenzwert zulässt, ein entsprechender Versuch führt daher zu einer Ausnahme „Nicht genügend Arbeitsspeicher“.

**Beispiel 2: LimitsOnly-Spezifikation**
```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='https://www.w3.org/2001/XMLSchema-instance'>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCoresLimit="1"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMBLimit="1024" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMBLimit="1024" />
    </Policies>
  </ServiceManifestImport>
```
In diesem Beispiel werden die Attribute `CpuCoresLimit` und `MemoryInMBLimit` verwendet, die nur in den SF-Versionen 7.2 oder höher verfügbar sind. Das CpuCoresLimit-Attribut wird verwendet, um einen CPU-Grenzwert von 1 Kern für **ServicePackageA** anzugeben. Da die CPU-Anforderung (`CpuCores`-Attribut) nicht angegeben wird, wird sie als 0 angenommen. Das `MemoryInMBLimit`-Attribut wird verwendet, um Arbeitsspeichergrenzwerte von 1.024 MB für CodeA1 und CodeA2 anzugeben, und da Anforderungen (`MemoryInMB`-Attribut) nicht angegeben werden, werden sie als 0 betrachtet. Daher wird für **ServicePackageA** die Arbeitsspeicheranforderung und der -grenzwert als 0 MB bzw. 2.048 MB berechnet. Da sowohl die CPU- als auch die Arbeitsspeicheranforderungen für **ServicePackageA** gleich 0 sind, muss CRM keine Last für Platzierung für die `servicefabric:/_CpuCores`- und `servicefabric:/_MemoryInMB`-Metriken berücksichtigen. Aus Sicht der Ressourcenkontrolle kann daher **ServicePackageA** auf jedem Knoten platziert werden, und zwar **unabhängig von der verbleibenden Kapazität**. Ähnlich wie in Beispiel 1 ist CodeA1 auf dem Knoten auf zwei Drittel eines Kerns und 1.024 MB Arbeitsspeicher beschränkt, und CodeA2 wird auf ein Drittel eines Kerns und 1.024 MB Arbeitsspeicher beschränkt.

**Beispiel 3: RequestsAndLimits-Spezifikation**
```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='https://www.w3.org/2001/XMLSchema-instance'>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="1" CpuCoresLimit="2"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMB="1024" MemoryInMBLimit="3072" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMB="2048" MemoryInMBLimit="4096" />
    </Policies>
  </ServiceManifestImport>
```
Aufbauend auf den ersten beiden Beispielen zeigt dieses Beispiel die Angabe sowohl von Anforderungen als auch von Grenzwerten für CPU und Arbeitsspeicher. **ServicePackageA** weist CPU- und Arbeitsspeicheranforderungen von 1 Kern und 3.072 (1.024 + 2.048) MB auf. Es kann nur auf einem Knoten platziert werden, der nach Subtraktion der Summe aller CPU-Anforderungen (und Arbeitsspeicheranforderungen) aller Dienstpakete, die auf dem Knoten platziert sind, von der gesamten CPU-Kapazität (und Arbeitsspeicherkapazität) des Knotens noch mindestens 1 Kern (und 3.072 MB) Kapazität aufweist. CodeA1 ist auf dem Knoten auf zwei Drittel von 2 Kernen und 3.072 MB Arbeitsspeicher beschränkt, während CodeA2 auf ein Drittel von 2 Kernen und 4.096 MB Arbeitsspeicher beschränkt ist.

### <a name="using-application-parameters"></a>Verwenden von Anwendungsparametern

Beim Angeben der Einstellungen für die Ressourcenkontrolle können Sie [Anwendungsparameter](service-fabric-manage-multiple-environment-app-configuration.md) verwenden, um mehrere App-Konfigurationen zu verwalten. Das folgende Beispiel zeigt die Verwendung von Anwendungsparametern:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='https://www.w3.org/2001/XMLSchema-instance'>

  <Parameters>
    <Parameter Name="CpuCores" DefaultValue="4" />
    <Parameter Name="CpuSharesA" DefaultValue="512" />
    <Parameter Name="CpuSharesB" DefaultValue="512" />
    <Parameter Name="MemoryA" DefaultValue="2048" />
    <Parameter Name="MemoryB" DefaultValue="2048" />
  </Parameters>

  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="[CpuSharesA]" MemoryInMB="[MemoryA]" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="[CpuSharesB]" MemoryInMB="[MemoryB]" />
    </Policies>
  </ServiceManifestImport>
```

In diesem Beispiel werden Standardparameterwerte für die Produktionsumgebung festgelegt, in denen jedes Dienstpaket 4 Kerne und 2 GB Arbeitsspeicher erhalten würde. Es ist möglich, die Standardwerte mit Anwendungsparameterdateien zu ändern. In diesem Beispiel kann eine Parameterdatei für einen lokalen Test der Anwendung verwendet werden, bei dem diese weniger Ressourcen als in der Produktion erhält:

```xml
<!-- ApplicationParameters\Local.xml -->

<Application Name="fabric:/TestApplication1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="CpuCores" DefaultValue="2" />
    <Parameter Name="CpuSharesA" DefaultValue="512" />
    <Parameter Name="CpuSharesB" DefaultValue="512" />
    <Parameter Name="MemoryA" DefaultValue="1024" />
    <Parameter Name="MemoryB" DefaultValue="1024" />
  </Parameters>
</Application>
```

> [!IMPORTANT]
> Das Angeben der Ressourcenkontrolle mit Anwendungsparameter ist ab Service Fabric Version 6.1 möglich.<br>
>
> Wenn die Ressourcenkontrolle mithilfe von Anwendungsparameter angegeben wird, kann Service Fabric nicht auf eine Version vor 6.1 herabgestuft werden.

## <a name="enforcing-the-resource-limits-for-user-services"></a>Erzwingen der Resourceneinschränkungen für Benutzerdienste

Während die Anwendung der Ressourcenkontrolle auf Ihre Service Fabric-Dienste sicherstellt, dass diese ressourcengesteuerten Dienste ihr Ressourcenkontingent nicht überschreiten können, müssen viele Benutzer dennoch einige ihrer Service Fabric-Dienste im nicht verwalteten Modus ausführen. Bei der Verwendung von nicht verwalteten Service Fabric-Diensten ist es möglich, in Situationen zu geraten, in denen „endlose“ nicht verwaltete Dienste alle verfügbaren Ressourcen auf den Service Fabric-Knoten verbrauchen, was zu schwerwiegenden Problemen führen kann:

* Nicht ausreichend verfügbare Ressourcen von anderen Diensten, die auf den Knoten ausgeführt werden (einschließlich Service Fabric-Systemdienste).
* Knoten, die in einem fehlerhaften Zustand enden
* Nicht reagierende APIs für die Service Fabric-Clusterverwaltung

Zur Vermeidung dieser Situationen können Sie mit Service Fabric die *Ressourceneinschränkungen für alle Service Fabric-Benutzerdienste erzwingen, die auf dem Knoten ausgeführt werden* (sowohl verwaltet als auch nicht verwaltet), um sicherzustellen, dass die Benutzerdienste niemals mehr als die angegebene Menge an Ressourcen verwenden. Dies wird erreicht, indem der Wert für die Konfiguration „EnforceUserServiceMetricCapacities“ im Abschnitt „PlacementAndLoadBalancing“ des „ClusterManifest“ auf „true“ festgelegt wird. Diese Einstellung ist standardmäßig deaktiviert.

```xml
<SectionName="PlacementAndLoadBalancing">
    <ParameterName="EnforceUserServiceMetricCapacities" Value="false"/>
</Section>
```

Weitere Hinweise:

* Die Erzwingung von Ressourceneinschränkungen gilt nur für die Ressourcenmetriken `servicefabric:/_CpuCores` und `servicefabric:/_MemoryInMB`.
* Die Erzwingung der Ressourceneinschränkung funktioniert nur, wenn Knotenkapazitäten für die Ressourcenmetriken für Service Fabric verfügbar sind, entweder über einen automatischen Erkennungsmechanismus oder über Benutzer, die die Knotenkapazitäten manuell angeben (wie im Abschnitt [Clustereinrichtung zum Aktivieren der Ressourcenkontrolle](service-fabric-resource-governance.md#cluster-setup-for-enabling-resource-governance) erläutert). Wenn Knotenkapazitäten nicht konfiguriert sind, kann die Erzwingungsfunktion für Ressourceneinschränkungen nicht verwendet werden, da Service Fabric nicht weiß, wie viele Ressourcen für Benutzerdienste reserviert werden müssen. Service Fabric gibt eine Integritätswarnung aus, wenn „EnforceUserServiceMetricCapacities“ den Wert „true“ aufweist, aber die Knotenkapazitäten nicht konfiguriert sind.

## <a name="other-resources-for-containers"></a>Weitere Ressourcen für Container

Neben CPU und Arbeitsspeicher ist es möglich, noch andere Ressourcengrenzwerte für Container anzugeben. Diese Grenzwerte werden auf Codepaketebene angegeben und angewendet, wenn der Container gestartet wird. Anders als bei CPU und Arbeitsspeicher sind diese Ressourcen im Cluster Resource Manager nicht bekannt, sodass dieser weder die Kapazität überprüft noch einen Lastenausgleich für sie ausführt.

* *MemorySwapInMB:* die Menge an Auslagerungsspeicher, die ein Container nutzen kann
* *MemoryReservationInMB:* der weiche Grenzwert für die Arbeitsspeicherüberwachung, die nur erzwungen wird, wenn auf dem Knoten Arbeitsspeicherkonflikte erkannt werden
* *CpuPercent:* CPU-Prozentsatz, den der Container verwenden kann. Wenn CPU-Anforderungen oder -Grenzwerte für das Dienstpaket angegeben werden, wird dieser Parameter effektiv ignoriert.
* *MaximumIOps:* maximale IOPS, die ein Container verwenden kann (lesen und schreiben)
* *MaximumIOBytesps:* maximale E/A (Byte/s), die ein Container verwenden kann (lesen und schreiben)
* *BlockIOWeight:* Block-E/A-Gewichtung relativ zu anderen Containern

Diese Ressourcen können mit CPU und Arbeitsspeicher kombiniert werden. Hier ist ein Beispiel dafür, wie zusätzliche Ressourcen für Container angegeben werden:

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuPercent="5"
            MemorySwapInMB="4084" MemoryReservationInMB="1024" MaximumIOPS="20" />
        </Policies>
    </ServiceManifestImport>
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Cluster Resource Manager finden Sie im Artikel [Einführung in den Cluster Resource Manager von Service Fabric](service-fabric-cluster-resource-manager-introduction.md).
* Weitere Informationen zu Anwendungsmodell, Dienstpaketen und Codepaketen sowie zur Vorgehensweise zum Zuweisen von Replikaten finden Sie unter [Modellieren von Anwendungen in Service Fabric][application-model-link].

<!-- Links -->
[application-model-link]: service-fabric-application-model.md
[hosting-model-link]: service-fabric-hosting-model.md
[cluster-resource-manager-description-link]: service-fabric-cluster-resource-manager-cluster-description.md
