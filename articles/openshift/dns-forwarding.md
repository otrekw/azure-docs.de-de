---
title: Konfigurieren der DNS-Weiterleitung für Azure Red Hat OpenShift 4
description: Konfigurieren der DNS-Weiterleitung für Azure Red Hat OpenShift 4
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: azure-redhat-openshift
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: 68cc5dbee5b96472d11bdfb3a8bf35d118638dcd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100633868"
---
# <a name="configure-dns-forwarding-on-an-azure-red-hat-openshift-4-cluster"></a>Konfigurieren der DNS-Weiterleitung für einen Azure Red Hat OpenShift 4-Cluster

Zum Konfigurieren der DNS-Weiterleitung auf einem Azure Red Hat OpenShift-Cluster müssen Sie den DNS-Operator ändern. Diese Änderung ermöglicht es Ihren Anwendungspods, die innerhalb des Clusters ausgeführt werden, Namen aufzulösen, die auf einem privaten DNS-Server außerhalb des Clusters gehostet werden. Diese Schritte für OpenShift 4.6 sind [hier](https://docs.openshift.com/container-platform/4.6/networking/dns-operator.html) dokumentiert.

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
Weitere Informationen zur DNS-Weiterleitung für OpenShift 4.6 finden Sie [hier](https://docs.openshift.com/container-platform/4.6/networking/dns-operator.html).