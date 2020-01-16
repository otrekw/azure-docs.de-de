---
title: Verwalten von Kontozugriffsschlüsseln
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie Ihre Speicherkonto-Zugriffsschlüssel anzeigen, verwalten und rotieren können.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/18/2019
ms.author: tamram
ms.openlocfilehash: 13adf6de420b54299d04a226dab81e75cbb9fef2
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "75975784"
---
# <a name="manage-storage-account-access-keys"></a>Verwalten von Speicherkonto-Zugriffsschlüsseln

Wenn Sie ein Speicherkonto erstellen, generiert Azure zwei 512-Bit-Speicherkonto-Zugriffsschlüssel. Mit diesen Schlüsseln können Sie den Zugriff auf Daten in Ihrem Speicherkonto per gemeinsam verwendetem Schlüssel autorisieren.

Microsoft empfiehlt die Verwendung von Azure Key Vault zum Verwalten Ihrer Zugriffsschlüssel sowie ein regelmäßiges Rotieren und Neugenerieren Ihrer Schlüssel. Mit Azure Key Vault können Sie die Schlüssel auf einfache Weise rotieren, ohne dass Ihre Anwendungen unterbrochen werden. Sie können Ihre Schlüssel auch manuell rotieren.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

## <a name="view-access-keys-and-connection-string"></a>Anzeigen von Zugriffsschlüsseln und Verbindungszeichenfolge

[!INCLUDE [storage-view-keys-include](../../../includes/storage-view-keys-include.md)]

## <a name="use-azure-key-vault-to-manage-your-access-keys"></a>Verwenden von Azure Key Vault zum Verwalten Ihrer Zugriffsschlüssel

Microsoft empfiehlt die Verwendung von Azure Key Vault zum Verwalten und Rotieren Ihrer Zugriffsschlüssel. Ihre Anwendung kann auf sichere Weise auf die Schlüssel in Key Vault zugreifen, sodass Sie diese nicht mit dem Anwendungscode speichern müssen. Weitere Informationen zur Verwendung von Key Vault für die Schlüsselverwaltung finden Sie in den folgenden Artikeln:

- [Verwalten von Speicherkontoschlüsseln mit Azure Key Vault und PowerShell](../../key-vault/key-vault-overview-storage-keys-powershell.md)
- [Verwalten von Speicherkontoschlüsseln mit Azure Key Vault und der Azure-Befehlszeilenschnittstelle](../../key-vault/key-vault-ovw-storage-keys.md)

## <a name="manually-rotate-access-keys"></a>Manuelles Rotieren von Zugriffsschlüsseln

Microsoft empfiehlt, Ihre Zugriffsschlüssel regelmäßig zu rotieren, um die Sicherheit Ihres Speicherkontos zu gewährleisten. Verwenden Sie nach Möglichkeit Azure Key Vault zum Verwalten Ihrer Zugriffsschlüssel. Wenn Sie Key Vault nicht verwenden, müssen Sie die Schlüssel manuell rotieren.

Es werden zwei Zugriffsschlüssel zugewiesen, sodass Sie Ihre Schlüssel rotieren können. Durch das Vorhandensein von zwei Schlüsseln ist sichergestellt, dass der Zugriff Ihrer Anwendung auf Azure Storage während des Prozesses erhalten bleibt.

> [!WARNING]
> Das erneute Generieren Ihrer Zugriffsschlüssel kann sich auf Anwendungen oder Azure-Dienste auswirken, die von dem Speicherkontoschlüssel abhängig sind. Alle Clients, die den Kontoschlüssel verwenden, um auf das Speicherkonto zuzugreifen, müssen aktualisiert werden, damit der neue Schlüssel verwendet wird, einschließlich Media Services, Cloud-, Desktop- und mobiler Anwendungen sowie grafischer Benutzeroberflächenanwendungen für Azure Storage wie [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/).

Befolgen Sie diesen Prozess, um Ihre Speicherkontoschlüssel zu rotieren:

1. Aktualisieren Sie die Verbindungszeichenfolgen in Ihrem Anwendungscode, um den sekundären Schlüssel zu verwenden.
2. Generieren Sie den primären Zugriffsschlüssel für das Speicherkonto neu. Klicken Sie im Azure-Portal auf dem Blatt **Zugriffsschlüssel** auf **Schlüssel 1 erneut generieren** und dann auf **Ja**, um das Generieren eines neuen Schlüssels zu bestätigen.
3. Aktualisieren Sie die Verbindungszeichenfolgen in Ihrem Code, um auf den neuen primären Zugriffsschlüssel zu verweisen.
4. Generieren Sie den sekundären Zugriffsschlüssel auf die gleiche Weise neu.

> [!NOTE]
> Es wird empfohlen, in allen Ihren Anwendungen jeweils nur einen Schlüssel gleichzeitig zu verwenden. Wenn Sie „Key 1“ an einigen Stellen und „Key 2“ an anderen verwenden, können Sie die Verwendung der Schlüssel nicht wechseln, ohne dass einige Anwendungen den Zugriff verlieren.

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Azure Storage-Konten](storage-account-overview.md)
- [Erstellen eines Speicherkontos](storage-account-create.md)
