---
title: Konfigurieren der erforderlichen Mindestversion der Transport Layer Security (TLS) für ein Speicherkonto
titleSuffix: Azure Storage
description: Konfigurieren Sie ein Speicherkonto so, dass eine Mindestversion der Transport Layer Security (TLS) für Clients erforderlich ist, die Anforderungen an Azure Storage senden.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/08/2020
ms.author: tamram
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: ab83f0ee656dfc717284c1e26d10dcb814fe1c9e
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86209033"
---
# <a name="configure-minimum-required-version-of-transport-layer-security-tls-for-a-storage-account"></a>Konfigurieren der erforderlichen Mindestversion der Transport Layer Security (TLS) für ein Speicherkonto

Die Kommunikation zwischen einer Clientanwendung und einem Azure Storage-Konto wird mithilfe der Transport Layer Security (TLS) verschlüsselt. Die TLS ist ein kryptografisches Standardprotokoll, mit dem Datenschutz und Datenintegrität zwischen Clients und Diensten über das Internet sichergestellt werden. Weitere Informationen zur TLS finden Sie unter [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security).

Azure Storage unterstützt derzeit drei Versionen des TLS-Protokolls: 1.0, 1.1 und 1.2. TLS 1.2 ist die sicherste TLS-Version. In Azure Storage wird TLS 1.2 für öffentliche HTTPS-Endpunkte verwendet, aber TLS 1.0 und TLS 1.1 werden aus Gründen der Abwärtskompatibilität weiterhin unterstützt.

Standardmäßig ermöglichen Azure Storage-Konten Clients das Senden und Empfangen von Daten mit der ältesten TLS-Version, TLS 1.0, und höheren Versionen. Wenn Sie strengere Sicherheitsmaßnahmen durchsetzen möchten, können Sie Ihr Speicherkonto so konfigurieren, dass Clients Daten mit einer neueren TLS-Version senden und empfangen müssen. Wenn für ein Speicherkonto eine TLS-Mindestversion erforderlich ist, verursachen alle Anforderungen mit einer früheren Version einen Fehler.

In diesem Artikel wird die Konfiguration eines Speicherkontos beschrieben, mit der Clients ihre Anforderungen unter Verwendung einer TLS-Mindestversion senden müssen. Informationen zum Angeben einer bestimmten TLS-Version beim Senden einer Anforderung aus einer Clientanwendung finden Sie unter [Konfigurieren der Transport Layer Security (TLS) für eine Clientanwendung](transport-layer-security-configure-client-version.md).

## <a name="detect-the-tls-version-used-by-client-applications"></a>Ermitteln der von Clientanwendungen verwendeten TLS-Version

Wenn Sie für Ihr Speicherkonto eine TLS-Mindestversion erzwingen, werden Anforderungen von Clients, die Daten mit einer früheren TLS-Version senden, möglicherweise abgelehnt. Um die Auswirkungen der Konfiguration einer TLS-Mindestversion auf Clientanwendungen zu verstehen, empfiehlt Microsoft, die Protokollierung für Ihr Azure Storage-Konto zu aktivieren und die Protokolle nach einem bestimmten Zeitraum zu analysieren. So können Sie ermitteln, welche TLS-Versionen von Clientanwendungen verwendet werden.

Für die Protokollierung der Anforderungen an Ihr Azure Storage-Konto und die Ermittlung der vom Client verwendeten TLS-Version können Sie die Azure Storage-Protokollierung in Azure Monitor (Vorschau) verwenden. Weitere Informationen finden Sie unter [Überwachen von Azure Storage](monitor-storage.md).

Die Azure Storage-Protokollierung in Azure Monitor unterstützt die Verwendung von Protokollabfragen zum Analysieren von Protokolldaten. Für die Abfrage von Protokollen können Sie einen Azure Log Analytics-Arbeitsbereich verwenden. Weitere Informationen zu Protokollabfragen finden Sie unter [Tutorial: Erste Schritte mit Log Analytics-Abfragen](../../azure-monitor/log-query/get-started-portal.md).

Wenn Sie Azure Storage-Daten mit Azure Monitor protokollieren und mit Azure Log Analytics analysieren möchten, müssen Sie zunächst eine Diagnoseeinstellung erstellen, die angibt, welche Anforderungstypen und für welche Speicherdienste Daten protokolliert werden sollen. Führen Sie zum Erstellen einer Diagnoseeinstellung im Azure-Portal die folgenden Schritte aus:

1. Registrieren Sie sich für die [Azure Storage-Protokollierung in Azure Monitor (Vorschau)](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u).
1. Erstellen Sie einen neuen Log Analytics-Arbeitsbereich in dem Abonnement, das Ihr Azure Storage-Konto enthält. Nachdem Sie die Protokollierung für Ihr Speicherkonto konfiguriert haben, sind die Protokolle im Log Analytics-Arbeitsbereich verfügbar. Weitere Informationen finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](../../azure-monitor/learn/quick-create-workspace.md).
1. Navigieren Sie zum Speicherkonto im Azure-Portal.
1. Klicken Sie im Abschnitt „Überwachung“ auf **Diagnoseeinstellungen (Vorschau)** .
1. Wählen Sie den Azure Storage-Dienst aus, für den Sie Anforderungen protokollieren möchten. Wählen Sie beispielsweise **Blob** aus, um Anforderungen an den Blobspeicher zu protokollieren.
1. Klicken Sie auf **Diagnoseeinstellung hinzufügen**.
1. Geben Sie einen Namen für die Diagnoseeinstellung an.
1. Wählen Sie unter **Kategoriedetails** im Abschnitt **Protokoll** aus, welche Typen von Anforderungen protokolliert werden sollen. Sie können Lese-, Schreib- und Löschanforderungen protokollieren. Wenn Sie z. B. **StorageRead** und **StorageWrite** auswählen, werden Lese- und Schreibanforderungen an den ausgewählten Dienst protokolliert.
1. Wählen Sie unter **Zieldetails** die Option **An Log Analytics senden** aus. Wählen Sie Ihr Abonnement und den zuvor erstellten Log Analytics-Arbeitsbereich aus, wie in der folgenden Abbildung dargestellt.

    :::image type="content" source="media/transport-layer-security-configure-minimum-version/create-diagnostic-setting-logs.png" alt-text="Screenshot zur Erstellung einer Diagnoseeinstellung für die Protokollierung von Anforderungen":::

Nachdem Sie die Diagnoseeinstellung erstellt haben, werden Anforderungen an das Speicherkonto gemäß dieser Einstellung protokolliert. Weitere Informationen finden Sie unter [Erstellen einer Diagnoseeinstellung zum Sammeln von Ressourcenprotokollen und -metriken in Azure](../../azure-monitor/platform/diagnostic-settings.md).

Eine Referenz der Felder, die in Azure Storage-Protokollen in Azure Monitor verfügbar sind, finden Sie unter [Ressourcenprotokolle (Vorschau)](monitor-storage-reference.md#resource-logs-preview).

### <a name="query-logged-requests-by-tls-version"></a>Abfragen protokollierter Anforderungen nach TLS-Version

Azure Storage-Protokolle in Azure Monitor enthalten die TLS-Version, die zum Senden einer Anforderung an ein Speicherkonto verwendet wird. Mithilfe der Eigenschaft **TlsVersion** können Sie die TLS-Version einer protokollierten Anforderung überprüfen.

Wenn Sie Protokolle für die letzten 7 Tage abrufen und ermitteln möchten, wie viele Anforderungen für Blobspeicher anhand der einzelnen TLS-Versionen gesendet wurden, öffnen Sie Ihren Log Analytics-Arbeitsbereich. Fügen Sie dann die folgende Abfrage in eine neue Protokollabfrage ein, und führen Sie sie aus. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```kusto
StorageBlobLogs
| where TimeGenerated > ago(7d) and AccountName == "<account-name>"
| summarize count() by TlsVersion
```

Die Ergebnisse zeigen die Anzahl der Anforderungen, die über die einzelnen TLS-Versionen gesendet wurden:

:::image type="content" source="media/transport-layer-security-configure-minimum-version/log-analytics-query-version.png" alt-text="Screenshot der Ergebnisse der Log Analytics-Abfrage zur Rückgabe der TLS-Version":::

### <a name="query-logged-requests-by-caller-ip-address-and-user-agent-header"></a>Abfragen protokollierter Anforderungen nach IP-Adresse des Aufrufers und Benutzer-Agent-Header

Azure Storage-Protokolle in Azure Monitor enthalten auch die IP-Adresse des Aufrufers und den Benutzer-Agent-Header, damit Sie ermitteln können, welche Clientanwendungen auf das Speicherkonto zugegriffen haben. Durch eine Analyse dieser Werte können Sie entscheiden, ob Clientanwendungen auf die Verwendung einer neueren TLS-Version aktualisiert werden müssen oder ob eine Clientanforderung problemlos abgelehnt werden kann, wenn sie nicht mit der TLS-Mindestversion gesendet wird.

Fügen Sie die folgende Abfrage in eine neue Protokollabfrage ein, und führen Sie sie aus, um die Protokolle der letzten 7 Tage abzurufen und die Clients zu ermitteln, die Anforderungen mit einer früheren TLS-Version als TLS 1.2 gesendet haben. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```kusto
StorageBlobLogs
| where TimeGenerated > ago(7d) and AccountName == "<account-name>" and TlsVersion != "TLS 1.2"
| project TlsVersion, CallerIpAddress, UserAgentHeader
```

## <a name="configure-the-minimum-tls-version-for-an-account"></a>Konfigurieren der TLS-Mindestversion für ein Konto

Verwenden Sie das Azure-Portal oder die Azure CLI, um die TLS-Mindestversion für ein Speicherkonto zu konfigurieren und die Version **minimumTlsVersion** für das Konto festzulegen. Diese Eigenschaft ist für alle Speicherkonten verfügbar, die mit dem Azure Resource Manager-Bereitstellungsmodell erstellt wurden. Weitere Informationen finden Sie unter [Speicherkontoübersicht](storage-account-overview.md).

# <a name="portal"></a>[Portal](#tab/portal)

Führen Sie die folgenden Schritte aus, um die TLS-Mindestversion für ein Speicherkonto im Azure-Portal zu konfigurieren:

1. Navigieren Sie zum Speicherkonto im Azure-Portal.
1. Wählen Sie die Einstellung **Konfiguration** aus.
1. Wählen Sie unter **TLS-Mindestversion** in der Dropdownliste die erforderliche TLS-Mindestversion für den Zugriff auf Daten in diesem Speicherkonto aus, wie in der folgenden Abbildung dargestellt.

    :::image type="content" source="media/transport-layer-security-configure-minimum-version/configure-minimum-version-portal.png" alt-text="Screenshot zur Konfiguration der TLS-Mindestversion im Azure-Portal":::

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Wenn Sie die TLS-Mindestversion für ein Speicherkonto mit der Azure CLI konfigurieren möchten, rufen Sie zunächst die Ressourcen-ID für Ihr Speicherkonto über den Befehl [az resource show](/cli/azure/resource#az-resource-show) ab. Rufen Sie danach den Befehl [az resource update](/cli/azure/resource#az-resource-update) auf, um die Eigenschaft **minimumTlsVersion** für das Speicherkonto festzulegen. Gültige Werte für **minimumTlsVersion** sind `TLS1_0`, `TLS1_1` und `TLS1_2`.

Im folgenden Beispiel wird die TLS-Mindestversion auf 1.2 festgelegt. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
storage_account_id=$(az resource show \
  --name <storage-account> \
  --resource-group <resource-group> \
  --resource-type Microsoft.Storage/storageAccounts \
  --query id \
  --output tsv)

az resource update \
  --ids $storage_account_id \
  --set properties.minimumTlsVersion="TLS1_2"
```

---

> [!NOTE]
> Nachdem Sie die TLS-Mindestversion für das Speicherkonto aktualisiert haben, kann es bis zu 30 Sekunden dauern, bis die Änderung vollständig weitergegeben wurde.

## <a name="check-the-minimum-required-tls-version-for-an-account"></a>Überprüfen der erforderlichen TLS-Mindestversion für ein Konto

Wenn Sie die für ein Speicherkonto konfigurierte erforderliche TLS-Mindestversion ermitteln möchten, überprüfen Sie die Azure Resource Manager-Eigenschaft **minimumTlsVersion**. Über den Azure Resource Graph-Explorer können Sie diese Eigenschaft für eine große Anzahl von Speicherkonten gleichzeitig überprüfen.

Die Eigenschaft **minimumTlsVersion** wird nicht standardmäßig festgelegt und gibt erst dann einen Wert zurück, wenn Sie sie explizit festgelegt haben. Das Senden von Anforderungen mit TLS-Version 1.0 oder höher wird vom Speicherkonto standardmäßig gestattet, wenn der Eigenschaftswert NULL ist.

### <a name="check-the-minimum-required-tls-version-for-a-single-storage-account"></a>Überprüfen der erforderlichen TLS-Mindestversion für ein einzelnes Speicherkonto

Wenn Sie die erforderliche TLS-Mindestversion für ein einzelnes Speicherkonto mithilfe der Azure CLI überprüfen möchten, rufen Sie den Befehl [az resource show](/cli/azure/resource#az-resource-show) auf, und fragen Sie die Eigenschaft **minimumTlsVersion** ab:

```azurecli-interactive
az resource show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --resource-type Microsoft.Storage/storageAccounts \
    --query properties.minimumTlsVersion \
    --output tsv
```

### <a name="check-the-minimum-required-tls-version-for-a-set-of-storage-accounts"></a>Überprüfen der erforderlichen TLS-Mindestversion für eine Gruppe von Speicherkonten

Wenn Sie die erforderliche TLS-Mindestversion für eine Gruppe von Speicherkonten mit optimaler Leistung überprüfen möchten, können Sie den Azure Resource Graph-Explorer im Azure-Portal verwenden. Weitere Informationen zur Verwendung des Resource Graph-Explorers finden Sie unter [Schnellstart: Ausführen Ihrer ersten Resource Graph-Abfrage mithilfe des Azure Resource Graph-Explorers](/azure/governance/resource-graph/first-query-portal).

Wenn Sie die folgende Abfrage im Resource Graph-Explorer ausführen, wird eine Liste der Speicherkonten zurückgegeben und für jedes Konto die TLS-Mindestversion angezeigt:

```kusto
resources
| where type =~ 'Microsoft.Storage/storageAccounts'
| extend minimumTlsVersion = parse_json(properties).minimumTlsVersion
| project subscriptionId, resourceGroup, name, minimumTlsVersion
```

---

## <a name="test-the-minimum-tls-version-from-a-client"></a>Testen der TLS-Mindestversion auf einem Client

Wenn Sie testen möchten, ob mit einer früheren Version durchgeführte Aufrufe durch die erforderliche TLS-Mindestversion für ein Speicherkonto verhindert werden, können Sie einen Client für die Verwendung einer früheren TLS-Version konfigurieren. Weitere Informationen zum Konfigurieren eines Clients für die Verwendung einer bestimmten TLS-Version finden Sie unter [Konfigurieren der Transport Layer Security (TLS) für eine Clientanwendung](transport-layer-security-configure-client-version.md).

Wenn ein Client auf ein Speicherkonto über eine TLS-Version zugreift, die nicht der für das Konto konfigurierten TLS-Mindestversion entspricht, gibt Azure Storage einen Fehler mit dem Fehlercode 400 zurück (Ungültige Anforderung). Zudem wird in einer Meldung darauf hingewiesen, dass die verwendete TLS-Version für Anforderungen an dieses Speicherkonto nicht gestattet ist.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren der Transport Layer Security (TLS) für eine Clientanwendung](transport-layer-security-configure-client-version.md)
- [Sicherheitsempfehlungen für Blob Storage](../blobs/security-recommendations.md)
