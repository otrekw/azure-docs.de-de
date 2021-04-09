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
ms.openlocfilehash: 95d0e1ceb9e05ce58f388c3f88dc98b2cf6a0cc5
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105559586"
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
> [!NOTE]
> Der -NoProxy-Parameter wurde in PowerShell 6.0 eingeführt. Wenn Sie eine ältere PowerShell-Version nutzen, entfernen Sie -NoProxy aus dem Anforderungstext, und stellen Sie sicher, dass Sie beim Abrufen der IMDS-Informationen keinen Proxy verwenden. [Hier](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#proxies)erhalten Sie weitere Informationen.
> 
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