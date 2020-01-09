---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 8840e6649058837207c75dbdf7baa5c0b11ef387
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75469731"
---
Ihr Gerät ist einem Speicherkonto zugeordnet, das als Ziel für Ihre Daten in Azure verwendet wird. Der Zugriff auf das Speicherkonto wird über das Abonnement und zwei Zugriffsschlüssel mit 512 Bit gesteuert, die dem Speicherkonto zugeordnet sind.

Einer der Schlüssel dient zur Authentifizierung, wenn das Data Box Edge-Gerät auf das Speicherkonto zugreift. Der andere Schlüssel wird in Reserve gehalten, sodass Sie die Schlüssel regelmäßig rotieren können.

Aus Sicherheitsgründen ist in vielen Datencentern eine Schlüsselrotation erforderlich. Es wird empfohlen, diese bewährten Methoden für die Schlüsselrotation zu befolgen:

- Ihr Speicherkontoschlüssel ähnelt dem Stammkennwort für das Speicherkonto. Achten Sie darauf, dass Ihr Kontoschlüssel immer gut geschützt ist. Geben Sie das Kennwort nicht an andere Benutzer weiter, vermeiden Sie, es hart zu codieren, und speichern Sie es nicht als Klartext an einem Ort, auf den andere Benutzer Zugriff haben.
- Generieren Sie Ihren Kontoschlüssel über das Azure-Portal neu, wenn Sie denken, er könnte gefährdet sein. Weitere Informationen finden Sie unter [Verwalten von Speicherkonto-Zugriffsschlüsseln](../articles/storage/common/storage-account-keys-manage.md).
- Der Azure-Administrator sollte den primären oder sekundären Schlüssel regelmäßig über den Abschnitt „Storage“ im Azure-Portal ändern oder neu generieren, um direkt auf das Speicherkonto zuzugreifen.