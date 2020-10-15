---
title: Aktivieren von cookiebasierter Affinität mit Application Gateway
description: Dieser Artikel enthält Informationen zum Aktivieren von cookiebasierter Affinität mit Application Gateway.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 3af2705fedbb9c19d4f128e8e997d3fa73f8b5a7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84807970"
---
# <a name="enable-cookie-based-affinity-with-an-application-gateway"></a>Aktivieren von cookiebasierter Affinität mit Application Gateway
Wie in der [Azure Application Gateway-Dokumentation](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#http-settings) beschrieben, unterstützt Application Gateway cookiebasierte Affinität, d.h. das Gateway kann nachfolgenden Datenverkehr von einer Benutzersitzung an den gleichen Server zur Verarbeitung weiterleiten.

## <a name="example"></a>Beispiel
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: guestbook
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/cookie-based-affinity: "true"
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: frontend
          servicePort: 80
```
