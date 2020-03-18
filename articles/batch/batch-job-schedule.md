---
title: Planen Ihrer Aufträge
description: Verwenden Sie die Auftragsplanung, um Ihre Tasks zu verwalten.
services: batch
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.topic: article
ms.workload: big-compute
ms.date: 02/20/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 55ea8fb4cc0e65deaa89d718c4a46513716dcf54
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78672176"
---
# <a name="schedule-jobs-for-efficiency"></a>Planen von Aufträgen für Effizienz

Die Planung von Batch-Aufträgen ermöglicht es Ihnen, die Aufträge, die Sie zuerst ausführen möchten, zu priorisieren und gleichzeitig Tasks zu berücksichtigen, die von anderen Tasks abhängig sind. Durch die Planung Ihrer Aufträge können Sie sicherstellen, dass Sie möglichst wenig Ressourcen verbrauchen. Knoten können stillgelegt werden, wenn sie nicht benötigt werden, Tasks, die von anderen Tasks abhängig sind, werden Just-In-Time erledig, was zur Optimierung der Workflows führt. Es wird jeweils nur ein Auftrag ausgeführt. Ein neuer Auftrag wird erst gestartet, wenn der vorherige abgeschlossen ist. Sie können festlegen, dass Ihr Auftrag automatisch fertig gestellt wird. 

## <a name="benefit-of-job-scheduling"></a>Vorteil der Auftragsplanung

Der Vorteil der Auftragsplanung besteht darin, dass Sie einen Zeitplan für die Erstellung des Auftrags festlegen können. Tasks, die Sie mit dem Task im Auftrags-Manager planen, werden einem Auftrag zugeordnet. Der Task im Auftrags-Manager erstellt die Tasks für den Auftrag. Dazu muss sich der Task im Auftrags-Manager beim Batch-Konto authentifizieren. Verwenden Sie das Zugriffstoken AZ_BATCH_AUTHENTICATION_TOKEN. Das Token ermöglicht den Zugriff auf den Rest des Auftrags. 

## <a name="use-the-portal-to-schedule-a-job"></a>Verwenden des Portals zum Planen eines Auftrags

   1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

   2. Wählen Sie das Batch-Konto aus, in dem Sie Aufträge planen möchten.

   3. Wählen Sie **Hinzufügen** aus, um einen neuen Auftragsplan zu erstellen und das **Basisformular** auszufüllen.



![Planen eines Auftrags][1]

**Auftragszeitplan-ID**: Der eindeutige Bezeichner für diesen Auftragszeitplan.

**Anzeigename**: Der Anzeigename für den Auftrag muss nicht eindeutig sein, ist aber auf maximal 1024 Zeichen begrenzt.

**Nicht ausführen bis**: Gibt den frühesten Zeitpunkt für die Auftragsausführung an. Wenn Sie diese Option nicht festlegen, ist der Zeitplan sofort zur Ausführung von Aufträgen verfügbar.

**Nicht ausführen nach**: Kein Auftrag wird nach der hier festgelegten Zeit ausgeführt. Wenn Sie keinen Zeitpunkt festlegen, erstellen Sie einen Zeitplan für wiederkehrende Aufträge, der aktiv bleibt, bis Sie ihn explizit beenden.

**Wiederholungsintervall**: Sie können die Zeitspanne zwischen Aufträgen angeben. Zu jedem Zeitpunkt kann nur ein Auftrag geplant sein. Wenn also der Zeitpunkt gekommen ist, im Rahmen eines Auftragsplans einen neuen Auftrag zu erstellen, der vorherige Auftrag aber noch ausgeführt wird, erstellt der Batch-Dienst den neuen Auftrag erst, wenn der vorherige Auftrag beendet ist.  

**Startfenster**: Hier geben Sie das Zeitintervall an, von der Erstellung des Auftrags bis zu dem Zeitpunkt, an dem der Auftrag abgeschlossen sein soll. Wird der aktuelle Auftrag nicht innerhalb des Zeitfensters abgeschlossen, wird der nächste Auftrag nicht gestartet.

Unten im Basisformular geben Sie den Pool an, in dem der Auftrag ausgeführt werden soll. Wählen Sie **Aktualisieren** aus, um die Pool-ID-Informationen zu ermitteln. 

![Festlegen des Pools][2]


**Pool-ID**: Identifizieren Sie den Pool, in dem der Auftrag ausgeführt werden soll.

**Task „Auftragskonfiguration“** : Wählen Sie **Aktualisieren** aus, um die Task im Auftrags-Manager sowie die Auftragsvorbereitungs- und Freigabe-Tasks zu benennen, falls Sie diese verwenden.

**Priorität:** Weisen Sie dem Auftrag eine Priorität zu.

**Max. Gesamtbetrachtungszeit**: Legen Sie die maximale Zeitspanne für die Ausführung des Auftrags fest. Wenn er nicht innerhalb des Zeitraums abgeschlossen wird, beendet Batch den Auftrag. Wenn Sie diese Option nicht festgelegen, gibt es keine zeitliche Beschränkung für den Auftrag.

**Max. Wiederholungszahl für Tasks**: Geben Sie an, wie oft ein Task wiederholt werden kann (maximal viermal). Dies ist nicht das gleiche wie die mögliche Anzahl der Wiederholungen für einen API-Befehl.

**Bei Abschluss aller Tasks**: Der Standardwert ist „No Action“.

**Bei Fehler eines Tasks**: Der Standardwert ist „No Action“. Ein Task schlägt fehl, wenn die Anzahl der Wiederholungsversuche erschöpft oder beim Starten des Tasks ein Fehler aufgetreten ist. 

Nachdem Sie **Speichern** ausgewählt haben und in der linken Navigation zu **Auftragszeitpläne** navigieren, können Sie die Ausführung des Auftrags nachverfolgen, indem Sie **Ausführungsinformationen** auswählen.


## <a name="for-more-information"></a>Weitere Informationen finden Sie unter

Informationen zum Verwalten eines Auftrags mit der Azure-Befehlszeilenschnittstelle finden Sie unter [az batch job-schedule](https://docs.microsoft.com/cli/azure/batch/job-schedule?view=azure-cli-latest).

## <a name="next-steps"></a>Nächste Schritte

[Erstellen von Taskabhängigkeiten zum Ausführen von Tasks, die von anderen Tasks abhängen](batch-task-dependencies.md).





[1]: ./media/batch-job-schedule/add_job_schedule-02.png
[2]: ./media/batch-job-schedule/add_job_schedule-03.png


