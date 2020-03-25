---
author: bucurb
ms.author: bobuc
ms.date: 09/18/2019
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: a3db5535e65e036ab10c35b7bc066a6286a9cad7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "76545206"
---
## <a name="configure-the-sensor-fingerprint-provider"></a>Konfigurieren des Anbieters für Fingerabdrucksensoren

Der erste Schritt besteht darin, einen Anbieter für Fingerabdrucksensoren zu erstellen und zu konfigurieren. Der Anbieter für Fingerabdrucksensoren liest die plattformspezifischen Sensoren auf Ihrem Gerät und konvertiert die gelesenen Daten in eine allgemeine Darstellung, die von der Raumanker-Cloudsitzung genutzt wird.