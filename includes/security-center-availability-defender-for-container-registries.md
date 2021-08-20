---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 05/19/2021
ms.topic: include
ms.openlocfilehash: 103a23a478a3629eb913c3d1672b3665bc9cba9c
ms.sourcegitcommit: cc099517b76bf4b5421944bd1bfdaa54153458a0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2021
ms.locfileid: "113559469"
---
## <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemein verfügbar (Generally Available, GA)|
|Preise:|**Azure Defender für Containerregistrierungen** wird gemäß den Angaben in der [Preisübersicht](../articles/security-center/security-center-pricing.md) abgerechnet.|
|Unterstützte Registrierungen und Images:|Linux-Images in ACR-Registrierungen, auf die mit Shellzugriff über das öffentliche Internet zugegriffen werden kann|
|Nicht unterstützte Registrierungen und Images:|Windows-Images<br>„Private“ Registrierungen<br>Extrem minimalistische Images wie [Docker-Scratch](https://hub.docker.com/_/scratch/)-Images oder Images ohne Distribution, die nur eine Anwendung und deren Laufzeitabhängigkeiten ohne Paket-Manager, Shell oder Betriebssystem enthalten<br>Images mit der [Spezifikation für das Imageformat Open Container Initiative (OCI)](https://github.com/opencontainers/image-spec/blob/master/spec.md)|
|Erforderliche Rollen und Berechtigungen:|**Sicherheitsleseberechtigter** und [Azure Container Registry: Rollen und Berechtigungen](../articles/container-registry/container-registry-roles.md)|
|Clouds:|:::image type="icon" source="../articles/security-center/media/icons/yes-icon.png" border="false"::: Kommerzielle Clouds<br>:::image type="icon" source="../articles/security-center/media/icons/yes-icon.png" border="false"::: US Gov und China Gov: Derzeit wird nur das Feature zur Überprüfung bei Pushvorgang unterstützt. Weitere Informationen finden Sie unter [Wann werden Images überprüft?](../articles/security-center/defender-for-container-registries-introduction.md#when-are-images-scanned)|
|||
