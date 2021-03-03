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
ms.openlocfilehash: 88a723abc606a527232b7c1949f35c1fedfdba50
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101706842"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-for-cognitive-services"></a>Konfigurieren von kundenseitig verwalteten Schlüsseln mit Azure Key Vault für Cognitive Services

Der Prozess zum Aktivieren von kundenseitig verwalteten Schlüsseln mit Azure Key Vault für Cognitive Services variiert je nach Produkt. Unter den folgenden Links finden Sie dienstspezifische Anweisungen:

## <a name="vision"></a>Bildanalyse

* [Custom Vision-Verschlüsselung für ruhende Daten](../custom-vision-service/encrypt-data-at-rest.md)
* [Verschlüsselung für ruhende Daten der Gesichtserkennungsdienste](../face/encrypt-data-at-rest.md)
* [Verschlüsselung für ruhende Daten der Formularerkennung](../form-recognizer/encrypt-data-at-rest.md)

## <a name="language"></a>Sprache

* [Verschlüsselung für ruhende Daten des Language Understanding-Diensts](../LUIS/encrypt-data-at-rest.md)
* [QnA Maker-Verschlüsselung für ruhende Daten](../QnAMaker/encrypt-data-at-rest.md)
* [Verschlüsselung für ruhende Daten des Übersetzers](../translator/encrypt-data-at-rest.md)

## <a name="decision"></a>Entscheidung

* [Content Moderator-Verschlüsselung für ruhende Daten](../Content-Moderator/encrypt-data-at-rest.md)
* [Verschlüsselung für ruhende Daten der Personalisierung](../personalizer/encrypt-data-at-rest.md)

## <a name="next-steps"></a>Nächste Schritte

* [What is Azure Key Vault? (Was ist Azure Key Vault?)](../../key-vault/general/overview.md)
* [Formular zum Anfordern von kundenseitig verwalteten Schlüsseln für Cognitive Services](https://aka.ms/cogsvc-cmk)