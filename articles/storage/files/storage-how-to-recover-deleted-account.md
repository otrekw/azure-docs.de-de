---
title: Wiederherstellen eines gelöschten Speicherkontos
description: Erfahren Sie, wie Sie ein gelöschtes Speicherkonto wiederherstellen.
author: todmccoy
manager: dcscontentpm
ms.service: storage
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: rogarana
ms.subservice: files
services: storage
tags: ''
ms.openlocfilehash: 05465d4a03335ac607ba8981116c66fd6dac9416
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2020
ms.locfileid: "78252628"
---
# <a name="how-to-recover-a-deleted-storage-account"></a>Wiederherstellen eines gelöschten Speicherkontos

Azure Storage bietet Datenresilienz mithilfe automatischer Replikate, das verhindert jedoch keine Beschädigung von Daten durch Benutzer oder Anwendungscode, unabhängig davon, ob es versehentlich oder absichtlich geschieht. Es sind erweiterte Methoden erforderlich, beispielsweise das Kopieren der Daten an einen sekundären Speicherort mit einem Überwachungsprotokoll, um Datengenauigkeit bei Anwendungsinstanzen oder Benutzerfehlern zu gewährleisten,

In der folgenden Tabelle finden Sie eine Übersicht über den Umfang der Speicherkontowiederherstellung, abhängig von der Replikationsstrategie.

| |LRS|ZRS|GRS|RA - GRS|
|---|---|---|---|---|
|Azure Resource Manager-Speicherkonto|Ja|Ja|Ja|Ja|
|Speicherkonto (klassisch)|Ja|Ja|Ja|Ja|

Sammeln Sie die folgenden Informationen und reichen Sie eine Supportanfrage an den Microsoft-Support ein:

* Speicherkontoname
* Datum der Löschung
* Speicherkontoregion
* Art der Löschung des Speicherkontos
* Methode zur Löschung des Speicherkontos (Portal, PowerShell usw.)

Wichtige Punkte

* Nach dem Löschvorgang dauert es in der Regel bis zu 15 Tage, bis der Speicherdienst die automatische Speicherbereinigung durchgeführt hat. Daher wird die Speicherkontowiederherstellung möglicherweise nicht mit einer Vereinbarung zum Servicelevel wiederhergestellt.
* Der Microsoft-Support versucht bei der Wiederherstellung des Containers/Kontos sein Bestes, aber die Wiederherstellung kann nicht garantiert werden.

> [!NOTE]
> Die Wiederherstellung ist unter Umständen nicht erfolgreich, wenn das Konto nochmals erstellt wurde. Wenn Sie Ihr Konto bereits neu erstellt haben, müssen Sie es vor der Wiederherstellung löschen.
