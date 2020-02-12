---
title: Überwachen von Sicherungen mit dem Backup-Explorer
description: In diesem Artikel wird beschrieben, wie Sie mit dem Backup-Explorer eine Echtzeitüberwachung von Sicherungen über mehrere Tresore, Abonnements, Regionen und Mandanten durchführen.
ms.reviewer: dcurwin
ms.topic: conceptual
ms.date: 02/03/2020
ms.openlocfilehash: 331d8aeeb828dedb6700a94fafa074c179bef7ab
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2020
ms.locfileid: "76992005"
---
# <a name="monitor-your-backups-with-backup-explorer"></a>Überwachen von Sicherungen mit dem Backup-Explorer

Da Organisationen immer mehr Computer in der Cloud sichern, ist es wichtig, über einen zentralen Ort zu verfügen, an dem operative Informationen zu großflächigen Sicherungen angezeigt werden können, um diese effizient zu überwachen.

Die Arbeitsmappe Backup-Explorer ist eine integrierte Azure Monitor-Arbeitsmappe, die Backup-Kunden eine zentrale Benutzeroberfläche für operative Überwachungsaktivitäten im Zusammenhang mit der Sicherung im gesamten Bereich von Azure (über Mandanten, Standorte, Abonnements, Ressourcengruppen und Tresore hinweg) bereitstellt. Allgemein stehen damit die folgenden Funktionen zur Verfügung:

* **Für die Skalierung** ist eine aggregierte Ansicht der Sicherungselemente, Aufträge, Warnungen, Richtlinien und Ressourcen, die nicht für die Sicherung konfiguriert sind, im gesamten Sicherungsbereich verfügbar. 
* **Detailinformationen zur Analyse** ermöglichen Ihnen die Auswahl ausführlicher Informationen zu den einzelnen Entitäten – Aufträge, Warnungen, Richtlinien und Sicherungselemente – von einem zentralen Ort aus.
* **Interaktive Schnittstellen** ermöglichen Ihnen, nach der Identifikation eines Problems Aktionen auszuführen, indem Sie direkt zu dem Sicherungselement oder der Azure-Ressource navigieren.

Die oben genannten Funktionen werden standardmäßig durch die native Integration mit Azure Resource Graph und Azure Monitor-Arbeitsmappen bereitgestellt.

> [!NOTE]
> * Der Backup-Explorer ist zurzeit nur für Azure-VM-Daten verfügbar.
> * Er ist als operatives Dashboard zum Anzeigen von Informationen zu Ihren Sicherungen in den letzten 7 Tagen (Maximum) gedacht.
> * Derzeit wird das Anpassen der Vorlage für den Backup-Explorer nicht unterstützt. Außerdem wird derzeit nicht empfohlen, benutzerdefinierte Automatisierungen für Azure Resource Graph-Daten zu programmieren.

## <a name="getting-started"></a>Erste Schritte

Sie greifen auf den Backup-Explorer zu, indem Sie zu einem beliebigen Ihrer Recovery Services-Tresore navigieren und auf der Seite **Übersicht** auf den Link **Backup-Explorer** klicken.

![Quicklink im Tresor](media/backup-azure-monitor-with-backup-explorer/vault-quick-link.png)

Wenn Sie auf den Link klicken, wird der Backup-Explorer mit einer aggregierten Ansicht für alle Tresore und Abonnements geöffnet, auf die Sie Zugriff haben. Wenn Sie ein Azure Lighthouse-Konto verwenden, können Sie Daten für alle Mandanten anzeigen, auf die Sie Zugriff haben. (Weitere Informationen finden Sie im Abschnitt weiter unten zu mandantenübergreifenden Ansichten.)

![Landing Page des Explorers](media/backup-azure-monitor-with-backup-explorer/explorer-landing-page.png)

## <a name="backup-explorer-use-cases"></a>Anwendungsfälle für den Backup-Explorer

Der Backup-Explorer besteht aus mehreren Registerkarten, die jeweils ausführliche Informationen zu einem bestimmten Typ von Sicherungsartefakten bereitstellt, z. B. Sicherungselementen, Aufträgen, Richtlinien usw. In diesem Abschnitt finden Sie eine kurze Übersicht über die einzelnen Registerkarten. Die Videos enthalten Beispiele für Anwendungsfälle jeder Registerkarte sowie eine Beschreibung der verschiedenen Steuerelemente, die den Benutzern zur Verfügung stehen.

### <a name="summary"></a>Zusammenfassung

Auf der Registerkarte „Zusammenfassung“ erhalten Sie eine Übersicht über den allgemeinen Zustand Ihrer Sicherungsumgebung. Sie können Informationen wie die Anzahl der geschützten Elemente, die Anzahl der Elemente ohne aktivierten Schutz, die Anzahl der erfolgreichen Aufträge in den letzten 24 Stunden usw. anzeigen. 

Jede der anderen Registerkarten stellt eine eigene Entität dar, beispielsweise: Sicherungselemente, Sicherungsaufträge, Sicherungswarnungen und Sicherungsrichtlinien. Sie können auch Informationen zu Computern anzeigen, für die Backup nicht konfiguriert wurde. Wenn Sie auf eine dieser Registerkarten klicken, werden spezifische Informationen zu dieser Entität angezeigt.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQYd]

### <a name="backup-items"></a>Sicherungselemente

Sie können alle Sicherungselemente gefiltert nach Abonnement, Tresor und anderen Parametern anzeigen. Wenn Sie auf den Namen eines Sicherungselements klicken, können Sie das Azure-Blatt für dieses Sicherungselement öffnen. Beispielsweise können Sie in der Tabelle feststellen, dass die letzte Sicherung für das Element „X“ fehlerhaft war. Wenn Sie auf „X“ klicken, wird das Blatt „Sicherung“ für dieses Element geöffnet, auf dem Sie bedarfsgesteuert einen Sicherungsvorgang initiieren können.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQYc]

### <a name="jobs"></a>Aufträge

Sie können Details aller Aufträge anzeigen, die in den letzten sieben Tagen ausgelöst wurden, indem Sie zur Registerkarte „Aufträge“ navigieren. Hier können Sie nach Auftragsvorgang, Auftragsstatus und Fehlercode (im Fall von fehlerhaften Aufträgen) filtern.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nOrh]

### <a name="alerts"></a>Alerts

Durch Klicken auf die Registerkarte „Warnungen“ können Sie Details zu allen Warnungen anzeigen, die in den letzten sieben Tagen in Ihren Tresoren ausgelöst wurden. Hier können Sie die Datensätze nach dem Typ der Warnung (z. B. Sicherungsfehler, Wiederherstellungsfehler), dem aktuellen Status der Warnung (z. B. „Aktiv“ oder „Gelöst“) und dem Warnungsschweregrad (z. B. „Kritisch“, „Warnung“, „Informationen“) filtern. Sie können auch zu der Azure-VM navigieren, indem Sie auf den Link zum virtuellen Computer klicken, und dann die erforderlichen Aktionen ausführen.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nTxe]

### <a name="policies"></a>Richtlinien

Auf der Registerkarte „Richtlinien“ können Sie wichtige Informationen zu allen Sicherungsrichtlinien anzeigen, die in Ihrer Sicherungsumgebung erstellt wurden. Sie können sehen, wie viele Elemente den einzelnen Richtlinien zugeordnet sind, sowie den Aufbewahrungszeitraum und die Sicherungshäufigkeit für die einzelnen Richtlinien anzeigen.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nLKV]

### <a name="backup-not-enabled"></a>Sicherung nicht aktiviert

Es ist wichtig, dass der Sicherungsadministrator einer Organisation schnell feststellen kann, für welche Computer in der Organisation noch keine Sicherung aktiviert wurde, damit die Sicherung für alle Computer aktiviert werden kann, die Schutz benötigen. Die Registerkarte **Backup Not Enabled** (Sicherung nicht aktiviert) stellt eine große Hilfe bei dieser Aufgabe dar.

Auf dieser Registerkarte wird eine Tabelle mit einer Liste der Elemente angezeigt, die nicht geschützt sind. Wenn in Ihrer Organisation für Produktions- und Testcomputer oder Computer, die verschiedene Funktionen mit der Anforderung einer jeweils separaten Sicherungsrichtlinie erfüllen, unterschiedliche Tags zugewiesen werden, können Sie durch das Filtern nach Tags spezifische Informationen zu einer bestimmten Klasse von Computern anzeigen. Wenn Sie auf den Namen eines Elements klicken, werden Sie zum Blatt **Sicherung konfigurieren** für das betreffende Element umgeleitet. Dort können Sie dann dieses Element mit einer entsprechenden Sicherungsrichtlinie sichern.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQXZ]

## <a name="exporting-to-excel"></a>Exportieren nach Excel

Wenn Sie in einem beliebigen Widget (Tabelle/Diagramm) rechts oben auf die Schaltfläche mit dem Pfeil nach unten klicken, wird der Inhalt des Widgets als Excel-Tabelle exportiert, und zwar genau wie angezeigt, also mit angewandten Filtern. Wenn Sie zusätzliche Zeilen einer Tabelle nach Excel exportieren möchten, können Sie über die Dropdownliste **Zeilen pro Seite** am oberen Rand jeder Registerkarte die Anzahl der Zeilen erhöhen, die auf der Seite angezeigt werden.

## <a name="pinning-to-dashboard"></a>Anheften an das Dashboard

Sie können am oberen Rand jedes Widgets auf das Symbol „Anheften“ klicken, um dieses Widget an Ihr Azure-Portal-Dashboard anzuheften. Auf diese Weise können Sie angepasste Dashboards erstellen, die auf die Anzeige der wichtigsten benötigten Informationen zugeschnitten sind.

## <a name="cross-tenant-views"></a>Mandantenübergreifende Ansichten

Wenn Sie ein Azure Lighthouse-Benutzer mit delegiertem Zugriff auf Abonnements in mehreren Mandantenumgebungen sind, können Sie den Standardabonnementfilter verwenden (durch Klicken auf das Filtersymbol rechts oben im Azure-Portal), um alle Abonnements auszuwählen, für die Sie die Daten anzeigen möchten. Dazu stellt der Backup-Explorer Informationen zu allen Tresoren in diesen Abonnements zusammen. Erfahren Sie mehr über [Azure Lighthouse](https://docs.microsoft.com/azure/lighthouse/overview).

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie, wie Sie Azure Monitor verwenden, um Erkenntnisse aus Ihren Sicherungsdaten zu ziehen.](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)