---
title: Behandeln von IoT Edge-Problemen auf Ihrem Azure Stack Edge Pro-Gerät mit GPU| Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie IoT Edge-Fehler auf Ihrem Azure Stack Edge Pro-GPU-Gerät beheben.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: troubleshooting
ms.date: 06/19/2021
ms.author: alkohli
ms.custom: contperf-fy21q4
ms.openlocfilehash: 405a9c62a551a011eb6d7b00025c6ae0a563e858
ms.sourcegitcommit: e39ad7e8db27c97c8fb0d6afa322d4d135fd2066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111987847"
---
# <a name="troubleshoot-iot-edge-issues-on-your-azure-stack-edge-pro-gpu-device"></a>Beheben von IoT Edge-Problemen auf Ihrem Azure Stack Edge Pro-GPU-Gerät 

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

In diesem Artikel wird beschrieben, wie Sie computebezogene Fehler auf einem Azure Stack Edge Pro GPU-Gerät beheben, indem Sie Laufzeitantworten für den IoT Edge-Agent und Fehler für den IoT Edge-Dienst überprüfen, der auf Ihrem Gerät installiert ist.

## <a name="review-iot-edge-runtime-responses"></a>IoT Edge-Laufzeitantworten

[!INCLUDE [Troubleshoot IoT Edge runtime](../../includes/azure-stack-edge-iot-troubleshoot-compute.md)]

## <a name="troubleshoot-iot-edge-service-errors"></a>Behandeln von IoT Edge-Dienstfehlern

Der folgende Fehler steht im Zusammenhang mit dem IoT Edge-Dienst auf Ihrem Azure Stack Edge Pro-GPU-Gerät.

[!INCLUDE [Troubleshoot IoT Edge errors](../../includes/azure-stack-edge-iot-troubleshoot-compute-error-detail.md)]


## <a name="next-steps"></a>Nächste Schritte

- [Debuggen von Kubernetes-Problemen im Zusammenhang mit IoT Edge](azure-stack-edge-gpu-connect-powershell-interface.md#debug-kubernetes-issues-related-to-iot-edge)
- [Beheben von Geräteproblemen](azure-stack-edge-gpu-troubleshoot.md)