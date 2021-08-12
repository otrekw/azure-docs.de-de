---
title: Die Befehle für die Hive LLAP-Workload-Verwaltung
titleSuffix: Azure HDInsight
description: Die Befehle für die Hive LLAP-Workload-Verwaltung
ms.service: hdinsight
ms.topic: reference
author: guptanikhil007
ms.author: guptan
ms.reviewer: jasonh
ms.date: 05/25/2021
ms.openlocfilehash: dc05ae5ec7cad35d5cda549e654caf3d44d91a03
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110482533"
---
# <a name="hive-llap-workload-management-commands"></a>Die Befehle für die Hive LLAP-Workload-Verwaltung

Die Workload-Verwaltungsfunktion kann mit den folgenden Hive-Befehlen gesteuert und verwaltet werden. Diese Befehle ähneln den vorhandenen ÄNDERN-, ERSTELLEN-, VERWERFEN- und ANZEIGEN-Anweisungen.

## <a name="alter-mapping"></a>Ändern der Zuordnung 
Dieser Befehl ändert das Routing von Abfragen an einen Ressourcenpool. 
#### <a name="syntax"></a>Syntax 
```hql
ALTER { USER | GROUP | APPLICATION } MAPPING 'entity_name' IN plan_name { TO pool_path | UNMANAGED } [ WITH ORDER num ]
```
#### <a name="example"></a>Beispiel 
```hql
ALTER USER MAPPING 'hive' IN demo_plan TO etl WITH ORDER 1;
```

## <a name="alter-pool"></a>Ändern des Pools 
Dieser Befehl ändert Abfrage-Pooleigenschaften, fügt Trigger hinzu und entfernt Trigger. 
#### <a name="syntax"></a>Syntax 
```hql
ALTER POOL plan_name.pool_path [ SET {property=value, ... } | UNSET { property, ... } ];
ALTER POOL plan_name.pool_path [ ADD | DROP ] TRIGGER name;
```
#### <a name="example"></a>Beispiel 
```hql
ALTER POOL demo_plan.default ADD TRIGGER defaultToETL;
```

## <a name="alter-resource-plan"></a>Ändern des Ressourcenplans 
Dieser Befehl aktiviert, deaktiviert, aktiviert, überprüft oder ändert einen Plan. 
#### <a name="syntax"></a>Syntax 
```hql
ALTER RESOURCE PLAN name [ VALIDATE | DISABLE | ENABLE | ACTIVATE | RENAME TO another_name | SET {property=value, ... } | UNSET {property, ... } ];
```
#### <a name="example"></a>Beispiel 
```hql
ALTER RESOURCE PLAN demo_plan SET DEFAULT POOL=etl, QUERY_PARALLELISM=3;
```

## <a name="alter-trigger"></a>Ändern des Triggers 
Dieser Befehl fügt einem Ressourcenpool einen Trigger hinzu oder entfernt ihn aus diesem. 
#### <a name="syntax"></a>Syntax 
```hql
ALTER TRIGGER plan_name.name { ADD TO | DROP FROM } { POOL path | UNMANAGED };
```
#### <a name="example"></a>Beispiel 
```hql
ALTER TRIGGER demo_plan.ETLKill ADD TO POOL etl;
```

## <a name="create-mapping"></a>Erstellen von Zuordnungen 
Dieser Befehl leitet Abfragen an einen Ressourcenpool weiter. 
#### <a name="syntax"></a>Syntax 
```hql
CREATE { USER | GROUP | APPLICATION } MAPPING 'entity_name' IN plan_name { TO pool_path | UNMANAGED } [ WITH ORDER num ];
```
#### <a name="example"></a>Beispiel 
```hql
CREATE USER MAPPING 'hive' IN demo_plan TO sys_accounts WITH ORDER 1;
```

## <a name="create-pool"></a>Erstellen eines Pools 
Dieser Befehl erstellt einen Abfragepool und fügt ihn einem Ressourcenplan hinzu. 
#### <a name="syntax"></a>Syntax 
```hql
CREATE POOL plan_name.path WITH ALLOC_FRACTION = decimal, QUERY_PARALLELISM = num, [ SCHEDULING_POLICY = scheduling_value ];
```
#### <a name="example"></a>Beispiel 
```hql
CREATE POOL demo_plan.etl WITH ALLOC_FRACTION = 0.20, QUERY_PARALLELISM = 2;
```

## <a name="create-resource-plan"></a>Erstellen eines Ressourcenplans 
Dieser Befehl erstellt einen Ressourcenplan 
#### <a name="syntax"></a>Syntax 
```hql
CREATE RESOURCE PLAN plan_name [ WITH QUERY PARALLELISM=number | LIKE name];
```
#### <a name="example"></a>Beispiel 
```hql
CREATE RESOURCE PLAN demo_plan;
```

## <a name="create-trigger"></a>Create Trigger (Trigger erstellen) 
Dieser Befehl erstellt einen Trigger und fügt ihn einem Ressourcenplan hinzu. 
#### <a name="syntax"></a>Syntax 
```hql
CREATE TRIGGER plan_name.name WHEN condition DO action;
```
#### <a name="example"></a>Beispiel 
```hql
CREATE TRIGGER demo_plan.defaultToETL WHEN  ELAPSED_TIME > 20000 DO MOVE TO etl;
```

## <a name="disable-workload-management"></a>Deaktivieren einer Workload-Verwaltung 
Dieser Befehl deaktiviert den aktiven Ressourcenplan. 
#### <a name="syntax"></a>Syntax 
```hql
DISABLE WORKLOAD MANAGEMENT;
```
#### <a name="example"></a>Beispiel 
```hql
DISABLE WORKLOAD MANAGEMENT
```

## <a name="drop-mapping"></a>Verwerfen einer Zuordnung 
Dieser Befehl entfernt eine Zuordnung aus einem Ressourcenplan. 
#### <a name="syntax"></a>Syntax 
```hql
DROP { USER | GROUP | APPLICATION } MAPPING 'entity_name' IN plan_name;
```
#### <a name="example"></a>Beispiel 
```hql
DROP USER MAPPING 'hive' IN demo_plan;
```

## <a name="drop-pool"></a>Verwerfen eines Pools 
Dieser Befehl entfernt einen Abfragepool aus einem Ressourcenplan. 
#### <a name="syntax"></a>Syntax 
```hql
DROP POOL plan_name.pool_path;
```
#### <a name="example"></a>Beispiel 
```hql
CREATE POOL demo_plan.etl;
```

## <a name="drop-resource-plan"></a>Verwerfen eines Ressourcenplans 
Dieser Befehl löscht einen Ressourcenplan. 
#### <a name="syntax"></a>Syntax 
```hql
DROP RESOURCE PLAN plan_name;
```
#### <a name="example"></a>Beispiel 
```hql
DROP RESOURCE PLAN demo_plan;
```

## <a name="drop-trigger"></a>Verwerfen eines Triggers 
Dieser Befehl löscht einen Trigger aus einem Ressourcenplan. 
#### <a name="syntax"></a>Syntax 
```hql
DROP TRIGGER plan_name.trigger_name;
```
#### <a name="example"></a>Beispiel 
```hql
DROP TRIGGER demo_plan.defaultToETL;
```

## <a name="replace-resource-plan-with"></a>Ersetzen des Ressourcenplans durch 
Dieser Befehl ersetzt den Inhalt eines Ressourcenplans durch den Inhalt eines anderen. 
#### <a name="syntax"></a>Syntax 
```hql
REPLACE RESOURCE PLAN name1 WITH name2; 
REPLACE ACTIVE RESOURCE PLAN name1 WITH name2;
```
#### <a name="example"></a>Beispiel 
```hql
REPLACE RESOURCE PLAN rp_plan1 WITH rp_plan2;
```

## <a name="show-resource-plan"></a>Anzeigen eines Ressourcenplans 
Dieser Befehl listet die Planinhalte auf. 
#### <a name="syntax"></a>Syntax 
```hql
SHOW RESOURCE PLAN plan_name;
```
#### <a name="example"></a>Beispiel 
```hql
SHOW RESOURCE PLAN demo_plan;
```

## <a name="show-resource-plans"></a>Anzeigen eines Ressourcenplans 
Dieser Befehl listet alle Ressourcenpläne auf. 
#### <a name="syntax"></a>Syntax 
```hql
SHOW RESOURCE PLANS;
```
#### <a name="example"></a>Beispiel 
```hql
SHOW RESOURCE PLANS;
```
