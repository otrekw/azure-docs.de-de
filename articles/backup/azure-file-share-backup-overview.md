---
title: Informationen zum Sichern von Azure-Dateifreigaben
description: Erfahren Sie, wie Sie Azure-Dateifreigaben im Recovery Services-Tresor sichern.
ms.topic: conceptual
ms.date: 03/05/2020
ms.openlocfilehash: fd5bb51b2c7b5c09e9d859b69c3094eb50c205b5
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2020
ms.locfileid: "80396280"
---
# <a name="about-azure-file-share-backup"></a>Informationen zum Sichern von Azure-Dateifreigaben

Bei der Sicherung von Azure-Dateifreigaben handelt es sich um eine native, cloudbasierte Sicherungslösung, die Ihre Daten in der Cloud schützt und den zusätzlichen Wartungsaufwand bei lokalen Sicherungslösungen eliminiert. Der Azure Backup-Dienst ist nahtlos in die Azure-Dateisynchronisierung integriert und ermöglicht Ihnen, Ihre Dateifreigabedaten und Sicherungen zu zentralisieren. Mit dieser einfachen, zuverlässigen und sicheren Lösung können Sie den Schutz für die Dateifreigaben Ihres Unternehmens in wenigen einfachen Schritten konfigurieren und dabei sicher sein, dass Sie Ihre Daten im Fall eines Notfallszenarios wiederherstellen können.

## <a name="key-benefits-of-azure-file-share-backup"></a>Wichtige Vorteile der Sicherung von Azure-Dateifreigaben

* Keine Infrastrukturanforderungen: Zum Konfigurieren des Schutzes für Ihre Dateifreigaben sind keine Bereitstellungen erforderlich.
* Integrierte Verwaltungsfunktionen: Sie können Sicherungen planen und den gewünschten Aufbewahrungszeitraum ohne zusätzlichen Aufwand für die Datenbereinigung angeben.
* Sofortige Wiederherstellung: Für die Sicherung von Azure-Dateifreigaben werden Dateifreigabe-Momentaufnahmen verwendet, sodass Sie einfach die Dateien auswählen können, die Sie sofort wiederherstellen möchten.
* Warnungen und Berichte: Sie können Warnungen bei Sicherungs- und Wiederherstellungsfehlern konfigurieren und die von Azure Backup bereitgestellte Berichterstellungslösung verwenden, um Erkenntnisse zu Sicherungen für Ihre Dateifreigaben zu erhalten.

## <a name="architecture"></a>Aufbau

![Architektur für Sicherungen von Azure-Dateifreigaben](./media/azure-file-share-backup-overview/azure-file-shares-backup-architecture.png)

## <a name="how-the-backup-process-works"></a>Funktionsweise des Sicherungsvorgangs

1. Der erste Schritt beim Konfigurieren der Sicherung für Azure-Dateifreigaben ist das Erstellen eines Recovery Services-Tresors. Der Tresor bietet eine konsolidierte Ansicht der Sicherungen, die für unterschiedliche Workloads konfiguriert wurden.

2. Nachdem Sie einen Tresor erstellt haben, ermittelt der Azure Backup-Dienst die Speicherkonten, die beim Tresor registriert werden können. Sie können das Speicherkonto auswählen, in dem die zu schützenden Dateifreigaben gehostet werden.

3. Nachdem Sie das Speicherkonto ausgewählt haben, werden im Azure Backup-Dienst die Dateifreigaben im Speicherkonto aufgelistet. Die zugehörigen Namen werden im Katalog auf Verwaltungsebene gespeichert.

4. Anschließend konfigurieren Sie die Sicherungsrichtlinie (Zeitplan und Aufbewahrung) gemäß Ihren Anforderungen und wählen die zu sichernden Dateifreigaben aus. Der Azure Backup-Dienst registriert die Zeitpläne auf Steuerungsebene, um geplante Sicherungen durchzuführen.

5. Der Azure Backup-Scheduler löst die Sicherungen basierend auf der angegebenen Richtlinie zum geplanten Zeitpunkt aus. Im Rahmen dieses Auftrags wird die Dateifreigabe-Momentaufnahme mit der Dateifreigabe-API erstellt. Nur die Momentaufnahme-URL wird im Metadatenspeicher gespeichert.

    >[!NOTE]
    >Die Dateifreigabedaten werden nicht an den Backup-Dienst übertragen, da dieser die Momentaufnahmen erstellt und verwaltet, die Teil Ihres Speicherkontos sind.

6. Sie können den Inhalt der Azure-Dateifreigabe (einzelne Dateien oder die gesamte Freigabe) aus den in der ursprünglichen Dateifreigabe verfügbaren Momentaufnahmen wiederherstellen. Nachdem der Vorgang ausgelöst wurde, wird die Momentaufnahme-URL aus dem Metadatenspeicher abgerufen, und die Daten werden aufgelistet und aus der Quellmomentaufnahme in die Zieldateifreigabe Ihrer Wahl übertragen.

7. Die Überwachungsdaten zu den Sicherungs- und Wiederherstellungsaufträgen werden an den Azure Backup-Überwachungsdienst gepusht. Auf diese Weise können Sie Cloudsicherungen für Ihre Dateifreigaben auf einem zentralen Dashboard überwachen. Außerdem können Sie Warnungen oder E-Mail-Benachrichtigungen konfigurieren, wenn die Integrität der Sicherung beeinträchtigt ist. E-Mails werden über den Azure-E-Mail-Dienst gesendet.

## <a name="backup-costs"></a>Sicherungskosten

Das Sichern von Azure-Dateifreigaben ist eine auf Momentaufnahmen basierende Lösung, und die für Momentaufnahmen anfallenden Speichergebühren werden zusammen mit der Azure Files-Nutzung gemäß den [hier](https://azure.microsoft.com/pricing/details/storage/files/) aufgeführten Preisen in Rechnung gestellt.

Die Gebühr für die geschützte Instanz für die Nutzung der Sicherungslösung entspricht jedoch dem im Abschnitt zum [Sichern von Azure Files](https://azure.microsoft.com/pricing/details/backup/) beschriebenen Preismodell. Der tatsächliche Preis wurde aktuell nur für die USA (USA, Westen-Mitte) aktualisiert. Für andere Regionen werden die genauen Preise in Kürze aktualisiert, mit geringfügigen regionalen Variationen, aber unter Verwendung des gleichen Preismodells.

>[!NOTE]
>Während der Vorschau gibt es keine „Gebühr für geschützte Instanzen“, und Ihnen werden nur Momentaufnahmen gemäß den [hier](https://azure.microsoft.com/pricing/details/storage/files/) aufgeführten Preisen in Rechnung gestellt.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Sichern von Azure-Dateifreigaben](backup-afs.md).
* Lesen Sie Antworten auf [Fragen zum Sichern von Azure Files](backup-azure-files-faq.md).
