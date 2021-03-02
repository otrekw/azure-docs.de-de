---
title: Erstellen von Warnungen für „Änderungsnachverfolgung und Bestand“ von Azure Automation
description: In diesem Artikel erfahren Sie, wie Sie Azure-Warnungen konfigurieren, um über den Status von Änderungen zu informieren, die von „Änderungsnachverfolgung und Bestand“ erkannt wurden.
services: automation
ms.subservice: change-inventory-management
ms.date: 10/15/2020
ms.topic: conceptual
ms.openlocfilehash: 4faa7837a75bab6dfab651862754cd92394c5137
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100585915"
---
# <a name="how-to-create-alerts-for-change-tracking-and-inventory"></a>Erstellen von Warnungen für „Änderungsnachverfolgung und Bestand“

Warnungen in Azure benachrichtigen Sie proaktiv über Ergebnisse von Runbookaufträgen, Probleme mit der Dienstintegrität oder andere Szenarien im Zusammenhang mit Ihrem Automation-Konto. Azure Automation enthält keine vorkonfigurierten Warnungsregeln, Sie können jedoch eigene Regeln basierend auf den generierten Daten erstellen. Dieser Artikel enthält Anleitungen zum Erstellen von Warnungsregeln, die auf Änderungen basieren, die von „Änderungsnachverfolgung und Bestand“ identifiziert werden.

Wenn Sie mit Azure Monitor-Warnungen nicht vertraut sind, lesen Sie zunächst den [Überblick über Warnungen in Microsoft Azure](../../azure-monitor/alerts/alerts-overview.md). Weitere Informationen zu Warnungen, bei denen Protokollabfragen verwendet werden, finden Sie unter [Protokollwarnungen in Azure Monitor](../../azure-monitor/alerts/alerts-unified-log.md).

## <a name="create-alert"></a>Erstellen einer Warnung

Das folgende Beispiel zeigt, dass die Datei **c:\windows\system32\drivers\etc\hosts** auf einem Computer geändert wurde. Diese Datei ist wichtig, da Sie von Windows zum Auflösen von Hostnamen in IP-Adressen verwendet wird. Dieser Vorgang hat Vorrang vor DNS und kann zu Konnektivitätsproblemen führen. Er kann auch zur Umleitung von Datenverkehr zu böswilligen oder anderweitig gefährlichen Websites führen.

![Diagramm mit der Änderung der Hostdatei](./media/configure-alerts/changes.png)

Lassen Sie uns anhand dieses Beispiels die Schritte zum Erstellen von Warnungen bei einer Änderung erläutern.

1. Wählen Sie auf der Seite **Nachverfolgung ändern** in Ihrem Automation-Konto **Log Analytics** aus.

2. Suchen Sie in der Protokollsuche nach Inhaltsänderungen in der Datei **hosts** mit der Abfrage `ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"`. Diese Abfrage sucht nach einer Inhaltsänderung für Dateien, deren vollqualifizierte Pfadnamen das Wort `hosts` enthalten. Sie können auch eine bestimmte Datei abfragen, indem Sie den Pfadabschnitt in seine vollqualifizierte Form ändern (z. B. unter Verwendung von `FileSystemPath == "c:\windows\system32\drivers\etc\hosts"`).

3. Nachdem die Abfrage die Ergebnisse geliefert hat, wählen Sie **Neue Warnungsregel** in der Protokollsuchfunktion aus, um die Seite zur **Erstellung von Warnungen** zu öffnen. Sie können zu dieser Seite auch über **Azure Monitor** im Azure-Portal navigieren.

4. Überprüfen Sie Ihre Abfrage erneut, und ändern Sie die Warnungslogik. In diesem Fall soll die Warnung ausgelöst werden, wenn auch nur eine Änderung auf allen Computern in der Umgebung erkannt wird.

    ![Änderung an der Abfrage zum Nachverfolgen von Änderungen an der Datei „hosts“](./media/configure-alerts/change-query.png)

5. Nachdem die Warnungslogik festgelegt wurde, weisen Sie Aktionsgruppen zu, um Aktionen als Reaktion auf das Auslösen der Warnung durchzuführen. In diesem Fall richten wir zu sendende E-Mails ein sowie ein ITSM-Ticket (IT-Service-Management), das erstellt werden soll.

Gehen Sie wie folgt vor, um Warnungen einzurichten, die Sie über den Status einer Updatebereitstellung informieren: Wenn Sie noch nicht mit Azure-Warnungen vertraut sind, lesen Sie die Informationen unter [Überblick über Warnungen in Microsoft Azure](../../azure-monitor/alerts/alerts-overview.md).

## <a name="configure-action-groups-for-your-alerts"></a>Konfigurieren von Aktionsgruppen für Ihre Warnungen

Nachdem Sie Ihre Warnungen konfiguriert haben, können Sie eine Aktionsgruppe einrichten. Dabei handelt es sich um eine Gruppe von Aktionen, die für mehrere Warnungen verwendet werden sollen. Dabei kann es sich beispielsweise um E-Mail-Benachrichtigungen, Runbooks oder Webhooks handeln. Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../../azure-monitor/alerts/action-groups.md).

1. Wählen Sie eine Warnung und dann unter **Aktionsgruppen** die Option **Neu erstellen** aus.

2. Geben Sie einen vollständigen und einen kurzen Namen für die Aktionsgruppe ein. Der Kurzname wird von der Updateverwaltung verwendet, wenn Benachrichtigungen unter Verwendung der angegebenen Gruppe gesendet werden.

3. Geben Sie unter **Aktionen** einen Namen ein, der die Aktion angibt, z. B. **E-Mail-Benachrichtigung**.

4. Wählen Sie für **Aktionstyp** den entsprechenden Typ aus, z. B. **Email/SMS/Push/Voice** (E-Mail/SMS/Push/Sprache).

5. Wählen Sie **Details bearbeiten** aus.

6. Füllen Sie den Bereich für den Aktionstyp aus. Wenn Sie beispielsweise **Email/SMS/Push/Voice** (E-Mail/SMS/Push/Sprache) verwenden, geben Sie einen Aktionsnamen ein, aktivieren Sie das Kontrollkästchen **E-Mail**, geben Sie eine gültige E-Mail-Adresse ein, und wählen Sie dann **OK** aus.

    ![Konfigurieren der E-Mail-Aktionsgruppe](./media/configure-alerts/configure-email-action-group.png)

7. Wählen Sie im Bereich „Aktionsgruppe hinzufügen“ die Option **OK** aus.

8. Für eine Warn-E-Mail können Sie den E-Mail-Betreff anpassen. Wählen Sie unter **Regel erstellen** die Option **Aktionen anpassen** und dann **E-Mail-Betreff** aus.

9. Klicken Sie abschließend auf **Warnungsregel erstellen**.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Warnungen in Azure Monitor](../../azure-monitor/alerts/alerts-overview.md).

* Erfahren Sie mehr über [Protokollabfragen](../../azure-monitor/logs/log-query-overview.md) zum Abrufen und Analysieren von Daten aus einem Log Analytics-Arbeitsbereich.

* Verwalten von [Nutzung und Kosten mit Azure Monitor-Protokollen](../../azure-monitor/logs/manage-cost-storage.md) beschreibt, wie Sie Ihre Kosten durch Ändern der Datenaufbewahrungsdauer steuern sowie Ihre Datennutzung analysieren und Warnungen dazu ausgeben.