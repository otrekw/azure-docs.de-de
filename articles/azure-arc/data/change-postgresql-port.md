---
title: Ändern des PostgreSQL-Ports
description: Ändern des Ports, an dem die Azure Arc-fähige PostgreSQL Hyperscale-Servergruppe lauscht.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 07/30/2021
ms.topic: how-to
ms.openlocfilehash: 47a64b9e1207536e951f61059d472a88e9f9d8c5
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122349807"
---
# <a name="change-the-port-on-which-the-server-group-is-listening"></a>Ändern des Ports, an dem die Servergruppe lauscht 

Das Ändern des Ports ist ein Standardbearbeitungsvorgang der Servergruppe. Führen Sie die folgenden Befehle aus, um den Port zu ändern:
```azurecli
 az postgres arc-server edit -n <server group name> --port <desired port number> --k8s-namespace <namespace> --use-k8s
```

Nehmen wir beispielsweise an, dass der Name Ihrer Servergruppe _postgres01_ lautet, und dass Sie an Port _866_ lauschen möchten. Sie würden den folgenden Befehl ausführen:
```azurecli
 az postgres arc-server edit -n postgres01 --port 866 --k8s-namespace <namespace> --use-k8s
```

## <a name="verify-that-the-port-was-changed"></a>Überprüfen, ob der Port geändert wurde

Um zu überprüfen, ob der Port geändert wurde, führen Sie den folgenden Befehl aus, um die Konfiguration Ihrer Servergruppe anzuzeigen:
```azurecli
az postgres arc-server show -n <server group name> --k8s-namespace <namespace> --use-k8s
```

Sehen Sie sich in der Ausgabe dieses Befehls die für das Element „Port“ im Abschnitt „Dienst“ der Spezifikationen Ihrer Servergruppe angezeigte Portnummer an.
Alternativ können Sie im Element „externalEndpoint“ des Statusabschnitts der Spezifikationen Ihrer Servergruppe überprüfen, ob auf die IP-Adresse die von Ihnen konfigurierte Portnummer folgt.

Zur Illustration: Wenn wir das obige Beispiel fortsetzen, würden Sie den folgenden Befehl ausführen:
```azurecli
az postgres arc-server show -n postgres01 --k8s-namespace <namespace> --use-k8s
```

und hier würden Sie sehen, dass auf den Port 866 verwiesen wird:

```console
"service": {
      "port": 866,
      "type": "LoadBalancer"
    },
```
und hier.

```console
"status": {
    "externalEndpoint": "12.678.345.09:866",
    "logSearchDashboard": "https://12.345.678.90:30777/kibana/app/kibana#/discover?_a=(query:(language:kuery,query:'custom_resource_name:postgres01'))",
    "metricsDashboard": "https://12.345.678.90:30777/grafana/d/postgres-metrics?var-Namespace=arc&var-Name=postgres01",
    "readyPods": "3/3",
    "state": "Ready"
  }
```
## <a name="next-steps"></a>Nächste Schritte
- Informationen zum [Herstellen einer Verbindung mit Ihrer Servergruppe](get-connection-endpoints-and-connection-strings-postgres-hyperscale.md).
- Informieren Sie sich darüber, wie Sie andere Aspekte Ihrer Servergruppe im Abschnitt „Vorgehensweise\Verwalten\Konfigurieren & Skalieren“ der Dokumentation konfigurieren können.
