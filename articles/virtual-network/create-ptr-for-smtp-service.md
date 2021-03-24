---
title: Konfigurieren von Reverse-Lookupzonen für eine SMTP-Bannerüberprüfung
titlesuffix: Azure Virtual Network
description: Beschreibt das Konfigurieren von Reverse-Lookupzonen für eine SMTP-Bannerüberprüfung in Azure
services: virtual-network
documentationcenter: virtual-network
author: genlin
manager: dcscontentpm
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: c8ffadb8d54db0c2a99dc12e45b5990155a0505e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "86135057"
---
# <a name="configure-reverse-lookup-zones-for-an-smtp-banner-check"></a>Konfigurieren von Reverse-Lookupzonen für eine SMTP-Bannerüberprüfung

Dieser Artikel beschreibt die Verwendung einer Reverse-Lookupzone in Azure DNS und das Erstellen eines Reverse-DNS-Eintrags (PTR) für die SMTP-Bannerüberprüfung.

## <a name="symptom"></a>Symptom

Wenn Sie einen SMTP-Server in Microsoft Azure hosten, erhalten Sie eventuell beim Senden oder Empfangen einer Nachricht von Remotemailservern die folgende Fehlermeldung:

**554: Kein PTR-Eintrag**

## <a name="solution"></a>Lösung

Für eine virtuelle IP-Adresse in Azure werden die Reverse-Einträge in Domänenzonen im Besitz von Microsoft erstellt, nicht in benutzerdefinierten Domänenzonen.

Um PTR-Einträge in Zonen im Besitz von Microsoft zu konfigurieren, verwenden Sie die -ReverseFqdn-Eigenschaft für die Ressource PublicIpAddress. Weitere Informationen finden Sie unter [Konfigurieren von Reverse-DNS für in Azure gehostete Dienste](../dns/dns-reverse-dns-for-azure-services.md).

Achten Sie beim Konfigurieren der PTR-Einträge darauf, dass die IP-Adresse und der Reverse-FQDN zum Abonnement gehören. Wenn Sie versuchen, einen Reverse-FQDN festzulegen, der nicht dem Abonnement angehört, erhalten Sie die folgende Fehlermeldung:

```output
Set-AzPublicIpAddress : ReverseFqdn mail.contoso.com that PublicIPAddress ip01 is trying to use does not belong to subscription <Subscription ID>. One of the following conditions need to be met to establish ownership:
                    
1) ReverseFqdn matches fqdn of any public ip resource under the subscription;
2) ReverseFqdn resolves to the fqdn (through CName records chain) of any public ip resource under the subscription;
3) It resolves to the ip address (through CName and A records chain) of a static public ip resource under the subscription.
```

Wenn Sie Ihr SMTP-Banner manuell in unseren standardmäßigen Reverse-FQDN ändern, kann beim Remotemailserver trotzdem ein Fehler auftreten, da er möglicherweise erwartet, dass der SMTP-Bannerhost mit dem MX-Eintrag für die Domäne übereinstimmt.
