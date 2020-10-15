---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: acf1195616d45b155445604ef0204528e5f7ca03
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "67178728"
---
Für Daten während einer Übertragung:

- Standardmäßiges TLS 1.2 wird für Daten verwendet, die zwischen dem Gerät und Azure übertragen werden. Es erfolgt kein Fallback auf TLS 1.1 oder auf ältere Versionen. Agent-Kommunikation wird blockiert, wenn TLS 1.2 nicht unterstützt wird. TLS 1.2 wird auch für Portal- und SDK-Verwaltung benötigt.
- Wenn Clients über die lokale Webbenutzeroberfläche eines Browsers auf Ihr Gerät zugreifen, wird standardmäßiges TLS 1.2 als sicheres Standardprotokoll verwendet.

    - Es empfiehlt sich, Ihren Browser für die Verwendung von TLS 1.2 zu konfigurieren.
    - Sollte der Browser TLS 1.2 nicht unterstützen, können Sie TLS 1.1 oder TLS 1.0 verwenden.
- Sie sollten SMB 3.0 mit Verschlüsselung verwenden, um Daten zu schützen, wenn Sie diese von Ihren Datenservern kopieren.
