---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/02/2021
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 52b9bee1d43c0f136889a6a54277d4bb45dd4a45
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101750721"
---
Sie können die Verschlüsselung auf der Ebene verwalteter Datenträger mit eigenen Schlüsseln verwalten. Die serverseitige Verschlüsselung für verwaltete Datenträger mit vom Kunden verwalteten Schlüsseln bietet eine integrierte Benutzerfunktionalität mit Azure Key Vault. Sie können entweder [ihre RSA-Schlüssel](../articles/key-vault/keys/hsm-protected-keys.md) in den Schlüsseltresor importieren oder neue RSA-Schlüssel in Azure Key Vault generieren. 

Die Verschlüsselung und Entschlüsselung von verwalteten Azure-Datenträgern erfolgt durch [Umschlagverschlüsselung](../articles/storage/common/storage-client-side-encryption.md#encryption-and-decryption-via-the-envelope-technique) vollständig transparent. Daten werden mithilfe eines [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)-256 basierten Datenverschlüsselungsschlüssels (DEK) verschlüsselt, der wiederum mit Ihren Schlüsseln geschützt wird. Der Speicherdienst generiert Datenverschlüsselungsschlüssel und verschlüsselt sie mit vom Kunden verwalteten Schlüsseln unter Verwendung der RSA-Verschlüsselung. Mithilfe der Umschlagverschlüsselung können Sie Ihre Schlüssel gemäß Ihren Kompatibilitätsrichtlinien regelmäßig rotieren (ändern), ohne Ihre VMs zu beeinträchtigen. Wenn Sie Ihre Schlüssel rotieren, verschlüsselt der Speicherdienst die Datenverschlüsselungsschlüssel mit den neuen, vom Kunden verwalteten Schlüsseln neu. 

#### <a name="full-control-of-your-keys"></a>Vollständige Kontrolle über Ihre Schlüssel

Sie müssen den Zugriff auf verwaltete Datenträger in Ihrer Key Vault-Instanz gewähren, damit Sie Ihre Schlüssel zum Verschlüsseln und Entschlüsseln des DEK verwenden können. Dies ermöglicht eine umfassende Kontrolle über Ihre Daten und Schlüssel. Sie können Ihre Schlüssel jederzeit deaktivieren oder den Zugriff auf verwaltete Datenträger widerrufen. Sie können auch die Verwendung von Verschlüsselungsschlüsseln mithilfe der Azure Key Vault-Überwachung überwachen, um sicherzustellen, dass nur verwaltete Datenträger oder andere vertrauenswürdige Azure-Dienste auf Ihre Schlüssel zugreifen.

Für SSD Premium, SSD Standard und HDD Standard gilt Folgendes: Wenn Sie Ihren Schlüssel deaktivieren oder löschen, werden alle virtuellen Computer mit Datenträgern, die diesen Schlüssel verwenden, automatisch heruntergefahren. Danach können die virtuellen Computer nur verwendet werden, wenn der Schlüssel erneut aktiviert wird oder wenn Sie einen neuen Schlüssel zuweisen.    

Wenn Sie bei Ultra Disks einen Schlüssel deaktivieren oder löschen, werden VMs mit Ultra Disks, die den Schlüssel verwenden, nicht automatisch heruntergefahren. Nachdem die Zuordnung der virtuellen Computer aufgehoben wurde und die virtuellen Computer neu gestartet wurden, wird der Schlüssel nicht mehr von den Datenträgern verwendet, und die virtuellen Computer werden nicht wieder online geschaltet. Um die virtuellen Computer wieder online zu schalten, müssen Sie einen neuen Schlüssel zuweisen oder den vorhandenen Schlüssel aktivieren.    

Das folgende Diagramm zeigt, wie verwaltete Datenträger Azure Active Directory und Azure Key Vault verwenden, um Anforderungen mit dem vom Kunden verwalteten Schlüssel zu senden:

:::image type="content" source="media/virtual-machines-managed-disks-description-customer-managed-keys/customer-managed-keys-sse-managed-disks-workflow.png" alt-text="Workflow für verwaltete Datenträger und kundenseitig verwaltete Schlüssel. Ein Administrator erstellt eine Azure Key Vault-Instanz, dann einen Datenträgerverschlüsselungssatz und richtet den Datenträgerverschlüsselungssatz ein. Der Satz wird einer VM zugeordnet, sodass der Datenträger Azure AD zur Authentifizierung verwenden kann.":::

In der folgenden Liste wird das Diagramm ausführlicher erläutert:

1. Ein Azure Key Vault-Administrator erstellt Schlüsseltresorressourcen.
1. Der Schlüsseltresoradministrator importiert die jeweiligen RSA-Schlüssel in Key Vault oder generiert neue RSA-Schlüssel in Key Vault.
1. Dieser Administrator erstellt eine Instanz der Datenträgerverschlüsselungssatz-Ressource und gibt eine Azure Key Vault-ID sowie eine Schlüssel-URL an. Der Datenträgerverschlüsselungssatz ist eine neue Ressource, die zur Vereinfachung der Schlüsselverwaltung für verwaltete Datenträger eingeführt wurde. 
1. Beim Erstellen eines Datenträgerverschlüsselungssatzes wird eine [vom System zugewiesene verwaltete Identität](../articles/active-directory/managed-identities-azure-resources/overview.md) in Azure Active Directory (AD) erstellt und mit dem Datenträgerverschlüsselungssatz verknüpft. 
1. Der Azure Key Vault-Administrator erteilt dann der verwalteten Identität Berechtigungen zum Durchführen von Vorgängen im Schlüsseltresor.
1. Ein VM-Benutzer erstellt Datenträger, indem er sie dem Datenträgerverschlüsselungssatz zuordnet. Der VM-Benutzer kann auch die serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln für vorhandene Ressourcen aktivieren, indem er diese dem Datenträgerverschlüsselungssatz zuordnet. 
1. Verwaltete Datenträger verwenden die verwaltete Identität, um Anforderungen an den Azure Key Vault zu senden.
1. Zum Lesen oder Schreiben von Daten senden verwaltete Datenträger Anforderungen an Azure Key Vault, um den Datenverschlüsselungsschlüssel zu verschlüsseln (wrap) und zu entschlüsseln (unwrap), um die Daten zu verschlüsseln und zu entschlüsseln. 

Informationen zum Widerrufen von Kunden verwalteter Schlüsseln finden Sie in den Artikeln zu [Azure Key Vault-PowerShell](/powershell/module/azurerm.keyvault/) und zur [Azure Key Vault-CLI](/cli/azure/keyvault). Durch das Widerrufen des Zugriffs wird der Zugriff auf alle Daten im Speicherkonto blockiert, da Azure Storage keinen Zugriff mehr auf den Verschlüsselungsschlüssel hat.

#### <a name="automatic-key-rotation-of-customer-managed-keys-preview"></a>Automatische Schlüsselrotation von kundenseitig verwalteten Schlüsseln (Vorschau)

Sie können die automatische Schlüsselrotation für die neueste Schlüsselversion aktivieren. Ein Datenträger verweist über den Datenträgerverschlüsselungssatz auf einen Schlüssel. Wenn Sie die automatische Rotation für einen Datenträgerverschlüsselungssatz aktivieren, aktualisiert das System automatisch alle verwalteten Datenträger, Momentaufnahmen und Images, die auf den Datenträgerverschlüsselungssatz verweisen, um die neue Version des Schlüssels innerhalb von einer Stunde zu verwenden. Das Feature ist derzeit in der Vorschauversion für eine begrenzte Anzahl von Regionen verfügbar. Informationen zur regionalen Verfügbarkeit finden Sie im Abschnitt zu [unterstützten Regionen](#supported-regions).