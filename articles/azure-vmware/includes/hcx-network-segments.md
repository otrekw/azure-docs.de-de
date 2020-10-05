---
title: VMware HCX-Netzwerksegmente
description: Für VMware HCx sind vier Netzwerke erforderlich.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: f01aba13d688dfed573acfc8c4344511fbfd9fa8
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2020
ms.locfileid: "91578739"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

Für VMware HCx sind vier Netzwerke erforderlich:

- **Verwaltungsnetzwerk:** In der Regel ist dieses Netzwerk mit dem Verwaltungsnetzwerk identisch, das im vSphere-Cluster verwendet wird.  Bestimmen Sie mindestens zwei IP-Adressen in diesem Netzwerksegment für VMware HCX (abhängig von Ihrer Bereitstellung sind möglicherweise mehr IP-Adressen erforderlich).

- **vMotion-Netzwerk:** In der Regel ist dieses Netzwerk mit dem Netzwerk identisch, das für vMotion im vSphere-Cluster verwendet wird.  Bestimmen Sie mindestens zwei IP-Adressen in diesem Netzwerksegment für VMware HCX (abhängig von Ihrer Bereitstellung sind möglicherweise mehr IP-Adressen erforderlich).  

   Das vMotion-Netzwerk muss über einen verteilten virtuellen Switch oder vSwitch0 verfügbar gemacht werden. Ist dies nicht der Fall, ändern Sie die Umgebung.

   > [!NOTE]
   > Wenn dieses Netzwerk nicht geroutet (privat) ist, geht das in Ordnung.

- **Uplink-Netzwerk:** Sie möchten ein neues Netzwerk für VMware HCX Uplink erstellen und es über eine Portgruppe auf Ihren vSphere-Cluster erweitern.  Bestimmen Sie mindestens zwei IP-Adressen in diesem Netzwerksegment für VMware HCX (abhängig von Ihrer Bereitstellung sind möglicherweise mehr IP-Adressen erforderlich).  

   > [!NOTE]
   > Die empfohlene Methode sieht so aus, dass Sie ein /29-Netzwerk erstellen, es funktioniert aber jede Netzwerkgröße.

- **Replikationsnetzwerk:** Sie möchten ein neues Netzwerk für VMware HCX Replication erstellen und dieses Netzwerk über eine Portgruppe auf Ihren vSphere-Cluster erweitern.  Bestimmen Sie mindestens zwei IP-Adressen in diesem Netzwerksegment für VMware HCX (abhängig von Ihrer Bereitstellung sind möglicherweise mehr IP-Adressen erforderlich).

   > [!NOTE]
   > Die empfohlene Methode sieht so aus, dass Sie ein /29-Netzwerk erstellen, es funktioniert aber jede Netzwerkgröße.