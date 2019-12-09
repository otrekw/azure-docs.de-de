---
title: Verwenden einer Azure-Dateifreigabe mit Azure Storage | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Azure-Dateifreigabe mit Windows und Windows Server verwenden.
author: todmccoy
manager: dcscontentpm
ms.service: storage
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: rogarana
ms.subservice: files
services: storage
tags: ''
ms.openlocfilehash: e1e581134b4e3821659c43bb7f55003239594bd9
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74234451"
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
