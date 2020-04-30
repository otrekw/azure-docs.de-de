---
title: Verwenden des Azure-Portals zum Konfigurieren von kundenseitig verwalteten Schlüsseln
titleSuffix: Cognitive Services
description: Hier erfahren Sie, wie Sie im Azure-Portal kundenseitig verwaltete Schlüssel mit Azure Key Vault konfigurieren. Mit von Kunden verwalteten Schlüsseln können Sie Zugriffssteuerungen erstellen, rotieren, deaktivieren und widerrufen.
services: cognitive-services
author: erindormier
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: egeaney
ms.openlocfilehash: 04a8a8d2520376931f2bb7727c1e751b83f6315f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "81455256"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-the-azure-portal"></a>Konfigurieren von kundenseitig verwalteten Schlüsseln mit Azure Key Vault über das Azure-Portal

Sie müssen Azure Key Vault zum Speichern Ihrer vom Kunden verwalteten Schlüssel verwenden. Sie können entweder Ihre eigenen Schlüssel erstellen und in einem Schlüsseltresor speichern oder mit den Azure Key Vault-APIs Schlüssel generieren. Die Cognitive Services-Ressource und der Schlüsseltresor müssen sich in derselben Region und in demselben Azure Active Directory-Mandanten (Azure AD) befinden, können aber verschiedenen Abonnements zugeordnet sein. Weitere Informationen zum Azure Key Vault finden Sie unter [What is Azure Key Vault? (Was ist der Azure Key Vault?)](https://docs.microsoft.com/azure/key-vault/key-vault-overview).

In diesem Artikel wird beschrieben, wie Sie eine Azure Key Vault-Instanz mit von Kunden verwalteten Schlüsseln über das [Azure-Portal](https://portal.azure.com/) konfigurieren. Informationen zum Erstellen eines Schlüsseltresors über das Azure-Portal finden Sie unter [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe des Azure-Portals](../../key-vault/secrets/quick-create-portal.md).

## <a name="configure-azure-key-vault"></a>Konfigurieren von Azure Key Vault

Bei der Verwendung kundenseitig verwalteter Schlüssel müssen für den Schlüsseltresor zwei Eigenschaften festgelegt werden: **Vorläufiges Löschen** und **Nicht Bereinigen**. Diese Eigenschaften sind standardmäßig nicht aktiviert, können aber entweder mithilfe von PowerShell oder per Azure CLI für einen neuen oder vorhandenen Schlüsseltresor aktiviert werden.

> [!IMPORTANT]
> Wenn Sie die Eigenschaften **Vorläufiges Löschen** und **Nicht Bereinigen** nicht aktiviert haben und Ihren Schlüssel löschen, können Sie die Daten in Ihrer Cognitive Services-Ressource nicht wiederherstellen.

Informationen zum Aktivieren dieser Eigenschaften für einen vorhandenen Schlüsseltresor finden Sie in den Abschnitten **Aktivieren des vorläufigen Löschens** und **Aktivieren des Bereinigungsschutzes** in einem der folgenden Artikel:

- [Verwenden des vorläufigen Löschens mit PowerShell](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell)
- [Verwenden des vorläufigen Löschens mit der CLI](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-cli)

Für die Azure Storage-Verschlüsselung werden nur RSA-Schlüssel der Größe 2048 unterstützt. Weitere Informationen zu Schlüsseln finden Sie unter **Key Vault-Schlüssel** in [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten in Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys).

## <a name="enable-customer-managed-keys"></a>Aktivieren von vom Kunden verwalteten Schlüsseln

Um vom Kunden verwaltete Schlüssel im Azure-Portal zu aktivieren, gehen Sie folgendermaßen vor:

1. Navigieren Sie zu Ihrer Cognitive Services-Ressource.
1. Klicken Sie auf dem Blatt **Einstellungen** für Ihre Cognitive Services Ressource auf **Verschlüsselung**. Wählen Sie wie im folgenden Screenshot gezeigt die Option **Von Kunden verwaltete Schlüssel** aus.

    ![Screenshot zur Auswahl der Option „Von Kunden verwaltete Schlüssel“](../media/cognitive-services-encryption/selectcmk.png)

## <a name="specify-a-key"></a>Angeben eines Schlüssels

Nachdem Sie „Von Kunden verwaltete Schlüssel“ aktiviert haben, können Sie einen Schlüssel angeben, der der Cognitive Services-Ressource zugeordnet werden soll.

### <a name="specify-a-key-as-a-uri"></a>Festlegen eines Schlüssels als URI

Gehen Sie wie folgt vor, um einen Schlüssel als URI anzugeben:

1. Um den Schlüssel-URI im Azure-Portal anzuzeigen, navigieren Sie zu Ihrem Schlüsseltresor, und wählen die Einstellung **Schlüssel** aus. Wählen Sie den gewünschten Schlüssel aus, und klicken Sie darauf, um dessen Versionen anzuzeigen. Wählen Sie eine Schlüsselversion aus, um die Einstellungen für diese Version anzuzeigen.
1. Kopieren Sie den Wert im Feld **Schlüsselbezeichner**, das den URI enthält.

    ![Screenshot mit Schlüssel-URI des Schlüsseltresors](../media/cognitive-services-encryption/key-uri-portal.png)

1. Wählen Sie in den Einstellungen unter **Verschlüsselung** für Ihr Speicherkonto die Option **Schlüssel-URI eingeben** aus.
1. Fügen Sie den kopierten URI in das Feld **Schlüssel-URI** ein.

   ![Screenshot zur Eingabe des Schlüssel-URI](../media/cognitive-services-encryption/ssecmk2.png)

1. Geben Sie das Abonnement an, das den Schlüsseltresor enthält.
1. Speichern Sie die Änderungen.

### <a name="specify-a-key-from-a-key-vault"></a>Festlegen eines Schlüssels aus einem Schlüsseltresor

Um einen Schlüssel aus einem Schlüsseltresor anzugeben, müssen Sie zunächst sicherstellen, dass Sie über einen Schlüsseltresor mit einem Schlüssel verfügen. Gehen Sie wie folgt vor, um einen Schlüssel aus einem Schlüsseltresor anzugeben:

1. Wählen Sie die Option **Select from Key Vault** (Aus Schlüsseltresor wählen).
1. Wählen Sie den Schlüsseltresor mit dem Schlüssel aus, den Sie verwenden möchten.
1. Wählen Sie den Schlüssel aus dem Schlüsseltresor aus.

   ![Screenshot mit Option für vom Kunden verwaltete Schlüssel](../media/cognitive-services-encryption/ssecmk3.png)

1. Speichern Sie die Änderungen.

## <a name="update-the-key-version"></a>Aktualisieren der Schlüsselversion

Wenn Sie eine neue Version eines Schlüssels erstellen, aktualisieren Sie die Cognitive Services-Ressource, damit sie die neue Version verwendet. Folgen Sie diesen Schritten:

1. Navigieren Sie zu Ihrer Cognitive Services-Ressource, und zeigen Sie die Einstellungen für die **Verschlüsselung** an.
1. Geben Sie den URI für die neue Schlüsselversion ein. Alternativ können Sie den Schlüsseltresor und den Schlüssel erneut auswählen, um die Version zu aktualisieren.
1. Speichern Sie die Änderungen.

## <a name="use-a-different-key"></a>Verwenden eines anderen Schlüssels

Gehen Sie wie folgt vor, um den für die Verschlüsselung verwendeten Schlüssel zu ändern:

1. Navigieren Sie zu Ihrer Cognitive Services-Ressource, und zeigen Sie die Einstellungen für die **Verschlüsselung** an.
1. Geben Sie den URI für den neuen Schlüssel ein. Alternativ können Sie den Schlüsseltresor und dann einen neuen Schlüssel auswählen.
1. Speichern Sie die Änderungen.

## <a name="disable-customer-managed-keys"></a>Deaktivieren von vom Kunden verwalteten Schlüsseln

Wenn Sie „Von Kunden verwaltete Schlüssel“ deaktivieren, wird Ihre Cognitive Services-Ressource mit von Microsoft verwalteten Schlüsseln verschlüsselt. Führen Sie die folgenden Schritte aus, um vom Kunden verwaltete Schlüssel zu deaktivieren:

1. Navigieren Sie zu Ihrer Cognitive Services-Ressource, und zeigen Sie die Einstellungen für die **Verschlüsselung** an.
1. Deaktivieren Sie das Kontrollkästchen neben der Einstellung **Eigenen Schlüssel verwenden**.

## <a name="next-steps"></a>Nächste Schritte

* [What is Azure Key Vault? (Was ist Azure Key Vault?)](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
* [Formular zum Anfordern von kundenseitig verwalteten Schlüsseln für Cognitive Services](https://aka.ms/cogsvc-cmk)
* [Verschlüsselung für ruhende Daten der Gesichtserkennungsdienste](../Face/face-encryption-of-data-at-rest.md)
* [QnA Maker-Verschlüsselung für ruhende Daten](../QnAMaker/qna-maker-encryption-of-data-at-rest.md)
* [Verschlüsselung für ruhende Daten des Language Understanding-Diensts](../LUIS/luis-encryption-of-data-at-rest.md)
* [Content Moderator-Verschlüsselung für ruhende Daten](../Content-Moderator/content-moderator-encryption-of-data-at-rest.md)
* [Verschlüsselung für ruhende Daten des Übersetzers](../translator/translator-encryption-of-data-at-rest.md)
* [Verschlüsselung für ruhende Daten der Personalisierung](../personalizer/personalizer-encryption-of-data-at-rest.md)
