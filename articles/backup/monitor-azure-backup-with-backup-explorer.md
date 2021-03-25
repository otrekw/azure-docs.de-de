---
title: Überwachen von Sicherungen mit dem Backup-Explorer
description: In diesem Artikel wird beschrieben, wie Sie mit dem Backup-Explorer eine Echtzeitüberwachung von Sicherungen für mehrere Tresore, Abonnements, Regionen und Mandanten durchführen.
ms.reviewer: dcurwin
ms.topic: conceptual
ms.date: 02/03/2020
ms.openlocfilehash: a50b9ee05be48113221f2a12f968540bd3a00b3b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88824411"
---
# <a name="monitor-your-backups-with-backup-explorer"></a>Überwachen von Sicherungen mit dem Backup-Explorer

Da Organisationen zunehmend mehr Computer in der Cloud sichern, wird es immer wichtiger, diese Sicherungen effizient zu überwachen. Am besten nutzen Sie einen zentralen Ort, um operative Informationen zu großflächigen Sicherungen anzuzeigen.

Der Backup-Explorer ist eine integrierte Azure Monitor-Arbeitsmappe, die Azure Backup-Kunden als einen solchen zentralen Ort nutzen können. Mit dem Backup-Explorer können Sie operative Aktivitäten in der gesamten Sicherungsumgebung von Azure über Mandanten, Standorte, Abonnements, Ressourcengruppen und Tresore hinweg überwachen. Allgemein stehen mit dem Backup-Explorer die folgenden Funktionen zur Verfügung:

* **Skalierbare Ansicht:** Für die noch nicht für die Sicherung konfigurierten Sicherungselemente, Aufträge, Warnungen, Richtlinien und Ressourcen im gesamten Sicherungsbereich ist eine aggregierte Ansicht verfügbar.
* **Detailinformationen zur Analyse:** Sie können an einem zentralen Ort detaillierte Informationen zu den einzelnen Aufträgen, Warnungen, Richtlinien und Sicherungselementen anzeigen.
* **Interaktive Schnittstellen:** Sie können ein identifiziertes Problem beheben, indem Sie direkt zum entsprechenden Sicherungselement oder der betreffenden Azure-Ressource navigieren.

Diese Funktionen werden standardmäßig durch die native Integration mit Azure Resource Graph und Azure Monitor-Arbeitsmappen bereitgestellt.

> [!NOTE]
>
> * Der Backup-Explorer ist derzeit nur für Daten von virtuellen Azure-Computern verfügbar.
> * Er ist als operatives Dashboard zum Anzeigen von Informationen zu Ihren Sicherungen in den letzten 7 Tagen (maximal) gedacht.
> * Backup-Explorer wird in nationalen Clouds derzeit nicht unterstützt.
> * Derzeit kann die Vorlage für den Backup-Explorer nicht angepasst werden.
> * Benutzerdefinierte Automatisierungen für Azure Resource Graph-Daten sollten nicht programmiert werden.
> * Derzeit ermöglicht der Sicherungs-Explorer nur die Überwachung von Sicherungen für maximal 1000 Abonnements (mandantenübergreifend).

## <a name="get-started"></a>Erste Schritte

Sie greifen auf den Backup-Explorer zu, indem Sie zu einem Ihrer Recovery Services-Tresore navigieren und auf der Seite **Übersicht** den Link **Backup-Explorer** auswählen.

![Quicklink im Tresor](media/backup-azure-monitor-with-backup-explorer/vault-quick-link.png)

Wenn Sie den Link auswählen, wird der Backup-Explorer mit einer aggregierten Ansicht für alle Tresore und Abonnements geöffnet, auf die Sie Zugriff haben. Wenn Sie ein Azure Lighthouse-Konto verwenden, können Sie Daten für alle Mandanten anzeigen, auf die Sie Zugriff haben. Weitere Informationen finden Sie im Abschnitt „Mandantenübergreifende Ansichten“ am Ende dieses Artikels.

![Landing Page des Backup-Explorers](media/backup-azure-monitor-with-backup-explorer/explorer-landing-page.png)

## <a name="backup-explorer-use-cases"></a>Anwendungsfälle für den Backup-Explorer

Im Backup-Explorer werden mehrere Registerkarten angezeigt, die jeweils ausführliche Informationen zu einem bestimmten Sicherungsartefakt (z. B. Sicherungselement, Auftrag oder Richtlinie) enthalten. In diesem Abschnitt finden Sie eine kurze Übersicht über die einzelnen Registerkarten. Die Videos enthalten Beispiele für Anwendungsfälle für jedes Sicherungsartefakt sowie eine Beschreibung der verfügbaren Steuerelemente.

### <a name="the-summary-tab"></a>Registerkarte „Zusammenfassung“

Die Registerkarte **Zusammenfassung** bietet eine Übersicht über den allgemeinen Zustand Ihrer Sicherungsumgebung. Sie können beispielsweise die Anzahl der geschützten Elemente, die Anzahl der Elemente ohne aktivierten Schutz oder die Anzahl der erfolgreichen Aufträge in den letzten 24 Stunden anzeigen.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQYd]

### <a name="the-backup-items-tab"></a>Registerkarte „Sicherungselemente“

Sie können alle Sicherungselemente nach Abonnement, Tresor und anderen Parametern filtern und anzeigen. Wenn Sie den Namen eines Sicherungselements auswählen, können Sie den Azure-Bereich für dieses Sicherungselement öffnen. Beispielsweise können Sie in der Tabelle sehen, dass die letzte Sicherung für das Element *X* fehlerhaft war. Wenn Sie *X* auswählen, wird der Bereich **Sicherung** für dieses Element geöffnet, in dem Sie bedarfsgesteuert einen Sicherungsvorgang initiieren können.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQYc]

### <a name="the-jobs-tab"></a>Registerkarte „Aufträge“

Wählen Sie die Registerkarte **Aufträge** aus, um die Details aller Aufträge anzuzeigen, die in den letzten 7 Tagen ausgelöst wurden. Hier können Sie nach *Auftragsvorgang*, *Auftragsstatus* und *Fehlercode* (bei fehlerhaften Aufträgen) filtern.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nOrh]

### <a name="the-alerts-tab"></a>Registerkarte „Warnungen“

Wählen Sie die Registerkarte **Warnungen** aus, um Details zu allen Warnungen anzuzeigen, die in den letzten 7 Tagen in Ihren Tresoren ausgelöst wurden. Sie können Warnungen nach dem Typ (*Sicherungsfehler* oder *Wiederherstellungsfehler*), dem aktuellen Status (*Aktiv* oder *Gelöst*) und dem Schweregrad (*Kritisch*, *Warnung* oder *Information*) filtern. Sie können auch einen Link auswählen, um zum entsprechenden virtuellen Azure-Computer zu navigieren und die gewünschte Aktion auszuführen.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nTxe]

### <a name="the-policies-tab"></a>Registerkarte „Richtlinien“

Wählen Sie die Registerkarte **Richtlinien** aus, um wichtige Informationen zu allen Sicherungsrichtlinien anzuzeigen, die in Ihrer Sicherungsumgebung erstellt wurden. Sie können die Anzahl der den einzelnen Richtlinien zugeordneten Elemente sowie den Aufbewahrungszeitraum und die Sicherungshäufigkeit für die einzelnen Richtlinien anzeigen.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nLKV]

### <a name="the-backup-not-enabled-tab"></a>Registerkarte „Backup Not Enabled“ (Sicherung nicht aktiviert)

Die Sicherung muss für alle Computer aktiviert werden, die Schutz erfordern. Mit dem Backup-Explorer können Sicherungsadministratoren schnell feststellen, welche Computer in einer Organisation noch nicht durch eine Sicherung geschützt sind. Wählen Sie zum Anzeigen dieser Informationen die Registerkarte **Backup Not Enabled** (Sicherung nicht aktiviert) aus.

Im Bereich **Backup Not Enabled** (Sicherung nicht aktiviert) wird eine Tabelle mit einer Liste nicht geschützter Computer angezeigt. In Ihrer Organisation werden möglicherweise unterschiedliche Tags für Produktions- und Testcomputer oder für Computer zugewiesen, die verschiedene Funktionen erfüllen. Da für jede Klasse von Computern eine separate Sicherungsrichtlinie festgelegt werden muss, können Sie durch Filtern nach Tags die für jede Klasse spezifischen Informationen anzeigen. Wenn Sie den Namen eines Computers auswählen, werden Sie zu dem Bereich **Sicherung konfigurieren** für diesen Computer weitergeleitet, in dem Sie eine geeignete Sicherungsrichtlinie anwenden können.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4nQXZ]

## <a name="export-to-excel"></a>Exportieren in Excel

Sie können die Inhalte einer Tabelle oder eines Diagramms als Excel-Tabelle exportieren. Die Inhalte werden unverändert exportiert, d. h. mit den angewandten vorhandenen Filtern. Wenn Sie zusätzliche Tabellenzeilen exportieren möchten, können Sie über die Dropdownliste **Zeilen pro Seite** am oberen Rand jeder Registerkarte die Anzahl der Zeilen erhöhen, die auf der Seite angezeigt werden sollen.

## <a name="pin-to-the-dashboard"></a>Anheften an das Dashboard

Sie können im oberen Bereich einer Tabelle oder eines Diagramms das Symbol „Anheften“ auswählen, um die Tabelle bzw. das Diagramm an Ihr Azure-Portal-Dashboard anzuheften. Durch Anheften dieser Informationen können Sie angepasste Dashboards erstellen, die auf die Anzeige der wichtigsten benötigten Informationen zugeschnitten sind.

## <a name="cross-tenant-views"></a>Mandantenübergreifende Ansichten

Wenn Sie Azure Lighthouse-Benutzer mit delegiertem Zugriff auf Abonnements in mehreren Mandantenumgebungen sind, können Sie den Standardabonnementfilter verwenden. Durch Auswählen des Symbols „Filter“ oben rechts im Azure-Portal können Sie die Abonnements anzeigen, deren Daten Sie einsehen möchten. Wenn Sie diese Funktion verwenden, werden mit dem Backup-Explorer Informationen zu allen Tresoren in den ausgewählten Abonnements zusammengestellt. Weitere Informationen finden Sie unter [Was ist Azure Lighthouse?](../lighthouse/overview.md)

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie, wie Sie Azure Monitor verwenden, um Erkenntnisse aus Ihren Sicherungsdaten zu ziehen.](./backup-azure-monitoring-use-azuremonitor.md)
