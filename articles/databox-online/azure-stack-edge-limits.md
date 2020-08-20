---
title: Grenzwerte für Azure Stack Edge | Microsoft-Dokumentation
description: Erfahren Sie mehr über Grenzwerte und empfohlene Größen beim Bereitstellen und Betreiben von Azure Stack Edge, einschließlich Grenzwerte für Dienste, Geräte und Speicher.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/22/2019
ms.author: alkohli
ms.openlocfilehash: 8bbcf3f61121813b91cb951809992d10977a640c
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87922709"
---
# <a name="azure-stack-edge-limits"></a>Grenzwerte für Azure Stack Edge

Berücksichtigen Sie die folgenden Grenzwerte beim Bereitstellen und Betreiben Ihrer Microsoft Azure Stack Edge-Lösung. 

## <a name="azure-stack-edge-service-limits"></a>Grenzwerte für den Azure Stack Edge-Dienst

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="azure-stack-device-limits"></a>Gerätelimits für Azure Stack

In der folgenden Tabelle werden die für ein Azure Stack Edge-Gerät geltenden Grenzwerte beschrieben. 

| BESCHREIBUNG | Wert |
|---|---|
|Nein. der Dateien pro Gerät |100 Mio. |
|Nein. der Freigaben pro Gerät |24 |
|Nein. Freigaben pro Container |1 |
|Maximale in eine Freigabe geschriebene Dateigröße| 5 TB |

## <a name="azure-storage-limits"></a>Speichergrenzwerte für Azure

[!INCLUDE [data-box-edge-gateway-storage-limits](../../includes/data-box-edge-gateway-storage-limits.md)]

## <a name="data-upload-caveats"></a>Einschränkungen beim Hochladen von Daten

[!INCLUDE [data-box-edge-gateway-storage-data-upload-caveats](../../includes/data-box-edge-gateway-storage-data-upload-caveats.md)]

## <a name="azure-storage-account-size-and-object-size-limits"></a>Größenbeschränkungen für das Azure-Speicherkonto und Objekte

[!INCLUDE [data-box-edge-gateway-storage-acct-limits](../../includes/data-box-edge-gateway-storage-acct-limits.md)]


## <a name="azure-object-size-limits"></a>Größenbeschränkungen für das Azure-Objekt

[!INCLUDE [data-box-edge-gateway-storage-object-limits](../../includes/data-box-edge-gateway-storage-object-limits.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Vorbereiten der Bereitstellung von Azure Stack Edge](azure-stack-edge-deploy-prep.md)
