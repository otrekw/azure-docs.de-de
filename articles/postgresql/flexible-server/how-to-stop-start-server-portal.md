---
title: 'Beenden/Starten – Azure-Portal – Azure Database for PostgreSQL: Flexible Server'
description: In diesem Artikel wird beschrieben, wie Sie Vorgänge in Azure Database for PostgreSQL im Azure-Portal beenden/starten.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 4c393e0048a0058ebe0fbf2b0ee65f6ae2e184c5
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90931193"
---
# <a name="stopstart-an-azure-database-for-postgresql---flexible-server-preview"></a>Beenden/Starten einer Azure Database for PostgreSQL: Flexible Server-Instanz (Vorschau)

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
