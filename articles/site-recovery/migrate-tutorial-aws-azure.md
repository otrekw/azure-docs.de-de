---
title: Migrieren von virtuellen AWS-Computern zu Azure mit dem Azure Site Recovery-Dienst | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie unter Amazon Web Services (AWS) ausgeführte virtuelle Windows-Computer mithilfe von Azure Site Recovery zu Azure migrieren.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/27/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: dd91e99b45405cca10b9ddc2982674e72ad6bf86
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87281292"
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>Migrieren von AWS-VMs (Amazon Web Services) zu Azure

In diesem Artikel werden Optionen zum Migrieren von AWS-Instanzen (Amazon Web Services) zu Azure beschrieben.

## <a name="migrate-with-azure-migrate"></a>Migrieren mit Azure Migrate

Es wird empfohlen, den [Azure Migrate](../migrate/migrate-services-overview.md)-Dienst zu verwenden, um AWS-Instanzen zu Azure zu migrieren. Azure Migrate bietet einen zentralisierten Hub zum Bewerten und Migrieren von lokalen Computern zu Azure mithilfe von Azure Migrate, anderen Azure-Diensten und Tools von Drittanbietern.

[Erfahren Sie](../migrate/tutorial-migrate-aws-virtual-machines.md), wie Sie AWS-Instanzen mit Azure Migrate migrieren. 


## <a name="migrate-with-site-recovery"></a>Migrieren mit Site Recovery

Site Recovery sollte nur für die Notfallwiederherstellung und nicht für Migrationen verwendet werden.

Wenn Sie bereits Azure Site Recovery verwenden und weiterhin für die AWS-Migration verwenden möchten, führen Sie dieselben Schritte aus, die Sie zum Einrichten von [Notfallwiederherstellung von physischen Computern](physical-azure-disaster-recovery.md) verwenden.


> [!NOTE]
> Wenn Sie ein Failover für Notfallwiederherstellung ausführen, führen Sie als letzten Schritt ein Commit für das Failover aus. Beim Migrieren von AWS-Instanzen ist die Option **Commit** nicht relevant. Sie wählen stattdessen die Option **Migration** aus. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Lesen Sie Antworten auf gängige Fragen](../migrate/resources-faq.md) zu Azure Migrate.
