---
title: VMware HCX-Netzwerksegmente
description: Für VMware HCx sind vier Netzwerke erforderlich.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 8137b4383d2a243d53db317db6f5a78b3bc68e67
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92173634"
---
<!-- Used in avs-production-ready-deployment.md and tutorial-deploy-vmware-hcx.md -->

Für VMware HCX werden vier Netzwerke benötigt:

- **Verwaltungsnetzwerk:** In der Regel ist dieses Netzwerk mit dem Verwaltungsnetzwerk identisch, das im vSphere-Cluster verwendet wird. Identifizieren Sie in diesem Netzwerksegment mindestens zwei IP-Adressen für VMware HCX. (Je nach Bereitstellung ist ggf. eine höhere Zahl erforderlich.)

- **vMotion-Netzwerk:** In der Regel ist dieses Netzwerk mit dem Netzwerk identisch, das für vMotion im vSphere-Cluster verwendet wird.  Identifizieren Sie in diesem Netzwerksegment mindestens zwei IP-Adressen für VMware HCX. (Je nach Bereitstellung ist ggf. eine höhere Zahl erforderlich.)  

   Das vMotion-Netzwerk muss über einen verteilten virtuellen Switch oder vSwitch0 verfügbar gemacht werden. Ist dies nicht der Fall, ändern Sie die Umgebung.

   > [!NOTE]
   > Wenn dieses Netzwerk nicht geroutet (privat) ist, ist dies kein Problem.

- **Uplink-Netzwerk:** Sie möchten ein neues Netzwerk für VMware HCX Uplink erstellen und es über eine Portgruppe auf Ihren vSphere-Cluster erweitern. Identifizieren Sie in diesem Netzwerksegment mindestens zwei IP-Adressen für VMware HCX. (Je nach Bereitstellung ist ggf. eine höhere Zahl erforderlich.)  

   > [!NOTE]
   > Die empfohlene Methode sieht so aus, dass Sie ein /29-Netzwerk erstellen, es funktioniert aber jede Netzwerkgröße.

- **Replikationsnetzwerk:** Sie möchten ein neues Netzwerk für VMware HCX Replication erstellen und dieses Netzwerk über eine Portgruppe auf Ihren vSphere-Cluster erweitern. Identifizieren Sie in diesem Netzwerksegment mindestens zwei IP-Adressen für VMware HCX. (Je nach Bereitstellung ist ggf. eine höhere Zahl erforderlich.)

   > [!NOTE]
   > Die empfohlene Methode sieht so aus, dass Sie ein /29-Netzwerk erstellen, es funktioniert aber jede Netzwerkgröße.