---
title: Verschlüsselung für ruhende Daten des Personalisierungdiensts
titleSuffix: Azure Cognitive Services
description: Verschlüsselung für ruhende Daten des Personalisierungdiensts.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: egeaney
ms.openlocfilehash: 63526454bb366b214c27bddce24ed9ebc09556b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80071053"
---
# <a name="personalizer-service-encryption-of-data-at-rest"></a>Verschlüsselung für ruhende Daten des Personalisierungdiensts

Mit dem Personalisierungsdienst werden Ihre Daten beim Speichern in der Cloud automatisch verschlüsselt. Durch die Verschlüsselung des Personalisierungsdiensts werden Ihre Daten ausreichend geschützt, um den Sicherheits- und Complianceanforderungen Ihrer Organisation gerecht zu werden.

[!INCLUDE [cognitive-services-about-encryption](../../../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Vom Kunden verwaltete Schlüssel sind nur im Tarif E0 verfügbar. Wenn Sie die Möglichkeit haben möchten, von Kunden verwaltete Schlüssel zu verwenden, füllen Sie das [Formular zum Anfordern von kundenseitig verwalteten Schlüsseln für den Personalisierungsdienst](https://aka.ms/cogsvc-cmk) aus, und reichen Sie es ein. Nach ca. 3–5 Werktagen erhalten Sie eine Rückmeldung zum Status Ihrer Anforderung. Je nach Bedarf können Sie in einer Warteschlange platziert und genehmigt werden, sobald Platz verfügbar ist. Nachdem Ihre Verwendung von CMK mit dem Personalisierungsdienst genehmigt wurde, müssen Sie eine neue Personalisierungsressource erstellen und als Tarif „E0“ auswählen. Nachdem die Personalisierungsressource mit dem Tarif „E0“ erstellt wurde, können Sie mit Azure Key Vault Ihre verwaltete Identität einrichten.

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk-regions.md)]

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Formular zum Anfordern von kundenseitig verwalteten Schlüsseln für den Personalisierungsdienst](https://aka.ms/cogsvc-cmk)
* [Weitere Informationen zu Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
