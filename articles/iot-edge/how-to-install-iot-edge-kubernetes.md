---
title: Installieren von IoT Edge unter Kubernetes | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie IoT Edge unter Kubernetes mit einer lokalen Entwicklungsclusterumgebung installieren können.
author: kgremban
manager: philmea
ms.author: veyalla
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: d5fc7b6b480056b56a2776cebd0fa87a5b96f9f0
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103201667"
---
# <a name="how-to-install-iot-edge-on-kubernetes-preview"></a>Installieren von IoT Edge unter Kubernetes (Vorschauversion)

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

IoT Edge kann mit Kubernetes integriert werden und es als robuste, hochverfügbare Infrastrukturschicht nutzen. So fügt sich diese Unterstützung in eine IoT Edge-Lösung auf hoher Stufe ein:

![k8s-Einführung](./media/how-to-install-iot-edge-kubernetes/kubernetes-model.png)

>[!TIP]
>Ein gutes mentales Modell für diese Integration ist die Vorstellung von Kubernetes als einer weiteren Betriebsumgebung, in der IoT Edge-Anwendungen zusätzlich zu Linux und Windows ausgeführt werden können.

## <a name="architecture"></a>Aufbau 
Auf Kubernetes stellt IoT Edge *Custom Resource Definition* (CRD) für Edge-Workloadbereitstellungen bereit. Der IoT Edge-Agent übernimmt die Rolle eines *CRD-Controllers*, der den von der Cloud verwalteten Sollzustand mit dem lokalen Clusterzustand abgleicht.

Die Lebensdauer der Module wird vom Kubernetes-Scheduler verwaltet, der die Verfügbarkeit der Module aufrechterhält und deren Platzierung wählt. IoT Edge verwaltet die darüber laufende Edge-Anwendungsplattform und gleicht kontinuierlich den im IoT Hub spezifizierten Sollzustand mit dem Zustand auf dem Edge-Cluster ab. Das Anwendungsmodell ist nach wie vor das bekannte Modell, das auf IoT Edge-Modulen und -Routen basiert. Der IoT Edge-Agent-Controller führt die *automatische* Übersetzung des IoT Edge-Anwendungsmodells in die nativen Kubernetes-Konstrukte wie Pods, Bereitstellungen, Dienste usw. durch.

Hier ist ein allgemeines Architekturdiagramm:

![Kubernetes-Bogen](./media/how-to-install-iot-edge-kubernetes/publicpreview-refresh-kubernetes.png)

Jede Komponente der Edge-Bereitstellungen bezieht sich auf einen gerätespezifischen Kubernetes-Namespace, sodass es möglich ist, die gleichen Clusterressourcen für mehrere Edge-Geräte und deren Bereitstellungen zu verwenden.

>[!NOTE]
>IoT Edge auf Kubernetes befindet sich in der [öffentlichen Vorschauversion](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="tutorials-and-references"></a>Tutorials und Referenzen 

Weitere Informationen einschließlich ausführlicher Tutorials und Referenzen finden Sie in der [Vorschaudokumentation zu IoT Edge auf Kubernetes](https://aka.ms/edgek8sdoc).
