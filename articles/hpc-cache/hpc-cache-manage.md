---
title: Verwalten und Aktualisieren von Azure HPC Cache
description: Verwalten und Aktualisieren von Azure HPC Cache im Azure-Portal
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 1/08/2020
ms.author: rohogue
ms.openlocfilehash: a166a904b2e63419efd5803fd54be1d1b59836fb
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2020
ms.locfileid: "75867082"
---
# <a name="manage-your-cache-from-the-azure-portal"></a>Verwalten des Caches im Azure-Portal

Auf der Übersichtsseite für den Cache im Azure-Portal werden Projektdetails, der Cachestatus und grundlegende Statistiken für den Cache angezeigt. Die Seite umfasst außerdem Steuerelemente zum Löschen des Caches, Leeren der Daten in den Langzeitspeicher oder Aktualisieren der Software.

Um die Übersichtsseite zu öffnen, wählen Sie die Cacheressource im Azure-Portal aus. Laden Sie z. B. die Seite **Alle Ressourcen**, und klicken Sie auf den Namen des Caches.

![Screenshot: Übersichtsseite einer Azure HPC Cache-Instanz](media/hpc-cache-overview.png)

Über die Schaltflächen oben auf der Seite können Sie den Cache verwalten:

* [**Leeren:** ](#flush-cached-data) Geänderte Daten werden in Speicherziele geschrieben.
* [**Aktualisieren:** ](#upgrade-cache-software) Die Cachesoftware wird aktualisiert.
* **Aktualisieren:** Die Übersichtsseite wird erneut geladen.
* [**Löschen:** ](#delete-the-cache) Der Cache wird dauerhaft zerstört.

Nachfolgend finden Sie weitere Informationen zu diesen Optionen.

## <a name="flush-cached-data"></a>Leeren zwischengespeicherter Daten

Über die Schaltfläche **Leeren** auf der Übersichtsseite wird festgelegt, dass alle geänderten Daten im Cache sofort in die Back-End-Speicherziele geschrieben werden. Die Daten im Cache werden routinemäßig in den Speicherzielen gespeichert. Daher ist es nicht notwendig, dies manuell durchzuführen, es sei denn, Sie möchten sicherstellen, dass das Back-End-Speichersystem auf dem neuesten Stand ist. Beispielsweise können Sie **Leeren** verwenden, bevor Sie eine Momentaufnahme des Speichers erstellen oder die Größe des Datasets überprüfen.

> [!NOTE]
> Während der Leerung können im Cache keine Clientanforderungen verarbeitet werden. Der Cachezugriff wird angehalten und erst fortgesetzt, nachdem der Vorgang abgeschlossen wurde.

![Screenshot: Schaltflächen im oberen Bereich, Option „Leeren“ hervorgehoben und Popupnachricht, in der die Leerungsaktion beschrieben und die Frage „Möchten Sie den Vorgang fortsetzen?“ gestellt wird, einschließlich der Schaltflächen „Ja“ (Standard) und „Nein“](media/hpc-cache-flush.png)

Wenn Sie den Leerungsvorgang für den Cache starten, werden im Cache keine Clientanforderungen mehr angenommen und der Cachestatus auf der Übersichtsseite ändert sich in **Wird geleert**.

Die Daten im Cache werden in den entsprechenden Speicherzielen gespeichert. Je nachdem, wie viele Daten geleert werden müssen, kann der Vorgang einige Minuten oder länger als eine Stunde dauern.

Nachdem alle Daten in den Speicherzielen gespeichert wurden, werden im Cache automatisch wieder Clientanforderungen angenommen. Als Cachestatus wird wieder **Fehlerfrei** angezeigt.

## <a name="upgrade-cache-software"></a>Ausführen eines Upgrades für die Cachesoftware

Wenn eine neue Softwareversion verfügbar ist, wird die Schaltfläche **Aktualisieren** aktiviert. Oben auf der Seite sollte auch eine Meldung zur Aktualisierung der Software angezeigt werden.

![Screenshot: obere Schaltflächenleiste mit aktivierter Schaltfläche „Aktualisieren“](media/hpc-cache-upgrade-button.png)

Der Clientzugriff wird während eines Softwareupgrades nicht unterbrochen, die Leistung des Caches wird jedoch verlangsamt. Planen Sie die Aktualisierung der Software für Nebenzeiten oder innerhalb eines geplanten Wartungszeitraums.

Die Aktualisierung der Software kann mehrere Stunden in Anspruch nehmen. Bei Caches, die mit höherem Durchsatz konfiguriert wurden, dauert das Upgrade länger als bei Caches mit geringeren Spitzendurchsatzwerten.

Wenn ein Softwareupgrade verfügbar ist, haben Sie in etwa eine Woche Zeit, dieses manuell anzuwenden. Das Enddatum ist in der Upgrademeldung aufgeführt. Wenn Sie das Upgrade in diesem Zeitraum nicht durchführen, wird es in Azure automatisch auf den Cache angewandt. Die zeitliche Steuerung des automatischen Upgrades kann nicht konfiguriert werden. Wenn Sie Bedenken in Bezug auf die Auswirkungen für die Cacheleistung haben, sollten Sie das Softwareupgrade vor Ablauf des entsprechenden Zeitraums manuell durchführen.

Klicken Sie auf die Schaltfläche **Aktualisieren**, um das Softwareupdate zu starten. Der Cachestatus ändert sich in **Upgrade wird ausgeführt**, bis der Vorgang abgeschlossen ist.

## <a name="delete-the-cache"></a>Löschen des Caches

Über die Schaltfläche **Löschen** wird der Cache zerstört. Wenn Sie einen Cache löschen, werden alle zugehörigen Ressourcen zerstört, und es fallen keine Kontogebühren mehr an.

Die Back-End-Speichervolumes, die als Speicherziele verwendet werden, sind nicht betroffen, wenn Sie den Cache löschen. Sie können sie später einem zukünftigen Cache hinzufügen oder ihre Verwendung einstellen.

> [!NOTE]
> Azure HPC Cache schreibt geänderte Daten nicht automatisch aus dem Cache in die Back-End-Speichersysteme, bevor der Cache gelöscht wird.
>
> Um sicherzustellen, dass alle Daten im Cache in den Langzeitspeicher geschrieben wurden, befolgen Sie dieses Verfahren:
>
> 1. [Entfernen](hpc-cache-edit-storage.md#remove-a-storage-target) Sie jedes Speicherziel aus Azure HPC Cache, indem Sie die Schaltfläche „Löschen“ auf der Seite „Speicherziele“ verwenden. Das System schreibt automatisch alle geänderten Daten aus dem Cache in das Back-End-Speichersystem, bevor das Ziel entfernt wird.
> 1. Warten Sie darauf, dass das Speicherziel vollständig entfernt wird. Der Prozess kann eine Stunde oder länger dauern, wenn viele Daten aus dem Cache geschrieben werden müssen. Wenn dies erfolgt ist, meldet eine Portalbenachrichtigung, dass der Löschvorgang erfolgreich durchgeführt wurde, und das Speicherziel wird aus der Liste entfernt.
> 1. Nachdem alle betroffenen Speicherziele gelöscht wurden, ist es sicher, den Cache zu löschen.
>
> Alternativ können Sie die Option [flush](#flush-cached-data) (Leeren) verwenden, um zwischengespeicherte Daten zu speichern, aber es besteht ein geringes Risiko, Arbeit zu verlieren, wenn ein Client eine Änderung in den Cache schreibt, nachdem das Leeren abgeschlossen ist, aber bevor die Cacheinstanz zerstört wird.

## <a name="cache-metrics-and-monitoring"></a>Metriken und Überwachung des Caches

Auf der Übersichtsseite werden Diagramme für einige grundlegende Cachestatistiken angezeigt: Cachedurchsatz, Vorgänge pro Sekunde und Latenz.

![Screenshot: drei Liniendiagramme mit den oben genannten Statistiken für einen Beispielcache](media/hpc-cache-overview-stats.png)

Diese Diagramme sind Bestandteil der integrierten Überwachungs- und Analysetools von Azure. Weitere Tools und Warnungen sind auf den Seiten unter der Überschrift **Überwachung** auf der Seitenleiste des Portals verfügbar. Weitere Informationen finden Sie im Abschnitt zum Azure-Portal in der [Dokumentation zur Überwachung mit Azure Monitor](../azure-monitor/insights/monitor-azure-resource.md#monitoring-in-the-azure-portal).

## <a name="next-steps"></a>Nächste Schritte

<!-- * Learn more about metrics and statistics for hpc cache -->
* Weitere Informationen zu [Azure-Metriken und Statistiktools](../azure-monitor/index.yml)
* Hilfe zu [Azure HPC Cache](hpc-cache-support-ticket.md)
