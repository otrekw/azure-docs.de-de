---
title: Konfigurieren von Azure Defender für Storage
titleSuffix: Azure Storage
description: Konfigurieren Sie Azure Defender für Storage, um Anomalien in Bezug auf die Kontoaktivität zu erkennen und Benachrichtigungen über potenziell schädliche Zugriffsversuche auf Ihr Konto zu erhalten.
services: storage
author: tamram
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.date: 03/30/2021
ms.author: tamram
ms.reviewer: ozgun
ms.openlocfilehash: e2f044ab267365885260b031638572846184bc83
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106063184"
---
# <a name="configure-azure-defender-for-storage"></a>Konfigurieren von Azure Defender für Storage

Azure Defender für Storage ermöglicht die Nutzung intelligenter Sicherheitsfunktionen zur Erkennung von ungewöhnlichen und möglicherweise schädlichen Versuchen, auf Speicherkonten zuzugreifen oder diese unbefugt zu nutzen. Aufgrund dieser Schutzebene können Sie Bedrohungen begegnen, ohne ein Sicherheitsexperte zu sein oder Systeme für die Überwachung der Sicherheit verwalten zu müssen.

Bei Anomalien im Rahmen von Aktivitäten werden Sicherheitswarnungen ausgelöst. Diese Sicherheitswarnungen sind in [Azure Security Center](https://azure.microsoft.com/services/security-center/) integriert und werden mit Informationen zu verdächtigen Aktivitäten und Empfehlungen zur Untersuchung und Beseitigung von Bedrohungen auch per E-Mail an Abonnementadministratoren gesendet.

Der Dienst erfasst Ressourcenprotokolle von an Blob Storage und Azure Files gesendete Lese-, Schreib- und Löschanforderungen, um Bedrohungen zu erkennen. Wenn Sie Warnungen von Azure Defender untersuchen möchten, können Sie mithilfe von Storage Analytics Logging entsprechende Speicheraktivitäten anzeigen. Weitere Informationen finden Sie unter **Konfigurieren der Protokollierung** in [Überwachen eines Speicherkontos im Azure-Portal](./manage-storage-analytics-logs.md#configure-logging).

## <a name="availability"></a>Verfügbarkeit

Azure Defender für Storage ist derzeit für Blob Storage, Azure Files und Azure Data Lake Storage Gen2 verfügbar. Zu den Kontotypen, die Azure Defender unterstützen, gehören Konten vom Typ „Allgemein v2“ sowie Blockblob- und Blob Storage-Konten. Azure Defender für Storage ist in allen öffentlichen Clouds und Clouds der US-Regierungsbehörden verfügbar, nicht aber in anderen Sovereign Cloud- oder Azure Government-Cloudregionen.

Konten mit für Data Lake Storage aktivierten hierarchischen Namespaces unterstützen Transaktionen, die sowohl die Azure Blob Storage-APIs als auch die Data Lake Storage-APIs verwenden. Azure-Dateifreigaben unterstützen Transaktionen über SMB.

Preisdetails, einschließlich einer kostenlosen 30-Tage-Testversion, finden Sie auf der [Seite mit der Preisübersicht zu Azure Security Center](https://azure.microsoft.com/pricing/details/security-center/).

Die folgende Liste biete eine Übersicht über die Verfügbarkeit von Azure Defender für Storage:

- Status des Release:
  - [Blob Storage](https://azure.microsoft.com/services/storage/blobs/) (allgemeine Verfügbarkeit)
  - [Azure Files](../files/storage-files-introduction.md) (allgemeine Verfügbarkeit)
  - Azure Data Lake Storage Gen2 (allgemeine Verfügbarkeit)
- Clouds:<br>
    ✔ Kommerzielle Clouds<br>
    ✔ US Gov<br>
    ✘ China Gov, andere Gov

## <a name="set-up-azure-defender"></a>Einrichten von Azure Defender

Sie können Azure Defender für Storage auf verschiedene Arten konfigurieren, die in den folgenden Abschnitten beschrieben werden.

### <a name="azure-security-center"></a>[Azure Security Center](#tab/azure-security-center)

Azure Defender ist in Azure Security Center integriert. Wenn Sie Azure Defender für Ihr Abonnement aktivieren, wird Azure Defender für Azure Storage automatisch für alle Ihre Speicherkonten aktiviert. Sie können Azure Defender für Ihre Speicherkonten in einem bestimmten Abonnement folgendermaßen aktivieren oder deaktivieren:

1. Starten Sie **Azure Security Center** im [Azure-Portal](https://portal.azure.com).
1. Wählen Sie im Hauptmenü unter **Verwaltung** die Option **Preise und Einstellungen** aus.
1. Wählen Sie das Abonnement aus, für das Sie Azure Defender aktivieren bzw. deaktivieren möchten.
1. Wählen Sie **Azure Defender ein** aus, um Azure Defender für das Abonnement zu aktivieren.
1. Suchen Sie unter **Azure Defender-Plan nach Ressourcentyp auswählen** nach der Zeile **Storage**, und wählen Sie in der Spalte **Plan** die Option **Aktiviert** aus.
1. Speichern Sie die Änderungen.

    :::image type="content" source="media/azure-defender-storage-configure/enable-azure-defender-security-center.png" alt-text="Screenshot: Aktivieren von Azure Defender für Storage in Security Center":::

Azure Defender ist nun für alle Speicherkonten in diesem Abonnement aktiviert.

### <a name="portal"></a>[Portal](#tab/azure-portal)

1. Starten Sie das [Azure-Portal](https://portal.azure.com/).
1. Navigieren Sie zum Speicherkonto. Wählen Sie unter **Einstellungen** die Option **Erweiterte Sicherheit** aus.
1. Wählen Sie **Azure Defender für Storage aktivieren** aus.

    :::image type="content" source="media/azure-defender-storage-configure/enable-azure-defender-portal.png" alt-text="Screenshot: Aktivieren von Azure Defender für ein Azure Storage-Konto":::

Azure Defender ist nun für dieses Speicherkonto aktiviert.

### <a name="template"></a>[Vorlage](#tab/template)

Verwenden Sie zur Bereitstellung eines Azure Storage-Kontos, bei dem Azure Defender aktiviert ist, eine Azure Resource Manager-Vorlage. Weitere Informationen finden Sie unter [Storage-Konto mit Advanced Threat Protection](https://azure.microsoft.com/resources/templates/201-storage-advanced-threat-protection-create/).

### <a name="azure-policy"></a>[Azure Policy](#tab/azure-policy)

Verwenden Sie eine Azure Policy-Instanz zum Aktivieren von Azure Defender für Speicherkonten unter einem bestimmten Abonnement oder einer bestimmten Ressourcengruppe.

1. Starten Sie die Azure-Seite **Richtlinie – Definitionen**.
1. Suchen Sie nach der Richtlinie **Azure Defender für Speicherkonten bereitstellen**.

    :::image type="content" source="media/azure-defender-storage-configure/storage-atp-policy-definitions.png" alt-text="Anwenden einer Richtlinie zum Aktivieren von Azure Defender für Speicherkonten":::

1. Wählen Sie ein Azure-Abonnement oder eine Ressourcengruppe aus.

    :::image type="content" source="media/azure-defender-storage-configure/storage-atp-policy2.png" alt-text="Auswählen eines Abonnements oder einer Ressourcengruppe für den Umfang der Richtlinie":::

1. Weisen Sie die Richtlinie zu.

    :::image type="content" source="media/azure-defender-storage-configure/storage-atp-policy1.png" alt-text="Zuweisen der Richtlinie zum Aktivieren von Azure Defender für Storage":::

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Um Azure Defender für ein Speicherkonto mit PowerShell zu aktivieren, stellen Sie zunächst sicher, dass Sie das Modul [Az.Security](https://www.powershellgallery.com/packages/Az.Security) installiert haben. Rufen Sie dann den Befehl [Enable-AzSecurityAdvancedThreatProtection](/powershell/module/az.security/enable-azsecurityadvancedthreatprotection) auf. Denken Sie daran, die Werte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```azurepowershell
Enable-AzSecurityAdvancedThreatProtection -ResourceId "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/"
```

Um die Azure Defender-Einstellung für ein Speicherkonto mit PowerShell zu überprüfen, rufen Sie den Befehl [Get-AzSecurityAdvancedThreatProtection](/powershell/module/az.security/get-azsecurityadvancedthreatprotection) auf. Denken Sie daran, die Werte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```azurepowershell
Get-AzSecurityAdvancedThreatProtection -ResourceId "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/"
```

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Um Azure Defender für ein Speicherkonto mit Azure CLI zu aktivieren, rufen Sie den Befehl [az security atp storage update](/cli/azure/security/atp/storage#az_security_atp_storage_update) auf. Denken Sie daran, die Werte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli
az security atp storage update \
    --resource-group <resource-group> \
    --storage-account <storage-account> \
    --is-enabled true
```

Um die Azure Defender-Einstellung für ein Speicherkonto mit der Azure CLI zu überprüfen, rufen Sie den Befehl [az security atp storage show](/cli/azure/security/atp/storage#az_security_atp_storage_show) auf. Denken Sie daran, die Werte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli
az security atp storage show \
    --resource-group <resource-group> \
    --storage-account <storage-account>
```

---

## <a name="explore-security-anomalies"></a>Untersuchen von sicherheitsrelevanten Anomalien

Wenn für die Speicheraktivität Anomalien auftreten, erhalten Sie eine E-Mail-Benachrichtigung mit Informationen zum verdächtigen Sicherheitsereignis. Einzelheiten des Ereignisses sind:

- Art der Anomalie
- Speicherkontoname
- Ereigniszeit
- Speichertyp
- Mögliche Ursachen
- Untersuchungsschritte
- Schritte zur Bereinigung

Die E-Mail enthält auch Details zu möglichen Ursachen und empfohlenen Aktionen zur Untersuchung und Eindämmung der potenziellen Bedrohung.

:::image type="content" source="media/azure-defender-storage-configure/storage-advanced-threat-protection-alert-email.png" alt-text="E-Mail mit Warnung zu Azure Defender für Storage":::

Sie können Ihre aktuellen Sicherheitswarnungen in Azure Security Center über die [Kachel Sicherheitswarnungen](../../security-center/security-center-managing-and-responding-alerts.md) anzeigen und verwalten. Durch Klicken auf eine bestimmte Warnung werden Details und Aktionen zum Untersuchen der aktuellen Bedrohung und zum Begegnen zukünftiger Bedrohungen bereitgestellt.

:::image type="content" source="media/azure-defender-storage-configure/storage-advanced-threat-protection-alert.png" alt-text="Warnung zu Azure Defender für Storage":::

## <a name="security-alerts"></a>Sicherheitswarnungen

Warnungen werden bei ungewöhnlichen und potenziell schädlichen Zugriffsversuchen oder Exploit-Vorgängen für Speicherkonten generiert. Eine Liste mit Warnungen für Azure Storage finden Sie unter [Warnungen für Azure Storage](../../security-center/alerts-reference.md#alerts-azurestorage).

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Defender für Storage](../../security-center/defender-for-storage-introduction.md)
- [Azure Security Center](../../security-center/security-center-introduction.md)
- [Protokolle in Azure Storage-Konten](/rest/api/storageservices/About-Storage-Analytics-Logging)
