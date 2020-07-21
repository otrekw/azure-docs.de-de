---
title: Verschlüsselung in Azure Backup
description: Erfahren Sie mehr über die Verschlüsselungsfunktionen in Azure Backup, die Sie dabei unterstützen, Ihre Sicherungsdaten zu schützen und die Sicherheitsanforderungen Ihres Unternehmens zu erfüllen.
ms.topic: conceptual
ms.date: 04/30/2020
ms.openlocfilehash: aafb9868dfb6a63ec9b6a3ae654b88b202a1a145
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171821"
---
# <a name="encryption-in-azure-backup"></a>Verschlüsselung in Azure Backup

Alle Ihre gesicherten Daten werden automatisch verschlüsselt, wenn sie in der Cloud mit Azure Storage-Verschlüsselung gespeichert werden. Dies hilft Ihnen, Ihre Sicherheits- und Complianceverpflichtungen zu erfüllen. Ruhende Daten werden mit der AES-256-Verschlüsselung – einer der stärksten verfügbaren Blockchiffren – verschlüsselt und sind mit dem FIPS 140-2-Standard konform.

Zusätzlich zur Verschlüsselung ruhender Daten werden alle Ihre Sicherungsdaten während der Übertragung über HTTPS übertragen. Sie bleiben immer im Azure-Backbone-Netzwerk.

Weitere Informationen finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](https://docs.microsoft.com/azure/storage/common/storage-service-encryption). Antworten auf Fragen zur Verschlüsselung können Sie in den [häufig gestellten Fragen zu Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq#encryption) finden.

## <a name="encryption-of-backup-data-using-platform-managed-keys"></a>Verschlüsselung von Sicherungsdaten mit von der Plattform verwalteten Schlüsseln

Standardmäßig werden alle Ihre Daten mit von der Plattform verwalteten Schlüsseln verschlüsselt. Sie brauch von Ihrer Seite aus keine expliziten Maßnahmen ergreifen, um diese Verschlüsselung zu aktivieren, und sie gilt für alle Workloads, die in Ihrem Recovery Services-Tresor gesichert werden.

## <a name="encryption-of-backup-data-using-customer-managed-keys"></a>Verschlüsselung von Sicherungsdaten mit von Kunden verwalteten Schlüsseln

Wenn Sie Ihre Azure-VMs sichern, können Sie Ihre Daten jetzt mithilfe von Schlüsseln verschlüsseln, die Sie besitzen und die von Ihnen verwaltet werden. Mit Azure Backup können Sie die in Azure Key Vault gespeicherten RSA-Schlüssel zum Verschlüsseln Ihrer Sicherungen verwenden. Der Verschlüsselungsschlüssel, der zum Verschlüsseln von Sicherungen verwendet wird, kann sich von dem für die Quelle verwendeten Verschlüsselungsschlüssel unterscheiden. Die Daten werden mithilfe eines AES-256 basierten Datenverschlüsselungsschlüssels (DEK) geschützt, der wiederum mit Ihren Schlüsseln geschützt wird. Dadurch erhalten Sie vollständige Kontrolle über die Daten und Schlüssel. Um die Verschlüsselung zu ermöglichen, muss dem Recovery Services-Tresor der Zugriff auf den Verschlüsselungsschlüssel in Azure Key Vault gewährt werden. Bei Bedarf können Sie den Schlüssel deaktivieren oder den Zugriff widerrufen. Sie müssen jedoch die Verschlüsselung mit ihren Schlüsseln aktivieren, bevor Sie versuchen, Elemente im Tresor zu schützen.

Weitere Informationen zum Verschlüsseln Ihrer Sicherungsdaten mit kundenseitig verwalteten Schlüsseln finden Sie [hier](encryption-at-rest-with-cmk.md).

## <a name="backup-of-managed-disk-vms-encrypted-using-customer-managed-keys"></a>Sichern von VMs mit verwalteten Datenträgern, die mit vom Kunden verwalteten Schlüsseln verschlüsselt wurden

Mit Azure Backup können Sie außerdem Ihre virtuellen Azure-Computer sichern, die Ihren Schlüssel für die [Speicherdienstverschlüsselung](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) verwenden. Der zum Verschlüsseln der Datenträger verwendete Schlüssel wird in Azure Key Vault gespeichert und von Ihnen verwaltet. Die Speicherdienstverschlüsselung (Storage Service Encryption, SSE) mit kundenseitig verwalteten Schlüsseln unterscheidet sich von Azure Disk Encryption (ADE), da ADE BitLocker (für Windows) bzw. DM-Crypt (für Linux) verwendet, um die Verschlüsselung auf Gastsystemen durchzuführen. Von der SSE werden die Daten dagegen im Speicherdienst verschlüsselt, sodass Sie jedes beliebige Betriebssystem oder Image für Ihren virtuellen Computer verwenden können. Weitere Informationen finden Sie unter [Verschlüsselung verwalteter Datenträger mit vom Kunden verwalteten Schlüsseln](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption#customer-managed-keys).

## <a name="infrastructure-level-encryption-for-backup-data"></a>Verschlüsselung auf Infrastrukturebene für Sicherungsdaten

Neben der Verschlüsselung Ihrer Daten im Recovery Services-Tresor unter Verwendung kundenseitig verwalteter Schlüssel können Sie auch eine zusätzliche, für die Speicherinfrastruktur konfigurierte Verschlüsselungsebene verwenden. Diese Infrastrukturverschlüsselung wird von der Plattform verwaltet und ermöglicht in Kombination mit der Verschlüsselung ruhender Daten unter Verwendung kundenseitig verwalteter Schlüssel eine zweischichtige Verschlüsselung Ihrer Sicherungsdaten. Hinweis: Die Infrastrukturverschlüsselung kann nur konfiguriert werden, wenn Sie sich zuvor dafür entscheiden, Ihre eigenen Schlüssel für die Verschlüsselung ruhender Daten zu verwenden. Bei der Infrastrukturverschlüsselung werden plattformseitig verwaltete Schlüssel für die Datenverschlüsselung verwendet.

>[!NOTE]
>Die Infrastrukturverschlüsselung ist derzeit als eingeschränkte Vorschau in den Regionen „USA, Osten“, „USA, Westen 2“, „USA, Süden-Mitte“, „US Gov Arizona“ und „US Gov Virginia“ verfügbar. Wenn Sie das Feature in einer dieser Regionen verwenden möchten, füllen Sie [dieses Formular](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapUN0VHNEpJS0ZUWklUNVdJSTEzR0hIOVRMVC4u) aus, und senden Sie es per E-Mail an [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com).

## <a name="backup-of-vms-encrypted-using-ade"></a>Sichern von mit ADE verschlüsselten VMs

Mit Azure Backup können Sie auch Ihre virtuellen Azure-Computer sichern, deren Datenträger für Betriebssystem oder Daten mithilfe von Azure Disk Encryption verschlüsselt werden. ADE verwendet BitLocker für Windows-VMs bzw. DM-Crypt für Linux-VMs, um Verschlüsselung auf Gastsystemen durchzuführen. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen verschlüsselter virtueller Computer mit Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption).

## <a name="next-steps"></a>Nächste Schritte

- [Sichern und Wiederherstellen eines verschlüsselten virtuellen Azure-Computers](backup-azure-vms-encryption.md)
