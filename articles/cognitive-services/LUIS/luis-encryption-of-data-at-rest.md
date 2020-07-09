---
title: Verschlüsselung für ruhende Daten des Language Understanding-Diensts
titleSuffix: Azure Cognitive Services
description: Verschlüsselung für ruhende Daten des Language Understanding-Diensts.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: 4fc816c3894120a5d1b356d91ebebbc56f21b530
ms.sourcegitcommit: ff19f4ecaff33a414c0fa2d4c92542d6e91332f8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "85052694"
---
# <a name="language-understanding-service-encryption-of-data-at-rest"></a>Verschlüsselung für ruhende Daten des Language Understanding-Diensts

Mit dem Language Understanding-Dienst werden Ihre Daten beim Speichern in der Cloud automatisch verschlüsselt. Die Verschlüsselung des Language Understanding-Diensts schützt Ihre Daten und unterstützt Sie beim Einhalten der Sicherheits- und Complianceanforderungen Ihrer Organisation.

## <a name="about-cognitive-services-encryption"></a>Informationen zur Cognitive Services-Verschlüsselung

Daten werden mittels [FIPS 140-2](https://en.wikipedia.org/wiki/FIPS_140-2)-konformer [256-Bit-AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)-Verschlüsselung ver- und entschlüsselt. Verschlüsselung und Entschlüsselung sind transparent, was bedeutet, dass die Verschlüsselung und der Zugriff für Sie verwaltet werden. Ihre Daten werden standardmäßig geschützt, und Sie müssen weder Code noch Anwendungen ändern, um die Verschlüsselung nutzen zu können.

## <a name="about-encryption-key-management"></a>Informationen zur Verwaltung von Verschlüsselungsschlüsseln

Standardmäßig verwendet Ihr Abonnement von Microsoft verwaltete Verschlüsselungsschlüssel. Es gibt auch eine Option zum Verwalten Ihres Abonnements mit ihren eigenen Schlüsseln. Kundenseitig verwaltete Schlüssel (CMK) bieten größere Flexibilität beim Erstellen, Rotieren, Deaktivieren und Widerrufen von Zugriffssteuerungen. Außerdem können Sie die zum Schutz Ihrer Daten verwendeten Verschlüsselungsschlüssel überwachen.

## <a name="customer-managed-keys-with-azure-key-vault"></a>Von Kunden verwaltete Schlüssel mit Azure Key Vault

Es gibt auch eine Option zum Verwalten Ihres Abonnements mit Ihren eigenen Schlüsseln. Kundenseitig verwaltete Schlüssel (Customer-Managed Keys, CMK) werden auch als Bring Your Own Key (BYOK) bezeichnet und bieten eine größere Flexibilität beim Erstellen, Rotieren, Deaktivieren und Widerrufen von Zugriffssteuerungen. Außerdem können Sie die zum Schutz Ihrer Daten verwendeten Verschlüsselungsschlüssel überwachen.

Sie müssen Azure Key Vault zum Speichern Ihrer vom Kunden verwalteten Schlüssel verwenden. Sie können entweder Ihre eigenen Schlüssel erstellen und in einem Schlüsseltresor speichern oder mit den Azure Key Vault-APIs Schlüssel generieren. Die Cognitive Services-Ressource und der Schlüsseltresor müssen sich in derselben Region und in demselben Azure Active Directory-Mandanten (Azure AD) befinden, aber sie können sich in verschiedenen Abonnements befinden. Weitere Informationen zum Azure Key Vault finden Sie unter [What is Azure Key Vault? (Was ist der Azure Key Vault?)](https://docs.microsoft.com/azure/key-vault/key-vault-overview).

### <a name="customer-managed-keys-for-language-understanding"></a>Vom Kunden verwaltete Schlüssel für Language Understanding

Wenn Sie die Möglichkeit haben möchten, von Kunden verwaltete Schlüssel zu verwenden, füllen Sie das  [Formular zum Anfordern von kundenseitig verwalteten Schlüsseln für den LUIS-Dienst](https://aka.ms/cogsvc-cmk) aus, und reichen Sie es ein. Nach ca. 3–5 Werktagen erhalten Sie eine Rückmeldung zum Status Ihrer Anforderung. Je nach Bedarf können Sie in einer Warteschlange platziert und genehmigt werden, sobald Platz verfügbar ist. Nachdem Ihre Verwendung von CMK mit LUIS genehmigt haben, müssen Sie im Azure-Portal eine neue Language Understanding-Ressource erstellen und E0 als Tarif auswählen. Die neue SKU funktioniert genauso wie die bereits verfügbare F0-SKU, mit Ausnahme von CMK. Benutzer können kein Upgrade von F0 auf die neue E0-SKU durchführen.

![Bild eines LUIS-Abonnements](../media/cognitive-services-encryption/luis-subscription.png)

### <a name="regional-availability"></a>Regionale Verfügbarkeit

Kundenseitig verwaltete Schlüssel sind in allen [Erstellungsregionen](luis-reference-regions.md) verfügbar. 

### <a name="limitations"></a>Einschränkungen

Es gibt einige Einschränkungen bei der Verwendung der Ebene E0 mit vorhandenen/zuvor erstellten Anwendungen:

* Die Migration zu einer E0-Ressource wird blockiert. Benutzer können ihre Apps nur zu F0-Ressourcen migrieren. Nachdem Sie eine vorhandene Ressource zu F0 migriert haben, können Sie in der Ebene E0 eine neue Ressource erstellen. [Hier ](https://docs.microsoft.com/azure/cognitive-services/luis/luis-migration-authoring) erfahren Sie mehr zur Migration  
* Das Verschieben von Anwendungen in eine oder aus einer E0-Ressource wird blockiert. Als Problemumgehung für diese Einschränkung kann die vorhandene Anwendung exportiert und als E0-Ressource importiert werden.
* Die Bing-Rechtschreibprüfung wird nicht unterstützt.
* Das Protokollieren des Endbenutzer-Datenverkehrs ist deaktiviert, wenn Ihre Anwendung eine E0-Anwendung ist.
* Die Vorbereitungsfunktion für Speech im Azure Bot-Dienst wird für Anwendungen in der Ebene E0 nicht unterstützt. Dieses Feature ist über den Azure Bot Service verfügbar, der CMK nicht unterstützt.
* Die Vorbereitungsfunktion für Speech im Portal erfordert Azure Blob Storage. Weitere Informationen finden Sie unter [Bring Your Own Storage](../Speech-Service/speech-encryption-of-data-at-rest.md#bring-your-own-storage-byos-for-customization-and-logging).

### <a name="enable-customer-managed-keys"></a>Aktivieren von vom Kunden verwalteten Schlüsseln

Eine neue Cognitive Services-Ressource wird immer mit von Microsoft verwalteten Schlüsseln verschlüsselt. Es ist nicht möglich, zum Zeitpunkt der Ressourcenerstellung kundenseitig verwaltete Schlüssel zu aktivieren. Kundenseitig verwaltete Schlüssel werden in Azure Key Vault gespeichert. Für den Schlüsseltresor müssen Zugriffsrichtlinien bereitgestellt werden, mit denen Schlüsselberechtigungen für die verwaltete Identität erteilt werden, die der Cognitive Services-Ressource zugeordnet ist. Die verwaltete Identität ist erst verfügbar, nachdem die Ressource mit dem Tarif für CMK erstellt wurde.

Informationen zum Verwenden von kundenseitig verwalteten Schlüsseln mit Azure Key Vault für die Cognitive Services-Verschlüsselung finden Sie in dem folgenden Artikel:

- [Konfigurieren von kundenseitig verwalteten Schlüsseln mit Key Vault für die Cognitive Services-Verschlüsselung über das Azure-Portal](../Encryption/cognitive-services-encryption-keys-portal.md)

Durch das Aktivieren von kundenseitig verwalteten Schlüsseln wird auch eine systemseitig zugewiesene verwaltete Identität aktiviert. Dies ist eine Funktion von Azure AD. Sobald die systemseitig zugewiesene verwaltete Identität aktiviert ist, wird diese Ressource bei Azure Active Directory registriert. Nach der Registrierung erhält die verwaltete Identität Zugriff auf die Key Vault-Instanz, die bei der Einrichtung des kundenseitig verwalteten Schlüssels ausgewählt wurde. Sie können sich weiter über [verwaltete Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) informieren.

> [!IMPORTANT]
> Wenn Sie systemseitig zugewiesene verwaltete Identitäten deaktivieren, wird der Zugriff auf den Schlüsseltresor entfernt, und alle mit den Kundenschlüsseln verschlüsselten Daten sind nicht mehr zugänglich. Alle Features, die von diesen Daten abhängen, funktionieren nicht mehr.

> [!IMPORTANT]
> Verwaltete Identitäten unterstützen derzeit keine verzeichnisübergreifenden Szenarien. Wenn Sie vom Kunden verwaltete Schlüssel im Azure-Portal konfigurieren, wird automatisch eine verwaltete Identität im Hintergrund zugewiesen. Wenn Sie anschließend das Abonnement, die Ressourcengruppe oder die Ressource von einem Azure AD-Verzeichnis in ein anderes Verzeichnis verschieben, wird die der Ressource zugeordnete verwaltete Identität nicht an den neuen Mandanten übertragen, sodass kundenseitig verwaltete Schlüssel möglicherweise nicht mehr funktionieren. Weitere Informationen finden Sie in [Häufig gestellte Fragen und bekannte Probleme mit verwalteten Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/known-issues#transferring-a-subscription-between-azure-ad-directories) unter **Übertragen eines Abonnements zwischen Azure AD-Verzeichnissen**.  

### <a name="store-customer-managed-keys-in-azure-key-vault"></a>Speichern von kundenseitig verwalteten Schlüsseln in Azure Key Vault

Wenn Sie kundenseitig verwaltete Schlüssel aktivieren möchten, müssen Sie Ihre Schlüssel in einer Azure Key Vault-Instanz speichern. Sie müssen die Eigenschaften **Vorläufiges Löschen** und **Do Not Purge** (Nicht bereinigen) im Schlüsseltresor aktivieren.

Für die Cognitive Services-Verschlüsselung werden nur RSA-Schlüssel der Größe 2048 unterstützt. Weitere Informationen zu Schlüsseln finden Sie unter **Key Vault-Schlüssel** in [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten in Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys).

### <a name="rotate-customer-managed-keys"></a>Rotieren von kundenseitig verwalteten Schlüsseln

Sie können einen vom Kunden verwalteten Schlüssel in Azure Key Vault entsprechend Ihren Konformitätsrichtlinien rotieren. Wenn der Schlüssel rotiert wird, müssen Sie die Cognitive Services-Ressource aktualisieren, sodass der neue Schlüssel-URI verwendet wird. Informationen dazu, wie Sie die Ressource im Azure-Portal zur Verwendung einer neuen Version des Schlüssels aktualisieren, finden Sie unter [Konfigurieren von kundenseitig verwalteten Schlüsseln für Cognitive Services über das Azure-Portal](../Encryption/cognitive-services-encryption-keys-portal.md) im Abschnitt **Aktualisieren der Schlüsselversion**.

Durch Rotieren des Schlüssels werden die Daten in der Ressource nicht erneut verschlüsselt. Es ist keine weitere Aktion durch den Benutzer erforderlich.

### <a name="revoke-access-to-customer-managed-keys"></a>Widerrufen des Zugriffs auf von Kunden verwaltete Schlüssel

Zum Widerrufen des Zugriffs auf von Kunden verwaltete Schlüssel können Sie PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden. Weitere Informationen finden Sie unter [Azure Key Vault – PowerShell](https://docs.microsoft.com/powershell/module/az.keyvault//) oder [Azure Key Vault – CLI](https://docs.microsoft.com/cli/azure/keyvault). Durch das Widerrufen des Zugriffs wird der Zugriff auf alle Daten in der Cognitive Services-Ressource blockiert, da Cognitive Services keinen Zugriff mehr auf den Verschlüsselungsschlüssel hat.

## <a name="next-steps"></a>Nächste Schritte

* [Formular zum Anfordern von kundenseitig verwalteten Schlüsseln für den LUIS-Dienst](https://aka.ms/cogsvc-cmk)
* [Weitere Informationen zu Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
