---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 09aa5affefc576606984ea7116b3d9bda4ba83d5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "96000543"
---
### <a name="run-multiple-containers-on-the-same-host"></a>Ausführen mehrerer Container auf dem gleichen Host

Wenn Sie beabsichtigen, mehrere Container mit offengelegten Ports auszuführen, stellen Sie sicher, dass jeder Container mit einem anderen offengelegten Port ausgeführt wird. Führen Sie beispielsweise den ersten Container an Port 5000 und den zweiten Container an Port 5001 aus.

Sie können diesen Container und einen anderen Azure Cognitive Services-Container zusammen auf dem Host ausführen. Sie können auch mehrere Container desselben Cognitive Services-Containers ausführen.
