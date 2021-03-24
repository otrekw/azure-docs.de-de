---
title: Abrufen von Lastenausgleichsinformationen mithilfe des Azure Instance Metadata Service
titleSuffix: Azure Load Balancer
description: Erfahren Sie, wie Sie Metadaten zum Lastenausgleich mithilfe des Azure Instance Metadata Service abrufen.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: allensu
ms.openlocfilehash: 0d7d08eb5e77e744fb6ce0abefc550bc79de4c8c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100519081"
---
# <a name="retrieve-load-balancer-information-by-using-the-azure-instance-metadata-service"></a>Abrufen von Lastenausgleichsinformationen mithilfe des Azure Instance Metadata Service

Der Azure Instance Metadata Service (IMDS) stellt Informationen zu VM-Instanzen bereit, die derzeit ausgeführt werden. Der Dienst ist eine REST-API, die unter einer bekannten, nicht routingfähigen IP-Adresse (169.254.169.254) verfügbar ist. 

Wenn Sie Instanzen von VMs oder VM-Gruppen hinter einer Azure Load Balancer Standard-Instanz platzieren, können Sie mit IMDS Metadaten zum Lastenausgleich und zu den Instanzen abrufen.

Die Metadaten umfassen die folgenden Informationen für die VMs oder VM-Skalierungsgruppen:

* Öffentliche IP-Adresse der Standard-SKU
* Konfigurationen für Eingangsregeln für den Lastenausgleich der einzelnen privaten IP-Adressen der Netzwerkschnittstelle
* Konfigurationen für Ausgangsregeln für den Lastenausgleich der einzelnen privaten IP-Adressen der Netzwerkschnittstelle

## <a name="access-the-load-balancer-metadata-using-the-imds"></a>Zugreifen auf die Lastenausgleichs-Metadaten mithilfe von IMDS

Weitere Informationen zum Zugreifen auf die Metadaten zum Lastenausgleich finden Sie unter [Zugreifen auf Informationen zum Lastenausgleich mithilfe des Azure Instance Metadata Service](howto-load-balancer-imds.md).

## <a name="troubleshoot-common-error-codes"></a>Problembehandlung bei häufigen Fehlercodes

Weitere Informationen zu häufigen Fehlercodes und Methoden zu deren Entschärfung finden Sie unter [Häufige Fehlercodes für den Azure Instance Metadata Service (IMDS)](troubleshoot-load-balancer-imds.md). 

## <a name="support"></a>Support

Wenn Sie nach mehreren Versuchen keine Antwort mit Metadaten erhalten, können Sie im Azure-Portal ein Supportproblem erstellen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu [Azure Instance Metadata Service](../virtual-machines/windows/instance-metadata-service.md)

[Bereitstellen einer Load Balancer Standard-Instanz](quickstart-load-balancer-standard-public-portal.md)

