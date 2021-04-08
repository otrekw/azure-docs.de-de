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
ms.openlocfilehash: 6e2443014f6788504a11784945078187a5a72de4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98985868"
---
# <a name="configure-azure-arc-enabled-sql-managed-instance"></a>Konfigurieren von Azure Arc-fähigen SQL Managed Instance-Instanzen

In diesem Artikel wird erläutert, wie eine Azure Arc-fähige SQL Managed Instance-Instanz konfiguriert wird.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="configure-resources"></a>Konfigurieren der Ressourcen

### <a name="configure-using-azure-data-cli-azdata"></a>Konfigurieren mithilfe der [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]

Sie können die Konfiguration von Azure Arc-fähigen SQL Managed Instance-Instanzen mithilfe der [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] bearbeiten. Führen Sie den folgenden Befehl aus, um die Konfigurationsoptionen anzuzeigen. 

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
