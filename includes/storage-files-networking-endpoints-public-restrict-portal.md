---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 6/2/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 6c594dbab51c46c382c21b4d87595cd7322f6036
ms.sourcegitcommit: 813f7126ed140a0dff7658553a80b266249d302f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2020
ms.locfileid: "84465122"
---
Navigieren Sie zu dem Speicherkonto, für das Sie den öffentlichen Endpunkt auf bestimmte virtuelle Netzwerke einschränken möchten. Wählen Sie im Inhaltsverzeichnis des Speicherkontos den Eintrag **Firewalls und virtuelle Netzwerke** aus. 

Wählen Sie oben auf der Seite das Optionsfeld **Ausgewählte Netzwerke** aus. Hierdurch werden einige Einstellungen zum Steuern der Einschränkung des öffentlichen Endpunkts eingeblendet. Klicken Sie auf **+ Vorhandenes virtuelles Netzwerk hinzufügen**, um das jeweilige virtuelle Netzwerk auszuwählen, für das der Zugriff auf das Speicherkonto über den öffentlichen Endpunkt zulässig sein soll. Hierfür müssen ein virtuelles Netzwerk und ein zugehöriges Subnetz ausgewählt werden. 

Aktivieren Sie **Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben**, um für vertrauenswürdige Microsoft-Erstanbieterdienste, z. B. die Azure-Dateisynchronisierung, den Zugriff auf das Speicherkonto zuzulassen.

[![Screenshot: Blatt „Firewalls und virtuelle Netzwerke“ mit einem bestimmten virtuellen Netzwerk, für das der Zugriff auf das Speicherkonto über den öffentlichen Endpunkt zulässig ist](media/storage-files-networking-endpoints-public-restrict-portal/restrict-public-endpoint-0.png)](media/storage-files-networking-endpoints-public-restrict-portal/restrict-public-endpoint-0.png#lightbox)