---
title: Grenzwerte für Azure Stack Edge Pro | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über Grenzwerte und empfohlene Größen beim Bereitstellen und Betreiben von Azure Stack Edge Pro, einschließlich Grenzwerten für Dienste, Geräte und Speicher.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: 5f3c39ce7334145d3ffc1d54badb1f7b766da70f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90904450"
---
# <a name="azure-stack-edge-pro-limits"></a>Für Azure Stack Edge Pro geltende Einschränkungen

Berücksichtigen Sie die folgenden Grenzwerte beim Bereitstellen und Betreiben Ihrer Microsoft Azure Stack Edge Pro-Lösung. 

## <a name="azure-stack-edge-service-limits"></a>Grenzwerte für den Azure Stack Edge-Dienst

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="azure-stack-edge-device-limits"></a>Grenzwerte für Azure Stack Edge-Geräte

In der folgenden Tabelle sind die für Azure Stack Edge Pro-Geräte geltenden Grenzwerte aufgeführt. 

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

- [Vorbereiten der Bereitstellung von Azure Stack Edge Pro](azure-stack-edge-deploy-prep.md)
