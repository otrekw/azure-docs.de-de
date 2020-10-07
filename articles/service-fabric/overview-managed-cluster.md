---
title: Verwaltete Service Fabric-Cluster (Vorschau)
description: Verwaltete Service Fabric-Cluster sind eine Weiterentwicklung des Azure Service Fabric-Clusterressourcenmodells, das die Bereitstellung und Clusterverwaltung optimiert.
ms.topic: overview
ms.date: 09/28/2020
ms.openlocfilehash: 3d26a92126491662d5c51b3c4e8900ffa547f830
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91410397"
---
# <a name="service-fabric-managed-clusters-preview"></a>Verwaltete Service Fabric-Cluster (Vorschau)

Verwaltete Service Fabric-Cluster sind eine Weiterentwicklung des Azure Service Fabric-Clusterressourcenmodells, das Ihr Bereitstellungs- und Clusterverwaltungsszenario optimiert.

Die ARM-Vorlage (Azure Resource Model) für herkömmliche Service Fabric-Cluster erfordert, dass Sie eine Clusterressource neben einer Reihe von unterstützenden Ressourcen definieren, die alle richtig „verbunden“ sein müssen (bei der Bereitstellung und während des gesamten Lebenszyklus des Clusters), damit der Cluster und Ihre Dienste ordnungsgemäß funktionieren. Im Gegensatz dazu besteht das Kapselungsmodell für verwaltete Service Fabric-Cluster aus einer einzelnen, *von Service Fabric verwalteten Clusterressource*. Alle zugrunde liegenden Ressourcen für den Cluster werden abstrahiert und von Azure in Ihrem Auftrag verwaltet.

**Herkömmliches Service Fabric-Clustermodell**
![Herkömmliches Service Fabric-Clustermodell][sf-composition]

**Verwaltetes Service Fabric-Clustermodell**
![Gekapseltes Service Fabric-Clustermodell][sf-encapsulation]

Im Hinblick auf Größe und Komplexität entspricht die ARM-Vorlage für einen verwalteten Service Fabric-Cluster etwa 100 Zeilen JSON-Code, während für die Definition eines typischen Service Fabric-Clusters etwa 1.000 Zeilen erforderlich sind:

| Service Fabric-Ressourcen | Ressourcen eines verwalteten Service Fabric-Clusters |
|----------|-----------|
| Service Fabric-Cluster | Verwalteter Service Fabric-Cluster |
| VM-Skalierungsgruppe(n) | |
| Load Balancer | |
| Öffentliche IP-Adresse | |
| Speicherkonten | |
| Virtuelles Netzwerk | |

Verwaltete Service Fabric-Cluster bieten im Vergleich zu herkömmlichen Clustern eine Reihe von Vorteilen:

**Vereinfachte Clusterbereitstellung und -verwaltung**
- Bereitstellen und Verwalten einer einzelnen Azure-Ressource
- Zertifikatverwaltung und automatische -rotation
- Vereinfachte Skalierungsvorgänge

**Vermeiden von Betriebsfehlern**
- Vermeiden von Konfigurationskonflikten mit zugrunde liegenden Ressourcen
- Blockieren unsicherer Vorgänge (z. B. Löschen eines Seedknotens)

**Standardmäßig bewährte Methoden**
- Vereinfachte Zuverlässigkeits- und Dauerhaftigkeitseinstellungen

Für verwaltete Service Fabric-Cluster fallen keine zusätzlichen Kosten an, die über die Kosten der zugrunde liegenden Ressourcen hinausgehen, die für den Cluster erforderlich sind.

## <a name="service-fabric-managed-cluster-skus"></a>SKUs für verwaltete Service Fabric-Cluster

Verwaltete Service Fabric-Cluster sind sowohl in der SKU „Basic“ als auch „Standard“ verfügbar.

| Feature | Basic | Standard |
| ------- | ----- | -------- |
| Netzwerkressource (SKU für [Load Balancer](../load-balancer/skus.md), [Öffentliche IP-Adresse](../virtual-network/public-ip-addresses.md)) | Basic | Standard |
| Mindestanzahl Knoten (VM-Instanz) | 3 | 5 |
| Maximale Anzahl Knoten pro Knotentyp | 100 | 100 |
| Maximale Anzahl Knotentypen | 1 | 20 |
| Hinzufügen/Entfernen von Knotentypen | Nein | Ja |
| Zonenredundanz | Nein | Ja |

## <a name="service-fabric-managed-cluster-feature-roadmap"></a>Roadmap der Funktion „Verwaltete Service Fabric-Cluster“
Da dies eine frühe Vorschauversion der verwalteten Service Fabric-Cluster ist, gibt es einige Funktionslücken, die Sie kennen sollten. Diese Funktionen werden in zukünftigen Versionen verfügbar sein. 

* Direktes Veröffentlichen von Anwendungen für Cluster aus Visual Studio
* Verwaltete Identitäten 
* ARM-Anwendungsbereitstellungen 
* Verfügbarkeitszonen 
* Reverse-Proxy 
* Automatische Skalierung 
* Aktualisieren von NSG-Regeln 
* Automatische Betriebssystemupgrades

## <a name="next-steps"></a>Nächste Schritte

Um erste Schritte mit verwalteten Service Fabric-Clustern auszuführen, probieren Sie den folgenden Schnellstart aus:

> [!div class="nextstepaction"]
> [Erstellen eines verwalteten Service Fabric-Clusters (Vorschau)](quickstart-managed-cluster-template.md)


[sf-composition]: ./media/overview-managed-cluster/sfrp-composition-resource.png
[sf-encapsulation]: ./media/overview-managed-cluster/sfrp-encapsulated-resource.png