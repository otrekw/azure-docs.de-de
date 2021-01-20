---
title: Verwenden von kundenseitig verwalteten Speicherkonten in Azure Monitor Log Analytics
description: Verwenden Sie Ihr eigenes Speicherkonto für Log Analytics-Szenarios.
ms.subservice: logs
ms.topic: conceptual
author: noakup
ms.author: noakuper
ms.date: 09/03/2020
ms.openlocfilehash: 706392d95e371fe303bb9f2c18f59e4a224d83c0
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2021
ms.locfileid: "98201058"
---
# <a name="using-customer-managed-storage-accounts-in-azure-monitor-log-analytics"></a>Verwenden von kundenseitig verwalteten Speicherkonten in Azure Monitor Log Analytics

Log Analytics verwendet Azure Storage für verschiedene Szenarien. Dies wird in der Regel automatisch verwaltet. In einigen Fällen ist jedoch erforderlich, dass Sie Ihr eigenes Speicherkonto bereitstellen und verwalten. Solche Konten werden auch als kundenseitig verwaltete Speicherkonten bezeichnet. In diesem Dokument werden die Verwendung von kundenseitig verwaltetem Speicher für WAD-/LAD-Protokolle, Private Link und die Verschlüsselung mit kundenseitig verwalteten Schlüsseln (Customer-Managed Keys, CMKs) behandelt. 

> [!NOTE]
> Es wird empfohlen, keine Abhängigkeiten von den Inhalten zu verwenden, die Log Analytics in den kundenseitig verwalteten Speicher hochlädt, da sich Formatierung und Inhalt ändern können.

## <a name="ingesting-azure-diagnostics-extension-logs-wadlad"></a>Erfassen von Azure-Diagnose-Erweiterungsprotokollen (WAD/LAD)
Die Erweiterungs-Agents für die Azure-Diagnose (auch WAD für Windows-Agents und LAD für Linux-Agents) erfassen verschiedene Betriebssystemprotokolle und speichern sie in einem kundenseitig verwalteten Speicherkonto. Anschließend können Sie diese Protokolle in Log Analytics importieren, um sie zu überprüfen und zu analysieren.
### <a name="how-to-collect-azure-diagnostics-extension-logs-from-your-storage-account"></a>Erfassen von Azure-Diagnose-Erweiterungsprotokollen in Ihrem Speicherkonto
Verbinden Sie das Speicherkonto als Speicherdatenquelle über das [Azure-Portal](./diagnostics-extension-logs.md#collect-logs-from-azure-storage) oder durch Aufrufen der [Storage Insights-API](/rest/api/loganalytics/storage%20insights/createorupdate) mit Ihrem Log Analytics-Arbeitsbereich.

Unterstützte Datentypen:
* syslog
* Windows-Ereignisse
* Service Fabric
* ETW-Ereignisse
* IIS-Protokolle

## <a name="using-private-links"></a>Verwenden von privaten Verbindungen
Kundenseitig verwaltete Speicherkonten werden zum Erfassen von benutzerdefinierten oder IIS-Protokollen verwendet, wenn private Verbindungen verwendet werden, um eine Verbindung mit Azure Monitor-Ressourcen herzustellen. Beim Erfassungsprozess dieser Datentypen werden Protokolle zunächst in ein zwischengeschaltetes Azure Storage-Konto hochgeladen und erst dann in einem Arbeitsbereich erfasst. 

### <a name="using-a-customer-managed-storage-account-over-a-private-link"></a>Verwenden eines kundenseitig verwalteten Speicherkontos über Private Link
#### <a name="workspace-requirements"></a>Arbeitsbereichsanforderungen
Beim Herstellen einer Verbindung mit Azure Monitor über eine private Verbindung können Log Analytics-Agents nur Protokolle an Arbeitsbereiche senden, auf die über eine private Verbindung zugegriffen werden kann. Aufgrund dieser Voraussetzung müssen Sie Folgendes ausführen:
* Konfigurieren eines Azure Monitor Private Link-Bereichsobjekts (Azure Monitor Private Link Scope, AMPLS)
* Herstellen einer Verbindung mit Ihren Arbeitsbereichen
* Verbinden Sie das AMPLS über eine private Verbindung mit Ihrem Netzwerk. 

Weitere Informationen zur AMPLS-Konfiguration finden Sie unter [Verwenden von Azure Private Link zum sicheren Verbinden von Netzwerken mit Azure Monitor](./private-link-security.md). 

#### <a name="storage-account-requirements"></a>Anforderungen an das Speicherkonto
Damit das Speicherkonto erfolgreich eine Verbindung mit Ihrer Private Link-Instanz herstellen kann, muss Folgendes der Fall sein:
* Das Speicherkonto muss sich in Ihrem VNet oder einem über Peering damit verbundenen Netzwerk befinden und über eine private Verbindung mit Ihrem VNet verbunden sein.
* Es muss sich in derselben Region befinden wie der Arbeitsbereich, mit dem es verknüpft ist.
* Es muss den Zugriff auf das Speicherkonto durch Azure Monitor zulassen. Wenn Sie nur ausgewählten Netzwerken den Zugriff auf Ihr Speicherkonto gestatten möchten, sollten Sie die folgende Ausnahme auswählen: „Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben“.
![Abbildung zu vertrauenswürdigen Microsoft-Diensten für ein Speicherkonto](./media/private-storage/storage-trust.png)
* Wenn Ihr Arbeitsbereich auch Datenverkehr aus anderen Netzwerken verarbeitet, sollten Sie das Speicherkonto so konfigurieren, dass eingehender Datenverkehr aus diesen Netzwerken/dem Internet zugelassen wird.

### <a name="using-a-customer-managed-storage-account-for-cmk-data-encryption"></a>Verwenden eines kundenseitig verwalteten Speicherkontos für die CMK-Datenverschlüsselung
Azure Storage verschlüsselt alle ruhenden Daten in einem Speicherkonto. Standardmäßig werden von Microsoft verwaltete Schlüssel (MMK) zum Verschlüsseln der Daten verwendet. Azure Storage erlaubt jedoch auch die Verwendung von CMK aus Azure Key Vault zum Verschlüsseln der Speicherdaten. Sie können entweder Ihre eigenen Schlüssel in Azure Key Vault importieren oder Schlüssel über die Azure Key Vault-APIs generieren.
#### <a name="cmk-scenarios-that-require-a-customer-managed-storage-account"></a>CMK-Szenarios, die einen kundenseitig verwalteten Speicher erfordern
* Verschlüsseln von Protokollwarnungsabfragen mit CMKs
* Verschlüsseln von gespeicherten Abfragen mit CMKs

#### <a name="how-to-apply-cmk-to-customer-managed-storage-accounts"></a>Anwenden von CMKs auf kundenseitig verwaltete Speicherkonten
##### <a name="storage-account-requirements"></a>Anforderungen an das Speicherkonto
Das Speicherkonto und der Schlüsseltresor müssen sich in derselben Region befinden, dürfen aber zu verschiedenen Abonnements gehören. Weitere Informationen zur Azure Storage-Verschlüsselung und -Schlüsselverwaltung finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](../../storage/common/storage-service-encryption.md).

##### <a name="apply-cmk-to-your-storage-accounts"></a>Anwenden von CMKs auf Ihre Speicherkonten
Verwenden Sie das [Azure-Portal](../../storage/common/customer-managed-keys-configure-key-vault.md?toc=%252fazure%252fstorage%252fblobs%252ftoc.json), [PowerShell](../../storage/common/customer-managed-keys-configure-key-vault.md?toc=%252fazure%252fstorage%252fblobs%252ftoc.json) oder die [CLI](../../storage/common/customer-managed-keys-configure-key-vault.md?toc=%252fazure%252fstorage%252fblobs%252ftoc.json), um Ihr Azure Storage-Konto für die Verwendung von CMK mit Azure Key Vault zu konfigurieren. 

## <a name="link-storage-accounts-to-your-log-analytics-workspace"></a>Verknüpfen von Speicherkonten mit dem Log Analytics-Arbeitsbereich
### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals
Öffnen Sie im Azure-Portal das Menü Ihres Arbeitsbereichs, und wählen Sie *Verknüpfte Speicherkonten* aus. Ein Blatt wird geöffnet, das die verknüpften Speicherkonten zu den oben erwähnten Anwendungsfällen anzeigt (Erfassung über Private Link, Anwenden von CMK auf gespeicherte Abfragen oder Warnungen).
![Abbildung des Blatts „Verknüpfte Speicherkonten“](./media/private-storage/all-linked-storage-accounts.png) Wenn Sie in der Tabelle einen Eintrag auswählen, werden die Details zum entsprechenden Speicherkonto angezeigt. Dort können Sie das verknüpfte Speicherkonto für diesen Typ festlegen oder aktualisieren. 
![Abbildung des Blatts zum Verknüpfen eines Speicherkontos](./media/private-storage/link-a-storage-account-blade.png) Sie können das gleiche Konto für verschiedene Anwendungsfälle verwenden.

### <a name="using-the-azure-cli-or-rest-api"></a>Verwenden der Azure CLI oder der REST-API
Sie können ein Speicherkonto auch über die [Azure CLI](/cli/azure/monitor/log-analytics/workspace/linked-storage) oder die [REST-API](/rest/api/loganalytics/linkedstorageaccounts) mit dem Arbeitsbereich verknüpfen.

Gültige Werte für dataSourceType:
* „CustomLogs“, um das Speicherkonto für die Erfassung von benutzerdefinierten Protokollen und IIS-Protokollen zu verwenden
* „Query“, um das Speicherkonto zum Speichern von gespeicherten Abfrage zu verwenden (für CMK-Verschlüsselung erforderlich)
* „Alerts“, um das Speicherkonto zum Speichern protokollbasierter Warnungen zu verwenden (für CMK-Verschlüsselung erforderlich)


## <a name="managing-linked-storage-accounts"></a>Verwalten von verknüpften Speicherkonten

### <a name="create-or-modify-a-link"></a>Erstellen oder Ändern einer Verknüpfung
Wenn Sie ein Speicherkonto mit einem Arbeitsbereich verknüpfen, verwendet Log Analytics ab dann dieses Speicherkonto anstelle des Speicherkontos des Diensts. Sie haben folgende Möglichkeiten: 
* Registrieren Sie mehrere Speicherkonten, um die Last durch Protokolle auf sie zu verteilen.
* Verwenden Sie dasselbe Speicherkonto für mehrere Arbeitsbereiche.

### <a name="unlink-a-storage-account"></a>Aufheben der Verknüpfung eines Speicherkontos
Wenn Sie ein Speicherkonto nicht mehr verwenden möchten, müssen Sie die Verknüpfung des Speichers mit dem Arbeitsbereich aufheben. Das Aufheben der Verknüpfung aller Speicherkonten mit einem Arbeitsbereich führt dazu, dass Log Analytics versucht, dienstseitig verwaltete Speicherkonten zu verwenden. Wenn Ihr Netzwerk nur eingeschränkt auf das Internet zugreifen kann, sind diese Speicher möglicherweise nicht verfügbar, und bei allen Szenarios, die Speicher benötigen, tritt ein Fehler auf.

### <a name="replace-a-storage-account"></a>Ersetzen eines Speicherkontos
So ersetzen Sie ein für die Erfassung verwendetes Speicherkonto:
1.  **Erstellen Sie eine Verknüpfung mit einem neuen Speicherkonto.** Die Protokollierungs-Agents rufen die aktualisierte Konfiguration ab und beginnen damit, Daten auch an den neuen Speicher zu senden. Dies kann einige Minuten dauern.
2.  **Heben Sie dann die Verknüpfung mit dem alten Speicherkonto auf, sodass die Agents nicht mehr in das entfernte Konto schreiben.** Der Erfassungsprozess liest weiterhin Daten aus diesem Konto, bis sämtliche Daten erfasst wurden. Löschen Sie das alte Speicherkonto erst, wenn Sie sicher sind, dass alle Protokolle erfasst wurden.

### <a name="maintaining-storage-accounts"></a>Verwalten von Speicherkonten
#### <a name="manage-log-retention"></a>Verwalten der Protokollaufbewahrung
Wenn Sie Ihr eigenes Speicherkonto verwenden, sind Sie für die Aufbewahrung verantwortlich. Log Analytics löscht keine Protokolle, die in Ihrem privaten Speicher gespeichert sind. Stattdessen sollten Sie eine Richtlinie einrichten, um die Last wie gewünscht zu verarbeiten.

#### <a name="consider-load"></a>Berücksichtigung der Last
Speicherkonten können eine bestimmte Last durch Lese- und Schreibanforderungen verarbeiten, bevor sie mit der Drosselung von Anforderungen beginnen. (Weitere Informationen finden Sie unter [Skalierbarkeits- und Leistungsziele für Standardspeicherkonten](../../storage/common/scalability-targets-standard-account.md).) Diese Drosselung wirkt sich auf die für die Erfassung von Protokollen benötigte Zeit aus. Wenn Ihr Speicherkonto überlastet ist, registrieren Sie ein zusätzliches Speicherkonto, um die Last auf die beiden Konten zu verteilen. Sehen Sie sich zur Überwachung der Kapazität und Leistung Ihres Speicherkontos die entsprechenden [Erkenntnisse im Azure-Portal]( https://docs.microsoft.com/azure/azure-monitor/insights/storage-insights-overview) an.

### <a name="related-charges"></a>Anfallende Gebühren
Die Gebühren für Speicherkonten werden anhand der Menge der gespeicherten Daten, des Speichertyps und des Redundanztyps berechnet. Details dazu finden Sie unter [Preise für Blockblobs](https://azure.microsoft.com/pricing/details/storage/blobs) und [Azure Table Storage – Preise](https://azure.microsoft.com/pricing/details/storage/tables).


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Verwenden von Azure Private Link zum sicheren Verbinden von Netzwerken mit Azure Monitor](private-link-security.md).
- Informieren Sie sich über [kundenseitig verwaltete Schlüssel in Azure Monitor](customer-managed-keys.md).
