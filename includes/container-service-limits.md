---
title: include file
description: include file
services: container-service
author: mlearned
ms.service: container-service
ms.topic: include
ms.date: 11/22/2019
ms.author: mlearned
ms.custom: include file
ms.openlocfilehash: 1c2dec106ae72ddead7bda54792fa74e38eb6660
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106081052"
---
| Resource                                                                                                           | Begrenzung                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Maximale Anzahl Cluster pro Abonnement                                                                                  | 1000                                                                                                                                                                                                        |
| Maximale Knoten pro Cluster mit VM-Verfügbarkeitsgruppen und der Load Balancer Basic-SKU                       | 100                                                                                                                                                                                                         |
| Maximale Knoten pro Cluster mit VM-Skalierungsgruppen und der [Load Balancer Standard-SKU][standard-load-balancer] | 1.000 (100 Knoten pro [Knotenpool][node-pool])                                                                                                                                                                 |
| Maximale Pods pro Knoten: [Grundlegende Netzwerkunterstützung][basic-networking] mit Kubernetes                                           | 110                                                                                                                                                                                                         |
| Maximale Pods pro Knoten: [Erweiterte Netzwerkunterstützung][advanced-networking] mit Azure Container Networking Interface        | Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle: 30<sup>1</sup><br />Azure Resource Manager-Vorlage: 30<sup>1</sup><br />Bereitstellung über das Portal: 30                                                                                        |
| Vorschau des OSM-AKS-Add-Ons (Open Service Mesh)                                                                          | Kubernetes-Clusterversion: 1.19+<sup>2</sup><br />OSM-Controller pro Cluster: 1<sup>2</sup><br />Pods pro OSM-Controller: 500<sup>2</sup><br />Von OSM verwaltete Kubernetes-Dienstkonten: 50<sup>2</sup> |

<sup>1</sup>Wenn Sie einen AKS-Cluster (Azure Kubernetes Service) über die Azure-Befehlszeilenschnittstelle oder über eine Resource Manager-Vorlage bereitstellen, kann dieser Wert auf bis zu 250 Pods pro Knoten festgelegt werden. Wenn Sie bereits einen AKS-Cluster bereitgestellt haben oder einen Cluster über das Azure-Portal bereitstellen, kann die maximale Anzahl von Pods pro Knoten nicht konfiguriert werden.<br />

<sup>2</sup>Das OSM-Add-On für AKS befindet sich in der Vorschauphase und wird noch weiter optimiert, bevor es allgemein verfügbar wird. Während der Vorschauphase wird empfohlen, die angezeigten Grenzwerte nicht zu überschreiten.<br />

<!-- LINKS - Internal -->

[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking
[standard-load-balancer]: ../articles/load-balancer/load-balancer-overview.md
[node-pool]: ../articles/aks/use-multiple-node-pools.md

<!-- LINKS - External -->

[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
