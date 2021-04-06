---
author: rothja
ms.service: iot-dps
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: bbe928084a758335e5fa3b0531726c77a6d599b8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "85839048"
---
Die folgende Tabelle enthält die Grenzwerte, die für Ressourcen des Azure IoT Hub Device Provisioning-Diensts gelten.

| Resource | Begrenzung |
| --- | --- |
| Maximale Anzahl von Gerätebereitstellungsdiensten pro Azure-Abonnement | 10 |
| Maximale Anzahl von Registrierungen (Enrollments) | 1\.000.000 |
| Maximale Anzahl von Registrierungen (Registrations) | 1\.000.000 |
| Maximale Anzahl von Registrierungsgruppen | 100 |
| Maximale Anzahl von Zertifizierungsstellen | 25 |
| Maximale Anzahl der verknüpften IoT-Hubs | 50 |
| Maximale Nachrichtengröße | 96 KB|

> [!NOTE]
> Sie können sich an den [Microsoft-Support](https://azure.microsoft.com/support/options/) wenden, um die Anzahl der Registrierungen für Ihren Bereitstellungsdienst zu erhöhen.

> [!NOTE]
> Die Erhöhung der maximalen Anzahl von Zertifizierungsstellen wird nicht unterstützt.

Der Device Provisioning Service drosselt Anforderungen, wenn die folgenden Kontingente überschritten werden.

| Drosselung | Wert pro Einheit |
| --- | --- |
| Vorgänge | 200/Minute/Dienst |
| Geräteregistrierungen | 200/Minute/Dienst |
| Abrufvorgang für Geräte | 5/10 Sekunden/Gerät |
