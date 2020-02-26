---
title: Problembehandlung bei der Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver
description: Erfahren Sie, wie Sie Probleme bei der Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver beheben.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: 6d2e027c47bef3186f95c2183b316b5c15511070
ms.sourcegitcommit: f255f869c1dc451fd71e0cab340af629a1b5fb6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2020
ms.locfileid: "77371472"
---
# <a name="troubleshooting-data-encryption-with-customer-managed-keys-in-azure-database-for-postgresql---single-server"></a>Problembehandlung bei der Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver mit kundenseitig verwalteten Schlüsseln
In diesem Artikel wird beschrieben, wie Sie häufige Probleme/Fehler auf einem Azure Database for PostgreSQL-Einzelserver ermitteln und beheben, der für die Datenverschlüsselung mit einem kundenseitig verwalteten Schlüssel konfiguriert ist.

## <a name="introduction"></a>Einführung
Wenn die Datenverschlüsselung mit einem kundenseitig verwalteten Schlüssel in Azure Key Vault konfiguriert ist, wird fortlaufender Zugriff auf diesen Schlüssel benötigt, damit der Server verfügbar bleibt. Wenn der Server den Zugriff auf den kundenseitig verwalteten Schlüssel in Azure Key Vault verliert, beginnt er damit, alle Verbindungen mit der entsprechenden Fehlermeldung abzulehnen. Zudem wird sein Zustand im Azure-Portal in ***Zugriff nicht möglich*** geändert.

Wenn ein nicht zugänglicher Azure Database for PostgreSQL-Einzelserver nicht mehr benötigt wird, kann er sofort gelöscht werden, um Kosten zu vermeiden. Alle weiteren Aktionen auf dem Server sind erst dann erlaubt, wenn der Zugriff auf Azure Key Vault wiederhergestellt wurde und der Server wieder verfügbar ist. Das Ändern der Datenverschlüsselungsoption von „Ja“ (kundenseitig verwaltet) zu „Nein“ (dienstseitig verwaltet) ist auf einem nicht zugänglichen Server ebenfalls nicht möglich, wenn dieser mit einem kundenseitig verwalteten Schlüssel verschlüsselt ist. Sie müssen den Schlüssel manuell erneut validieren, um den Server wieder verfügbar zu machen. Dies ist notwendig, um die Daten vor nicht autorisierten Zugriffen zu schützen, wenn die Berechtigungen für den kundenseitig verwalteten Schlüssel widerrufen wurden.

## <a name="common-errors-causing-server-to-become-inaccessible"></a>Häufige Fehler, die zu nicht zugänglichen Servern führen

Die meisten Probleme, die bei der Verwendung der Datenverschlüsselung mit Azure Key Vault auftreten, werden durch einen der folgenden Konfigurationsfehler verursacht:

Der Schlüsseltresor ist nicht verfügbar oder nicht vorhanden.

* Der Schlüsseltresor wurde versehentlich gelöscht.
* Ein zeitweiliger Netzwerkfehler führt dazu, dass der Schlüsseltresor nicht verfügbar ist.

Es liegen keine Berechtigungen für den Zugriff auf den Schlüsseltresor vor, oder der Schlüssel ist nicht vorhanden.

* Der Schlüssel wurde versehentlich gelöscht oder deaktiviert, oder der Schlüssel ist abgelaufen.
* Die von der Instanz verwaltete Identität des Azure Database for PostgreSQL-Einzelservers wurde versehentlich gelöscht.
* Die Berechtigungen, die der verwalteten Azure Database for PostgreSQL-Identität für die Schlüssel erteilt wurde, sind nicht ausreichend (sie umfassen „Get“, „Wrap“ und „Unwrap“ nicht).
* Die Berechtigungen für die instanzseitig verwaltete Identität des Azure Database for PostgreSQL-Einzelservers wurden widerrufen oder gelöscht.

## <a name="identify-and-resolve-common-errors"></a>Identifizieren und Beheben von häufigen Fehlern
### <a name="errors-on-the-key-vault"></a>Fehler in Key Vault

#### <a name="disabled-key-vault"></a>Key Vault deaktiviert
* AzureKeyVaultKeyDisabledMessage
* **Erläuterung**: Der Vorgang konnte auf dem Server nicht ausgeführt werden, weil der Azure Key Vault-Schlüssel deaktiviert ist.

#### <a name="missing-key-vault-permissions"></a>Fehlende Berechtigungen für Key Vault
* AzureKeyVaultMissingPermissionsMessage
* Der Server verfügt nicht über die erforderlichen Get-, Wrap- und Unwrap-Berechtigungen für die Azure Key Vault-Berechtigungen. Erteilen Sie dem Dienstprinzipal mit der entsprechenden ID alle fehlenden Berechtigungen.

### <a name="mitigation"></a>Minderung
* Vergewissern Sie sich, dass der kundenseitig verwaltete Schlüssel in Key Vault vorhanden ist:
* Identifizieren Sie den Schlüsseltresor, und navigieren Sie dann im Azure-Portal zum Schlüsseltresor.
* Stellen Sie sicher, dass der durch den Schlüssel-URI identifizierte Schlüssel vorhanden ist.


## <a name="next-steps"></a>Nächste Schritte
[Einrichten der Datenverschlüsselung mit einem vom Kunden verwalteten Schlüssel für Azure Database for PostgreSQL über das Azure-Portal](howto-data-encryption-portal.md)