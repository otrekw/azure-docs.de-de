---
title: Grenzwerte für Azure Stack Edge Pro | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über Grenzwerte und empfohlene Größen beim Bereitstellen und Betreiben von Azure Stack Edge Pro, einschließlich Grenzwerten für Dienste, Geräte und Speicher.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 10/12/2020
ms.author: alkohli
ms.openlocfilehash: f1bb5662b48765c08ec58d1f7fa9d341de4937bc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91992763"
---
# <a name="azure-stack-edge-pro-limits"></a>Für Azure Stack Edge Pro geltende Einschränkungen

Berücksichtigen Sie die folgenden Grenzwerte beim Bereitstellen und Betreiben Ihrer Microsoft Azure Stack Edge Pro-Lösung. 

## <a name="azure-stack-edge-service-limits"></a>Grenzwerte für den Azure Stack Edge-Dienst

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="azure-stack-edge-device-limits"></a>Grenzwerte für Azure Stack Edge-Geräte

In der folgenden Tabelle sind die für Azure Stack Edge Pro-Geräte geltenden Grenzwerte aufgeführt. 

In der folgenden Tabelle werden die für ein Azure Stack Edge-Gerät geltenden Grenzwerte beschrieben.

| BESCHREIBUNG | Wert |
|---|---|
|Nein. der Dateien pro Gerät |100 Mio. |
|Nein. Freigaben pro Container |1 |
|Maximale Anzahl von Freigabeendpunkten und REST-Endpunkten pro Gerät| 24 |
|Maximale Anzahl von mehrstufigen Speicherkonten pro Gerät| 24|
|Maximale in eine Freigabe geschriebene Dateigröße| 5 TB |
|Maximale Anzahl von Ressourcengruppen pro Gerät| 800 |

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
