---
title: 'Azure Batch: Taskzeitplan-Fehlerereignis'
description: Referenz zum Taskzeitplan-Fehlerereignis in Azure Batch. Dieses Ereignis wird ausgegeben, wenn ein Task nicht geplant werden konnte und der Vorgang zu einem späteren Zeitpunkt wiederholt wird.
ms.topic: reference
ms.date: 09/20/2020
ms.openlocfilehash: 549281d2b2c371e8f09c584e771cf44f7abc8a00
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91852075"
---
# <a name="task-schedule-fail-event"></a>Taskzeitplan-Fehlerereignis

 Dieses Ereignis wird ausgegeben, wenn ein Task nicht geplant werden konnte und der Vorgang zu einem späteren Zeitpunkt wiederholt wird. Dies ist ein temporärer Fehler zum Zeitpunkt der Taskplanung aufgrund von Ressourceneinschränkungen, z. B. nicht genügend Slots auf Knoten verfügbar, um einen Task auszuführen, für den `requiredSlots` angegeben ist.

 Das folgende Beispiel zeigt den Text eines Taskzeitplan-Fehlerereignisses.

```
{
    "jobId": "job-01",
    "id": "task-01",
    "taskType": "User",
    "systemTaskVersion": 665378862,
    "requiredSlots": 1,
    "nodeInfo": {
        "poolId": "pool-01",
        "nodeId": " "
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 0
    },
    "schedulingError": {
        "category": "UserError",
        "code": "JobPreparationTaskFailed",
        "message": "Task cannot run because the job preparation task failed on node"
    }
}
```

|Elementname|type|Notizen|
|------------------|----------|-----------|
|`jobId`|String|Die ID des Auftrags, der den Task enthält.|
|`id`|Zeichenfolge|Die ID des Tasks.|
|`taskType`|Zeichenfolge|Der Typ des Tasks. Entweder „JobManager“, was bedeutet, dass dies ein Auftrags-Manager-Task ist, oder „User“, was bedeutet, dass dies nicht der Fall ist. Dieses Ereignis wird nicht für Auftragsvorbereitungstasks, Auftragsfreigabetasks oder Starttasks ausgegeben.|
|`systemTaskVersion`|Int32|Dies ist der interne Wiederholungszähler für einen Task. Der Batch-Dienst kann intern einen Task wiederholen, um vorübergehende Probleme zu berücksichtigen. Bei diesen Problemen kann es sich um interne Planungsfehler oder Versuche handeln, Computeknoten mit einem fehlerhaften Status wiederherzustellen.|
|`requiredSlots`|Int32|Die erforderlichen Slots zum Ausführen des Tasks.|
|[`nodeInfo`](#nodeInfo)|Komplexer Typ|Enthält Informationen zu den Computeknoten, auf dem der Task ausgeführt wurde.|
|[`multiInstanceSettings`](#multiInstanceSettings)|Komplexer Typ|Gibt an, dass der Task ein Task mit mehreren Instanzen ist, für den mehrere Computeknoten erforderlich sind.  Ausführliche Informationen finden Sie unter [`multiInstanceSettings`](/rest/api/batchservice/get-information-about-a-task).|
|[`constraints`](#constraints)|Komplexer Typ|Die Ausführungseinschränkungen, die für diesen Task gelten.|
|[`schedulingError`](#schedulingError)|Komplexer Typ|Enthält Informationen zum Planungsfehler für den Task.|

###  <a name="nodeinfo"></a><a name="nodeInfo"></a> nodeInfo

|Elementname|type|Notizen|
|------------------|----------|-----------|
|`poolId`|String|Die ID des Pools, auf den der Task angewendet wurde.|
|`nodeId`|Zeichenfolge|Die ID des Knotens, auf dem der Task ausgeführt wurde.|

###  <a name="multiinstancesettings"></a><a name="multiInstanceSettings"></a> multiInstanceSettings

|Elementname|type|Notizen|
|------------------|----------|-----------|
|`numberOfInstances`|Int32|Die Anzahl der Computeknoten, die vom Task benötigt werden.|

###  <a name="constraints"></a><a name="constraints"></a> Einschränkungen

|Elementname|type|Notizen|
|------------------|----------|-----------|
|`maxTaskRetryCount`|Int32|Gibt an, wie oft der Task maximal wiederholt werden kann. Der Batch-Dienst wiederholt einen Task, wenn sein Exitcode ungleich null ist.<br /><br /> Beachten Sie, dass dieser Wert die Anzahl der Wiederholungen ausdrücklich steuert. Der Batch-Dienst wiederholt den Task einmal und kann ihn anschließend bis zu diesem Grenzwert wiederholen. Wenn beispielsweise die maximale Anzahl von Wiederholungsversuchen 3 ist, versucht der Batch-Dienst einen Task bis zu viermal (ein erster Versuch und drei Wiederholungsversuche).<br /><br /> Wenn die maximale Anzahl von Wiederholungsversuchen 0 ist, wiederholt der Batch-Dienst Tasks nicht.<br /><br /> Wenn die maximale Anzahl von Wiederholungsversuchen -1 ist, wiederholt der Batch-Dienst Tasks unbegrenzt.<br /><br /> Der Standardwert ist 0 (keine Wiederholungsversuche).|


###  <a name="schedulingerror"></a><a name="schedulingError"></a> schedulingError

|Elementname|type|Notizen|
|------------------|----------|-----------|
|`category`|String|Die Kategorie des Fehlers.|
|`code`|Zeichenfolge|Ein Bezeichner für den Taskplanungsfehler. Codes sind unveränderlich und sollten programmgesteuert verwendet werden.|
|`message`|Zeichenfolge|Eine Meldung, die den Taskplanungsfehler beschreibt und für die Anzeige auf einer Benutzeroberfläche geeignet sein sollte.|
