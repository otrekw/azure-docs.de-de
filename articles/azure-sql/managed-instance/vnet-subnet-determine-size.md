---
title: Bestimmen der erforderlichen Subnetzgröße und des Bereichs
titleSuffix: Azure SQL Managed Instance
description: In diesem Thema erfahren Sie, wie Sie die Größe des Subnetzes berechnen, in dem Azure SQL Managed Instance bereitgestellt wird.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 02/22/2019
ms.openlocfilehash: 5eebde1fc95cb50f8ff7c13b6cbc411484ddf943
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84039521"
---
# <a name="determine-required-subnet-size--range-for-azure-sql-managed-instance"></a>Bestimmen von Subnetzgröße und -bereich für Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL Managed Instance muss in einem [virtuellen Azure-Netzwerk (VNET)](../../virtual-network/virtual-networks-overview.md) bereitgestellt werden.

Die Anzahl der Instanzen von SQL Managed Instance, die im Subnetz des VNET bereitgestellt werden können, hängt von der Größe des Subnetzes (dem Subnetzbereich) ab.

Bei der Erstellung einer Instanz von SQL Managed Instance ordnet Azure abhängig vom Tarif, den Sie bei der Bereitstellung ausgewählt haben, eine Reihe von virtuellen Computern zu. Da diese virtuellen Computer Ihrem Subnetz zugeordnet sind, benötigen sie IP-Adressen. Um bei regulären Vorgängen und bei der Wartung von Diensten Hochverfügbarkeit sicherzustellen, kann Azure zusätzliche virtuelle Computer zuordnen. Daher ist die Anzahl der erforderlichen IP-Adressen in einem Subnetz größer als die Anzahl der Instanzen von SQL Managed Instance in diesem Subnetz.

Konzeptionell benötigt eine Instanz von SQL Managed Instance zwischen 16 und 256 IP-Adressen in einem Subnetz. Daher können Sie für die Definition Ihrer Subnetz-IP-Adressbereiche 28- bis 24-Bit-Masken verwenden. Eine 28-Bit-Maske (14 Hosts pro Netzwerk) ist eine gute Größe für eine Bereitstellung für einen universellen Zweck oder für geschäftskritische Bereitstellungen. Eine 27-Bit-Maske (30 Hosts pro Netzwerk) ist ideal für Bereitstellungen mit mehreren Instanzen von SQL Managed Instance innerhalb des gleichen VNET. 26- (62 Hosts) und 24-Bit-Masken (254 Hosts) ermöglichen die weitere Skalierung über das VNET hinaus, um zusätzliche Instanzen von SQL Managed Instance zu unterstützen.

> [!IMPORTANT]
> Eine Subnetzgröße mit 16 IP-Adressen ist das absolute Minimum mit eingeschränktem Potenzial, bei dem ein Skalierungsvorgang wie eine Änderung der V-Kern-Größe nicht unterstützt wird. Die Auswahl eines Subnetzes mit dem Präfix „/27“ oder dem längsten Präfix wird dringend empfohlen.

## <a name="determine-subnet-size"></a>Bestimmen der Subnetzgröße

Wenn Sie mehrere Instanzen von SQL Managed Instance innerhalb des Subnetzes bereitstellen möchten und die Subnetzgröße optimieren müssen, führen Sie mithilfe der folgenden Parameter eine Berechnung durch:

- Azure verwendet fünf IP-Adressen im Subnetz für den eigenen Bedarf.
- Jede allgemeine Instanz benötigt zwei Adressen.
- Jede unternehmenskritisch Instanz benötigt vier Adressen.

**Beispiel:** Sie planen, mit drei universellen und zwei unternehmenskritischen Instanzen von SQL Managed Instance zu arbeiten. Dies bedeutet, dass Sie 5 + 3 * 2 + 2 * 4 = 19 IP-Adressen benötigen. Da IP-Adressbereiche in Zweierpotenzen definiert sind, benötigen Sie den IP-Adressbereich von 32 (2^5) IP-Adressen. Aus diesem Grund müssen Sie das Subnetz mit der Subnetzmaske /27 reservieren.

> [!IMPORTANT]
> Die oben gezeigte Berechnung wird mit weiteren Verbesserungen demnächst veralten.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Was ist SQL Managed Instance?](sql-managed-instance-paas-overview.md).
- Machen Sie sich mit der [Verbindungsarchitektur für SQL Managed Instance](connectivity-architecture-overview.md) vertraut.
- Erfahren Sie, wie Sie [ein VNET erstellen, in dem Sie SQL Managed Instance bereitstellen können](virtual-network-subnet-create-arm-template.md).
- Bei Problemen im Zusammenhang mit dem DNS lesen Sie [Konfigurieren eines benutzerdefinierten DNS](custom-dns-configure.md).
