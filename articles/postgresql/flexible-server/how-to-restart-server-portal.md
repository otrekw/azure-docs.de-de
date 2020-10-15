---
title: Neustarten – Azure-Portal – Azure Database for PostgreSQL – Flexible Server
description: In diesem Artikel wird beschrieben, wie Sie im Azure-Portal Neustartvorgänge für Azure Database for PostgreSQL durchführen.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 952bd6dddf9f276ed1a4a18f03799147f1902198
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90930303"
---
# <a name="restart-azure-database-for-postgresql---flexible-server"></a>Neustarten von Azure Database for PostgreSQL – Flexible Server

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

Dieser Artikel enthält Schritt-für-Schritt-Anleitungen zum Durchführen eines Neustarts der Flexible Server-Instanz. Dieser Vorgang ist nützlich, um alle Änderungen statischer Parameter zu übernehmen, die einen Neustart des Datenbankservers erfordern. Das Verfahren ist dasselbe für Server, die mit zonenredundanter Hochverfügbarkeit konfiguriert sind. 

> [!IMPORTANT]
> Bei Konfiguration mit Hochverfügbarkeit werden sowohl der primäre als auch der Standbyserver gleichzeitig neu gestartet.

## <a name="pre-requisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

-   Sie benötigen eine Flexible Server-Instanz.

## <a name="restart-your-flexible-server"></a>Neustarten Ihrer Flexible Server-Instanz

Führen Sie diese Schritte aus, um Ihre Flexible Server-Instanz neu zu starten.

1.  Wählen Sie im  [Azure-Portal](https://portal.azure.com/) Ihre Flexible Server-Instanz aus, die Sie neu starten möchten.

2.  Klicken Sie im linken Bereich auf **Übersicht** und dann auf **Neu starten**.
   
     :::image type="content" source="./media/how-to-restart-server-portal/restart-base-page.png" alt-text="Option „Neu starten“":::

3.  Eine Bestätigungsmeldung wird eingeblendet.

4.  Klicken Sie auf **Ja**, wenn Sie fortfahren möchten.
   
     :::image type="content" source="./media/how-to-restart-server-portal/restart-pop-up.png" alt-text="Option „Neu starten“":::
 
6.  Es wird eine Benachrichtigung angezeigt, dass der Neustartvorgang eingeleitet wurde.

## <a name="next-steps"></a>Nächste Schritte

-   Weitere Informationen zur [Geschäftskontinuität](./concepts-business-continuity.md)
-   Weitere Informationen zur  [zonenredundanten Hochverfügbarkeit](./concepts-high-availability.md)
