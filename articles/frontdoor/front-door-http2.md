---
title: Azure Front Door – Unterstützung für HTTP2 | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zur HTTP/2-Unterstützung in Azure Front Door.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 8a3ae8065553b34a72528cb0f2681e327dc90097
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985183"
---
# <a name="http2-support-in-azure-front-door"></a>HTTP/2-Unterstützung in Azure Front Door

Derzeit ist die HTTP/2-Unterstützung für alle Azure Front Door-Konfigurationen aktiviert. Es ist keine weitere Aktion vom Kunden erforderlich.

HTTP/2 ist eine größere Überarbeitung von HTTP/1.1. Sie bietet eine höhere Webleistung, kürzere Antwortzeiten und eine verbesserte Benutzeroberfläche, während gleichzeitig die gewohnten HTTP-Methoden, Statuscodes und Semantik beibehalten werden. Obwohl HTTP/2 darauf ausgelegt ist, unter HTTP und HTTPS zu funktionieren, unterstützen viele Clientwebbrowser HTTP/2 nur über TLS (Transport Layer Security).

> [!NOTE]
> Die Unterstützung des HTTP/2-Protokolls ist nur für Anforderungen von Clients an Front Door verfügbar. Die Kommunikation von Front Door zu Back-Ends im Back-End-Pool erfolgt über HTTP/1.1. 

### <a name="http2-benefits"></a>Vorteile von HTTP/2

HTTP/2 bietet unter anderem folgende Vorteile:

*   **Multiplexing und Parallelität**

    Bei der Verwendung von HTTP 1.1 benötigt man für die Anforderung mehrerer Ressourcen mehrere TCP-Verbindungen, und mit jeder Verbindung ist Leistungsaufwand verbunden. HTTP/2 ermöglicht die Anforderung mehrerer Ressourcen über eine einzelne TCP-Verbindung.

*   **Header-Komprimierung**

    Durch das Komprimieren von HTTP-Headern für bereitgestellte Ressourcen wird die Zeit bei der Übertragung erheblich reduziert.

*   **Datenstrom-Abhängigkeiten**

    Datenstromabhängigkeiten ermöglichen dem Client, dem Server anzugeben, welche Ressourcen Vorrang haben.


## <a name="http2-browser-support"></a>HTTP/2-Browserunterstützung

Alle wichtigen Browser haben HTTP/2-Unterstützung in ihren aktuellen Versionen implementiert. Nicht unterstützte Browser führen ein automatisches Fallback auf HTTP/1.1 durch.

|Browser|Mindestversion|
|-------------|------------|
|Microsoft Edge| 12|
|Google Chrome| 43|
|Mozilla Firefox| 38|
|Opera| 32|
|Safari| 9|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu HTTP/2 finden Sie in den folgenden Ressourcen:

- [HTTP/2-Homepage](https://http2.github.io/) (in englischer Sprache)
- [Häufig gestellte Fragen zu HTTP/2 (offiziell)](https://http2.github.io/faq/) (in englischer Sprache)
- Erfahren Sie mehr über das [Erstellen einer Front Door-Instanz](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).
