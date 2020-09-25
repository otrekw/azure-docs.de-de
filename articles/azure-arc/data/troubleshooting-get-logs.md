---
title: Abrufen von Protokollen für die Problembehandlung bei Azure Arc-fähigen Datencontrollern
description: Rufen Sie Dienstprotokolle für die Problembehandlung bei Azure Arc-fähigen Datencontrollern ab.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 71c84b35c001be7fafdc2df53014050ae21dec63
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931297"
---
# <a name="get-azure-arc-enabled-data-services-logs"></a>Abrufen von Protokollen für Azure Arc-fähige Datendienste

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

Um die Protokolle für Azure Arc-fähige Datendienste abzurufen, benötigen Sie das Azure Data CLI-Tool. [Installationsanweisungen](./install-client-tools.md)

Sie müssen in der Lage sein, sich als Administrator beim Controllerdienst für Azure Arc-fähige Datendienste anzumelden.

## <a name="get-azure-arc-enabled-data-services-logs"></a>Abrufen von Protokollen für Azure Arc-fähige Datendienste

Zur Problembehandlung können Sie die Protokolle für Azure Arc-fähige Datendienste zu allen Pods oder bestimmten Pods abrufen.  Dazu können Sie Kubernetes-Standardtools wie den Befehl `kubectl logs` verwenden. In diesem Artikel verwenden Sie alternativ das Azure Data CLI-Tool, das das Abrufen sämtlicher Protokolle auf einmal erleichtert.

Stellen Sie zunächst sicher, dass Sie beim Datencontroller angemeldet sind.

```console
azdata login
```

Führen Sie dann den folgenden Befehl aus, um die Protokolle zu sichern:
```console
azdata arc dc debug copy-logs --namespace <namespace name> --exclude-dumps --skip-compress

#Example:
#azdata arc dc debug copy-logs --namespace arc --exclude-dumps --skip-compress
```

Die Protokolldateien werden standardmäßig im aktuellen Arbeitsverzeichnis im Unterverzeichnis namens „logs“ erstellt.  Mithilfe des Parameters `--target-folder` können Sie die Protokolldateien in ein anderes Verzeichnis ausgeben.

Sie können die Dateien komprimieren, indem Sie den Parameter `--skip-compress` weglassen.

Sie können Speicherabbilder auslösen und einschließen, indem Sie `--exclude-dumps` weglassen. Dies wird jedoch nur empfohlen, wenn Speicherabbilder vom Microsoft-Support angefordert wurden.  Zum Erstellen eines Speicherabbilds muss die Einstellung `allowDumps` des Datencontrollers zum Zeitpunkt der Erstellung des Datencontrollers auf `true` festgelegt sein.

Optional können Sie die Protokolle filtern, um Protokolle nur für einen bestimmten Pod (`--pod`) oder Container (`--container`) nach Namen zu erfassen.

Sie können auch Filter anwenden, um Protokolle für eine bestimmte benutzerdefinierte Ressource zu erfassen, indem Sie die Parameter `--resource-kind` und `--resource-name` übergeben.  Der Parameterwert `resource-kind` muss einem der Namen der benutzerdefinierten Ressourcendefinitionen entsprechen, die mit dem Befehl `kubectl get customresourcedefinition` abgerufen werden können.

```console
azdata arc dc debug copy-logs --target-folder <desired folder> --exclude-dumps --skip-compress -resource-kind <custom resource definition name> --resource-name <resource name> --namespace <namespace name>

#Example
#azdata arc dc debug copy-logs --target-folder C:\temp\logs --exclude-dumps --skip-compress --resource-kind postgresql-12 --resource-name pg1 --namespace arc
```

Beispiel für eine Ordnerhierarchie.  Beachten Sie, dass die Ordnerhierarchie nach Podnamen, dann nach Containern und dann nach der Verzeichnishierarchie innerhalb des Containers organisiert ist.

```console
<export directory>
├───debuglogs-arc-20200827-180403
│   ├───bootstrapper-vl8j2
│   │   └───bootstrapper
│   │       ├───apt
│   │       └───fsck
│   ├───control-j2dm5
│   │   ├───controller
│   │   │   └───controller
│   │   │       ├───2020-08-27
│   │   │       └───2020-08-28
│   │   └───fluentbit
│   │       ├───agent
│   │       ├───fluentbit
│   │       └───supervisor
│   │           └───log
│   ├───controldb-0
│   │   ├───fluentbit
│   │   │   ├───agent
│   │   │   ├───fluentbit
│   │   │   └───supervisor
│   │   │       └───log
│   │   └───mssql-server
│   │       ├───agent
│   │       ├───mssql
│   │       ├───mssql-server
│   │       └───supervisor
│   │           └───log
│   ├───controlwd-ln6j8
│   │   └───controlwatchdog
│   │       └───controlwatchdog
│   ├───logsdb-0
│   │   └───elasticsearch
│   │       ├───agent
│   │       ├───elasticsearch
│   │       ├───provisioner
│   │       └───supervisor
│   │           └───log
│   ├───logsui-7gg2d
│   │   └───kibana
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───kibana
│   │       └───supervisor
│   │           └───log
│   ├───metricsdb-0
│   │   └───influxdb
│   │       ├───agent
│   │       ├───influxdb
│   │       └───supervisor
│   │           └───log
│   ├───metricsdc-2f62t
│   │   └───telegraf
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───supervisor
│   │       │   └───log
│   │       └───telegraf
│   ├───metricsdc-jznd2
│   │   └───telegraf
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───supervisor
│   │       │   └───log
│   │       └───telegraf
│   ├───metricsdc-n5vnx
│   │   └───telegraf
│   │       ├───agent
│   │       ├───apt
│   │       ├───fsck
│   │       ├───supervisor
│   │       │   └───log
│   │       └───telegraf
│   ├───metricsui-h748h
│   │   └───grafana
│   │       ├───agent
│   │       ├───grafana
│   │       └───supervisor
│   │           └───log
│   └───mgmtproxy-r5zxs
│       ├───fluentbit
│       │   ├───agent
│       │   ├───fluentbit
│       │   └───supervisor
│       │       └───log
│       └───service-proxy
│           ├───agent
│           ├───nginx
│           └───supervisor
│               └───log
└───debuglogs-kube-system-20200827-180431
    ├───coredns-8bbb65c89-kklt7
    │   └───coredns
    ├───coredns-8bbb65c89-z2vvr
    │   └───coredns
    ├───coredns-autoscaler-5585bf8c9f-g52nt
    │   └───autoscaler
    ├───kube-proxy-5c9s2
    │   └───kube-proxy
    ├───kube-proxy-h6x56
    │   └───kube-proxy
    ├───kube-proxy-nd2b7
    │   └───kube-proxy
    ├───metrics-server-5f54b8994-vpm5r
    │   └───metrics-server
    └───tunnelfront-db87f4cd8-5xwxv
        ├───tunnel-front
        │   ├───apt
        │   └───journal
        └───tunnel-probe
            ├───apt
            ├───journal
            └───openvpn
```