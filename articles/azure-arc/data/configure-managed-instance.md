---
title: Konfigurieren von Azure Arc-fähigen SQL Managed Instance-Instanzen
description: Konfigurieren von Azure Arc-fähigen SQL Managed Instance-Instanzen
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: vin-yu
ms.author: vinsonyu
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: f687923f1185d848c2c4f2f40d949c71a7743d65
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931773"
---
# <a name="configure-azure-arc-enabled-sql-managed-instance"></a>Konfigurieren von Azure Arc-fähigen SQL Managed Instance-Instanzen

In diesem Artikel wird erläutert, wie eine Azure Arc-fähige SQL Managed Instance-Instanz konfiguriert wird.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="configure-resources-for-azure-arc-enabled-sql-managed-instance"></a>Konfigurieren von Ressourcen für eine Azure Arc-fähige SQL Managed Instance-Instanz

### <a name="configure-using-azdata"></a>Konfigurieren mithilfe von azdata

Sie können die Konfiguration von Azure Arc-fähigen SQL Managed Instance-Instanzen mit der `azdata` CLI bearbeiten. Führen Sie den folgenden Befehl aus, um die Konfigurationsoptionen anzuzeigen. 

```
azdata arc sql mi edit --help
```

Im folgenden Beispiel werden die CPU-Kern- und Arbeitsspeicheranforderungen und Grenzwerte festgelegt.

```
azdata arc sql mi edit --cores-limit 4 --cores-request 2 --memory-limit 4Gi --memory-request 2Gi -n <NAME_OF_SQL_MI>
```

Zum Anzeigen der Änderungen, die an der SQL Managed Instance-Instanz vorgenommen wurden, können Sie die folgenden Befehle verwenden, um die YAML-Konfigurationsdatei anzuzeigen:

```
azdata arc sql mi show -n <NAME_OF_SQL_MI>
```

## <a name="configure-server-options"></a>Konfigurieren von Serveroptionen

Nach der Erstellung können Sie Serverkonfigurationseinstellungen für die Azure Arc-fähige SQL Managed Instance-Instanz konfigurieren. In diesem Artikel wird beschrieben, wie Einstellungen wie das Aktivieren oder Deaktivieren des MSSQL-Agents oder das Aktivieren bestimmter Ablaufverfolgungsflags für Problembehandlungsszenarien festgelegt werden können.

Führen Sie die folgenden Schritte aus, um diese Einstellungen zu konfigurieren:

1. Erstellen Sie eine benutzerdefinierte `mssql-custom.conf`-Datei, die die gewünschten Einstellungen enthält. Im folgenden Beispiel werden der SQL-Agent und das Ablaufverfolgungsflag 1204 aktiviert:

   ```
   [sqlagent]
   enabled=true
   
   [traceflag]
   traceflag0 = 1204
   ```

1. Kopieren Sie die `mssql-custom.conf`-Datei in `/var/opt/mssql` im `mssql-miaa`-Container, der sich im `master-0`-Pod befindet. Ersetzen Sie `<namespaceName>` durch den Namen des Big Data-Clusters.

   ```bash
   kubectl cp mssql-custom.conf master-0:/var/opt/mssql/mssql-custom.conf -c mssql-server -n <namespaceName>
   ```

1. Starten Sie die SQL Server-Instanz neu.  Ersetzen Sie `<namespaceName>` durch den Namen des Big Data-Clusters.

   ```bash
   kubectl exec -it master-0  -c mssql-server -n <namespaceName> -- /bin/bash
   supervisorctl restart mssql-server
   exit
   ```


**Bekannte Einschränkungen**
- Für die oben genannten Schritte sind Administratorberechtigungen für den Kubernetes-Cluster erforderlich.
- Dies kann sich während der Vorschau ändern.
