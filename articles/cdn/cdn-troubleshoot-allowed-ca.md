---
title: Zulässige Zertifizierungsstellen für die Aktivierung von benutzerdefiniertem HTTPS für Azure CDN
description: Wenn Sie Ihr eigenes Zertifikat zur Aktivierung von HTTPS in einer benutzerdefinierten Domäne verwenden, müssen Sie für die Erstellung eine zulässige Zertifizierungsstelle (CA) verwenden.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: 81d2209c4b76db685e5a8d2625c84469d5c3dc43
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985846"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Zulässige Zertifizierungsstellen für die Aktivierung von benutzerdefiniertem HTTPS für Azure CDN

Sie müssen bestimmte Zertifikatanforderungen erfüllen, wenn Sie [HTTPS mit Ihrem eigenen Zertifikat](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#tlsssl-certificates) für eine benutzerdefinierte Azure Content Delivery Network-Domäne (CDN-Domäne) aktivieren. Das Standardprofil **Azure CDN von Microsoft** erfordert ein Zertifikat einer der in der folgenden Liste aufgeführten genehmigten Zertifizierungsstellen. Bei Verwendung eines Zertifikats einer nicht genehmigten Zertifizierungsstelle oder eines selbstsignierten Zertifikats wird die Anforderung abgelehnt. Die Profile vom Typ **Azure CDN Standard von Verizon** und **Azure CDN Premium von Verizon** akzeptieren jedes gültige Zertifikat jeder gültigen Zertifizierungsstelle.

> [!NOTE]
> Die Option zum Aktivieren von HTTPS mit Ihrem eigenen Zertifikat für die benutzerdefinierte Domäne ist *nicht* für Profile vom Typ **Azure CDN Standard von Akamai** verfügbar. 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

