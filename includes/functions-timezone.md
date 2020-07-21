---
ms.openlocfilehash: b4fbeb6baa4516ca4cf3ca6194195fae2c688b07
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86165656"
---
Als Standardzeitzone wird in Verbindung mit den CRON-Ausdrücken die Coordinated Universal Time (UTC) verwendet. Wenn Sie möchten, dass Ihr CRON-Ausdruck auf einer anderen Zeitzone basiert, erstellen Sie eine App-Einstellung für die Funktionen-App mit dem Namen `WEBSITE_TIME_ZONE`. 

Der Wert dieser Einstellung hängt davon ab, unter welchem Betriebssystem und Plan Ihre Funktions-App ausgeführt wird.

|Betriebssystem |Plan |Wert |
|-|-|-|
| **Windows** |All | Legen Sie den Wert auf den Namen der gewünschten Zeitzone fest (gemäß [Microsoft-Zeitzonenindex](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749073(v=ws.10) ). |
| **Linux** |Premium<br/>Dediziert |Legen Sie den Wert auf den Namen der gewünschten Zeitzone fest (gemäß [tz-Datenbank](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)). |

> [!NOTE]
> `WEBSITE_TIME_ZONE` wird im Linux-Verbrauchsplan zurzeit nicht unterstützt.

Beispiel: *Eastern Standard Time* (Windows) oder *America/New_York* (Linux) entspricht „UTC-05:00“. Wenn Sie Ihren Trigger mit Timer täglich um 10:00 Uhr (EST) auslösen möchten, verwenden Sie den folgenden NCRONTAB-Ausdruck, der die UTC-Zeitzone berücksichtigt:

```
"0 0 15 * * *"
``` 

Sie können auch eine App-Einstellung für Ihre Funktions-App mit dem Namen `WEBSITE_TIME_ZONE` erstellen, den Wert auf `Eastern Standard Time` (Windows) bzw. `America/New_York` (Linux) festlegen und den folgenden NCRONTAB-Ausdruck verwenden: 

```
"0 0 10 * * *"
``` 

Wenn Sie `WEBSITE_TIME_ZONE` verwenden, wird die Uhrzeit an Abweichungen in der jeweiligen Zeitzone angepasst, z.B. an die Sommerzeit. 
