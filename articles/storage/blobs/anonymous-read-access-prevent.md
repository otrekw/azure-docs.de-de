---
title: Verhindern von anonymem öffentlichem Lesezugriff auf Container und Blobs
titleSuffix: Azure Storage
description: ''
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/06/2020
ms.author: tamram
ms.reviewer: fryu
ms.openlocfilehash: 90d7cd65bbc07524391f34fe0efce2b044664cef
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86209047"
---
# <a name="prevent-anonymous-public-read-access-to-containers-and-blobs"></a>Verhindern von anonymem öffentlichem Lesezugriff auf Container und Blobs

Anonymer öffentlicher Lesezugriff auf Container und Blobs in Azure Storage ist eine praktische Methode für die Weitergabe von Daten, stellt aber unter Umständen auch ein Sicherheitsrisiko dar. Anonymer Zugriff muss mit Bedacht aktiviert werden, und Sie müssen wissen, wie der anonyme Zugriff auf Ihre Daten zu bewerten ist. Betriebliche Komplexität, menschliche Fehler oder böswillige Angriffe auf öffentlich zugängliche Daten können kostspielige Datenschutzverletzungen zur Folge haben. Microsoft empfiehlt, den anonymen Zugriff nur zu aktivieren, wenn dies für Ihr Anwendungsszenario erforderlich ist.

Standardmäßig kann ein Speicherkontobenutzer mit entsprechenden Berechtigungen den öffentlichen Zugriff auf Container und Blobs konfigurieren. Diese Funktion kann auf der Speicherkontoebene deaktiviert werden, damit Container und Blobs im Konto nicht mehr für den öffentlichen Zugriff konfiguriert werden können.

In diesem Artikel erfahren Sie, wie Sie anonyme Anforderungen für ein Speicherkonto analysieren und den anonymen Zugriff für das gesamte Speicherkonto oder für einen einzelnen Container verhindern.

## <a name="detect-anonymous-requests-from-client-applications"></a>Erkennen anonymer Anforderungen von Clientanwendungen

Wenn Sie den öffentlichen Lesezugriff für ein Speicherkonto deaktivieren, besteht die Gefahr, dass Anforderungen für Container und Blobs abgelehnt werden, die gegenwärtig für den öffentlichen Zugriff konfiguriert sind. Durch die Deaktivierung des öffentlichen Zugriffs für ein Speicherkonto werden die Einstellungen für den öffentlichen Zugriff für alle Container in diesem Speicherkonto überschrieben. Wenn der öffentliche Zugriff für das Speicherkonto deaktiviert wird, tritt künftig bei anonymen Anforderungen für dieses Konto ein Fehler auf.

Um die möglichen Auswirkungen der Deaktivierung des öffentlichen Zugriffs auf Clientanwendungen zu verstehen, empfiehlt Microsoft, die Protokollierung und Metriken für dieses Konto zu aktivieren und Muster anonymer Anforderungen für ein Zeitintervall zu analysieren. Verwenden Sie Metriken, um die Anzahl anonymer Anforderungen für das Speicherkonto zu ermitteln, und Protokolle, um zu ermitteln, auf welche Container anonym zugegriffen wird.

### <a name="monitor-anonymous-requests-with-metrics-explorer"></a>Überwachen anonymer Anforderungen mit dem Metrik-Explorer

Verwenden Sie den Azure-Metrik-Explorer im Azure-Portal, um anonyme Anforderungen für ein Speicherkonto nachzuverfolgen. Weitere Informationen zum Metrik-Explorer finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../../azure-monitor/platform/metrics-getting-started.md).

Gehen Sie wie folgt vor, um eine Metrik zur Nachverfolgung anonymer Anforderungen zu erstellen:

1. Navigieren Sie zum Speicherkonto im Azure-Portal. Wählen Sie im Abschnitt **Überwachung** die Option **Metriken** aus.
1. Wählen Sie **Metrik hinzufügen** aus. Geben Sie im Dialogfeld **Metrik** die folgenden Werte an:
    1. Behalten Sie im Bereichsfeld den Namen des Speicherkontos bei.
    1. Legen Sie **Metriknamespace** auf *Blob* fest. Von dieser Metrik werden nur Anforderungen für Blobspeicher gemeldet.
    1. Legen Sie das Feld **Metrik** auf *Transaktionen* fest.
    1. Legen Sie das Feld **Aggregation** auf *Summe* fest.

    Die neue Metrik liefert die Summe der Transaktionen für Blobspeicher in einem bestimmten Zeitraum. Die resultierende Metrik sieht wie folgt aus:

    :::image type="content" source="media/anonymous-read-access-prevent/configure-metric-blob-transactions.png" alt-text="Screenshot: Konfigurieren einer Metrik zum Summieren von Blobtransaktionen":::

1. Wählen Sie als Nächstes die Schaltfläche **Filter hinzufügen** aus, um einen Filter für die Metrik für anonyme Anforderungen zu erstellen.
1. Geben Sie im Dialogfeld **Filter** die folgenden Werte an:
    1. Legen Sie **Eigenschaft** auf *Authentifizierung* fest.
    1. Legen Sie das Feld **Operator** auf das Gleichheitszeichen (=) fest.
    1. Legen Sie das Feld **Werte** auf *Anonym* fest.
1. Wählen Sie rechts oben das gewünschte Zeitintervall für die Metrik aus. Sie können auch angeben, wie präzise die Aggregation von Anforderungen sein soll. Hierzu können Intervalle zwischen einer Minute und einem Monat angegeben werden.

Nachdem Sie die Metrik konfiguriert haben, werden nach und nach anonyme Anforderungen im Diagramm angezeigt. Die folgende Abbildung zeigt aggregierte anonyme Anforderungen der letzten 30 Minuten:

:::image type="content" source="media/anonymous-read-access-prevent/metric-anonymous-blob-requests.png" alt-text="Screenshot: Aggregierte anonyme Anforderungen für Blobspeicher":::

Sie können auch eine Warnungsregel konfigurieren, um benachrichtigt zu werden, wenn eine bestimmte Anzahl anonymer Anforderungen an Ihr Speicherkonto gerichtet wird. Weitere Informationen finden Sie unter [Erstellen, Anzeigen und Verwalten von Metrikwarnungen mit Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

### <a name="analyze-logs-to-identify-containers-receiving-anonymous-requests"></a>Analysieren von Protokollen zur Identifizierung von Containern, die anonyme Anforderungen erhalten

In Azure Storage-Protokollen werden Details zu Anforderungen für das Speicherkonto erfasst. Hierzu zählt auch die Information, wie eine Anforderung autorisiert wurde. Sie können die Protokolle analysieren, um zu ermitteln, von welchen Containern anonyme Anforderungen empfangen werden.

Wenn Sie Anforderungen für Ihr Azure Storage-Konto protokollieren möchten, um anonyme Anforderungen zu untersuchen, können Sie die Azure Storage-Protokollierung in Azure Monitor (Vorschauversion) verwenden. Weitere Informationen finden Sie unter [Überwachen von Azure Storage](../common/monitor-storage.md).

Die Azure Storage-Protokollierung in Azure Monitor unterstützt die Verwendung von Protokollabfragen für die Analyse von Protokolldaten. Für die Abfrage von Protokollen können Sie einen Azure Log Analytics-Arbeitsbereich verwenden. Weitere Informationen zu Protokollabfragen finden Sie unter [Tutorial: Erste Schritte mit Log Analytics-Abfragen](../../azure-monitor/log-query/get-started-portal.md).

#### <a name="create-a-diagnostic-setting-in-the-azure-portal"></a>Erstellen einer Diagnoseeinstellung im Azure-Portal

Wenn Sie Azure Storage-Daten mit Azure Monitor protokollieren und mit Azure Log Analytics analysieren möchten, müssen Sie zunächst eine Diagnoseeinstellung erstellen, die angibt, welche Anforderungstypen und für welche Speicherdienste Daten protokolliert werden sollen. Führen Sie zum Erstellen einer Diagnoseeinstellung im Azure-Portal die folgenden Schritte aus:

1. Registrieren Sie sich für die [Azure Storage-Protokollierung in Azure Monitor (Vorschauversion)](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u).
1. Erstellen Sie in dem Abonnement, das Ihr Azure Storage-Konto enthält, einen neuen Log Analytics-Arbeitsbereich. Nachdem Sie die Protokollierung für Ihr Speicherkonto konfiguriert haben, sind die Protokolle im Log Analytics-Arbeitsbereich verfügbar. Weitere Informationen finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](../../azure-monitor/learn/quick-create-workspace.md).
1. Navigieren Sie zum Speicherkonto im Azure-Portal.
1. Klicken Sie im Abschnitt „Überwachung“ auf **Diagnoseeinstellungen (Vorschau)** .
1. Wählen Sie **Blob** aus, um Anforderungen für Blobspeicher zu protokollieren.
1. Klicken Sie auf **Diagnoseeinstellung hinzufügen**.
1. Geben Sie einen Namen für die Diagnoseeinstellung an.
1. Wählen Sie unter **Kategoriedetails** im Abschnitt **Protokoll** aus, welche Arten von Anforderungen protokolliert werden sollen. Da es sich bei anonymen Anforderungen um Leseanforderungen handelt, wählen Sie **StorageRead** aus, um anonyme Anforderungen zu erfassen.
1. Wählen Sie unter **Zieldetails** die Option **An Log Analytics senden** aus. Wählen Sie Ihr Abonnement und den zuvor erstellten Log Analytics-Arbeitsbereich aus, wie in der folgenden Abbildung zu sehen:

    :::image type="content" source="media/anonymous-read-access-prevent/create-diagnostic-setting-logs.png" alt-text="Screenshot: Erstellen einer Diagnoseeinstellung für die Protokollierung von Anforderungen":::

Nachdem Sie die Diagnoseeinstellung erstellt haben, werden Anforderungen für das Speicherkonto gemäß dieser Einstellung protokolliert. Weitere Informationen finden Sie unter [Erstellen von Diagnoseeinstellungen zum Senden von Plattformprotokollen und Metriken an verschiedene Ziele](../../azure-monitor/platform/diagnostic-settings.md).

Eine Referenz der Felder, die in Azure Storage-Protokollen in Azure Monitor verfügbar sind, finden Sie unter [Ressourcenprotokolle (Vorschau)](../common/monitor-storage-reference.md#resource-logs-preview).

#### <a name="query-logs-for-anonymous-requests"></a>Abfragen anonymer Anforderungen in Protokollen

Azure Storage-Protokolle in Azure Monitor enthalten die Art der Autorisierung, die bei einer Anforderung für ein Speicherkonto verwendet wurde. Filtern Sie in Ihrer Protokollabfrage nach der Eigenschaft **AuthenticationType**, um anonyme Anforderungen anzuzeigen.

Wenn Sie Protokolle für anonyme Blobspeicheranforderungen der letzten sieben Tage abrufen möchten, öffnen Sie Ihren Log Analytics-Arbeitsbereich. Fügen Sie dann die folgende Abfrage in eine neue Protokollabfrage ein, und führen Sie sie aus. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```kusto
StorageBlobLogs
| where TimeGenerated > ago(7d) and AuthenticationType == "Anonymous"
| project TimeGenerated, AccountName, AuthenticationType, Uri
```

Sie können auch eine auf dieser Abfrage basierende Warnungsregel konfigurieren, um über anonyme Anforderungen informiert zu werden. Weitere Informationen finden Sie unter [Erstellen, Anzeigen und Verwalten von Protokollwarnungen mithilfe von Azure Monitor](../../azure-monitor/platform/alerts-log.md).

## <a name="remediate-anonymous-public-access"></a>Behandeln von anonymem öffentlichem Zugriff

Nachdem Sie anonyme Anforderungen für Container und Blobs in Ihrem Speicherkonto untersucht haben, können Sie Maßnahmen ergreifen, um den öffentlichen Zugriff einzuschränken oder zu verhindern. Müssen einige Container in Ihrem Speicherkonto für öffentliche Zugriffe verfügbar sein, können Sie die Einstellung für den öffentlichen Zugriff für jeden Container in Ihrem Speicherkonto konfigurieren. Dank dieser Option kann der öffentliche Zugriff äußerst präzise gesteuert werden. Weitere Informationen finden Sie unter [Festlegen der öffentlichen Zugriffsebene auf einen Container](anonymous-read-access-configure.md#set-the-public-access-level-for-a-container).

Zur Erhöhung der Sicherheit können Sie den öffentlichen Zugriff für ein gesamtes Speicherkonto deaktivieren. Durch die Einstellung für den öffentlichen Zugriff für ein Speicherkonto werden die individuellen Einstellungen für Container in diesem Konto überschrieben. Wenn Sie den öffentlichen Zugriff für ein Speicherkonto deaktivieren, kann auf Container, die für öffentlichen Zugriff konfiguriert sind, nicht mehr anonym zugegriffen werden. Weitere Informationen finden Sie unter [Aktivieren oder Deaktivieren des öffentlichen Lesezugriffs für ein Speicherkonto](anonymous-read-access-configure.md#enable-or-disable-public-read-access-for-a-storage-account).

Falls in Ihrem Szenario bestimmte Container für öffentlichen Zugriff verfügbar sein müssen, empfiehlt es sich gegebenenfalls, diese Container und die zugehörigen Blobs in Speicherkonten zu verschieben, die für öffentlichen Zugriff reserviert sind. Anschließend können Sie den öffentlichen Zugriff für alle anderen Speicherkonten deaktivieren.

### <a name="verify-that-public-access-to-a-blob-is-not-permitted"></a>Vergewissern, dass der öffentliche Zugriff auf ein Blob nicht gestattet ist

Wenn Sie sich vergewissern möchten, dass öffentliche Zugriffe auf ein bestimmtes Blob verweigert werden, können Sie versuchen, das Blob über seine URL herunterzuladen. Ist der Download erfolgreich, ist das Blob weiterhin öffentlich verfügbar. Ist das Blob nicht öffentlich zugänglich, da der öffentliche Zugriff für das Speicherkonto deaktiviert wurde, wird eine Fehlermeldung mit dem Hinweis angezeigt, dass öffentliche Zugriffe auf dieses Speicherkonto nicht zulässig sind.

Im folgenden Beispiel wird gezeigt, wie Sie mithilfe von PowerShell versuchen können, ein Blob über seine URL herunterzuladen. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```powershell
$url = "<absolute-url-to-blob>"
$downloadTo = "<file-path-for-download>"
Invoke-WebRequest -Uri $url -OutFile $downloadTo -ErrorAction Stop
```

### <a name="verify-that-modifying-the-containers-public-access-setting-is-not-permitted"></a>Vergewissern, dass die Einstellung für den öffentlichen Zugriff des Containers nicht geändert werden kann

Wenn Sie sich vergewissern möchten, dass die Einstellung für den öffentlichen Zugriff eines Containers nicht geändert werden kann, nachdem der öffentliche Zugriff für das Speicherkonto deaktiviert wurde, können Sie versuchen, die Einstellung zu ändern. Wurde der öffentliche Zugriff für das Speicherkonto deaktiviert, kann die Einstellung für den öffentlichen Zugriff eines Containers nicht geändert werden.

Im folgenden Beispiel wird gezeigt, wie Sie mithilfe von PowerShell versuchen können, die Einstellung für den öffentlichen Zugriff eines Containers zu ändern. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```powershell
$rgName = "<resource-group>"
$accountName = "<storage-account>"
$containerName = "<container-name>"

$storageAccount = Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context

Set-AzStorageContainerAcl -Context $ctx -Container $containerName -Permission Blob
```

### <a name="verify-that-creating-a-container-with-public-access-enabled-is-not-permitted"></a>Vergewissern, dass kein Container mit aktiviertem öffentlichem Zugriff erstellt werden kann

Wenn der öffentliche Zugriff für das Speicherkonto deaktiviert ist, können Sie keinen neuen Container mit aktiviertem öffentlichem Zugriff erstellen. Zur Überprüfung können Sie versuchen, einen Container mit aktiviertem öffentlichem Zugriff zu erstellen.

Im folgenden Beispiel wird gezeigt, wie Sie mithilfe von PowerShell versuchen können, einen Container mit aktiviertem öffentlichem Zugriff zu erstellen. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:
 
```powershell
$rgName = "<resource-group>"
$accountName = "<storage-account>"
$containerName = "<container-name>"

$storageAccount = Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context

New-AzStorageContainer -Name $containerName -Permission Blob -Context $ctx
```

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren des anonymen, öffentlichen Lesezugriffs auf Container und Blobs](anonymous-read-access-configure.md)
- [Anonymer Zugriff auf öffentliche Container und Blobs mit .NET](anonymous-read-access-client.md)