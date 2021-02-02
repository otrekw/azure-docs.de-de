---
title: Hauptversionsupgrade in Azure Database for MySQL Single Server
description: In diesem Artikel wird beschrieben, wie Sie ein Upgrade der Hauptversion für Azure Database for MySQL Single Server durchführen.
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 1/13/2021
ms.openlocfilehash: b62f4ebc61ac27478788d8b2bae5e4145f87ac8b
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2021
ms.locfileid: "98630199"
---
# <a name="major-version-upgrade-in-azure-database-for-mysql-single-server"></a>Hauptversionsupgrade in Azure Database for MySQL Single Server

> [!IMPORTANT]
> Das Hauptversionsupgrade für Azure Database for MySQL Single Server befindet sich in der öffentlichen Vorschauphase.

In diesem Artikel wird beschrieben, wie Sie im Azure-Portal ein direktes Upgrade Ihrer MySQL-Hauptversion für Azure Database for MySQL Single Server durchführen.

Mit diesem Feature können Kunden einfach per Klick direkte Upgrades ihrer MySQL 5.6-Server auf MySQL-Version 5.7 durchführen, ohne dass Daten verschoben oder Verbindungszeichenfolgen für Anwendungen geändert werden müssen.

> [!Note]
> * Hauptversionsupgrade ist nur für das Upgrade von MySQL 5.6 auf MySQL 5.7 verfügbar<br>
> * Das Hauptversionsupgrade wird auf dem Replikatserver noch nicht unterstützt.
> * Der Server ist während des gesamten Upgradevorgangs nicht verfügbar. Es wird daher empfohlen, Upgrades während Ihres geplanten Wartungsfensters durchzuführen.

## <a name="perform-major-version-upgrade-from-mysql-56-to-mysql-57-using-azure-portal"></a>Durchführung des Hauptversionsupgrades von MySQL 5.6 auf MySQL 5.7 über das Azure-Portal

Führen Sie die folgenden Schritte aus, um ein Hauptversionsupgrade für Ihren Azure Database for MySQL-Server 5.6 über das Azure-Portal durchzuführen:

> [!IMPORTANT]
> Wir empfehlen, das Upgrade zunächst an einer wiederhergestellten Kopie des Servers durchzuführen, anstatt direkt ein Upgrade der Produktionsumgebung durchzuführen. Weitere Informationen finden Sie unter [Durchführen einer Zeitpunktwiederherstellung](howto-restore-server-portal.md#point-in-time-restore).

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihren vorhandenen Azure Database for MySQL-Server 5.6 aus.

2. Klicken Sie auf der Seite **Übersicht** in der Symbolleiste auf die Schaltfläche **Upgrade**.

3. Wählen Sie im Abschnitt **Upgrade** die Option **OK** aus, um den Azure Database for MySQL-Server 5.6 auf Version 5.7 zu aktualisieren.

   :::image type="content" source="./media/how-to-major-version-upgrade-portal/upgrade.png" alt-text="Azure Database for MySQL: Übersicht über Upgrade":::

4. Mit einer Benachrichtigung wird bestätigt, dass der Upgradevorgang erfolgreich war.


## <a name="perform-major-version-upgrade-from-mysql-56-to-mysql-57-using-azure-cli"></a>Durchführung des Hauptversionsupgrades von MySQL 5.6 auf MySQL 5.7 über die Azure-Befehlszeilenschnittstelle

Führen Sie die folgenden Schritte aus, um ein Hauptversionsupgrade für Ihren Azure Database for MySQL-Server 5.6 über die Azure-Befehlszeilenschnittstelle durchzuführen:

> [!IMPORTANT]
> Wir empfehlen, das Upgrade zunächst an einer wiederhergestellten Kopie des Servers durchzuführen, anstatt direkt ein Upgrade der Produktionsumgebung durchzuführen. Weitere Informationen finden Sie unter [Durchführen einer Zeitpunktwiederherstellung](howto-restore-server-cli.md#server-point-in-time-restore).

1. Installieren Sie die [Azure-Befehlszeilenschnittstelle für Windows](/cli/azure/install-azure-cli) oder verwenden Sie die Azure-Befehlszeilenschnittstelle in [Azure Cloud Shell](../cloud-shell/overview.md), um die Upgradebefehle auszuführen. 
 
   Für dieses Upgrade ist mindestens Version 2.16.0 oder höher der Azure-Befehlszeilenschnittstelle erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert. Führen Sie az version aus, um die installierte Version und die abhängigen Bibliotheken zu ermitteln. Führen Sie az upgrade aus, um das Upgrade auf die aktuelle Version durchzuführen.

2. Nachdem Sie sich angemeldet haben, führen Sie den Befehl [az mysql server upgrade](https://docs.microsoft.com/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_upgrade&preserve-view=true) aus:
    
   ```azurecli
   az mysql server upgrade --name testsvr --resource-group testgroup --subscription MySubscription --target-server-version 5.7"
   ```
   
   Die Eingabeaufforderung zeigt die Meldung „-Running“ (Wird ausgeführt) an. Nachdem diese Meldung nicht mehr angezeigt wird, ist das Versionsupgrade abgeschlossen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="when-will-this-upgrade-feature-be-ga-as-we-have-mysql-v56-in-our-production-environment-that-we-need-to-upgrade"></a>Wann wird dieses Upgrade-Feature allgemein verfügbar sein, da wir MySQL v5.6 in unserer Produktionsumgebung verwenden, das ein Upgrade erfordert?

Die allgemeine Verfügbarkeit dieses Features ist vor der Einstellung von MySQL v5.6 geplant. Das Feature ist jedoch bereit für die Produktionsumgebung und wird von Azure voll unterstützt, sodass Sie es mit Zuversicht in Ihrer Umgebung ausführen können sollten. Als empfohlene bewährte Methode empfehlen wir Ihnen dringend, es zuerst auf einer wiederhergestellten Kopie des Servers auszuführen und zu testen, sodass Sie die Downtime während des Upgrades abschätzen können, und einen Anwendungskompatibilitätstest durchzuführen, bevor Sie es in der Produktionsumgebung ausführen. Weitere Informationen zum Erstellen einer Zeitpunktkopie Ihres Servers finden Sie unter [Durchführen einer Zeitpunktwiederherstellung](howto-restore-server-portal.md#point-in-time-restore). 

### <a name="will-this-cause-downtime-of-the-server-and-if-so-how-long"></a>Führt dies zu einer Downtime des Servers und wenn ja, wie lange dauert dies?

Ja, der Server wird während des Upgradeprozesses nicht verfügbar sein, sodass wir Ihnen empfehlen, diesen Vorgang während Ihres geplanten Wartungsfensters durchzuführen. Die geschätzte Downtime hängt von der Datenbankgröße, der bereitgestellten Speichergröße (bereitgestellte IOPs) und der Anzahl der Tabellen in der Datenbank ab. Die Upgradezeit verhält sich direkt proportional zur Anzahl der Tabellen auf dem Server. Die Upgrades von Basic-SKU-Servern werden voraussichtlich länger dauern, da es sich um eine Standardspeicherplattform handelt. Um die Downtime für Ihre Serverumgebung abzuschätzen, empfehlen wir, das Upgrade zunächst auf einer wiederhergestellten Kopie des Servers durchzuführen.  

### <a name="it-is-noted-that-it-is-not-supported-on-replica-server-yet-what-does-that-mean-concrete"></a>Es wird darauf hingewiesen, dass es auf dem Replikatserver noch nicht unterstützt wird. Was bedeutet das konkret?

Derzeit wird das Upgrade der Hauptversion für Replikatserver nicht unterstützt, was bedeutet, dass Sie es nicht für Server ausführen sollten, die an der Replikation beteiligt sind (weder Quell- noch Replikatserver). Wenn Sie das Upgrade der an der Replikation beteiligten Server testen möchten, bevor wir die Replikatunterstützung für das Upgrade-Feature hinzufügen, empfehlen wir folgende Schritte:

1. Während Ihrer geplanten Wartung [beenden Sie die Replikation und löschen den Replikatserver](howto-read-replicas-portal.md), nachdem Sie seinen Namen und alle Konfigurationsinformationen (Firewalleinstellungen, Konfiguration der Serverparameter, falls sie sich vom Quellserver unterscheidet) erfasst haben.
2. Führen Sie ein Upgrade des Quellservers durch.
3. Stellen Sie einen neuen Lesereplikatserver mit demselben Namen und denselben Konfigurationseinstellungen bereit, die in Schritt 1 erfasst wurden. Der neue Replikatserver weist automatisch v5.7 auf, nachdem der Quellserver auf v5.7 aktualisiert wurde.

### <a name="what-will-happen-if-we-do-not-choose-to-upgrade-our-mysql-v56-server-before-february-5-2021"></a>Was passiert, wenn wir das Upgrade unseres MySQL v5.6-Servers nicht vor dem 5. Februar 2021 durchführen?

Sie können Ihren MySQL v5.6-Server weiterhin wie bisher betreiben. Azure **wird niemals** ein erzwungenes Upgrade auf Ihrem Server durchführen. Es gelten jedoch die in der Richtlinie [Azure Database for MySQL: Versionsrichtlinie](concepts-version-policy.md) dokumentierten Einschränkungen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich unter [Azure Database for MySQL: Versionsrichtlinie](concepts-version-policy.md).
