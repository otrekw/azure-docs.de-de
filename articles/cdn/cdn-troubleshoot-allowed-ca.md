---
title: Zulässige Zertifizierungsstellen für die Aktivierung von benutzerdefiniertem HTTPS für Azure CDN
description: Wenn Sie Ihr eigenes Zertifikat zur Aktivierung von HTTPS in einer benutzerdefinierten Domäne verwenden, müssen Sie für die Erstellung eine zulässige Zertifizierungsstelle (CA) verwenden.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/18/2018
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: 29b6cb25e021e86ce6663b4db5c89217aaf70a37
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84887400"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Zulässige Zertifizierungsstellen für die Aktivierung von benutzerdefiniertem HTTPS für Azure CDN

Sie müssen bestimmte Zertifikatanforderungen erfüllen, wenn Sie [HTTPS mit Ihrem eigenen Zertifikat](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#tlsssl-certificates) für eine benutzerdefinierte Azure Content Delivery Network-Domäne (CDN-Domäne) aktivieren. Das Standardprofil **Azure CDN von Microsoft** erfordert ein Zertifikat einer der in der folgenden Liste aufgeführten genehmigten Zertifizierungsstellen. Bei Verwendung eines Zertifikats einer nicht genehmigten Zertifizierungsstelle oder eines selbstsignierten Zertifikats wird die Anforderung abgelehnt. Die Profile vom Typ **Azure CDN Standard von Verizon** und **Azure CDN Premium von Verizon** akzeptieren jedes gültige Zertifikat jeder gültigen Zertifizierungsstelle.

> [!NOTE]
> Die Option zum Aktivieren von HTTPS mit Ihrem eigenen Zertifikat für die benutzerdefinierte Domäne ist *nicht* für Profile vom Typ **Azure CDN Standard von Akamai** verfügbar. 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

