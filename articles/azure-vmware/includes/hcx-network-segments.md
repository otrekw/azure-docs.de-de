---
title: VMware HCX-Netzwerksegmente
description: Für VMware HCx sind vier Netzwerke erforderlich.
ms.topic: include
ms.date: 03/13/2021
ms.openlocfilehash: f2f5597471355bf4d74be7fbe370e6fa58770bf1
ms.sourcegitcommit: b28e9f4d34abcb6f5ccbf112206926d5434bd0da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2021
ms.locfileid: "107251411"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

Es gibt verschiedene Möglichkeiten, VMware HCX-Netzwerksegmente lokal zu konfigurieren. Nachfolgend wird eine einfache Konfiguration beschrieben, die einen Pilot- oder einen kleinen Produktionsanwendungsfall unterstützt.  Beim Entwerfen für Hunderte oder Tausende Workloads muss diese Konfiguration abhängig von den Anforderungen der Migration möglicherweise geändert werden.  

Ermitteln Sie als Vorbereitung der VMware HCX-Bereitstellung zur Unterstützung des Pilot- oder kleinen Produktionsanwendungsfalls Folgendes:

- **Verwaltungsnetzwerk:** Bei der lokalen Bereitstellung von VMware HCX müssen Sie ein Verwaltungsnetzwerk für VMware HCX identifizieren.  In der Regel handelt es sich um dasselbe Verwaltungsnetzwerk, das von Ihrem lokalen VMware-Cluster verwendet wird.  Identifizieren Sie in diesem Netzwerksegment mindestens **zwei** IP-Adressen für VMware HCX. Abhängig von der Skalierung der Bereitstellung, die über den Pilot- oder den kleinen Anwendungsfall hinausgeht, benötigen Sie möglicherweise größere Zahlen.

  > [!NOTE]
  > Statt bei der Vorbereitung auf große Umgebungen das für den lokalen VMware-Cluster verwendete Verwaltungsnetzwerk zu verwenden, erstellen Sie ein neues Netzwerk vom Typ „/26“, und stellen Sie dieses Netzwerk als Portgruppe für Ihren lokalen VMware-Cluster bereit.  Anschließend können Sie bis zu 10 Dienstnetze und 60 Netzwerkextender (-1 pro Dienstnetz) erstellen. Mit privaten Azure VMware Solution-Clouds können pro Netzwerkextender **acht** Netzwerke gestreckt werden.
  >

- **Uplink-Netzwerk:** Bei der lokalen Bereitstellung von VMware HCX müssen Sie ein Uplink-Netzwerk für VMware HCX identifizieren. Verwenden Sie dasselbe Netzwerk wie für das Verwaltungsnetzwerk. 

- **vMotion-Netzwerk:** Bei der lokalen Bereitstellung von VMware HCX müssen Sie ein vMotion-Netzwerk für VMware HCX identifizieren.  In der Regel handelt es sich um dasselbe Netzwerk, das auch von Ihrem lokalen VMware-Cluster für vMotion verwendet wird.  Identifizieren Sie in diesem Netzwerksegment mindestens **zwei** IP-Adressen für VMware HCX. Abhängig von der Skalierung der Bereitstellung, die über den Pilot- oder den kleinen Anwendungsfall hinausgeht, benötigen Sie möglicherweise größere Zahlen.

   Das vMotion-Netzwerk muss über einen verteilten virtuellen Switch oder vSwitch0 verfügbar gemacht werden. Ist dies nicht der Fall, ändern Sie die Umgebung entsprechend.

   > [!NOTE]
   > Viele VMware-Umgebungen verwenden nicht geroutete Netzwerksegmente für vMotion. Dies stellt kein Problem dar.
  
- **Replikationsnetzwerk:** Bei der lokalen Bereitstellung von VMware HCX müssen Sie ein Replikationsnetzwerk definieren. Verwenden Sie das gleiche Netzwerk wie für Ihre Verwaltungs- und Uplink-Netzwerke.  Wenn der lokale Clusterhost ein dediziertes Replication-VMkernel-Netzwerk verwendet, reservieren Sie in diesem Netzwerksegment **zwei** IP-Adressen, und nutzen Sie das Replication-VMkernel-Netzwerk für das Replikationsnetzwerk.
