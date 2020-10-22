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
ms.openlocfilehash: 55d8b5ebdfb226247f8a500f36e6df3ae02ea58a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91619048"
---
# <a name="azure-integration-runtime-ip-addresses"></a>IP-Adressen von Azure Integration Runtime

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Die von Azure Integration Runtime verwendeten IP-Adressen richten sich nach der Region, in der sich Ihre Azure Integration Runtime-Instanz befindet. *Alle* Azure Integration Runtime-Instanzen in ein und derselben Region verwenden dieselben IP-Adressbereiche.

> [!IMPORTANT]  
> Datenflüsse und Azure Integration Runtime-Instanzen, die ein verwaltetes virtuelles Netzwerk aktivieren, unterstützen die Verwendung von festgelegten IP-Adressbereichen nicht.
>
> Sie können diese IP-Adressbereiche für Datenverschiebungen, Pipeline und externe Aktivitäten verwenden. Diese IP-Adressbereiche können auch verwendet werden, um in Datenspeichern, Netzwerksicherheitsgruppen (NSG) oder Firewalls für eingehenden Datenverkehr von Azure Integration Runtime einzurichten. 

## <a name="azure-integration-runtime-ip-addresses-specific-regions"></a>IP-Adressen von Azure Integration Runtime: Bestimmte Regionen

Lassen Sie Datenverkehr von den IP-Adressen zu, die für Azure Integration Runtime in der jeweiligen Azure-Region aufgeführt sind, in der sich Ihre Ressourcen befinden. Sie können eine IP-Adressbereichsliste der Diensttags über den [Downloadlink für den IP-Adressbereich der Diensttags](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#discover-service-tags-by-using-downloadable-json-files) abrufen. Wenn die Azure-Region z. B. **AustraliaEast** ist, können Sie eine IP-Adressbereichsliste von **DataFactory.AustraliaEast** erhalten.


## <a name="known-issue-with-azure-storage"></a>Bekanntes Problem mit Azure Storage

* Beim Herstellen einer Verbindung mit einem Azure Storage-Konto haben IP-Netzwerkregeln keine Auswirkungen auf Anforderungen aus der Azure Integration Runtime-Instanz, die sich in derselben Region befindet wie das Storage-Konto. Weitere Informationen finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-an-internet-ip-range). 

  Stattdessen wird empfohlen, [beim Herstellen einer Verbindung mit Azure Storage vertrauenswürdige Dienste](https://techcommunity.microsoft.com/t5/azure-data-factory/data-factory-is-now-a-trusted-service-in-azure-storage-and-azure/ba-p/964993) zu verwenden. 

## <a name="next-steps"></a>Nächste Schritte

* [Sicherheitsüberlegungen für Datenverschiebungen in Azure Data Factory](data-movement-security-considerations.md)
