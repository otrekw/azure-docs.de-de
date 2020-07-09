---
title: Verwalten von Änderungsnachverfolgung und Bestand in Azure Automation
description: In diesem Artikel wird beschrieben, wie Sie Änderungsnachverfolgung und Bestand verwenden, um Änderungen an Software und Microsoft-Diensten in Ihrer Umgebung nachzuverfolgen.
services: automation
ms.subservice: change-inventory-management
ms.date: 06/15/2020
ms.topic: conceptual
ms.openlocfilehash: 0eebd626013614bb6240fc0e6530a358a2b86d1c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84781190"
---
# <a name="manage-change-tracking-and-inventory"></a>Verwalten der Änderungsnachverfolgung und des Bestands

Wenn Sie eine neue Datei oder einen neuen Registrierungsschlüssel zur Nachverfolgung hinzufügen, aktiviert Azure Automation dieses Element für [Änderungsnachverfolgung und Bestand](change-tracking.md). In diesem Artikel wird beschrieben, wie Nachverfolgung konfiguriert, Nachverfolgungsergebnisse überprüft und Warnungen behandelt werden, wenn Änderungen erkannt werden.

Bevor Sie die Verfahren in diesem Artikel verwenden, stellen Sie sicher, dass Sie „Änderungsnachverfolgung und Bestand“ über eines der folgenden Verfahren auf Ihren VMs aktiviert haben:

* [Aktivieren von „Änderungsnachverfolgung und Bestand“ über ein Automation-Konto](automation-enable-changes-from-auto-acct.md)
* [Aktivieren von „Änderungsnachverfolgung und Bestand“ durch Durchsuchen im Azure-Portal](automation-enable-changes-from-browse.md)
* [Aktivieren von „Änderungsnachverfolgung und Bestand“ über ein Runbook](automation-enable-changes-from-runbook.md)
* [Aktivieren von „Änderungsnachverfolgung und Bestand“ über einen virtuellen Azure-Computer](automation-enable-changes-from-vm.md)

## <a name="limit-the-scope-for-the-deployment"></a><a name="scope-configuration"></a>Beschränken des Bereichs für die Bereitstellung

Von „Änderungsnachverfolgung und Bestand“ wird eine Bereichskonfiguration innerhalb des Arbeitsbereichs verwendet, um die Zielcomputer für Änderungen anzugeben. Weitere Informationen finden Sie unter [Arbeiten mit Bereichskonfigurationen für „Änderungsnachverfolgung und Bestand“](automation-scope-configurations-change-tracking.md).

## <a name="track-files"></a>Dateien nachverfolgen

Sie können mit „Änderungsnachverfolgung und Bestand“ Änderungen an Dateien und Ordnern/Verzeichnissen verfolgen. In diesem Abschnitt erfahren Sie, wie Sie die Dateinachverfolgung unter Windows und Linux konfigurieren.

### <a name="configure-file-tracking-on-windows"></a>Konfigurieren der Dateinachverfolgung unter Windows

Führen Sie zum Konfigurieren der Dateinachverfolgung auf Windows-Computern die folgenden Schritte aus:

1. Wählen Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** die Option **Änderungsnachverfolgung** aus. 
2. Klicken Sie auf **Einstellungen bearbeiten** (das Zahnradsymbol).
3. Wählen Sie auf der Seite „Arbeitsbereichskonfiguration“ die Option **Windows-Dateien** aus, und klicken Sie dann auf **+ Hinzufügen**, um eine neue nachzuverfolgende Datei hinzuzufügen.
4. Geben Sie im Bereich „Windows-Datei für Änderungsnachverfolgung hinzufügen“ die Informationen zu der nachzuverfolgenden Datei bzw. dem Ordner ein, und klicken Sie auf **Speichern**. Die folgende Tabelle definiert die Eigenschaften, die Sie für die Informationen verwenden können.

    |Eigenschaft  |BESCHREIBUNG  |
    |---------|---------|
    |Aktiviert     | TRUE, wenn die Einstellung angewendet wird, andernfalls FALSE.        |
    |Item Name     | Anzeigename der nachzuverfolgenden Datei        |
    |Group     | Gruppenname für die logische Gruppierung von Dateien        |
    |Pfad eingeben     | Der für die Datei zu überprüfende Pfad, z. B. **c:\temp\\\*.txt**. Sie können auch Umgebungsvariablen verwenden, beispielsweise `%winDir%\System32\\\*.*`.       |
    |Pfadtyp     | Der Typ des Pfads. Mögliche Werte sind „Datei“ und „Ordner“.        |    
    |Rekursion     | TRUE, wenn ob beim Suchen nach dem nachzuverfolgenden Element die Rekursion verwendet wird, andernfalls FALSE.        |    
    |Dateiinhalt hochladen | „True“, um den Dateiinhalt von nachverfolgten Änderungen hochzuladen, andernfalls „False“.|

5. Stellen Sie sicher, dass Sie „True“ für **Dateiinhalt hochladen** angeben. Diese Einstellung ermöglicht die Nachverfolgung von Dateiinhalten für den angegebenen Dateipfad.

### <a name="configure-file-tracking-on-linux"></a>Konfigurieren der Dateinachverfolgung unter Linux

Führen Sie zum Konfigurieren der Dateinachverfolgung auf Linux-Computern die folgenden Schritte aus:

1. Wählen Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** die Option **Änderungsnachverfolgung** aus. 
2. Klicken Sie auf **Einstellungen bearbeiten** (das Zahnradsymbol).
3. Wählen Sie auf der Seite „Arbeitsbereichskonfiguration“ die Option **Linux-Dateien** aus, und klicken Sie dann auf **+ Hinzufügen**, um eine neue nachzuverfolgende Datei hinzuzufügen.
4. Geben Sie im Bereich „Linux-Datei für Änderungsnachverfolgung hinzufügen“ die Informationen zu der Datei oder dem Verzeichnis ein, die bzw. das nachverfolgt werden soll, und klicken Sie auf **Speichern**. Die folgende Tabelle definiert die Eigenschaften, die Sie für die Informationen verwenden können.

    |Eigenschaft  |BESCHREIBUNG  |
    |---------|---------|
    |Aktiviert     | TRUE, wenn die Einstellung angewendet wird, andernfalls FALSE.        |
    |Item Name     | Anzeigename der nachzuverfolgenden Datei        |
    |Group     | Gruppenname für die logische Gruppierung von Dateien        |
    |Pfad eingeben     | Der zu überprüfende Pfad für die Datei, z. B. **/etc/*.conf**.       |
    |Pfadtyp     | Der Typ des Pfads. Mögliche Werte sind „Datei“ und „Verzeichnis“.        |
    |Rekursion     | TRUE, wenn ob beim Suchen nach dem nachzuverfolgenden Element die Rekursion verwendet wird, andernfalls FALSE.        |
    |Sudo verwenden     | „True“, um sudo bei der Suche nach dem Element zu verwenden, andernfalls „False“.         |
    |Links     | Die Einstellung, die bestimmt, wie symbolische Verknüpfungen beim Durchlaufen von Verzeichnissen behandelt werden sollen. Mögliche Werte:<br> „Ignore“: Symbolische Verknüpfungen werden ignoriert, und die referenzierten Dateien/Verzeichnisse werden nicht einbezogen.<br>„Follow“: Folgt den symbolischen Verknüpfungen bei der Rekursion und bindet auch die referenzierten Dateien/Verzeichnisse ein.<br>„Manage“: Folgt den symbolischen Verknüpfungen und ermöglicht das Ändern von zurückgegebenen Inhalten.<br>**Hinweis:** Die Option „Manage“ wird nicht empfohlen, da sie das Abrufen von Dateiinhalten nicht unterstützt.    |
    |Dateiinhalt hochladen | „True“, um den Dateiinhalt von nachverfolgten Änderungen hochzuladen, andernfalls „False“. |

5. Stellen Sie sicher, dass Sie „True“ für **Dateiinhalt hochladen** angeben. Diese Einstellung ermöglicht die Nachverfolgung von Dateiinhalten für den angegebenen Dateipfad.

   ![Linux-Datei hinzufügen](./media/change-tracking-file-contents/add-linux-file.png)

## <a name="track-file-contents"></a>Dateiinhalt nachverfolgen

Die Nachverfolgung von Dateiinhalten ermöglicht Ihnen, den Inhalt einer Datei vor und nach einer verfolgten Änderung einzusehen. Die Funktion speichert den Dateiinhalt nach jeder Änderung in einem [Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-account-overview). Im Folgenden finden Sie einige Regeln, die beim Nachverfolgen von Dateiinhalten einzuhalten sind:

* Für die Speicherung von Dateiinhalten ist ein Standardspeicherkonto unter Verwendung des Ressourcen-Manager-Bereitstellungsmodells erforderlich. 
* Verwenden Sie keine Speicherkonten mit Premium- oder klassischem Bereitstellungsmodell. Weitere Informationen finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/common/storage-create-storage-account.md).
* Sie können das Speicherkonto nur mit einem einzigen Automation-Konto verbinden.
* [Änderungsnachverfolgung und Bestand](change-tracking.md) muss in Ihrem Automation-Konto aktiviert sein.

### <a name="enable-tracking-for-file-content-changes"></a>Nachverfolgung von Dateiinhaltsänderungen aktivieren

Führen Sie die folgenden Schritte aus, um das Nachverfolgen von Änderungen an Dateiinhalten zu aktivieren:

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto, und wählen Sie dann unter **Konfigurationsverwaltung** die Option **Änderungsnachverfolgung** aus.
2. Klicken Sie auf **Einstellungen bearbeiten** (das Zahnradsymbol).
3. Wählen Sie **Dateiinhalt** aus, und klicken Sie auf **Link**. Durch diese Auswahl wird der Bereich „Inhaltsspeicherort für Änderungsnachverfolgung hinzufügen“ geöffnet.

   ![Hinzufügen des Inhaltsorts](./media/change-tracking-file-contents/enable.png)

4. Wählen Sie das Abonnement und das Speicherkonto aus, die für die Speicherung von Dateiinhalten verwendet werden sollen. 

5. Wenn Sie die Nachverfolgung von Dateiinhalten für alle vorhandenen nachverfolgten Dateien aktivieren möchten, wählen Sie **Ein** für **Dateiinhalte für alle Einstellungen hochladen**. Sie können diese Einstellung für jeden Dateipfad nachträglich ändern.

   ![Speicherkonto festlegen](./media/change-tracking-file-contents/storage-account.png)

6. Änderungsnachverfolgung und Bestand zeigen Speicherkonto und SAS-URIs (Shared Access Signature) an, wenn die Nachverfolgung von Dateiinhaltsänderungen aktiviert ist. Die Signaturen laufen nach 365 Tagen ab, und Sie können Sie neu erstellen, indem Sie auf **Erneut generieren** klicken.

   ![Auflisten von Kontoschlüsseln](./media/change-tracking-file-contents/account-keys.png)

### <a name="view-the-contents-of-a-tracked-file"></a>Anzeigen des Inhalts einer nachverfolgten Datei

Sobald Änderungsnachverfolgung und Bestand eine Änderung an einer nachverfolgten Datei erkennen, können Sie den Inhalt der Datei im Bereich „Änderungsdetails“ anzeigen.  

![Änderungen auflisten](./media/change-tracking-file-contents/change-list.png)

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto, und wählen Sie dann unter **Konfigurationsverwaltung** die Option **Änderungsnachverfolgung** aus.

2. Wählen Sie eine Datei in der Liste der Änderungen aus, und wählen Sie **Änderungen am Dateiinhalt anzeigen** aus, um den Inhalt der Datei anzuzeigen. Im Änderungsdetailsbereich werden die Standarddateiinformationen von vorher und nachher für jede Eigenschaft angezeigt.

   ![Änderungsdetails](./media/change-tracking-file-contents/change-details.png)

3. Der Dateiinhalt wird in einer nebeneinander angeordneten Ansicht angezeigt. Sie können **Inline** auswählen, um eine Inlineansicht der Änderungen anzuzeigen.

## <a name="track-registry-keys"></a>Registrierungsschlüssel nachverfolgen

Führen Sie zum Konfigurieren der Nachverfolgung von Registrierungsschlüsseln auf Windows-Computern die folgenden Schritte aus:

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto, und wählen Sie dann unter **Konfigurationsverwaltung** die Option **Änderungsnachverfolgung** aus. 
2. Klicken Sie auf **Einstellungen bearbeiten** (das Zahnradsymbol).
3. Wählen Sie auf der Seite „Arbeitsbereichskonfiguration“ **Windows-Registrierung** aus.
4. Klicken Sie auf **+ Hinzufügen**, um einen neuen Registrierungsschlüssel zur Nachverfolgung hinzuzufügen.
5. Geben Sie im Bereich „Windows-Registrierung für Änderungsnachverfolgung hinzufügen“ die Informationen zu dem Schlüssel ein, der nachverfolgt werden soll, und klicken Sie auf **Speichern**. Die folgende Tabelle definiert die Eigenschaften, die Sie für die Informationen verwenden können.

    |Eigenschaft  |BESCHREIBUNG  |
    |---------|---------|
    |Aktiviert     | „True“, wenn eine Einstellung angewendet wird, andernfalls „False“.        |
    |Item Name     | Anzeigename des nachzuverfolgenden Registrierungsschlüssels.        |
    |Group     | Gruppenname für die logische Gruppierung von Registrierungsschlüsseln.        |
    |Windows-Registrierungsschlüssel   | Schlüsselname mit Pfad, z. B. `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup`.      |

## <a name="search-logs-for-change-records"></a>Protokolle nach Änderungsdatensätzen durchsuchen

Sie können verschiedene Suchvorgänge in den Azure Monitor-Protokollen für Änderungsdatensätze durchführen. Klicken Sie bei geöffneter Seite „Änderungsnachverfolgung“ auf **Log Analytics**, um die Seite „Protokolle“ zu öffnen. Die folgende Tabelle enthält Beispiele für Protokollsuchen nach Änderungsdatensätzen.

|Abfrage  |BESCHREIBUNG  |
|---------|---------|
|`ConfigurationData`<br>&#124; `where ConfigDataType == "WindowsServices" and SvcStartupType == "Auto"`<br>&#124; `where SvcState == "Stopped"`<br>&#124; `summarize arg_max(TimeGenerated, *) by SoftwareName, Computer`         | Zeigt die aktuellen Bestandsdatensätze für Microsoft-Dienste an, die auf „Auto“ festgelegt, aber als „Beendet“ gemeldet wurden. Die Ergebnisse werden auf den aktuellen Datensatz für den angegebenen Softwarenamen und Computer beschränkt.    |
|`ConfigurationChange`<br>&#124; `where ConfigChangeType == "Software" and ChangeCategory == "Removed"`<br>&#124; `order by TimeGenerated desc`|Zeigt Änderungsdatensätze für entfernte Software an.|

## <a name="create-alerts-on-changes"></a>Warnungen bei Änderungen erstellen

Das folgende Beispiel zeigt, dass die Datei **c:\windows\system32\drivers\etc\hosts** auf einem Computer geändert wurde. Diese Datei ist wichtig, da Sie von Windows zum Auflösen von Hostnamen in IP-Adressen verwendet wird. Dieser Vorgang hat Vorrang vor DNS und kann zu Konnektivitätsproblemen führen. Er kann auch zur Umleitung von Datenverkehr zu böswilligen oder anderweitig gefährlichen Websites führen.

![Diagramm mit der Änderung der Hostdatei](./media/change-tracking-file-contents/changes.png)

Lassen Sie uns anhand dieses Beispiels die Schritte zum Erstellen von Warnungen bei einer Änderung erläutern.

1. Wählen Sie in Ihrem Automation-Konto unter **Konfigurationsverwaltung** die Option **Änderungsnachverfolgung** und dann **Log Analytics** aus. 
2. Suchen Sie in der Protokollsuche nach Inhaltsänderungen in der Datei **hosts** mit der Abfrage `ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"`. Diese Abfrage sucht nach einer Inhaltsänderung für Dateien, deren vollqualifizierte Pfadnamen das Wort `hosts` enthalten. Sie können auch eine bestimmte Datei abfragen, indem Sie den Pfadabschnitt in seine vollqualifizierte Form ändern (z. B. unter Verwendung von `FileSystemPath == "c:\windows\system32\drivers\etc\hosts"`).

3. Nachdem die Abfrage die Ergebnisse geliefert hat, klicken Sie auf **Neue Warnungsregel** in der Protokollsuchfunktion, um die Seite zur Erstellung von Warnungen zu öffnen. Sie können zu dieser Seite auch über **Azure Monitor** im Azure-Portal navigieren. 

4. Überprüfen Sie Ihre Abfrage erneut, und ändern Sie die Warnungslogik. In diesem Fall soll die Warnung ausgelöst werden, wenn auch nur eine Änderung auf allen Computern in der Umgebung erkannt wird.

    ![Änderung an der Abfrage zum Nachverfolgen von Änderungen an der Datei „hosts“](./media/change-tracking-file-contents/change-query.png)

5. Nachdem die Warnungslogik festgelegt wurde, weisen Sie Aktionsgruppen zu, um Aktionen als Reaktion auf das Auslösen der Warnung durchzuführen. In diesem Fall richten wir zu sendende E-Mails ein sowie ein ITSM-Ticket (IT-Service-Management), das erstellt werden soll. 

    ![Konfigurieren der bei einer Änderung zu benachrichtigenden Aktionsgruppe](./media/change-tracking/action-groups.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu Bereichskonfigurationen finden Sie unter [Arbeiten mit Bereichskonfigurationen für „Änderungsnachverfolgung und Bestand“](automation-scope-configurations-change-tracking.md).
* Wenn Sie Protokolle durchsuchen müssen, die in Ihrem Log Analytics Arbeitsbereich gespeichert sind, finden Sie unter [Protokollsuchvorgänge in Azure Monitor-Protokollen](../log-analytics/log-analytics-log-searches.md) weitere Informationen.
* Wenn Sie mit den Bereitstellungen fertig sind, fahren Sie mit [Aufheben der Verknüpfung eines Arbeitsbereichs über ein Automation-Konto](automation-unlink-workspace-change-tracking.md) fort.
* Informationen zum Löschen Ihrer VMs aus „Änderungsnachverfolgung und Bestand“ finden Sie unter [Entfernen von VMs aus Änderungsnachverfolgung und Bestand](automation-remove-vms-from-change-tracking.md).
* Informationen zum Behandeln von Problemen bei diesem Feature finden Sie unter [Problembehandlung bei Änderungsnachverfolgung und Bestand](troubleshoot/change-tracking.md).
