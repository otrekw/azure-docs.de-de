---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/08/2021
ms.author: alkohli
ms.openlocfilehash: 4740ce4bd59598747cdd9c2147fbbd460b6e648e
ms.sourcegitcommit: c6a2d9a44a5a2c13abddab932d16c295a7207d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2021
ms.locfileid: "107285195"
---
Ihr Gerät ist einem Speicherkonto zugeordnet, das als Ziel für Ihre Daten in Azure verwendet wird. Der Zugriff auf das Speicherkonto wird über das Abonnement und zwei Zugriffsschlüssel mit 512 Bit gesteuert, die dem Speicherkonto zugeordnet sind.

Einer der Schlüssel dient zur Authentifizierung, wenn das Azure Stack Edge-Gerät auf das Speicherkonto zugreift. Der andere Schlüssel wird in Reserve gehalten, sodass Sie die Schlüssel regelmäßig rotieren können.

Aus Sicherheitsgründen ist in vielen Datencentern eine Schlüsselrotation erforderlich. Es wird empfohlen, diese bewährten Methoden für die Schlüsselrotation zu befolgen:

- Ihr Speicherkontoschlüssel ähnelt dem Stammkennwort für das Speicherkonto. Achten Sie darauf, dass Ihr Kontoschlüssel immer gut geschützt ist. Geben Sie das Kennwort nicht an andere Benutzer weiter, vermeiden Sie, es hart zu codieren, und speichern Sie es nicht als Klartext an einem Ort, auf den andere Benutzer Zugriff haben.
- [Generieren Sie Ihren Kontoschlüssel neu](../articles/storage/common/storage-account-keys-manage.md#manually-rotate-access-keys) über das Azure-Portal, wenn Sie denken, er könnte gefährdet sein.
- Der Azure-Administrator sollte den primären oder sekundären Schlüssel regelmäßig über den Abschnitt „Storage“ im Azure-Portal ändern oder neu generieren, um direkt auf das Speicherkonto zuzugreifen.
- Sie können auch Ihren eigenen Verschlüsselungsschlüssel verwenden, um die Daten Ihres Azure Storage-Kontos zu schützen. Wenn Sie einen vom Kunden verwalteten Schlüssel angeben, wird dieser zum Schützen und Steuern des Zugriffs auf den Schlüssel verwendet, mit dem Ihre Daten verschlüsselt werden. Weitere Informationen zum Schützen Ihrer Daten finden Sie unter [Aktivieren von kundenseitig verwalteten Schlüsseln für Ihr Azure Storage-Konto](../articles/storage/common/customer-managed-keys-overview.md#enable-customer-managed-keys-for-a-storage-account).
