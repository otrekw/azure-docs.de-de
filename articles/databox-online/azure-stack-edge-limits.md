---
title: Grenzwerte für Azure Stack Edge | Microsoft-Dokumentation
description: Erfahren Sie mehr über Grenzwerte und empfohlene Größen beim Bereitstellen und Betreiben von Azure Stack Edge, einschließlich Grenzwerte für Dienste, Geräte und Speicher.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: e21f5a89d9f1f21eb99071a141794f99c07a8dfa
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89079811"
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
