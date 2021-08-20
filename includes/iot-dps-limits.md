---
author: wesmc7777
ms.service: iot-dps
ms.topic: include
ms.date: 06/15/2021
ms.author: wesmc
ms.openlocfilehash: b3c218314f56fb53d0690ac0659192bb955e4301
ms.sourcegitcommit: 5fabdc2ee2eb0bd5b588411f922ec58bc0d45962
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2021
ms.locfileid: "112539002"
---
> [!NOTE]
> In einigen Bereichen dieses Diensts lassen sich Grenzwerte anpassen. Diese sind in der nachfolgenden Tabelle in der Spalte *Anpassbar?* entsprechend gekennzeichnet. Wenn der Grenzwert angepasst werden kann, enthält die Spalte *Anpassbar?* den Wert *Ja*.
>
> Falls Ihr Unternehmen die Anhebung eines anpassbaren Grenzwertes oder Kontingents über den Standardgrenzwert hinaus benötigt, können Sie zusätzliche Ressourcen anfordern, indem Sie [ein Supportticket eröffnen](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

Die folgende Tabelle enthält die Grenzwerte, die für Ressourcen des Azure IoT Hub Device Provisioning-Diensts gelten.

| Resource | Begrenzung | Anpassbar? |
| --- | --- | --- |
| Maximale Anzahl von Gerätebereitstellungsdiensten pro Azure-Abonnement | 10 | Ja |
| Maximale Anzahl von Registrierungen (Registrations) | 1\.000.000 | Ja |
| Maximale Anzahl individueller Registrierungen | 1\.000.000 | Ja |
| Maximale Anzahl von Registrierungsgruppen *(X.509-Zertifikat)* | 100 | Ja |
| Maximale Anzahl von Registrierungsgruppen *(symmetrischer Schlüssel)* | 100 | Nein |
| Maximale Anzahl von Zertifizierungsstellen | 25 | Nein |
| Maximale Anzahl der verknüpften IoT-Hubs | 50 | Nein |
| Maximale Nachrichtengröße | 96 KB| Nein |

> [!TIP]
> Wenn der harte Grenzwert für Registrierungsgruppen mit symmetrischem Schlüssel ein Blockierproblem ist, wird empfohlen, individuelle Registrierungen als Problemumgehung zu verwenden.

Für den Device Provisioning-Dienst gelten die folgenden Ratenbegrenzungen.

| Rate | Wert pro Einheit | Anpassbar? |
| --- | --- | --- |
| Vorgänge | 200/Minute/Dienst | Ja |
| Geräteregistrierungen | 200/Minute/Dienst | Ja |
| Abrufvorgang für Geräte | 5/10 Sekunden/Gerät | Nein |

Jeder API-Aufruf für DPS ist als ein *Vorgang* abrechenbar. Dies schließt alle Dienst-APIs und die Geräteregistrierungs-API ein. Der Abrufvorgang für die Geräteregistrierung wird nicht abgerechnet.