---
title: Aktualisieren einer Servergruppe – Hyperscale (Citus) – Azure Database for PostgreSQL
description: In diesem Artikel wird beschrieben, wie Sie PostgreSQL und Citus in Azure Database for PostgreSQL aktualisieren können – Hyperscale (Citus).
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 4/5/2021
ms.openlocfilehash: 161204bf02ac36c1f5a3969cf57c61e98560c9b5
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2021
ms.locfileid: "107518897"
---
# <a name="upgrade-hyperscale-citus-server-group"></a>Aktualisieren einer Hyperscale (Citus)-Servergruppe

In diesen Anleitungen wird beschrieben, wie Sie alle Servergruppenknoten auf eine neue Hauptversion von PostgreSQL aktualisieren können.

## <a name="test-the-upgrade-first"></a>Testen Sie zuerst das Upgrade.

Das Aktualisieren von PostgreSQL führt zu mehr Änderungen, als Sie sich vorstellen können, weil Hyperscale (Citus) die [Datenbankerweiterungen](concepts-hyperscale-extensions.md), einschließlich der Citus-Erweiterung, aktualisiert.
Wir empfehlen dringend, dass Sie Ihre Anwendung mit der neuen PostgreSQL- und Citus-Version testen, bevor Sie Ihre Produktionsumgebung aktualisieren.

Eine praktische Möglichkeit zum Testen ist das Erstellen einer Kopie Ihrer Servergruppe mithilfe der [Zeitpunktwiederherstellung](concepts-hyperscale-backup.md#restore). Aktualisieren Sie die Kopie, und testen Sie damit Ihre Anwendung. Nachdem Sie überprüft haben, ob alles einwandfrei funktioniert, aktualisieren Sie die ursprüngliche Servergruppe.

## <a name="upgrade-a-server-group-in-the-azure-portal"></a>Aktualisieren einer Servergruppe im Azure-Portal

1. Wählen Sie im Abschnitt **Übersicht** einer Hyperscale (Citus)-Servergruppe die Schaltfläche **Upgrade** aus.
1. Ein Dialogfeld mit der aktuellen Version von PostgreSQL und Citus wird angezeigt.
   Wählen Sie in der Liste **Upgrade auf** eine neue PostgreSQL-Version aus.
1. Überprüfen Sie, ob der Wert in **Citus version after upgrade** (Citus-Version nach dem Upgrade) Ihren Erwartungen entspricht.
   Dieser Wert ändert sich basierend auf der von Ihnen ausgewählten PostgreSQL-Version.
1. Wählen Sie die Schaltfläche **Upgrade** aus, um den Vorgang fortzusetzen.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über [unterstützte PostgreSQL-Versionen](concepts-hyperscale-versions.md).
* Sehen Sie sich an, [welche Erweiterungen](concepts-hyperscale-extensions.md) in jeder PostgreSQL-Version bei einer Hyperscale (Citus)-Servergruppe enthalten sind.
