---
title: Verwenden kundenseitig verwalteter Schlüssel mit Azure Key Vault für die Verwaltung der Kontoverschlüsselung
titleSuffix: Azure Storage
description: Sie können Ihren eigenen Verschlüsselungsschlüssel verwenden, um die Daten Ihres Speicherkontos zu schützen. Wenn Sie einen vom Kunden verwalteten Schlüssel angeben, wird dieser zum Schützen und Steuern des Zugriffs auf den Schlüssel verwendet, mit dem Ihre Daten verschlüsselt werden. Vom Kunden verwaltete Schlüssel ermöglichen eine höhere Flexibilität bei der Verwaltung von Zugriffssteuerungen.
services: storage
author: tamram
ms.service: storage
ms.date: 03/12/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 5dedd70b51361936808724ef70b96cdf9cfa13f5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85515403"
---
# <a name="use-customer-managed-keys-with-azure-key-vault-to-manage-azure-storage-encryption"></a>Verwenden kundenseitig verwalteter Schlüssel mit Azure Key Vault für die Verwaltung der Azure Storage-Verschlüsselung

Sie können Ihren eigenen Verschlüsselungsschlüssel verwenden, um die Daten Ihres Speicherkontos zu schützen. Wenn Sie einen vom Kunden verwalteten Schlüssel angeben, wird dieser zum Schützen und Steuern des Zugriffs auf den Schlüssel verwendet, mit dem Ihre Daten verschlüsselt werden. Vom Kunden verwaltete Schlüssel ermöglichen eine höhere Flexibilität bei der Verwaltung von Zugriffssteuerungen.

Sie müssen Azure Key Vault zum Speichern Ihrer vom Kunden verwalteten Schlüssel verwenden. Sie können entweder Ihre eigenen Schlüssel erstellen und in einem Schlüsseltresor speichern oder mit den Azure Key Vault-APIs Schlüssel generieren. Das Speicherkonto und der Schlüsseltresor müssen sich in derselben Region und in demselben Azure Active Directory (Azure AD)-Mandanten befinden, aber sie können sich in verschiedenen Abonnements befinden. Weitere Informationen zum Azure Key Vault finden Sie unter [What is Azure Key Vault? (Was ist der Azure Key Vault?)](../../key-vault/general/overview.md).

## <a name="about-customer-managed-keys"></a>Informationen zu kundenseitig verwalteten Schlüsseln

Im folgenden Diagramm ist dargestellt, wie Azure Active Directory und Azure Key Vault von Azure Storage verwendet werden, um Anforderungen mit dem vom Kunden verwalteten Schlüssel zu senden:

![Das Diagramm zeigt, wie vom Kunden verwaltete Schlüssel in Azure Storage funktionieren](media/encryption-customer-managed-keys/encryption-customer-managed-keys-diagram.png)

In der folgenden Liste werden die nummerierten Schritte im Diagramm erläutert:

1. Ein Azure Key Vault-Administrator erteilt Berechtigungen für Verschlüsselungsschlüssel an die dem Speicherkonto zugeordnete verwaltete Identität.
2. Ein Azure Storage-Administrator konfiguriert die Verschlüsselung mit einem vom Kunden verwalteten Schlüssel für das Speicherkonto.
3. Azure Storage verwendet die dem Speicherkonto zugeordnete verwaltete Identität, um den Zugriff auf Azure Key Vault über Azure Active Directory zu authentifizieren.
4. Azure Storage umschließt den Kontoverschlüsselungsschlüssel mit dem Kundenschlüssel in Azure Key Vault.
5. Bei Lese-/Schreibvorgängen sendet Azure Storage Anforderungen an Azure Key Vault, um die Umschließung für den Kontoverschlüsselungsschlüssel aufzuheben und so Verschlüsselungs- und Entschlüsselungsvorgänge durchzuführen.

## <a name="create-an-account-that-supports-customer-managed-keys-for-queues-and-tables"></a>Erstellen eines Kontos, das kundenseitig verwaltete Schlüssel für Warteschlangen und Tabellen unterstützt

In den Warteschlangen- und Tabellenspeicherdiensten gespeicherte Daten sind nicht automatisch durch einen vom Kunden verwalteten Schlüssel geschützt, wenn Schlüssel dieser Art für das Speicherkonto aktiviert sind. Sie können diese Dienste optional beim Erstellen des Speicherkontos konfigurieren, um sie in diese Schutzmaßnahme einzubeziehen.

Weitere Informationen zur Erstellung eines Speicherkontos, für das vom Kunden verwaltete Schlüssel für Warteschlangen und Tabellen unterstützt werden, finden Sie unter [Erstellen eines Kontos, das kundenseitig verwaltete Schlüssel für Tabellen und Warteschlangen unterstützt](account-encryption-key-create.md).

Daten in den Blob- und Dateidiensten sind immer durch vom Kunden verwaltete Schlüssel geschützt, wenn diese für das Speicherkonto konfiguriert werden.

## <a name="enable-customer-managed-keys-for-a-storage-account"></a>Aktivieren von kundenseitig verwalteten Schlüsseln für ein Speicherkonto

Vom Kunden verwaltete Schlüssel können nur für vorhandene Speicherkonten aktiviert werden. Für den Schlüsseltresor müssen Zugriffsrichtlinien bereitgestellt werden, mit denen Schlüsselberechtigungen für die verwaltete Identität erteilt werden, die dem Speicherkonto zugeordnet ist. Die verwaltete Identität ist erst verfügbar, nachdem das Speicherkonto erstellt wurde.

Wenn Sie einen vom Kunden verwalteten Schlüssel konfigurieren, umschließt Azure Storage den Stammdaten-Verschlüsselungsschlüssel für das Konto im zugeordneten Schlüsseltresor mit dem vom Kunden verwalteten Schlüssel. Die Aktivierung der vom Kunden verwalteten Schlüssel hat keine Auswirkung auf die Leistung und ist sofort wirksam.

Wenn Sie den für die Azure Storage-Verschlüsselung verwendeten Schlüssel ändern, indem Sie kundenseitig verwaltete Schlüssel aktivieren oder deaktivieren, die Schlüsselversion aktualisieren oder einen anderen Schlüssel angeben, wird die Verschlüsselung des Stammschlüssels zwar geändert, aber die Daten in Ihrem Azure Storage-Konto müssen nicht erneut verschlüsselt werden.

Wenn Sie vom Kunden verwaltete Schlüssel aktivieren oder deaktivieren oder den Schlüssel bzw. die Schlüsselversion ändern, ändert sich der Schutz des Stammverschlüsselungsschlüssels. Die Daten in Ihrem Azure Storage-Konto müssen aber nicht neu verschlüsselt werden.

Informationen zum Verwenden von kundenseitig verwalteten Schlüsseln mit Azure Key Vault für die Azure Storage-Verschlüsselung finden Sie in folgenden Artikeln:

- [Konfigurieren von kundenseitig verwalteten Schlüsseln mit Key Vault für die Azure Storage-Verschlüsselung über das Azure-Portal](storage-encryption-keys-portal.md)
- [Konfigurieren von kundenseitig verwalteten Schlüsseln mit Key Vault für die Azure Storage-Verschlüsselung mithilfe von PowerShell](storage-encryption-keys-powershell.md)
- [Konfigurieren von kundenseitig verwalteten Schlüsseln mit Key Vault für die Azure Storage-Verschlüsselung mithilfe der Azure-Befehlszeilenschnittstelle](storage-encryption-keys-cli.md)

> [!IMPORTANT]
> Kundenseitig verwaltete Schlüssel basieren auf verwalteten Identitäten für Azure-Ressourcen, einem Feature von Azure AD. Verwaltete Identitäten unterstützen derzeit keine verzeichnisübergreifenden Szenarien. Wenn Sie vom Kunden verwaltete Schlüssel im Azure-Portal konfigurieren, wird Ihrem Speicherkonto eine verwaltete Identität im Hintergrund automatisch zugewiesen. Wenn Sie anschließend das Abonnement, die Ressourcengruppe oder das Speicherkonto von einem Azure AD-Verzeichnis in ein anderes Verzeichnis verschieben, wird die dem Speicherkonto zugeordnete verwaltete Identität nicht an den neuen Mandanten übertragen, sodass vom Kunden verwaltete Schlüssel möglicherweise nicht mehr funktionieren. Weitere Informationen finden Sie in [Häufig gestellte Fragen und bekannte Probleme mit verwalteten Identitäten für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories) unter **Übertragen eines Abonnements zwischen Azure AD-Verzeichnissen**.  

## <a name="store-customer-managed-keys-in-azure-key-vault"></a>Speichern von kundenseitig verwalteten Schlüsseln in Azure Key Vault

Zum Aktivieren von kundenseitig verwalteten Schlüsseln in einem Speicherkonto müssen Sie die Schlüssel in einer Azure Key Vault-Instanz speichern. Sie müssen die Eigenschaften **Vorläufiges Löschen** und **Do Not Purge** (Nicht bereinigen) im Schlüsseltresor aktivieren.

Die Azure Storage-Verschlüsselung unterstützt RSA- und RSA-HSM-Schlüssel mit einer Größe von 2.048, 3.072 und 4.096 Bit. Weitere Informationen zu Schlüsseln finden Sie unter **Key Vault-Schlüssel** in [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten in Azure Key Vault](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys).

## <a name="rotate-customer-managed-keys"></a>Rotieren von kundenseitig verwalteten Schlüsseln

Sie können einen vom Kunden verwalteten Schlüssel in Azure Key Vault entsprechend Ihren Konformitätsrichtlinien rotieren. Wenn der Schlüssel rotiert wird, müssen Sie das Speicherkonto so aktualisieren, dass der neue URI für die Schlüsselversion verwendet wird. Informationen dazu, wie Sie das Speicherkonto im Azure-Portal zur Verwendung einer neuen Version des Schlüssels aktualisieren, finden Sie im Abschnitt **Aktualisieren der Schlüsselversion** unter [Konfigurieren von kundenseitig verwalteten Schlüsseln für Azure Storage über das Azure-Portal](storage-encryption-keys-portal.md).

Durch Rotieren des Schlüssels werden die Daten im Speicherkonto nicht erneut verschlüsselt. Es ist keine weitere Aktion durch den Benutzer erforderlich.

## <a name="revoke-access-to-customer-managed-keys"></a>Widerrufen des Zugriffs auf von Kunden verwaltete Schlüssel

Sie können den Zugriff des Speicherkontos auf den vom Kunden verwalteten Schlüssel jederzeit widerrufen. Nachdem der Zugriff auf die vom Kunden verwalteten Schlüssel widerrufen wurde oder der Schlüssel deaktiviert oder gelöscht wurde, können Clients keine Vorgänge mehr aufrufen, bei denen für ein Blob oder die zugehörigen Metadaten Lese- oder Schreibvorgänge durchgeführt werden. Wenn versucht wird, die folgenden Vorgänge aufzurufen, tritt für alle Benutzer ein Fehler mit dem Code „403 (Verboten)“ auf:

- [List Blobs](/rest/api/storageservices/list-blobs), wenn der Aufruf mit dem Parameter `include=metadata` für den Anforderungs-URI erfolgt
- [Get Blob](/rest/api/storageservices/get-blob)
- [Get Blob Properties](/rest/api/storageservices/get-blob-properties)
- [Get Blob Metadata](/rest/api/storageservices/get-blob-metadata)
- [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata)
- [Snapshot Blob](/rest/api/storageservices/snapshot-blob), wenn der Aufruf mit dem Anforderungsheader `x-ms-meta-name` erfolgt
- [Copy Blob](/rest/api/storageservices/copy-blob)
- [Copy Blob From URL](/rest/api/storageservices/copy-blob-from-url)
- [Set Blob Tier](/rest/api/storageservices/set-blob-tier)
- [Put Block](/rest/api/storageservices/put-block)
- [Put Block From URL](/rest/api/storageservices/put-block-from-url)
- [Append Block](/rest/api/storageservices/append-block)
- [Append Block From URL](/rest/api/storageservices/append-block-from-url)
- [Put Blob](/rest/api/storageservices/put-blob)
- [Put Page](/rest/api/storageservices/put-page)
- [Put Page From URL](/rest/api/storageservices/put-page-from-url)
- [Incremental Copy Blob](/rest/api/storageservices/incremental-copy-blob)

Stellen Sie den Zugriff auf den vom Kunden verwalteten Schlüssel wieder her, um diese Vorgänge erneut aufzurufen.

Alle Datenvorgänge, die in diesem Abschnitt nicht aufgeführt sind, können durchgeführt werden, nachdem vom Kunden verwaltete Schlüssel widerrufen wurden oder ein Schlüssel deaktiviert oder gelöscht wurde.

Zum Widerrufen des Zugriffs auf von Kunden verwaltete Schlüssel können Sie [PowerShell](storage-encryption-keys-powershell.md#revoke-customer-managed-keys) oder die [Azure-Befehlszeilenschnittstelle](storage-encryption-keys-cli.md#revoke-customer-managed-keys) verwenden.

## <a name="customer-managed-keys-for-azure-managed-disks"></a>Von Kunden verwaltete Schlüssel für verwaltete Azure-Datenträger

Mit von Kunden verwalteten Schlüsseln kann auch die Verschlüsselung von verwalteten Azure-Datenträgern verwaltet werden. Das Verhalten der von Kunden verwalteten Schlüssel unterscheidet sich für verwaltete Datenträger von dem für Azure Storage-Ressourcen. Weitere Informationen finden Sie unter [Serverseitige Verschlüsselung von Azure Managed Disks (Windows)](../../virtual-machines/windows/disk-encryption.md) bzw. [Serverseitige Verschlüsselung von Azure Managed Disks (Linux)](../../virtual-machines/linux/disk-encryption.md).

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von kundenseitig verwalteten Schlüsseln mit Key Vault für die Azure Storage-Verschlüsselung über das Azure-Portal](storage-encryption-keys-portal.md)
- [Konfigurieren von kundenseitig verwalteten Schlüsseln mit Key Vault für die Azure Storage-Verschlüsselung mithilfe von PowerShell](storage-encryption-keys-powershell.md)
- [Konfigurieren von kundenseitig verwalteten Schlüsseln mit Key Vault für die Azure Storage-Verschlüsselung mithilfe der Azure-Befehlszeilenschnittstelle](storage-encryption-keys-cli.md)
- [Azure Storage encryption for data at rest (Azure Storage-Verschlüsselung für ruhende Daten)](storage-service-encryption.md)
