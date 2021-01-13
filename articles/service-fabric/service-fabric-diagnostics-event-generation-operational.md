---
title: Azure Service Fabric-Ereignisliste
description: Eine umfassende Liste mit Ereignissen, die von Azure Service Fabric bereitgestellt werden, um Sie bei der Clusterüberwachung zu unterstützen.
author: srrengar
ms.topic: reference
ms.date: 2/25/2019
ms.author: srrengar
ms.openlocfilehash: e69b407bc7d58a83616daa44272ec008ccff9fad
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85846658"
---
# <a name="list-of-service-fabric-events"></a>Liste mit Service Fabric-Ereignissen 

Service Fabric macht einen primären Satz von Clusterereignissen in Form von [Service Fabric-Ereignissen](service-fabric-diagnostics-events.md) verfügbar, um Sie über den Status Ihres Clusters zu informieren. Diese basieren auf Aktionen, die von Service Fabric für Ihre Knoten und Cluster ausgeführt werden, oder auf Verwaltungsentscheidungen eines Clusterbesitzers/-betreibers. Auf diese Ereignisse kann durch Konfigurieren auf verschiedene Art und Weise zugegriffen werden, einschließlich Konfigurieren von [Azure Monitor-Protokollen mit Ihrem Cluster](service-fabric-diagnostics-oms-setup.md) oder Abfragen von [EventStore](service-fabric-diagnostics-eventstore.md). Auf Windows-Computern sind diese Ereignisse in das „EventLog“ (Ereignisprotokoll) eingebunden, sodass Service Fabric-Ereignisse in der Ereignisanzeige angezeigt werden. 

Im Folgenden sind einige Merkmale dieser Ereignisse aufgeführt:
* Jedes Ereignis ist an eine bestimmte Entität im Cluster gebunden, z. B. Anwendung, Dienst, Knoten, Replikat.
* Jedes Ereignis verfügt über mehrere gemeinsame Felder: „EventInstanceId“ (Ereignis-Instanz-ID), „EventName“ (Ereignisname), und „Category“ (Kategorie).
* Jedes Ereignis verfügt über Felder, die das Ereignis an die Entität binden, der es zugeordnet ist. So besitzt beispielsweise das Ereignis „ApplicationCreated“ (Anwendung erstellt) Felder, die den Namen der erstellten Anwendung identifizieren.
* Ereignisse sind so strukturiert, dass sie für detaillierte Analysen in einer Reihe von Tools benutzt werden können. Zusätzlich werden relevante Details für ein Ereignis, im Gegensatz zu einer langen Zeichenfolge, als separate Eigenschaften definiert. 
* Ereignisse werden durch verschiedene Subsysteme in Service Fabric geschrieben und durch „Source(Task)“ (Quelle(Aufgabe)) identifiziert. Weitere Informationen zu diesen Subsystemen finden Sie unter [Service Fabric Architecture (Service Fabric-Architektur)](service-fabric-architecture.md) und unter [Service Fabric Technical Overview (Technische Übersicht über Service Fabric)](service-fabric-technical-overview.md).

Im Folgenden sind diese Service Fabric-Ereignisse sortiert nach Entität aufgelistet:

## <a name="cluster-events"></a>Clusterereignisse

**Ereignisse für Clusterupgrades**

Weitere Informationen zu Clusterupgrades finden Sie [hier](service-fabric-cluster-upgrade-windows-server.md).

| EventId | Name | Category | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | 
| --- | --- | --- | --- | --- | --- | 
| 29627 | ClusterUpgradeStarted | Aktualisieren | Ein Clusterupgrade wurde gestartet. | CM | Informational |
| 29628 | ClusterUpgradeCompleted | Aktualisieren | Ein Clusterupgrade wurde abgeschlossen. | CM | Informational | 
| 29629 | ClusterUpgradeRollbackStarted | Aktualisieren | Das Rollback eines Clusterupgrades wurde gestartet.  | CM | Warnung | 
| 29630 | ClusterUpgradeRollbackCompleted | Aktualisieren | Das Rollback eines Clusterupgrades wurde abgeschlossen. | CM | Warnung | 
| 29631 | ClusterUpgradeDomainCompleted | Aktualisieren | Der Upgradevorgang für eine Upgradedomäne wurde während eines Clusterupgrades beendet. | CM | Informational | 

## <a name="node-events"></a>Knotenereignisse

**Ereignisse für den Knotenlebenszyklus** 

| EventId | Name | Category | BESCHREIBUNG |Quelle (Aufgabe) | Ebene |
| --- | --- | ---| --- | --- | --- | 
| 18602 | NodeDeactivateCompleted | StateTransition | Die Deaktivierung eines Knotens wurde abgeschlossen. | FM | Informational | 
| 18603 | NodeUp | StateTransition | Der Cluster hat erkannt, dass ein Knoten gestartet wurde. | FM | Informational | 
| 18604 | NodeDown | StateTransition | Der Cluster hat erkannt, dass ein Knoten heruntergefahren wurde. Bei einem Neustart eines Knotens wird ein NodeDown-Ereignis gefolgt von einem NodeUp-Ereignis angezeigt. |  FM | Fehler | 
| 18605 | NodeAddedToCluster | StateTransition |  Dem Cluster wurde ein neuer Knoten hinzugefügt, und Service Fabric kann für diesen Knoten Anwendungen bereitstellen. | FM | Informational | 
| 18606 | NodeRemovedFromCluster | StateTransition |  Ein Knoten wurde aus dem Cluster entfernt. Service Fabric stellt für diesen Knoten keine Anwendungen mehr bereit. | FM | Informational | 
| 18607 | NodeDeactivateStarted | StateTransition |  Die Deaktivierung eines Knotens wurde gestartet. | FM | Informational | 
| 25621 | NodeOpenSucceeded | StateTransition |  Ein Knoten wurde erfolgreich gestartet. | FabricNode | Informational | 
| 25622 | NodeOpenFailed | StateTransition |  Ein Knoten konnte nicht gestartet und nicht mit dem Ring verknüpft werden. | FabricNode | Fehler | 
| 25624 | NodeClosed | StateTransition |  Ein Knoten wurde erfolgreich heruntergefahren. | FabricNode | Informational | 
| 25626 | NodeAborted | StateTransition |  Ein Knoten wurde nicht ordnungsgemäß heruntergefahren. | FabricNode | Fehler | 

## <a name="application-events"></a>Anwendungsereignisse

**Ereignisse für den Anwendungslebenszyklus**

| EventId | Name | Category | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | 
| --- | --- | --- | --- | --- | --- | 
| 29620 | ApplicationCreated | LifeCycle | Eine neue Anwendung wurde erstellt. | CM | Informational | 
| 29625 | ApplicationDeleted | LifeCycle | Eine vorhandene Anwendung wurde gelöscht. | CM | Informational | 
| 23083 | ApplicationProcessExited | LifeCycle | Ein Prozess in einer Anwendung wurde beendet. | Hosting | Informational | 

**Ereignisse für Anwendungsupgrades**

Weitere Informationen zu Anwendungsupgrades finden Sie [hier](service-fabric-application-upgrade.md).

| EventId | Name | Category | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | 
| --- | --- | ---| --- | --- | --- | 
| 29621 | ApplicationUpgradeStarted | Aktualisieren | Ein Anwendungsupgrade wurde gestartet. | CM | Informational | 
| 29622 | ApplicationUpgradeStarted | Aktualisieren | Ein Anwendungsupgrade wurde abgeschlossen. | CM | Informational | 
| 29623 | ApplicationUpgradeRollbackStarted | Aktualisieren | Das Rollback eines Anwendungsupgrades wurde gestartet. |CM | Warnung | 
| 29624 | ApplicationUpgradeRollbackCompleted | Aktualisieren | Das Rollback eines Anwendungsupgrades wurde abgeschlossen. | CM | Warnung | 
| 29626 | ApplicationUpgradeDomainCompleted | Aktualisieren | Der Upgradevorgang für eine Upgradedomäne wurde während eines Anwendungsupgrades beendet. | CM | Informational | 

## <a name="service-events"></a>Dienstereignisse

**Ereignisse für den Dienstlebenszyklus**

| EventId | Name | Category | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | 
| --- | --- | ---| --- | --- | --- |
| 18657 | ServiceCreated | LifeCycle | Ein neuer Dienst wurde erstellt. | FM | Informational | 
| 18658 | ServiceDeleted | LifeCycle | Ein vorhandener Dienst wurde gelöscht. | FM | Informational | 

## <a name="partition-events"></a>Partitionsereignisse

**Ereignisse für Partitionsverschiebungen**

| EventId | Name | Category | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | 
| --- | --- | ---| --- | --- | --- |
| 18940 | PartitionReconfigured | LifeCycle | Die Neukonfiguration einer Partition wurde abgeschlossen. | RA | Informational | 

## <a name="replica-events"></a>Replikatereignisse

**Replikatlebenszyklusereignisse**

| EventId | Name | Category | BESCHREIBUNG |Quelle (Aufgabe) | Ebene |
| --- | --- | ---| --- | --- | --- |
| 61701 | ReliableDictionaryOpened | LifeCycle | Ein zuverlässiges Wörterbuch wurde geöffnet. | DistributedDictionary | Informational |
| 61702 | ReliableDictionaryClosed | LifeCycle | Ein zuverlässiges Wörterbuch wurde geschlossen. | DistributedDictionary | Informational |
| 61703 | ReliableDictionaryCheckpointRecovered | LifeCycle | Ein zuverlässiges Wörterbuch hat seinen Prüfpunkt wiederhergestellt. | DistributedDictionary | Informational |
| 61704 | ReliableDictionaryCheckpointFilesSent | LifeCycle | Das Replikat hat die Prüfpunktdateien des zuverlässigen Wörterbuchs gesendet. | DistributedDictionary | Informational |
| 61705 | ReliableDictionaryCheckpointFilesReceived | LifeCycle | Das Replikat hat die Prüfpunktdateien des zuverlässigen Wörterbuchs empfangen. | DistributedDictionary | Informational |
| 61963 | ReliableQueueOpened | LifeCycle | Eine zuverlässige Warteschlange wurde geöffnet. | DistributedQueue | Informational |
| 61964 | ReliableQueueClosed | LifeCycle | Eine zuverlässige Warteschlange wurde geschlossen. | DistributedQueue | Informational |
| 61965 | ReliableQueueCheckpointRecovered | LifeCycle | Eine zuverlässige Warteschlange hat ihren Prüfpunkt wiederhergestellt. | DistributedQueue | Informational |
| 61966 | ReliableQueueCheckpointFilesSent | LifeCycle | Das Replikat hat die Prüfpunktdateien der zuverlässigen Warteschlange gesendet. | DistributedQueue | Informational |
| 63647 | ReliableQueueCheckpointFilesReceived | LifeCycle | Das Replikat hat die Prüfpunktdateien der zuverlässigen Warteschlange empfangen. | DistributedQueue | Informational |
| 63648 | ReliableConcurrentQueueOpened | LifeCycle | Eine zuverlässige parallele Warteschlange wurde geöffnet. | ReliableConcurrentQueue | Informational |
| 63649 | ReliableConcurrentQueueClosed | LifeCycle | Eine zuverlässige parallele Warteschlange wurde geschlossen. | ReliableConcurrentQueue | Informational |
| 63650 | ReliableConcurrentQueueCheckpointRecovered | LifeCycle | Eine zuverlässige parallele Warteschlange hat ihren Prüfpunkt wiederhergestellt. | ReliableConcurrentQueue | Informational |
| 61687 | TStoreError | Fehler | In der zuverlässigen Sammlung ist ein unerwarteter Fehler aufgetreten. | TStore | Fehler |
| 63831 | PrimaryFullCopyInitiated | LifeCycle | Das primäre Replikat hat eine vollständige Kopie initiiert. | TReplicator | Informational |
| 63832 | PrimaryPartialCopyInitiated | LifeCycle | Das primäre Replikat hat eine Teilkopie initiiert. | TReplicator | Informational |
| 16831 | BuildIdleReplicaStarted | LifeCycle | Das primäre Replikat hat mit dem Erstellen von Leerlaufreplikaten begonnen. | Replikation | Informational |
| 16832 | BuildIdleReplicaCompleted | LifeCycle | Das primäre Replikat hat das Erstellen von Leerlaufreplikaten abgeschlossen. | Replikation | Informational |
| 16833 | BuildIdleReplicaFailed | LifeCycle | Das primäre Replikat konnte keine Leerlaufreplikate erstellen. | Replikation | Warnung |
| 16834 | PrimaryReplicationQueueFull | Health | Die Replikationswarteschlange des primären Replikats ist voll. | Replikation | Warnung |
| 16835 | PrimaryReplicationQueueWarning | Health | Die Replikationswarteschlange des primären Replikats ist fast voll. | Replikation | Warnung |
| 16836 | PrimaryReplicationQueueWarningMitigated | Health | Die Auslastung der Replikationswarteschlange des primären Replikats ist im Normalbereich. | Replikation | Informational |
| 16837 | SecondaryReplicationQueueFull | Health | Die Replikationswarteschlange des sekundären Replikats ist voll. | Replikation | Warnung |
| 16838 | SecondaryReplicationQueueWarning | Health | Die Replikationswarteschlange des sekundären Replikats ist fast voll. | Replikation | Warnung |
| 16839 | SecondaryReplicationQueueWarningMitigated | Health | Die Auslastung der Replikationswarteschlange des sekundären Replikats ist im Normalbereich. | Replikation | Informational |
| 16840 | PrimaryFaultedSlowSecondary | Health | Das primäre Replikat verfügt über ein fehlerhaftes, langsames sekundäres Replikat. | Replikation | Warnung |
| 16841 | ReplicatorFaulted | Health | Das Replikat ist ausgefallen. | Replikation | Warnung |

## <a name="container-events"></a>Containerereignisse

**Ereignisse für den Containerlebenszyklus** 

| EventId | Name | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 23074 | ContainerActivated | Ein Container wurde gestartet. | Hosting | Informational | 1 |
| 23075 | ContainerDeactivated | Ein Container wurde beendet. | Hosting | Informational | 1 |
| 23082 | ContainerExited | Ein Container wurde beendet. Überprüfen Sie das UnexpectedTermination-Flag. | Hosting | Informational | 1 |

## <a name="health-reports"></a>Integritätsberichte

Das [Service Fabric-Integritätsmodell](service-fabric-health-introduction.md) bietet eine umfassende, flexible und erweiterbare Integritätsevaluierung und -berichterstellung. Ab der Service Fabric-Version 6.2 werden Integritätsdaten als Plattformereignisse geschrieben, um Verlaufsinformationen für das Integritätsereignis bereitzustellen. Um die Anzahl von Integritätsereignissen gering zu halten, schreiben wir nur Folgendes als Service Fabric-Ereignisse:

* Alle Integritätsberichte für `Error` oder `Warning`
* Integritätsberichte für `Ok` während Übergängen
* Wenn ein `Error`- oder `Warning`-Integritätsereignis abläuft. Damit kann bestimmt werden, wie lange eine Entität fehlerhaft war.

**Ereignisse für Clusterintegritätsberichte**

| EventId | Name | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | --- | --- | --- | --- |
| 54428 | ClusterNewHealthReport | Ein Integritätsbericht für einen neuen Cluster ist verfügbar. | HM | Informational | 1 |
| 54437 | ClusterHealthReportExpired | Ein Integritätsbericht für einen vorhandenen Cluster ist abgelaufen. | HM | Informational | 1 |

**Ereignisse für Knotenintegritätsberichte**

| EventId | Name | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 54423 | NodeNewHealthReport | Ein Integritätsbericht für einen neuen Knoten ist verfügbar. | HM | Informational | 1 |
| 54432 | NodeHealthReportExpired | Ein Integritätsbericht für einen vorhandenen Knoten ist abgelaufen. | HM | Informational | 1 |

**Ereignisse für Anwendungsintegritätsberichte**

| EventId | Name | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 54425 | ApplicationNewHealthReport | Ein Integritätsbericht für eine neue Anwendung wurde erstellt. Dies gilt für nicht bereitgestellte Anwendungen. | HM | Informational | 1 |
| 54426 | DeployedApplicationNewHealthReport | Ein Integritätsbericht für eine neue bereitgestellte Anwendung wurde erstellt. | HM | Informational | 1 |
| 54427 | DeployedServicePackageNewHealthReport | Ein Integritätsbericht für einen neuen bereitgestellten Dienst wurde erstellt. | HM | Informational | 1 |
| 54434 | ApplicationHealthReportExpired | Ein Integritätsbericht für eine vorhandene Anwendung ist abgelaufen. | HM | Informational | 1 |
| 54435 | DeployedApplicationHealthReportExpired | Ein Integritätsbericht für eine vorhandene bereitgestellte Anwendung ist abgelaufen. | HM | Informational | 1 |
| 54436 | DeployedServicePackageHealthReportExpired | Ein Integritätsbericht für einen vorhandenen bereitgestellten Dienst ist abgelaufen. | HM | Informational | 1 |

**Ereignisse für Dienstintegritätsberichte**

| EventId | Name | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 54424 | ServiceNewHealthReport | Ein Integritätsbericht für einen neuen Dienst wurde erstellt. | HM | Informational | 1 |
| 54433 | ServiceHealthReportExpired | Ein Integritätsbericht für einen vorhandenen Dienst ist abgelaufen. | HM | Informational | 1 |

**Ereignisse für Partitionsintegritätsberichte**

| EventId | Name | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 54422 | PartitionNewHealthReport | Ein Integritätsbericht für eine neue Anwendung wurde erstellt. | HM | Informational | 1 |
| 54431 | PartitionHealthReportExpired | Ein Integritätsbericht für eine vorhandene Partition ist abgelaufen. | HM | Informational | 1 |

**Ereignisse für Replikatintegritätsberichte**

| EventId | Name | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 54429 | StatefulReplicaNewHealthReport | Ein Integritätsbericht für ein zustandsbehaftetes Replikat wurde erstellt. | HM | Informational | 1 |
| 54430 | StatelessInstanceNewHealthReport | Ein Integritätsbericht für ein neues zustandsloses Replikat wurde erstellt. | HM | Informational | 1 |
| 54438 | StatefulReplicaHealthReportExpired | Ein Integritätsbericht für ein vorhandenes zustandsbehaftetes Replikat ist abgelaufen. | HM | Informational | 1 |
| 54439 | StatelessInstanceHealthReportExpired | Ein Integritätsbericht für eine vorhandene zustandslose Instanz ist abgelaufen. | HM | Informational | 1 |

## <a name="chaos-testing-events"></a>Chaostestereignisse 

**Chaossitzungsereignisse**

| EventId | Name | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 50021 | ChaosStarted | Eine Chaostestsitzung wurde gestartet. | Prüfbarkeit | Informational | 1 |
| 50023 | ChaosStopped | Eine Chaostestsitzung wurde beendet. | Prüfbarkeit | Informational | 1 |

**Chaosknotenereignisse**

| EventId | Name | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 50033 | ChaosNodeRestartScheduled | Der Neustart eines Knoten wurde als Teil einer Chaostestsitzung geplant. | Prüfbarkeit | Informational | 1 |
| 50087 | ChaosNodeRestartCompleted | Der Neustart eines Knoten wurde als Teil einer Chaostestsitzung beendet. | Prüfbarkeit | Informational | 1 |

**Chaosanwendungsereignisse**

| EventId | Name | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 50053 | ChaosCodePackageRestartScheduled | Ein Neustart eines Codepakets wurde während einer Chaostestsitzung geplant. | Prüfbarkeit | Informational | 1 |
| 50101 | ChaosCodePackageRestartCompleted | Ein Neustart eines Codepakets wurde während einer Chaostestsitzung abgeschlossen. | Prüfbarkeit | Informational | 1 |

**Chaospartitionsereignisse**

| EventId | Name | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 50069 | ChaosPartitionPrimaryMoveScheduled | Die Verschiebung einer primären Partition wurde als Teil einer Chaostestsitzung geplant. | Prüfbarkeit | Informational | 1 |
| 50077 | ChaosPartitionSecondaryMoveScheduled | Die Verschiebung einer sekundären Partition wurde als Teil einer Chaostestsitzung geplant. | Prüfbarkeit | Informational | 1 |
| 65003 | PartitionPrimaryMoveAnalysis | Die Tiefenanalyse der Verschiebung der primären Partition ist verfügbar. | Prüfbarkeit | Informational | 1 |

**Chaosreplikatereignisse**

| EventId | Name | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 50047 | ChaosReplicaRestartScheduled | Ein Neustart eines Replikats wurde als Teil einer Chaostestsitzung geplant. | Prüfbarkeit | Informational | 1 |
| 50051 | ChaosReplicaRemovalScheduled | Die Entfernung eines Replikats wurde als Teil einer Chaostestsitzung geplant. | Prüfbarkeit | Informational | 1 |
| 50093 | ChaosReplicaRemovalCompleted | Die Entfernung eines Replikats wurde als Teil einer Chaostestsitzung abgeschlossen. | Prüfbarkeit | Informational | 1 |

## <a name="other-events"></a>Sonstige Ereignisse

**Korrelationsereignisse**

| EventId | Name | BESCHREIBUNG |Quelle (Aufgabe) | Ebene | Version |
| --- | --- | ---| --- | --- | --- |
| 65011 | CorrelationOperational | Eine Korrelation wurde ermittelt. | Prüfbarkeit | Informational | 1 |

## <a name="events-prior-to-version-62"></a>Ereignisse vor Version 6.2

Im Anschluss finden Sie eine umfassende Liste mit Ereignissen, die von Service Fabric vor Version 6.2 bereitgestellt wurden:

| EventId | Name | Quelle (Aufgabe) | Ebene |
| --- | --- | --- | --- |
| 25620 | NodeOpening | FabricNode | Informational |
| 25621 | NodeOpenedSuccess | FabricNode | Informational |
| 25622 | NodeOpenedFailed | FabricNode | Informational |
| 25623 | NodeClosing | FabricNode | Informational |
| 25624 | NodeClosed | FabricNode | Informational |
| 25625 | NodeAborting | FabricNode | Informational |
| 25626 | NodeAborted | FabricNode | Informational |
| 29627 | ClusterUpgradeStart | CM | Informational |
| 29628 | ClusterUpgradeComplete | CM | Informational |
| 29629 | ClusterUpgradeRollback | CM | Informational |
| 29630 | ClusterUpgradeRollbackComplete | CM | Informational |
| 29631 | ClusterUpgradeDomainComplete | CM | Informational |
| 23074 | ContainerActivated | Hosting | Informational |
| 23075 | ContainerDeactivated | Hosting | Informational |
| 29620 | ApplicationCreated | CM | Informational |
| 29621 | ApplicationUpgradeStart | CM | Informational |
| 29622 | ApplicationUpgradeComplete | CM | Informational |
| 29623 | ApplicationUpgradeRollback | CM | Informational |
| 29624 | ApplicationUpgradeRollbackComplete | CM | Informational |
| 29625 | ApplicationDeleted | CM | Informational |
| 29626 | ApplicationUpgradeDomainComplete | CM | Informational |
| 18566 | ServiceCreated | FM | Informational |
| 18567 | ServiceDeleted | FM | Informational |

## <a name="next-steps"></a>Nächste Schritte

* Verschaffen Sie sich einen Überblick über die [Diagnosen in Service Fabric](service-fabric-diagnostics-overview.md).
* Unter [Übersicht über den EventStore-Dienst in Service Fabric](service-fabric-diagnostics-eventstore.md) erfahren Sie mehr über den EventStore-Dienst.
* Ändern Ihrer [Azure-Diagnose](service-fabric-diagnostics-event-aggregation-wad.md)-Konfiguration zum Sammeln weiterer Protokolle
* [Einrichten von Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) zum Anzeigen der Betriebskanalprotokolle
