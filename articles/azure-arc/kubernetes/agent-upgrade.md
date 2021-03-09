---
title: Upgrade für Kubernetes-Agents mit Azure Arc-Aktivierung
services: azure-arc
ms.service: azure-arc
ms.date: 03/03/2021
ms.topic: article
author: shashankbarsin
ms.author: shasb
description: Steuern von Agent-Upgrades für Kubernetes mit Azure Arc-Aktivierung
keywords: Kubernetes, Arc, Azure, K8s, Container, Agent, Upgrade
ms.openlocfilehash: 172654f655e594c295a8807b417c32d7849bd4b1
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102121914"
---
# <a name="upgrading-azure-arc-enabled-kubernetes-agents"></a>Upgrade für Kubernetes-Agents mit Azure Arc-Aktivierung

Kubernetes mit Azure Arc-Aktivierung bietet Funktionen für automatische und manuelle Upgrades für seine Agents. Wenn Sie die Deaktivierung automatischer Upgrades verwenden und sich stattdessen auf manuelle Upgrades verlassen, gilt die Richtlinie zur Versionsunterstützung für Arc-Agents und den zugrunde liegenden Kubernetes-Cluster.

## <a name="toggle-auto-upgrade-on-or-off-when-connecting-cluster-to-azure-arc"></a>Aktivieren/Deaktivieren von automatischen Upgrades beim Herstellen einer Verbindung zwischen Cluster und Azure Arc

Kubernetes mit Azure Arc-Aktivierung bietet seinen Agents standardmäßig verfügbare Funktionen für automatisches Upgrades.

Der folgende Befehl verbindet einen Cluster mit Azure Arc mit **aktiviertem** automatischen Upgrade:

```console
az connectedk8s connect --name AzureArcTest1 --resource-group AzureArcTest
```

Bei aktiviertem automatischem Upgrade fragt der Agent stündlich Azure ab, ob eine neuere Version der Agents verfügbar ist. Wenn der Agent eine verfügbare neuere Version findet, löst er ein Helm-Chart-Upgrade für die Azure Arc-Agents aus.

Um das automatische Upgrade zu deaktivieren, geben Sie den Parameter `--disable-auto-upgrade` an, während Sie den Cluster mit Azure Arc verbinden. Der folgende Befehl verbindet einen Cluster mit Azure Arc mit **deaktiviertem** automatischen Upgrade:

```console
az connectedk8s connect --name AzureArcTest1 --resource-group AzureArcTest --disable-auto-upgrade
```

> [!TIP]
> Wenn Sie planen, das automatische Upgrade zu deaktivieren, lesen Sie die [Richtlinie zur Versionsunterstützung](#version-support-policy) für Kubernetes mit Azure Arc-Aktivierung.

## <a name="toggle-auto-upgrade-onoff-after-connecting-cluster-to-azure-arc"></a>Aktivieren/Deaktivieren von automatischen Upgrades nach dem Herstellen einer Verbindung zwischen Cluster und Azure Arc

Nachdem Sie eine Verbindung zwischen Cluster und Azure Arc hergestellt haben, können Sie die Funktion für automatische Upgrades mit dem unten gezeigten Befehl `az connectedk8s update` aktivieren:

```console
az connectedk8s update --name AzureArcTest1 --resource-group AzureArcTest --auto-upgrade false
```

## <a name="manually-upgrade-agents"></a>Manuelles Upgrade von Agents

Wenn Sie das automatische Upgrade für Agents deaktiviert haben, können Sie Upgrades für diese Agenten manuell initiieren, indem Sie den Befehl `az connectedk8s upgrade` folgendermaßen verwenden:

```console
az connectedk8s upgrade -g AzureArcTest1 -n AzureArcTest --agent-version 1.0.1
```

Kubernetes mit Azure Arc-Aktivierung folgt dem standardmäßigen [semantischen Versionsverwaltungsschema](https://semver.org/) von `MAJOR.MINOR.PATCH` für die Versionsverwaltung seiner Agents. 

Für jede Zahl in der Version gilt, dass allgemeine Kompatibilität mit der vorherigen Version besteht:

* **Hauptversionen** werden geändert, wenn es inkompatible API-Updates gibt oder die Abwärtskompatibilität fehlerhaft ist.
* **Nebenversionen** werden geändert, wenn Funktionsänderungen hinsichtlich anderer kleinerer Releases abwärtskompatibel sind.
* **Patchversionen** werden geändert, wenn abwärtskompatible Fehlerbehebungen vorgenommen werden.

## <a name="version-support-policy"></a>Richtlinie zur Versionsunterstützung

Wenn Sie Supportprobleme erzeugen, praktiziert Kubernetes mit Azure Arc-Aktivierung die folgende Richtlinie zur Versionsunterstützung:

* Kubernetes-Agents mit Azure Arc-Aktivierung verfügen über ein Supportfenster von „N-2“, wobei „N“ das letzte kleinere Release von Agents darstellt. 
  * Wenn Kubernetes mit Azure Arc-Aktivierung heute z. B. „0.28.a“ einführt, werden die Versionen „0.28.a“, „0.28.b“, „0.27.c“, „0.27.d“, „0.26.e“ und „0.26.f“ von Azure Arc unterstützt.

* Kubernetes-Cluster, die eine Verbindung mit Azure Arc herstellen, besitzen ein Supportfenster von „N-2“, wobei „N“ das letzte stabile kleinere Release der [Upstreamversion von Kubernetes](https://github.com/kubernetes/kubernetes/releases) ist. 
  * Wenn Kubernetes heute z. B. „1.20.a“ einführt, werden die Versionen „1.20.a“, „1.20.b“, „1.19.c“, „1.19.d“, „1.18.e“ und „1.18.f“ unterstützt.

### <a name="how-often-are-minor-version-releases-of-azure-arc-enabled-kubernetes-available"></a>Wie oft werden Releases von Nebenversionen von Kubernetes mit Azure Arc-Aktivierung veröffentlicht?

Etwa einmal im Monat wird eine Nebenversion von Kubernetes-Agents mit Azure Arc-Aktivierung veröffentlicht.

### <a name="what-happens-if-im-using-an-agent-version-or-a-kubernetes-version-outside-the-official-support-window"></a>Was geschieht, wenn ich eine Agent-Version oder eine Kubernetes-Version außerhalb des offiziellen Supportfensters verwende?

„Außerhalb des Supportfensters“ bedeutet, dass die von Ihnen ausgeführten Versionen außerhalb der unterstützten „N-2“-Versionen der Agents und Kubernetes-Upstreamclustern liegen. Um mit dem Supportproblem fortzufahren, werden Sie aufgefordert, für den Cluster und die Agents ein Upgrade auf eine unterstützte Version durchzuführen.

## <a name="next-steps"></a>Nächste Schritte

* Führen Sie den Schnellstart zum [Verbinden eines Kubernetes-Clusters mit Azure Arc](./connect-cluster.md) durch.
* Sie haben bereits einen Kubernetes-Cluster, der mit Azure Arc verbunden ist? [Erstellen Sie Konfigurationen in Ihrem Arc-fähigen Kubernetes-Cluster](./use-gitops-connected-cluster.md).
* Erfahren Sie, wie Sie [Azure Policy zum Anwenden von Konfigurationen im großen Stil verwenden](./use-azure-policy.md).