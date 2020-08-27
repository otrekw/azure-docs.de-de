---
title: Manuelles Beenden oder Starten von Containergruppen
description: Hier erfahren Sie, wie Sie eine Containergruppe in Azure Container Instances manuell beenden oder starten.
ms.topic: article
ms.date: 08/11/2020
ms.openlocfilehash: f697f18459959cf40e71d3ca90cd8b42aaa74239
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88799076"
---
# <a name="manually-stop-or-start-containers-in-azure-container-instances"></a>Manuelles Beenden oder Starten von Containern in Azure Container Instances

Die Einstellung [Neustartrichtlinie](container-instances-restart-policy.md) einer Containergruppe bestimmt, wie Containerinstanzen standardmäßig gestartet oder beendet werden. Sie können die Standardeinstellung außer Kraft setzen, indem Sie eine Containergruppe manuell beenden oder starten.

[!INCLUDE [container-instances-restart-ip](../../includes/container-instances-restart-ip.md)]

## <a name="stop"></a>Beenden

Sie können eine ausgeführte Containergruppe manuell beenden, indem Sie z. B. den Befehl [az container stop][az-container-stop] oder das Azure-Portal verwenden. Für bestimmte Containerworkloads möchten Sie möglicherweise eine zeitintensive Containergruppe nach einem definierten Zeitraum beenden, um Kosten zu sparen. 

*Wenn eine Containergruppe in den Zustand „Beendet“ wechselt, wird sie beendet und verwendet alle Container in der Gruppe wieder. Der Containerzustand bleibt nicht erhalten.*

Wenn die Container wiederverwendet werden, wird die Zuordnung der [Ressourcen](container-instances-container-groups.md#resource-allocation) aufgehoben und die Abrechnung für die Containergruppe beendet.

Die Aktion „Beenden“ hat keine Auswirkungen, wenn die Containergruppe bereits beendet wurde (also im Zustand „Erfolgreich“ oder „Fehler“ ist). Angenommen, eine Containergruppe mit einmalig auszuführenden Containertasks, die erfolgreich ausgeführt wurden, wird im Zustand „Erfolgreich“ beendet. Versuche, die Gruppe in diesem Zustand zu beenden, ändern den Zustand nicht. 

## <a name="start"></a>Start

Wenn eine Containergruppe beendet ist (weil die Container einzeln beendet wurden oder weil Sie die Gruppe manuell beendet haben), können Sie die Container starten. Verwenden Sie beispielsweise den Befehl [az container stop][az-container-start] oder das Azure-Portal, um die Container in der Gruppe manuell zu starten. Wenn das Containerimage für einen Container aktualisiert wird, wird ein neues Image gepullt. 

Durch das Starten einer Containergruppe beginnt eine neue Bereitstellung mit der gleichen Containerkonfiguration. Diese Aktion kann Ihnen helfen, eine bekannte Containergruppenkonfiguration, die wie erwartet funktioniert, schnell wiederzuverwenden. Sie müssen keine neue Containergruppe erstellen, um die gleiche Workload auszuführen.

Alle Container in einer Containergruppe werden durch diese Aktion gestartet. Einen bestimmten Container in der Gruppe können Sie nicht starten.

Nachdem Sie eine Containergruppe manuell gestartet oder neu gestartet haben, wird die Containergruppe gemäß der konfigurierten Neustartrichtlinie ausgeführt.
  
## <a name="restart"></a>Neu starten

Sie können eine Containergruppe neu starten, während sie ausgeführt wird, indem Sie z. B. den Befehl [az container restart][az-container-restart] verwenden. Diese Aktion startet alle Container in der Containergruppe neu. Wenn das Containerimage für einen Container aktualisiert wird, wird ein neues Image gepullt. 

Der Neustart einer Containergruppe ist hilfreich, wenn Sie ein Bereitstellungsproblem beheben möchten. Wenn beispielsweise eine temporäre Ressourcenbeschränkung die erfolgreiche Ausführung Ihrer Container verhindert, kann ein Neustart der Gruppe das Problem beheben.

Alle Container in einer Containergruppe werden durch diese Aktion neu gestartet. Einen bestimmten Container in der Gruppe können Sie nicht neu starten.

Nachdem Sie eine Containergruppe manuell neu gestartet haben, wird die Containergruppe gemäß der konfigurierten Neustartrichtlinie ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Neustartrichtlinieneinstellungen](container-instances-restart-policy.md) in Azure Container Instances.

Zusätzlich zum manuellen Beenden und Starten einer Containergruppe mit der vorhandenen Konfiguration können Sie [die Einstellungen einer ausgeführten Containergruppe aktualisieren](container-instances-update.md).

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-container-restart]: /cli/azure/container?view=azure-cli-latest#az-container-restart
[az-container-start]: /cli/azure/container?view=azure-cli-latest#az-container-start
[az-container-stop]: /cli/azure/container?view=azure-cli-latest#az-container-stop
