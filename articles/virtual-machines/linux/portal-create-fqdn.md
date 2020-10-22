---
title: Erstellen eines FQDN für VMs im Azure-Portal
description: Erfahren Sie, wie Sie einen vollqualifizierten Domänennamen (FQDN) für einen virtuellen Linux-Computer im Azure-Portal erstellen.
author: cynthn
ms.service: virtual-machines
ms.subservice: networking
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: deae22ad0c763e48df053d19beefba6054ed2767
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91331470"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>Erstellen eines vollqualifizierten Domänennamens im Azure-Portal für eine Linux-VM

Beim Erstellen eines virtuellen Computers (Virtual Machine, VM) im [Azure-Portal](https://portal.azure.com) wird automatisch eine öffentliche IP als Ressource für den virtuellen Computer erstellt. Mit dieser IP-Adresse greifen Sie per Remotezugriff auf den virtuellen Computer zu. Obwohl das Portal standardmäßig keinen [vollqualifizierten Domänennamen](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (Fully Qualified Domain Name, FQDN) erstellt, können Sie nach der Erstellung des virtuellen Computers einen solchen hinzufügen. Dieser Artikel demonstriert die einzelnen Schritte, um einen DNS-Namen oder einen FQDN zu erstellen.

## <a name="create-a-fqdn"></a>Erstellen eines FQDN
In diesem Artikel wird davon ausgegangen, dass Sie bereits einen virtuellen Computer erstellt haben. Bei Bedarf können Sie [einen virtuellen Computer im Portal](quick-create-portal.md) oder [mit der Azure-CLI](quick-create-cli.md) erstellen. Sobald Ihr virtueller Computer eingerichtet ist und ausgeführt wird, gehen Sie folgendermaßen vor:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Sie können jetzt mithilfe dieses DNS-Namens eine Remoteverbindung mit dem virtuellen Computer herstellen, z.B. mit `ssh azureuser@mydns.westus.cloudapp.azure.com`.

## <a name="next-steps"></a>Nächste Schritte
Ihr virtueller Computer verfügt nun über eine öffentliche IP und einen DNS-Namen. Als Nächstes können Sie allgemeine Anwendungsframeworks oder Dienste bereitstellen, z.B. nginx, MongoDB, Docker usw.

Außerdem können Sie sich ausführlicher [über die Verwendung von Resource Manager informieren](../../azure-resource-manager/management/overview.md), um Hinweise zum Erstellen Ihrer Azure-Bereitstellungen zu erhalten.

