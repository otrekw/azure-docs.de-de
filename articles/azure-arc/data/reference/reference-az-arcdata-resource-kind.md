---
title: Referenz zu „az arcdata resource-kind“
titleSuffix: Azure Arc-enabled data services
description: Referenzartikel zu „az arcdata resource-kind“-Befehlen.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 07/30/2021
ms.topic: reference
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.openlocfilehash: 597bc660140ec2f0ac856a3f6c67db600480aa63
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122340055"
---
# <a name="az-arcdata-resource-kind"></a>az arcdata resource-kind
## <a name="commands"></a>Befehle
| Befehl | BESCHREIBUNG|
| --- | --- |
[az arcdata resource-kind list](#az-arcdata-resource-kind-list) | Listet die verfügbaren benutzerdefinierten Ressourcentypen für Arc auf, die definiert und erstellt werden können.
[az arcdata resource-kind get](#az-arcdata-resource-kind-get) | Ruft die Vorlagendatei für den Arc-Ressourcentyp (resource-kind) ab.
## <a name="az-arcdata-resource-kind-list"></a>az arcdata resource-kind list
Listet die verfügbaren benutzerdefinierten Ressourcentypen für Arc auf, die definiert und erstellt werden können. Nach dem Auflisten können Sie die Vorlagendatei abrufen, die zum Definieren oder Erstellen dieser benutzerdefinierten Ressource erforderlich ist.
```bash
az arcdata resource-kind list 
```
### <a name="examples"></a>Beispiele
Dies ist ein Beispielbefehl zum Auflisten der verfügbaren benutzerdefinierten Ressourcentypen für Arc.
```bash
az arcdata resource-kind list
```
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
## <a name="az-arcdata-resource-kind-get"></a>az arcdata resource-kind get
Ruft die Vorlagendatei für den Arc-Ressourcentyp (resource-kind) ab.
```bash
az arcdata resource-kind get --kind -k 
                             [--dest -d]
```
### <a name="examples"></a>Beispiele
Dies ist ein Beispielbefehl zum Abrufen der CRD-Vorlagendatei für einen Arc-Ressourcentyp.
```bash
az arcdata resource-kind get --kind SqlManagedInstance
```
### <a name="required-parameters"></a>Erforderliche Parameter
#### `--kind -k`
Dies ist die Art der Arc-Ressource, für die Sie die Vorlagendatei erstellen möchten.
### <a name="optional-parameters"></a>Optionale Parameter
#### `--dest -d`
Dies ist das Verzeichnis, in dem Sie die Vorlagendateien platzieren möchten.
`template`
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
