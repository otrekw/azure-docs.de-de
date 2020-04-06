---
title: Verschlüsselung für ruhende Daten des Speech-Diensts
titleSuffix: Azure Cognitive Services
description: Verschlüsselung für ruhende Daten des Speech-Diensts.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: 3ccc9820f38a8c32d0b390663eb6b4430b42e8f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79372083"
---
# <a name="speech-service-encryption-of-data-at-rest"></a>Verschlüsselung für ruhende Daten des Speech-Diensts

Mit dem Speech-Dienst werden Ihre Daten beim Speichern in der Cloud automatisch verschlüsselt. Durch die Verschlüsselung des Speech-Diensts werden Ihre Daten ausreichend geschützt, um den Sicherheits- und Complianceanforderungen Ihrer Organisation gerecht zu werden.

## <a name="about-cognitive-services-encryption"></a>Informationen zur Cognitive Services-Verschlüsselung

Daten werden mittels [FIPS 140-2](https://en.wikipedia.org/wiki/FIPS_140-2)-konformer [256-Bit-AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)-Verschlüsselung ver- und entschlüsselt. Verschlüsselung und Entschlüsselung sind transparent, was bedeutet, dass die Verschlüsselung und der Zugriff für Sie verwaltet werden. Ihre Daten werden standardmäßig geschützt, und Sie müssen weder Code noch Anwendungen ändern, um die Verschlüsselung nutzen zu können.

## <a name="about-encryption-key-management"></a>Informationen zur Verwaltung von Verschlüsselungsschlüsseln

Wenn Sie Custom Speech und Custom Voice verwenden, speichert der Speech-Dienst möglicherweise die folgenden Daten in der Cloud:  

* Speech-Ablaufverfolgungsdaten – nur wenn Sie die Ablaufverfolgung für Ihren benutzerdefinierten Endpunkt aktivieren
* Hochgeladene Trainings- und Testdaten

Standardmäßig werden Ihre Daten im Speicher von Microsoft gespeichert, und Ihr Abonnement verwendet von Microsoft verwaltete Verschlüsselungsschlüssel. Sie haben auch die Möglichkeit, Ihr eigenes Speicherkonto vorzubereiten. Der Zugriff auf den Speicher wird durch die verwaltete Identität verwaltet, und der Speech-Dienst kann nicht direkt auf Ihre eigenen Daten zugreifen, wie z. B. Daten der Sprachablaufverfolgung, Daten der Trainingsanpassung und benutzerdefinierte Modelle.

Weitere Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

## <a name="bring-your-own-storage-byos-for-customization-and-logging"></a>Bring Your Own Storage (BYOS) für Anpassung und Protokollierung

Um Zugriff auf Bring Your Own Storage anzufordern, füllen Sie das  [Formular zum Anfordern von Bring Your Own Storage (BYOS) für den Speech-Dienst](https://aka.ms/cogsvc-cmk) aus, und übermitteln Sie es. Nach der Genehmigung müssen Sie ein eigenes Speicherkonto erstellen, um die Daten zu speichern, die für die Anpassung und Protokollierung erforderlich sind. Wenn Sie ein Speicherkonto hinzufügen, aktiviert die Speech-Dienstressource eine vom System zugewiesene verwaltete Identität. Nachdem die vom System zugewiesene verwaltete Identität aktiviert ist, wird diese Ressource bei Azure Active Directory (AAD) registriert. Nach der Registrierung erhält die verwaltete Identität Zugriff auf das Speicherkonto. Hier erfahren Sie mehr zu verwalteten Identitäten. Weitere Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

> [!IMPORTANT]
> Wenn Sie vom System zugewiesene verwaltete Identitäten deaktivieren, wird der Zugriff auf das Speicherkonto entfernt. Dies führt dazu, dass die Teile des Speech-Diensts nicht mehr funktionieren, die Zugriff auf das Speicherkonto benötigen.  

## <a name="regional-availability"></a>Regionale Verfügbarkeit

BYOS ist derzeit in den folgenden Regionen verfügbar:

* USA, Süden-Mitte
* USA, Westen 2
* East US

## <a name="next-steps"></a>Nächste Schritte

* [Formular zum Anfordern von Bring Your Own Storage (BYOS) für den Speech-Dienst](https://aka.ms/cogsvc-cmk)
* [Was sind verwaltete Identitäten?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
