---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 01/25/2021
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3c76988557bfa338bcfb606f568036422a536c1d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98798603"
---
Navigieren Sie zu dem Speicherkonto, für das Sie den öffentlichen Endpunkt auf bestimmte virtuelle Netzwerke einschränken möchten. Wählen Sie im Inhaltsverzeichnis des Speicherkontos den Eintrag **Netzwerke** aus. 

Wählen Sie oben auf der Seite das Optionsfeld **Ausgewählte Netzwerke** aus. Hierdurch werden einige Einstellungen zum Steuern der Einschränkung des öffentlichen Endpunkts eingeblendet. Klicken Sie auf **+ Vorhandenes virtuelles Netzwerk hinzufügen**, um das jeweilige virtuelle Netzwerk auszuwählen, für das der Zugriff auf das Speicherkonto über den öffentlichen Endpunkt zulässig sein soll. Hierfür müssen ein virtuelles Netzwerk und ein zugehöriges Subnetz ausgewählt werden. 

Aktivieren Sie **Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben**, um für vertrauenswürdige Microsoft-Erstanbieterdienste, z. B. die Azure-Dateisynchronisierung, den Zugriff auf das Speicherkonto zuzulassen.

[![Screenshot: Blatt „Netzwerke“ mit einem bestimmten virtuellen Netzwerk, für das der Zugriff auf das Speicherkonto über den öffentlichen Endpunkt zulässig ist](media/storage-files-networking-endpoints-public-restrict-portal/restrict-public-endpoint-0.png)](media/storage-files-networking-endpoints-public-restrict-portal/restrict-public-endpoint-0.png#lightbox)