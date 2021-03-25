---
title: Auflisten von Azure Arc-fähigen, in einem Azure Arc-Datencontroller erstellten PostgreSQL Hyperscale-Servergruppen
description: Auflisten von Azure Arc-fähigen, in einem Azure Arc-Datencontroller erstellten PostgreSQL Hyperscale-Servergruppen
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 7577ca4b8a1d7db7ea99aadfef4fd2a445b66425
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90931799"
---
# <a name="list-the-azure-arc-enabled-postgresql-hyperscale-server-groups-created-in-an-azure-arc-data-controller"></a>Auflisten von Azure Arc-fähigen, in einem Azure Arc-Datencontroller erstellten PostgreSQL Hyperscale-Servergruppen

In diesem Artikel wird erläutert, wie Sie die Liste der Servergruppen abrufen, die in Ihrem Azure Arc-Datencontroller erstellt wurden.

Verwenden Sie zum Abrufen eine der folgenden Methoden, nachdem Sie eine Verbindung mit dem Azure Arc-Datencontroller hergestellt haben:

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="from-cli-with-azdata"></a>Über die CLI mit azdata
Das allgemeine Befehlsformat lautet:
```console
azdata arc postgres server list
```

Dabei wird eine Ausgabe wie die folgende zurückgegeben:
```console
Name        State    Workers
----------  -------  ---------
postgres01  Ready    2
postgres02  Ready    2
```
Weitere Informationen zu den für diesen Befehl verfügbaren Parametern erhalten Sie, wenn Sie Folgendes ausführen:
```console
azdata arc postgres server list --help
```

## <a name="from-cli-with-kubectl"></a>Über die CLI mit kubectl
Führen Sie einen der folgenden Befehle aus.

**Führen Sie folgenden Befehl aus, um die Servergruppen unabhängig von der Postgres-Version aufzulisten:**
```console
kubectl get postgresqls
```
Dabei wird eine Ausgabe wie die folgende zurückgegeben:
```console
NAME                                             STATE   READY-PODS   EXTERNAL-ENDPOINT   AGE
postgresql-12.arcdata.microsoft.com/postgres01   Ready   3/3          10.0.0.4:30499      51s
postgresql-12.arcdata.microsoft.com/postgres02   Ready   3/3          10.0.0.4:31066      6d
```

**Führen Sie folgenden Befehl aus, um die Servergruppen für eine bestimmte Postgres-Version aufzulisten:**
```console
kubectl get postgresql-12
```

Ersetzen Sie zum Auflisten der Servergruppen, in denen Version 11 von Postgres ausgeführt wird, _postgresql-12_ durch _postgresql-11_.

## <a name="next-steps"></a>Nächste Schritte:

* [Lesen Sie den Artikel zum Abrufen der Verbindungsendpunkte, und erstellen Sie die Verbindungszeichenfolgen, um eine Verbindung mit Ihrer Servergruppe herzustellen.](get-connection-endpoints-and-connection-strings-postgres-hyperscale.md)
* [Lesen Sie den Artikel zur Konfiguration einer Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppe.](show-configuration-postgresql-hyperscale-server-group.md)
