---
title: Beheben von Problemen bei der Datenverschlüsselung für Azure Database for MySQL
description: Erfahren Sie, wie Sie Probleme bei der Datenverschlüsselung für Ihre Azure Database for MySQL-Instanzen beheben.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: 4b517a463ec949d804798787ad4b35b53145a4a8
ms.sourcegitcommit: f255f869c1dc451fd71e0cab340af629a1b5fb6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2020
ms.locfileid: "77371488"
---
# <a name="troubleshooting-data-encryption-with-customer-managed-keys-in-azure-database-for-mysql"></a>Beheben von Problemen bei der Datenverschlüsselung mit vom Kunden verwalteten Schlüsseln in Azure Database for MySQL
In diesem Artikel wird beschrieben, wie Sie häufige Probleme/Fehler ermitteln und beheben, die bei einer für die Datenverschlüsselung mit einem vom Kunden verwalteten Schlüssel konfigurierten Azure Database for MySQL-Instanz auftreten.

## <a name="introduction"></a>Einführung
Wenn die Datenverschlüsselung mit einem vom Kunden verwalteten Schlüssel in Azure Key Vault konfiguriert ist, wird fortlaufender Zugriff auf diesen Schlüssel benötigt, damit der Server verfügbar bleiben kann. Wenn der Server den Zugriff auf den kundenseitig verwalteten Schlüssel in Azure Key Vault verliert, beginnt der Server, alle Verbindungen mit der entsprechenden Fehlermeldung abzulehnen. Zudem wird sein Zustand im Azure-Portal in ***Nicht zugänglich*** geändert.

Wenn ein unzugänglicher Azure Database for MySQL-Server nicht mehr benötigt wird, kann er sofort gelöscht werden, um Kosten zu vermeiden. Alle weiteren Aktionen auf dem Server sind erst dann erlaubt, wenn der Zugriff auf Azure Key Vault wiederhergestellt wurde und der Server wieder verfügbar ist. Das Ändern der Datenverschlüsselungsoption von „Ja“ (kundenseitig verwaltet) zu „Nein“ (dienstseitig verwaltet) ist auf einem nicht zugänglichen Server ebenfalls nicht möglich, wenn dieser mit kundenseitig verwalteten Schlüsseln verschlüsselt ist. Sie müssen den Schlüssel manuell erneut validieren, um den Server wieder verfügbar zu machen. Dies ist erforderlich, um die Daten vor einem nicht autorisierten Zugriff zu schützen, wenn die Berechtigungen für den vom Kunden verwalteten Schlüssel widerrufen wurden.

## <a name="common-errors-causing-server-to-become-inaccessible"></a>Häufige Fehler, die zu unzugänglichen Servern führen

Die meisten Probleme, die auftreten, wenn Sie die Datenverschlüsselung mit Azure Key Vault verwenden, werden durch einen der folgenden Konfigurationsfehler verursacht:

Der Schlüsseltresor ist nicht verfügbar oder nicht vorhanden.

* Der Schlüsseltresor wurde versehentlich gelöscht.
* Ein zeitweiliger Netzwerkfehler führt dazu, dass der Schlüsseltresor nicht verfügbar ist.

Es liegen keine Berechtigungen für den Zugriff auf den Schlüsseltresor vor, oder der Schlüssel ist nicht vorhanden.

* Der Schlüssel wurde versehentlich gelöscht oder deaktiviert, oder der Schlüssel ist abgelaufen.
* Die verwaltete Identität der Azure Database for MySQL-Instanz wurde versehentlich gelöscht.
* Die der verwalteten Identität für den Azure Database for MySQL-Server erteilten Berechtigungen für die Schlüssel sind nicht ausreichend (sie beinhalten nicht Get, Wrap und Unwrap).
* Die Berechtigungen für die verwaltete Identität der Azure Database for MySQL-Serverinstanz wurden widerrufen.

## <a name="identify-and-resolve-common-errors"></a>Identifizieren und Beheben von häufigen Fehlern
### <a name="errors-on-the-key-vault"></a>Fehler im Schlüsseltresor

#### <a name="disabled-key-vault"></a>Deaktivierter Schlüsseltresor
* AzureKeyVaultKeyDisabledMessage
* **Erläuterung:** Der Vorgang konnte auf dem Server nicht ausgeführt werden, da der Azure Key Vault-Schlüssel deaktiviert ist.

#### <a name="missing-key-vault-permissions"></a>Fehlende Schlüsseltresorberechtigungen
* AzureKeyVaultMissingPermissionsMessage
* Der Server verfügt nicht über die erforderlichen Get-, Wrap- und Unwrap-Berechtigungen für Azure Key Vault. Gewähren Sie dem Dienstprinzipal mit der ID die fehlenden Berechtigungen.

### <a name="mitigation"></a>Minderung
* Vergewissern Sie sich, dass der kundenseitig verwaltete Schlüssel in Key Vault vorhanden ist:
* Identifizieren Sie den Schlüsseltresor, und navigieren Sie dann im Azure-Portal zum Schlüsseltresor.
* Stellen Sie sicher, dass der durch den Schlüssel-URI identifizierte Schlüssel vorhanden ist.


## <a name="next-steps"></a>Nächste Schritte
[Einrichten der Datenverschlüsselung mit einem vom Kunden verwalteten Schlüssel für Azure Database for MySQL über das Azure-Portal](howto-data-encryption-portal.md)