---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e4d20cd39d2a843ee1ab57a412ac668b3495fdb1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "67177915"
---
>[!NOTE]
>Ab dem 1. Juli 2018 wird die Unterstützung für TLS 1.0 und 1.1 vom Azure-VPN-Gateway entfernt. Das VPN-Gateway unterstützt dann nur noch TLS 1.2. Informationen zum Beibehalten der Unterstützung finden Sie unter [Updates zum Ermöglichen der Unterstützung von TLS 1.2](#tls1).

Die folgenden älteren Algorithmen werden ebenfalls am 1. Juli 2018 für TLS als veraltet markiert:

* RC4 (Rivest Cipher 4)
* DES (Data Encryption Algorithm)
* 3DES (Triple Data Encryption Algorithm)
* MD5 (Message Digest 5)

### <a name="how-do-i-enable-support-for-tls-12-in-windows-7-and-windows-81"></a><a name="tls1"></a>Wie aktiviere ich Unterstützung für TLS 1.2 unter Windows 7 und Windows 8.1?

[!INCLUDE [tls 1.2](vpn-gateway-tls-include.md)]
