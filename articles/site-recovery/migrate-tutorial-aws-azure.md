---
title: Migrieren virtueller AWS-Computer zu Azure mit Azure Migrate
description: In diesem Artikel werden Optionen zum Migrieren von AWS-Instanzen zu Azure beschrieben. Dazu wird Azure Migrate empfohlen.
services: site-recovery
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/27/2019
ms.custom: MVC
ms.openlocfilehash: 6c3f20f0fa3bc6ad41e76626d3cb02ec1c0b96e9
ms.sourcegitcommit: d63f15674f74d908f4017176f8eddf0283f3fac8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "106581341"
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>Migrieren von AWS-VMs (Amazon Web Services) zu Azure

In diesem Artikel werden Optionen zum Migrieren von AWS-Instanzen (Amazon Web Services) zu Azure beschrieben.

## <a name="migrate-with-azure-migrate"></a>Migrieren mit Azure Migrate

Es wird empfohlen, den [Azure Migrate](../migrate/migrate-services-overview.md)-Dienst zu verwenden, um AWS EC2-Instanzen zu Azure zu migrieren. Azure Migrate ist speziell für die Servermigration konzipiert. Azure Migrate bietet einen zentralen Hub für die Ermittlung, Bewertung und Migration von lokalen Computern zu Azure.

[Erfahren Sie](../migrate/tutorial-migrate-aws-virtual-machines.md), wie Sie AWS-Instanzen mit Azure Migrate migrieren. 


## <a name="migrate-with-site-recovery"></a>Migrieren mit Site Recovery

Site Recovery sollte nur für die Notfallwiederherstellung und nicht für Migrationen verwendet werden.

Wenn Sie bereits Azure Site Recovery verwenden und weiterhin für die AWS-Migration verwenden möchten, führen Sie dieselben Schritte aus, die Sie zum Einrichten von [Notfallwiederherstellung von physischen Computern](physical-azure-disaster-recovery.md) verwenden.


> [!NOTE]
> Wenn Sie ein Failover für Notfallwiederherstellung ausführen, führen Sie als letzten Schritt ein Commit für das Failover aus. Beim Migrieren von AWS-Instanzen ist die Option **Commit** nicht relevant. Sie wählen stattdessen die Option **Migration** aus. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Lesen Sie Antworten auf gängige Fragen](../migrate/resources-faq.md) zu Azure Migrate.
