---
title: VMware HCX-Netzwerksegmente
description: Für VMware HCx sind vier Netzwerke erforderlich.
ms.topic: include
ms.date: 03/13/2021
ms.openlocfilehash: e9b37c125db82a95c137ede8d642888fba8b6c80
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103622138"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

Es gibt verschiedene Möglichkeiten, VMware HCX-Netzwerksegmente lokal zu konfigurieren. Nachfolgend wird eine einfache Konfiguration beschrieben, die einen Pilot- oder einen kleinen Produktionsanwendungsfall unterstützt.  Beim Entwerfen für Hunderte oder Tausende Workloads muss diese Konfiguration abhängig von den Anforderungen der Migration möglicherweise geändert werden.  

Ermitteln Sie als Vorbereitung der VMware HCX-Bereitstellung zur Unterstützung des Pilot- oder kleinen Produktionsanwendungsfalls Folgendes:

- **Verwaltungsnetzwerk:** Bei der lokalen Bereitstellung von VMware HCX müssen Sie ein Verwaltungsnetzwerk definieren.  In der Regel handelt es sich um dasselbe Verwaltungsnetzwerk, das von Ihrem lokalen VMware-Cluster verwendet wird.  Identifizieren Sie in diesem Netzwerksegment mindestens **zwei** IP-Adressen für VMware HCX. Abhängig von der Skalierung der Bereitstellung, die über den Pilot- oder den kleinen Anwendungsfall hinausgeht, benötigen Sie möglicherweise größere Zahlen.

> [!NOTE]
   > Empfohlen wird die Erstellung eines Netzwerks vom Typ „/26“, da Sie bis zu zehn Dienstnetze und 60 Netzwerkextender (-1 pro Dienstnetz) verwenden können. Mit privaten Azure VMware Solution-Clouds können pro Netzwerkextender **acht** Netzwerke gestreckt werden.
   >
   
- **vMotion-Netzwerk:** Bei der lokalen Bereitstellung von VMware HCX müssen Sie ein vMotion-Netzwerk definieren.  In der Regel handelt es sich um dasselbe Netzwerk, das auch von Ihrem lokalen VMware-Cluster für vMotion verwendet wird.  Identifizieren Sie in diesem Netzwerksegment mindestens **zwei** IP-Adressen für VMware HCX. Abhängig von der Skalierung der Bereitstellung, die über den Pilot- oder den kleinen Anwendungsfall hinausgeht, benötigen Sie möglicherweise größere Zahlen.

   Das vMotion-Netzwerk muss über einen verteilten virtuellen Switch oder vSwitch0 verfügbar gemacht werden. Ist dies nicht der Fall, ändern Sie die Umgebung entsprechend.

   > [!NOTE]
   > Viele VMware-Umgebungen verwenden nicht geroutete Netzwerksegmente für vMotion. Dies stellt kein Problem dar.

- **Uplink-Netzwerk:** Bei der lokalen Bereitstellung von VMware HCX müssen Sie ein Uplink-Netzwerk definieren. Verwenden Sie das als Uplink-Netzwerk definierte Verwaltungsnetzwerk.
   
- **Replikationsnetzwerk:** Bei der lokalen Bereitstellung von VMware HCX müssen Sie ein Replikationsnetzwerk definieren. Verwenden Sie das als Replikationsnetzwerk definierte Verwaltungsnetzwerk.  Wenn der lokale Clusterhost ein dediziertes Replication-VMkernel-Netzwerk verwendet, reservieren Sie in diesem Netzwerksegment **zwei** IP-Adressen, und nutzen Sie das Replication-VMkernel-Netzwerk für das Replikationsnetzwerk.
