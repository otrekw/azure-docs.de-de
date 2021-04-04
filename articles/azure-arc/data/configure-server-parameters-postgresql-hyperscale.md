---
title: Konfigurieren der Serverparameter für die Postgres-Engine Ihrer PostgreSQL Hyperscale-Servergruppe in Azure Arc
titleSuffix: Azure Arc enabled data services
description: Konfigurieren der Serverparameter für die Postgres-Engine Ihrer PostgreSQL Hyperscale-Servergruppe in Azure Arc
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: cdbddfc84b3f71576cfd0299f2babec859b4ef1f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92311062"
---
# <a name="set-the-database-engine-settings-for-azure-arc-enabled-postgresql-hyperscale"></a>Festlegen der Einstellungen der Datenbank-Engine für Azure Arc-fähiges PostgreSQL Hyperscale

In diesem Dokument wird beschrieben, wie Sie die Einstellungen der Datenbank-Engine Ihrer PostgreSQL Hyperscale-Servergruppe auf benutzerdefinierte (nicht standardmäßige) Werte festlegen. Ausführliche Informationen dazu, welche Parameter für die Datenbank-Engine festgelegt werden können und wie die Standardwerte lauten, finden Sie [hier](https://www.postgresql.org/docs/current/runtime-config.html) in der PostgreSQL-Dokumentation.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

> [!NOTE]
> Das Festlegen der folgenden Parameter wird in der Vorschau nicht unterstützt: 
>
> - `archive_command`
> - `archive_timeout`
> - `log_directory`
> - `log_file_mode`
> - `log_filename`
> - `restore_command`
> - `shared_preload_libraries`
> - `synchronous_commit`
> - `ssl`
> - `wal_level`

## <a name="syntax"></a>Syntax

Der Befehl zum Konfigurieren der Einstellungen für die Datenbank-Engine hat das folgende allgemeine Format:

```console
azdata arc postgres server edit -n <server group name>, [{--engine-settings, -e}] [{--replace-engine-settings, --re}] {'<parameter name>=<parameter value>, ...'}
```

## <a name="show-current-custom-values"></a>Aktuelle benutzerdefinierte Werte anzeigen

### <a name="with-azure-data-cli-azdata-command"></a>Mit dem Befehl [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]

```console
azdata arc postgres server show -n <server group name>
```

Zum Beispiel:

```console
azdata arc postgres server show -n postgres01
```

Durch diesen Befehl werden die Spezifikationen der Servergruppe zurückgegeben, in der die von Ihnen festgelegten Parameter angezeigt werden sollen. Wenn kein Abschnitt für die Engine/Einstellungen vorhanden ist, bedeutet dies, dass alle Parameter mit dem Standardwert ausgeführt werden:

```console
 "
...
engine": {
      "settings": {
        "default": {
          "autovacuum_vacuum_threshold": "65"
        }
      }
    },
...
```

### <a name="with-kubectl-command"></a>Mit kubectl-Befehl

Führen Sie die folgenden Schritte aus.

1. Rufen Sie die Art der benutzerdefinierten Ressourcendefinition für Ihre Servergruppe ab.

   Führen Sie Folgendes aus:

   ```console
   azdata arc postgres server show -n <server group name>
   ```

   Zum Beispiel:

   ```console
   azdata arc postgres server show -n postgres01
   ```

   Durch diesen Befehl werden die Spezifikationen der Servergruppe zurückgegeben, in der die von Ihnen festgelegten Parameter angezeigt werden sollen. Wenn kein Abschnitt für die Engine/Einstellungen vorhanden ist, bedeutet dies, dass alle Parameter mit dem Standardwert ausgeführt werden:

   ```output
   > {
     >"apiVersion": "arcdata.microsoft.com/v1alpha1",
     >"**kind**": "**postgresql-12**",
     >"metadata": {
       >"creationTimestamp": "2020-08-25T14:32:23Z",
       >"generation": 1,
       >"name": "postgres01",
       >"namespace": "arc",  
   ```

   Suchen Sie in den Ausgabeergebnissen nach dem Feld `kind`, und notieren Sie sich den Wert, beispielsweise `postgresql-12`.

2. Beschreiben Sie die benutzerdefinierte Kubernetes-Ressource, die Ihrer Servergruppe entspricht. 

   Das allgemeine Format des Befehls lautet:

   ```console
   kubectl describe <kind of the custom resource> <server group name> -n <namespace name>
   ```

   Zum Beispiel:

   ```console
   kubectl describe postgresql-12 postgres01
   ```

   Wenn für die Engine-Einstellungen benutzerdefinierte Werte festgelegt sind, werden diese zurückgegeben. Zum Beispiel:

   ```output
   Engine:
   ...
    Settings:
      Default:
        autovacuum_vacuum_threshold:  65
   ```

   Wenn Sie keine benutzerdefinierten Werte für die Engine-Einstellungen festgelegt haben, ist der Abschnitt für die Engine-Einstellungen des `resultset` leer, beispielsweise:

   ```output
   Engine:
   ...
       Settings:
         Default:
   ```

## <a name="set-custom-values-for-engine-settings"></a>Festlegen von benutzerdefinierten Werten für Engine-Einstellungen

Mit den folgenden Befehlen werden die Parameter des Koordinatorknotens und der Workerknoten von PostgreSQL Hyperscale auf dieselben Werte festgelegt. Es ist noch nicht möglich, Parameter in der Servergruppe pro Rolle festzulegen. Das heißt, es ist noch nicht möglich, einen Parameter mit einem bestimmten Wert auf dem Koordinatorknoten und mit einem anderen Wert auf den Workerknoten zu konfigurieren.

### <a name="set-a-single-parameter"></a>Festlegen eines einzelnen Parameters

```console
azdata arc server edit -n <server group name> -e <parameter name>=<parameter value>
```

Zum Beispiel:

```console
azdata arc postgres server edit -n postgres01 -e shared_buffers=8MB
```

### <a name="set-multiple-parameters-with-a-single-command"></a>Festlegen mehrerer Parameter mit einem einzigen Befehl

```console
azdata arc postgres server edit -n <server group name> -e '<parameter name>=<parameter value>, <parameter name>=<parameter value>,...'
```

Zum Beispiel:

```console
azdata arc postgres server edit -n postgres01 -e 'shared_buffers=8MB, max_connections=50'
```

### <a name="reset-a-parameter-to-its-default-value"></a>Zurücksetzen eines Parameters auf den Standardwert

Wenn Sie einen Parameter auf den Standardwert zurücksetzen möchten, legen Sie ihn fest, ohne einen Wert anzugeben. 

Zum Beispiel:

```console
azdata arc postgres server edit -n postgres01 -e shared_buffers=
```

### <a name="reset-all-parameters-to-their-default-values"></a>Zurücksetzen aller Parameter auf die Standardwerte

```console
azdata arc postgres server edit -n <server group name> -e '' -re
```

Zum Beispiel:

```console
azdata arc postgres server edit -n postgres01 -e '' -re
```

## <a name="special-considerations"></a>Besondere Überlegungen

### <a name="set-a-parameter-which-value-contains-a-comma-space-or-special-character"></a>Festlegen eines Parameters, dessen Wert ein Komma, ein Leerzeichen oder ein Sonderzeichen enthält

```console
azdata arc postgres server edit -n <server group name> -e '<parameter name>="<parameter value>"'
```

Zum Beispiel:

```console
azdata arc postgres server edit -n postgres01 -e 'custom_variable_classes = "plpgsql,plperl"'
```

### <a name="pass-an-environment-variable-in-a-parameter-value"></a>Übergeben einer Umgebungsvariablen in einem Parameterwert

Die Umgebungsvariable sollte in Anführungszeichen (" ") eingeschlossen werden, damit sie nicht aufgelöst wird, bevor sie festgelegt wird.

Beispiel: 

```console
azdata arc postgres server edit -n postgres01 -e 'search_path = "$user"'
```

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über das [Aufskalieren](scale-out-postgresql-hyperscale-server-group.md) der Servergruppe (Hinzufügen von Workerknoten).
- Erfahren Sie mehr über das [zentrale Hoch- oder Herunterskalieren](scale-up-down-postgresql-hyperscale-server-group-using-cli.md) der Servergruppe (Vergrößern/Verkleinern von Arbeitsspeicher/virtuellen Kernen).
