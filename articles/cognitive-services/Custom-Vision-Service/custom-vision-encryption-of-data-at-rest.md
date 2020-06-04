---
title: Custom Vision-Verschlüsselung für ruhende Daten
titleSuffix: Azure Cognitive Services
description: Custom Vision-Verschlüsselung für ruhende Daten.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: egeaney
ms.openlocfilehash: 287344b325237843a549973ae61f569eae6dac93
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83202229"
---
# <a name="custom-vision-encryption-of-data-at-rest"></a>Custom Vision-Verschlüsselung für ruhende Daten

Azure Custom Vision verschlüsselt Ihre Daten beim Speichern in der Cloud automatisch. Durch die Custom Vision-Verschlüsselung werden Ihre Daten ausreichend geschützt, um den Sicherheits- und Complianceanforderungen Ihrer Organisation gerecht zu werden.

[!INCLUDE [cognitive-services-about-encryption](../../../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Vom Kunden verwaltete Schlüssel sind nur für Ressourcen erhältlich, die nach dem 11. Mai 2020 erstellt wurden. Um CMK mit Custom Vision zu verwenden, müssen Sie eine neue Custom Vision-Ressource erstellen. Nachdem die Ressource erstellt wurde, können Sie mit Azure Key Vault Ihre verwaltete Identität einrichten.

### <a name="regional-availability"></a>Regionale Verfügbarkeit

Kundenseitig verwaltete Schlüssel sind zurzeit in den folgenden Regionen verfügbar:

* USA, Süden-Mitte
* USA, Westen 2
* East US
* US Government, Virginia

[!INCLUDE [cognitive-services-cmk](../../../includes/cognitive-services-cmk.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Formular zum Anfordern von kundenseitig verwalteten Schlüsseln für Custom Vision](https://aka.ms/cogsvc-cmk)
* [Weitere Informationen zu Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)


