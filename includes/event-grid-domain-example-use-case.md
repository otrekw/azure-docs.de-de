---
title: include file
description: include file
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 03/04/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 2459cf6c5055dcde83dccf37addc160aeeaa64ad
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102198654"
---
Ereignisdomänen lassen sich am einfachsten an einem Beispiel veranschaulichen. Angenommen, Sie betreiben Contoso Construction Machinery, ein Unternehmen, das Traktoren, Bagger und anderes schweres Gerät herstellt. Als Teil des Geschäftsbetriebs übermitteln Sie Echtzeitinformationen zur Ausrüstungswartung, zur Systemintegrität und zu Vertragsaktualisierungen an Kunden. Alle diese Informationen werden an verschiedene Endpunkte weitergeleitet (z.B. an Ihre App, an Kundenendpunkte und andere Infrastrukturen, die Kunden eingerichtet haben).

Ereignisdomänen ermöglichen es Ihnen, Contoso Construction Machinery als eine einzige Ereigniseinheit zu modellieren. Jeder Ihrer Kunden wird als ein Thema in der Domäne dargestellt. Authentifizierung und Autorisierung werden mit Azure Active Directory durchgeführt. Jeder Ihrer Kunden kann sein Thema abonnieren und sich seine Ereignisse zustellen lassen. Verwalteter Zugriff über die Ereignisdomäne stellt sicher, dass die Kunden nur auf ihr jeweiliges Thema zugreifen können.

Außerdem erhalten Sie einen einzigen Endpunkt, in dem Sie alle Ihre Kundenereignisse veröffentlichen können. Event Grid sorgt dafür, dass jedes Thema nur die Ereignisse kennt, die sich auf den Mandanten beziehen.

:::image type="content" source="./media/event-grid-domain-example-use-case/contoso-construction-example.png" alt-text="Contoso Construction-Beispiel" lightbox="./media/event-grid-domain-example-use-case/contoso-construction-example.png":::