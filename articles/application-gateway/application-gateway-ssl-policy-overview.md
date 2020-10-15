---
title: Übersicht über die TLS-Richtlinie für Azure Application Gateway
description: Erfahren Sie, wie Sie die TLS-Richtlinie für Azure Application Gateway konfigurieren und den Ver- und Entschlüsselungsaufwand für eine Back-End-Serverfarm verringern.
services: application gateway
author: amsriva
ms.service: application-gateway
ms.topic: conceptual
ms.date: 11/16/2019
ms.author: amsriva
ms.openlocfilehash: 16c6dd28d47573c2ad5b0d5a331b0dc48e7aacef
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85253629"
---
# <a name="application-gateway-tls-policy-overview"></a>TLS-Richtlinienübersicht für Azure Application Gateway

Mit Azure Application Gateway können Sie die TLS/SSL-Zertifikatverwaltung zentralisieren sowie den Ver- und Entschlüsselungsaufwand für eine Back-End-Serverfarm verringern. Diese zentralisierte TLS-Behandlung ermöglicht auch die Angabe einer zentralen TLS-Richtlinie, die auf die Sicherheitsanforderungen Ihrer Organisation abgestimmt ist. Dadurch können Sie sowohl Complianceanforderungen erfüllen als auch Sicherheitsrichtlinien und empfohlene Vorgehensweisen umsetzen.

Mit der TLS-Richtlinie können Sie die TLS-Protokollversion sowie Verschlüsselungssammlungen und die Reihenfolge steuern, in der Verschlüsselungen im Rahmen des TLS-Handshakes verwendet werden. Application Gateway bietet zwei Mechanismen für das Steuern der TLS-Richtlinie. Sie können eine vordefinierte Richtlinie oder eine benutzerdefinierte Richtlinie verwenden.

## <a name="predefined-tls-policy"></a>Vordefinierte TLS-Richtlinie

Application Gateway verfügt über drei vordefinierte Sicherheitsrichtlinien. Sie können Ihr Gateway mit diesen Richtlinien konfigurieren, um das gewünschte Maß an Sicherheit zu erhalten. Die Richtliniennamen werden mit dem Jahr und Monat versehen, in dem sie konfiguriert wurden. Jede Richtlinie bietet verschiedene TLS-Protokollversionen und Verschlüsselungssammlungen. Verwenden Sie möglichst die neuesten TLS-Richtlinien, um ein Höchstmaß an TLS-Sicherheit zu gewährleisten.

### <a name="appgwsslpolicy20150501"></a>AppGwSslPolicy20150501

|Eigenschaft  |Wert  |
|---|---|
|Name     | AppGwSslPolicy20150501        |
|MinProtocolVersion     | TLSv1_0        |
|Standard| True (wenn keine vordefinierte Richtlinie angegeben ist) |
|CipherSuites     |TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_DHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_DHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_DHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_DHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA<br>TLS_DHE_DSS_WITH_AES_256_CBC_SHA256<br>TLS_DHE_DSS_WITH_AES_128_CBC_SHA256<br>TLS_DHE_DSS_WITH_AES_256_CBC_SHA<br>TLS_DHE_DSS_WITH_AES_128_CBC_SHA<br>TLS_RSA_WITH_3DES_EDE_CBC_SHA<br>TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA |
  
### <a name="appgwsslpolicy20170401"></a>AppGwSslPolicy20170401
  
|Eigenschaft  |Wert  |
|   ---      |  ---       |
|Name     | AppGwSslPolicy20170401        |
|MinProtocolVersion     | TLSv1_1        |
|Standard| False |
|CipherSuites     |TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA |
  
### <a name="appgwsslpolicy20170401s"></a>AppGwSslPolicy20170401S

|Eigenschaft  |Wert  |
|---|---|
|Name     | AppGwSslPolicy20170401S        |
|MinProtocolVersion     | TLSv1_2        |
|Standard| False |
|CipherSuites     |TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 <br>    TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 <br>    TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA <br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA <br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA<br> |

## <a name="custom-tls-policy"></a>Benutzerdefinierte TLS-Richtlinie

Wenn eine vordefinierte TLS-Richtlinie für Ihre Anforderungen konfiguriert werden muss, müssen Sie eine eigene benutzerdefinierte TLS-Richtlinie definieren. Mit einer benutzerdefinierten TLS-Richtlinie haben Sie die uneingeschränkte Kontrolle über die zu unterstützende Mindestversion des TLS-Protokolls sowie über unterstützte Verschlüsselungssammlungen und deren jeweilige Priorität.
 
### <a name="tlsssl-protocol-versions"></a>TLS/SSL-Protokollversionen

* SSL 2.0 und 3.0 sind für alle Anwendungsgateways standardmäßig deaktiviert. Diese Protokollversionen sind nicht konfigurierbar.
* Bei einer benutzerdefinierten TLS-Richtlinie können Sie ein beliebiges der drei folgenden Protokolle als TLS-Mindestprotokollversion für Ihr Gateway auswählen: TLSv1_0, TLSv1_1 und TLSv1_2.
* Wenn keine TLS-Richtlinie definiert wird, werden alle drei Protokolle („TLSv1_0“, „TLSv1_1“ und „TLSv1_2“) aktiviert.

### <a name="cipher-suites"></a>Verschlüsselungssammlungen

Application Gateway unterstützt die folgenden Verschlüsselungssammlungen. Aus diesen können Sie Ihre benutzerdefinierte Richtlinie auswählen. Die Reihenfolge der Verschlüsselungssammlungen bestimmt ihre Priorität bei der TLS-Aushandlung.


- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

> [!NOTE]
> Die für die Verbindung verwendeten TLS-Verschlüsselungssammlungen basieren auf dem Typ des verwendeten Zertifikats. In Verbindungen zwischen Client und Anwendungsgateways basieren die verwendeten Verschlüsselungssammlungen auf dem Typ des Serverzertifikats auf dem Anwendungsgatewaylistener. In Verbindungen zwischen Anwendungsgateways und Back-End-Pools basieren die verwendeten Verschlüsselungssammlungen auf dem Typ des Serverzertifikats auf den Back-End-Poolservern.

## <a name="known-issue"></a>Bekanntes Problem
Application Gateway v2 unterstützt derzeit nicht die folgenden Verschlüsselungen nicht:
- DHE-RSA-AES128-GCM-SHA256
- DHE-RSA-AES128-SHA
- DHE-RSA-AES256-GCM-SHA384
- DHE-RSA-AES256-SHA
- DHE-DSS-AES128-SHA256
- DHE-DSS-AES128-SHA
- DHE-DSS-AES256-SHA256
- DHE-DSS-AES256-SHA

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren einer TLS-Richtlinie finden Sie unter [Konfigurieren von TLS-Richtlinienversionen und Verschlüsselungssammlungen für Application Gateway](application-gateway-configure-ssl-policy-powershell.md).
