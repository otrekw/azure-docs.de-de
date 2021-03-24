---
title: Beenden der Überwachung Ihres Azure- und Red Hat OpenShift v4-Clusters | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Überwachung Ihres Azure Red Hat OpenShift- und Red Hat OpenShift Version 4-Clusters mit Container Insights beenden können.
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: 09ca05a25ce9bb02b8a3d515acf060e2e9e7e8c2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101731798"
---
# <a name="how-to-stop-monitoring-your-azure-and-red-hat-openshift-v4-cluster"></a>Beenden der Überwachung Ihres Azure- und Red Hat OpenShift v4-Clusters

Die Überwachung Ihres Azure Red Hat OpenShift- und Red Hat OpenShift Version 4.x-Clusters kann nach der Aktivierung wieder beendet werden, wenn Sie den Cluster nicht mehr mit Container Insights überwachen möchten. In diesem Artikel wird beschrieben, wie Sie dies erreichen.  

## <a name="how-to-stop-monitoring-using-helm"></a>Beenden der Überwachung mithilfe von Helm

1. Führen Sie den folgenden Helm-Befehl aus, um zunächst das auf Ihrem Cluster installierte Container Insights-Helm-Chartrelease zu identifizieren.

    ```
    helm list
    ```

    Die Ausgabe ähnelt der folgenden:

    ```
    NAME                            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
    azmon-containers-release-1      default         3               2020-04-21 15:27:24.1201959 -0700 PDT   deployed        azuremonitor-containers-2.7.0   7.0.0-1
    ```

    *azmon-containers-release-1* stellt das Helm-Chartrelease für Container Insights dar.

2. Führen Sie den folgenden Helm-Befehl aus, um das Chartrelease zu löschen.

    `helm delete <releaseName>`

    Beispiel:

    `helm delete azmon-containers-release-1`

    Dadurch wird das Release aus dem Cluster entfernt. Sie können dies überprüfen, indem Sie den `helm list`-Befehl ausführen:

    ```
    NAME                            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
    ```

Die Änderung der Konfiguration kann einige Minuten dauern. Da Helm Ihre Releases auch nach dem Löschen nachverfolgt, können Sie den Verlauf eines Clusters überwachen und ein Release mit `helm rollback` sogar wiederherstellen.

## <a name="next-steps"></a>Nächste Schritte

Wenn der Log Analytics-Arbeitsbereich nur als Unterstützung für die Clusterüberwachung erstellt wurde und nicht mehr benötigt wird, müssen Sie ihn manuell löschen. Wie ein Arbeitsbereich gelöscht wird, erfahren Sie unter [Löschen eines Log Analytics-Arbeitsbereichs](../logs/delete-workspace.md).