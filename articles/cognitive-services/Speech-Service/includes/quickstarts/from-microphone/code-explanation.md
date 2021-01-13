---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2020
ms.author: trbye
ms.openlocfilehash: fdcb5bb57afe145b57cb1223242d4d5625a2dfb7
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400730"
---
Für die Erstellung eines Sprachkonfigurationsobjekts sind der Abonnementschlüssel und die Region der Speech-Ressource erforderlich. Das Konfigurationsobjekt wird zum Instanziieren eines Spracherkennungsobjekts benötigt.

Die Erkennungsinstanz bietet mehrere Spracherkennungsmöglichkeiten. In diesem Beispiel wird Sprache einmal erkannt. Durch diese Funktion wird dem Speech-Dienst mitgeteilt, dass Sie einen einzelnen Ausdruck zur Erkennung senden und dass die Spracherkennung beendet werden soll, sobald der Ausdruck ermittelt wurde. Sobald das Ergebnis vorliegt, wird der Erkennungsgrund in der Konsole ausgegeben.

> [!TIP]
> Das Speech SDK verwendet für die Erkennung standardmäßig die Sprache `en-us`. Informationen zum Auswählen der Ausgangssprache finden Sie unter [Angeben der Ausgangssprache für die Spracherkennung](../../../how-to-specify-source-language.md).