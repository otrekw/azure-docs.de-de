---
title: Verschieben zwischen Regionen – Azure-Portal – Azure Database for PostgreSQL (Einzelserver)
description: Verschieben Sie einen Azure Database for PostgreSQL-Server mithilfe eines Lesereplikats und des Azure-Portals aus einer Azure-Region in eine andere.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 06/29/2020
ms.openlocfilehash: 0eaac8ac65b490e1b8de716d79bd36d2f7a7567c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91707658"
---
# <a name="move-an-azure-database-for-azure-database-for-postgresql---single-server-to-another-region-by-using-the-azure-portal"></a>Verschieben eines Azure Database for PostgreSQL-Einzelservers mithilfe des Azure-Portals in eine andere Region

Es gibt verschiedene Szenarien, in denen ein vorhandener Azure Database for PostgreSQL-Server aus einer Region in eine andere Region verschoben wird. Ein derartiges Szenario könnte beispielsweise darin bestehen, dass Sie im Rahmen Ihrer Planung für die Notfallwiederherstellung einen Produktionsserver in eine andere Region verschieben möchten.

Sie können ein [regionsübergreifendes Lesereplikat](concepts-read-replicas.md#cross-region-replication) für Azure Database for PostgreSQLverwenden, um die Verschiebung in eine andere Region vorzunehmen. Hierzu erstellen Sie zunächst ein Lesereplikat in der Zielregion. Beenden Sie anschließend die Replikation auf dem Lesereplikatserver, um ihn zu einem eigenständigen Server zu machen, der sowohl Lese- als auch Schreibdatenverkehr akzeptiert. 

> [!NOTE]
> Der Schwerpunkt des vorliegenden Artikels liegt auf dem Verschieben des Servers in eine andere Region. Wenn Sie Ihren Server in eine andere Ressourcengruppe oder ein anderes Abonnement verschieben möchten, finden Sie entsprechende Informationen im Artikel [Verschieben](https://docs.microsoft.com/azure/azure-resource-manager/management/move-resource-group-and-subscription). 

## <a name="prerequisites"></a>Voraussetzungen

- Das Feature für regionsübergreifende Lesereplikate ist nur für Azure Database for PostgreSQL-Einzelservers in den Tarifen „Universell“ oder „Arbeitsspeicheroptimiert“ verfügbar. Stellen Sie sicher, dass für den Quellserver einer der folgenden Tarife festgelegt ist.

- Stellen Sie sicher, dass sich der Azure Database for P-Quellserver in der Azure-Region befindet, aus der die Verschiebung erfolgen soll.

## <a name="prepare-to-move"></a>Vorbereiten der Verschiebung

Führen Sie die folgenden Schritte aus, um den Quellserver für Replikation mit dem Azure-Portal vorzubereiten: 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Wählen Sie den vorhandenen Azure Database for PostgreSQL-Server aus, den Sie als Quellserver verwenden möchten. Mit dieser Aktion wird die Seite **Übersicht** geöffnet.
1. Wählen Sie im Menü des Servers **Replikation** aus. Wenn die Azure-Replikationsunterstützung mindestens auf **Replica** festgelegt ist, können Sie Lesereplikate erstellen. 
1. Wenn die Azure-Replikationsunterstützung nicht auf mindestens **Replica** festgelegt ist, legen Sie sie entsprechend fest. Wählen Sie **Speichern** aus.
1. Starten Sie den Server neu, um die Änderung zu übernehmen, indem Sie **Ja** auswählen.
1. Sie erhalten zwei Azure-Portalbenachrichtigungen, sobald der Vorgang abgeschlossen ist. Es gibt eine Benachrichtigung zur Aktualisierung des Serverparameters. Eine weitere Benachrichtigung bezieht sich auf den Neustart des Servers, der unmittelbar erfolgt.
1. Aktualisieren Sie die Azure-Portalseite zum Aktualisieren der Replikationssymbolleiste. Sie können jetzt schreibgeschützte Replikate für diesen Server erstellen.

Führen Sie die folgenden Schritte aus, um mithilfe des Azure-Portals einen regionsübergreifenden Lesereplikatserver in der Zielregion zu erstellen:

1. Wählen Sie den vorhandenen Azure Database for PostgreSQL-Server aus, den Sie als Quellserver verwenden möchten.
1. Wählen Sie im Menü unter **EINSTELLUNGEN** die Option **Replikation** aus.
1. Wählen Sie **Replikat hinzufügen**.
1. Geben Sie einen Namen für den Replikatserver ein.
1. Wählen Sie den Standort für den Replikatserver aus. Der Standardstandort ist mit dem des primären Servers identisch. Vergewissern Sie sich, dass Sie den Zielspeicherort ausgewählt haben, an dem das Replikat bereitgestellt werden soll.
1. Wählen Sie **OK** aus, um die Erstellung des Replikats zu bestätigen. Während der Replikaterstellung werden die Daten vom Quellserver auf das Replikat kopiert. Die Erstellung kann mehrere Minuten oder sogar länger dauern, dies hängt von der Größe des Quellservers ab.

>[!NOTE]
> Wenn Sie ein Replikat erstellen, erbt dieses weder die Firewallregeln noch die VNET-Dienstendpunkte des primären Servers. Diese Regeln müssen separat für das Replikat eingerichtet werden.

## <a name="move"></a>Move

> [!IMPORTANT]
> Der eigenständige Server kann nicht wieder in ein Replikat umgewandelt werden.
> Stellen Sie vor dem Beenden der Replikation auf einem Lesereplikat sicher, dass das Replikat alle erforderlichen Daten enthält.

Führen Sie die folgenden Schritte aus, um im Azure-Portal die Replikation in das Replikat zu beenden:

1. Nachdem das Replikat erstellt wurde, suchen Sie den Azure Database for PostgreSQL-Quellserver, und wählen Sie ihn aus. 
1. Wählen Sie im Menü unter **EINSTELLUNGEN** die Option **Replikation** aus.
1. Wählen Sie den Replikatserver aus.
1. Wählen Sie **Replikation beenden** aus.
1. Klicken Sie auf **OK**, um zu bestätigen, dass Sie die Replikation beenden möchten.

## <a name="clean-up-source-server"></a>Bereinigen des Quellservers

Möglicherweise möchten Sie den Azure Database for PostgreSQL-Quellserver löschen. Führen Sie dazu die folgenden Schritte aus:

1. Nachdem das Replikat erstellt wurde, suchen Sie den Azure Database for PostgreSQL-Quellserver, und wählen Sie ihn aus.
1. Wählen Sie im Fenster **Übersicht** die Option **Löschen** aus.
1. Geben Sie den Namen des Quellservers ein, um zu bestätigen, dass Sie ihn löschen möchten.
1. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie mithilfe des Azure-Portals einen Azure Database for PostgreSQL-Server aus einer Region in eine andere verschoben und anschließend die nicht mehr benötigten Quellressourcen bereinigt. 

- Weitere Informationen zu [Lesereplikaten](concepts-read-replicas.md)
- Weitere Informationen zum [Verwalten von Lesereplikaten im Azure-Portal](howto-read-replicas-portal.md)
- Weitere Informationen zu den Optionen für [Geschäftskontinuität](concepts-business-continuity.md)
