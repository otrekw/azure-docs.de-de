---
title: Zentrales Hoch- und Herunterskalieren einer Azure Database for PostgreSQL Hyperscale-Servergruppe mithilfe der CLI (azdata oder kubectl)
description: Zentrales Hoch- und Herunterskalieren einer Azure Database for PostgreSQL Hyperscale-Servergruppe mithilfe der CLI (azdata oder kubectl)
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 303a919cc0afc9b5db49918233f3e5718a896646
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2020
ms.locfileid: "92148044"
---
# <a name="scale-up-and-down-an-azure-database-for-postgresql-hyperscale-server-group-using-cli-azdata-or-kubectl"></a>Zentrales Hoch- und Herunterskalieren einer Azure Database for PostgreSQL Hyperscale-Servergruppe mithilfe der CLI (azdata oder kubectl)



Es kann vorkommen, dass Sie die Merkmale oder die Definition einer Servergruppe ändern müssen. Zum Beispiel:

- Zentrales Hoch- oder Herunterskalieren der Anzahl von virtuellen Kernen, die von den einzelnen Koordinatorknoten oder Workerknoten verwendet werden
- Zentrales Hoch- oder Herunterskalieren des Speichers, der von den einzelnen Koordinatorknoten oder Workerknoten verwendet wird

In diesem Leitfaden wird erläutert, wie Sie virtuelle Kerne und/oder Arbeitsspeicher skalieren.

Wenn Sie die Einstellungen der virtuellen Kerne oder des Arbeitsspeichers der Servergruppe zentral hoch-oder herunterskalieren, haben Sie die Möglichkeit, für jede Einstellung der virtuellen Kerne oder des Arbeitsspeichers einen Minimal- und/oder einen Maximalwert festzulegen. Wenn Sie die Servergruppe so konfigurieren möchten, dass eine bestimmte Anzahl von virtuellen Kernen oder eine bestimmte Menge an Arbeitsspeicher verwendet wird, legen Sie die Minimalwerteinstellungen auf die Maximalwerteinstellungen fest.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="show-the-current-definition-of-the-server-group"></a>Anzeigen der aktuellen Definition der Servergruppe

Führen Sie einen der folgenden Befehle aus, um die aktuelle Definition der Servergruppe und die aktuellen Einstellungen für virtuelle Kerne und den Arbeitsspeicher anzuzeigen:

### <a name="cli-with-azdata"></a>CLI mit azdata

```console
azdata arc postgres server show -n <server group name>
```
### <a name="cli-with-kubectl"></a>CLI mit kubectl

```console
kubectl describe postgresql-12/<server group name> [-n <namespace name>]
```
> [!NOTE]
> Wenn Sie eine Servergruppe mit PostgreSQL-Version 11 erstellt haben, führen Sie stattdessen `kubectl describe postgresql-11/<server group name>` aus.

Die Konfiguration der Servergruppe wird zurückgegeben. Wenn Sie die Servergruppe mit den Standardeinstellungen erstellt haben, sollte die Definition wie folgt angezeigt werden:

```console
"scheduling": {
      "default": {
        "resources": {
          "requests": {
            "memory": "256Mi"
          }
        }
      }
    },
```

## <a name="interpret-the-definition-of-the-server-group"></a>Interpretieren der Definition der Servergruppe

In der Definition einer Servergruppe ist der Abschnitt, der die Einstellungen der minimalen/maximalen Anzahl virtueller Kerne pro Knoten und die minimale/maximale Größe des Arbeitsspeichers pro Knoten festlegt, der Abschnitt **„scheduling“** . In diesem Abschnitt werden die maximalen Einstellungen in einem Unterabschnitt namens **„limits“** und die minimalen Einstellungen im Unterabschnitt **„requests“** persistent gespeichert.

Wenn Sie minimale Einstellungen festlegen, die sich von den maximalen Einstellungen unterscheiden, gewährleistet die Konfiguration, dass der Servergruppe bei Bedarf die angeforderten Ressourcen zugewiesen werden. Die von Ihnen festgelegten Grenzwerte werden nicht überschritten.

Die Ressourcen (virtuelle Kerne und Arbeitsspeicher), die tatsächlich von der Servergruppe verwendet werden, entsprechen den maximalen Einstellungen und sind von den Workloads und den Ressourcen abhängig, die auf dem Cluster verfügbar sind. Wenn Sie die Einstellungen nicht durch ein Maximum begrenzen, kann Ihre Servergruppe alle Ressourcen verwenden, die der Kubernetes-Cluster den Kubernetes-Knoten zuweist, auf denen Ihre Servergruppe geplant ist.

Diese Einstellungen für virtuelle Kerne und Arbeitsspeicher gelten für die einzelnen PostgreSQL Hyperscale-Knoten (Koordinatorknoten und Workerknoten). Das separate Festlegen der Definitionen des Koordinatorknotens und des Workerknotens wird noch nicht unterstützt.

In einer Standardkonfiguration ist nur der Mindestspeicher auf 256Mi festgelegt, da dies die Mindestmenge an Speicher ist, die für die Ausführung von PostgreSQL Hyperscale empfohlen wird.

> [!NOTE]
> Das Festlegen eines Mindestwerts bedeutet nicht, dass die Servergruppe diesen Mindestwert notwendigerweise verwendet. Das bedeutet, dass garantiert wird, dass der Servergruppe mindestens dieses Minimum zugewiesen wird, wenn sie es benötigt. Nehmen wir beispielsweise an, dass wir `--minCpu 2` festlegen. Dies bedeutet nicht, dass die Servergruppe immer mindestens 2 virtuelle Kerne verwendet. Es bedeutet vielmehr, dass die Servergruppe weniger als 2 virtuelle Kerne verwenden kann, wenn sie nicht so viel benötigt, und es wird garantiert, dass mindestens 2 virtuelle Kerne zugewiesen werden, wenn diese später benötigt werden. Dies impliziert, dass der Kubernetes-Cluster Ressourcen anderen Workloads so zuordnet, dass er der Servergruppe 2 virtuelle Kerne zuordnen kann, wenn sie benötigt werden.

>[!NOTE]
>Bevor Sie die Konfiguration Ihres Systems ändern, stellen Sie sicher, dass Sie sich [hier](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities) mit dem Kubernetes-Ressourcenmodell vertraut machen.

## <a name="scale-up-the-server-group"></a>Zentrales Hochskalieren der Servergruppe

Die Einstellungen, die Sie festlegen, müssen innerhalb der Konfiguration berücksichtigt werden, die Sie für den Kubernetes-Cluster festlegen. Stellen Sie sicher, dass Sie keine Werte festlegen, die ihr Kubernetes-Cluster nicht erfüllen kann. Dies kann zu Fehlern oder unvorhersehbarem Verhalten führen. Wenn der Status Ihrer Servergruppe beispielsweise lange Zeit nach der Änderung der Konfiguration im Status _updating_ (wird aktualisiert) verbleibt, kann das ein Hinweis darauf sein, dass Sie die unten aufgeführten Parameter auf Werte festlegen, die Ihr Kubernetes-Cluster nicht erfüllen kann. Wenn dies der Fall ist, machen Sie die Änderung rückgängig, oder lesen Sie den Abschnitt zur Problembehandlung.

Angenommen, Sie möchten beispielsweise die Definition der Servergruppe wie folgt zentral hochskalieren:

- Min vCore = 2 (Minimale Anzahl virtueller Kerne = 2)
- Max vCore = 4 (Maximale Anzahl virtueller Kerne = 4)
- Min memory = 512Mb (Mindestwert Arbeitsspeicher = 512 MB)
- Max Memory = 1Gb (Maximalwert Arbeitsspeicher = 1 GB)

Sie können einen der folgenden Ansätze verwenden:

### <a name="cli-with-azdata"></a>CLI mit azdata

```console
azdata arc postgres server edit -n <name of your server group> --cores-request <# core-request>  --cores-limit <# core-limit>  --memory-request <# memory-request>Mi  --memory-limit <# memory-limit>Mi
```

> [!CAUTION]
> Das folgende Beispiel veranschaulicht, wie Sie den Befehl verwenden können. Stellen Sie vor dem Ausführen eines Bearbeitungsbefehls sicher, dass die Parameter auf Werte festgelegt sind, die der Kubernetes-Cluster akzeptieren kann.

```console
azdata arc postgres server edit -n <name of your server group> --cores-request 2  --cores-limit 4  --memory-request 512Mi  --memory-limit 1024Mi
```

Der Befehl wird erfolgreich ausgeführt, wenn Folgendes angezeigt wird:

```console
<name of your server group> is Ready
```

> [!NOTE]
> Weitere Informationen zu diesen Parametern erhalten Sie, wenn Sie `azdata arc postgres server edit --help` ausführen.

### <a name="cli-with-kubectl"></a>CLI mit kubectl

```console
kubectl edit postgresql-12/<server group name> [-n <namespace name>]
```

Sie gelangen in den vi-Editor, in dem Sie navigieren und die Konfiguration ändern können. Verwenden Sie Folgendes, um die gewünschte Einstellung dem Namen des Felds in der Spezifikation zuzuordnen:

> [!CAUTION]
> Das folgende Beispiel veranschaulicht, wie Sie die Konfiguration bearbeiten können. Stellen Sie vor dem Aktualisieren der Konfiguration sicher, dass die Parameter auf Werte festgelegt sind, die der Kubernetes-Cluster akzeptieren kann.

Beispiel:
- Min vCore = 2 -> scheduling\default\resources\requests\cpu
- Max vCore = 4 -> scheduling\default\resources\limits\cpu
- Min memory = 512Mb -> scheduling\default\resources\requests\cpu
- Max Memory = 1Gb ->  scheduling\default\resources\limits\cpu

Wenn Sie mit dem vi-Editor nicht vertraut sind, finden Sie [hier](https://www.computerhope.com/unix/uvi.htm) eine Beschreibung der Befehle, die Sie möglicherweise benötigen:
- Bearbeitungsmodus: `i`
- Navigation mit Pfeiltasten
- _stop editing (Bearbeitung beenden): `esc`
- _exit without saving (ohne Speichern beenden): `:qa!`
- _exit after saving (nach Speichern beenden): `:qw!`


## <a name="show-the-scaled-up-definition-of-the-server-group"></a>Anzeigen der hochskalierten Definition der Servergruppe

Führen Sie den Befehl erneut aus, um die Definition der Servergruppe anzuzeigen, und vergewissern Sie sich, dass sie wie gewünscht festgelegt ist:

### <a name="cli-with-azdata"></a>CLI mit azdata

```console
azdata arc postgres server show -n <the name of your server group>
```
### <a name="cli-with-kubectl"></a>CLI mit kubectl

```console
kubectl describe postgresql-12/<server group name>  [-n <namespace name>]
```
> [!NOTE]
> Wenn Sie eine Servergruppe mit PostgreSQL-Version 11 erstellt haben, führen Sie stattdessen `kubectl describe postgresql-11/<server group name>` aus.


Die neue Definition der Servergruppe wird angezeigt:

```console
"scheduling": {
      "default": {
        "resources": {
          "limits": {
            "cpu": "4",
            "memory": "1024Mi"
          },
          "requests": {
            "cpu": "2",
            "memory": "512Mi"
          }
        }
      }
    },
```

## <a name="scale-down-the-server-group"></a>Herunterskalieren der Servergruppe

Zum horizontalen Herunterskalieren der Servergruppe führen Sie denselben Befehl aus, legen jedoch für die Einstellungen, die Sie zentral herunterskalieren möchten, geringere Werte fest. Um die Anforderungen und/oder Grenzwerte zu entfernen, geben Sie die entsprechenden Werte als leere Zeichenfolge an.

## <a name="next-steps"></a>Nächste Schritte

- [Skalieren Ihrer Azure Database for PostgreSQL Hyperscale-Servergruppe](scale-out-postgresql-hyperscale-server-group.md)
- [Speicherkonfiguration und Kubernetes-Speicherkonzepte](storage-configuration.md)
- [Kubernetes-Ressourcenmodell](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities)
