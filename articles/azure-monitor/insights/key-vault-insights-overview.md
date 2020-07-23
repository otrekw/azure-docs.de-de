---
title: Überwachen von Key Vault mit Azure Monitor für Key Vault (Vorschauversion) | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zu Azure Monitor für Key Vault-Instanzen.
services: azure-monitor
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 04/13/2019
ms.openlocfilehash: 7b52a1ee67c22fb3bded49a80d35305bdf612f10
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86498950"
---
# <a name="monitoring-your-key-vault-service-with-azure-monitor-for-key-vault-preview"></a>Überwachen Ihres Schlüsseltresordiensts mit Azure Monitor für Key Vault (Vorschauversion)
Azure Monitor für Key Vault (Vorschauversion) ermöglicht eine umfassende Überwachung Ihrer Schlüsseltresore. Zu diesem Zweck wird eine einheitliche Darstellung der Anforderungen, Leistung, Fehler und Wartezeit im Zusammenhang mit Key Vault bereitgestellt.
In diesem Artikel erfahren Sie, wie Sie das Onboarding von Azure Monitor für Key Vault (Vorschauversion) durchführen und die Umgebung anpassen.

## <a name="introduction-to-azure-monitor-for-key-vault-preview"></a>Einführung in Azure Monitor für Key Vault (Vorschauversion)

Bevor Sie sich mit der Umgebung befassen, sollten Sie sich mit der Darstellung und Visualisierung von Informationen vertraut machen.
-    **Perspektive im gewünschten Umfang** mit einer Momentaufnahme der Leistung auf der Grundlage der Anforderungen, einer Aufschlüsselung von Fehlern sowie einer Übersicht über die Vorgänge und die Wartezeit.
-   **Drilldownanalyse** für einen bestimmten Schlüsseltresor, um eine ausführliche Analyse zu ermöglichen.
-    **Anpassbarkeit**. Dadurch können Sie ändern, welche Metriken angezeigt werden, auf Ihre Grenzwerte abgestimmte Schwellenwerte festlegen und Ihre eigene Arbeitsmappe speichern. Diagramme in der Arbeitsmappe können an Azure-Dashboards angeheftet werden.

Von Azure Monitor für Key Vault werden Protokolle mit Metriken kombiniert, um eine globale Überwachungslösung bereitzustellen. Die metrikbasierten Überwachungsdaten stehen allen Benutzern zur Verfügung. Für die Einbeziehung protokollbasierter Visualisierungen müssen die Benutzer jedoch unter Umständen die [Protokollierung ihrer Azure Key Vault-Instanz aktivieren](../../key-vault/general/logging.md).

## <a name="configuring-your-key-vaults-for-monitoring"></a>Konfigurieren Ihrer Schlüsseltresore für die Überwachung

> [!NOTE]
> Die Aktivierung von Protokollen ist ein kostenpflichtiger Dienst für zusätzliche Überwachungsfunktionen.

1. Auf der Registerkarte „Vorgänge und Wartezeit“ können Sie ermitteln, wie viele und welche Schlüsseltresore aktiviert sind. Wählen Sie die Schaltfläche **Aktivieren** aus, um mit der Erfassung zu beginnen. Dadurch gelangen Sie zu einer separaten Arbeitsmappe, in der die Schlüsseltresore aufgeführt sind, für die Diagnoseprotokolle aktiviert werden müssen.

    ![Screenshot: Registerkarte „Vorgänge und Wartezeit“ mit blauer Aktivierungsschaltfläche](./media/key-vaults-insights-overview/enable-logging.png)

2. Klicken Sie zum Aktivieren von Diagnoseprotokollen unterhalb der Aktionenspalte auf den Link **Aktivieren**, und erstellen Sie eine neue Diagnoseeinstellung, durch die Protokolle an einen Log Analytics-Arbeitsbereich gesendet werden. Es empfiehlt sich, alle Protokolle an den gleichen Arbeitsbereich zu senden.

3. Nachdem Sie die Diagnoseeinstellungen gespeichert haben, können Sie alle protokollbasierten Diagramme und Visualisierungen unterhalb der Key Vault-Erkenntnisse anzeigen. Beachten Sie, dass es einige Minuten oder Stunden dauern kann, bis die Protokolle aufgefüllt werden.

4. Weitere Informationen zur Aktivierung von Diagnoseprotokollen für Ihren Key Vault-Dienst finden Sie im [vollständigen Leitfaden](../../key-vault/general/logging.md).

## <a name="view-from-azure-monitor"></a>Anzeigen in Azure Monitor

In Azure Monitor können Sie Details zu Anforderungen, zur Wartezeit und zu Fehlern mehrerer Schlüsseltresore in Ihrem Abonnement anzeigen sowie Leistungsprobleme und Drosselungsszenarien leichter identifizieren.

Gehen Sie wie folgt vor, um die Nutzung und Vorgänge Ihrer Speicherkonten für alle Ihre Abonnements anzuzeigen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Wählen Sie im linken Bereich des Azure-Portals **Monitor** und im Abschnitt „Erkenntnisse“ die Option **Schlüsseltresore (Vorschau)** aus.

![Screenshot: Übersicht mit mehreren Diagrammen](./media/key-vaults-insights-overview/overview.png)

## <a name="overview-workbook"></a>Arbeitsmappe „Übersicht“

In der Arbeitsmappe „Übersicht“ für das ausgewählte Abonnement enthält die Tabelle interaktive Schlüsseltresormetriken für innerhalb des Abonnements gruppierte Schlüsseltresore. Ergebnisse können auf der Grundlage der Optionen gefiltert werden, die Sie in den folgenden Dropdownlisten auswählen:

* Abonnements: Nur Abonnements mit Schlüsseltresoren werden aufgelistet.

* Schlüsseltresore: Vorab sind standardmäßig nur bis zu fünf Schlüsseltresore ausgewählt. Wenn Sie in der Bereichsauswahl alle oder mehrere Schlüsseltresore auswählen, werden bis zu 200 Schlüsseltresore zurückgegeben. Wenn Sie also beispielsweise über insgesamt 573 Schlüsseltresore in drei ausgewählten Abonnements verfügen, werden lediglich 200 Tresore angezeigt.

* Zeitbereich: Standardmäßig werden die Informationen der letzten 24 Stunden basierend auf der entsprechenden Auswahl angezeigt.

Der Zähler unter der Dropdownliste gibt Aufschluss über die Gesamtanzahl von Schlüsseltresoren im Abonnement sowie über die Anzahl ausgewählter Schlüsseltresore. Für die Spalten der Arbeitsmappe, in denen Anforderungs-, Fehler- und Wartezeitmetriken gemeldet werden, stehen bedingte farbcodierte Wärmebilder zur Verfügung. Die dunkelste Farbe hat den höchsten Wert, und eine hellere Farbe basiert auf den niedrigsten Werten.

## <a name="failures-workbook"></a>Arbeitsmappe „Fehler“

Wählen Sie oben auf der Seite die Option **Fehler** aus, um die entsprechende Registerkarte zu öffnen. Dort wird neben API-Treffern und der Häufigkeit im Zeitverlauf auch die Anzahl bestimmter Antwortcodes angezeigt.

![Screenshot: Arbeitsmappe „Fehler“](./media/key-vaults-insights-overview/failures.png)

Für Spalten in der Arbeitsmappe, in denen API-Treffermetriken mit einem blauen Wert dargestellt werden, gibt es eine bedingte Farbcodierung oder Wärmebilder. Die dunkelste Farbe hat den höchsten Wert, und eine hellere Farbe basiert auf den niedrigsten Werten.

In der Arbeitsmappe werden erfolgreiche Vorgänge (2xx-Statuscodes), Authentifizierungsfehler (401/403-Statuscodes), Drosselungen (429-Statuscodes) und andere Fehler (4xx-Statuscodes) angezeigt.

Informationen zum besseren Verständnis der einzelnen Statuscodes finden Sie in der [Dokumentation zu Status- und Antwortcodes von Azure Key Vault](../../key-vault/general/authentication-requests-and-responses.md).

## <a name="operations--latency-workbook"></a>Arbeitsmappe „Vorgänge und Wartezeit“

Wählen Sie oben auf der Seite die Option **Vorgänge und Wartezeit** aus, um die Registerkarte **Vorgänge und Wartezeit** zu öffnen. Auf dieser Registerkarte können Sie das Onboarding Ihrer Schlüsseltresore für die Überwachung durchführen. Ausführlichere Schritte finden Sie im Abschnitt [Konfigurieren Ihrer Schlüsseltresore für die Überwachung](#configuring-your-key-vaults-for-monitoring).

Sie sehen, wie viele Schlüsseltresore für die Protokollierung aktiviert sind. Wenn mindestens ein Tresor ordnungsgemäß konfiguriert wurde, werden Tabellen mit den Vorgängen und Statuscodes für die einzelnen Schlüsseltresore angezeigt. Durch Klicken auf den Detailbereich einer Zeile erhalten Sie zusätzliche Informationen zum jeweiligen Vorgang.

![Screenshot: Diagramme für Vorgänge und Wartezeit](./media/key-vaults-insights-overview/logs.png)

Sollten in diesem Abschnitt keine Daten angezeigt werden, informieren Sie sich im Abschnitt weiter oben, wie Sie Protokolle für Azure Key Vault aktivieren, oder lesen Sie weiter unten den Abschnitt zur Problembehandlung.

## <a name="view-from-a-key-vault-resource"></a>Anzeigen über eine Key Vault-Ressource

Sie können direkt von einem Schlüsseltresor aus auf Azure Monitor für Key Vault zugreifen:

1. Wählen Sie im Azure-Portal die Option „Schlüsseltresore“ aus.

2. Wählen Sie in der Liste einen Schlüsseltresor aus. Wählen Sie im Abschnitt „Überwachung“ die Option „Insights (Vorschau)“ aus.

Alternativ können Sie auch in der Arbeitsmappe auf der Azure Monitor-Ebene den Ressourcennamen eines Schlüsseltresors auswählen, um auf diese Sichten zuzugreifen.

![Screenshot: Anzeigen über eine Key Vault-Ressource](./media/key-vaults-insights-overview/key-vault-resource-view.png)

In der Arbeitsmappe **Übersicht** für den Schlüsseltresor werden mehrere Leistungsmetriken angezeigt, die eine schnelle Bewertung folgender Aspekte ermöglichen:

- Interaktive Leistungsdiagramme mit den wichtigsten Details zu Schlüsseltresortransaktionen, Wartezeit und Verfügbarkeit.

- Metriken und Statuskacheln zur Hervorhebung der Dienstverfügbarkeit, der Gesamtanzahl von Transaktionen für die Schlüsseltresorressource und der Gesamtwartezeit.

Durch Auswählen einer der anderen Registerkarten für **Fehler** oder **Vorgänge** gelangen Sie zu den entsprechenden Arbeitsmappen.

![Screenshot: Fehleransicht](./media/key-vaults-insights-overview/resource-failures.png)

In der Arbeitsmappe „Fehler“ werden die Ergebnisse aller Schlüsseltresoranforderungen im gewählten Zeitrahmen aufgeschlüsselt und nach erfolgreichen Vorgängen (2xx), Authentifizierungsfehlern (401/403), Drosselung (429) und anderen Fehlern kategorisiert.

![Screenshot: Vorgangsansicht](./media/key-vaults-insights-overview/operations.png)

Die Arbeitsmappe „Vorgänge“ ermöglicht es Benutzern, sich mit sämtlichen Details aller Transaktionen zu befassen. Diese können über die Kacheln im oberen Bereich nach Ergebnisstatus gefiltert werden.

![Screenshot: Vorgangsansicht](./media/key-vaults-insights-overview/info.png)

Benutzer können in der oberen Tabelle auch Sichten auf der Grundlage spezifischer Transaktionstypen festlegen, wodurch die untere Tabelle dynamisch aktualisiert wird. Dort können Benutzer dann umfassende Vorgangsdetails in einem eingeblendeten Kontextbereich anzeigen.

>[!NOTE]
> Zum Anzeigen dieser Arbeitsmappe müssen die Diagnoseeinstellungen aktiviert sein. Weitere Informationen zum Aktivieren von Diagnoseeinstellungen finden Sie unter [Azure Key Vault-Protokollierung](../../key-vault/general/logging.md).

## <a name="pin-and-export"></a>Anheften und exportieren

Sie können einen beliebigen Metrikabschnitt an ein Azure-Dashboard anheften, indem Sie rechts oben im Abschnitt das Stecknadelsymbol auswählen.

Die Ergebnisse der Übersichts- oder Fehlerarbeitsmappe für mehrere Abonnements und Schlüsseltresore können im Excel-Format exportiert werden. Wählen Sie hierzu rechts neben dem Stecknadelsymbol das Downloadsymbol aus.

![Screenshot: Ausgewähltes Stecknadelsymbol](./media/key-vaults-insights-overview/pin.png)

## <a name="customize-azure-monitor-for-key-vault"></a>Anpassen von Azure Monitor für Key Vault

In diesem Abschnitt werden gängige Szenarien für die Bearbeitung der Arbeitsmappe erläutert, um sie zur Unterstützung Ihrer Datenanalyseanforderungen anzupassen:
*  Festlegen des Bereichs der Arbeitsmappe, sodass immer ein bestimmtes Abonnement oder mindestens ein bestimmter Schlüsseltresor ausgewählt ist
* Ändern von Metriken im Raster
* Ändern des Schwellenwerts für Anforderungen
* Ändern des Farbrendering

Aktivieren Sie zum Vornehmen von Anpassungen zunächst den Bearbeitungsmodus, indem Sie auf der oberen Symbolleiste die Schaltfläche **Anpassen** auswählen.

![Screenshot: Schaltfläche „Anpassen“](./media/key-vaults-insights-overview/customize.png)

Anpassungen werden in einer benutzerdefinierten Arbeitsmappe gespeichert, um zu verhindern, dass die Standardkonfiguration in unserer veröffentlichten Arbeitsmappe überschrieben wird. Arbeitsmappen werden in einer Ressourcengruppe gespeichert, und zwar entweder in Ihrem privaten Bereich „Meine Berichte“ oder im Bereich „Freigegebene Berichte“, der für alle Benutzer mit Zugriff auf die Ressourcengruppe zugänglich ist. Nachdem Sie die benutzerdefinierte Arbeitsmappe gespeichert haben, müssen Sie zum Arbeitsmappenkatalog wechseln, um die Mappe zu starten.

![Screenshot: Arbeitsmappenkatalog](./media/key-vaults-insights-overview/gallery.png)

### <a name="specifying-a-subscription-or-key-vault"></a>Angeben eines Abonnements oder Schlüsseltresors

Sie können den Bereich der Übersichts- oder Fehlerarbeitsmappe für mehrere Abonnements und Schlüsseltresore bei jeder Ausführung auf mindestens ein bestimmtes Abonnement oder mindestens einen bestimmten Schlüsseltresor festlegen. Führen Sie dazu die folgenden Schritte aus:

1. Wählen Sie im Portal die Option **Monitor** und anschließend im linken Bereich die Option **Schlüsseltresore (Vorschau)** aus.
2. Wählen Sie in der Arbeitsmappe **Übersicht** in der Befehlsleiste den Befehl **Bearbeiten** aus.
3. Wählen Sie in der Dropdownliste **Abonnements** mindestens ein Abonnement aus, das Sie als Standard verwenden möchten. Beachten Sie, dass die Arbeitsmappe die Auswahl von bis zu 10 Abonnements unterstützt.
4. Wählen Sie in der Dropdownliste **Schlüsseltresore** mindestens ein Konto aus, das Sie als Standard verwenden möchten. Beachten Sie, dass die Arbeitsmappe die Auswahl von bis zu insgesamt 200 Speicherkonten unterstützt.
5. Wählen Sie in der Befehlsleiste den Befehl **Speichern unter** aus, um eine Kopie der Arbeitsmappe mit Ihren Anpassungen zu speichern. Klicken Sie dann auf **Bearbeitung abgeschlossen**, um zum Lesemodus zurückzukehren.

## <a name="troubleshooting"></a>Problembehandlung

Allgemeine Anleitungen zur Problembehandlung finden Sie im Artikel zur [Problembehandlung für arbeitsmappenbasierte Erkenntnisse](troubleshoot-workbooks.md).

Dieser Abschnitt unterstützt Sie bei der Diagnose und Behandlung einiger bekannter Probleme, die bei der Verwendung von Azure Monitor für Key Vault (Vorschauversion) auftreten können. In der Liste unten finden Sie die für Ihre spezifische Fragestellung relevanten Informationen.

### <a name="resolving-performance-issues-or-failures"></a>Beheben von Leistungsproblemen oder Fehlern

Informationen zur Behandlung schlüsseltresorbezogener Probleme, die Sie mit Azure Monitor für Key Vault (Vorschauversion) identifizieren, finden Sie in der [Dokumentation zu Azure Key Vault](../../key-vault/index.yml).

### <a name="why-can-i-only-see-200-key-vaults"></a>Warum werden nur 200 Schlüsseltresore angezeigt?

Die Anzahl auswähl- und anzeigbarer Schlüsseltresore ist auf 200 begrenzt. Es können also maximal 200 Schlüsseltresore ausgewählt werden. Dieser Grenzwert ist unabhängig von der Anzahl ausgewählter Abonnements.

### <a name="why-dont-i-see-all-my-subscriptions-in-the-subscription-picker"></a>Warum werden in der Abonnementauswahl nicht alle meine Abonnements angezeigt?

Es werden nur Abonnements angezeigt, die Schlüsseltresore enthalten (aus dem gewählten Abonnementfilter). Diese werden im Header des Azure-Portals unter „Verzeichnis + Abonnement“ ausgewählt.

![Screenshot: Abonnementfilter](./media/key-vaults-insights-overview/Subscriptions.png)

### <a name="i-am-getting-an-error-message-that-the-query-exceeds-the-maximum-number-of-workspacesregions-allowed-what-to-do-now"></a>Ich erhalte eine Fehlermeldung mit dem Hinweis, dass bei der Abfrage die maximal zulässige Anzahl von Arbeitsbereichen/Regionen überschritten wird. Was kann ich tun?

Aktuell sind maximal 25 Regionen und 200 Arbeitsbereiche zulässig. Verringern Sie die Anzahl von Abonnements und/oder Ressourcengruppen, um Ihre Daten anzuzeigen.

### <a name="i-want-to-make-changes-or-add-additional-visualizations-to-key-vault-insights-how-do-i-do-so"></a>Wie kann ich Änderungen an Key Vault-Erkenntnissen vornehmen oder weitere Visualisierungen hinzufügen?

Wenn Sie Änderungen vornehmen möchten, wählen Sie den Bearbeitungsmodus aus, um die Arbeitsmappe zu ändern. Anschließend können Sie Ihre Arbeit als neue Arbeitsmappe speichern, die an ein bestimmtes Abonnement und an eine bestimmte Ressourcengruppe gebunden ist.

### <a name="what-is-the-time-grain-once-we-pin-any-part-of-the-workbooks"></a>Welches Aggregationsintervall wird verwendet, wenn ein Teil der Arbeitsmappen angeheftet wird?

Dies hängt vom gewählten Zeitbereich ab, da wir das Aggregationsintervall „Auto“ verwenden.

### <a name="what-is-the-time-range-when-any-part-of-the-workbook-is-pinned"></a>Welcher Zeitbereich wird verwendet, wenn ein Teil der Arbeitsmappe angeheftet wird?

Der Zeitbereich hängt von den Dashboardeinstellungen ab.

### <a name="why-do-i-not-see-any-data-for-my-key-vault-under-the-operations--latency-sections"></a>Warum werden in den Abschnitten von „Vorgänge und Wartezeit“ keine Daten für meine Key Vault-Instanz angezeigt?

Zum Anzeigen Ihrer protokollbasierten Daten müssen Protokolle für die einzelnen Schlüsseltresore aktiviert werden, die Sie überwachen möchten. Dies kann in den Diagnoseeinstellungen für den jeweiligen Schlüsseltresor konfiguriert werden. Die Daten müssen an einen bestimmten Log Analytics-Arbeitsbereich gesendet werden.

### <a name="i-have-already-enabled-logs-for-my-key-vault-why-am-i-still-unable-to-see-my-data-under-operations--latency"></a>Ich habe bereits Protokolle für meine Key Vault-Instanz aktiviert, unter „Vorgänge und Wartezeit“ werden aber trotzdem keine Daten angezeigt. Woran liegt das?

Diagnoseprotokolle sind aktuell nicht rückwirkend. Daten werden also erst angezeigt, nachdem Aktionen für Ihre Schlüsseltresore ausgeführt wurden. Dies kann es eine Weile (zwischen einigen Stunden und einem Tag) dauern – je nachdem, wie aktiv Ihr Schlüsseltresor ist.

Wenn Sie zahlreiche Schlüsseltresore und Abonnements ausgewählt haben, können die Daten außerdem unter Umständen aufgrund von Abfrageeinschränkungen nicht angezeigt werden. Verringern Sie ggf. die Anzahl ausgewählter Abonnements oder Schlüsseltresore, damit Ihre Daten angezeigt werden können. 

### <a name="what-if-i-want-to-see-other-data-or-make-my-own-visualizations-how-can-i-make-changes-to-the-key-vault-insights"></a>Was kann ich tun, wenn ich andere Daten anzeigen oder eigene Visualisierungen erstellen möchte? Wie kann ich Änderungen an den Key Vault-Erkenntnissen vornehmen?

Sie können die vorhandene Arbeitsmappe im Bearbeitungsmodus bearbeiten und Ihre Arbeit als neue Arbeitsmappe speichern, die alle neuen Änderungen enthält.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich unter [Erstellen interaktiver Berichte mit Azure Monitor-Arbeitsmappen](../platform/workbooks-overview.md) über die Szenarien, die Arbeitsmappen unterstützen sollen, wie Sie neue Berichte erstellen und vorhandene Berichte anpassen können, und vieles mehr.
