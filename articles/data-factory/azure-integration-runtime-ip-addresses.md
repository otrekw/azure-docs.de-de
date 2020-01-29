---
title: IP-Adressen von Azure Integration Runtime
description: Erfahren Sie, welche IP-Adressen Sie für eingehenden Datenverkehr zulassen müssen, damit Sie Firewalls zum Sichern des Netzwerkzugriffs auf Datenspeicher ordnungsgemäß konfigurieren können.
services: data-factory
ms.author: abnarain
author: nabhishek
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/06/2020
ms.openlocfilehash: d64475ce524a3e859ed68a46552fedf30068f71d
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514781"
---
# <a name="azure-integration-runtime-ip-addresses"></a>IP-Adressen von Azure Integration Runtime

Die von Azure Integration Runtime verwendeten IP-Adressen richten sich nach der Region, in der sich Ihre Azure Integration Runtime-Instanz befindet. *Alle* Azure Integration Runtime-Instanzen in ein und derselben Region verwenden dieselben IP-Adressbereiche.

> [!IMPORTANT]  
> Datenflüsse verwenden diese IPs derzeit nicht. 
>
> Sie können diese IP-Adressbereiche für Datenverschiebungen, Pipeline und externe Aktivitäten verwenden. Diese IP-Adressbereiche können auch verwendet werden, um in Datenspeichern, Netzwerksicherheitsgruppen oder Firewalls Whitelists für eingehenden Datenverkehr von Azure Integration Runtime einzurichten. 

## <a name="azure-integration-runtime-ip-addresses-specific-regions"></a>IP-Adressen von Azure Integration Runtime: Bestimmte Regionen

Lassen Sie Datenverkehr von den IP-Adressen zu, die für Azure Integration Runtime in der jeweiligen Azure-Region aufgeführt sind, in der sich Ihre Ressourcen befinden:

|                | Region              | IP-Adressen                                                 |
| -------------- | ------------------- | ------------------------------------------------------------ |
| Asia           | Asien, Osten           | 20.189.104.128/25, </br>20.189.106.0/26, </br>13.75.39.112/28 |
| &nbsp;         | Asien, Südosten      | 20.43.128.128/25, </br>20.43.130.0/26, </br>40.78.236.176/28 |
| Australien      | Australien (Osten)      | 20.37.193.0/25,</br>20.37.193.128/26,</br>13.70.74.144/28    |
| &nbsp;         | Australien, Südosten | 20.42.225.0/25,</br>20.42.225.128/26,</br>13.77.53.160/28    |
| Brasilien         | Brasilien Süd        | 191.235.224.128/25,</br>191.235.225.0/26,</br>191.233.205.160/28 |
| Canada         | Kanada, Mitte      | 52.228.80.128/25,</br>52.228.81.0/26,</br>13.71.175.80/28    |
| Europa         | Nordeuropa        | 20.38.82.0/23,</br>20.38.80.192/26,</br>13.69.230.96/28      |
| &nbsp;         | Europa, Westen         | 40.74.26.0/23,</br>40.74.24.192/26,</br>13.69.67.192/28      |
| Frankreich         | Frankreich, Mitte      | 20.43.40.128/25,</br>20.43.41.0/26,</br>40.79.132.112/28     |
| Indien          | Indien, Mitte       | 52.140.104.128/25,</br>52.140.105.0/26,</br>20.43.121.48/28  |
| Japan          | Japan, Osten          | 20.43.64.128/25,</br>20.43.65.0/26,</br>13.78.109.192/28     |
| Korea          | Korea, Mitte       | 20.41.64.128/25,</br>20.41.65.0/26,</br>52.231.20.64/28      |
| Südafrika   | Südafrika, Norden  | 102.133.124.104/29,</br>102.133.216.128/25,</br>102.133.217.0/26 |
| United Kingdom | UK, Süden            | 51.104.24.128/25,</br>51.104.25.0/26,</br>51.104.9.32/28     |
| USA  | USA (Mitte)          | 20.37.154.0/23,</br>20.37.156.0/26,</br>20.44.10.64/28       |
|                | East US             | 20.42.2.0/23,</br>20.42.4.0/26,</br>40.71.14.32/28           |
|                | USA (Ost 2)            | 20.41.2.0/23,</br>20.41.4.0/26,</br>20.44.17.80/28           |
|                | USA, Osten 2 (EUAP)      | 20.39.8.128/26,</br>20.39.8.96/27,</br>40.75.35.144/28       |
|                | USA Nord Mitte    | 40.80.185.0/25,</br>40.80.185.128/26,</br>52.162.111.48/28   |
|                | USA Süd Mitte    | 40.119.9.0/25,</br>40.119.9.128/26,</br>13.73.244.32/28      |
|                | USA, Westen-Mitte     | 52.150.137.128/25,</br>52.150.136.192/26,</br>13.71.199.0/28 |
|                | USA (Westen)             | 40.82.250.0/23,</br>40.82.249.64/26,</br>13.86.219.208/28    |
|                | USA, Westen 2            | 20.42.132.0/23,</br>20.42.129.64/26,</br>13.66.143.128/28    |

## <a name="known-issue-with-azure-storage"></a>Bekanntes Problem mit Azure Storage

* Beim Herstellen einer Verbindung mit einem Azure Storage-Konto haben IP-Netzwerkregeln keine Auswirkungen auf Anforderungen aus der Azure Integration Runtime-Instanz, die sich in derselben Region befindet wie das Storage-Konto. Weitere Informationen finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-an-internet-ip-range). 

  Stattdessen wird empfohlen, [beim Herstellen einer Verbindung mit Azure Storage vertrauenswürdige Dienste](https://techcommunity.microsoft.com/t5/azure-data-factory/data-factory-is-now-a-trusted-service-in-azure-storage-and-azure/ba-p/964993) zu verwenden. 

## <a name="next-steps"></a>Nächste Schritte

* [Sicherheitsüberlegungen für Datenverschiebungen in Azure Data Factory](data-movement-security-considerations.md)
