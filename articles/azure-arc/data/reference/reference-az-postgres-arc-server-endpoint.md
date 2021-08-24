---
title: Referenz zu „az postgres arc-server endpoint“
titleSuffix: Azure Arc-enabled data services
description: Referenzartikel zu „az postgres arc-server endpoint“-Befehlen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 07/30/2021
ms.topic: reference
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.openlocfilehash: 8b3bf65f4e38177712f6deb333ba5d4994542b2f
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122347171"
---
# <a name="az-postgres-arc-server-endpoint"></a>az postgres arc-server endpoint
## <a name="commands"></a>Befehle
| Befehl | BESCHREIBUNG|
| --- | --- |
[az postgres arc-server endpoint list](#az-postgres-arc-server-endpoint-list) | Listet die Endpunkte einer Servergruppe mit PostgreSQL Hyperscale mit Azure Arc-Unterstützung auf
## <a name="az-postgres-arc-server-endpoint-list"></a>az postgres arc-server endpoint list
Listet die Endpunkte einer Servergruppe mit PostgreSQL Hyperscale mit Azure Arc-Unterstützung auf
```bash
az postgres arc-server endpoint list [--name -n] 
                                     [--k8s-namespace -k]  
                                     
[--use-k8s]
```
### <a name="examples"></a>Beispiele
Listet die Endpunkte einer Servergruppe mit PostgreSQL Hyperscale mit Azure Arc-Unterstützung auf
```bash
az postgres arc-server endpoint list --name postgres01  --k8s-namespace namespace --use-k8s
```
### <a name="optional-parameters"></a>Optionale Parameter
#### `--name -n`
Name der Servergruppe mit PostgreSQL Hyperscale mit Azure Arc-Unterstützung.
#### `--k8s-namespace -k`
Der Kubernetes-Namespace, in dem die PostgreSQL Hyperscale-Servergruppe mit Azure Arc-Unterstützung bereitgestellt wird. Wenn kein Namespace angegeben ist, wird der in kubeconfig definierte Namespace verwendet.
#### `--use-k8s`
Verwenden Sie lokale Kubernetes-APIs, um diese Aktion auszuführen.
### <a name="global-arguments"></a>Globale Argumente
#### `--debug`
Ausführlichkeit der Protokollierung erhöhen, um alle Debugprotokolle anzuzeigen.
#### `--help -h`
Zeigen Sie diese Hilfemeldung an, und schließen Sie sie.
#### `--output -o`
Ausgabeformat.  Zulässige Werte: „json“, „jsonc“, „none“, „table“, „tsv“, „yaml“, „yamlc“.  Standardwert: json.
#### `--query -q`
JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter [http://jmespath.org](http://jmespath.org).
#### `--subscription`
Der Name oder die ID des Abonnements. Sie können das standardmäßig verwendete Abonnement mittels `az account set -s NAME_OR_ID` konfigurieren.
#### `--verbose`
Ausführlichkeit der Protokollierung erhöhen. „--debug“ für vollständige Debugprotokolle verwenden.
