---
title: Zulässige Zertifizierungsstellen für Azure Front-Door Standard/Premium (Vorschau)
description: In diesem Artikel werden alle Zertifizierungsstellen aufgelistet, die zulässig sind, wenn Sie Ihr eigenes Zertifikat erstellen.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: troubleshooting
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: e31ad0734630fbd0b4960c26f8d562cea66ae675
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102434865"
---
# <a name="allowed-certificate-authorities-for-azure-front-door-standardpremium-preview"></a>Zulässige Zertifizierungsstellen für Azure Front-Door Standard/Premium (Vorschau)

Wenn Sie das HTTPS-Feature mit Ihrem eigenen Zertifikat für eine benutzerdefinierte Azure Front Door Standard/Premium-Domäne aktivieren, benötigen Sie eine zulässige Zertifizierungsstelle (ZS), um Ihr TLS/SSL-Zertifikat zu erstellen. Bei der Verwendung einer unzulässigen Zertifizierungsstelle oder eines selbstsignierten Zertifikats wird Ihre Anforderung abgelehnt.

[!INCLUDE [cdn-front-door-allowed-ca](../../../includes/cdn-front-door-allowed-ca.md)]
