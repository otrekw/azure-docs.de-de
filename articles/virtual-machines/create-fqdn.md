---
title: Erstellen eines FQDN für VMs im Azure-Portal
description: Erfahren Sie, wie Sie im Azure-Portal einen vollqualifizierten Domänennamen (FQDN) für eine VM erstellen.
author: cynthn
ms.service: virtual-machines
ms.subservice: networking
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 1/12/2020
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: aadf4000da4ab88a3c3b4dee37d2179eb49d39d7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98132063"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>Erstellen eines vollqualifizierten Domänennamens im Azure-Portal für eine Linux-VM

Beim Erstellen eines virtuellen Computers (Virtual Machine, VM) im [Azure-Portal](https://portal.azure.com) wird automatisch eine öffentliche IP als Ressource für den virtuellen Computer erstellt. Mit dieser IP-Adresse greifen Sie per Remotezugriff auf den virtuellen Computer zu. Obwohl das Portal standardmäßig keinen [vollqualifizierten Domänennamen](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (Fully Qualified Domain Name, FQDN) erstellt, können Sie nach der Erstellung des virtuellen Computers einen solchen hinzufügen. Dieser Artikel demonstriert die einzelnen Schritte, um einen DNS-Namen oder einen FQDN zu erstellen. 

## <a name="create-a-fqdn"></a>Erstellen eines FQDN
In diesem Artikel wird davon ausgegangen, dass Sie bereits einen virtuellen Computer erstellt haben. Bei Bedarf können Sie im Portal eine [Linux](./linux/quick-create-portal.md)- oder [Windows](./windows/quick-create-portal.md)-VM erstellen. Sobald Ihr virtueller Computer eingerichtet ist und ausgeführt wird, gehen Sie folgendermaßen vor:


1. Wählen Sie im Portal Ihren virtuellen Computer aus. 
1. Wählen Sie im linken Menü die Option **Konfiguration** aus.
1. Geben Sie unter **DNS-Namensbezeichnung** das Präfix ein, das Sie verwenden möchten.
1. Wählen Sie im oberen Bereich der Seite **Speichern** aus.
1. Wechseln Sie zum VM-Übersichtsblatt zurück, indem Sie im linken Menü **Übersicht** auswählen. 
1. Überprüfen Sie, ob der *DNS-Name* richtig angezeigt wird. 

## <a name="next-steps"></a>Nächste Schritte

Sie können auch DNS-Zonen mithilfe von [Azure DNS-Zonen](../dns/dns-getstarted-portal.md) verwalten.

