---
title: Kundenseitig verwaltete Schlüssel für Cognitive Services
titleSuffix: Cognitive Services
description: Hier erfahren Sie, wie Sie im Azure-Portal kundenseitig verwaltete Schlüssel mit Azure Key Vault konfigurieren. Mit von Kunden verwalteten Schlüsseln können Sie Zugriffssteuerungen erstellen, rotieren, deaktivieren und widerrufen.
services: cognitive-services
author: erindormier
ms.service: cognitive-services
ms.topic: include
ms.date: 05/28/2020
ms.author: egeaney
ms.openlocfilehash: 1369f30c4a3bcb0a391a5f2b2a63191590afd622
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310698"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-for-cognitive-services"></a>Konfigurieren von kundenseitig verwalteten Schlüsseln mit Azure Key Vault für Cognitive Services

Der Prozess zum Aktivieren von kundenseitig verwalteten Schlüsseln mit Azure Key Vault für Cognitive Services variiert je nach Produkt. Unter den folgenden Links finden Sie dienstspezifische Anweisungen:

## <a name="vision"></a>Bildanalyse

* [Custom Vision-Verschlüsselung für ruhende Daten](../Custom-Vision-Service/custom-vision-encryption-of-data-at-rest.md)
* [Verschlüsselung für ruhende Daten der Gesichtserkennungsdienste](../Face/face-encryption-of-data-at-rest.md)
* [Verschlüsselung für ruhende Daten der Formularerkennung](../form-recognizer/form-recognizer-encryption-of-data-at-rest.md)

## <a name="language"></a>Sprache

* [Verschlüsselung für ruhende Daten des Language Understanding-Diensts](../LUIS/luis-encryption-of-data-at-rest.md)
* [QnA Maker-Verschlüsselung für ruhende Daten](../QnAMaker/qna-maker-encryption-of-data-at-rest.md)
* [Verschlüsselung für ruhende Daten des Übersetzers](../translator/translator-encryption-of-data-at-rest.md)

## <a name="decision"></a>Entscheidung

* [Content Moderator-Verschlüsselung für ruhende Daten](../Content-Moderator/content-moderator-encryption-of-data-at-rest.md)
* [Verschlüsselung für ruhende Daten der Personalisierung](../personalizer/personalizer-encryption-of-data-at-rest.md)

## <a name="next-steps"></a>Nächste Schritte

* [What is Azure Key Vault? (Was ist Azure Key Vault?)](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
* [Formular zum Anfordern von kundenseitig verwalteten Schlüsseln für Cognitive Services](https://aka.ms/cogsvc-cmk)