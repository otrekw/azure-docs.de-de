---
title: Verwenden des Azure-Portals zum Konfigurieren von kundenseitig verwalteten Schlüsseln
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie über das Azure-Portal von Kunden verwaltete Schlüssel mit Azure Key Vault für die Azure Storage-Verschlüsselung konfigurieren.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/19/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 192e58b101b824ca0cc0c732e02647838be6dc35
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81456480"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-the-azure-portal"></a>Konfigurieren von kundenseitig verwalteten Schlüsseln mit Azure Key Vault über das Azure-Portal

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

In diesem Artikel wird beschrieben, wie Sie eine Azure Key Vault-Instanz mit von Kunden verwalteten Schlüsseln über das [Azure-Portal](https://portal.azure.com/) konfigurieren. Informationen zum Erstellen eines Schlüsseltresors über das Azure-Portal finden Sie unter [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe des Azure-Portals](../../key-vault/secrets/quick-create-portal.md).

## <a name="configure-azure-key-vault"></a>Konfigurieren von Azure Key Vault

Bei Verwendung kundenseitig verwalteter Schlüssel mit der Azure Storage-Verschlüsselung müssen für den Schlüsseltresor die beiden Eigenschaften **Vorläufiges Löschen** und **Nicht Bereinigen** festgelegt werden. Diese Eigenschaften sind standardmäßig nicht aktiviert, können aber entweder mithilfe von PowerShell oder der Azure CLI für einen neuen oder vorhandenen Schlüsseltresor aktiviert werden.

Informationen zum Aktivieren dieser Eigenschaften für einen vorhandenen Schlüsseltresor finden Sie in den Abschnitten **Aktivieren des vorläufigen Löschens** und **Aktivieren des Bereinigungsschutzes** in einem der folgenden Artikel:

- [Verwenden des vorläufigen Löschens mit PowerShell](../../key-vault/general/soft-delete-powershell.md)
- [Verwenden des vorläufigen Löschens mit der CLI](../../key-vault/general/soft-delete-cli.md)

Für die Azure Storage-Verschlüsselung werden nur 2048-Bit-RSA- und RSA-HSM-Schlüssel unterstützt. Weitere Informationen zu Schlüsseln finden Sie unter **Key Vault-Schlüssel** in [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten in Azure Key Vault](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys).

## <a name="enable-customer-managed-keys"></a>Aktivieren von vom Kunden verwalteten Schlüsseln

Um vom Kunden verwaltete Schlüssel im Azure-Portal zu aktivieren, gehen Sie folgendermaßen vor:

1. Navigieren Sie zum Speicherkonto.
1. Klicken Sie auf dem Blatt **Einstellungen** Blatt für das Speicherkonto auf **Verschlüsselung**. Wählen Sie die Option **Von Kunden verwaltete Schlüssel** wie in der folgenden Abbildung gezeigt aus.

    ![Portal-Screenshot mit der Verschlüsselungsoption](./media/storage-encryption-keys-portal/portal-configure-encryption-keys.png)

## <a name="specify-a-key"></a>Angeben eines Schlüssels

Nach der Aktivierung von vom Kunden verwalteten Schlüsseln können Sie einen Schlüssel angeben, der dem Storage-Konto zugeordnet werden soll.

### <a name="specify-a-key-as-a-uri"></a>Festlegen eines Schlüssels als URI

Gehen Sie wie folgt vor, um einen Schlüssel als URI anzugeben:

1. Um den Schlüssel-URI im Azure-Portal anzuzeigen, navigieren Sie zu Ihrem Schlüsseltresor, und wählen die Einstellung **Schlüssel** aus. Wählen Sie den gewünschten Schlüssel aus, und klicken Sie darauf, um dessen Versionen anzuzeigen. Wählen Sie eine Schlüsselversion aus, um die Einstellungen für diese Version anzuzeigen.
1. Kopieren Sie den Wert im Feld **Schlüsselbezeichner**, das den URI enthält.

    ![Screenshot mit Schlüssel-URI des Schlüsseltresors](media/storage-encryption-keys-portal/portal-copy-key-identifier.png)

1. Wählen Sie in den Einstellungen unter **Verschlüsselung** für Ihr Speicherkonto die Option **Schlüssel-URI eingeben** aus.
1. Fügen Sie den kopierten URI in das Feld **Schlüssel-URI** ein.

   ![Screenshot zur Eingabe des Schlüssel-URI](./media/storage-encryption-keys-portal/portal-specify-key-uri.png)

1. Geben Sie das Abonnement an, das den Schlüsseltresor enthält.
1. Speichern Sie die Änderungen.

### <a name="specify-a-key-from-a-key-vault"></a>Festlegen eines Schlüssels aus einem Schlüsseltresor

Um einen Schlüssel aus einem Schlüsseltresor anzugeben, müssen Sie zunächst sicherstellen, dass Sie über einen Schlüsseltresor mit einem Schlüssel verfügen. Gehen Sie wie folgt vor, um einen Schlüssel aus einem Schlüsseltresor anzugeben:

1. Wählen Sie die Option **Select from Key Vault** (Aus Schlüsseltresor wählen).
1. Wählen Sie den Schlüsseltresor mit dem Schlüssel aus, den Sie verwenden möchten.
1. Wählen Sie den Schlüssel aus dem Schlüsseltresor aus.

   ![Screenshot mit Option für vom Kunden verwaltete Schlüssel](./media/storage-encryption-keys-portal/portal-select-key-from-key-vault.png)

1. Speichern Sie die Änderungen.

## <a name="update-the-key-version"></a>Aktualisieren der Schlüsselversion

Wenn Sie eine neue Version eines Schlüssels erstellen, aktualisieren Sie das Speicherkonto, damit dieses die neue Version nutzt. Folgen Sie diesen Schritten:

1. Navigieren Sie zu Ihrem Speicherkonto, und zeigen Sie die Einstellungen zur **Verschlüsselung** an.
1. Geben Sie den URI für die neue Schlüsselversion ein. Alternativ können Sie den Schlüsseltresor und den Schlüssel erneut auswählen, um die Version zu aktualisieren.
1. Speichern Sie die Änderungen.

## <a name="use-a-different-key"></a>Verwenden eines anderen Schlüssels

Führen Sie die folgenden Schritte aus, um den für die Azure Storage-Verschlüsselung verwendeten Schlüssel zu ändern:

1. Navigieren Sie zu Ihrem Speicherkonto, und zeigen Sie die Einstellungen zur **Verschlüsselung** an.
1. Geben Sie den URI für den neuen Schlüssel ein. Alternativ können Sie den Schlüsseltresor und dann einen neuen Schlüssel auswählen.
1. Speichern Sie die Änderungen.

## <a name="disable-customer-managed-keys"></a>Deaktivieren von vom Kunden verwalteten Schlüsseln

Wenn Sie vom Kunden verwaltete Schlüssel deaktivieren, wird Ihr Speicherkonto wieder mit von Microsoft verwalteten Schlüsseln verschlüsselt. Führen Sie die folgenden Schritte aus, um vom Kunden verwaltete Schlüssel zu deaktivieren:

1. Navigieren Sie zu Ihrem Speicherkonto, und zeigen Sie die Einstellungen zur **Verschlüsselung** an.
1. Deaktivieren Sie das Kontrollkästchen neben der Einstellung **Eigenen Schlüssel verwenden**.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Storage encryption for data at rest (Azure Storage-Verschlüsselung für ruhende Daten)](storage-service-encryption.md)
- [What is Azure Key Vault? (Was ist Azure Key Vault?)](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
