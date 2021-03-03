---
title: Abrufen von Lastenausgleichs-Metadaten mithilfe des Azure Instance Metadata Service (IMDS)
titleSuffix: Azure Load Balancer
description: Erfahren Sie, wie Sie Metadaten zum Lastenausgleich mithilfe des Azure Instance Metadata Service abrufen.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: how-to
ms.date: 02/12/2021
ms.author: allensu
ms.openlocfilehash: 5196b03ccd513e4afd93b8b8fcf18f7c2580024a
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100519217"
---
# <a name="retrieve-load-balancer-metadata-using-the-azure-instance-metadata-service-imds"></a>Abrufen von Lastenausgleichs-Metadaten mithilfe des Azure Instance Metadata Service (IMDS)

## <a name="prerequisites"></a>Voraussetzungen

* Verwenden Sie für Ihre Anforderung die [neueste API-Version](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#supported-api-versions).

## <a name="sample-request-and-response"></a>Beispielanforderung und -antwort
> [!IMPORTANT]
> In diesem Beispiel werden Proxys umgangen. Beim Abfragen von IMDS **müssen** Sie Proxys umgehen. Weitere Informationen finden Sie unter [Proxys](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#proxies).
### <a name="windows"></a>[Windows](#tab/windows/)

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254:80/metadata/loadbalancer?api-version=2020-10-01" | ConvertTo-Json
```

### <a name="linux"></a>[Linux](#tab/linux/)

```bash
curl -H "Metadata:true" --noproxy "*" "http://169.254.169.254:80/metadata/loadbalancer?api-version=2020-10-01"
```

---
### <a name="sample-response"></a>Beispiel für eine Antwort

```json
{
   "loadbalancer": {
    "publicIpAddresses":[
      {
         "frontendIpAddress":"51.0.0.1",
         "privateIpAddress":"10.1.0.4"
      }
   ],
   "inboundRules":[
      {
         "frontendIpAddress":"50.0.0.1",
         "protocol":"tcp",
         "frontendPort":80,
         "backendPort":443,
         "privateIpAddress":"10.1.0.4"
      },
      {
         "frontendIpAddress":"2603:10e1:100:2::1:1",
         "protocol":"tcp",
         "frontendPort":80,
         "backendPort":443,
         "privateIpAddress":"ace:cab:deca:deed::1"
      }
   ],
   "outboundRules":[
      {
         "frontendIpAddress":"50.0.0.1",
         "privateIpAddress":"10.1.0.4"
      },
      {
         "frotendIpAddress":"2603:10e1:100:2::1:1",
         "privateIpAddress":"ace:cab:deca:deed::1"
      }
    ]
   }
}

```

## <a name="next-steps"></a>Nächste Schritte
[Häufige Fehlercodes und Schritte zur Problembehandlung](troubleshoot-load-balancer-imds.md)

Weitere Informationen zu [Azure Instance Metadata Service](../virtual-machines/windows/instance-metadata-service.md)

[Abrufen aller Metadaten für eine Instanz](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#access-azure-instance-metadata-service)

[Bereitstellen einer Load Balancer Standard-Instanz](quickstart-load-balancer-standard-public-portal.md)

