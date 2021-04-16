---
title: Erstellen eines FQDN für VMs im Azure-Portal
description: Erfahren Sie, wie Sie im Azure-Portal einen vollqualifizierten Domänennamen (FQDN) für eine VM erstellen.
author: cynthn
ms.service: virtual-machines
ms.subservice: networking
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 04/01/2021
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b010070b7a45c24037c6de4648574c01b017d759
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107107396"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-linux-vm"></a>Erstellen eines vollqualifizierten Domänennamens im Azure-Portal für eine Linux-VM

Beim Erstellen eines virtuellen Computers (Virtual Machine, VM) im [Azure-Portal](https://portal.azure.com) wird automatisch eine öffentliche IP als Ressource für den virtuellen Computer erstellt. Mit dieser IP-Adresse greifen Sie per Remotezugriff auf den virtuellen Computer zu. Obwohl das Portal standardmäßig keinen [vollqualifizierten Domänennamen](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (Fully Qualified Domain Name, FQDN) erstellt, können Sie nach der Erstellung des virtuellen Computers einen solchen hinzufügen. Dieser Artikel demonstriert die einzelnen Schritte, um einen DNS-Namen oder einen FQDN zu erstellen. 

## <a name="create-a-fqdn"></a>Erstellen eines FQDN
In diesem Artikel wird davon ausgegangen, dass Sie bereits einen virtuellen Computer erstellt haben. Bei Bedarf können Sie im Portal eine [Linux](./linux/quick-create-portal.md)- oder [Windows](./windows/quick-create-portal.md)-VM erstellen. Sobald Ihr virtueller Computer eingerichtet ist und ausgeführt wird, gehen Sie folgendermaßen vor:


1. Wählen Sie im Portal Ihren virtuellen Computer aus. 
1. Wählen Sie im Menü auf der linken Seite die Option **Eigenschaften** aus.
1. Wählen Sie unter **Öffentliche IP-Adresse/DNS-Namensbezeichnung** Ihre IP-Adresse aus.
2. Geben Sie unter **DNS-Namensbezeichnung** das Präfix ein, das Sie verwenden möchten.
3. Wählen Sie im oberen Bereich der Seite **Speichern** aus.
4. Wählen Sie im Menü auf der linken Seite **Übersicht** aus, um zum VM-Übersichtsblatt zurückzukehren.
5. Überprüfen Sie, ob der **DNS-Name** richtig angezeigt wird. 

## <a name="next-steps"></a>Nächste Schritte

Sie können auch DNS-Zonen mithilfe von [Azure DNS-Zonen](../dns/dns-getstarted-portal.md) verwalten.

