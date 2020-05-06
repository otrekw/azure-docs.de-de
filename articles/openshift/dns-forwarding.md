---
title: Konfigurieren der DNS-Weiterleitung für Azure Red Hat OpenShift 4
description: Konfigurieren der DNS-Weiterleitung für Azure Red Hat OpenShift 4
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: 6784c69c049945f3bd4977ecd647d57ec59bf9f1
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82232631"
---
# <a name="configure-dns-forwarding-on-an-azure-red-hat-openshift-4-cluster"></a>Konfigurieren der DNS-Weiterleitung für einen Azure Red Hat OpenShift 4-Cluster

Zum Konfigurieren der DNS-Weiterleitung auf einem Azure Red Hat OpenShift-Cluster müssen Sie den DNS-Operator ändern. Diese Änderung ermöglicht es Ihren Anwendungspods, die innerhalb des Clusters ausgeführt werden, Namen aufzulösen, die auf einem privaten DNS-Server außerhalb des Clusters gehostet werden. Diese Schritte sind für OpenShift 4.3 [hier](https://docs.openshift.com/container-platform/4.3/networking/dns-operator.html) dokumentiert.

Wenn Sie z. B. alle DNS-Anforderungen für *.example.com weiterleiten möchten, damit sie von einem DNS-Server (192.168.100.10) aufgelöst werden, können Sie die Operatorkonfiguration bearbeiten, indem Sie Folgendes ausführen:
 
```bash
oc edit dns.operator/default
```
 
Dadurch wird ein Editor gestartet, und Sie können `spec: {}` durch Folgendes ersetzen:
 
 ```yaml
spec:
  servers:
  - forwardPlugin:
      upstreams:
      - 192.168.100.10
    name: example-dns
    zones:
    - example.com
```

Speichern Sie die Datei, und beenden Sie den Editor.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur DNS-Weiterleitung für OpenShift 4.3 finden Sie [hier](https://docs.openshift.com/container-platform/4.3/networking/dns-operator.html).