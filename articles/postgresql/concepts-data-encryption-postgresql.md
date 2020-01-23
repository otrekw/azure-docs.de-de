---
title: Datenverschlüsselung auf Azure Database for PostgreSQL-Einzelservern mit kundenseitig verwalteten Schlüsseln
description: Die Datenverschlüsselung auf Azure Database for PostgreSQL-Einzelservern mit kundenseitig verwalteten Schlüsseln ermöglicht BYOK-Szenarien (Bring Your Own Key) für den Schutz von ruhenden Daten und gibt Organisationen die Möglichkeit, Verwaltungsaufgaben von den Schlüsseln und Daten zu trennen.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 263fdda178752ee22997a03a11902a7bff4791dc
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028611"
---
# <a name="azure-database-for-postgresql-single-server-data-encryption-with-customer-managed-key"></a>Datenverschlüsselung auf Azure Database for PostgreSQL-Einzelservern mit kundenseitig verwalteten Schlüsseln

> [!NOTE]
> Derzeit müssen Sie den Zugriff anfordern, um diese Funktion verwenden zu können. Wenden Sie sich zu diesem Zweck an AskAzureDBforPostgreSQL@service.microsoft.com.

Die Datenverschlüsselung auf Azure Database for PostgreSQL-Einzelservern mit kundenseitig verwalteten Schlüsseln ermöglicht BYOK-Szenarien (Bring Your Own Key) für den Schutz von ruhenden Daten und gibt Organisationen die Möglichkeit, Verwaltungsaufgaben von den Schlüsseln und Daten zu trennen. Bei der vom Kunden verwalteten Verschlüsselung ist der Kunde vollständig für die Verwaltung des Lebenszyklus von Schlüsseln (Schlüsselerstellung, -upload, -rotation, -löschung) und der Schlüsselnutzungsberechtigungen sowie für die Überwachung von Vorgängen für Schlüssel verantwortlich, hat damit aber auch vollständige Kontrolle.

Für Azure Database for PostgreSQL-Einzelserver wird die Datenverschlüsselung auf Serverebene festgelegt. Bei dieser Form der Datenverschlüsselung ist der Schlüssel, der für die Verschlüsselung des Datenbank-Verschlüsselungsschlüssels (Database Encryption Key, DEK) verwendet wird, ein vom Kunden verwalteter asymmetrischer Schlüssel, der in einer kundeneigenen und vom Kunden verwalteten [Azure Key Vault-Instanz (AKV)](../key-vault/key-Vault-secure-your-key-Vault.md) gespeichert ist, einem cloudbasierten externen Schlüsselverwaltungssystem. AKV bietet hochverfügbaren und skalierbaren sicheren Speicher für RSA-Kryptografieschlüssel, der optional von FIPS 140-2 Level 2-geprüften Hardwaresicherheitsmodulen (HSM) geschützt wird. Dabei wird kein direkter Zugriff auf einen gespeicherten Schlüssel zugelassen, sondern Verschlüsselung und Entschlüsselung werden mithilfe des Schlüssels für die autorisierten Entitäten bereitgestellt. Der Schlüssel kann von Key Vault generiert, importiert oder von [einem lokalen HSM-Gerät an Key Vault übertragen werden](../key-vault/key-Vault-hsm-protected-keys.md).

> [!NOTE]
> Dieses Feature steht in allen Azure-Regionen zur Verfügung, in denen Azure Database for PostgreSQL-Einzelserver die Tarife „Universell“ und „Arbeitsspeicheroptimiert“ unterstützen.

## <a name="benefits"></a>Vorteile

Die Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver bietet die folgenden Vorteile:

* Mehr Transparenz und differenziertere Steuerung und Verwaltung des Verschlüsselungsschlüssels 
* Zentrale Verwaltung und Organisation von Schlüsseln durch das Hosten in Azure Key Vault 
* Möglichkeit der Trennung von Aufgaben bei der Verwaltung von Schlüsseln und Daten innerhalb der Organisation
* Trennung der Schlüsselverwaltung von der Datenverwaltung innerhalb einer Organisation, sodass der Key Vault-Administrator Zugriffsberechtigungen widerrufen kann, um den Zugriff auf die verschlüsselte Datenbank zu verhindern 
* Mehr Vertrauen der Endkunden, da Azure Key Vault so entworfen wurde, dass Microsoft die Verschlüsselungsschlüssel weder einsehen noch extrahieren kann

## <a name="terminology-and-description"></a>Terminologie und Beschreibung

**Datenverschlüsselungsschlüssel (DEK)** : Ein symmetrischer AES256-Schlüssel zum Verschlüsseln einer Partition oder eines Datenblocks. Das Verschlüsseln jedes Datenblocks mit einem anderen Schlüssel erschwert Kryptoanalyseangriffe. Der Ressourcenanbieter oder die Anwendungsinstanz, die einen spezifischen Block ver- oder entschlüsselt, muss Zugriff auf die DEKs gewähren. Wenn ein DEK durch einen neuen Schlüssel ersetzt wird, müssen nur die Daten im verknüpften Block erneut mit dem neuen Schlüssel verschlüsselt werden.

**Schlüsselverschlüsselungsschlüssel (KEK):** Zum Verschlüsseln der Datenverschlüsselungsschlüssel wird ein Verschlüsselungsschlüssel verwendet. Mit einem KEK, der Key Vault nie verlässt, können die DEKs selbst verschlüsselt und gesteuert werden. Die Entität, die auf den KEK zugreifen kann, kann sich von der Entität unterscheiden, die den DEK erfordert. Da der KEK erforderlich ist, um DEKs zu entschlüsseln, ist der KEK ein Punkt, an dem DEKs gelöscht werden können, indem der KEK gelöscht wird.

Die Datenverschlüsselungsschlüssel (DEKs), die mit Schlüsselverschlüsselungsschlüsseln (KEKs) verschlüsselt wurden, werden separat gespeichert. Nur eine Entität mit Zugriff auf den KEK kann diese DEKs entschlüsseln. Weitere Informationen finden Sie unter [Sicherheit bei der Verschlüsselung ruhender Daten](../security/fundamentals/encryption-atrest.md).

## <a name="how-data-encryption-with-customer-managed-key-works"></a>Funktionsweise der Datenverschlüsselung mit kundenseitig verwalteten Schlüsseln

![Übersicht über Bring Your Own Key (BYOK)](media/concepts-data-access-and-security-data-encryption/postgresql-data-encryption-overview.png)

Damit ein PostgreSQL-Server die in Azure Key Vault (AKV) gespeicherten kundenseitig verwalteten Schlüssel für die Verschlüsselung des DEK verwenden kann, muss ein Key Vault-Administrator dem Server über dessen eindeutige Identität die folgenden Zugriffsrechte gewähren:

* **get:** zum Abrufen des öffentlichen Teils und der Eigenschaften des Schlüssels in Key Vault
* **wrapKey:** zum Verschlüsseln des DEK
* **unwrapKey:** zum Entschlüsseln des DEK

Der Key Vault-Administrator kann auch die [Protokollierung von Key Vault-Überwachungsereignissen aktivieren](../azure-monitor/insights/azure-key-vault.md), damit sie später überprüft werden können.

Wenn der Server für die Verwendung des in Key Vault gespeicherten vom Kunden verwalteten Schlüssels konfiguriert ist, sendet der Server den DEK für Verschlüsselungen an Key Vault. Key Vault gibt den verschlüsselten DEK zurück, der in der Benutzerdatenbank gespeichert wird. Entsprechend sendet der Server bei Bedarf den geschützten DEK zur Entschlüsselung an Key Vault. Prüfer können Azure Monitor verwenden, um die AuditEvent-Protokolle von Key Vault zu überprüfen, sofern die Protokollierung aktiviert wurde.

## <a name="requirements-for-configuring-data-encryption-for-azure-database-for-postgresql-single-server"></a>Anforderungen für die Konfiguration der Datenverschlüsselung für Azure Database for PostgreSQL-Einzelserver

### <a name="requirements-for-configuring-akv"></a>Anforderungen an die Konfiguration von AKV

* Key Vault und der Azure Database for PostgreSQL-Einzelserver müssen demselben Azure Active Directory-Mandanten (AAD) angehören. Mandantenübergreifende Interaktionen zwischen Key Vault und Server werden nicht unterstützt. Wenn Sie später Ressourcen verschieben möchten, müssen Sie die Datenverschlüsselung neu konfigurieren. Erfahren Sie mehr über das Verschieben von Ressourcen.
* Die Funktion zum vorläufigen Löschen muss in Key Vault aktiviert sein, um bei einer versehentlichen Löschung des Schlüssels (oder Schlüsseltresors) Datenverluste zu vermeiden. Vorläufig gelöschte Ressourcen werden 90 Tage lang aufbewahrt, sofern sie nicht in der Zwischenzeit vom Kunden wiederhergestellt oder endgültig gelöscht werden. Den Aktionen zum Wiederherstellen und endgültigen Löschen sind über Key Vault-Zugriffsrichtlinien eigene Berechtigungen zugewiesen. Die Funktion für vorläufiges Löschen ist standardmäßig deaktiviert und kann mit PowerShell oder der Befehlszeilenschnittstelle aktiviert werden. Über das Azure-Portal ist die Aktivierung nicht möglich.
* Gewähren Sie dem Azure Database for PostgreSQL-Einzelserver mit den Berechtigungen **get, wrapKey, unwrapKey** unter Verwendung der eindeutigen verwalteten Identität Zugriff auf Key Vault. Wenn Sie das Azure-Portal verwenden, wird die eindeutige Identifizierung automatisch erstellt, wenn die Datenverschlüsselung auf dem PostgreSQL-Einzelserver aktiviert wird. Ausführliche Anleitungen für die Verwendung des Azure-Portals finden Sie unter [Konfigurieren der Datenverschlüsselung für PostgreSQL-Einzelserver](howto-data-encryption-portal.md).

* Wenn Sie eine Firewall mit AKV verwenden, müssen Sie die Option *Vertrauenswürdige Microsoft-Dienste zulassen* aktivieren.

### <a name="requirements-for-configuring-customer-managed-key"></a>Anforderungen an die Konfiguration kundenseitig verwalteter Schlüssel

* Der vom Kunden verwaltete Schlüssel, der zum Verschlüsseln des DEK verwendet werden soll, muss ein asymmetrischer RSA 2028-Schlüssel sein.
* Das Schlüsselaktivierungsdatum (sofern festgelegt) muss ein Datum und eine Uhrzeit in der Vergangenheit sein. Das Ablaufdatum (sofern festgelegt) muss ein Datum und eine Uhrzeit in der Zukunft sein.
* Der Schlüssel muss sich im Zustand *Aktiviert* befinden.
* Wenn Sie einen vorhandenen Schlüssel in Key Vault importieren, müssen Sie ihn in einem unterstützten Dateiformat (`.pfx`, `.byok`, `.backup`) bereitstellen.

## <a name="recommendations-when-using-data-encryption-using-customer-managed-key"></a>Empfehlungen zur Verwendung der Datenverschlüsselung mit kundenseitig verwalteten Schlüsseln

### <a name="recommendation-for-configuring-akv"></a>Empfehlungen für die Konfiguration von AKV

* Legen Sie eine Ressourcensperre für Key Vault fest, um zu steuern, wer diese wichtige Ressource löschen kann, und um ein versehentliches oder nicht autorisiertes Löschen zu verhindern. Erfahren Sie mehr über Ressourcensperren.
* Aktivieren Sie die Überwachung und Berichterstellung für alle Verschlüsselungsschlüssel: Key Vault stellt Protokolle bereit, die sich problemlos in andere SIEM-Tools (Security Information & Event Management) einfügen lassen. Log Analytics in Azure Monitor ist ein Beispiel für einen Dienst, der bereits integriert ist.

* Stellen Sie sicher, dass sich Key Vault und der Azure Database for PostgreSQL-Einzelserver in derselben Region befinden, um einen schnelleren Zugriff für Wrap-/Unwrap-Vorgänge bei DEKs sicherzustellen.

### <a name="recommendation-for-configuring-customer-managed-key"></a>Empfehlungen für die Konfiguration kundenseitig verwalteter Schlüssel

* Bewahren Sie eine Kopie des kundenseitig verwalteten Schlüssels (KEK) an einem sicheren Ort auf, oder hinterlegen Sie ihn bei einem entsprechenden Dienst.

* Wenn der Schlüssel in Key Vault generiert wird, erstellen Sie eine Schlüsselsicherung, bevor Sie den Schlüssel zum ersten Mal in AKV verwenden. Die Sicherung kann nur in einer Azure Key Vault-Instanz wiederhergestellt werden. Unter [Backup-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyVault/backup-azkeyVaultkey) erfahren Sie mehr zu diesem Befehl.

## <a name="inaccessible-customer-managed-key-condition"></a>Kein Zugriff auf den kundenseitig verwalteten Schlüssel

Wenn die Datenverschlüsselung mit einem vom Kunden verwalteten Schlüssel in Azure Key Vault (AKV) konfiguriert ist, wird fortlaufender Zugriff auf diesen Schlüssel benötigt, damit der Server online bleiben kann. Falls der Server keinen Zugriff mehr auf den kundenseitig verwalteten Schlüssel in AKV hat, beginnt er nach etwa 10 Minuten, mit einer entsprechenden Fehlermeldung alle Verbindungen zu verweigern, und ändert den Serverstatus in **Kein Zugriff**. Die einzige zulässige Aktion für eine Datenbank im Zustand „Kein Zugriff“ ist das Löschen der Datenbank.

### <a name="accidental-key-access-revocation-from-the-azure-key-vault-akv"></a>Versehentliche Sperrung des Zugriffs auf den Schlüssel in Azure Key Vault (AKV)

Es kann vorkommen, dass ein Benutzer mit ausreichenden Zugriffsrechten für Key Vault den Serverzugriff auf den Schlüssel versehentlich durch eine der folgende Aktionen deaktiviert:

* Widerrufen der Berechtigungen „get“, „wrapKey“ und „unwrapKey“ vom Server
* Löschen des Schlüssels
* Löschen des Schlüsseltresors
* Ändern von Firewallregeln des Schlüsseltresors

* Löschen der verwalteten Identität des Servers in Azure Active Directory

## <a name="monitoring-of-the-customer-managed-key-in-the-key-vault"></a>Überwachen des kundenseitig verwalteten Schlüssels im Schlüsseltresor

Konfigurieren Sie die folgenden Azure-Features, um den Datenbankzustand zu überwachen und Warnungen bei Verlust des Zugriffs auf die TDE-Schutzvorrichtung zu aktivieren:

* [Azure Resource Health:](../service-health/resource-health-overview.md) Eine Datenbank, auf die nicht zugegriffen werden kann und die den Zugriff auf den Kundenschlüssel verloren hat, wird als „Kein Zugriff“ angezeigt, nachdem die erste Verbindung mit der Datenbank verweigert wurde.
* [Aktivitätsprotokoll:](../service-health/alerts-activity-log-service-notifications.md) Ist der Zugriff auf den Kundenschlüssel im vom Kunden verwalteten Schlüsseltresor nicht möglich, werden dem Aktivitätsprotokoll entsprechende Einträge hinzugefügt. Durch die Erstellung von Warnungen für diese Ereignisse können Sie den Zugriff schnellstmöglich wiederherstellen.

* [Aktionsgruppen](../azure-monitor/platform/action-groups.md) können definiert werden, um Benachrichtigungen und Warnungen gemäß Ihren Präferenzen zu senden – also etwa per E-Mail/SMS/Pushbenachrichtigung/Sprachnachricht, per Logik-App, Webhook, ITSM oder Automation-Runbook.

## <a name="restore-and-replica-with-customers-managed-key-in-the-key-vault"></a>Wiederherstellen und Replizieren des kundenseitig verwalteten Schlüssels in Key Vault

Nachdem ein Azure Database for PostgreSQL-Einzelserver mit dem kundenseitig verwalteten, in Key Vault gespeicherten Schlüssel verschlüsselt wurde, werden alle neu erstellten Kopien des Servers (entweder über lokale oder Geowiederherstellungsvorgänge oder durch Lesereplikate) ebenfalls mit demselben vom Kunden verwalteten Schlüssel verschlüsselt. Sie können jedoch so geändert werden, dass für die Verschlüsselung der neue kundenseitig verwaltete Schlüssel angegeben wird. Wenn der vom Kunden verwaltete Schlüssel geändert wird, beginnen alte Sicherungen des Servers damit, den neuesten Schlüssel zu verwenden.

Um Probleme bei der Einrichtung einer kundenseitig verwalteten Datenverschlüsselung während der Wiederherstellung oder der Lesereplikaterstellung zu vermeiden, sollten Sie unbedingt die folgenden Schritte auf dem Master und dem wiederhergestellten bzw. Replikatserver ausführen:

* Initiieren Sie die Wiederherstellung oder die Erstellung eines Lesereplikats auf dem Master des Azure Database for PostgreSQL-Einzelservers.
* Der neu erstellte Server (wiederhergestellt oder Replikat) bleibt im Zustand „Kein Zugriff“, da seiner eindeutigen Identität noch keine Berechtigungen für Azure Key Vault (AKV) erteilt wurden.
* Überprüfen Sie auf dem wiederhergestellten Server oder dem Replikatserver erneut den vom Kunden verwalteten Schlüssel in den Einstellungen zur Datenverschlüsselung, um sicherzustellen, dass dem neu erstellten Server die Berechtigungen „wrap“ und „unwrap“ für den in AKV gespeicherten Schlüssel erteilt wurden.

* Beide oben aufgeführten Schritte müssen ausgeführt werden, um sicherzustellen, dass die Datenverschlüsselung sowohl auf dem Master als auch auf dem wiederhergestellten bzw. Replikatserver beibehalten wird.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie die [Datenverschlüsselung mit einem kundenseitig verwalteten Schlüssel für Ihren Azure Database for PostgreSQL-Einzelserver über das Azure-Portal einrichten](howto-data-encryption-portal.md).
