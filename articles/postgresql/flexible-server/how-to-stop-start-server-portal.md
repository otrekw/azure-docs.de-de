---
title: 'Beenden/Starten – Azure-Portal – Azure Database for PostgreSQL: Flexible Server'
description: In diesem Artikel wird beschrieben, wie Sie Vorgänge in Azure Database for PostgreSQL im Azure-Portal beenden/starten.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: a08c9244644228bd6d9154bdc4882fc91334bc62
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "110797372"
---
# <a name="stopstart-an-azure-database-for-postgresql---flexible-server-preview-using-azure-portal"></a>Beenden/Starten von Azure Database for PostgreSQL – Flexible Server (Vorschau) über das Azure-Portal

> [!IMPORTANT]
> Azure Database for PostgreSQL: Flexible Server befindet sich aktuell in der Vorschau.

Dieser Artikel enthält ausführliche Anweisungen zum Beenden und Starten einer Flexible Server-Instanz.

## <a name="pre-requisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

-   Sie müssen über Azure Database for PostgreSQL: Flexible Server verfügen.

## <a name="stop-a-running-server"></a>Beenden eines ausgeführten Servers

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Flexible Server-Instanz aus, die Sie beenden möchten.

2.  Klicken Sie auf der Seite **Übersicht** in der Symbolleiste auf die Schaltfläche **Beenden**.

> [!NOTE]
> Sobald der Server beendet wurde, sind keine anderen Verwaltungsvorgänge für die Flexible Server-Instanz mehr verfügbar.

Beachten Sie, dass beendete Server nach sieben Tagen automatisch wieder gestartet werden. Alle ausstehenden Wartungsupdates werden beim nächsten Start des Servers angewendet.

## <a name="start-a-stopped-server"></a>Starten eines beendeten Servers

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Flexible Server-Instanz aus, die Sie starten möchten.

2.  Klicken Sie auf der Seite **Übersicht** in der Symbolleiste auf die Schaltfläche **Starten**.

> [!NOTE]
> Sobald der Server gestartet wurde, sind alle anderen Verwaltungsvorgänge für die Flexible Server-Instanz wieder verfügbar.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Compute- und Speicheroptionen in Azure Database for PostgreSQL: Flexible Server](./concepts-compute-storage.md).
